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

