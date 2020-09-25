# useFullscreen

이번 hook에서는 iframe에서는 안되는 것으로 보이니 새창으로 열어 보시면 됩니다.

```jsx
import React, { useRef } from "react";
import "./styles.css";

const useFullscreen = (callback) => {
  const element = useRef();

  const runCb = (isFull) => {
    if (typeof callback === "function") {
      callback(isFull);
    }
  };

  const triggerFull = () => {
    if (element.current) {
      if (element.current.requestFullscreen) {
        element.current.requestFullscreen();
      } else if (element.current.mozRequestFullScreen) {
        element.current.mozRequestFullScreen();
      } else if (element.current.webkitRequestFullscreen) {
        element.current.webkitRequestFullscreen();
      } else if (element.current.msRequestFullscreen) {
        element.current.msRequestFullscreen();
      }
      runCb(true);
    }
  };
  const exitFull = () => {
    if (document.fullscreenElement === null) {
      return;
    }

    if (document.exitFullscreen) {
      document.exitFullscreen();
    } else if (document.mozCancelFullScreen) {
      document.mozCancelFullScreen();
    } else if (document.webkitExitFullscreen) {
      document.webkitExitFullscreen();
    } else if (document.msExitFullscreen) {
      document.msExitFullscreen();
    }
    runCb(false);
  };

  return { element, triggerFull, exitFull };
};

export default function App() {
  const onFullS = (isFull) => {
    console.log(isFull ? "We are full" : "We are small");
  };
  const { element, triggerFull, exitFull } = useFullscreen(onFullS);

  return (
    <div className="App" style={{ height: "1000vh" }}>
      <div ref={element}>
        <img
          src="https://t1.daumcdn.net/liveboard/movie/fab860ebc347492089892b760d995bbc.jpg"
          alt="가오갤"
          style={{ width: "500px" }}
        />
        <button onClick={exitFull}>Exit fullscreen</button>
      </div>
      <button onClick={triggerFull}>Make fullscreen</button>
    </div>
  );
}

```

{% embed url="https://codesandbox.io/s/usefullscreen-3p9g5?fontsize=14&hidenavigation=1&theme=dark" %}



