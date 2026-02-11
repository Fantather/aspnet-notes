*Route Constraints* - Механизм ограничения сопоставления маршрутов, позволяющий уточнить допустимые значения для параметров URL.

Если значение параметра не удовлетворяет ограничению, маршрут не будет выбран, и система продолжит поиск другого подходящего маршрута (или вернет 404).


## Синтаксис
Ограничения указываются внутри фигурных скобок параметра после двоеточия. Можно комбинировать несколько ограничений, разделяя их двоеточием.

* **Базовый формат**
  `{parameter:constraint}`

* **С аргументами**
  `{parameter:constraint(value)}`

* **Цепочка ограничений**
  `{parameter:int:min(1)}` — параметр должен быть целым числом И больше либо равен 1.


### Встроенные ограничения (Основные)
* **Типы данных**
  * **int**, **long**, **float**, **double**, **decimal**
    Проверяет, что значение приводимо к числовому типу.
  * **bool**
    Значения `true` или `false` (регистронезависимо).
  * **guid**
    Значение должно быть валидным GUID (например, `b0c7...`).
  * **datetime**
    Значение должно быть валидной датой (парсится `DateTime.Parse`).


* **Строковые проверки**
  * **alpha**
    Только буквы английского алфавита (a-z, A-Z).
  * **length(len)**
    Строка точной длины.
  * **length(min, max)**
    Длина строки в диапазоне.
  * **minlength(len)**, **maxlength(len)**
    Минимальная и максимальная длина.
  * **regex(expression)**
    Соответствие регулярному выражению. Важно: экранируйте спецсимволы, если используете атрибуты C#.


* **Числовые диапазоны**
  * **range(min, max)**
    Число в диапазоне (включительно).
  * **min(value)**, **max(value)**
    Минимальное или максимальное значение числа.

### Примеры использования
```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

// 1. Id должен быть целым числом
app.MapGet("/users/{id:int}", (int id) => $"User {id}");

// 2. Имя файла (только буквы), расширение (ровно 3 буквы)
app.MapGet("/files/{name:alpha}.{ext:length(3)}", (string name, string ext) => 
    $"File: {name}.{ext}");

// 3. Телефон по регулярному выражению (например, 123-456-7890)
app.MapGet("/phone/{number:regex(^\\d{{3}}-\\d{{3}}-\\d{{4}}$)}", (string number) => 
    $"Calling {number}...");

// 4. Ограничение диапазона (возраст от 18 до 99)
app.MapGet("/verify/{age:range(18, 99)}", (int age) => "Access Granted");

app.Run();
````


### Кастомные ограничения (Custom Constraints)
Если встроенных проверок недостаточно, можно создать свое ограничение, реализовав интерфейс `IRouteConstraint`.

**Шаг 1: Создание класса**
``` C#
public class BlockedIpConstraint : IRouteConstraint
{
    // Метод Match возвращает true, если маршрут подходит
    public bool Match(
        HttpContext? httpContext, 
        IRouter? route, 
        string routeKey, 
        RouteValueDictionary values, 
        RouteDirection routeDirection)
    {
        // Получаем значение параметра
        if (values.TryGetValue(routeKey, out var value) && value is string ip)
        {
            // Пример логики: блокируем конкретный IP
            return ip != "192.168.0.1"; 
        }
        return false;
    }
}
```

**Шаг 2: Регистрация**
Необходимо добавить ограничение в `RouteOptions` внутри `Program.cs`.

```C#
builder.Services.AddRouting(options =>
{
    options.ConstraintMap.Add("notBlocked", typeof(BlockedIpConstraint));
});
```

**Шаг 3: Применение**
``` C#
app.MapGet("/server/{ip:notBlocked}", (string ip) => $"Connecting to {ip}...");
```
