Пользовательские Html-хелперы позволяют инкапсулировать логику генерации HTML-разметки в C# код для её многократного использования в различных Представлениях (Views). Технически они представляют собой статические методы расширения (Extension methods) для интерфейса `IHtmlHelper`.

## Виды методов-хелперов
**Обычные хелперы**
  Расширяют базовый интерфейс `IHtmlHelper` и принимают данные в качестве обычных параметров (строк, массивов, объектов). Они независимы от конкретной модели Представления

**Строго типизированные хелперы**
  Расширяют интерфейс `IHtmlHelper<TModel>`. Они предназначены для работы с конкретной ViewModel и используют лямбда-выражения (`Expression<Func<TModel, TValue>>`) для извлечения значений и метаданных свойств модели

## Создание собственного хелпера
Пользовательские хелперы принято размещать в отдельной папке на уровне проекта, которая обычно называется `Helpers`.

В примере ниже создается метод расширения `CreateList`, который генерирует HTML-список `<ul>` на основе переданного массива строк. Для безопасной и удобной генерации тегов используется класс `TagBuilder`.

```csharp
using Microsoft.AspNetCore.Html;
using Microsoft.AspNetCore.Mvc.Rendering;
using System.Text.Encodings.Web;

namespace MyProject.Helpers
{
    public static class ListHelper
    {
        public static HtmlString CreateList(this IHtmlHelper html, string[] items)
        {
            // Создаем корневой тег ul
            TagBuilder ul = new TagBuilder("ul");
            
            foreach (string item in items)
            {
                // Создаем вложенный тег li
                TagBuilder li = new TagBuilder("li");
                li.InnerHtml.Append(item);
                
                // Добавляем li внутрь ul
                ul.InnerHtml.AppendHtml(li);
            }
            
            // Добавляем CSS атрибут
            ul.Attributes.Add("class", "itemsList");
            
            // Рендерим результат в строку
            using var writer = new System.IO.StringWriter();
            ul.WriteTo(writer, HtmlEncoder.Default);
            
            // Возвращаем безопасную для рендеринга HTML строку
            return new HtmlString(writer.ToString());
        }
    }
}
````


## Подключение и использование во View
Чтобы созданный метод расширения стал доступен в файле `.cshtml`, необходимо импортировать пространство имен, в котором находится статический класс хелпера.

_Для того чтобы хелпер был доступен глобально во всех Представлениях, директиву `@using` рекомендуется добавлять в файл `_ViewImports.cshtml`._

```CS
@using MyProject.Helpers
@model MyViewModel

@{
    string[] cities = new string[] { "London", "Paris", "Berlin" };
    string[] countries = new string[] { "Great Britain", "France", "Germany" };
}

<h3>Cities</h3>
@Html.CreateList(cities)
<br />

<h3>Countries</h3>
@ListHelper.CreateList(Html, countries)
```