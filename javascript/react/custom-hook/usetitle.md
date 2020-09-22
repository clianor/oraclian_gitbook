# useTitle

이건 그다지 실용적인 훅인지 잘 모르겠다.

helmet을 쓸거 같은데..

```jsx
import React, { useState, useEffect } from "react";
import "./styles.css";

const useTitle = (initialTitle) => {
  const [title, setTitle] = useState(initialTitle);
  const updateTitle = () => {
    const htmlTitle = document.querySelector("title");
    htmlTitle.innerText = title;
  };

  useEffect(updateTitle, [title]);

  return setTitle;
};

export default function App() {
  const titleUpdater = useTitle("Loading...");

  useEffect(() => {
    titleUpdater("Hello");
  }, []);

  return (
    <div className="App">
      <h1>Hello</h1>
    </div>
  );
}

```

{% embed url="https://codesandbox.io/s/usetitle-r0s7o?fontsize=14&hidenavigation=1&theme=dark" %}



