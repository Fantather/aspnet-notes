Подробнее про [[C charp/Синтаксис/LINQ/Упорядочивание/OrderBy|OrderBy]]

- Сортирует телефоны по имени по возрастанию (`A → Z`)
- SQL эквивалент: `ORDER BY Name ASC`
- Порядок теперь гарантирован благодаря `OrderBy`
```C#
using (Context db = new Context())
{
	var phones = db.Phones.OrderBy(p => p.Name);
	foreach (var phone in phones)
		Console.WriteLine($"{phone.Id}.{phone.Name} ({phone.Price})");
}
```

- Сортировка по имени по убыванию (`Z → A`)
- Вывод форматируется, чтобы имена и цены красиво выравнивались (`-30`, `-10` — это ширина поля в консоли)
```C#
using (Context db = new Context())
{
var phones = db.Phones.OrderByDescending(p => p.Name);

foreach (Phone phone in phones)
	Console.WriteLine($"{phone.Id}.{phone.Name,-30} ({phone.Price,-10})");
}
```

- Сначала сортируем по цене по возрастанию
- Если цены одинаковые, сортируем по имени компании
- SQL эквивалент: `ORDER BY Price ASC, Company.Name ASC`
- Используется для многоуровневой сортировки
```C#
using (Context db = new Context())
{
	var phones = db.Phones.OrderBy(p => p.Price).ThenBy(p => p.Company.Name);

	foreach (Phone phone in phones)
		Console.WriteLine($"{phone.Id}.{phone.Name,-30} ({phone.Price, -10})");
}
```