## Fluent API

[[Entity FM/Урок 5(заполнить название дз)/код урока]]
[[Налаштування моделей]]

**Fluent API** — это способ конфигурировать сущности и их связи в EF Core с помощью **кода**, а не атрибутов. Используется в методе `OnModelCreating` контекста данных (`DbContext`).

Конструктор без параметров обязателен для миграций

![[Pasted image 20250904200217.png]]

### Основные возможности Fluent API:

1. **Настройка ключей**
    
```C#
modelBuilder.Entity<User>()     .HasKey(u => u.Id); // задаем первичный ключ
```

2. **Настройка свойств**
    
```C#
modelBuilder.Entity<User>()     .Property(u => u.Name)     .IsRequired()        // NOT NULL     .HasMaxLength(50);   // максимальная длина
```


3. **Настройка связей**
- **Один к одному** 
```C#
modelBuilder.Entity<User>()     .HasOne(u => u.Profile)     .WithOne(p => p.User)     .HasForeignKey<Profile>(p => p.UserId);
```

- **Один ко многим**
```C#
modelBuilder.Entity<User>()     .HasMany(u => u.Posts)     .WithOne(p => p.User)     .HasForeignKey(p => p.UserId);
```

- **Многие ко многим**
```C#
modelBuilder.Entity<User>()     .HasMany(u => u.Roles)     .WithMany(r => r.Users)     .UsingEntity<UserRole>();
```

4. **Конфигурация таблиц**
    
```C#
modelBuilder.Entity<User>()     .ToTable("UsersTable"); // переименовать таблицу
```


5. **Конфигурация индексов**
    
```C#
modelBuilder.Entity<User>()     .HasIndex(u => u.Email)     .IsUnique(); // уникальный индекс
```

#резюме 
**Плюсы Fluent API:**

- Полный контроль над конфигурацией.
    
- Можно задавать все настройки, которые недоступны через атрибуты.
    
- Подходит для сложных моделей и связей.
 ___
## **Fluent API EF Core**

|Метод / Конфигурация|Пример|Пояснение|
|---|---|---|
|`HasKey`|`modelBuilder.Entity<User>().HasKey(u => u.Id);`|Задаёт первичный ключ, можно составной|
|`Property`|`modelBuilder.Entity<User>().Property(u => u.Name);`|Конфигурирует свойства|
|`IsRequired`|`modelBuilder.Entity<User>().Property(u => u.Name).IsRequired();`|Поле обязательно для заполнения|
|`HasMaxLength`|`modelBuilder.Entity<User>().Property(u => u.Name).HasMaxLength(50);`|Ограничение длины строки|
|`HasColumnName`|`modelBuilder.Entity<User>().Property(u => u.Name).HasColumnName("UserName");`|Переименование колонки|
|`ToTable`|`modelBuilder.Entity<User>().ToTable("UsersTable");`|Переименование таблицы|
|`HasOne / WithOne`|`modelBuilder.Entity<User>().HasOne(u => u.Profile).WithOne(p => p.User).HasForeignKey<Profile>(p => p.UserId);`|Связь «один к одному»|
|`HasMany / WithOne`|`modelBuilder.Entity<User>().HasMany(u => u.Posts).WithOne(p => p.User).HasForeignKey(p => p.UserId);`|Связь «один ко многим»|
|`HasMany / WithMany`|`modelBuilder.Entity<User>().HasMany(u => u.Roles).WithMany(r => r.Users).UsingEntity<UserRole>();`|Связь «многие ко многим»|
|`Ignore`|`modelBuilder.Entity<User>().Ignore(u => u.TempValue);`|Игнорирование свойства|
|`HasIndex`|`modelBuilder.Entity<User>().HasIndex(u => u.Email).IsUnique();`|Создание индекса и уникальности|
|`HasDefaultValue`|`modelBuilder.Entity<User>().Property(u => u.IsActive).HasDefaultValue(true);`|Значение по умолчанию для колонки|
|`HasDefaultValueSql`|`modelBuilder.Entity<User>().Property(u => u.CreatedAt).HasDefaultValueSql("GETDATE()");`|SQL-выражение по умолчанию|
|`HasColumnType`|`modelBuilder.Entity<User>().Property(u => u.Price).HasColumnType("decimal(10,2)");`|Явное указание типа колонки|
|`HasConversion`|`modelBuilder.Entity<User>().Property(u => u.Status).HasConversion<string>();`|Конвертация свойства в другой тип для БД|
|`HasComment`|`modelBuilder.Entity<User>().Property(u => u.Name).HasComment("Имя пользователя");`|Добавление комментария к колонке|
