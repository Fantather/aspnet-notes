У меня в классе для Словаря было приватное поле `Dictionary<string, List<string>`
Это для слова которое переводят
И Список для перевода, ведь перевод может быть не один
Я хотел, что бы класс имел свойство `IReadOnlyDictionary<string, List<string>`, но тогда, внутренний `List<string>` оставался изменяемым, что меня не устроило

ИИ много чего предложил, самым эффективным для эффективного возвращения полностью неизменяемого значения он назвал создание своего собственного класса обёртки
Объект этого класса будет публичным свойством в основном классе

Так же он написал, что если к Словарю не будут обращаться 1 000 000 раз в секунду, то реализовывать в нём кеш, что бы не создавать небольшой `ReadOnlyList<string>` (перевод же нужно вернуть) при каждом обращении по ключу - не имеет смысла

Но я решил это сделать

**Основной класс**
```C#
private readonly Dictionary<string, List<string>> _dictionary;
public readonly CachingReadOnlyDictionaryWrapper ReadOnlyWrapper;
```

**Класс-обёртка**
```C#
internal class ChacingReadOnlyDictionaryWrapper : IReadOnlyDictionary<string, IReadOnlyList<string>>
{
    private readonly Dictionary<string, List<string>> _sourceDictionary;                    // Ссылка на оригинальный Словарь
    private readonly ConcurrentDictionary<string, IReadOnlyList<string>> _cache = new();    // Кеш

    // Конструктор
    public ChacingReadOnlyDictionaryWrapper(Dictionary<string, List<string>> sourceDictionary)
    {
        _sourceDictionary = sourceDictionary ?? throw new ArgumentNullException(nameof(sourceDictionary));
    }

    public static implicit operator ChacingReadOnlyDictionaryWrapper(Dictionary<string, List<string>> dictionary)
    {
        return new ChacingReadOnlyDictionaryWrapper(dictionary);
    }


    // Свойства интерфейса
    public int Count => _sourceDictionary.Count;
    public IEnumerable<string> Keys => _sourceDictionary.Keys;
    public IEnumerable<IReadOnlyList<string>> Values => Keys.Select(key => this[key]);


    public IReadOnlyList<string> this[string key]
    {
        get
        {
            if (_cache.TryGetValue(key, out var cached))
                return cached;

            if (_sourceDictionary.TryGetValue(key, out List<string> sourseList))
            {
                IReadOnlyList<string> readOnlyList = sourseList.AsReadOnly();
                _cache[key] = readOnlyList;
                return readOnlyList;
            }

            throw new KeyNotFoundException($"Key {key} not found.");
        }
    }

    // Методы интерфейса
    public bool TryGetValue(string key, out IReadOnlyList<string>? value)
    {
        if( _cache.TryGetValue(key, out value))
            return true;

        if (_sourceDictionary.TryGetValue(key, out List<string> list))
        {
            value = list.AsReadOnly();
            _cache[key] = value;
            return true;
        }

        value = null;
        return false;
    }

    public bool ContainsKey(string key) => _sourceDictionary.ContainsKey(key);

    public IEnumerator<KeyValuePair<string, IReadOnlyList<string>>> GetEnumerator()
    {
        foreach(var key in Keys)
            yield return new KeyValuePair<string, IReadOnlyList<string>>(key, this[key]);
    }
    IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();


    // --- Методы для управления Кешем ---

    // Удалить конкретный ключ из Кеша
    public void Invalidate(string key) => _cache.TryRemove(key, out _);

    // Полностью очистить Кеш
    public void Clear() => _cache.Clear();
}
```
