```jsx
import React, { useReducer, useState } from "react";

// 1. Редьюсер для todo-листа
function todoReducer(state, action) {

  switch (action.type) {

    case "add":

      return [...state, { id: Date.now(), text: action.payload, completed: false }];

    case "toggle":

      return state.map(todo =>

        todo.id === action.payload ? { ...todo, completed: !todo.completed } : todo

      );

    case "delete":

      return state.filter(todo => todo.id !== action.payload);

    default:

      return state;

  }

}

// 2. Начальное состояние

const initialTodos = [];

export default function App() {

  const [todos, dispatch] = useReducer(todoReducer, initialTodos);

  const [input, setInput] = useState("");

  const handleAdd = () => {

    if (input.trim()) {

      dispatch({ type: "add", payload: input });

      setInput("");

    }

  };

  return (

    <div style={{ padding: 20 }}>

      <h1>Todo List with useReducer</h1>

      <div style={{ marginBottom: 12 }}>

        <input

          type="text"

          value={input}

          onChange={(e) => setInput(e.target.value)}

          placeholder="Enter new todo"

          style={{ padding: 8, width: 200, marginRight: 8 }}

        />

        <button onClick={handleAdd}>Add Todo</button>

      </div>

      <ul>

        {todos.map(todo => (

          <li key={todo.id} style={{ marginBottom: 6 }}>

            <span

              onClick={() => dispatch({ type: "toggle", payload: todo.id })}

              style={{

                textDecoration: todo.completed ? "line-through" : "none",

                cursor: "pointer",

                marginRight: 8

              }}

            >

              {todo.text}

            </span>

            <button onClick={() => dispatch({ type: "delete", payload: todo.id })}>

              Delete

            </button>

          </li>

        ))}

      </ul>

    </div>

  );

}
```