
|Команда|Что делает|
|---|---|
|`Add-Migration InitialCreate`|Создаёт миграцию на основе твоих классов `DbSet`|
|`Update-Database`|Применяет миграции к базе данных (создаёт базу и таблицы)|
|`Scaffold-DbContext "строка подключения" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Context TestUserDbContext`|Генерирует модели и DbContext на основе существующей базы|
[[Scaffold]]
[[Миграции]]
