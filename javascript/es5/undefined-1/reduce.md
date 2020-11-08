# reduce

### reduce 함수란?

reduce 함수는 재귀적으로 받은 함수를 연속적으로 실행해준 결과를 리턴해주는 함수입니다.

```javascript
function _each(list, iter) {
  for (var i = 0; i < list.length; i++) {
    iter(list[i]);
  }
  return list;
}

var add = function(a, b) {
  return a + b;
};


function _reduce(list, iter, memo) {
  _each(list, function(val) {
    memo = iter(memo, val);
  });
  return memo;
}

console.log(_reduce([1, 2, 3], add, 0));
// 6

memo = add(0, 1);
memo = add(memo, 2);
memo = add(memo, 3);
console.log(memo);
// 6
```

실제 reduce와 같이 memo 인자가 생략되어도 문제가 없도록 동작을 시키기 위해선 아래와 같이 작성할 수 있습니다.

```javascript
function _each(list, iter) {
  for (var i = 0; i < list.length; i++) {
    iter(list[i]);
  }
  return list;
}

var add = function(a, b) {
  return a + b;
};

var slice = Array.prototype.slice;

function _rest(list, num) {
  return slice.call(list, num || 1);
}

function _reduce(list, iter, memo) {
  if(arguments.length === 2) {
    memo = list[0];
    list = _rest(list);
  }

  _each(list, function(val) {
    memo = iter(memo, val);
  });
  return memo;
}

console.log(_reduce([1, 2, 3], add));
// 6
```

