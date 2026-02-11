
Уникальный Айдишник
помогает так же при защите данных 
GUID (Globally Unique Identifier) — это глобально уникальный идентификатор.

- Он используется для уникальной идентификации объектов, записей в базе данных, COM-объектов и т.д.
    
- Формат стандартный: 32 цифры в шестнадцатеричной системе, разделённые дефисами:
3F2504E0-4F89-11D3-9A0C-0305E82C3301

методы
Guid g = Guid.NewGuid();

Console.WriteLine(g.ToString());      // стандартное отображение
Console.WriteLine(g.ToString("N"));   // без дефисов
Console.WriteLine(g.ToString("D"));   // стандартный формат с дефисами
Console.WriteLine(g.ToString("B"));   // в фигурных скобках
Console.WriteLine(g.ToString("P"));   // в круглых скобках
Console.WriteLine(g.ToString("X"));   // в виде массива байт

____
### 1. Создание модели с GUID
c#
using System;
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

public class User
{
    [Key]
    public Guid UserId { get; set; }  // GUID как PK

    [Required]
    [MaxLength(50)]
    public string Name { get; set; }
}

- Guid используется как тип первичного ключа.
    
- Обычно его генерируют на стороне C# (Guid.NewGuid()).
___
### 2. Контекст базы данных

c#
using Microsoft.EntityFrameworkCore;

public class AppDbContext : DbContext
{
    public DbSet<User> Users { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(
            @"Server=DESKTOP-BDMPPLC\SQLEXPRESS;Database=TestDb;Trusted_Connection=True;");
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.UserId)
            .HasDefaultValueSql("NEWID()"); // генерировать GUID на стороне SQL
    }
}

HasDefaultValueSql("NEWID()") позволяет SQL Server автоматически создавать GUID при вставке записи, если C# не указал UserId.
___
### 3. Создание и сохранение объекта
c#
using (var context = new AppDbContext())
{
    var user = new User
    {
        UserId = Guid.NewGuid(), // можно не указывать, если SQL генерирует
        Name = "Olga"
    };

    context.Users.Add(user);
    context.SaveChanges();

    Console.WriteLine($"User saved with GUID: {user.UserId}");
}


  - Guid.NewGuid() создаёт уникальный идентификатор на стороне C#.
    
- Если использовать HasDefaultValueSql("NEWID()"), можно пропустить UserId при создании.

### 4. Запрос по GUID

c#
Guid searchId = Guid.Parse("вставьте-существующий-GUID");

var user = context.Users.FirstOrDefault(u => u.UserId == searchId);
if (user != null)
{
    Console.WriteLine($"Found user: {user.Name}");
}


#резюме 
- модель с GUID
    
- контекст EF Core
    
- генерация и сохранение
    
- поиск по GUID