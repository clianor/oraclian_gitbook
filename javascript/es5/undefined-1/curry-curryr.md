# 커링, curry, curryr

### 커링이란?

커링은 함수와 인자를 다루는 기법으로써 단일 호출로 처리하는 함수를 함수의 인자를 하나씩 채워가며 필요한 인자가 모두 채워지면 함수의 본체를 실행하는 기법입니다.

커리 함수를 이용하면 일반적으로 작성한 함수를 커링이 되도록 만들어 줄 수 있습니다.

```javascript
function _curry(fn) {
  return function(a) {
    return function(b) {
      return fn(a, b);
    }
  }
}

var add = function(a, b) {
  return a + b;
};

console.log(add(10, 5));
// 15

var add = _curry(function(a, b) {
  return a + b;
});

var add10 = add(10);
console.log(add10(5));
// 15

console.log(add(10)(5));
// 15

function _curry(fn) {
  return function(a, b) {
    return arguments.length == 2 ? fn(a, b) : function(b) {
      return fn(a, b);
    }
  }
}

var add = function(a, b) {
  return a + b;
};

console.log(add(1, 2));
// 3
```

함수의 인자가 뒤에서부터 가야하는 경우 \_curryr 함수를 만들어 사용할 수 있습니다.

```javascript
function _curryr(fn) {
  return function(a, b) {
    return arguments.length == 2 ? fn(a, b) : function(b) {
      return fn(b, a);
    }
  }
}

var sub = _curryr(function(a, b) {
  return a - b;
});
console.log(sub(10, 5));
// 5

var sub10 = sub(10);
console.log(sub10(3));
// -7
```

### \_get

자바스크립트에서 오브젝트의 특정 키값을 가져오고자 할때 코드의 안정성을 더 높일 수 있는 방식입니다.

```javascript
function _get(obj, key) {
  return obj == null ? undefined : obj[key];
}

var users = [{name : '1ilsang', value : 5}, {name : '2ilsang', value : 15}];
var user1 = users[0];
console.log(user1.name);
console.log(_get(user1, 'name'));

// console.log(users[10].name);  //err
console.log(_get(users[10], 'name')); //undefined
```

\_get을 이용하면 error가 나지 않고 undefined를 반환하도록 작성할 수 있습니다.

\_get도 커리를 이용할 수 있습니다.

```javascript
var _get = _curryr(function(obj, key) {
  return obj == null ? undefined : obj[key];
});
var users = [{name : '1ilsang', value : 5}, {name : '2ilsang', value : 15}];
var user1 = users[0];

console.log(user1.name);
// 1ilsang
console.log(_get(user1, 'name'));
// 1ilsang
console.log(_get('name')(user1));
// 1ilsang

var get_name = _get('name');
console.log(get_name(user1));
// 1ilsang
```



