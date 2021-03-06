# OOP\(Object Oriented Programming\)

### 객체지향이란 무엇일까?

객체지향은 데이터를 추상화시켜 속성\(field\)과 기능\(method\)을 가진 객체로 만들어 객체들간의 상호작용으로 프로그램을 구성하는 프로그래밍 패러다임입니다.

### 객체지향의 특성은?

1. 추상화 추상화에 대해 찾아보면 보통 두 가지가 많이 보입니다. - 공통의 속성이나 기능을 묶어 이름을 붙이는 것 - 목적과 관련이 없는 부분을 제거하여 필요한 부분만 표현하는 것 이는 결국 어떠한 데이터를 필요한 부분들과 공통적인 부분들을 묶어 추상화 한다고 할 수 있습니다. 만약 교수, 학생 이라는 데이터가 있을때 이 두 데이터는 각각 교수, 학생이라는 클래스로 정의될 수 있으며 **교수**라는 클래스에는 이름, 전화번호라는 속성을를 가질 수 있으며 강의하다라는 행위를 가질 수 있고 **학생**이라는 클래스에는 이름, 전화번호, 강의를듣다라는 행위를 가질 수 있습니다. 이렇게 각각의 데이터에 필수적인 속성과와 행위를 묶는것을 추상화라 할 수있습니다. 이러한 추상화의 문제점은 정해진 답이 존재하지 않는다는 것이고 보는 관점에 따라 전혀 다른 방식으로 추상화가 이루어질 수 있다는 점이 있으며, 다른 객체들과 구분이 되는 핵심적인 특징에만 집중하여 불필요한 요소들을 제거하여 복잡도를 관리할 수 있다는 장점도 있습니다.
2. 캡슐화 캡슐화를 하는 중요한 목적은 은닉화를 위함입니다. 은닉화를 하는 이유는 객체의 속성를 보호하기 위함인데 특정 속성에 외부에서 접근하여 변경할 수 없게 데이터를 보호하는 것입니다. 하지만 캡슐화가 곧 은닉화는 아니며 캡슐화는 추상화를 통해 정의된 속성들을 가지고 특정한 목적을 위한 기능을 구현하기 위해 메서드와 변수를 묶어 하나의 기능 즉 메서드를 구현하는 것을 의미합니다. 캡슐화를 통해 우리는 모듈내 응집도를 올릴 수 있고 외부에서는 접근할 수 있는 속성나 메소드가 제한됨으로써 약한 결합력을 얻게 됩니다. \(이번 프로그래머스 웹 프론트엔 과제를 거치며 결합도 관련해 지적을 받았다...\)
3. 재사용성\(상속\) 재사용성은 간단히 말해 데이터들 간에도 공통되는 속성과 기능이 존재하는데 이를 상속이라는 개념으로 이미 따로 정의된 공통 속성과 기능을 이용할 수 있게 하는 것입니다. 추상화에서 설명했던 교수와 학생 클래스를 이용하여 설명하면 교수와 학생은 둘다 사람이고 이 둘에게는 이름과 전화번호라는 공통된 속성이 존재합니다. 그렇다면 이 둘의 공통되는 이름과 전화번호라는 속성을 사람이라는 클래스를 정의하여 그 클래스가 이 두 속성을 가지게하고 교수와 학생이 사람이라는 클래스를 상속받으면 교수와 학생 모두 이름과 전화번호라는 속성을 가지게 됩니다. 여기서 오버라이딩이라는 개념이 나왔는데 부모 클래스의 메서드를 자식 클래스에서 재정의 하는 것을 말합니다.
4. 다형성 다형성은 하나의 요소가 상황에 따라 다른 의미로 해석될 수 있는것을 의미합니다. 예를들면 위의 재사용성에서 표현한 사람이라는 클래스에 식사하다라는 행위로 밥을 먹다를 정의해두었다고 가정하면 교수라는 클래스에서 식사하다를 상속받아 교직원식당에서 밥을 먹다로 재정의할 수 있으며 학생이라는 클래스에서는 식사하다를 학생식당에서 밥을 먹다로 재정의할 수 있습니다. 이를 **오버라이딩**이라하며 재정의라합니다. 하지만 항상 교수님들이 교직원식당에서 밥을먹거나 학생들이 학생식당에서 밥을 먹는것은 아니므로 어디서 식사하는지를 넘겨줄 수 있습니다. 하지만 어디서 식사를 하는지 값도 같이 넘겨줄 수 있지만 넘겨주지 않을수도 있습니다. 이렇게 식사하다라는 하나의 행위에 인자나 자료형에 따라 다른 기능을 수행할 수 있는것을 **오버로딩**이라고 합니다.

### 객체지향의 장점은?

* 강한 응집력과 약한 결합도.
* 재사용성, 다른이가 작성한 클래스를 그대로 이용하거나 상속받아 사용할 수 있다.
* 약한 결합도로 인한 유지보수가 용이하다.
* 모듈화 하기 쉬워 업무분담이 용이하다.

### 객체지향의 단점은?

* 처리속도가 상대적으로 느리다.
* 객체가 많아지면 용량이 커질 수 있다.
* 설계시 많은 노력과 시간이 필요하다.

### SOLID란?

* SRP \(단일 책임 원칙\) : 하나의 클래스는 하나의 책임만 가져야한다, DB 정규화와 비슷, 낮은 결합도와 높은 응집도를 추구하는 것, 변화에 대한 유연성을 확보하기 위함.
* OCP \(개방-폐쇄 원칙\) : 모듈은 확장에는 열려있으나 변경에는 닫혀있어야한다.
* LSP \(리스코프 치환 원칙\) : 자식 클래스는 언제나 부모 클래스를 대체할 수 있어야한다는 원칙, 인터페이스만 알면 구현체를 알지 못해도 사용이 지장이 없어야함, 다형성과 관련.
* ISP \(인터페이스 분리 원칙\) : 하나의 일반적인 인터페이스보다 여러개의 구체적인 인터페이스가 더 낫다.
* DIP \(의존성 역전 원칙\) : 상위 모듈이 하위 모듈에 의존해서는 안된다.

### 추상 클래스와 인터페이스 차이는?

추상 클래스는 그 자체로는 클래스의 역할을 수행할 수 없는 클래스이지만 추상 메서드를 가지고 다른 클래스의 기본 바탕이 되는 부모 클래스로 사용되는 클래스입니다.

추상 클래스에서 말한 추상 메서드란 선언만 되어 있고 구현부가 작성되어 있지 않은 것을 말하는데 추상 클래스는 이러한 추상 메서드를 자식 클래스에서 구현을 강제함으로써 미완성된 클래스를 완성시키는데 초점을 맞춘 것입니다.

인터페이스는 추상 클래스와는 좀 다르게 모든 메서드가 곧 추상 메서드여야 합니다.

인터페이스는 추상 클래스보다 더 추상화 되어 있기 때문에 구현부를 가진 메서드나 멤버 변수를 가질 수 없으며 추상 메서드나 상수만 멤버로 가질 수 있습니다.

또한 인터페이스는 인터페이스만을 상속받을 수 있으며 다중 상속이 가능합니다.

