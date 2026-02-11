## 2. **Code First**

- **Смысл**: сначала пишется C#-код (модели + `DbContext`), а EF сам создаёт/обновляет базу данных под эти модели.
    
- Разработка ведётся «от кода» → «к БД».
    
- ✅ Подходит, если:
    
    - БД ещё не существует, вы сами её проектируете;
        
    - приложение полностью контролирует структуру БД;
        
    - нужны миграции (пошаговое обновление схемы).
        
- ⚠️ Минусы:
    
    - если БД уже есть и сложная — миграции могут быть неудобны;
        
    - требуется дисциплина при изменении моделей.

```c#
public class Teacher
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
    public string Surname { get; set; } = "";
}

public class AcademyContext : DbContext
{
    public DbSet<Teacher> Teachers { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseMySql(
            "server=localhost;user=root;password=1234;database=academy;",
            new MySqlServerVersion(new Version(8, 0, 36))
        );
    }
}

```


$СODE FIRST$
1. Create models(классы)[[Пишем в коде с шарпа]]
2.  Settings
3. Migration [[Миграции]]

