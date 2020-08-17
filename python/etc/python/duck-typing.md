# Duck Typing

덕 타이핑은 동적 타이핑의 한 종류로 객체가 어떤 타입에 걸맞는 변수나 메소드를 가진다면 객체를 해당 타입으로 간주하는 것입니다.  
  
덕 타이핑이라는 용어는 덕 테스트에서 유래한 것으로 아래의 구절에서 유래된 것입니다.

> #### 만약 어떤 새가 오리처럼 걷고, 헤엄치고, 꽥꽥거리는 소리를 낸다면 나는 그 새를 오리라고 부를 것이다.

이러한 덕 타이핑을 파이썬으로 표현한다면 다음과 같이 표현할 수 있습니다.

```python
class Duck:
    def walk(self):
        print("뒤둥뒤뚱!")
    
    def quack(self):
        print("꽥꽥!")

class Person:
    def walk(self):
        print("이 사람이 오리처럼 걷습니다.")
    
    def quack(self):
        print("이 사람이 오리처럼 소리칩니다..")

def in_the_forest(duck):
    duck.quack()
    duck.walk()

if __name__ == "__main__":
    donald = Duck()
    john = Person()
    in_the_forest(donald)
    in_the_forest(john)

```

위의 예제에서 Person이라는 클래스로부터 생성된 인스턴스는 사람임에도 불구하고 walk와 quack 두 메소드가 존재하므로 오리로 간주되는 예시입니다.  
  
파이썬에서는 매직 메소드와 같이 다양한 곳에서 덕 파이핑을 이용하고 있습니다.  
\_\_iter\_\_\(\) 와 [\_\_getitem\_\_\(\)](https://blog.weirdx.io/post/21466)을 구현하여  iterable을 표현하는 것과 같이 사용됩니다.

