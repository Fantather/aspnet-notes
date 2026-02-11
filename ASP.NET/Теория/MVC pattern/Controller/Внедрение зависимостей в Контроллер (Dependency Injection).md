ASP.NET Core имеет встроенный DI-контейнер, который управляет жизненным циклом сервисов. Контроллеры запрашивают необходимые зависимости явно, а контейнер предоставляет их экземпляры.

## 1. Внедрение через конструктор (Constructor Injection)
Это стандартный и наиболее рекомендуемый способ получения зависимостей. Зависимости объявляются как параметры конструктора контроллера и сохраняются в `private readonly` полях.

### Особенности
**Явность**
  Сразу видно, какие сервисы необходимы контроллеру для работы.

**Тестируемость**
  Легко подменять реализации (моки) при Unit-тестировании.

**Глобальная доступность**
  Внедренный сервис доступен во всех методах (Action) контроллера.

### Реализация
```csharp
public class HomeController : Controller
{
    private readonly ILogger<HomeController> _logger;
    private readonly IEmailService _emailService;

    // Контейнер автоматически найдет и передаст эти сервисы
    public HomeController(ILogger<HomeController> logger, IEmailService emailService)
    {
        _logger = logger;
        _emailService = emailService;
    }

    public IActionResult Index()
    {
        _logger.LogInformation("Index page visited");
        return View();
    }
}
````

## 2. Внедрение в метод действия (Action Injection)
Используется, когда сервис нужен только в _одном_ конкретном методе контроллера, и нет смысла внедрять его через конструктор (чтобы не перегружать его или не инстанцировать "тяжелый" сервис для каждого запроса).

### Особенности
**Атрибут [FromServices]**
Указывает DI-контейнеру, что параметр метода нужно взять из сервисов, а не из Request Body или URL.

**Оптимизация**
Позволяет избежать создания экземпляра сервиса, если вызываются другие методы контроллера, где этот сервис не нужен (актуально для _Transient_ сервисов).

### Реализация
```C#
public class ReportsController : Controller
{
    // Сервис IReportGenerator создается только при вызове этого метода
    [HttpPost("generate")]
    public IActionResult GenerateReport(
        [FromServices] IReportGenerator generator, 
        [FromBody] ReportRequest request)
    {
        var report = generator.Create(request);
        return Ok(report);
    }
}
```

## 3. Service Locator (через HttpContext)
Получение сервисов вручную через свойство `HttpContext.RequestServices`

### Особенности
**Анти-паттерн**
Этот подход считается _Service Locator pattern_ и часто рассматривается как анти-паттерн, так как скрывает зависимости класса. Взглянув на конструктор, нельзя понять, что контроллеру нужен этот сервис.

**Сценарии использования**
Используется крайне редко, например, в старых версиях ASP.NET, при миграции кода, или в специфичных инфраструктурных методах (например, внутри кастомных атрибутов или Middleware, где нет прямого DI), но внутри контроллера этого стоит избегать.

### Реализация
```C#
public class LegacyController : Controller
{
    [HttpGet("manual")]
    public IActionResult ManualRetrieval()
    {
        // Ручное извлечение сервиса из контейнера
        var service = HttpContext.RequestServices.GetService<IDateTimeService>();
        
        return Ok(service.Now);
    }
}
```

## Рекомендации по выбору
**Constructor Injection**
Используй по умолчанию для 99% случаев. Это чистая архитектура.

**[FromServices]**
Используй, если сервис "тяжелый" или редко используемый, и нужен только в одном методе из множества в этом контроллере.

**RequestServices**
Используй только если другие способы невозможны (крайний случай).