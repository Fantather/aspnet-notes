Если стандартных методов недостаточно, можно создать свой класс, реализующий `IActionResult`. Для этого нужно реализовать единственный асинхронный метод `ExecuteResultAsync`.

### Пример: HtmlResult
Простая реализация, возвращающая HTML-разметку.
```csharp
public class HtmlResult : IActionResult
{
    private readonly string _html;

    public HtmlResult(string html)
    {
        _html = html;
    }

    public async Task ExecuteResultAsync(ActionContext context)
    {
        // 1. Настройка ответа
        var response = context.HttpContext.Response;
        response.ContentType = "text/html";
        
        // 2. Запись в поток ответа
        var bytes = System.Text.Encoding.UTF8.GetBytes(_html);
        await response.Body.WriteAsync(bytes, 0, bytes.Length);
    }
}
````

### Использование в контроллере
``` C#
[HttpGet("custom-html")]
public IActionResult GetHtml()
{
    return new HtmlResult("<h1>Custom Result</h1><p>Works!</p>");
}
```