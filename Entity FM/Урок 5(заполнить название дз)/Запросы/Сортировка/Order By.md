[[Entity FM/Урок 5(заполнить название дз)/Круд операций#5) `OrderBy()` / `OrderByDescending()`]]
#### `OrderBy` / `OrderByDescending`

- Используется для **первичной сортировки** по одному полю.
    
- `OrderBy` — по возрастанию, `OrderByDescending` — по убыванию.

```c#
var result = context.Products
    .OrderBy(p => p.Price)               // сортировка по возрастанию
    .ToList();

var resultDesc = context.Products
    .OrderByDescending(p => p.Price)    // сортировка по убыванию
    .ToList();

```

![[Pasted image 20250911204307.png]]