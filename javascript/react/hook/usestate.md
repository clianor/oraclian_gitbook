# useState

Hook의 useState는 기존에 클래스형 컴포넌트에서만 사용이 가능하던 state를 사용하기 위한 기능입니다.

useState는 함수 컴포넌트에 state를 추가할 수 있도록 해줍니다.  
사용법은 매우 간단한데 아래와 같이 사용할 수 있습니다.

```jsx
import React, { useState } from "react";

function Button() {
  const [count, setCount] = useState(0);

  const onIncrement = () => setCount(count + 1);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={onIncrement}>Click me</button>
    </div>
  );
}
```

useState 함수는 state변수와 해당 변수를 갱신할 수 있는 함수 두 쌍을 반환합니다.  
이 두 쌍을 보통 위와같이 비구조화 할당 문법을 사용하여 사용합니다.

사용은 매우 간단한 편으로 위와 같이 JSX 문법을 통해 사용하거나 갱신할 수 있는 함수를 통해 값을 변경시켜 주면 됩니다.

단지 useState 함수는 하나의 상태만 관리하기 때문에 여러 개의 상태를 관리하고 싶다면 useState를 여러번 사용해야합니다.

