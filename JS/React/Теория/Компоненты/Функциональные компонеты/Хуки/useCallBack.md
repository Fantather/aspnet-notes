useCallBack - позволяет мемоизировать callBack

JS перерисовывает всё что есть в render при малейшем изменении, useCallBack оптимизирует это
Он гарантирует, что функция будет создана заново только в случае изменения зависимостей

То есть основная причина использования useCallBack это оптимизация дочерних компонентов, которые завёрнуты в useCallBack



В этом примере компонент button не перерисовывается
```JSX
import React, { useState, useCallback, useEffect } from "react";

function App() {
  return (
    <div>
      <ParentComponent />
    </div>
  );
}

export default App;

function ParentComponent() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState("");
  
  const handleCountClick = useCallback(() => {
    setCount((c) => c + 1);
  }, []);
  const handleTextChange = (e) => {
    setText(e.target.value);
  };

  return (
    <div>
      <h2>Counter: {count}</h2>
      <Button
        onClick={handleCountClick}
        label={`Increment (Count: ${count})`}
      />
      <hr />
      <h3>Changing the text triggers the parent render:</h3>
      <input
        type="text"
        value={text}
        onChange={handleTextChange}
        placeholder="Enter text"
      />
    </div>
  );
}

const Button = React.memo(function Button({ onClick, label }) {
  console.log("Button rendered");
  return <button onClick={onClick}>{label}</button>;
});
```