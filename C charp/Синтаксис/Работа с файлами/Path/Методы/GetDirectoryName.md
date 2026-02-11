Метод берёт из строки путь без имени файла
`D:\Temp\*.txt` - станет `D:\Temp`

```C#
string folder = Path.GetDirectoryName(@"D:\Temp\*.txt");
```