Для установки SqlClient используется команда `Install-Package Microsoft.Data.SqlClient`

В папке appsetting.json можно указать строку подключения
```JSON
"ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=testdb;Trusted_Connection=True;"
  },
```


Получаем строку подключения в коде
```C#
//Получаем сервис IConfiguration, через свойство Services 
var configurationService = app.Services.GetService<IConfiguration>();

//С помощью индексатора обращаемся к нужной строке подключения.
//Необходимо указать полный путь к секции, через двоеточие
string connectionString = configurationService["ConnectionStrings:DefaultConnection"];
```