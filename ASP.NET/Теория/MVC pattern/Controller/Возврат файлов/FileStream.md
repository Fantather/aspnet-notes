```C#
public IActionResult Index()
{
    string filePath = _appEnvironment.WebRootPath + "/Files/hello.txt";
    string fileName = "hello.txt";
    string fileType = "text/plain";

    //return PhysicalFile(filePath, fileType, fileName);

    //byte[] array = System.IO.File.ReadAllBytes(filePath);
    //return File(array, fileType, fileName);

    FileStream fs = new FileStream(filePath, FileMode.Open);
    return File(fs, fileType, fileName);
}
```