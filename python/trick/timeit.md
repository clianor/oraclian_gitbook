# timeit 모듈을 이용한 코드 실행시간 측정

timeit 모듈을 이용하면 특정 코드의 실행시간을 측정할 수 있습니다.  
기본적으로 가비지 컬렉터도 비활성화한채로 측정하기에 거의 정확한 수치로 측정할 수 있으면 매우 작은 단위까지 측정이됩니다.  
  
timeit 에는 대표적인 두 함수가 존재하는데 바로 timeit과 repeat입니다.  
timeit과 repeat는 기능은 거의 동일하지만 다른점은 repeat는 timeit을 여러번 수행한 값을 리스트로 리턴해준다는 점이 다릅니다.  
  
이 두 함수는 아래와 같이 사용할 수 있습니다.

```python
#
import timeit

def test():
    return "-".join(str(n) for n in range(100))

print(timeit.timeit('test()', setup='from __main__ import test', 
                    number=10000))
# result: 0.1899744
print(timeit.repeat('test()', setup='from __main__ import test', 
                    number=10000, repeat=5))
# result: [0.19007759999999999, 0.18938719999999998,
#          0.1905635, 0.19035120000000005, 0.1903842]
```

이때 넘겨주는 코드와 setup을 해주는 파라미터가 스트링으로 넘어가는 모습을 볼 수 있는데 이는 아마 콜백으로 받아 처리하는 것이 아닌 timeit 내부에서 eval로 코드를 실행해주는것이 아닌가 싶습니다.  
  
내가 만든 코드가 있을때 time 모듈을 임포트하여 테스트할 수도 있지만 매우 간단하게 매우 많은 횟수의 반복을 통해 코드의 전체 수행시간을 측정할 수 있다는 점에서 매우 강력한 기능입니다.

