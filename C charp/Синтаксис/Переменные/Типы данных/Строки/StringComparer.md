Это готовая реализация интерфейса `IComparer<string>` которая позволяет работать с разными правилами сортировки и регистрами букв, принятыми в конкретной языковой среде

- `StringComparer.Ordinal` - побайтовое (точное) сравнение (регистрозависимое)
- `StringComparer.OrdinalIgnoreCase` - побайтовое, без учёта регистра
- `StringComparer.CurrentCultureIgnoreCase` - текущая культура, без учёта регистра


```C#
var names = new[] { "zebra", "Apple", "банан" };

// Сортировка без учёта регистра, по текущей культуре
Array.Sort(names, StringComparer.CurrentCultureIgnoreCase);

// Сортировка «чистой» байтовой (ordinal)
Array.Sort(names, StringComparer.Ordinal);
```