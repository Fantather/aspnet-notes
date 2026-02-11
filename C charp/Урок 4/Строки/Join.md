Метод `String.Join` в C# используется для **объединения элементов массива или коллекции в одну строку**, вставляя между ними заданный **разделитель**

```C#
public static string Join(string separator, params string[] values);
public static string Join(string separator, IEnumerable<string> values);
public static string Join<T>(string separator, IEnumerable<T> values);
```