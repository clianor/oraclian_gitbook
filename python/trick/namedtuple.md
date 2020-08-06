# namedtuple를 이용한 클래스 대안

namedtuple를 이용하여 클래스의 대체제로 사용할 수 있습니다.  
namedtuple와 클래스를 직접 선언하여 사용할때와의 차이점은 크게 불변성과 관련이 있습니다.  
아래의 코드를 보면 알 수 있지만 namedtuple를 이용하면 클래스처럼 필드에 접근할 수 있으나 단지 그 필드의 값을 수정할 수 없다는 점이 보이는데 이는 튜플의 불변성 때문입니다.

```python
from collections import namedtuple

Car = namedtuple('Car', 'color mileage')

print(Car)
# <class '__main__.Car'>

my_car = Car('red', 3812.4)
print(my_car.color, my_car.mileage)
# red 3812.4

print(my_car)
# Car(color='red', mileage=3812.4)

my_car.color = 'blue'
# AttributeError: can't set attribute
```



