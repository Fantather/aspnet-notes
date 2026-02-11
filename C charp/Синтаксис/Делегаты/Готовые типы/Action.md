Делегат который ничего не возвращает
Принимает до 16 параметров

```C#
Action<string> print = str => Console.Writeline(str);
print("Hello"); // Выведет "Hello"
```