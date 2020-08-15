# F\(\) expressions

F\(\) 표현식은 간단히 말해 데이터베이스의 컬럼을 직접 참조하는 기능입니다.  
파이썬의 메모리에 가져오지 않고도 테이블의 특정 컬럼과 관련된 연산 또는 표현식을 직접 수행할 수 있게됩니다.  
  
먼저 F 객체를 사용하지 않았을 경우부터 차근차근 보겠습니다.

```python
reporter = Reporters.objects.get(name='Tintin')
reporter.stories_filed += 1
reporter.save()
```

먼저 이 경우에는 총 두 번의 쿼리가 수행됩니다.  
바로 처음으로 이름이 Tintin인 리포터를 찾는 쿼리가 수행되고, 두 번째로 그 리포터의 stories\_filed를 1 증가시키는 쿼리가 수행됩니다.  
또한 이 경우에는 stories\_filed에 값을 증가시키면서 메모리에 적재되는 과정도 이루어집니다.  
  
이 부분들을 최적화 하기 위해 F 객체를 사용할 수 있습니다.

{% tabs %}
{% tab title="첫 번째 방법" %}
```python
from django.db.models import F

reporter = Reporters.objects.get(name='Tintin')
reporter.stories_filed = F('stories_filed') + 1
reporter.save()
```
{% endtab %}

{% tab title="두 번째 방법" %}
```python
from django.db.models import F

reporter = Reporters.objects.filter(name='Tintin')
reporter.update(stories_filed=F('stories_filed') + 1)
```
{% endtab %}

{% tab title="세 번째 방법" %}
```python
from django.db.models import F

Reporter.objects.all().update(stories_filed=F('stories_filed') + 1)
```
{% endtab %}
{% endtabs %}

이 부분은 몰랐던 점인데 F 객체는 save를 한 이후에도 계속 지속이 된다고 합니다.  
그래서 첫 번째 방법의 코드에서 save를 두 번 수행하면 stories\_filed의 증가가 두 번 이루어지게 된다고 합니다.  
이를 막기 위해서는 refresh\_from\_db\(\)를 사용하여 모델 객체를 저장한 뒤 리로드하면 이런 지속성을 방지할 수 있다고 합니다.  
  
만약 F 객체를 이용하여 필드들을 결합할때 필드들의 타입이 다른 경우엔 ExpressionWrapper로 감싸야합니다.  
아래는 그 예시입니다.

```python
from django.db.models import DateTimeField, ExpressionWrapper, F

Ticket.objects.annotate(
    expires=ExpressionWrapper(
        F('active_at') + F('duration'),
        output_field=DateTimeField()
    )
)
```

만약 F 객체를 활용하여 ForeignKey를 F 객체로 별칭을 다는 경우에는 그 인스턴스 대신에 그 PK 값을 가져오게 됩니다.  
아래는 그 예시입니다.

```python
from django.db.models import F

car = Company.objects.annotate(built_by=F('manufacturer'))[0]
car.manufacturer
# <Manufacturer: Toyota>
car.built_by
# 3
```

마지막으로 F 객체를 이용하여 특정 필드의 null 값의 순서를 지정할 수 있습니다.  
아래는 그 예시입니다.

```python
from django.db.models import F

Company.objects.order_by(F('last_contacted').desc(nulls_last=True))
```

위의 코드대로면 last\_contacted 필드를 기준으로 null값은 뒤에 정렬이 되게 됩니다.

