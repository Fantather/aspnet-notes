Делегаты с возвращаемым значением
Последний тип-параметр - тип возвращаемого значения
До 16 входных параметров

```C#
public int MyMethod(Func<T1, T2, ...> func)
{
	var result = func(T1 value1, T2 value2, ... );
	return result;
}
```