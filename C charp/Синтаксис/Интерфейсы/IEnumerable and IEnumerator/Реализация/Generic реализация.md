**Краткая реализация generic версии GetEnumerator**
```C#
public class MyCollection<T> : IEnumerable<T>
{
	private readonly List<T> _items = new List<T>(); // 1
	public void Add(T item) => _items.Add(item);     // 2
	
	public IEnumerator<T> GetEnumerator()
	{
		foreach (var item in _items)
			yield return item;                        // 3
	}
	
	IEnumerable IEnumerable.GetEnumerator() => return GetEnumerator(); // 4
}
```

1. Ключевое слово `private` у `_items` не позволяет менять его снаружи, оставляя для взаимодействия только методы `Add` и реализацию `IEnumerable<T>`

2. Ключевое слово `readonly` у `_items` не позволяет перенаправить переменную на другой список, но оставляет возможность менять содержимое списка

3. Напоминаю, что `IEnumerable` используется для возможности хождения по коллекции и совместимости с `foreach`
   При полной реализации нам бы пришлось писать класс-перечислитель, который бы хранил текущую позицию элемента, с методами `Current`, `MoveNext`, `Reset`, возможно `Dispose`
   `yield return` в свою очередь создаст такой класс  за нас, автоматически
   Запись получается понятной и короткой

4. `IEnumerable.GetEnumerator()` - это приватная, явная реализация для `Non generic` метода `GetEnumerator`. Она не нужна для работы нашего класса, но `generic` версия интерфейса `IEnumerable` наследуется от `Non generic` версии, так что требует реализации этого метода. Сделать метод публичным запретит компилятор



**Полная реализация generic версии GetEnumerator**
```C#
public class MyCollection<T> : IEnumerable<T>
{
	private readonly List<T> _items = new List<T>();
	public void Add(T item) => _items.Add(item);
	
	public IEnumerator<T> GetEnumerator() => new NaturalEnumerator(this);
	IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
	
	private class NaturalEnumerator : IEnumerator<T>
	{
		private readonly MyCollection<T> _parent; // Ссылка на родительский класс
		private int _index = -1;
		
		public NaturalEnumerator(MyCollection<T> parent) => _parent = parent;
		
		public T Current
		{
			get
			{
				if(_index < 0 || _index >= _parent._items.Count)
				throw new InvalidOperationException();
				
				return _parent._items[_index];
			}
		}
		object IEnumerator.Current => Current;
		
		public bool MoveNext()
		{
			_index++;
			return _index < _parent._items.Count;
		}
		
		public void Reset()
		{
			_index = -1;
		}
		
		public void Dispose()
		{
			// No resources to release
			// Обычно оставляем пустым
			// Если нечего освобождать, то об этом нужно написать
		}
	}
}
```

1. `GetEnumerator()` тут создаёт объект `NaturalEnumerator`, который будет выполнять логику перебора
2. `NaturalEnumerator()` - Это наш собственный `IEnumerator` который хранит в себе ссылку на нашу коллекцию и в котором мы обязаны реализовать свойство `Current` и методы `MoveNext`, `Reset`, `Dispose`
   - Свойство `Current { get; }` - Возвращает текущий элемент типа `T`, и `Generic` и `Non generic` версии требуют перегрузки
   - `MoveNext` - Смещает `Current` и возвращает `false`, при выходе за границы массива
   - `Reset` - сбрасывает счётчик в начальное состояние (обычно в `-1`), чтобы перебор можно было начать заново
   - `Dispose` - освобождение ресурсов, потому что `IEnumerator<T>` наследуется от `IDisposable`, в простых перечислителях обычно остаётся пустым

3. Поле `_parent` получает ссылку на экземпляр родительского класса `MyCollection<T>`. К нему применяется `private readonly`, чтобы внутри перечислителя нельзя было переназначить это поле после конструктора - но саму коллекцию через `IEnumerator<T>` изменить нельзя, потому что интерфейс не содержит методов модификации.
4. Поле `_index` начинается с -1, чтобы до первого вызова `MoveNext()` перечислитель находился "до" первого элемента. При первом же вызове `MoveNext()` счётчик смещается на `0` и возвращается первый элемент