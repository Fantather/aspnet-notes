![[Pasted image 20251205201232.png]]![[Pasted image 20251205201658.png]]
как оно выглядит в jsx и в js

jsx это невалидный js 
но очень удобный для написания интерфейса

сборщиком проэкта он преобразовывается в синтаксис js

![[Pasted image 20251205201909.png]]

jsx похож на html
он чуть упрощает так восприятие

jsx позвроляет встраивать переменные используя фигурные скобки
```jsx
function App() {

const element = <h1>Hello, world!</h1>;
 const name = 'Alex';
 const component = <Welcome name="Sara" />;
  return (
    <div>
      <h1>First Lesson</h1>
      <p>{element}</p>
      <p>Hello, {name}!</p>
      <Welcome name='John'/>
      {component}
      <User />
    </div>

  );

}

  

function Welcome(props){
  return <h1>Hello, {props.name}</h1>;

}

  
  

function User() {
  const user = {
    avatarUrl:      "https://t3.ftcdn.net/jpg/05/17/79/88/360_F_517798849_WuXhHTpg2djTbfNf0FQAjzFEoluHpnct.jpg",
  };

  const element = <img src={user.avatarUrl} alt="User Avatar" height="50px" />;
  const divStyle = {
    backgroundColor: "blue",
    color: "white",
  };

  const element2 = <div style={divStyle}>Hello World!</div>;
  return (
    <div>
      {element}
      {element2}
    </div>
  );

}
export default App;
```

components можно использовать вызывая или встроенные
props - можно передать любое количество елементов

[[virtual DOM]]