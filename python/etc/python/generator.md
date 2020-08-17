# Generator

파이썬에서 제너레이터는 제터레이터 함수가 호출될때 이터러블한 객체를 반환하는 이터레이터의 일종입니다.  
제너레이터 함수나 일반적인 함수와 사용법은 거의 동일하나 다른 점은 yield 구문을 이용하여 next\(\)로 호출될때마다 원하는 시점에 데이터를 반환하는 함수입니다.  
보통 함수는 한번 호출이 되고 나면 다시 진입할 수 없으나 제너레이터 함수는 한 번 호출이 된 이후로도 더 반환할 데이터가 없을때까지는 계속 진입할 수 있다는 점이 일반적인 함수와는 다릅니다.

```python
def multi_yield():
    yield_str = "This will print the first string"
    yield yield_str
    yield_str = "This will print the second string"
    yield yield_str
    
for i in multi_yield():
    print(i)
    
# 출력
# This will print the first string
# This will print the second string

generator_value = multi_yield()
print(next(generator_value))
print(next(generator_value))

# 출력
# This will print the first string
# This will print the second string
```

제너레이터는 위와 같이 yield 구문을 이용해 여러번 진입할 수 있으며 for in 구문을 이용할 수도 있으며 next\(\) 함수를 두 번 호출함으로써 for in 과 같은 결과를 볼 수 있습니다.  
  
이러한 제너레이터의 장점은 필요할때마다 필요한 연산을 수행 후 값을 받을 수 있기 때문에 모든 값을 메모리에 미리 적재하고 있을 필요가 없다는 점이고 이러한 점 때문에 파이썬 2점대 버전에서는 range 대신에 xrange를 사용하도록 추천되고 있었으나 3점대 버전에 들어오면서 부터는 xrange가 range로 대체되었기 때문에 range를 사용해도 무방합니다.  
단 range는 실제 내부 구현은 제너레이터와 동일하지만 실제로 호출하여 사용할때는 제너레이터가 아니므로 next와 같은 제너레이터 함수들을 사용할 수 없습니다.

