**GetInvocationList**
```C#
var list = multiDelegate.GetInvocationList();
foreach (Delegate d in list)
{
    try
    {
        d.DynamicInvoke(args); // медленнее; лучше — привести тип и вызвать явно
    }
    catch (Exception ex) { /* обработать */ }
}
```

`DynamicInvoke` использует рефлексию и медленнее, плюс оборачивает исключения в `TargetInvocationException`

Если вам нужно:

- перехватывать исключения по-отдельности для каждого подписчика, или
- анализировать/видеть target/method для каждого элемента,  то используйте `GetInvocationList()` и вызывайте каждый элемент в цикле

```C#
if (multi != null)
{
    foreach (Delegate d in multi.GetInvocationList())
    {
        try
        {
            ((Action)d)(); // приведение к конкретному типу + вызов
        }
        catch (Exception ex)
        {
            // обработать, но продолжить
        }
    }
}
```