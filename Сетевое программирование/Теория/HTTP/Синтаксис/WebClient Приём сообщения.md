```C#
await Task.Run(() =>
{
	try
	{
		// WebClient предоставляет общие методы обмена данными с ресурсом, заданным URI
		//обертка для более старого HttpWebRequest.
		WebClient client = new WebClient();

		//  DownloadData загружает данные с указанным URI в байтовый массив.
		byte[] urlData = client.DownloadData("https://learn.microsoft.com/ru-ru/azure/rtos/netx-duo/netx-duo-web-http/chapter1");

		// преобразуем данные в строку
		string data = Encoding.UTF8.GetString(urlData);

		// сохраняем полученные данные в файл
		StreamWriter sw = new StreamWriter("../../HTTP.html", false);
		sw.WriteLine(data);
		
		MessageBox.Show("Файл успешно загружен!");
	}
	catch (Exception ex)
	{
		MessageBox.Show(ex.Message);
	}
	finally
	{
		sw.Close();
	}
});
```