Вычисляет сумму всех числовых значений в последовательности
Может работать с любыми числовыми типами, включая `nullable` версии

Подробнее о [[Sum]]


```C#
using (Context db = new Context())
{
    int sum1 = db.Phones.Sum(p => p.Price); // суммарная цена всех телефонов
    int sum2 = db.Phones
        .Where(p => p.Company.Name == "Apple")
        .Sum(p => p.Price); // суммарная цена телефонов Apple

    Console.WriteLine(sum1);
    Console.WriteLine(sum2);
}
```