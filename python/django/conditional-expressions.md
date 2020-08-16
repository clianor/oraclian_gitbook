# Conditional Expressions

장고의 ORM 으로도 if ... elif ... else 와 같은 조건문을 사용할 수 있습니다.  
이제부터 설명할 내용에서 사용될 모델은 다음과 같습니다.

```python
from django.db import models

class Client(models.Model):
    REGULAR = 'R'
    GOLD = 'G'
    PLATINUM = 'P'
    ACCOUNT_TYPE_CHOICES = [
        (REGULAR, 'Regular'),
        (GOLD, 'Gold'),
        (PLATINUM, 'Platinum'),
    ]
    name = models.CharField(max_length=50)
    registered_on = models.DateField()
    account_type = models.CharField(
        max_length=1,
        choices=ACCOUNT_TYPE_CHOICES,
        default=REGULAR,
    )
```

장고의 ORM에서 대표적으로 사용되는 조건식은 두 가지가 있습니다.  
바로 When 과 Case 입니다.  
먼저 When 부터 보도록 하겠습니다.

#### 1. When

```python
from django.db.models import F, When

When(account_type=Client.GOLD, then='name')
When(account_type=Client.GOLD, then=F('name'))
```

우선 장고에서 위의 두 표현식은 동일합니다.  
then에서 문자열 인수는 필드를 참조하기 때문입니다.  
위 내용은 Client 모델에서 account\_type의 값이 값이 'G'라면 name 필드의 값을 리턴하는 조건입니다.  
sql에서의 case 문 내부에 오는 when, then과 동일합니다.

```python
import Q, When
from datetime import date

When(registered_on__gt=date(2014, 1, 1),
     registered_on__lt=date(2015, 1, 1),
     then='account_type')
When(Q(name__startswith="John") | Q(name__startswith="Paul"), then='name')
```

위의 표현식은 When 내부에서 복잡한 조건을 표현할 수 있다는 것을 보여주는 내용입니다.  
더블 스코어도 사용할 수 있으며 Q 객를 활용한 표현식도 가능합니다.

```python
import When
from django.db.models import Exists, OuterRef

non_unique_account_type = Client.objects.filter(
    account_type=OuterRef('account_type'),
).exclude(pk=OuterRef('pk')).values('pk')
When(Exists(non_unique_account_type), then=Value('non unique'))
```

위의 표현식은 유니크한지 안한지 여부를 판단하는 When입니다.  
아직도 OuterRef에 대해서 익숙치 않아 정확히 어떻게 동작하는지는 잘 모르겠네요 더 찾아보고 설명을 수정하겠습니다.

#### 2. Case

Case는 위에서 설명한 When과 함께 사용되며 Case가 하나 또는 복수의 When을 인자로 받아 처리됩니다.

```python
from datetime import date, timedelta
from django.db.models import Case, CharField, When

Client.objects.create(
    name='Jane Doe',
    account_type=Client.REGULAR,
    registered_on=date.today() - timedelta(days=36))
    
Client.objects.create(
    name='James Smith',
    account_type=Client.GOLD,
    registered_on=date.today() - timedelta(days=5))
    
Client.objects.create(
    name='Jack Black',
    account_type=Client.PLATINUM,
    registered_on=date.today() - timedelta(days=10 * 365))
    
Client.objects.annotate(
    discount=Case(
        When(account_type=Client.GOLD, then=Value('5%')),
        When(account_type=Client.PLATINUM, then=Value('10%')),
        default=Value('0%'),
        output_field=CharField(),
    ),
).values_list('name', 'discount')
# <QuerySet [('Jane Doe', '0%'), ('James Smith', '5%'), ('Jack Black', '10%')]>
```

위의 코드는 Client의 account\_type에 따라서 할인률을 Case와 When을 가지고 보여주는 코드입니다.  
만약 등급이 GOLD 일 경우에는 5%, PLATINUM 일때는 10%의 할인을 해주고 둘 다 아닌 경우에는 할인을 해주지 않으며 이 출력값은 문자열로 discount라는 필드로 생성하는 코드입니다.

