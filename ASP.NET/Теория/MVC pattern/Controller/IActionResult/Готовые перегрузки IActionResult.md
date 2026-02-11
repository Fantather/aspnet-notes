В ASP.NET Core контроллеры обычно возвращают объекты, реализующие интерфейс `IActionResult`. Это позволяет гибко управлять ответом HTTP (статус-код, заголовки, тело). Базовый класс `ControllerBase` предоставляет множество готовых методов-помощников.


## Готовые перегрузки (Built-in Helpers)
### Статус-коды (Status Codes)
Методы для возврата стандартных HTTP-статусов без тела или с простым сообщением.

**Ok (200)**
  `return Ok();` — успешное выполнение без данных
  `return Ok(object);` — успешное выполнение с сериализацией объекта (JSON по умолчанию)

**BadRequest (400)**
  `return BadRequest();` — ошибка клиента
  `return BadRequest("Error message");` — ошибка с описанием
  `return BadRequest(ModelState);` — ошибка валидации модели

**NotFound (404)**
  `return NotFound();` — ресурс не найден
  `return NotFound(object);` — не найден с доп. информацией

**NoContent (204)**
  `return NoContent();` — успешное выполнение, но ответ не содержит тела (часто используется при DELETE или UPDATE)

**StatusCode**
  `return StatusCode(418);` — возврат произвольного кода статуса
  `return StatusCode(500, "Internal Error");`

### Форматирование данных
**Json**
  `return Json(data);`
  Принудительно сериализует данные в JSON, игнорируя настройки согласования содержимого (Content Negotiation). Устанавливает `Content-Type: application/json`

**Content**
  `return Content("Raw text");`
  Возвращает простую строку. Можно указать MIME-тип: `return Content("<h1>Hi</h1>", "text/html");`

### Перенаправления (Redirections)
Эти методы возвращают статусы группы 3xx (обычно 302 Found или 301 Moved Permanently).

**Redirect**
  `return Redirect("/home/index");`
  `return Redirect("https://google.com");`
  Перенаправление по указанному URL (относительному или абсолютному)

**RedirectToAction**
  `return RedirectToAction("Privacy");` — на метод в текущем контроллере
  `return RedirectToAction("Index", "Home");` — на метод `Index` контроллера `Home`
  `return RedirectToAction("Edit", new { id = 5 });` — с передачей параметров маршрута

**RedirectToRoute**
  `return RedirectToRoute("default", new { controller = "Home", action = "Index" });`
  Перенаправление, основанное на имени маршрута, заданном в конфигурации `MapRoute`

**LocalRedirect**
  `return LocalRedirect("/local/path");`
  Аналогичен `Redirect`, но выбрасывает исключение, если переданный URL является внешним (защита от Open Redirect атак)



## Best Practices
**Предпочтение специфичным типам**
Используй `ActionResult<T>` вместо простого `IActionResult` в сигнатуре метода для Swagger/OpenAPI документации. Это позволяет явно указать возвращаемый тип данных при статусе 200

```C#
public ActionResult<Product> Get() => _service.GetProduct();
```

**Использование Helper-методов**
Предпочитай методы `Ok()`, `NotFound()` ручному созданию `new OkObjectResult()`. Это делает код чище и читаемее