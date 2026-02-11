Короткий ответ — да, **проверки нужны**, если твой метод _не умеет_ работать с `null`
Выбросить `ArgumentNullException` на входе — правильный и общепринятый паттерн «fail fast». Вызывать исключение из отдельного вспомогательного метода — тоже нормально; у этого подхода есть свои плюсы и минусы. Ниже — разбор и готовые варианты с рекомендациями.

# Почему всё-таки проверять

- Компилятор (nullable reference types) — это только подсказка для разработчика/анализатора. В рантайме любой может передать `null`.
    
- Явная проверка даёт предсказуемое поведение (чёткое исключение `ArgumentNullException`) и удобный стек вызовов для отладки.
    
- Особенно важно на **публичных/публично-доступных** API: проверяй входные параметры всегда.
    

# Где можно НЕ проверять

- В _внутренних_ приватных методах, если ты гарантируешь, что все публичные вызовы уже проверили параметры.
    
- В горячих участках кода, где накладные расходы проверки критичны — но это редкий случай, и тогда нужна документация/контракты.
    

# Как лучше выбросить исключение — прямо в методе или через вспомогательный?

Оба варианта рабочие.

Плюсы **встроенных** проверок:

- Простота и читаемость (видно сразу у вызова).
    
- Локализованное сообщение (если нужно указать метод/контекст).
    

Плюсы **вспомогательного** метода (`CheckPairNull` / `ThrowIfNull` / `ThrowHelper`):

- Нет дублирования кода.
    
- Можно централизованно управлять форматом сообщения и аннотациями (например, `[DoesNotReturn]`).
    
- При использовании `ThrowHelper` компилятор/ритерны JIT могут лучше инлайнить основной путь (микро-оптимизация).
    

В общем: **вспомогательный метод — ок**, если он прост и понятен. Нет ничего «плохого», что исключение выбрасывается из него.

# Какие исключения и как содержать `paramName`

- `ArgumentNullException` — для `null`.
    
- `ArgumentException` — для недопустимых значений (например, пустая строка, если ты этого не принимаешь).
    
- Используй `nameof(param)` как `paramName`. Не обязательно включать имя метода в текст — стек вызовов это покажет.
    

# Практические примеры

1. Простой inline-вариант
    

```csharp
public void AddWord(string sourceWord, string targetWord)
{
    if (string.IsNullOrWhiteSpace(sourceWord))
        throw new ArgumentNullException(nameof(sourceWord));
    if (string.IsNullOrWhiteSpace(targetWord))
        throw new ArgumentNullException(nameof(targetWord));

    if (!_dictionary.TryAdd(sourceWord, new List<string>{ targetWord }))
        AddTranslate(sourceWord, targetWord);
}
```

Используй `IsNullOrWhiteSpace` если пустые/пробелы тоже нельзя.

2. Вспомогательный метод (очень читаемо)
    

```csharp
private static void CheckPairNull(string sourceWord, string targetWord)
{
    if (string.IsNullOrWhiteSpace(sourceWord))
        throw new ArgumentNullException(nameof(sourceWord));
    if (string.IsNullOrWhiteSpace(targetWord))
        throw new ArgumentNullException(nameof(targetWord));
}

public void AddWord(string sourceWord, string targetWord)
{
    CheckPairNull(sourceWord, targetWord);
    if (!_dictionary.TryAdd(sourceWord, new List<string>{ targetWord }))
        AddTranslate(sourceWord, targetWord);
}
```

3. Современный аккуратный ThrowHelper (использует `CallerArgumentExpression`)
    

```csharp
using System.Runtime.CompilerServices;
using System.Diagnostics.CodeAnalysis;

public static class Guard
{
    public static void NotNullOrWhiteSpace(
        string? value,
        [CallerArgumentExpression("value")] string? paramName = null)
    {
        if (string.IsNullOrWhiteSpace(value))
            throw new ArgumentNullException(paramName);
    }
}
```

Использование:

```csharp
public void AddWord(string sourceWord, string targetWord)
{
    Guard.NotNullOrWhiteSpace(sourceWord);
    Guard.NotNullOrWhiteSpace(targetWord);

    // ...
}
```

`CallerArgumentExpression` позволяет автоматически подставлять имя аргумента в исключение.

# Рекомендация (что выбрать)

1. Для **публичных API** — обязательно проверяй и кидай `ArgumentNullException`/`ArgumentException`.
    
2. Для **внутренних/приватных** — можно полагаться на проверки выше по цепочке, но это должно быть очевидно (и документировано).
    
3. Если дублирование проверок мешает — вынеси `Guard`/`ThrowHelper`. Это чисто практично и нормально.
    
4. Не траться на сложные кастомные сообщения с указанием имени метода — `paramName` + стек вызовов обычно достаточно.
    

---

Хочешь — могу сразу:

- переписать твой метод с `Guard`-вариантом (и подправить `TryAdd` вызов, у тебя в примере был синтаксически некорректный `[targetWord]`),
    
- или показать реализацию `ThrowHelper` с `[DoesNotReturn]` и micro-оптимизациями. Что тебе полезнее — готов сделать прямо здесь.