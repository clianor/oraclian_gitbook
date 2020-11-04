# each

이번 강의에선 \_each 함수를 작성하게 되며 이 함수는 iterable한 객체와 iter라는 인자를 받게 됩니다.

이 \_each 함수는 반복을 돌며 하는 작업을 완전히 위임하는 함수입니다.

```javascript
// _filter, _map으로 리팩토링
function _filter(list, predi) {
  var new_list = [];
  _each(list, function(val) {
    if (predi(val)) {
      new_list.push(val);
    }
  })
  return new_list;
}

function _map(list, mapper) {
  var new_list = [];
  _each(list, function(val) {
    new_list.push(mapper(val));
  });
  return new_list;
}

function _each(list, iter) {
  for (var i = 0; i < list.length; i++) {
    iter(list[i]);
  }
  return list;
}

// 1. 30세 이상인 users를 거른다.
var over_30 = _filter(users, function(users) { return users.age >= 30; })
console.log(over_30);

// 2. 30세 이상인 users의 names를 수집한다.
var names = _map(over_30, function(user) { return user.name; })
console.log(names);

// 3. 30세 미만인 users를 거른다.
var under_30 = _filter(users, function(users) { return users.age < 30; });
console.log(under_30);

// 4. 30세 미만인 users의 ages를 수집한다.
var ages = _map(under_30, function(user) { return user.age; })
console.log(ages);

```

이렇게 \_each 함수를 작성함으로써 코드가 점점 간결해지고 명령적인 코드가 숨게되어 보다 선언적이며 단순하며, 안정적인 코드를 작성하기 쉬워진다는 장점이 있습니다.

