Модель (Model) — это компонент, который представляет данные приложения и бизнес-логику
Основная задача модели это описывать логику и структуру используемых сущностей

В отличие от View и Controller, Модель не знает о существовании пользовательского интерфейса или HTTP-запросов.

## Виды Моделей
В реальном приложении термин "Модель" часто разделяют на несколько категорий, чтобы не смешивать ответственность.

**1. Domain Model (Доменная модель / Entity)**
  Классы, которые напрямую отражают структуру базы данных (если используется Entity Framework).
  Содержат только данные и, возможно, базовую логику валидации.
  *Пример:* Класс `User` с полями `Id`, `Email`, `PasswordHash`.

**2. View Model (Модель представления)**
  Классы, специально созданные для передачи данных в *View*.
  Могут содержать данные из разных сущностей (например, Пользователь + Его последние заказы) или отформатированные данные (дата в виде строки).
  Позволяют скрыть лишние поля (например, не передавать пароль во View)
  Хранятся в отдельной папке ViewModels и в названии имеют приставку ViewModel

**3. Input Model / DTO (Data Transfer Object)**
  Классы, предназначенные для получения данных *от* пользователя (из форм или API-запросов).
  Часто содержат атрибуты валидации.

## Валидация данных (Data Annotations)
ASP.NET Core использует атрибуты из пространства имен `System.ComponentModel.DataAnnotations` для проверки данных *до* того, как они попадут в бизнес-логику.

**Основные атрибуты**
  `[Required]` — поле обязательно для заполнения
  `[StringLength(100)]` — ограничение длины строки
  `[Range(1, 100)]` — число должно быть в диапазоне
  `[EmailAddress]` — проверка формата email
  `[Compare("Password")]` — для подтверждения пароля
  `[Phone]` — проверка формата телефона

**Как это работает**
  1. Клиент отправляет форму.
  2. ModelBinder (связыватель модели) пытается заполнить объект данными.
  3. Проверяются атрибуты валидации.
  4. Результат проверки записывается в `ModelState.IsValid` внутри контроллера.

## Пример реализации
### Класс Модели (Input Model)
```csharp
using System.ComponentModel.DataAnnotations;

public class RegisterUserViewModel
{
    [Required(ErrorMessage = "Имя обязательно")]
    [StringLength(50, MinimumLength = 3)]
    public string UserName { get; set; }

    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Compare("Password", ErrorMessage = "Пароли не совпадают")]
    public string ConfirmPassword { get; set; }
}
````

### Использование в Контроллере
```C#
[HttpPost]
public IActionResult Register(RegisterUserViewModel model)
{
    // 1. Проверяем, прошли ли данные валидацию
    if (!ModelState.IsValid)
    {
        // Если есть ошибки, возвращаем ту же форму (View отобразит ошибки)
        return View(model);
    }

    // 2. Если всё ок — сохраняем в БД
    _userService.CreateUser(model);

    return RedirectToAction("Index", "Home");
}
```

### Отображение ошибок во View
```CS
@model RegisterUserViewModel

<form asp-action="Register" method="post">
    <input asp-for="UserName" />
    <span asp-validation-for="UserName" class="text-danger"></span>
    
    <button type="submit">Регистрация</button>
    
    <div asp-validation-summary="All"></div>
</form>
```