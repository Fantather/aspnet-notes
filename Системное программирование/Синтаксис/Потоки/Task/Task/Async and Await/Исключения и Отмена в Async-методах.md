Для отлова исключений из async-методов используется стандартный try catch

`await` проверяет, имеет ли задача статус *Faulted*, если да, то он заглядывает внутрь *AggregateException* и бросает первое исключение наружу
То есть, асинхронная обработка ошибок будет так же проста, как и синхронная

При отмене, `await` видит у Task статус *Canceled* и бросает наружу *OperationCanceledException*

```C#
try
{
    string result = await canceledTask; 
}
catch (OperationCanceledException) 
{
    Console.WriteLine("Задача была отменена.");
}
catch (InvalidOperationException ex)
{
    // ...а здесь бы мы отловили исключение
    Console.WriteLine($"Задача бросила исключение: {ex.Message}");
}
```