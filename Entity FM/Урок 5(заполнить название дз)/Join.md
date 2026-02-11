### Group Join (аналог LEFT JOIN)

**Синтаксис (query syntax):**

```c#
var query = from c in context.Categories
            join p in context.Products
            on c.Id equals p.CategoryId into productsGroup
            select new 
            {
                CategoryName = c.Name,
                Products = productsGroup   // все продукты этой категории
            };

```
Объяснение:

- `into productsGroup` — создаёт **группу связанных элементов**.
    
- Возвращает все записи из левой таблицы (`Categories`) даже если совпадений нет (аналог `LEFT JOIN`).
    
- `Products` — коллекция, можно использовать `ToList()`, `Count()`, `Any()` и т.д.
![[Pasted image 20250911204558.png]]
результат - [[Entity FM/Урок 5(заполнить название дз)/Запросы/LINQ to entities/Select]]
___
Разница с [[Entity FM/Урок 5(заполнить название дз)/Запросы/Include]] и Load
мы там брали все сразу, а тут конкретные поля и самостоятельно делаем наборы
___

### через методы расширения
```c#
var query = context.Categories
    .GroupJoin(
        context.Products,
        c => c.Id,
        p => p.CategoryId,
        (c, productsGroup) => new 
        {
            CategoryName = c.Name,
            Products = productsGroup
        })
    .ToList();

```
![[Pasted image 20250911204844.png]]

___
**Примечания:**

- Отличие от обычного `Join`: обычный `Join` возвращает только совпадения (`INNER JOIN`), а `Group Join` — все из левой таблицы и связанные элементы.
    
- Часто используется вместе с `SelectMany` для имитации `LEFT JOIN ... ON ...`.