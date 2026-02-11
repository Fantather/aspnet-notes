`GroupJoin` - соединяет элементы двух коллекций на основе общего ключа
Для Каждого элемента левой последовательности создаётся  массив соответствующих элементов из правой коллекции
Если таких элементов не нашлось - массив будет пустым


**Выполнение**
`DE-B` - первым делом считывает весь источник данных в память (буферизует его), применяет к этой полной коллекции свою логику (сортирует, группирует) и только после этого начинает возвращать элементы из уже готового результата


**Сигнатура**

- `outer` - Внешняя последовательность
- `inner` - Внутренняя последовательность
  
- `outerKeySelector` - Как получить ключ из элемента
  внешней последовательности
- `innerKeySelector` - Как получить ключ из элемента внутренней последовательности
  
- `resultSelector` - Как сформировать результат

```C#
public static IEnumerable<TResult> GroupJoin<TOuter, TInner, TKey, TResult>(
    this IEnumerable<TOuter> outer,
    IEnumerable<TInner> inner,
    Func<TOuter, TKey> outerKeySelector,
    Func<TInner, TKey> innerKeySelector,
    Func<TOuter, IEnumerable<TInner>, TResult> resultSelector
)
```


**Принцип работы**
Метод принимает
- Внешнюю и Внутреннюю последовательности
- Методы которые возвращают значение ключа, для Внешней и Внутренней коллекции
- Алгоритм проходит по каждому элементу в последовательности `outer` и ищет все элементы в последовательности `inner`, у которых ключ Совпадает с ключом текущего элемента `outer`
- Для найденной пары "Элемент из `outer` - коллекция элементов из `inner` вызывается метод `resultSelector`. Эта функция принимает два аргумента:
  - Сам элемент из `outer`
  - Всю найденную коллекцию (группу) элементов из `inner` (если совпадений не было, будет передана пустая коллекция)
  - Метод возвращает коллекцию объектов `TResult`, которую сформировала функция `resultSelector`


**Пример**

`Employees` - это `IEnumerable<Employee>`
```C#
var departmentGroups = department.GroupJoin(
	employees,
	d => d.Id,
	e => e.DepartmentId,
	(department, departmentEmployees) => new
	{
		DepartmentName = department.Name,
		Employees = departmentEmployees
	}
);
```