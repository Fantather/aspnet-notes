Метод расширения для `IApplicationBuilder`, который позволяет создавать ветвление конвейера обработки запросов (Pipeline) на основе совпадения пути запроса.

Используется, когда необходимо выделить отдельную логику обработки для конкретного префикса URL.

**Ключевые особенности**
* **Ветвление**
  При совпадении пути управление передается в новую ветку конвейера. Если ветка обрабатывает запрос полностью, основной конвейер (после `Map`) не выполняется.

* **Сопоставление пути**
  Проверяет начало пути запроса (`Request.Path`). Если путь начинается с указанного сегмента, условие считается истинным.

* **Изменение PathBase**
  Внутри ветки `Map` matched-сегмент пути переносится из `Request.Path` в `Request.PathBase`. Это позволяет вложенному middleware работать с путем так, будто он является корнем.

**Синтаксис**
Принимает строку (путь) и делегат конфигурации для новой ветки `IApplicationBuilder`

```csharp
public static IApplicationBuilder Map(
    this IApplicationBuilder app,
    PathString pathMatch,
    Action<IApplicationBuilder> configuration)    
```


### Пример использования
``` C#
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

// Стандартный middleware для всех запросов
app.Use(async (context, next) =>
{
    Console.WriteLine("Global Middleware: Start");
    await next();
});

// Ветвление для пути "/admin"
app.Map("/admin", adminApp =>
{
    // Этот middleware сработает только если URL начинается с /admin
    adminApp.Run(async context =>
    {
        await context.Response.WriteAsync("Admin Area");
    });
});

// Ветвление для пути "/api"
app.Map("/api", apiApp =>
{
    // Вложенное ветвление: /api/status
    apiApp.Map("/status", statusApp =>
    {
        statusApp.Run(async context =>
        {
            await context.Response.WriteAsync("API Status: OK");
        });
    });
    
    // Если запрос пришел на /api, но не на /api/status
    apiApp.Run(async context =>
    {
        await context.Response.WriteAsync("General API Endpoint");
    });
});

app.Run(async context =>
{
    await context.Response.WriteAsync("Home Page");
});

app.Run();
```

**Поведение PathBase при использовании Map**
Если запрос приходит на `/admin/users`:
1. До входа в `Map("/admin")`:
    - `Path`: `/admin/users`
    - `PathBase`: `` (пусто)

2. Внутри ветки `Map`:
    - `Path`: `/users`
    - `PathBase`: `/admin`


Это позволяет писать переиспользуемые компоненты, которые не зависят от того, по какому префиксу они смонтированы