## Attributes (Data Annotations)

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

|Атрибут|Пример|Пояснение|
|---|---|---|
|`[Key]`|`[Key] public int Id { get; set; }`|Определяет первичный ключ|
|`[Required]`|`[Required] public string Name { get; set; }`|Поле обязательно для заполнения (NOT NULL)|
|`[MaxLength(n)]`|`[MaxLength(50)] public string Name { get; set; }`|Ограничение длины строки|
|`[MinLength(n)]`|`[MinLength(5)] public string Name { get; set; }`|Минимальная длина строки|
|`[StringLength(max, MinimumLength = min)]`|`[StringLength(100, MinimumLength = 5)] public string Name { get; set; }`|Задаёт диапазон длины строки|
|`[Column("ColumnName")]`|`[Column("UserName")] public string Name { get; set; }`|Переименование колонки|
|`[Table("TableName")]`|`[Table("UsersTable")] public class User { ... }`|Переименование таблицы|
|`[ForeignKey("ForeignKeyProperty")]`|`[ForeignKey("UserId")] public Profile Profile { get; set; }`|Указывает внешний ключ|
|`[InverseProperty("NavigationProperty")]`|`[InverseProperty("Posts")] public User User { get; set; }`|Для явного указания навигационной связи|
|`[NotMapped]`|`[NotMapped] public string TempValue { get; set; }`|Свойство не создается как колонка в БД|
|`[DatabaseGenerated(DatabaseGeneratedOption.Identity)]`|`[DatabaseGenerated(DatabaseGeneratedOption.Identity)] public int Id { get; set; }`|Автоинкремент поля|
|`[Index(IsUnique = true)]` (EF Core 5+)|`[Index(IsUnique = true)] public string Email { get; set; }`|Создание индекса и уникальности колонки|
