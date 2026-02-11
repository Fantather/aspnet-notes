#### `ThenBy` / `ThenByDescending`
[[Order By]]
- Используется **для вторичной сортировки**, когда первичная сортировка даёт одинаковые значения.
    
- Применяется после `OrderBy` или `OrderByDescending`.
```c#
var result = context.Products
    .OrderBy(p => p.Category)           // сначала сортируем по категории
    .ThenBy(p => p.Price)               // внутри категории — по цене
    .ToList();

```

#### Примечания:

- Можно цепочкой добавлять несколько `ThenBy` для многокритериальной сортировки.
    
- Работает как в **LINQ to Objects**, так и в **EF Core**, генерируя SQL `ORDER BY`.

Then By сортировка по имени компании:

![[Pasted image 20250911204401.png]]