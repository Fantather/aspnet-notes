**ViewModel** — это паттерн проектирования, при котором создается специальный класс (POCO), содержащий только те данные, которые необходимы для отображения конкретного Представления (View).

Это **основной и рекомендуемый способ** передачи данных от Контроллера к View, в отличие от `ViewData` и `ViewBag`.

## Зачем нужен ViewModel?
**1. Строгая типизация**
  В отличие от динамических `ViewBag`, здесь ты работаешь с конкретным типом. Компилятор проверит имена свойств, а Visual Studio даст полный IntelliSense (автодополнение). Если ты переименуешь свойство в классе, IDE предложит переименовать его и во View.

**2. Объединение нескольких сущностей**
  *View* может принимать только **одну** модель через директиву `@model`.
  *Вопрос:* Как передать на страницу данные о Товаре, Покупателе и список похожих товаров одновременно?
  *Ответ:* Создать класс `ProductPageViewModel`, который будет содержать в себе все эти объекты как свойства.

**3. Разделение ответственности**
  Модели базы данных (Entity) часто содержат данные, которые не нужно показывать пользователю (пароли, служебные ID, даты изменений). ViewModel позволяет отфильтровать лишнее и передать только нужное (например, отформатированную дату или склеенное имя).


## Реализация: Передача нескольких объектов
Допустим, нам нужно вывести страницу товара, где есть сам **Товар** (Product), данные **Пользователя** (User) и **Заголовок** страницы.

### Шаг 1. Создаем класс ViewModel
Обычно их хранят в папке `Models/ViewModels`.
```csharp
using MyProject.Domain; // Пространство имен с сущностями БД

public class ProductDetailsViewModel
{
    // Свойство для заголовка
    public string PageTitle { get; set; }

    // Данные о товаре (Сущность из БД)
    public Product Product { get; set; }

    // Данные о пользователе (Сущность из БД)
    public User CurrentUser { get; set; }
    
    // Можно добавить вычисляемые свойства специально для View
    public bool IsOnSale => Product.Price < Product.OldPrice;
}
````

### Шаг 2. Заполняем ViewModel в Контроллере
```C#
public IActionResult Details(int id)
{
    // 1. Получаем данные из источников (БД, сервисы)
    var product = _productService.GetById(id);
    var user = _userService.GetCurrentUser();

    // 2. Создаем и заполняем ViewModel
    var viewModel = new ProductDetailsViewModel
    {
        PageTitle = $"Купить {product.Name}",
        Product = product,
        CurrentUser = user
    };

    // 3. Передаем ЕДИНСТВЕННЫЙ объект во View
    return View(viewModel);
}
```

### Шаг 3. Используем во View
```CS
@model MyProject.Models.ViewModels.ProductDetailsViewModel

<h1>@Model.PageTitle</h1>

<div class="product-card">
    <h2>@Model.Product.Name</h2>
    <p>Цена: @Model.Product.Price</p>

    @if (Model.IsOnSale)
    {
        <span class="badge">Скидка!</span>
    }
</div>

<div class="user-panel">
    <p>Привет, @Model.CurrentUser.UserName!</p>
</div>
```