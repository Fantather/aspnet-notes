Подсчитывает количество элементов  в последовательности
Может использовать предикат - что бы подсчитать количество элементов, удовлетворяющих условию

Подробнее в [[Count]]


Общее количество телефонов
```C#
using (Context db = new Context())
{
    int number1 = db.Phones.Count();
	
    Console.WriteLine("Count");
    Console.WriteLine(number1);
}
```


количество моделей, содержащих "Galaxy"
```C#
using (Context db = new Context())
{
    int number2 = db.Phones.Count(p => p.Name.Contains("Galaxy"));
	
    Console.WriteLine("Count");
    Console.WriteLine(number2);
}
```