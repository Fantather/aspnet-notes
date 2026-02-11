```JSX
import React from "react";

function App() {
  return (
    <div>
      <ProductList />
    </div>
  );
}

export default App;

// Компонент DataLoader (Провайдер Логики)
function DataLoader({ url, children }) {
  const [data, setData] = React.useState(null);
  const [loading, setLoading] = React.useState(true);

  // Логика загрузки данных
  React.useEffect(() => {
    setLoading(true);
    fetch(url)
      .then((r) => r.json())
      .then((data) => {
        setData(data);
        setLoading(false);
      })
      .catch(() => {
        setData([]);
        setLoading(false);
      });
  }, [url]);

  if (loading) {
    return <p>Loading...</p>;
  }

  return children(data);
}

// ------------------------------------------------------------------

// Потребитель Логики (Consumer)
function ProductList() {
  return (

    <DataLoader url="[https://fakestoreapi.com/products"](https://fakestoreapi.com/products%22 "https://fakestoreapi.com/products%22")>

      {(products) => (
        <div>
          <h2>Products ({products.length})</h2>
          <ul style={{ lineHeight: "1.8" }}>
            {products.map((p) => (
              <li key={p.id}>
                {p.title} — {p.price}$
              </li>
            ))}
          </ul>
        </div>
      )}
    </DataLoader>
  );
}
```