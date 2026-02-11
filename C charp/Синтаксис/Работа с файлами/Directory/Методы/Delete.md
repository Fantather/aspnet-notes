Метод для удаления папок

`Directory.Delete(path, bool)`
- `path` - путь к удаляемой папке
- `bool` - флаг, разрешает удалять не пустые папки, без него будет исключение

**Удаление папки**
```C#
string folder = @"C:\Temp";

if(Directory.Exists(folder))
	Directory.Delete(folder);
```

**Удаление папки и всех подпапок**
Удаляет вне зависимости от наличия содержимого
```C#
string folder = @"C:\Temp";

if(Directory.Exists(folder))
{
	// Получаем путь ко всем подпапкам в папке
	string[] subdirs = Directory.GetDirectories(folder);
	
	foreach(string dir in subdirs)
		Directory.Delete(dir, true);  // true - удалить рекурсивно
}
```


**Удаление только пустых подпапок в папке**
```C#
string folder = @"C:\Temp";

if(Directory.Exists(folder))
{
	// Получаем путь ко всем подпапкам в папке
	string[] subdirs = Directory.GetDirectories(folder);
	
	foreach(string dir in subdirs)
	{
		if(Directory.GetFiles(dir).Length == 0 && Directory.GetFiles(dir).Length == 0)
		{
			Directiry.Delete(dir);
		}
	}
}
```