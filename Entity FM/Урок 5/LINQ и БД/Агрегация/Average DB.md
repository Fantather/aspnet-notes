Вычисляет среднее арифметическое числовых значений в последовательности
Может работать с любыми числовыми типами

Подробнее о [[Average]]

```C#
using (Context db = new Context())
{
    double avgPrice = db.Phones
        .Where(p => p.Company.Name == "Sumsung")
        .Average(p => p.Price); // средняя цена телефонов Samsung

    Console.WriteLine($"AvgPrice {avgPrice}");
}
```