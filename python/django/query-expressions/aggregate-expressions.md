# Aggregate\(\) expressions

aggregate\(\) 표현식은 sql에 연관지어 이야기해보면 그룹함수를 이용한 표현식을 의미합니다.  
sum, max, min과 같은 집함수들을 이용할 수 있게합니다.  
  
사용법은 아래와 같습니다.

```python
from django.db.models import Count

Company.objects.annotate(
    managers_required=(Count('num_employees') / 4) + Count('num_managers')
)
```



