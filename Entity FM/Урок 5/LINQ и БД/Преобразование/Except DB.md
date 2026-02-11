Возвращает элементы первой последовательности, которых нет во второй (разность множеств), значения не повторяются

Подробнее про [[Except]]

Берёт только те телефоны, которые удовлетворяют оба условия одновременно (дешевле 50 000 и Samsung)
```C#
using (Context db = new Context())
{
	var selector1 = db.Phones.Where(p => p.Price < 50000);
	var selector2 = db.Phones.Where(p => p.Name.Contains("Samsung"));
	var phones = selector1.Except(selector2);

	foreach (var item in phones)
		Console.WriteLine(item.Name);
}
```