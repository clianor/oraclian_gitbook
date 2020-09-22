# useInput

React Hooks는 리액트 컴포넌트가 아닌 다른 함수로 작성하여도 로직을 처리할 수 있습니다.

```jsx
import React, { useState } from "react";
import "./styles.css";

const useInput = (initialValue, validator) => {
  const [value, setValue] = useState(initialValue);

  const onChange = (event) => {
    const {
      target: { value }
    } = event;
    let willUpdate = true;

    if (typeof validator === "function") {
      willUpdate = validator(value);
    }

    if (willUpdate) {
      setValue(value);
    }
  };

  return { value, onChange };
};

export default function App() {
  const maxLen = (value) => value.length <= 10;
  const notEmail = (value) => !value.includes("@");
  const name = useInput("Mr.", maxLen);
  const name2 = useInput("Mr.", notEmail);

  return (
    <div className="App">
      <h1>Hello</h1>
      <input placeholder="Name" {...name} />
      <input placeholder="Name2" {...name2} />
    </div>
  );
}

```

{% embed url="https://codesandbox.io/embed/useinput-bq02f?fontsize=14&hidenavigation=1&theme=dark" %}







