Возвращает общие элементы двух последовательностей (логическое И множеств), не повторяет значения

Требует перегрузки `GetHashCode`, а так же `Equals` или передачи собственного компаратора

Сложность - `O(n + m)`

**Выполнение**
`DE-B` - Буферизующее выполнение
Буферизирует вторую коллекцию в `HashSet`
Эффективно находит пересечение с помощью хэш-таблицы
Порядок элементов соответствует порядку в первой коллекции


**Перегрузки**
- `arr1.Intersect(arr2)` - По умолчанию используется стандартный компаратор `EqualityComparer<T>.Default`
- `arr1.Intersect(arr2, comparer)` - Поддерживает пользовательский компаратор

**Примеры**
```C#
var list1 = new List<int> { 1, 2, 3, 4 };
var list2 = new List<int> { 2, 3, 5 };
var result = list1.Intersect(list2); // { 2, 3 }
```

С кастомным компаратором
```C#
var products1 = new[] { new Product("Apple"), new Product("Banana") };
var products2 = new[] { new Product("APPLE"), new Product("Orange") };
var common = products1.Intersect(products2, new ProductNameComparer());
// Продукты с именами "Apple" (без учета регистра)
```