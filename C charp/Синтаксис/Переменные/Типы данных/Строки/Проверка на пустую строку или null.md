`string.IsNullOrEmpty(строка)` - Возвращает `true`, если строка не валидная

``` C#
string? str;
if(string.IsNullOrEmpty(str))
{
	Console.WriteLine("Err");
	return;
}
```