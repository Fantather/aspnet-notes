Возвращает общие элементы двух последовательностей (логическое И множеств), не повторяет значения

Подробнее про [[Intersect]]


```C#
using (Context db = new Context())
{
	var phones = db.Phones.Where(p => p.Price < 50000)
		.Intersect(db.Phones.Where(p => p.Name.Contains("Samsung")));
	
	foreach (var item in phones)
		Console.WriteLine(item.Name);
}
Console.ReadLine();
```