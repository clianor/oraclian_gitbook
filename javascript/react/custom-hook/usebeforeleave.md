# useBeforeLeave

```jsx
import React, { useEffect } from "react";
import "./styles.css";

const useBeforeLeave = (onBefore) => {
  if (typeof onBefore !== "function") {
    return;
  }

  const handle = (event) => {
    const { clientY } = event;
    if (clientY <= 0) {
      onBefore();
    }
  };

  useEffect(() => {
    document.addEventListener("mouseleave", handle);

    return () => {
      document.removeEventListener("mouseleave");
    };
  });
};

export default function App() {
  const begForLife = () => console.log("Pls dont leave");
  useBeforeLeave(begForLife);

  return (
    <div className="App">
      <h1>Hello </h1>
    </div>
  );
}

```

{% embed url="https://codesandbox.io/s/usebeforeleave-e5891?fontsize=14&hidenavigation=1&theme=dark" %}



