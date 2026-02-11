Стандартный тип делегата для событий
Он нужен что бы можно было объявлять события и их обработчики без ручного объявления типа

Сигнатура:
```C#
public delegate void EventHandler(object? sender, EventArgs e);
```
- `object? sender` - ссылка на объект, который вызвал событие
- `EventArgs e` - объект который содержит дополнительную информацию о событии, если её нет то пишем `EventArgs.Empty`

По стандарту `.NET` методы обработчики начинаются с `On`
```C#
// Класс с дополнительной информацией о событии, не обязателен
public class InformationEventArgs(int value) : EventArgs
{
	public int Value { get; set; } = value;
}

public class Subsriber()
{
	public void OnSendInformation(object? sender, InformationEventArgs e)
	{
		Console.Writeline($"Value: {e.Value}");
	}
}
```