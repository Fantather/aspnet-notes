![[Pasted image 20250911191442.png]]

## ThenInclude

- `Include` загружает **связь первого уровня**.
    
- `ThenInclude` используется **для вложенных/глубоких связей**, т.е. когда нужно подгрузить связанные сущности **у уже включённой сущности**.
    
- Работает только после `Include` или другого `ThenInclude`

```c#
var orders = context.Orders
    .Include(o => o.OrderItems)             // загружаем все позиции заказа
        .ThenInclude(oi => oi.Product)      // и продукты внутри каждой позиции
    .Include(o => o.Customer)               // загружаем заказчика
    .ToList();

```
- **Что происходит:**
    
    1. `Include(o => o.OrderItems)` – EF Core подгружает коллекцию OrderItems для каждого заказа.
        
    2. `ThenInclude(oi => oi.Product)` – для каждого OrderItem подгружается связанный Product.
        
    3. `Include(o => o.Customer)` – отдельная связь первого уровня, Customer тоже загружается жадно.
        
- Без `ThenInclude` вложенные связи не подгружаются автоматически.


##  Важные моменты

1. `ThenInclude` используется **только после Include или другого ThenInclude**.
    
2. Можно строить цепочки нескольких уровней:
```c#
.Include(a => a.B)
    .ThenInclude(b => b.C)
        .ThenInclude(c => c.D)
```

3. Для коллекций и одиночных навигационных свойств `ThenInclude` работает одинаково.