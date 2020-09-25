# usePercentage

노마드 코더 니꼴라스 형님의 유용한 훅 10개를 보다가 직접 유용할만한 훅 하나를 만들었습니다.

해당 엘리먼트의 화면상의 위치를 퍼센티지로 가져오는 훅입니다.

```jsx
import React, { useEffect, useRef, useState } from "react";
import "./styles.css";

const usePercentage = () => {
  const [state, setState] = useState({
    percentage: undefined
  });
  const element = useRef();

  const onScroll = () => {
    if (element.current) {
      const rect = element.current.getBoundingClientRect();
      setState({
        percentage: (rect.top / window.innerHeight) * 100
      });
    }
  };

  useEffect(() => {
    window.addEventListener("scroll", onScroll);
    return () => {
      window.removeEventListener("scroll", onScroll);
    };
  }, []);

  return { ref: element, state };
};

export default function App() {
  const { ref, state } = usePercentage();
  return (
    <div className="App" style={{ height: "1000vh" }}>
      <div style={{ backgroundColor: "black", height: "100vh" }}></div>
      <h1
        ref={ref}
        style={{
          opacity: state.percentage <= 70 && state.percentage >= 10 ? 1 : 0,
          transition: "2s",
          willChange: "opacity"
        }}
      >
        Hello
      </h1>
    </div>
  );
}

```

{% embed url="https://codesandbox.io/s/usepercentage-43bnu?fontsize=14&hidenavigation=1&theme=dark" %}



