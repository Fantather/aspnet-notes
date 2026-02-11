Используется для поиска файлов по маске
Возвращает массив строк, каждая строка - это путь к файлу, который соответствует указанной маске

**Исключения**
- `DirectoryNotFoundException` - Если не найдена указанная папка
- `UnauthorizedAccessException` - Если недостаточно прав доступа


**Поиск в указанной папке**
```C#
string path = @"C:\Temp";   // Папка для поиска
string mask = "*.txt"       // Маска файлов

// Получаем файлы по маске
string[] files = Directory.GetFiles(path, mask);
```


**С поиском во всех подпапках**
```C#
string[] files = Directory.GetFiles(path, mask, SearchOption.AllDirectories);
```
- `SearchOption.TopDirectory` - Искать только в указанной папке
- `SearchOption.AllDirectories` - Искать во всех вложенных папках

**Несколько масок**
Метод `GetFiles` принимает только одну маску за раз, но для нескольких масок можно сделать цикл
```C#
string[] masks = {"*.txt", "*.json"};
foreach string mask in masks
{
	string[] files = Directory.GetFiles(path, mask, SearchOption.AllDirectories);
	foreach(string f in files)
		Console.WriteLine(f);
}
```


**Полный пример**
```C#
string path = @"C:\Temp";   // Папка для поиска
string mask = "*.txt"       // Маска файлов

// Получаем файлы по маске
string[] files = Directory.GetFiles(path, mask);

Console.Writeline("Найденные файлы:")
foreach (string file in files)
	Console.Writeline(file);
```