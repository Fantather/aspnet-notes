Для `Scaffld` нужно установить пакеты
microsoft.entityframeworkcore.design
microsoft.entityframeworkcore.sqlserver



```C#
Scaffold-DbContext "Server=FANTATHER\\SQLEXPRESS;Database=EF_Core_HW1;Trusted_Connection=True;TrustServerCertificate=True" Microsoft.EntityFrameworkCore.SqlServer
```