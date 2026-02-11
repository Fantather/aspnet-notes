Если разные потоки используют один блок кода - этому блока кода нужно написать `lock`
```C#
SynchronizationContext ctx;
lock (ctx)
{
	
}
```