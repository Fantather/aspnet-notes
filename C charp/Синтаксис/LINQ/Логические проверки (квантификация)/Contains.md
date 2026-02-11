`Contains` - определяет, содержится ли указанный элемент в последовательности

Требует реализации `GetHashCode`, а так же `Equals` или использования кастомного компаратора 

**Выполнение**
**IE** - запрос выполняется Немедленно в месте своего вызова
Не обладает явным `short-circuiting` (коротким замыканием), но может быть оптимизирован, на пример через `HashSet`

**Перегрузки**
- `Contains (TSourse item)` - использует стандартный компаратор (`EqualityComparer<TSourse>.Default`)
- `Contains (TSourse item, IEqualityComparer<TSourse> comparer)` - использует переданный компаратор для сравнения


**Пример**
```C#
var numbers = new List<int> { 1, 2, 3 };
bool containsTwo = numbers.Contains(2); // true
```

С кастомным компаратором, для сравнения строк без учета регистра
```C#
var names = new List<string> { "Alice", "Bob" };
bool hasAlice = names.Contains("alice", StringComparer.OrdinalIgnoreCase); // true
```