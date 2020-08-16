# Subquery\(\) Expressions

장고의 ORM으로 서브쿼리는 아래와 같이 표현할 수 있습니다.

```python
from django.db.models import OuterRef, Subquery

newest = Comment.objects.filter(post=OuterRef('pk')).order_by('-created_at')
Post.objects.annotate(
    newest_commenter_email=Subquery(newest.values('email')[:1])
)
```

위의 ORM 표현식은 아래와 같은 sql로 표현됩니다.

```sql
SELECT "post"."id", (
    SELECT U0."email"
    FROM "comment" U0
    WHERE U0."post_id" = ("post"."id")
    ORDER BY U0."created_at" DESC LIMIT 1
) AS "newest_commenter_email" FROM "post"
```

먼저 ORM을 하나씩 설명해보자면 첫 줄에서  미리 서브쿼리를 작성합니다.   
ORM의 3 번째 줄에서는 post 테이블의 pk와 비교하기 위해 OuterRef를 사용하였으며 생성된 날짜순으로 내림차순을 수행한 서브쿼리를 작성하였습니다.  
4 번째 줄에서는 newest\_commenter\_email 필드를 추가하여 서브쿼리의 결과중 정렬된 첫 번째 값만 뽑아 사용한 예시입니다.

위와는 조금 다르게 서브쿼리를 조건절에도 사용할 수 있습니다.

```sql
from datetime import timedelta
from django.utils import timezone

one_day_ago = timezone.now() - timedelta(days=1)
posts = Post.objects.filter(published_at__gte=one_day_ago)
Comment.objects.filter(post__in=Subquery(posts.values('pk')))
```

위와 같이 사용한다면 하루 전날 이후로 생성된 포스트들의 댓글들만 조회하게 됩니다.  
위의 코드가 쿼리가 된다고하면 아래와 비슷하 될것 같습니다.  
\(실제로 쿼리를 출력해보지는 않았습니다\)

```sql
SELECT *
FROM "comments"
WHERE "comments"."id" IN (
    SELECT U0."id"
    FROM "posts" U0
    WHERE U0."published_at" >= DATE_ADD(NOW(), INTERVAL -1 DAY);
)
```

  
서브쿼리와 함께 여러 함수들도 같이 사용할 수 있으며 EXITS와 같은 함수를 사용하여 표현할 수 있습니다.

```sql
from django.db.models import Exists, OuterRef
from datetime import timedelta
from django.utils import timezone

one_day_ago = timezone.now() - timedelta(days=1)
recent_comments = Comment.objects.filter(
    post=OuterRef('pk'),
    created_at__gte=one_day_ago,
)
Post.objects.annotate(recent_comment=Exists(recent_comments))
```

위의 코드는 아래의 sql로 표현됩니다.

```sql
SELECT "post"."id", "post"."published_at", EXISTS(
    SELECT U0."id", U0."post_id", U0."email", U0."created_at"
    FROM "comment" U0
    WHERE (
        U0."created_at" >= YYYY-MM-DD HH:MM:SS AND
        U0."post_id" = ("post"."id")
    )
) AS "recent_comment" FROM "post"
```

위의 코드와 쿼리가 의미하는 것은 각 게시물에 하루 전날 이후로 작성된 댓글이 있는지 확인하는 쿼리입니다.  
위의 쿼리에서 YYYY-MM-DD 와 같이 datetime string format은 one\_day\_ago 변수의 값이 들어가는 부분을 장고 문서에서는 저렇게 표현한 것 같습니다.

