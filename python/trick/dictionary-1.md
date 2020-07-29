# Dictionary를 정렬하는 방법

오늘은 PyTricks Dictionary를 정렬하는 방법에 관한 메일이 날아왔습니다.  
그런데 이 내용은 파이썬을 쓰는 사람이라면 아마 거의 알 내용이네요.

```python
# Dictionary 선언 및 초기화
dict_value = {'a': 4, 'b': 3, 'c': 2, 'd': 1}

# 정렬을 하는 기준을 value로 잡음
sorted(dict_value.items(), key=lambda x: x[1])
# result: [('d', 1), ('c', 2), ('b', 3), ('a', 4)]

# 위와 같으나 operator 함수를 사용하면 아래와 같이 사용할 수 있습니다.
import operator
sorted(dict_value.items(), key=operator.itemgetter(1))
# result: [('d', 1), ('c', 2), ('b', 3), ('a', 4)]

```

operator 모듈을 임포트하고 직접 연산을 호출해주면 성능상 이점이 있다고 하네요.  
하지만 테스트를 해보았을때 눈에 띄는 엄청난 차이는 보기 힘들었습니다.  
조금이라도 성능을 올려아 하는 부분이 필요할때 유용할 것 같습니다.  


