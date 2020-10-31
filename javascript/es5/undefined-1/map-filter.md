# 회원목록, map, filter

### 데이터 셋

```javascript
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
```

### 명령형 코드

```javascript
// 1. 30세 이상인 users를 거른다.
var temp_users = [];
for (var i = 0; i < users.length; i++) {
  if (users[i].age >= 30) {
    temp_users.push(users[i]);
  }
}
console.log(temp_users);

// 2. 30세 이상인 users의 names를 수집한다.
var names = [];
for (var i = 0; i < temp_users.length; i++) {
  names.push(temp_users[i].name);
}
console.log(names);

// 3. 30세 미만인 users를 거른다.
var temp_users = [];
for (var i = 0; i < users.length; i++) {
  if (users[i].age < 30) {
    temp_users.push(users[i]);
  }
}
console.log(temp_users);

// 4. 30세 미만인 users의 ages를 수집한다.
var ages = [];
for (var i = 0; i < temp_users.length; i++) {
  ages.push(temp_users[i].age);
}
console.log(ages);

```

### \_filter, \_map으로 리팩토링

```javascript
// _filter, _map으로 리팩토링
function _filter(users, predi) {
  var new_list = [];
  for (var i = 0; i < users.length; i++) {
    if (predi(users[i])) {
      new_list.push(users[i]);
    }
  }
  return new_list;
}

function _map(list, mapper) {
  var new_list = [];
  for (var i = 0; i < list.length; i++) {
    new_list.push(mapper(list[i]));
  }
  return new_list;
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

응용형 함수 : 함수가 함수를 인자로 받아서 원하는 시점에 해당하는 함수가 알고 있는 인자를 적용하는 것을 응용형 함수, 응용형 프로그래밍, 적용형 프로그래밍이라고 함

고차함수 : 함수를 인자로 받거나 함수를 리턴하거나 인자로 받은 함수를 함수내에서 실행하는 함수를 말함

함수형 프로그래밍에선 대입문을 많이 사용하지 않는 경향이 있음  
대입문을 줄여 코드를 좀더 간결하게 만들고자 함  
함수형 프로그래밍에선 함수를 통과해나가면서 한 번에 값을 만들어 나가는 방식으로 프로그래밍을 수행함 \( 함수의 중첩으로 \)

```javascript
// 1. 30세 이상인 users를 거르고 names를 수집한다.
console.log(
  _map(
    _filter(users, function(users) { return users.age >= 30; }),
    function(user) { return user.name; }
  ));

// 2. 30세 미만인 users를 거르고 ages를 수집한다.
console.log(
  _map(
    _filter(users, function(users) { return users.age < 30; }),
    function(user) { return user.age; }
  ));
```

위의 코드는 대입문을 줄여 보다 안정적이고 테스트가 쉬운 코드로 변경됨

