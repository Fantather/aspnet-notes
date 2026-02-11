Делегат для конвертирования одного типа в другой
Первым указываем принимаемый тип, потом возвращаемый `Converter<Tinput, Toutput>`
Подходящая сигнатура - `Toutput MyConverter(Tinput)`

```C#
string MyConverter(int value)
{
	return value.ToString();
}

List<string> ConvertList(List<int> collection, Converter<int, string> conv)
{
	List<string> result = [];
	foreach(int item in collection)
		result.Add(conv(item));
	
	return result;
}
```