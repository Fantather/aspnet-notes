`EF.Functions.Like` позволяет использовать **SQL-оператор `LIKE`** прямо в LINQ-запросах

Это удобно, если хочешь искать строки с шаблоном, например: `%text%`, `text%`, `%text`

```C#
using (Context db = new Context())
{
	var phones = db. Phones. Where(p =>
		EF.Functions.Like(p. Name, "%Galaxy%"));
		
	foreach (Phone phone in phones)
		Console.WriteLine($"{phone.Name} ({phone. Price})");
}
```