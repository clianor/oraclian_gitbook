# Query Expressions

장고에는 많은 쿼리 표현식이 있습니다.  
먼저 공식 문서에 있는 장고의 쿼리표현식의 예제를 가져와 조금씩 설명을 해보겠습니다.

#### 1 . 의자의 수보다 직원의 수가 더 많은 회사 찾기.

```python
from django.db.models import F

Company.objects.filter(num_employees__gt=F('num_chairs'))
```

여기서 나오는 \_\_gt는 오른쪽에 보다 크다는 의미로서 F\(\) 표현식 이용하여 테이블의 레코드 중에서 의자의 수보다 직원의 수가 많은이라는 의미를 가지게하고 filter를 통해 sql의 where문과 동일한 기능을 수행하게 합니다.

#### 2. 직원 수가 의자보다 두 배 이상 많은 회사 찾기.

```python
from django.db.models import F

Company.objects.filter(num_employees__gt=F('num_chairs') * 2)

Company.objects.filter(num_employees__gt=F('num_chairs') + F('num_chairs'))
```

이번에는 두 개의 쿼리셋이 있습니다.  
이 두 쿼리셋은 동일한 내용을 보여주는 쿼리셋입니다.  
1번에서 설명한 \_\_gt와 같은 조건 키워드를 이용하였으며 직원의 수가 의자의 수보다 두 배가 많아야 하므로 하나의 레코드에서 num\_employees &gt; num\_chairs \* 2또는 num\_employees &gt; num\_chairs + num\_chairs와 동일한 결과를 반환하고자 위와 같이 작성된 쿼리셋입니다.

#### 3. 모든 직원을 앉히기 위해 필요한 의자의 갯수 구하기.

```python
from django.db.models import F

company = Company.objects.filter(num_employees__gt=F('num_chairs'))
                .annotate(
                    chairs_needed=F('num_employees') - F('num_chairs')
                ).first()
                
company.num_employees
# 120
company.num_chairs
# 50
company.chairs_needed
# 70
```

이번에는 2번까지 나오지 않았던 annotate라는 새로운 개념이 있습니다.  
이 annotate는 정말 단순히 이야기하자면 sql에서 as와 같은 의미입니다.  
기존의 장고의 모델에서 선언된 필드명이 마음에 들지 않다거나 특정한 목적을 가진 새로운 필드를 추가할때 사용합니다.  
위의 쿼리셋에서는 먼저 코드에서는 의자의 수보다 직원의 수가 많은 회사를 찾고, 그러한 회사들 중에서 chairs\_needed라는 새로운 필드를 추가합니다.  
이 새로 추가된 chairs\_needed라는 필드는 직원수 - 의자수의 값을 가지게 됩니다.

#### 4. 표현식을 사용하여 회사 생성하기

```python
from django.db.models import F
from django.db.models.functions import Value, Upper

company = Company.objects.create(name='Google', ticker=Upper(Value('goog')))
# 데이터 접근이 필요한 경우 객체를 다시 디비로부터 읽어옴
company.refresh_from_db()
company.ticker
# 'GOOG'
```

장고에서는 sql의 함수들이 이미 정의되어 있습니다.  
위의 쿼리셋에서는 ticker라는 필드에 값을 삽입할땐 입력된 스트링을 대문자로 변환하는 과정을 거치고 있습니다.  
이때 'goog'라는 소문자 스트링을 Value 함수로 감싸는 모습을 볼 수 있는데 이는 일반적으로 생략할 수 있습니다.  
그리고 Upper 함수를 이용하여 'goog' 스트링을 'GOOG'로 변환하여 저장을 하게 됩니다.  
create 함수를 사용해서 객체를 받아온다고해서 디비의 내용을 바로 가져올 수 있는것은 아닌데 캐싱되어 있는 데이터가 아닌 디비의 데이터를 가져오기 위해선 refresh\_from\_db 메소드를 이용하여 모델 객체를 다시 디비로부터 읽어옵니다.

#### 5. 회사의 제품의 갯수를 구하기.

```python
from django.db.models import Count, F

Company.objects.annotate(num_products=Count('products'))

Company.objects.annotate(num_products=Count(F('products')))
```

지금까지의 응용입니다.  
이번에 토스의 과제를 치루면서 가운트를 세야했던 일이 있었는데 그때 이렇게 사용하면 더 좋았을텐데 라는 생각이 드네요.

#### 6. 집계에는 복잡한 표현식도 들어갈 수 있다.

```python
from django.db.models import Count, F

Company.objects.annotate(num_offerings=Count(F('products') + F('services')))
```

이 부분은 정확히 어떻게 동작하는지 몰라 확인하고 다시 써야할거 같네요.  
우선은 회사의 제품들과 서비스들의 갯수의 합을 구하는 것 같습니다.

#### 7. order\_by에서도 표현식을 직접 사용할 수 있다.

```python
from django.db.models import Count, F, Value
from django.db.models.functions import Length, Upper

Company.objects.order_by(Length('name').asc())
Company.objects.order_by(Length('name').desc())
```

회사의 이름의 길이에 따라 오름차순 또는 내림차순 하는 예제입니다.

#### 8. order\_by에서 더블 언더스코어를 활용하여 조회할 수 있다.

```python
from django.db.models import CharField
from django.db.models.functions import Length

CharField.register_lookup(Length)
Company.objects.order_by('name__length')
```

register\_lookup은 처음보는 기능이였습니다.  
이 기능은 기존에 장고에서 지원하지 않는 더블 언더스코어를 확장해서 쓸 수 있는 기능입니다.  
위의 표현식은 7번과 동일합니다.

#### 9. Boolean 표현식은 필터에서 바로 사용할 수 있

```python
from django.db.models import Exists

Company.objects.filter(
    Exists(Employee.objects.filter(company=OuterRef('pk'), salary__gt=10))
)
```

이 것은 장고의 queryset이 마지막까지 지연된다는 점을 활용한 방법입니다.  
장고의 ORM은 filter가 호출된 시점 혹은 그 표현식이 끝나는 시점에서 디비에서 불러오지 않습니다.  
그리하여 위와 같이 salary가 10보다 큰 직원이 있는 회사인 경우만 조회하는 표현식입니다.

