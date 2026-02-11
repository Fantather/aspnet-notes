## Настройка HttpClient в ASP.NET Core

### Хранение настроек
**Конфигурация в appsettings.json**
  Секретные ключи и базовые адреса следует выносить в конфигурационный файл для разделения кода и данных

**Синтаксис хранения**
  Настройки группируются в секции с использованием стандартного формата JSON
```json
{
  "ApiSettings": {
    "AccessToken": "твой_длинный_jwt_токен"
  }
}
````


### Настройка в Program.cs
**Доступ к конфигурации**
Чтение данных осуществляется через свойство _builder.Configuration_ с использованием двоеточия для навигации по вложенным секциям

**Регистрация типизированного клиента**
Метод _AddHttpClient_ внедряет сервис в контейнер зависимостей и позволяет задать глобальные настройки для конкретного экземпляра _HttpClient_

**Настройка BaseAddress**
Определяет корневой маршрут для всех запросов в рамках данного клиента, обязательно требуя завершающего слеша в строке URL

**Настройка DefaultRequestHeaders**
Обеспечивает автоматическое добавление заданных HTTP-заголовков, таких как _Authorization_ со схемой _Bearer_, к каждому исходящему запросу


```C#
// Чтение токена по пути Секция:Ключ
string accessToken = builder.Configuration["ApiSettings:AccessToken"];

// Регистрация сервиса и настройка HTTP клиента
builder.Services.AddHttpClient<TmdbService>(client =>
{
    // Установка базового адреса
    client.BaseAddress = new Uri("[https://api.themoviedb.org/3/](https://api.themoviedb.org/3/)");
    
    // Установка защищенного заголовка авторизации
    client.DefaultRequestHeaders.Add("Authorization", $"Bearer {accessToken}");
});
```


### Использование настроенного клиента
**Внедрение через конструктор**
Благодаря паттерну внедрения зависимостей, в конструктор сервиса передается уже полностью сконфигурированный экземпляр _HttpClient_

**Выполнение запросов**
При вызове методов HTTP-клиента указывается только конечный маршрут (endpoint), так как базовый адрес и необходимые заголовки подставляются автоматически

```C#
public class TmdbService
{
    private readonly HttpClient _httpClient;

    // Сюда приходит клиент с уже установленным BaseAddress и заголовками
    public TmdbService(HttpClient httpClient)
    {
        _httpClient = httpClient;
    }

    public async Task<MovieResponse?> GetMoviesAsync()
    {
        // Используется только конечный маршрут
        var response = await _httpClient.GetAsync("movie/popular");
        
        // Дальнейшая обработка ответа...
    }
}
```