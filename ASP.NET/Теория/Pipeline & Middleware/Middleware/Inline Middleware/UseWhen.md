Метод для условного выполнения middleware с возвратом в основной конвейер.

Позволяет выполнить ответвление (ветку middleware) на основе условия, а после завершения работы ветки — **продолжить** выполнение основного конвейера (если ветка сама не прервала запрос).

**Ключевые особенности**
* **Возврат в основной поток**
  Главное отличие от `MapWhen`. Если middleware внутри ветки `UseWhen` вызывают `next()`, управление возвращается в точку ветвления и переходит к следующему компоненту основного `app`.

* **Общая область памяти**
  Ветка использует тот же `HttpContext`. Изменения, внесенные в `Items` или заголовки внутри ветки, будут доступны в основном конвейере после возврата.

* **Предикат**
  Как и `MapWhen`, принимает `Func<HttpContext, bool>` для проверки условия.


### Синтаксис
```csharp
public static IApplicationBuilder UseWhen(
    this IApplicationBuilder app,
    Func<HttpContext, bool> predicate,
    Action<IApplicationBuilder> configuration)
````

**Пример использования**
Классический сценарий: логгирование или аудит только для определенных запросов (например, API), не прерывая их обработку.

``` C#
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.Use(async (context, next) =>
{
    Console.WriteLine("1. Global start");
    await next();
});

// Условие: путь начинается с /api
app.UseWhen(
    context => context.Request.Path.StartsWithSegments("/api"),
    branch =>
    {
        // Этот код выполнится только для /api
        branch.Use(async (context, next) =>
        {
            Console.WriteLine("2. API Logger: Start");
            await next(); // Важно вызвать next, чтобы вернуться в основной поток
            Console.WriteLine("4. API Logger: End");
        });
        
        // ВНИМАНИЕ: Если здесь использовать branch.Run, 
        // основной конвейер (пункт 3) НЕ выполнится.
    }
);

app.Run(async context =>
{
    Console.WriteLine("3. Main Handler");
    await context.Response.WriteAsync("Response generated");
});

app.Run();
```

**Порядок выполнения для запроса /api/users**
1. `1. Global start`
2. `2. API Logger: Start` (зашли в ветку)
3. `3. Main Handler` (вернулись в основной поток)
4. `4. API Logger: End` (выход из стека вызовов ветки)


### Сравнение с MapWhen
**UseWhen**
  _Заходит_ в гости и возвращается. 
  Используется для "сквозной" логики (логи, метрики, модификация заголовков).

**MapWhen**
  _Уходит_ навсегда. 
  Используется для альтернативной обработки (другой API, обработка ошибок, статические файлы).
