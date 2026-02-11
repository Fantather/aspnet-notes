Проверяет, существует ли файл по указанному пути
Возвращает `bool`

```C#
string path = @"C:\Temp\test.txt";

if(File.Exists(path))
{
	//....//
}
```