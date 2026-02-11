Это **универсальный способ** свёртки (редукции) коллекции по заданной логике

```C#
int product = numbers.Aggregate((acc, x) => acc * x); // 24 (1×2×3×4)

// С начальным значением
int sumWithStart = numbers.Aggregate(10, (acc, x) => acc + x); // 20
```