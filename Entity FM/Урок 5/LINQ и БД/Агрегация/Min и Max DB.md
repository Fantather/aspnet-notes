Находит Минимальный/Максимальный элемент в последовательности

Подробнее про [[Min и Max]]

```C#
using (Context db = new Context())
{
    int minPrice = db.Phones.Min(p => p.Price); // минимальная цена
    int maxPrice = db.Phones.Max(p => p.Price); // максимальная цена

    Console.WriteLine($"MinPrice {minPrice}");
    Console.WriteLine($"MaxPrice {maxPrice}");
}
```