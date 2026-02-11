**Проблема:** React пытается отрисовать компонент мгновенно, но данные приходят с сервера через секунду. Попытка сделать `items.map` или деструктуризацию `{ title } = item` приведет к ошибке `Cannot read property of undefined`

**Решение:** Ранний возврат. Если данных нет — прерываем выполнение функции

```JSX
export default function ItemDetails() {
    const [item] = useFetch(...);

    // 1. Защита (Early Return)
    if (!item) return <div>Загрузка...</div>;

    // 2. Если код дошел сюда, item точно существует
    const { title, price } = item; 
    
    return <div>{title}</div>;
}
```