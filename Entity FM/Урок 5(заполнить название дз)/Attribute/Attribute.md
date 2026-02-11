## Attributes (Data Annotations)

Атрибуты зашиваются в метаданные

**Атрибуты** — это **специальные атрибуты C#**, которые применяются прямо к свойствам или классам сущностей для конфигурации EF Core.

**
### Основные атрибуты:

1. **Ключи**
    
```c#
[Key]   // первичный ключ public int Id { get; set; }
```

2. **Обязательные поля**
    
```C#
[Required] // NOT NULL public string Name { get; set; }
```

3. **Максимальная длина**
    
```C#
[MaxLength(50)] public string Name { get; set; }
```

4. **Переименование столбцов**
    
```C#
[Column("UserName")] public string Name { get; set; }
```

5. **Игнорирование свойства**
    
```C#
[NotMapped] // EF не создаст колонку public string TempValue { get; set; }
```
![[Pasted image 20250905183359.png]]
6. **Внешние ключи**
    
```C#
[ForeignKey("UserId")] public Profile Profile { get; set; }
```

#резюме 
 **Плюсы Attributes:**

- Быстро и удобно для простых моделей.
    
- Хорошо читается прямо в коде сущности.
    
- Подходит для базовой конфигурации.
___
## **Атрибуты (Data Annotations) EF Core**

|Категория|Атрибут|Назначение|Пример|
|---|---|---|---|
|**EF Core – ключи и связи**|`[Key]`|Первичный ключ|`public int Id { get; set; }`|
||`[ForeignKey("NavigationProperty")]`|Внешний ключ|`public int CategoryId { get; set; }`|
||`[NotMapped]`|Поле игнорируется EF|`public int TempValue { get; set; }`|
||`[Index(IsUnique = true)]`|Создание индекса (EF Core 7+)|`public string Email { get; set; }`|
||`[ConcurrencyCheck]`|Проверка конкурентного доступа|`public int Version { get; set; }`|
||`[Timestamp]`|Версия строки для concurrency|`public byte[] RowVersion { get; set; }`|
|**Валидация данных**|`[Required]`|Поле обязательно|`public string Name { get; set; }`|
||`[MaxLength(n)]`|Максимальная длина|`public string Name { get; set; }`|
||`[MinLength(n)]`|Минимальная длина|`public string Password { get; set; }`|
||`[StringLength(max, MinimumLength=min)]`|Длина строки|`public string Name { get; set; }`|
||`[Range(min, max)]`|Диапазон значений|`public int Age { get; set; }`|
||`[RegularExpression("regex", ErrorMessage="")]`|Проверка формата строки|`public string Email { get; set; }`|
||`[EmailAddress]`|Проверка email|`public string Email { get; set; }`|
||`[Phone]`|Проверка номера телефона|`public string PhoneNumber { get; set; }`|
||`[Url]`|Проверка URL|`public string Website { get; set; }`|
||`[CreditCard]`|Проверка номера карты|`public string CardNumber { get; set; }`|
|**Сериализация**|`[Serializable]`|Позволяет сериализовать класс|`[Serializable] public class Person { ... }`|
||`[NonSerialized]`|Игнорировать поле при сериализации|`[NonSerialized] private int tempValue;`|
|**Устаревание**|`[Obsolete("сообщение", true/false)]`|Помечает устаревший метод/класс|`[Obsolete("Используй NewMethod", true)] public void OldMethod() {}`|

### **2. Атрибуты для валидации данных**

|Атрибут|Пример|Описание|
|---|---|---|
|`[Required]`|`public string Email { get; set; }`|Поле не может быть `null`|
|`[Range(min, max)]`|`public int Age { get; set; }`|Значение должно быть в диапазоне|
|`[RegularExpression("regex", ErrorMessage="...")]`|`public string Phone { get; set; }`|Проверка по регулярному выражению|
|`[EmailAddress]`|`public string Email { get; set; }`|Проверка корректности email|
|`[Phone]`|`public string PhoneNumber { get; set; }`|Проверка корректности номера телефона|
|`[Url]`|`public string Website { get; set; }`|Проверка корректного URL|
|`[CreditCard]`|`public string CardNumber { get; set; }`|Проверка корректного номера карты|
>[!info]Пример с регулярным выражением

```c#
using System.ComponentModel.DataAnnotations;

public class User
{
    [Key]
    public int Id { get; set; }

    [Required(ErrorMessage = "Имя обязательно")]
    [MaxLength(50)]
    public string Name { get; set; }

    [Required(ErrorMessage = "Email обязателен")]
    [RegularExpression(@"^[^@\s]+@[^@\s]+\.[^@\s]+$", 
        ErrorMessage = "Некорректный формат email")]
    public string Email { get; set; }

    [Phone(ErrorMessage = "Некорректный номер телефона")]
    public string PhoneNumber { get; set; }
}

```
![[Pasted image 20250905201047.png]]