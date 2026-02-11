*Route Parameters* - Механизм захвата динамических сегментов URL и передачи их значений в обработчики (Handlers) или контроллеры.

Параметры определяются в шаблоне маршрута с помощью фигурных скобок `{}`. Имена параметров в шаблоне должны быть уникальными в пределах одного маршрута.


**Типы параметров**
* **Обязательные параметры**
  Стандартное объявление `{name}`.
  Если сегмент отсутствует в URL, маршрут не совпадет (вернется 404), и обработчик не будет вызван.

* **Необязательные параметры (Optional)**
  Объявляются знаком вопроса в конце имени: `{name?}`.
  Если сегмент отсутствует, маршрут все равно считается совпавшим. В коде аргумент должен быть Nullable или иметь значение по умолчанию.

* **Параметры со значением по умолчанию**
  Синтаксис `{name=value}`.
  Если сегмент отсутствует в URL, система маршрутизации автоматически подставит указанное значение.

* **Catch-all (Параметры "жадного" захвата)**
  Синтаксис `{**name}` (Можно `{*name}`, но эта запись устарела)
  Захватывает остаток пути URL, включая слэши `/`. Всегда должен быть последним сегментом в шаблоне.

### Способы получения значений (Binding)
**1. Minimal API (Прямая привязка)**
В Minimal API имена аргументов делегата автоматически сопоставляются с именами параметров маршрута.

```csharp
var app = WebApplication.Create(args);

// 1. Обязательный параметр
// Запрос: /users/42 -> id = 42
app.MapGet("/users/{id}", (int id) => $"User ID: {id}");

// 2. Необязательный параметр
// Аргумент должен допускать null (int?)
// Запрос: /search -> query = null
// Запрос: /search/books -> query = "books"
app.MapGet("/search/{query?}", (string? query) => 
    query is null ? "Search Page" : $"Searching for: {query}");

// 3. Catch-all
// Запрос: /files/images/logo.png -> path = "images/logo.png"
app.MapGet("/files/{*path}", (string path) => $"Serving file: {path}");
````


**2. Controllers (Model Binding)**
В контроллерах механизм Model Binding ищет значения параметров по имени аргумента метода действия.

``` C#
[Route("api/[controller]")]
[ApiController]
public class ProductsController : ControllerBase
{
    // Атрибут [FromRoute] указывает источник данных явно, 
    // но обычно это происходит автоматически.
    
    [HttpGet("{id}")]
    public IActionResult GetById([FromRoute] int id)
    {
        return Ok($"Product {id}");
    }
    
    // Пример со значением по умолчанию в шаблоне
    [HttpGet("category/{name=all}")]
    public IActionResult GetByCategory(string name)
    {
        // Если запрос /api/products/category -> name будет "all"
        return Ok(name);
    }
}
```


**3. Низкоуровневый доступ (RouteValues)**
Если вы находитесь в Middleware или вам нужен доступ к "сырым" данным маршрутизации без привязки к методу.

```C#
app.MapGet("/debug/{id}", (HttpContext context) =>
{
    // Коллекция RouteValues хранит все захваченные параметры
    var idValue = context.Request.RouteValues["id"];
    
    return $"Raw value from dictionary: {idValue}";
});
```


### Важные нюансы
**Чувствительность к регистру**
  Сам захват параметров нечувствителен к регистру (Case-insensitive). Запросы `/user/admin` и `/USER/ADMIN` попадут в один маршрут, и параметр примет значение `ADMIN`.

**Очередность**
  Параметры URL имеют высокий приоритет при привязке моделей (выше, чем Query String или Body), но точный порядок зависит от конфигурации источника данных (Binding Source).

**Декодирование**
  Значения параметров автоматически декодируются из URL-кодировки (URL decoded). Если в URL передано `C%23` (C#), в ```