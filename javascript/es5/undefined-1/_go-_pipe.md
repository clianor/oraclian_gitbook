# 파이프라인, \_go, \_pipe, 화살표 함수

### \_pipe 함수

함수들을 받아 함수들을 연속적으로 실행해주는 함수.

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

function _pipe() {
  var fns = arguments;
  return function(arg) {
    return _reduce(fns, function(arg, fn) {
      return fn(arg);
    }, arg);
  };
}

var f1 = _pipe(
  function(a) { return a + 1; },
  function(a) { return a * 2; },
  function(a) { return a * a; });

console.log(f1(1));
// 16
```

\_pipe는 결국 \_reduce라 할 수 있지만 \_pipe의 좀 더 추상화된 개념이라 할 수 있습니다.

이 \_pipe 함수는 함수를 리턴하는 함수라고 할 수 있습니다.

### \_go 함수

\_\_pipe는 함수를 리턴하는 함수라 할 수 있으며 \_\_go 함수는 함수를 리턴하지 않는 \_\_pipe 함수의 즉시 실행 버전이라고 할 수 있습니다.

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

function _pipe() {
  var fns = arguments;
  return function(arg) {
    return _reduce(fns, function(arg, fn) {
      return fn(arg);
    }, arg);
  };
}

function _go(arg) {
  var fns = _rest(arguments);
  return _pipe.apply(null, fns)(arg);
}

_go(
  1,
  function (a) { return a + 1; },
  function (a) { return a * 2; },
  function (a) { return a * a; },
  console.log);
// 16
```

### users에 \_go 적용

```javascript
// 데이터
var users = [
  { id: 1, name: "ID", age: 36 },
  { id: 2, name: "BJ", age: 32 },
  { id: 3, name: "JM", age: 32 },
  { id: 4, name: "PJ", age: 27 },
  { id: 5, name: "HA", age: 25 },
  { id: 6, name: "JE", age: 26 },
  { id: 7, name: "JI", age: 31 },
  { id: 8, name: "MP", age: 23 }
];

function _curryr(fn) {
  return function(a, b) {
    return arguments.length == 2 ? fn(a, b) : function(b) {
      return fn(b, a);
    }
  }
}

var _filter = _curryr(function(list, predi) {
  var new_list = [];
  _each(list, function(val) {
    if (predi(val)) {
      new_list.push(val);
    }
  })
  return new_list;
});

var _map = _curryr(function(list, mapper) {
  var new_list = [];
  _each(list, function(val) {
    new_list.push(mapper(val));
  });
  return new_list;
});

function _each(list, iter) {
  for (var i = 0; i < list.length; i++) {
    iter(list[i]);
  }
  return list;
}

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

function _pipe() {
  var fns = arguments;
  return function(arg) {
    return _reduce(fns, function(arg, fn) {
      return fn(arg);
    }, arg);
  };
}

function _go(arg) {
  var fns = _rest(arguments);
  return _pipe.apply(null, fns)(arg);
}

var _get = _curryr(function(obj, key) {
  return obj == null ? undefined : obj[key];
});

// 기존 방식
_go(
  users,
  function(users) {
    return _filter(users, function(user) { return user.age >= 30; });
  },
  function(users) {
    return _map(users, function(user) { return _get(user, "name") });
  },
  console.log);
// ["ID", "BJ", "JM", "JI"]

// 함수형 프로그래밍 방식으로 리팩토링한 방
_go(
  users,
  _filter(function(user) { return user.age >= 30; }),
  _map(_get("name")),
  console.log);
// ["ID", "BJ", "JM", "JI"]
```

지금까지 작성된 함수들과 커링을 이용하면 위와 같이 코드를 매우 간결하게 표현할 수 있습니다.

