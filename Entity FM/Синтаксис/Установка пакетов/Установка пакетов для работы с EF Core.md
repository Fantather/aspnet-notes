В проект, который будет работать с БД следует добавить следующие пакеты

##### Microsoft.EntityFrameworkCore
Это ядро EF Core
Это сама ORM - классы `DbContext`, `DbSet`, механизмы отслеживания изменений, перевод LINQ-запросов в выражения и общая логика миграций
Но этот пакет сам по себе не знает, как говорить с конкретной СУБД - для этого нужен провайдер (Например, *SqlServer*)

##### Microsoft.EntityFrameworkCore.SqlServer
Провайдер для MS SQL / Azure SQL
Этот пакет генерирует поддержку именно Microsoft SQL Server Manager
Расширение `UseSqlServer(...)`


```C#
Microsoft.EntityFrameworkCore.SqlServer
Microsoft.EntityFrameworkCore.Tools
Microsoft.EntityFrameworkCoer.Design
```