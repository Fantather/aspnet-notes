Рассмотрим этот код
```C#
private void button1_Click(object sender, EventArgs e)
{
    MyTaskAsync();
    MessageBox.Show($"Продолжаю работу в потоке {Thread.CurrentThread.ManagedThreadId}");
}

public static async void MyTaskAsync()
{
    Task<string> task2 = Task.Run(() =>
    {
        Thread.Sleep(2000);
        return "Привет из";
    });
	
    string result = await task2;
    
    // Этот код выполнится ПОСЛЕ того, как 'task' завершится
    MessageBox.Show($"{result} потока {Thread.CurrentThread.ManagedThreadId}");
}
```

`private void button1_Click` - обыкновенный обработчик кнопки

`MyTaskAsync();` - Это асинхронный метод
- Компилятор зайдёт в метод
- Создаст объект *Task*, который возвращает строку и сразу начнёт его выполнение из-за `Task.Run`
- Встретит `await`, мгновенно вернёт объект *Task* с ещё не завершённой задачей и метод начнёт ожидать её завершения
- Пока `task2` выполняется управление будет возвращено в UI-поток, который сможет продолжить свою работу
- Как только `task2` завершится, остальная часть метода продолжит своё выполнение, в ThreadPool или UI-потоке, опционально
  
То есть на практике я сначала увижу сообщение *Продолжаю работу в потоке 1*
А потом мне выведется сообщение *Привет из потока 1*, потому что выполнение после `await` продолжилось в UI-потоке