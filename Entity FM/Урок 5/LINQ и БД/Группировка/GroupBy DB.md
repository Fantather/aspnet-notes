Это оператор, который разбивает коллекцию на группы по какому-то ключу
Элементы в группах сохраняют исходный порядок последовательности

Подробнее о [[GroupBy|GroupBy]]

Группировка по имени компании
```C#
using (Context db = new Context())
{
    var groups = db.Phones.GroupBy(p => p.Company.Name);

    foreach (var group in groups)
    {
        Console.WriteLine(group.Key); // имя компании
        foreach (var phone in group)
            Console.WriteLine($"{phone.Name} - {phone.Price}"); // вывод телефонов
    }
}
```

Группировка с последующим подсчётом количества телефонов у каждой компании
```C#
var groups1 = db.Phones.GroupBy(p => p.Company.Name)
                      .Select(g => new { Name = g.Key, Count = g.Count() });

foreach (var c in groups)
        Console.WriteLine($"Производитель: {c.Name} Кол-во моделей: {c.Count}");
```

Получение максимальной цены модели у каждой компании
```C#
var groupsMaxPrice = db.Phones.GroupBy(p => p.Company.Name)
                          .Select(g => new { Name = g.Key, MaxPrice = g.Max(p => p.Price) });

foreach (var c in groupsMaxPrice)
        Console.WriteLine($"Производитель: {c.Name} Max ціна моделей: {c.MaxPrice}");
```


Большой пример
```C#
using (Context db = new Context())
{
    // Группировка телефонов по названию компании
    var res_phgroup = db.Phones.GroupBy(p => p.Company.Name);

    foreach (var group in res_phgroup)
    {
        Console.WriteLine();
        Console.WriteLine(group.Key); // название компании
        Console.WriteLine("______________________");
        foreach (var item_phone in group)
        {
            Console.WriteLine(item_phone.Name); // названия телефонов в группе
        }
    }

    Console.WriteLine();
    Console.WriteLine();

    // Вывод количества телефонов в каждой группе
    var comp_count = res_phgroup.Select(g => new { Name = g.Key, Count = g.Count() });

    foreach (var group in comp_count)
    {
        Console.WriteLine($"CompanyName: {group.Name,-30},  PhoneCount: {group.Count,-20}");
    }
}
```