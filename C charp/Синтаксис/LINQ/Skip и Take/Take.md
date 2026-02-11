`Take` - Возвращает указанное количество элементов, начиная с начала последовательности


**Сигнатура**
```C#
IEnumerable<TSource> Take<TSource>(this IEnumerable<TSource> source, int count)
```


**Особенности**
- Если `count` превышает количество элементов, возвращается пустая последовательность
- Если `count = 0`, возвращаются все элементы
- Если `count < 0`, вызывает исключение `ArgumentOutOfRangeException`


**Пример**
```C#
var numbers = new[] { 1, 2, 3, 4, 5 };
var result = numbers.Take(3); // Берём первые 3 элемента
// Результат: { 1, 2, 3 }
```