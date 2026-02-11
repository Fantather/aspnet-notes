## `DbContext`

- `DbContext` — это **основной класс** в EF Core, через который приложение работает с базой данных.
    
- Он отвечает за:
    
    1. Подключение к БД.
        
    2. Отслеживание изменений у объектов.
        
    3. Формирование SQL-запросов.
        
    4. Выполнение операций **CRUD** (Create, Read, Update, Delete).
        
    5. Управление транзакциями.
        

 Можно сказать, что `DbContext` — это **мост между кодом C# и базой данных**.

```c#
using Microsoft.EntityFrameworkCore;

public class AppDbContext : DbContext
{
    public DbSet<Customer> Customers { get; set; }
    public DbSet<Order> Orders { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        // строка подключения к SQL Server
        optionsBuilder.UseSqlServer("Server=.;Database=ShopDb;Trusted_Connection=True;");
    }
}

```

- `DbSet<T>` соответствует **таблице** в базе данных.
    
- В примере будут таблицы `Customers` и `Orders`.
    
- `OnConfiguring` задаёт строку подключения.

## Работа  с дб контекст

```c#
using (var context = new AppDbContext())
{
    // Create
    var customer = new Customer { Name = "Olga" };
    context.Customers.Add(customer);
    context.SaveChanges();

    // Read
    var customers = context.Customers.ToList();

    // Update
    var firstCustomer = context.Customers.First();
    firstCustomer.Name = "Updated name";
    context.SaveChanges();

    // Delete
    context.Customers.Remove(firstCustomer);
    context.SaveChanges();
}

```
## Основные свойства и методы

- **`DbSet<TEntity>`** → таблицы (наборы сущностей).
    
- **[[Entity FM/Урок 5(заполнить название дз)/Круд операций#5 `SaveChanges()` saveChanges||SaveChanges()]]** → сохраняет изменения в БД.
    
- **`Database`** → управление базой (например, `context.Database.EnsureCreated()`, `context.Database.Migrate()`).
    
- **`ChangeTracker`** → отслеживает изменения у сущностей.
    
- **`Entry(entity)`** → позволяет работать с конкретной сущностью (например, для явной загрузки связей).
    

---

## Жизненный цикл `DbContext`

- `DbContext` — **легковесный** и предназначен для **короткоживущих операций**.
    
- Обычно создаётся **на один запрос** (например, в Web API).

```С#
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

```
___
#резюме 
- `DbContext` = рабочая единица EF Core.
- Связывает C# классы и таблицы БД.
- Управляет подключением, отслеживанием изменений и транзакциями.
- Основной инструмент для всех CRUD-операций.