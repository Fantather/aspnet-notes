## **`Include`**

- `Include` позволяет **[[Жадная загрузка (Eager Loading)|жадно]] загружать связанные сущности** вместе с основной сущностью.
    
- Без `Include` EF Core **не подгружает связанные объекты** сразу, и при обращении к ним будет выполняться отдельный SQL-запрос (если включена [[Ленивая загрузка|ленивная загрузка]]) или будет `null`.
    
- Используется для **навигационных свойств** (связей `1:1`, `1:М`, `М:М`).

Пример
```c#
class Order
{
    public int Id { get; set; }
    public Customer Customer { get; set; }       // связь 1:1
    public ICollection<OrderItem> OrderItems { get; set; } // связь 1:М
}

class Customer
{
    public int Id { get; set; }
    public string Name { get; set; }
}

class OrderItem
{
    public int Id { get; set; }
    public Product Product { get; set; }
}

class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

## **Простой пример Include**

```c#
var orders = context.Orders                    
.Include(o => o.Customer)                     
.ToList();
```

- **Что делает:** загружает **заказы вместе с заказчиками** одним запросом.
    
- SQL примерно:
```sql
SELECT o.*, c.*
FROM Orders o
LEFT JOIN Customers c ON o.CustomerId = c.Id
```

## **Include для коллекций**
```c#
var orders = context.Orders
                    .Include(o => o.OrderItems)
                    .ToList();

```
- Загружает заказы с коллекцией OrderItems.
    
- Каждая коллекция подгружается **с отдельным JOIN или отдельным запросом** в зависимости от стратегии EF Core.
## **Комбинация Include**

```C#
var orders = context.Orders                     
.Include(o => o.Customer)                     
.Include(o => o.OrderItems)                     
.ToList();
```
`
- Загружаются сразу **несколько навигационных свойств** (заказчики + позиции заказа).
    

---

## **Include + ThenInclude**

- Если внутри включённой коллекции есть свои связи, используем `ThenInclude`.
    

```C#
var orders = context.Orders     
.Include(o => o.OrderItems)         
.ThenInclude(oi => oi.Product)     
.Include(o => o.Customer)     
.ToList();
```

- Подгружает: **заказ → позиции заказа → продукт каждой позиции** + заказчика.
    

---

## **Важные моменты**

1. `Include` работает только с **==IQueryable==**.
    
2. Можно делать **несколько Include** на разные навигационные свойства.
    
3. Без Include связанные данные не подгружаются (если нет Lazy Loading).
    
4. При большом количестве вложенных Includes стоит проверять SQL-запросы, чтобы избежать **слишком больших JOIN’ов**.
