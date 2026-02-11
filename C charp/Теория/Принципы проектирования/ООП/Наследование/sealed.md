Ключевое слово `sealed` используется для запрета наследования

*`sealed` для класса:*
Предотвращает наследование от этого класса
Используется для безопасности (чтобы никто не мог изменить поведение критически важного класса, например, `string` в C# sealed), или для оптимизации (компилятор может инлайнить вызовы методов sealed-классов)
```C#
public sealed class NoOneCanInheritFromMe
{
    // ... 
}

// ОШИБКА КОМПИЛЯЦИИ:
public class TryToInherit : NoOneCanInheritFromMe {}
```


*`sealed` для метода:*
Предотвращает дальнейшее переопределение виртуального или абстрактного метода в производных классах
Может применяться только к методам, которые уже переопределены (`override`)

```C#
public class BaseClass
{
    public virtual void MyMethod() { }
}

public class DerivedClass : BaseClass
{
    public sealed override void MyMethod() { }
}

public class FurtherDerivedClass : DerivedClass
{
    // ОШИБКА КОМПИЛЯЦИИ: Cannot override inherited member because it is sealed.
    // public override void MyMethod() { }
}
```