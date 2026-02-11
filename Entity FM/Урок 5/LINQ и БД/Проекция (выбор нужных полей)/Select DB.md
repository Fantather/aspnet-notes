Подробнее про [[C charp/Синтаксис/LINQ/Выбор нужных полей (проекция)/Select|Select]]

Более узко смотрим на объект
Создали коллекцию анонимных классов
```C#
using (Context db = new Context())
{
	var phones = db.Phones.Select(p => new
	{
		Nm = p.Name,
		Compn = p.Company.Name
	});
	
	foreach (var phone in phones)
		Console.WriteLine($"{phone.Nm,-30} - {phone.Compn, -20}");
}
```

Создаём коллекцию объектов `ModelPhone`
```C#
using (Context db = new Context())
{
	var phones = db.Phones.Select(p => new ModelPhone
	{
		Name = p.Name,
	    Price = p.Price,
    });
    
	foreach (var phone in phones)
		Console.WriteLine($"{phone.Name} ({phone.Price})");
}
```