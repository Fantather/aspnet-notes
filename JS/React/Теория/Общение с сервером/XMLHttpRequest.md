Устаревшая, Сложно читать (В сравнении), плохо работает со сложным JSON (Это не все минусы)
Имеет смысл использовать только если нужно отслеживать процесс загрузки (Есть современные аналоги, которые позволяют это сделать)

```JSX
import React from "react";

function App() {
  return (
    <div>
      <h1>Example of work XMLHttpRequest</h1>
      <button onClick={getDataOldSchool}>Get data</button>
      <pre id="output"></pre>
    </div>
  );
}

function getDataOldSchool() {
  const xhr = new XMLHttpRequest();
  xhr.open("GET", "[https://jsonplaceholder.typicode.com/posts/1"](https://jsonplaceholder.typicode.com/posts/1%22 "https://jsonplaceholder.typicode.com/posts/1%22"));
  xhr.onload = function () {
    if (xhr.status === 200) {
      const result = JSON.parse(xhr.response);
      document.getElementById("output").textContent =
        "Success:\n" + JSON.stringify(result, null, 2);
      console.log("Успех:", result);
    } else {
      document.getElementById("output").textContent =
        "Server error: " + xhr.status;
      console.error("Server error:", xhr.status);
    }
  };

  xhr.onerror = function () {
    document.getElementById("output").textContent =
      "Network error (the Internet may be unavailable)";
    console.error("Network error (Internet connection lost)");
  };

  xhr.send();
}

export default App;
```