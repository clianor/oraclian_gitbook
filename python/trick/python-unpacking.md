# Python에서 함수의 인자를 unpacking 하는 방법

Python에서는 언패킹을 통해 함수에 값을 전달할 수 있습니다.  
또한 함수에서는 위치인자와 키워드인자라는 용어가 있는데 이는 값이 위치에 매핑되면 **위치인자** 그리고 키워드에 의해 매핑된다면 **키워드인자**라고 부릅니다.  
  
아래의 코드를 먼저 보겠습니다.

```python
def myFunc(x, y, z):
    print(x, y, z)

tuple_vec = (1, 2, 3)
dic_vec = {'x': 1, 'y': 2, 'z': 3}

myFunc(*tuple_vec)
# result: 1 2 3
myFunc(**dic_vec)
# result: 1 2 3
myFunc(*[1,2], **{'z': 3})
# result: 1 2 3

```

파이썬에서는 Asterisk의 갯수로 각각의 언패킹을 수행할 수 있는데 만약 한개일 경우 함수의 위치인자로 들어가게 됩니다.  
그리고 Asterisk의 갯수가 두개인 경우에는 키워드인자로 들어가게됩니다.  
  
이러한 언패킹 기법을 이용하여 아래와 같은 방식으로도 사용할 수 있습니다.

```python
dict_vec = {'x': 1, 'y': 2, 'z': 3}

print(*dict_vec.keys())
# result: x y z
print(*dict_vec)
# result: x y z

tuple_vec = [1, 2 ,3, 4, 5]
print(tuple_vec)
# result: [1, 2, 3, 4, 5]
print(*tuple_vec)
# result: 1 2 3 4 5

a, *b, c = tuple_vec
print(a)
# result: 1
print(b)
# result: [2, 3, 4]
print(c)
# result: 5

```



