# 한 번에 여러 플래그를 테스트하는 방법

파이썬뿐만 아니라 여러 언어들에서 한번에 여러 플래그를 테스트 하는 방법들이 있습니다.

그중 파이썬만의 독특한 함수와 기법들이 탄생했습니다.

아래는 파이썬에서 하나의 표현식으로 여러 플래그를 테스트 하는 방법의 예시입니다.

```python
x, y, z = 0, 1, 0

if x == 1 or y == 1 or z == 1:
    print('passed')

if 1 in (x, y, z):
    print('passed')

if x or y or z:
    print('passed')

if any((x, y, z)):
    print('passed')
    
if not all((x,y,z)):
    print('passed')
```

또한 파이썬에서는 and와 or를 이용해서 삼항 연산자를 사용한 방법도 있습니다.

```python
a, b = 1, 2

print(a == b and a or b)
# result: 2

print(a != b and a or b)
# result: 1
```

이렇게 사용할 수 있는 이유는 x == 0 또는 x == 1와 같은 조건이 참인지 거짓인지에 따라 거짓이라면 and의 뒷 부분이 실행될 이유가 없기에 x == 1과 같은 False 값이 and의 좌측에 오는 경우 and의 우측은 무시되고 False를 반환합니다.  
그리고 or 는 좌측값과는 무관하게 우측이 실행되므로 리턴값이 1이되는 기법입니다.

과거엔 저도 이런 기법을 사용했었지만 가독성이 좋지 않으므로 `True if 조건식  else False`와 같은 방식을 쓰기를 추천합니다.

