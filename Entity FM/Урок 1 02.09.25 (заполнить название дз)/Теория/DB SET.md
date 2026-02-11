к классе контекста мы создаем классы назвая
и в бд так назовется табица

**DB SET**- коллекция объектов юзеров. И она пов'язана с коллекцией записей в базе данних  ^dbset
на ее основе будет созданна соответсущая таблица в базе данных
```c#
public class AcademyContext : DbContext
{
    public DbSet<User> Users { get; set; }   // коллекция пользователей
    public DbSet<Lecture> Lectures { get; set; } // коллекция лекций
}

```

## 1. В `DbContext` ты объявляешь свойства типа `DbSet<T>`.
```csharp
public class AcademyContext : DbContext
{
    public DbSet<Teacher> Teachers { get; set; }
    public DbSet<Lecture> Lectures { get; set; }
}

```
- EF Core понимает: `Teacher` и `Lecture` — это сущности (модели).
    
- По умолчанию название таблицы в базе будет совпадать с названием свойства `DbSet` или с именем класса сущности (если `DbSet` отсутствует).
    
    - В примере выше создадутся таблицы `Teachers` и `Lectures`.
        
    - Если ты назовёшь свойство иначе:
```c#
public DbSet<Teacher> PeopleWhoTeach { get; set; }

```

- - то EF создаст таблицу `PeopleWhoTeach`.
        
- Если нужно задать другое имя таблицы вручную, используют аннотацию:
```C#
[Table("tbl_teachers")]
public class Teacher
{
    public int Id { get; set; }
    public string Name { get; set; } = null!;
}
```

или настройку в `OnModelCreating`:

```c#
modelBuilder.Entity<Teacher>().ToTable("tbl_teachers");
```
#резюме
- `DbSet` в контексте = таблица в базе.
    
- Имя по умолчанию берётся из названия свойства (`Teachers`) или из класса (`Teacher` → `Teachers`).
    
- Можно переименовать через атрибут `[Table]` или Fluent API.
___
![[Pasted image 20250902190019.png]]

[[Mapping|Такое является меппингом]]
___
