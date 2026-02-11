Выделение логики промежуточного ПО в отдельный класс для обеспечения инкапсуляции, тестируемости и соблюдения принципа единственной ответственности.

В ASP.NET Core такие классы чаще всего создаются **по соглашению** (Convention-based), не реализуя жестких интерфейсов, но следуя строгим правилам сигнатуры.


### Правила создания (Convention-based)
* **Публичный конструктор**
  Должен принимать параметр типа `RequestDelegate`. Через этот делегат будет вызываться следующий компонент в конвейере.

* **Метод Invoke или InvokeAsync**
  Класс обязан содержать публичный метод с именем `Invoke` или `InvokeAsync`.

* **Сигнатура метода**
  Метод должен принимать первым параметром `HttpContext` и возвращать `Task`.


**Внедрение зависимостей (DI)**
* **Через конструктор**
  Используется для сервисов с жизненным циклом *Singleton*. Так как сам экземпляр Middleware создается один раз за время жизни приложения, внедрение Scoped-сервисов (например, EF Core DbContext) в конструктор приведет к ошибке или утечке памяти (Captive Dependency).

* **Через метод InvokeAsync**
  Используется для сервисов с жизненным циклом *Scoped* и *Transient*. Сервисы передаются как аргументы метода после `HttpContext`. ASP.NET Core резолвит их автоматически для каждого запроса.


**Пример реализации**
```csharp
using System.Globalization;

public class RequestCultureMiddleware
{
    private readonly RequestDelegate _next;

    // Конструктор: получаем делегат следующего компонента
    public RequestCultureMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // Метод обработки: вызывается для каждого запроса
    // ILogger передается через DI (Method Injection)
    public async Task InvokeAsync(HttpContext context, ILogger<RequestCultureMiddleware> logger)
    {
        var cultureQuery = context.Request.Query["culture"];
        
        if (!string.IsNullOrWhiteSpace(cultureQuery))
        {
            var culture = new CultureInfo(cultureQuery);
            CultureInfo.CurrentCulture = culture;
            CultureInfo.CurrentUICulture = culture;
            
            logger.LogInformation($"Culture changed to {culture}");
        }

        // Передача управления дальше по конвейеру
        await _next(context);
    }
}
````

**Регистрация (Extension Method)**
Для соблюдения стиля кода и удобства регистрации принято создавать методы расширения (Extension Methods) для `IApplicationBuilder`.

``` C#
public static class RequestCultureMiddlewareExtensions
{
    public static IApplicationBuilder UseRequestCulture(
        this IApplicationBuilder builder)
    {
        // Аналог app.UseMiddleware<RequestCultureMiddleware>()
        return builder.UseMiddleware<RequestCultureMiddleware>();
    }
}
```

**Применение в Program.cs**
```C#
var app = builder.Build();

// Использование через метод расширения
app.UseRequestCulture();

app.Run();
```


**Factory-based Middleware (IMiddleware)**
Альтернативный подход, требующий реализации интерфейса `IMiddleware`.

- Middleware регистрируется в DI-контейнере как сервис.
- Позволяет внедрять _Scoped_ зависимости прямо в конструктор.
- Требует явной регистрации класса в `builder.Services`.
- 