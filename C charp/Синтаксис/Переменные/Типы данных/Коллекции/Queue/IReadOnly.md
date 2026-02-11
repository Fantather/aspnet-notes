Вот более «полный» вариант с `IReadOnlyCollection<Visitor>`, чтобы было безопасно и можно узнать количество элементов:

```csharp
using System.Collections.Generic;

namespace Generics_HW.Task_3
{
    internal class CafeQueue
    {
        private Queue<Visitor> queue = new Queue<Visitor>();

        // Публичное свойство только для чтения
        public IReadOnlyCollection<Visitor> Queue => queue;

        // Методы для безопасного изменения очереди
        public void Enqueue(Visitor visitor) => queue.Enqueue(visitor);

        public Visitor Dequeue()
        {
            return queue.Count > 0 ? queue.Dequeue() : null;
        }

        public Visitor Peek()
        {
            return queue.Count > 0 ? queue.Peek() : null;
        }
    }
}
```

✅ Что здесь важно:

1. Извне нельзя сделать `Queue.Enqueue(...)` или `Queue.Dequeue()`.
    
2. Можно безопасно пройтись по всем элементам через `foreach`:
    

```csharp
foreach (var visitor in cafeQueue.Queue)
{
    Console.WriteLine(visitor.Name);
}
```

3. Через `Queue.Count` доступно количество элементов, без возможности их изменить напрямую.
    

Если хочешь, я могу нарисовать **мини-схему**, как очередь и публичное свойство «ограждают» доступ к внутренним данным. Это очень помогает визуально понять инкапсуляцию.

Хочешь такую схему?