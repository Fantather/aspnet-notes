Метод расширения для добавления в конвейер терминального (конечного) промежуточного ПО.

Используется для завершения обработки запроса. В отличие от `Use`, делегат, переданный в `Run`, не получает параметр `next`, что делает невозможным передачу управления следующему компоненту.


### Ключевые особенности
* **Терминальность**
  Компонент, добавленный через `Run`, считается последним в цепочке вызовов. Любые Middleware, зарегистрированные после `Run`, никогда не будут выполнены (если `Run` сработал).

* **Отсутствие параметра next**
  Делегат принимает только `HttpContext`. Сигнатура метода не предусматривает механизма продолжения конвейера.


### Синтаксис
```csharp
public static void Run(
    this IApplicationBuilder app,
    RequestDelegate handler)
````

**Пример использования**
Обычно `Run` используется в конце метода `Configure` (как заглушка для неизвестных маршрутов) или внутри веток `Map`/`MapWhen` для формирования окончательного ответа.

``` C#
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

// Middleware 1: Сработает
app.Use(async (context, next) =>
{
    Console.WriteLine("Before Run");
    await next();
    Console.WriteLine("After Run");
});

// Middleware 2: Терминальный компонент
app.Run(async context =>
{
    Console.WriteLine("Processing Request in Run");
    await context.Response.WriteAsync("Hello from Run!");
});

// Middleware 3: Никогда не выполнится (Unreachable code)
app.Use(async (context, next) =>
{
    Console.WriteLine("This will never be reached");
    await next();
});

app.Run();
```


**Особенности работы с ветвлением**
Если `Run` используется внутри `Map` или `MapWhen`, он завершает только _эту конкретную ветку_. Основной конвейер после выхода из ветки (в случае `MapWhen` или `UseWhen`) может продолжить работу в зависимости от логики ветвления, но сам `Run` не возвращает управление.

_Важно:_ Если конвейер завершится без вызова какого-либо терминального middleware (например, запрос прошел через все `Use`, но никто не записал ответ), ASP.NET Core вернет статус 404 Not Found.