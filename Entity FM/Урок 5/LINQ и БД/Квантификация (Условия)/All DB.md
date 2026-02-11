Определяет, все ли элементы последовательности удовлетворяют условию
Для пустой последовательности возвращает `True`

Подробнее про [[All]]

Проверяет, все ли телефоны Samsung
```C#
using (Context db = new Context())
{
    bool result2 = db.Phones.All(p => p.Company.Name == "Sumsung");
    Console.WriteLine($"db.Phones.All(p => p.Company.Name == \"Sumsung\") = {result2}");
}
```