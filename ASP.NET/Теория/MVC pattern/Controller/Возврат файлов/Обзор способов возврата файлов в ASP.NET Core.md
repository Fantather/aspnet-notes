В контроллерах ASP.NET Core для возврата файлов используется возвращаемый тип `IActionResult` (или его наследники `FileResult`). Существует несколько основных способов вернуть файл клиенту в зависимости от того, где он находится и в каком виде хранится.

## Основные методы возврата
**File (VirtualFileResult)**
  Используется, когда файл находится внутри папки *wwwroot* (веб-корень приложения).
  Путь указывается относительный.

**PhysicalFile (PhysicalFileResult)**
  Используется, когда файл находится в произвольном месте на диске сервера (вне *wwwroot*).
  Требует указания *абсолютного* пути.

**File (FileContentResult)**
  Используется, когда файл хранится в оперативной памяти в виде массива байтов (`byte[]`).
  Часто применяется, если файл генерируется на лету или достается из БД.

**File (FileStreamResult)**
  Используется, когда файл представлен в виде открытого потока (`Stream`).
  Полезно для эффективной передачи больших файлов без полной загрузки их в память.


## Примеры реализации
### Возврат файла из wwwroot (VirtualFileResult)
Самый простой способ, если файл является статическим ресурсом

```csharp
[HttpGet]
public IActionResult GetFileFromWwwRoot()
{
    // Путь относительно папки wwwroot
    var filePath = "images/picture.jpg";
    var fileType = "image/jpeg";
    var fileName = "downloaded_picture.jpg"; // Имя для скачивания

    return File(filePath, fileType, fileName);
}
````

### Возврат файла по абсолютному пути (PhysicalFileResult)
Используется для файлов, хранящихся, например, в специальной папке `Uploads` на сервере

```C#
[HttpGet]
public IActionResult GetPhysicalFile()
{
    // Получаем полный путь к файлу
    var filePath = Path.Combine(Directory.GetCurrentDirectory(), "Uploads", "report.pdf");
    var fileType = "application/pdf";

    return PhysicalFile(filePath, fileType, "report.pdf");
}
```

### Возврат массива байтов (FileContentResult)
Подходит для файлов, сгенерированных в памяти (например, PDF-отчет или Excel-таблица) или полученных из BLOB-поля базы данных.

```C#
[HttpGet]
public IActionResult GetBytesFile()
{
    // Пример массива байтов (здесь могла быть выборка из БД)
    byte[] fileBytes = System.IO.File.ReadAllBytes("temp/data.txt");
    var fileType = "text/plain";

    return File(fileBytes, fileType, "data.txt");
}
```

### Возврат потока (FileStreamResult)
_Наиболее эффективный_ способ для больших файлов, так как данные передаются клиенту частями по мере чтения, не занимая много оперативной памяти.

```C#
[HttpGet]
public IActionResult GetStreamFile()
{
    var filePath = Path.Combine(Directory.GetCurrentDirectory(), "videos", "big_movie.mp4");
    
    // Открываем поток (FileStream сам закроется после отправки ответа)
    var stream = new FileStream(filePath, FileMode.Open, FileAccess.Read);
    var fileType = "video/mp4";

    return File(stream, fileType, "movie.mp4");
}
```


## Важные нюансы
**MIME-типы**
Вторым аргументом всегда передается _MIME-тип_ (Content-Type). Это подсказывает браузеру, как обрабатывать файл (открыть как PDF, показать как картинку или скачать).

Пример: `application/octet-stream` (для неизвестных бинарных данных, форсирует скачивание)


**Имя файла**
Третий аргумент (имя файла) является _необязательным_

Если он указан, браузер предложит скачать файл (`Content-Disposition: attachment`)

Если не указан, браузер попытается открыть файл внутри окна (`Content-Disposition: inline`)