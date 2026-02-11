Это оператор приведения типа с безопасной проверкой
Если приведение невозможно вернёт `null` без вызова исключения

```C#
object? obj;
Employee other = obj as Employee;
```
Пытаемся привести `Object` к `Employee`

```C#
object? obj;
if((Employee as obj) == null)
{
	return false;
}
```
Если привести к `Employee` не удалось, то `as obj` вернёт `null`