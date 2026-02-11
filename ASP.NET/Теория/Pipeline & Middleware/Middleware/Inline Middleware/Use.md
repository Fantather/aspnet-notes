Метод для регистрации компонента Middleware, который может выполнять действия как до, так и после передачи управления следующему компоненту в конвейере.

В отличие от `Run`, метод `Use` принимает делегат `next`, позволяя продолжить цепочку обработки.

### Ключевые особенности
* **Параметр next**
  Представляет собой делегат `RequestDelegate`. Вызов `await next()` передает управление следующему Middleware в конвейере.

* **Двунаправленная обработка**
  Код, расположенный *до* вызова `next()`, выполняется при получении запроса. Код, расположенный *после* `await next()`, выполняется при возврате ответа (когда следующие компоненты завершили работу).

* **Прерывание конвейера (Short-circuiting)**
  Если внутри `Use` не вызвать `next()`, выполнение следующих Middleware не начнется. Это аналогично поведению метода `Run`, но применяется условно (например, вернуть кэшированный ответ и не идти в базу данных).


### Синтаксис
```csharp
app.Use(async (context, next) =>
{
    // Действия с запросом (Request)
    await next(); 
    // Действия с ответом (Response)
});
````


**Пример использования**
Логирование времени выполнения запроса — классический пример, требующий кода "до" и "после".

``` C#
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.Use(async (context, next) =>
{
    Console.WriteLine($"[1] Before next: {context.Request.Path}");
    
    // Засекаем время
    var stopwatch = System.Diagnostics.Stopwatch.StartNew();
    
    // Передаем управление следующему компоненту
    await next();
    
    // Этот код выполнится уже после того, как отработает app.Run
    stopwatch.Stop();
    Console.WriteLine($"[1] After next. Elapsed: {stopwatch.ElapsedMilliseconds} ms");
});

app.Use(async (context, next) =>
{
    Console.WriteLine("[2] Before next");
    await next();
    Console.WriteLine("[2] After next");
});

app.Run(async context =>
{
    Console.WriteLine("[3] Terminal Endpoint");
    await context.Response.WriteAsync("Hello World");
});

app.Run();
```


**Порядок выполнения в консоли**
1. `[1] Before next: /`
2. `[2] Before next`
3. `[3] Terminal Endpoint`
4. `[2] After next`
5. `[1] After next. Elapsed: ...`

Это демонстрирует структуру вложенности, часто называемую _The Onion Architecture_ (Луковая архитектура) в контексте Middleware, где первый добавленный компонент является самой внешней оболочкой.