Возвращает элементы первой последовательности, которых нет во второй (разность множеств), значения не повторяются

Требует перегрузки `GetHashCode`, а так же `Equals` или передачи собственного компаратора

Сложность - `O(n + m)`

**Выполнение**
`DE-B` - буферизирует вторую коллекцию в `HashSet`
Эффективно находит разность с помощью хэш-таблицы
Порядок элементов соответствует порядку в первой коллекции


**Примеры**
```C#
var list1 = new List<int> { 1, 2, 3, 4 };
var list2 = new List<int> { 2, 3 };
var result = list1.Except(list2); // { 1, 4 }
```

С кастомным компаратором
```C#
var allProducts = GetProducts();
var excludedProducts = GetExcludedProducts();
var availableProducts = allProducts.Except(excludedProducts, new ProductComparer());
```