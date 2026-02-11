Определяет, содержит ли последовательность хотя бы Один элемент
Может принимать условие, которому должен соответствовать элемент

Подробнее про [[C charp/Синтаксис/LINQ/Логические проверки (квантификация)/Any|Any]]

Проверяет, есть ли хотя бы один телефон Samsung
```C#
using (Context db = new Context())
{
    bool result1 = db.Phones.Any(p => p.Company.Name == "Sumsung");

    Console.WriteLine($"db.Phones.Any(p => p.Company.Name == \"Sumsung\") = {result1}");
}
```