Позволяет использовать state в функциональных компонентах
Возвращает массив
Его первое значение это текущее значение state-переменной
Второе это функция-сеттер, которая позволяет задать state-переменной новое значение

Используется когда состояние простое


#### Пример
```JSX
import { useState } from 'react';
function UseStateExample() {

  // Объявляем новую переменную состояния "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click on me
      </button>
    </div>
  );
}

export default UseStateExample
```

`[count, setCount]` - Деструктуризация массива, для удобства использования


```jsx
import React from "react";

export function UserData({name, age}) {
    const [user, setUser] = React.useState({name: name, age: age});
    function handleNameChange(event) {
      setUser({...user, name: event.target.value});  
   }

    function handleAgeChange(event) {  
      setUser({...user, age: event.target.value});  
   }

    return (
      <div>
        <h3>Name: {user.name}</h3>
        <h3>Age: {user.age}</h3>
        <div>
          <p>Name: <input type="text" value = {user.name} onChange={handleNameChange} /></p>
          <p>Age: <input type="number" min="0" max="110" value={user.age} onChange={handleAgeChange} /></p>
        </div>
      </div>
    );
  }
```

`...user` - распаковываем объект в другой, то есть полностью его копируем, а уже потом указываем переменную, значение которой будет изменено