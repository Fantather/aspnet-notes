Специальный хук, который предназначен для выполнения побочных эффектов (например, записи данных)

Принимает исполняемую функцию и массив зависимостей (Массив при изменении значений которого функция будет выполняться снова)

Важен для обеспечения Чистоты функции, в плане вызова Сторонних Эффектов после рендера компонента ([[Архитектурные правила UI-компонентов#Чистота функции рендеринга (Render Purity)|Подробнее тут]])

**Вызов при Монтировании компонента**
Если нужно что бы переданная функция была вызвана Только при Монтировании компонента в Virtual DOM то массив зависимостей должен быть пустым

**Выполнение действий перед размонтированием** 
Нужно внутри исполняемой функции через return вернуть функцию, которая содержит нужные действия
Когда нужна отписка:
- Отписаться от прослушивания события
- Очищать таймеры
- Отписаться от различных подписок(Web Socket, Absorbal)

Заменяет ComponentUpdate, ComponentDidMount, ComponentDidUnMount у классовых компонентов

```JSX
import React, { useState, useEffect } from 'react';

function UseEffectExample() {
  const [count, setCount] = useState(0);

  // По принципу componentDidMount и componentDidUpdate:
  useEffect(() => {

    // Обновляем заголовок документа, используя API браузера
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click on me
      </button>
    </div>
  );
}

export default UseEffectExample
```


```JSX
import React, { useState, useEffect } from 'react';


export default function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```


```JSX
import React, { useState, useEffect } from 'react';

function UseEffectExample() {
  const [count, setCount] = useState(0);
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  }, [count]); // Перезапускать эффект только если count поменялся

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me!
      </button>
    </div>
  );
}

export default UseEffectExample
```