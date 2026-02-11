Позволяет мемоизировать(кешировать) данные

Передаём хук Функцию, которую нужно вызывать при изменении отслеживаемого значения и сами отслеживаемые значения, завёрнутые в массив (Учитель сказал что это не массив, просто он так написал)
Возвращает результат переданной функции
`const memoizedFactorial = useMemo(() => calculateFactorial(number), [number]);`


Мемоизация позволяет сохранить результат тяжёлой, ресурсоёмкой операции
Если зависимости не изменятся, то операция пересчитываться не будет

Функция создатель и массив зависимостей, за которым мы следим



Пример
```JSX
//App.jsx


import { useState, useMemo } from "react";

function App() {

  return (

    <div>

      <FactorialCalculator />

    </div>

  );

}

export default App;

function calculateFactorial(n) {

  console.log("We calculate the factorial...");

  // Имитация задержки

  let result = 1;

  for (let i = 1; i <= n; i++) {

    result *= i;

  }

  return result;

}

function FactorialCalculator() {

  const [number, setNumber] = useState(5);

  const [count, setCount] = useState(0);

  // 1. ИСПОЛЬЗУЕМ useMemo:

  // Значение `memoizedFactorial` будет пересчитано только при изменении `number`.

  // Изменение `count` не вызовет повторного вычисления!

  const memoizedFactorial = useMemo(() => calculateFactorial(number), [number]);

  return (

    <div>

      {/* Секция факториала */}

      <h3>🔢 Calculating factorials</h3>

      <input

        type="number"

        value={number}

        onChange={(e) => setNumber(Number(e.target.value))}

      />

      <p>

        Factorial of a number {number} = **{memoizedFactorial}**

      </p>

      <hr />

      {/* Секция счетчика */}

      <h3>Independent meter</h3>

      <button onClick={() => setCount(count + 1)}>

        Increase the counter (Count: {count})

      </button>

      <p>The counter changes, but the factorial is not recalculated.</p>

    </div>

  );

}
```


В этом коде, если убрать Мемоизацию, то при клике на кнопку счётчика, Факториал тоже будет пересчитываться





Оптимизация дочернего компонента, что бы он не перерисовывался при любом изменении родителя
```jsx
import React, { useState, useMemo } from "react";

function App() {

  return (

    <div>

      <ParentComponent />

    </div>

  );

}

export default App;

// Дочерний компонент, который оптимизирован

const OptimizedChild = React.memo(({ data }) => {

  console.log("ChildComponent redrawn!");

  return (

    <div>

      <h4>Subordinate component</h4>

      <ul>

        {data.map((item, index) => (

          <li key={index}>{item.name}</li>

        ))}

      </ul>

    </div>

  );

});

function ParentComponent() {

  const [text, setText] = useState("");

  const [counter, setCounter] = useState(0);

  // БЕЗ useMemo: Массив будет создаваться заново при каждом рендере,

  // что приведет к перерисовке OptimizedChild, даже если counter не меняется.

  // const data = [{ name: `Элемент ${counter}` }];

  // 2. ИСПОЛЬЗУЕМ useMemo:

  // Массив `data` будет создан заново ТОЛЬКО при изменении `counter`.

  // Изменение `text` не приведет к созданию нового массива,

  // и OptimizedChild не будет перерисован.

  const data = useMemo(() => [{ name: `Element ${counter}` }], [counter]);

  return (

    <div>

      <h2>Parental component</h2>

      <input

        type="text"

        value={text}

        onChange={(e) => setText(e.target.value)}

        placeholder="Enter text (will trigger parent rendering)"

      />

      <p>Text: {text}</p>

      <button onClick={() => setCounter((c) => c + 1)}>

        Increase counter (Changes child props)

      </button>

      <hr />

      {/* Передаем мемоизированный массив */}

      <OptimizedChild data={data} />

    </div>

  );

}
```




