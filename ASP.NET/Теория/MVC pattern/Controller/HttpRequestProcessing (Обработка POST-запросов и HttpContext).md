В ASP.NET Core данные POST-запроса обычно передаются в теле (Body), однако параметры могут присутствовать и в строке запроса (URL). Для корректного извлечения данных необходимо явно указывать атрибуты привязки.

## Источники данных в POST-запросах
Контроллер различает данные, пришедшие из URL, и данные из тела запроса, используя атрибуты *Binding Attributes*.

### Основные атрибуты
**[FromBody]**
  Указывает, что данные необходимо считать из тела запроса и десериализовать (обычно из JSON или XML). Используется для REST API

**[FromForm]**
  Указывает, что данные приходят в формате форм (`x-www-form-urlencoded` или `multipart/form-data`). Это стандартный способ отправки HTML-форм

**[FromQuery]**
  Принудительно заставляет искать параметр в строке URL, даже если метод является POST

### Пример комбинированной передачи
Можно одновременно принимать данные из URL и из тела запроса.
```csharp
[HttpPost("update/{id}")]
public IActionResult UpdateProduct(
    [FromRoute] int id,               // Из URL пути
    [FromQuery] string reason,        // Из URL Query String (?reason=fix)
    [FromBody] ProductModel model)    // Из тела запроса (JSON)
{
    // Логика обновления
    return Ok();
}
````


## Передача форм (Form Data)
Когда клиент отправляет HTML-форму (без AJAX/JSON), данные кодируются как пары ключ-значение в теле запроса.

### Типы контента
**application/x-www-form-urlencoded**

Стандартная кодировка, аналогична строке запроса, но находится в теле

**multipart/form-data**
Используется для загрузки файлов вместе с данными формы

### Реализация приема формы
```C#
public class UserFormData
{
    public string Username { get; set; }
    public string Email { get; set; }
}

[HttpPost("register")]
// Принимает данные формы, игнорируя JSON
public IActionResult Register([FromForm] UserFormData data)
{
    // data.Username
    // data.Email
    return Ok();
}
```


## Работа с HttpContext
`HttpContext` предоставляет прямой доступ к объектам `Request` и `Response`, минуя механизм Model Binding. Это полезно, когда структура данных неизвестна заранее или требуется доступ к заголовкам и кукам.

### Доступ к данным формы
Свойство `Request.Form` содержит коллекцию значений, переданных через POST-форму.

**Особенности**
Доступно только если `Content-Type` запроса является `application/x-www-form-urlencoded` или `multipart/form-data`. Если тело содержит JSON, `Request.Form` выбросит исключение или будет пустым (в зависимости от версии)

**Пример реализации**
```C#
[HttpPost("manual-form")]
public IActionResult ManualFormProcessing()
{
    // Чтение значения по ключу "username"
    if (Request.Form.ContainsKey("username"))
    {
        var username = Request.Form["username"];
        // Работа со значением
    }

    // Чтение коллекции файлов
    var files = Request.Form.Files;
    
    return Ok();
}
```

### Доступ к строке запроса (URL)
Свойство `Request.Query` позволяет читать параметры из URL вручную.

**Пример реализации**
```C#
[HttpPost("manual-query")]
public IActionResult ManualQueryProcessing()
{
    // Получение значения ?id=123
    if (Request.Query.ContainsKey("id"))
    {
        var idStr = Request.Query["id"];
        // Преобразование вручную
        if (int.TryParse(idStr, out int id))
        {
            // Логика
        }
    }
    return Ok();
}
```


## Резюме
1. Для JSON данных используй `[FromBody]`
2. Для стандартных HTML форм используй `[FromForm]`
3. Для параметров URL в POST-запросе используй `[FromQuery]` или `[FromRoute]`
4. Используй `HttpContext.Request` только в случае, когда стандартный Model Binding не справляется или требуется динамическая обработка