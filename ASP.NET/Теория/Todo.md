Как в итоге может выглядеть папка представления контроллера
![[Pasted image 20260209203019.png]]

Прочитать конспект учитаеля о AJAX и 




---
Про методы хелперы
Обычные и строготипизированные
Подключение Хелпера к View
Хелпер находится во вкладке Helpers
```C#
public static HtmlString CreateList(this IHtmlHelper html, string[] items)
{
    TagBuilder ul = new TagBuilder("ul");
    foreach (string item in items)
    {
        TagBuilder li = new TagBuilder("li");
        li.InnerHtml.Append(item);
        ul.InnerHtml.AppendHtml(li);
    }
    ul.Attributes.Add("class", "itemsList");
    var writer = new System.IO.StringWriter();
    ul.WriteTo(writer, HtmlEncoder.Default);
    return new HtmlString(writer.ToString());
}



// View
@{
    string[] cities = new string[] { "London", "Paris", "Berlin." };
    string[] countries = new string[] { "Great Britain", "France", "Germany." };
}

<h3>Cities</h3>
@Html.CreateList(cities)
<br />

<!-- или можно вызвать так -->

<h3>Countries</h3>
@ListHelper.CreateList(Html, countries)
```

О HTML хелперах
Обязательно упомянуть(
Значения по умолчанию
Примеры, как реализовать разные распространённые теги, обязательно select)

О Тег хелперах
Обязательно упомянуть(
Упомянуть их импорт
Значения по умолчанию
Примеры, как реализовать разные распространённые теги, обязательно select, form, тег a, link
Отдельно о разнице между тег и html хелперах)

И эти тоже
script asp-src-include
asp-area
```C#
<script src="[https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.2.0.min.js"](https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.2.0.min.js%22 "https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.2.0.min.js%22")
        asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
        asp-fallback-test="window.jQuery"
        crossorigin="anonymous"
```

О тег хелпере chache
Его свойства (Обязательно expires-after, expires-on, vary-by-user, vary-by-query)
Что так можно кешировать


Преимущества и недостатки html хелперов и тег хелперов
Учитель говорил что у каждого из них есть место для применения в своём контексте, так что уточнить для чего именно они применяются стоит 