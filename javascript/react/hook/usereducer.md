# useReducer

useReducer는 useState를 대체할 수 있는 함수입니다.

리듀서는 다음 state가 이전 state에 의존적인 경우에 주로 사용되는데 콜백 대신에 dispatch를 이용하여 컴포넌트의 성능을 최적화 할 수 있습니다.

리덕스를 사용해봤다면 리덕스에서 사용할때와 크게 다르지 않은 모습을 볼 수 있습니다.  
아래는 useEffect에서 사용한 예제를 useReducer를 이용하여 다시 작성하였습니다.

```jsx
import React, { useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "INCREMENT":
      return { count: state.count + 1 };
    default:
      return state;
  }
}

function Button() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <div>
      <p>You clicked {state.count} times</p>
      <button onClick={() => dispatch({ type: "INCREMENT" })}>Click me</button>
    </div>
  );
}
```

리액트에서는 dispatch 함수가 리렌더링 시에도 변경되지 않으리라는 것을 보장하기 때문에 useEffect나 useCallback 의존성 목록에 포함시키지 않아도 괜찮습니다.

dispatch를 사용할때 값도 같이 넘기는 방법이 존재합니다.  
dispatch 함수를 사용하여 값을 넘길때 type으로 액션의 타입을 지정하고 payload로 값을 넘겨주면 됩니다.

```jsx
import React, { useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "INCREMENT":
      return { count: state.count + 1 };
    case "ADD":
      return { count: state.count + action.payload.add };
    default:
      return state;
  }
}

function Button() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <div>
      <p>You clicked {state.count} times</p>
      <button onClick={() => dispatch({ type: "INCREMENT" })}>Click me</button>
      <button onClick={() => dispatch({ type: "ADD", payload: { add: 2 } })}>+2</button>
    </div>
  );
}
```

위의 내용을 통해 리듀서를 사용할때의 장점을 알 수 있는데 상태의 변경에 관한 부분이 컴포넌트의 외부로 분리되어 있다는 점입니다.

이로써 하나의 컴포넌트가 방대해지는걸 방지할 수 있습니다.

하지만 위의 코드에 아직도 문제가 있습니다.

원래 리듀서에서 하나의 액션이 하나의 값을 생성해주더라도 다른 값은 그대로 보존하고 있어야하는데 위의 방식대로라면 값을 유지해야하는 경우도 직접 다 작성해주어야 하는 문제가 있습니다.

이제 위에서 작성한 코드를 한번더 수정을 해보겠습니다.

```jsx
import React, { useReducer } from "react";

function reducer(state, action) {
  switch (action.type) {
    case "INCREMENT":
      return {
        ...state,
        count: state.count + 1
      };
    case "ADD":
      return { 
        ...state,
        count: state.count + action.payload.add
      };
    default:
      return state;
  }
}

function Button() {
  const [state, dispatch] = useReducer(reducer, { count: 0 });

  return (
    <div>
      <p>You clicked {state.count} times</p>
      <button onClick={() => dispatch({ type: "INCREMENT" })}>Click me</button>
      <button onClick={() => dispatch({ type: "ADD", payload: { add: 2 } })}>
        +2
      </button>
    </div>
  );
}
```

reducer의 내부에서 리턴해주는 값이 스프레드 연산자를 이용하면 state에서 관리하는 값이 늘어나도 문제가 없습니다.

