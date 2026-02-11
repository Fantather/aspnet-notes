Проблемы:
1. Гонка состояний (Если пользователь будет быстро кликать по вкладкам)
2. Отсутствует кеширование
3. Нет автоматического обновления данных

Для решения этих проблем мы будем использовать библиотеку

```JSX
import React, { useState } from "react";

function App() {

  const [data, setData] = useState(null);

  const [error, setError] = useState(null);

  const [loading, setLoading] = useState(false);

  const getData = () => {

    setLoading(true);

    setError(null);

    fetch("https://jsonplaceholder.typicode.com/posts/1")

      .then((response) => {

        if (!response.ok) {

          throw new Error("Server error: " + response.status);

        }

        return response.json();

      })

      .then((result) => {

        setData(result);

      })

      .catch((err) => {

        setError(err.message);

      })

      .finally(() => {

        setLoading(false);

      });

  };

  return (

    <div>

      <h1>Example of work fetch()</h1>

      <button onClick={getData}>Get data</button>

      {loading && <p>Loading...</p>}

      {error && <pre style={{ color: "red" }}>{error}</pre>}

      {data && <pre>{JSON.stringify(data, null, 2)}</pre>}

    </div>

  );

}

export default App;
```