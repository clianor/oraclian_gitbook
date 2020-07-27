# Dictionary 병합

자바스크립트를 사용하면서 spread 문법에 대해 되게 감명 깊게 느끼고 사용해 왔습니다.

파이썬에서도 비슷한 방식으로 사용할 수 있는데 아래와 같이 사용할 수 있습니다.

```python
first_dic = { 'a': 1, 'b': 2 }
second_dic = { 'b': 3, 'c': 4 }

print({ **first_dic, **second_dic })
# result: {'a': 1, 'b': 3, 'c': 4}

print({ **first_dic, **second_dic, 'c': 1 })
# result: {'a': 1, 'b': 3, 'c': 1}
```

위와 같이 사용한다면 반복문을 사용하거나 별도의 빈 딕셔너리를 선언하지 않고서 단일 표현식으로 두 Dictionary를 병합할 수 있습니다.

때때로 유용하게 사용될거 같네



