# useNetwork

```jsx
import React, { useEffect, useState } from "react";
import "./styles.css";

const useNetwork = (onChange) => {
  const [state, setState] = useState(navigator.onLine);
  const handleChange = () => {
    if (typeof onChange === "function") {
      onChange(navigator.onLine);
    }
    setState(navigator.onLine);
  };
  useEffect(() => {
    window.addEventListener("online", handleChange);
    window.addEventListener("offline", handleChange);
    return () => {
      window.removeEventListener("online", handleChange);
      window.removeEventListener("offline", handleChange);
    };
  }, []);
  return state;
};

export default function App() {
  const handleNetworkChange = (online) => {
    console.log(online ? "We just went online" : "We are offline");
  };
  const onLine = useNetwork(handleNetworkChange);
  return (
    <div className="App">
      <h1>{onLine ? "OnLIne" : "OffLine"}</h1>
    </div>
  );
}

```

{% embed url="https://codesandbox.io/s/usenetwork-0z0dt?fontsize=14&hidenavigation=1&theme=dark" %}



