# Секции (Sections) в Razor
Секции — это механизм в движке Razor, который позволяет из конкретного Представления (View) передавать куски разметки или кода в строго определенные места мастер-страницы (Layout)

## Зачем нужны секции
Обычно `_Layout.cshtml` содержит общую структуру, а `@RenderBody()` выводит основное содержимое страницы. Но иногда нужно изменить что-то *вне* этого основного блока

**Подключение специфичных скриптов и стилей**
  Если на одной конкретной странице нужен тяжелый JS-график или CSS-файл, нет смысла грузить его глобально для всего сайта в `_Layout`. Секция позволяет внедрить этот `<script>` прямо перед закрывающим тегом `</body>` только для этой страницы

**Динамические элементы интерфейса**
  Секции можно использовать для вывода любых частей HTML. Например, для боковой панели (Sidebar), которая меняется в зависимости от раздела сайта, для кастомных хлебных крошек (Breadcrumbs) или виджетов в шапке (Header)


## Использование в Layout (RenderSection)
В файле макета (например, `_Layout.cshtml`) необходимо определить место, куда будет вставляться секция. Для этого используется метод `RenderSection` или его современный асинхронный аналог `RenderSectionAsync`

*В современном ASP.NET Core рекомендуется использовать именно асинхронную версию*
```cs
<!DOCTYPE html>
<html>
<head>
    <title>@ViewBag.Title</title>
    @await RenderSectionAsync("Styles", required: false)
</head>
<body>
    <header>
        <h1>Мой сайт</h1>
        @await RenderSectionAsync("TopMenu", required: false)
    </header>

    <main>
        @RenderBody()
    </main>

    <script src="~/js/global.js"></script>
    @await RenderSectionAsync("Scripts", required: false)
</body>
</html>
````

**Обязательность секции (required)**
Второй параметр `required: false` означает, что если View не определит эту секцию, ошибки не будет. По умолчанию этот параметр равен `true`, и если страница не передаст секцию, приложение упадет с ошибкой


## Определение секции во View (@section)
В самом файле представления (например, `Index.cshtml`) секция определяется с помощью директивы `@section`, за которой следует имя секции и блок кода в фигурных скобках

```CS
@model HomePageViewModel

<h2>Добро пожаловать</h2>
<p>Основной контент страницы...</p>

@section Styles {
    <link rel="stylesheet" href="~/css/home-page-custom.css" />
}

@section TopMenu {
    <nav>
        <a href="/promo">Акции только для Главной</a>
    </nav>
}

@section Scripts {
    <script>
        console.log("Этот скрипт загрузится только на главной странице");
    </script>
}
```


## Проверка наличия секции (IsSectionDefined)
Иногда в Layout нужно добавить какую-то оборачивающую разметку (например, `<div>` с рамкой), _только если_ секция была передана. Для этого используется метод `IsSectionDefined`

```CS
@if (IsSectionDefined("Sidebar"))
{
    <div class="sidebar-wrapper bg-light border-right">
        @await RenderSectionAsync("Sidebar", required: false)
    </div>
}
```