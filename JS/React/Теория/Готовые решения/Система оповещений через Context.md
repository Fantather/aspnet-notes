Вот готовый конспект в стиле Obsidian. Он содержит только суть: архитектуру, код и ключевые моменты, чтобы ты мог быстро восстановить систему с нуля.

---

## Архитектура системы

1. **NotificationContext**: Глобальное состояние (массив уведомлений) и логика (добавление, таймеры, удаление).
    
2. **NotificationList**: Компонент-оркестратор. Слушает массив и рендерит список через _Portal_.
    
3. **NotificationItem**: "Глупый" компонент. Отображает контент и анимацию таймера.
    
4. **CSS Animation**: Отвечает за плавное уменьшение полоски времени (без JS-интервалов).
    

---

## Реализация

### 1. Подготовка HTML

Файл: index.html

Добавь контейнер для портала рядом с корнем приложения. Это нужно, чтобы уведомления не зависели от верстки основного приложения.

HTML

```
<body>
  <div id="root"></div>
  <div id="notificationsRoot"></div>
</body>
```

### 2. Логика (Context + Provider)

Файл: store/NotificationContext.jsx

Содержит стейт и методы управления.

- **id**: Генерируем через `crypto.randomUUID()` для уникальности.
    
- **Оптимизация**: При удалении проверяем наличие ID, чтобы избежать лишних ререндеров.
    
- **Таймер**: Запускается один раз при создании уведомления.
    

JavaScript

```
import { createContext, useContext, useState } from "react";
import NotificationList from "../components/NotificationList/NotificationList";

// Значения по умолчанию (заглушки)
export const NotificationContext = createContext({
  notifications: [],
  toast: () => {},
  close: () => {}
});

export default function NotificationContextProvider({ children }) {
  const [notifications, setNotifications] = useState([]);

  // Основной метод вызова
  function toast(text, duration = 3000) {
    const newNotification = {
      id: crypto.randomUUID(),
      text,
      duration
    };

    setNotifications(prev => [...prev, newNotification]);
    scheduleRemoval(newNotification.id, duration);
  }

  // Мгновенное удаление из стейта
  function close(targetId) {
    setNotifications(prev => {
       // Оптимизация: если элемента уже нет, не меняем стейт
       if (!prev.some(n => n.id === targetId)) return prev;
       return prev.filter(({ id }) => id !== targetId);
    });
  }

  // Таймер удаления (приватная функция)
  function scheduleRemoval(targetId, duration) {
    setTimeout(() => close(targetId), duration);
  }

  const contextValue = {
    notifications,
    toast,
    close
  };

  return (
    <NotificationContext.Provider value={contextValue}>
      {/* Рендерим список внутри провайдера, передаем функцию закрытия */}
      <NotificationList notifications={notifications} onItemClick={close} />
      {children}
    </NotificationContext.Provider>
  );
}

export const useNotification = () => useContext(NotificationContext);
```

### 3. Рендеринг списка (Portal)

Файл: components/NotificationList/NotificationList.jsx

Использует createPortal для переноса верстки в #notificationsRoot.

- **Замыкание в onClick**: Создаем стрелочную функцию `() => onItemClick(id)`, чтобы "запомнить" ID элемента и не передавать его в сам компонент Item.
    

JavaScript

```
import { createPortal } from "react-dom";
import NotificationItem from "../NotificationItem/NotificationItem";
import "./NotificationList.css";

export default function NotificationList({ notifications, onItemClick }) {
  const notificationsRoot = document.getElementById("notificationsRoot");

  if (!notificationsRoot) return null;

  const listJSX = notifications.map(({ id, text, duration }) => (
    <NotificationItem
      key={id}
      text={text}
      duration={duration}
      // Передаем обработчик, который уже знает ID
      onClick={() => onItemClick(id)}
    />
  ));

  return createPortal(listJSX, notificationsRoot);
}
```

### 4. Компонент уведомления (Item)

Файл: components/NotificationItem/NotificationItem.jsx

Отвечает только за визуал.

- **animationDuration**: Передаем длительность через _inline-style_, чтобы CSS-анимация синхронизировалась с JS-таймером.
    
- **onClick**: Просто вызывает переданную функцию (которая уже содержит логику закрытия).
    

JavaScript

```
import "./NotificationItem.css";

export default function NotificationItem({ text, duration, onClick }) {
  return (
    <div className="notification" onClick={onClick}>
      <div className="notification-content">{text}</div>
      <div 
        className="progress-bar" 
        style={{ animationDuration: `${duration}ms` }}
      ></div>
    </div>
  );
}
```

### 5. Стилизация (CSS)

Файл: `components/NotificationList/NotificationList.css` (общий файл)

- **#notificationsRoot**: Используем `position: fixed`, чтобы уведомления всегда были на экране при скролле.
    
- **@keyframes**: Анимация `shrink` меняет ширину от 100% до 0%.
    

CSS

```
/* Контейнер для портала */
#notificationsRoot {
  position: fixed;
  top: 20px;
  right: 20px;
  z-index: 9999; /* Поверх всего */
  display: flex;
  flex-direction: column;
  gap: 10px;
}

/* Карточка уведомления */
.notification {
  width: 300px;
  background-color: #333;
  color: white;
  border-radius: 8px;
  padding: 16px;
  position: relative;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
  cursor: pointer;
  font-family: sans-serif;
}

/* Полоска времени */
.progress-bar {
  position: absolute;
  bottom: 0;
  left: 0;
  height: 4px;
  background-color: aquamarine;
  width: 100%;
  
  /* Анимация запускается автоматически */
  animation-name: shrink;
  animation-timing-function: linear;
  animation-fill-mode: forwards;
}

@keyframes shrink {
  from { width: 100%; }
  to { width: 0%; }
}
```

### 6. Использование

Файл: `App.jsx` или любой компонент

1. Обернуть приложение в `NotificationContextProvider`.
    
2. В компоненте вызвать хук `useNotification`.
    

JavaScript

```
import { useNotification } from "./store/NotificationContext";

export default function MyComponent() {
  const { toast } = useNotification();

  const handleClick = () => {
    // Вызываем уведомление на 5 секунд
    toast("Операция выполнена успешно!", 5000);
  };

  return <button onClick={handleClick}>Показать toast</button>;
}
```