
cтили определяются как обычный джава скрипт объект
где ключи имена а стили свойства

```jsx
function App() {

  return (

    <div>

      <StyledButton>Regular button</StyledButton>

      <StyledButton onClick={() => alert("The main button has been pressed!")}>

        Home button

      </StyledButton>

    </div>

  );

}

export default App;

function StyledButton(props) {

  const buttonStyles = {

    backgroundColor: "#007bff",

    color: "white",

    padding: "10px 20px",

    borderRadius: "5px",

    border: "none",

    cursor: "pointer",

    fontSize: "16px",

  };

  return (

    <button style={buttonStyles} onClick={props.onClick}>

      {props.children}

    </button>

  );

}
```

![[Pasted image 20251212203745.png]]

все стили определили как объекты
```jsx
<button

      style={{

        padding: "10px",

        color: "white",

        backgroundColor: "black",

      }}

      onClick={props.onClick}

    >

      {props.children}

    </button>
```

так можно тожн
