В стандартном CSS (`import './style.css'`) все классы попадают в **глобальную** область видимости.
Из за этого могут возникнуть конфликты имен

# Поэтому
В CSS Modules имена классов и анимаций имеют **локальную** область видимости по умолчанию.
___
![[Pasted image 20251212203041.png]]

```jsx
import styles from './ModuleHeader.module.css';

  

function ModuleHeader(props) {

  return <header className={styles.header}>{props.title}</header>;

}

export default ModuleHeader;
```



- Стили применяются только к тому компоненту, в который они импортированы.
    
- Можно не бояться использовать простые имена (например, `.container`, `.title`, `.active`), так как они не будут конфликтовать с такими же именами в других компонентах.

___
ОНИ УЖЕ СКОМПИЛИРОВАННЫЕ   - 
преобразованные сборщиком 
**Как это работает:** При сборке проекта инструмент берет имя класса из CSS и добавляет к нему уникальный хеш.
не
```css

    .error { color: red; }

```

а

```html

<button class="Button_error__ax7LZ">Button</button>

```

- `Button` — имя файла/компонента.
    
- `error` — исходное имя класса.
    
- `ax7LZ` — уникальный хеш (гарантирует уникальность).

вот так на страничке
```html
```
<button style="color: red">Button</button>
```
```

___

Что бы оно работало то наш модуль стиля мы называем

`НазваниеКомпонента.module.css`

а если  точнее то нам надо именно 
`.module.css` и название

затем в jsx необходимо
```jsx
import styles from './ModuleHeader.module.css';
```

```jsx
function ModuleHeader(props) {
  return <header className={styles.header}>{props.title}</header>;
}

export default ModuleHeader;
```

```jsx

/*Module CSS Lesson*/
import React from 'react';
import ModuleHeader from "./components/exampleComponents/ModuleHeader.jsx";
class App extends React.Component {
  titleText = "Hello world!";

  helpText = "Help text.";

  

  render() {

    return (

      <div>
        <ModuleHeader title="Module Header Text" />
      </div>

    );

  }
}

  
export default App;
```