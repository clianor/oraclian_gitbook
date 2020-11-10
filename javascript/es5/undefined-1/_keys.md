# 다형성 높이기, \_keys, 에러

## \_each의 다형성 높이기

### 1. \_each 함수 null 대응하기

길이를 확인해야하는 부분에 length라는 프로퍼티가 존재하지 않는 객체가 넘어온 경우 또는 데이터 자체가 넘어오지 않은 경우 프로퍼티에 접근시 에러가 발생하게 됩니다.

이때 사용할 수 있는 \_length 함수를 작성할 수 있습니다.  
최신 문법에서는 옵셔널 체이닝을 이용하면 똑같이 이용할 수 있습니다.

```javascript
function _curryr(fn) {
  return function(a, b) {
    return _length(arguments) == 2 ? fn(a, b) : function(b) {
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
  for (var i = 0; i < _length(list); i++) {
    iter(list[i]);
  }
  return list;
}

var slice = Array.prototype.slice;

function _rest(list, num) {
  return slice.call(list, num || 1);
}

function _reduce(list, iter, memo) {
  if(_length(arguments)=== 2) {
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

var _length = _get('length');

_each(null, console.log);
console.log(_filter(null, function(v) { return v; }));

_go(null,
    _filter(function(v) { return v % 2; }),
    _map(function(v) { return v * v; }));

```

### 2. \_keys 만들기 및 \_is\_object로 object 인지 확인하

\_keys 함수는 Object.keys는 null을 넘길 경우 에러가 나기 때문에 좀더 안정적이게 동작하도록 만든 함수입니다.

```javascript
function _is_object(obj) {
  return typeof obj === "object" && !!obj;
}

function _keys(obj) {
  return _is_object(obj) ? Object.keys(obj) : [];
}

console.log(_keys({ name: 'ID', age: 33 }));
// ["name", "age"]
console.log(_keys([1, 2, 3, 4]));
// ["0", "1", "2", "3"]
console.log(_keys(10));
// []
console.log(_keys(null));
// []
```

### 3. \_each 다형성 높이기

```javascript
function _curryr(fn) {
  return function(a, b) {
    return _length(arguments) == 2 ? fn(a, b) : function(b) {
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
  var keys = _keys(list);

  for (var i = 0; i < keys.length; i++) {
    iter(list[keys[i]]);
  }
  return list;
}

var slice = Array.prototype.slice;

function _rest(list, num) {
  return slice.call(list, num || 1);
}

function _reduce(list, iter, memo) {
  if(_length(arguments)=== 2) {
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

var _length = _get('length');

_each({
  13: 'ID',
  19: 'HD',
  29: 'YD',
}, function(name) {
  console.log(name);
});
// ID
// HD
// YD

console.log(_map({
  13: 'ID',
  19: 'HD',
  29: 'YD'
}, function(name) {
  return name.toLowerCase();
}));
// ["id", "hd", "yd"]

_go({
    13: 'ID',
    19: 'HD',
    29: 'YD'
  },
  _map(function(name) { return name.toLowerCase(); }),
  console.log);
// ["id", "hd", "yd"]

var users = [
  { id: 1, age: 36 },
  { id: 2, name: "BJ", age: 32 },
  { id: 3, name: "JM", age: 32 },
  { id: 4, name: "PJ", age: 27 },
  { id: 5, name: "HA", age: 25 },
  { id: 6, name: "JE", age: 26 },
  { id: 7, name: "JI", age: 31 },
  { id: 8, name: "MP", age: 23 }
];

/**
 * 옵셔널 체이닝 이용
 */
_go({
    1: users[0],
    3: users[2],
    5: users[4]
  },
  _map(function(user) {
    return user.name?.toLowerCase();
  }),
  console.log);
// [undefined, "jm", "ha"]
```



