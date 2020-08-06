# json.dumps를 활용하여 Python dict를 이쁘게 출력하는 방법

파이썬에서는 json이라는 기본 모듈이 존재합니다.  
이 모듈은 파이썬 내에서 json 형식의 문자열을 Python Object로 변환하거나 Python Object를 json 형태의 문자열로 변환하는 기능을 가지고 있습니다.  
  
json.dumps를 사용하면 Python Object를 json 형태의 문자열로 변환합니다. \( json 인코딩 \)  
json.loads를 사용하면 json 형태의 문자열을 Python Object 형태로 변환합니다. \( json 디코딩 \)

```python
my_mapping = {'a': 23, 'b': 42, 'c': 0xc0ffee}
print(my_mapping)
# {'b': 42, 'c': 12648430. 'a': 23}

import json
print(json.dumps(my_mapping, indent=4, sort_keys=True))
# {
#     "a": 23,
#     "b": 42,
#     "c": 12648430
# }

```

