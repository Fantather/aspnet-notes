Попадает в тот же поток, что и метод к которому он привязан
Метод который автоматически сработает после того, как сработает метод, который мы отправили в `ThreadPool`



![[Pasted image 20250926194708.png]]
**Создание**
Теперь метод `SomeMethodInThreadCompleted` выполнится сразу после `SomeMethodInThread`, в том же Пулле потоков
```C#
MyDelegate d = SomeMethodInThread;
AsyncCallback ac = SomeMethodInThreadCompleted;
d.BeginInvoke(1, ac, d);
// Зачем я передаю сюда сам делегат?
```

`AsyncState` - для данных 