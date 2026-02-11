ASP.NET Core использует механизм *Model Binding* для сопоставления данных из HTTP-запроса с параметрами методов контроллера. При передаче данных через URL используются два основных источника: *Route Data* (данные маршрута) и *Query String* (строка запроса).

## Простые типы данных
Для примитивных типов (`int`, `string`, `Guid`, `bool`) привязка происходит автоматически по совпадению имен.

### Источники данных
**[FromRoute]**
  Извлекает значения из шаблона маршрута, например `/products/{id}`

**[FromQuery]**
  Извлекает значения из строки запроса после знака `?`, например `?category=phones`

### Пример реализации
```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
    // GET: api/products/details/42?culture=ru-RU
    [HttpGet("details/{id}")]
    public IActionResult GetProduct(
        [FromRoute] int id, 
        [FromQuery] string culture)
    {
        // id = 42
        // culture = "ru-RU"
        return Ok();
    }
}
````

## Передача сложных объектов
Если метод контроллера принимает класс, _Model Binding_ пытается сопоставить свойства этого класса с параметрами из `Query String`.

### Особенности
**Атрибут [FromQuery]**
Необходимо явно указывать этот атрибут перед параметром сложного типа, чтобы привязчик искал свойства в строке запроса

**Сопоставление имен**
Имена параметров в URL должны совпадать с именами свойств класса (регистр обычно игнорируется)

### Пример реализации
```C#
public class ProductFilter
{
    public string Category { get; set; }
    public decimal MinPrice { get; set; }
    public bool IsActive { get; set; }
}

// GET: api/products/search?Category=Electronics&MinPrice=100&IsActive=true
[HttpGet("search")]
public IActionResult SearchProducts([FromQuery] ProductFilter filter)
{
    // filter.Category = "Electronics"
    // filter.MinPrice = 100.0
    // filter.IsActive = true
    return Ok();
}
```


## Массивы и коллекции
ASP.NET Core поддерживает привязку массивов и списков из строки запроса.

### Массив примитивов
Для передачи нескольких значений одного параметра ключ в URL повторяется.

**Синтаксис URL**
`?ids=1&ids=2&ids=3`

**Реализация**
```C#
// GET: api/products/batch?ids=10&ids=20&ids=30
[HttpGet("batch")]
public IActionResult GetBatch([FromQuery] int[] ids)
{
    // ids.Length = 3
    // ids[0] = 10...
    return Ok();
}
```

### Массив объектов
Передача списка сложных объектов через URL возможна, но приводит к длинным строкам запроса. Используется индексация.

**Синтаксис URL**
`?filters[0].Field=Name&filters[0].Value=Test&filters[1].Field=Price&filters[1].Value=100`

**Реализация**
```C#
public class SearchCriterion
{
    public string Field { get; set; }
    public string Value { get; set; }
}

[HttpGet("advanced-search")]
public IActionResult AdvancedSearch([FromQuery] List<SearchCriterion> filters)
{
    // filters[0].Field = "Name"
    // filters[1].Field = "Price"
    return Ok();
}
```


## Нейминг и рекомендации
**Избыточность атрибутов**
Для простых типов (`int`, `string`) атрибуты `[FromQuery]` часто опускают, если имя параметра явно не совпадает с шаблоном маршрута, но явное указание _улучшает читаемость_

**Ограничения URL**
Передача массивов объектов через URL имеет ограничение по длине строки запроса (обычно 2048 символов). Для больших объемов данных _предпочтительнее использовать POST-запросы и тело сообщения_