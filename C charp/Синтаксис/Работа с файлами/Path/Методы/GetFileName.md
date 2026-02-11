Метод берёт только имя файла или маску
Было `D:\Temp\*.txt` - стало `*.txt`
```C#
string mask = Path.GetFileName(@"D:\Temp\*.txt");
```