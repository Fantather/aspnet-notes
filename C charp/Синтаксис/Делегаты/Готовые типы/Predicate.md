Делегат принимающий одно значение и возвращающий `bool`
Используется для проверки значения на соответствие каким-либо условиям, то есть в методах поиска или фильтрации
Аналог `Func<T, bool>`

```C#
int[] SortOdd(int[] arr, Predicate<int> pred)
{
	List<int> result = [];
	foreach(int item in arr)
		if(pred(item))
			result.Add(item);
	
	return result.ToArray();
}
```


Предикаты принимают только один параметр, но если необходимо принять несколько, то можно сгруппировать их в Кортеже и доставать оттуда
А ещё лучше, использовать `Func` с несколькими параметрами
```C#
public Predicate<(double a, double b, double c)> MyPred = sides => sides.a == sides.b && sides.b == sides.c;
```