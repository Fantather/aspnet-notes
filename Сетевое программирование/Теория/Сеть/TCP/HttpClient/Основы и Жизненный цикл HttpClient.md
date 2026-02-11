**HttpClient** — это класс в пространстве имен `System.Net.Http` для отправки HTTP-запросов и получения HTTP-ответов от ресурса, идентифицируемого по URI.


## Основные характеристики
* Находится в сборке `System.Net.Http`.
* Предоставляет асинхронные методы для основных HTTP-глаголов (GET, POST, PUT, DELETE).
* Реализует `IDisposable`, но имеет нюансы с освобождением ресурсов.


## Проблема с IDisposable и Socket Exhaustion
Вопреки стандартной практике работы с `IDisposable`, оборачивать `HttpClient` в блок `using` при каждом запросе — **антипаттерн**.

### Неправильное использование
```csharp
// ПЛОХО: Создание нового экземпляра на каждый запрос
using (var client = new HttpClient())
{
    var result = await client.GetAsync("[https://api.example.com](https://api.example.com)");
}
````

### Техническое обоснование
При уничтожении объекта `HttpClient` (вызов `Dispose`), базовый сокет не освобождается мгновенно. Он переходит в состояние `TIME_WAIT` операционной системы, ожидая закрытия соединения.

- Если создавать много экземпляров `HttpClient` под нагрузкой, операционная система исчерпает доступные сокеты (Ephemeral ports).
    
- Это приводит к исключению `SocketException`.


### Правильное использование (Singleton)
Для простых консольных приложений (до внедрения `IHttpClientFactory`) рекомендуется использовать статический или единственный экземпляр на всё время жизни приложения.

```C#
public class DataService
{
    // Статический экземпляр
    private static readonly HttpClient _httpClient = new HttpClient();

    public async Task GetDataAsync()
    {
        // Переиспользование одного и того же клиента
        var response = await _httpClient.GetAsync("[https://api.example.com/data](https://api.example.com/data)");
    }
}
```

_Однако, у подхода Singleton есть свой минус: он не реагирует на изменения DNS. Если IP-адрес удаленного сервиса изменится, Singleton продолжит стучаться по старому адресу до перезагрузки приложения._

## Основные методы запросов

`HttpClient` предоставляет методы-обертки для стандартных HTTP-методов.

### GET (Получение данных)

Используется для запроса представления ресурса.

C#

```
// Получение только заголовков и статуса
HttpResponseMessage response = await _httpClient.GetAsync("[https://api.example.com/users](https://api.example.com/users)");

// Проверка на успешный статус код (200-299)
// Если статус ошибка - выбросит HttpRequestException
response.EnsureSuccessStatusCode(); 

// Чтение тела ответа как строки
string content = await response.Content.ReadAsStringAsync();
```

### POST (Отправка данных)

Используется для отправки сущностей к определенному ресурсу.

C#

```
var newUser = new { Name = "John", Age = 30 };

// Сериализация и отправка JSON (System.Net.Http.Json)
// Метод PostAsJsonAsync автоматически добавляет заголовок Content-Type: application/json
var response = await _httpClient.PostAsJsonAsync("[https://api.example.com/users](https://api.example.com/users)", newUser);
```

### Управление заголовками

Заголовки можно задавать глобально для клиента или для конкретного запроса.

**Глобальные (DefaultRequestHeaders):**

Применяются ко всем запросам этого клиента.

C#

```
_httpClient.DefaultRequestHeaders.Add("User-Agent", "MyApp/1.0");
_httpClient.DefaultRequestHeaders.Accept.Add(
    new MediaTypeWithQualityHeaderValue("application/json"));
```

**Для одного запроса (HttpRequestMessage):**

Если нужен полный контроль над запросом, используется `SendAsync`.

C#

```
var request = new HttpRequestMessage(HttpMethod.Get, "[https://api.example.com/secret](https://api.example.com/secret)");
request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", "token_xyz");

var response = await _httpClient.SendAsync(request);
```

---

**Связанные темы:**

[[02_IHttpClientFactory]] — Решение проблем с сокетами и DNS

[[03_Resilience_Polly]] — Обработка ошибок сети

```

### Следующий шаг
Это была база. Переходим ко второму файлу `02_IHttpClientFactory.md`, где разберем **Typed Clients** (типизированные клиенты) — это стандарт де-факто для ASP.NET Core.

Готов генерировать вторую часть?
```