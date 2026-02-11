```C#
Timer timer = new(10000);  // 10 сек
timer.Elapsed += Timer_Elapsed;  // Подписка

// Обработчик
private static void Timer_Elapsed(object? sender, ElapsedEventArgs e)
{
	Console.WriteLine($"Таймер сработал в {e.SignalTime}");
}
```

- `ElapsedEventArgs`
  Стандартный тип из пространства имён `System.Timers`
  Содержит свойство `SignalTime` - хранит время, когда таймер был остановлен
  
- `Timer_Elapsed`
  - Это стандартное название для Обработчика событий таймера
  - Принято делать его `private`, потому что снаружи доступ к нему обычно не требуется, но не обязательно


**Предотвращение повторных перекрывающихся вызовов**
Мы просто останавливаем таймер в начале выполнения кода обработчика