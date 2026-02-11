Вот полная инструкция для твоего `Program.cs` (Minimal API).

### 1. Установка пакета (NuGet)

Без этого кода работать не будет. Открой **Консоль диспетчера пакетов** (Tools -> NuGet Package Manager -> Package Manager Console) и выполни:

PowerShell

```
Install-Package Swashbuckle.AspNetCore
```

### 2. Настройка в Program.cs

В файле `Program.cs` нужно добавить код в двух местах:

1. **До** `builder.Build()` — регистрация сервисов (генератора).
    
2. **После** `builder.Build()` — включение middleware (самой страницы).
    

Вот готовый шаблон. Скопируй нужные строки в свой файл:

C#

```
var builder = WebApplication.CreateBuilder(args);

// --- 1. РЕГИСТРАЦИЯ СЕРВИСОВ (До builder.Build) ---

// Обязательно для Minimal API: позволяет Swagger читать твои MapGet/MapPost
builder.Services.AddEndpointsApiExplorer();

// Сам генератор Swagger
builder.Services.AddSwaggerGen();

// Твои сервисы (репозиторий и т.д.)
builder.Services.AddSingleton<IUserRepository, UserRepository>();

var app = builder.Build();

// --- 2. НАСТРОЙКА КОНВЕЙЕРА (После builder.Build) ---

// Включаем Swagger (обычно только для разработки, но можно убрать if)
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();   // Создает JSON-файл схемы
    app.UseSwaggerUI(); // Создает красивую HTML-страницу
}

// Твои маршруты (Endpoints)
app.MapGet("/", ...);
// ...

app.Run();
```

### 3. Как запустить

1. Запусти проект (F5 или Ctrl+F5).
    
2. В браузере к адресу сайта добавь `/swagger`.
    
    - Пример: `https://localhost:7051/swagger`
        

### Почему это работает?

1. **`AddEndpointsApiExplorer`**: Сканирует твой код и находит все конструкции `app.MapGet`, `app.MapPost` и т.д. Без этой строки в Minimal API Swagger будет пустым.
    
2. **`AddSwaggerGen`**: Превращает найденные маршруты в документацию.
    
3. **`UseSwaggerUI`**: Рисует тот самый сине-зеленый интерфейс, где можно нажимать кнопки "Try it out".