*Join*
- Соединяет две последовательности по общему ключу
- Создает плоский список пар элементов, у которых совпали ключи

Подробнее о [[C charp/Синтаксис/LINQ/Объединение/Join/Join|Join]]

Пример
```C#
using (Context db = new Context())
{
    var phones = db.Phones.Join(db.Companies,
        p => p.CompanyId,
        c => c.Id,
        (p, c) => new 
        {
            Name = p.Name,
            Company = c.Name,
            Price = p.Price
        });

    foreach (var p in phones)
        Console.WriteLine("{0} ({1}) - {2}", p.Name, p.Company, p.Price);
}
```
- Используется метод `Join` LINQ для соединения таблиц `Phones` и `Companies`
- `p => p.CompanyId` — поле в первой таблице (`Phones`), по которому будет связь
- `c => c.Id` — поле во второй таблице (`Companies`), с которым сравниваем
- `(p, c) => new { ... }` — результат соединения: проецируем в анонимный объект с нужными свойствами
- `foreach` выводит каждый объект на консоль: `Имя телефона (Компания) - Цена`


Аналог через синтаксис запросов
```C#
var phones = from p in db.Phones
             join c in db.Companies on p.CompanyId equals c.Id
             select new { Name = p.Name, Company = c.Name, Price = p.Price };
```


Пример
```C#
using (Context db = new Context())
{
	var result = from phone in db.Phones
				 join company in db.Companies on phone.CompanyId equals company.Id
				 join country in db.Countries on company.CountryId equals country.Id
				 select new
				 {
					 Name = phone.Name,
					 Company = company.Name,
					 Price = phone.Price,
					 Country = country.Name
				 };

	foreach (var p in result)
		Console.WriteLine("{0} ({1}) - {2} - {3}", p.Name, p.Company, p.Price, p.Country);
}
```
- Делает соединение 3-х таблиц (`Phones → Companies → Countries`).
- Результат проецируется в анонимный объект с дополнительным полем `Country`