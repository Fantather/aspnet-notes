Объединяет две последовательности и возвращает не повторяющиеся элементы из каждой коллекции, удаляя дубликаты (логическое ИЛИ для множеств)

Подробнее про [[Union]]

```C#
using (Context db = new Context())
{
	var phones = db.Phones.Where(p => p.Price < 50000)
		.Union(db.Phones.Where(p => p.Name.Contains("Samsung")));
	foreach (var item in phones)
		Console.WriteLine(item.Name);
}
Console.ReadLine();
```
- Берём все телефоны дешевле 50 000 и объединяем их с телефонами Samsung
- `Union` убирает дубликаты
- Порядок строк не гарантируется, потому что это SQL-запрос без `ORDER BY`


```C#
using (Context db = new Context())
{
	var result = db.Phones.Select(p => new { Name = p.Name })
		.Union(db.Companies.Select(c => new { Name = c.Name }));

	foreach (var item in result)
		Console.WriteLine(item.Name);
}
Console.ReadLine();
```
- Берём имена телефонов и имена компаний, объединяем их
- Опять `Union` убирает дубликаты
- Порядок возвращаемых строк случайный, зависит от базы, пока не применён `OrderBy`