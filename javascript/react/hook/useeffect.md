# useEffect

useEffect는 컴포넌트가 마운트되거나 언마운트, 업데이트 될때 특정 작업을 처리는 함수입니다.  
class형에서는 componentDidMount, componentDidUpdate와 같은 동작을 한다고 볼 수 있습니다.

useEffect는 아래와 같이 사용할 수 있습니다.

```jsx
import React, { useState, useEffect } from "react";

function Button() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
    console.log(document.title);
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

위의 코드를 실행하여 Click을 클릭하게 되면 title이 계속 바뀜과 동시에 console창에 title이 같이 찍히게 되는 것을 볼 수 있습니다.

위의 코드에서는 useEffect로 인해서 클릭할때마다 계속 title이 변함과 동시에 console에 찍히는 문제를 해결하기 위해 mount 되었을때만 실행하고자 한다면 아래와 같이 코드를 작성할 수 있습니다.

```jsx
import React, { useState, useEffect } from "react";

function Button() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
    console.log(document.title);
  }, []);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

이 둘의 차이는 useEffect 함수의 두 번째 인자로 빈 배열을 넘기느냐 넘기지 않느냐밖에 보이지 않습니다.

두 번째 인자로 빈 배열을 넘기면 리액트에서는 useEffect 함수가 prop이나 state의 그 어떤 값에도 의존하지 않고 마운트 될때가 아니라면 effect의 잦은 실행을 방지할 수 있습니다.

만약 effect가 특정 상태나 props의 변화에 맞춰 실행이 되어야 한다면 빈 배열 대신에 변수를 담아서 실행해주면 됩니다.

만약 useEffect에서 componentWillUpdate\(\)나 componentWillUnmount\(\)의 작업을 수행하고 싶다면 cleanup 함수를 반환해주면 됩니다.

이는 다음과 같이 작성할 수 있습니다.

```jsx
import React, { useState, useEffect } from "react";

function Button() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
    console.log(document.title);

    return () => {
      console.log("cleanup");
      console.log(`CleanUp You clicked ${count} times`);
    }
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

만약 위의 코드를 실행하고 Click me 버튼을 클릭한면 콘솔창에선 아래와 같은 모습을 볼 수 있습니다.

![](../../../.gitbook/assets/image%20%285%29.png)



