# 일급함수, 함수로 함수 실행하기

일급 함수란 함수를 값으로 다룰 수 있는 것을 말하며, 변수에 담거나 함수의 인자로 넘겨지거나 리턴값으로 전달 할 수 있음을 말합니다.

```javascript
var f1 = function(a) {
  return a * a;
};
console.log(f1);
// ƒ (a) {
//   return a * a;
// }

var f2 = add;
console.log(f2);

function f3(f) {
  return f();
}

console.log(f3(function() { return 10; }));
// 10

console.log(f3(function() { return 20; }));
// 20
```

```javascript
// add_maker
function add_maker(a) {
  return function(b) {
    return a + b;
  }
}

var add10 = add_maker(10);
console.log(add10(20));
// 30

var add5 = add_maker(5);
var add15 = add_maker(15);

console.log(add5(10));
// 15
console.log(add15(10));
// 25
```

위의 add\_maker는 일급함수의 함수가 함수를 리턴할 수 있음을 이용한 기능이며, 클로저를 활용한 예제입니다.

add\_maker의 내부에 존재하는 함수는 순수 함수의 조건을 만족하기 때문에 순수 함수입니다.

클로저 : 내부 함수가 외부 함수의 컨텍스트에 접근할 수 있을 것을 말함.  
               외부 함수의 실행이 끝나서 외부함수가 소멸되더라도 내부 함수가  
               외부 함수의 지역 변수에 접근할 수 있는 매커니즘

```javascript
function f4(f1, f2, f3) {
  return f3(f1() + f2());
}

console.log(f4(
  function() { return 2; },
  function() { return 1; },
  function(a) { return a * a; }
));
// 9
```

함수가 함수들을 인자로 받아서 원하는 시점에 원하는 인자로 받은 함수를 받아둔 인자를 이용해 로직을 완성해 나가는 것이며, 순수 함수들을 만들고 이러한 순수 함수들을 조합해 함수들의 평가 시점이나 평가 방법들과 같은 큰 로직을 만들어 나가는 방식을 함수형 패러다임이라 할 수 있다.

