# useTabs

```jsx
import React, { useState } from "react";
import "./styles.css";

const content = [
  {
    tab: "Section 1",
    content: "I'm the content of the Section1"
  },
  {
    tab: "Section 2",
    content: "I'm the content of the Section2"
  }
];

const useTabs = (initialTab, allTabs) => {
  if (!allTabs || !Array.isArray(allTabs)) {
    return;
  }

  const [currentIndex, setCurrentIndex] = useState(initialTab);

  return {
    currentItem: allTabs[currentIndex],
    changeItem: setCurrentIndex
  };
};

export default function App() {
  const { currentItem, changeItem } = useTabs(0, content);

  return (
    <div className="App">
      <h1>
        {content.map((section, index) => (
          <button onClick={() => changeItem(index)}>{section.tab}</button>
        ))}
        <div>{currentItem.content}</div>
      </h1>
    </div>
  );
}

```

{% embed url="https://codesandbox.io/s/usetabs-e1rc1?fontsize=14&hidenavigation=1&theme=dark" %}



