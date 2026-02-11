Любая операция, которая взаимодействует с внешним миром (API, DOM)

Плохой код
```JSX
export default function App() {

  const userId = 1;

  // ПЛОХОЙ КОД — побочный эффект непосредственно в теле компонента!

  fetch(`api/users/${userId}`)

    .then((res) => res.json())

    .then((data) => console.log(data));

  return (

    <div>

      <h1>User Profile</h1>

      <p>

        Rendering a side effect directly in the component body is bad practice

        because it runs on every render.

      </p>

    </div>

  );

}
```


**Хороший код**
```JSX
import React, { useEffect, useState } from "react";

export default function App() {
  const [user, setUser] = useState(null);
  const userId = 1; // 🇷🇺 Для примера используем фиксированный ID

  // ХОРОШО: выполняем реальный запрос к API после рендера
  useEffect(() => {
    // Изолированный побочный эффект: загрузка данных пользователя
    fetch(`[https://jsonplaceholder.typicode.com/users/](https://jsonplaceholder.typicode.com/users/ "https://jsonplaceholder.typicode.com/users/")${userId}`)
      .then((res) => res.json())
      .then((data) => setUser(data))
      .catch((err) => console.error("Loading error:", err));
  }, [userId]);

  return (
    <div style={{ padding: 20 }}>
      <h1>User Profile</h1>
      {/* Показываем состояние загрузки */}
      {!user && <p>Loading user...</p>}
      {/* Если данные загружены, отображаем их */}
      {user && (
        <pre
          style={{
            background: "#f4f4f4",
            padding: 16,
            borderRadius: 12,
            marginTop: 16,
          }}
        >
          {JSON.stringify(user, null, 2)}
        </pre>
      )}
    </div>
  );
}
```



