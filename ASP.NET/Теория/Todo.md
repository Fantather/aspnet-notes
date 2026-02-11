Расписать способы возврата файла с сервера
Расписать Основы Controller View Model

Что такое Движок представления Razor
Какие конструкции можно использовать через Razor?
Расписать ViewData, ViewBag и то что в них отличается лишь синтаксис
Модель представления как основной способ передачи данных от контроллера во View
Как передать несколько объектов через модель представления (Через класс видимо)

О мастерстранице Layout.cshtml(О едином общем шаблоне), зачем она нужна
И чем она отличается от View (RenderBody(), ViewStart.cshtml)

О секциях RenderSectionAsync
Зачем они нужны (Не только что бы передавать стили и скрипты, но и любые части разметки)
Как использовать в коде Layout

О том что ViewImports подключает пространства имён для представления, что бы не накапливать их в Файлах

Что такое Секции и как они используются

Как в итоге может выглядеть папка представления контроллера
![[Pasted image 20260209203019.png]]

Прочитать конспект учитаеля о AJAX и 




**Урок о моделях**
До 59-й минуты он показывал как заставить индекс сохраняться
Кааак
Дописать

Написать про привязчик модели
ModelState
О методе Bind и зачем он нужен (Для более тонкой настройки привязчика при скаффолдинге на пример)
О том как работает ModelState.IsValid, уточнить, что nulluble тип делает параметр не опязательным
О атрибуте `[Required]` и nulluble не ссылочном типе

Расписать о Атрибутах, которые можно указать у параметров контроллера
В каком порядке они ищутся и когда нужны

О Model.Errors
О папке Infrostructure 1.57
О создании собственного привязчика модели
```C#
public class CustomDateTimeModelBinder : IModelBinder
{
    private readonly IModelBinder fallbackBinder;
    public CustomDateTimeModelBinder(IModelBinder fallbackBinder)
    {
        this.fallbackBinder = fallbackBinder;
    }

    public Task BindModelAsync(ModelBindingContext bindingContext)
    {
        // с помощью поставщика значений получаем данные из запроса
        var datePartValues = bindingContext.ValueProvider.GetValue("Date");
        var timePartValues = bindingContext.ValueProvider.GetValue("Time");

        // если не найдено значений с данными ключами, вызываем привязчик модели по умолчанию
        if (datePartValues == ValueProviderResult.None || timePartValues == ValueProviderResult.None)
            return fallbackBinder.BindModelAsync(bindingContext);

        // получаем значения
        string? date = datePartValues.FirstValue;
        string? time = timePartValues.FirstValue;

        // Парсим дату и время
        DateTime.TryParse(date, out var parsedDateValue);
        DateTime.TryParse(time, out var parsedTimeValue);

        // Объединяем полученные значения в один объект DateTime
        var result = new DateTime(parsedDateValue.Year,
                        parsedDateValue.Month,
                        parsedDateValue.Day,
                        parsedTimeValue.Hour,
                        parsedTimeValue.Minute,
                        parsedTimeValue.Second);

        // устанавливаем результат привязки
        bindingContext.Result = ModelBindingResult.Success(result);
        return Task.CompletedTask;
    }
}


// Provider
public class CustomDateTimeModelBinderProvider : IModelBinderProvider
{
    public IModelBinder? GetBinder(ModelBinderProviderContext context)
    {
        // Для объекта SimpleTypeModelBinder необходим сервис ILoggerFactory
        // Получаем его из сервисов
        ILoggerFactory loggerFactory = context.Services.GetRequiredService<ILoggerFactory>();
        IModelBinder binder = new CustomDateTimeModelBinder(new SimpleTypeModelBinder(typeof(DateTime), loggerFactory));
        return context.Metadata.ModelType == typeof(DateTime) ? binder : null;
    }
}

// Добавление нашего собственного провайдера в builder
builder.Services.AddControllersWithViews(opts =>
{
    opts.ModelBinderProviders.Insert(0, new CustomDateTimeModelBinderProvider());
});
```
