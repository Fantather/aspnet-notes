```C#
await Task.Run( () =>
{ 
	try
	{
		// WebRequest — базовый класс abstract модели "запрос-ответ" платформы .NET Framework для доступа к данным из Интернета.
		// Выполняет запрос к универсальному коду ресурса (URI). 
		// HttpWebRequest предоставляет ориентированную на HTTP-протокол реализацию класса WebRequest.
		string query = "https://learn.microsoft.com/ru-ru/dotnet/api/system.net.httprequestheader?view=netframework-4.8";

		// Create инициализирует новый экземпляр HttpWebRequest для заданной схемы URI.
		HttpWebRequest request = (HttpWebRequest)HttpWebRequest.Create(query /* URI, определяющий интернет-ресурс */);

		// GetResponse возвращает ответ от интернет-ресурса.
		HttpWebResponse response = (HttpWebResponse)request.GetResponse();////блокуючий

		// GetResponseStream возвращает поток, используемый для чтения основного текста ответа с сервера
		StreamReader sr = new StreamReader(response.GetResponseStream(), Encoding.UTF8);

		// считываем данные из потока в строку
		string data = sr.ReadToEnd();

		// сохраняем полученные данные в файл
		StreamWriter sw = new StreamWriter("../../MicrosoftLearn.html", false);
		sw.WriteLine(data);
		MessageBox.Show("Файл успешно загружен с сервера: " + response.Server);
	}
	catch (Exception ex)
	{
		MessageBox.Show(ex.Message);
	}
	finally
	{
		sw.Close();
		response.Close();
		sr.Close();
	}
});
```