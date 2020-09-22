# useClick

componentDidMount 때만 클릭 이벤트를 추가하고 componentWillUnMount 일때 이벤트 리스너를 제거하는 cleanup을 작성한 훅입니다.

```jsx
import React, { useRef, useEffect } from "react";
import "./styles.css";

const useClick = (onClick) => {
  const element = useRef();
  useEffect(() => {
    if (element.current) {
      element.current.addEventListener("click", onClick);
    }

    return () => {
      if (element.current) {
        element.current.removeEventListener("click", onClick);
      }
    };
  }, []);
  return element;
};

export default function App() {
  const onClick = () => console.log("say Hello");

  const title = useClick(onClick);

  return (
    <div className="App">
      <h1 ref={title}>Hello</h1>
    </div>
  );
}

```

{% embed url="https://codesandbox.io/s/naughty-lovelace-0gd57?fontsize=14&hidenavigation=1&theme=dark" %}

