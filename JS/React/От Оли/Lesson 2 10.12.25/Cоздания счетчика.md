![[Pasted image 20251210194134.png]]
![[Pasted image 20251210194144.png]]

this.state count - хранит текущее состояние

для инкремента и декремента мы вещаем on click и через метод set state мы вешаем состояние

prevstate -  хранит прошле состояние. Менять состояние лусше на основе преведущего
и мы его значение увеличиваем на один (это лучше чем писать просто count ибо иначе будет полная каша)

```jsx
import React from "react";

function App() {

  return (

    <div>

      <h1>First Lesson</h1>

      <Counter />

    </div>

  );

}

class Counter extends React.Component {

  constructor(props) {

    super(props);

    // Инициализация состояния компонента

    this.state = {

      count: 0

    };

  }

  // Метод для увеличения счетчика

  increment = () => {

    this.setState((prevState) => ({

      count: prevState.count + 1

    }));

  };

  // Метод для уменьшения счетчика

  decrement = () => {

    this.setState((prevState) => ({

      count: prevState.count - 1

    }));

  };

  render() {

    return (

      <div>

        <h1>{this.props.title}</h1>

        <p>Current Count: {this.state.count}</p>

        <button onClick={this.increment}>Increment</button>

        <button onClick={this.decrement}>Decrement</button>

      </div>

    );

  }

}

export default App;
```