# Func\(\) Expressions

Func\(\) 표현식은 데이터베이스의 기능과 관련된 모든 표현식의 기본입니다.  
간단히 문자열을 대문자 또는 소문자로 만든다던지 datetime 형태의 데이터를 특정 형태로 만든다던지와 같은 작업을 수행할 수 있습니다.  
  
사용법은 아래와 같습니다.

```python
from django.db.models import F, Func

queryset.annotate(field_lower=Func(F('field'), function='LOWER'))
```



