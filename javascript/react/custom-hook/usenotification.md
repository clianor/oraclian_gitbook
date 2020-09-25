# useNotification

```jsx
import React from "react";
import "./styles.css";

const useNotification = (title, options) => {
  if (!("Notification" in window)) {
    return;
  }

  const fireNotif = () => {
    Notification.requestPermission().then((permission) => {
      if (permission === "granted") {
        new Notification(title, options);
      } else {
        return;
      }
    });
  };
  return fireNotif;
};

export default function App() {
  const triggerNotif = useNotification("Can I steal your kimchi?", {
    body: "I love kimchi dont you"
  });

  return (
    <div className="App">
      <button onClick={triggerNotif}>Hello</button>
    </div>
  );
}

```

{% embed url="https://codesandbox.io/s/usenotification-28whb?fontsize=14&hidenavigation=1&theme=dark" %}



