Метод расширения, позволяющий разветвлять конвейер обработки запросов на основе произвольного логического условия (предиката).

В отличие от `Map`, который смотрит только на путь запроса, `MapWhen` позволяет использовать любые данные из `HttpContext` (заголовки, куки, параметры строки запроса, данные аутентификации и т.д.).

Используется для альтернативной обработки (другой API, обработка ошибок, статические файлы)


**Ключевые особенности**
* **Предикат условия**
  Принимает делегат `Func<HttpContext, bool>`. 
  Если он возвращает `true`, управление передается в новую ветку.

* **Изоляция ветки (Терминальность)**
  Если условие выполнилось, запрос уходит в ветку и **не возвращается** в основной конвейер после неё. Если middleware в ветке не сформировал ответ, клиент получит пустой ответ (или 404), но код после блока `MapWhen` выполнен не будет.

* **Отсутствие изменения PathBase**
  В отличие от `Map`, `MapWhen` *не изменяет* `Request.Path` или `Request.PathBase`, так как условие может быть не связано с путем.


**Синтаксис**
```csharp
public static IApplicationBuilder MapWhen(
    this IApplicationBuilder app,
    Func<HttpContext, bool> predicate,
    Action<IApplicationBuilder> configuration)
````

### Пример использования
Реализация специальной логики для старых браузеров или API-клиентов на основе заголовков.

``` C#
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.Use(async (context, next) =>
{
    Console.WriteLine("Global Log: Request received");
    await next();
});

// Условие: наличие параметра запроса ?export=true
app.MapWhen(
    context => context.Request.Query.ContainsKey("export"),
    branch =>
    {
        // Эта ветка выполнится только если условие true
        branch.Run(async context =>
        {
            await context.Response.WriteAsync("Exporting data...");
        });
    }
);

// Условие: заголовок User-Agent содержит "Googlebot"
app.MapWhen(
    context => context.Request.Headers["User-Agent"].ToString().Contains("Googlebot"),
    botBranch =>
    {
        botBranch.Run(async context =>
        {
            await context.Response.WriteAsync("Hello, Googlebot!");
        });
    }
);

// Основной конвейер (выполняется, если ни одно условие MapWhen не сработало)
app.Run(async context =>
{
    await context.Response.WriteAsync("Standard Page");
});

app.Run();
```


### Отличие от UseWhen

**MapWhen**
  _Полностью уводит_ поток выполнения в ветку. Используется, когда нужно _заменить_ стандартную обработку на другую.

**UseWhen**
  Выполняет ветку, а затем _возвращается_ в основной конвейер. Используется, когда нужно _добавить_ действие (например, логирование) при определенном условии, но продолжить выполнение основного кода.