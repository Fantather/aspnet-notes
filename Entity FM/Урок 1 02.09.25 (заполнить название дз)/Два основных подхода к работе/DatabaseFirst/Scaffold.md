![[Pasted image 20250902205529.png]]
```pmc
Scaffold-DbContext "Server=НАЗВАНИЕ ВАШЕГО SQL СЕРВЕРА;Database=НАЗВАНИЕ ВАШЕЙ БАЗЫ ДАННЫХ;TrustServerCertificate=true;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```


- `"Server=...;Database=...;..."` — строка подключения к твоему SQL Server.
    
- `Microsoft.EntityFrameworkCore.SqlServer` — провайдер базы данных (SQL Server)
- 
Опционально, чтобы файлы генерировались в определённой папке и с нужными настройками, можно добавить:

`-OutputDir Models -Context TestUserDbContext -Force`

- `-OutputDir Models` → куда положить сгенерированные классы.
    
- `-Context TestUserDbContext` → имя контекста, который будет создан.
    
- `-Force` → перезаписать файлы, если они уже есть.
    

Итог: после выполнения команды ты получишь **классы сущностей и DbContext**, соответствующие таблицам в базе, и сможешь работать с ними через EF Core.

