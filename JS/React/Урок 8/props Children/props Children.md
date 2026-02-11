`props Children` - Позволяет создавать Закрывающиеся теги
В примере элемент modal принимает children
Сам элемент создаёт модальное окно

Но что оно будет содержать передаётся через children

```JSX
import React from "react";
import "./assets/css/modal.css";

function App() {
  const [isModalOpen, setIsModalOpen] = React.useState(true);

  return (
    <div>
      <h1>My Website</h1>
      <button className="open-btn" onClick={() => setIsModalOpen(true)}>
        Open Modal
      </button>
      <Modal isOpen={isModalOpen}>
        <button className="close-btn" onClick={() => setIsModalOpen(false)}>
          ❌ Close
        </button>
        <h3>Welcome to Wildlands!</h3>
        <p>Registration is required to continue.</p>
        <form>
          <input type="email" placeholder="Enter email" />
        </form>
      </Modal>
    </div>
  );
}

export default App;

// Дочерний компонент

function Modal({ children, isOpen }) {
  if (!isOpen) return null;
  return (
    <div className="modal-overlay">
      <div className="modal-content">{children}</div>
    </div>
  );
}
```