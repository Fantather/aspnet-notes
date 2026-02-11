`.Load()` - загружает данные в контекст (`DbContext`), ничего не возвращает

`.Local` - возвращает все объекты, которые были загружены в Контекст
```C#
// Загружаем все продукты в контекст
context.Products.Load();

// Теперь они доступны в памяти через DbSet.Local
var products = context.Products.Local.ToObservableCollection();
```


С фильтром
```C#
context.Products
    .Where(p => p.Price > 10000)
    .Load();

// В памяти окажутся только дорогие продукты
var expensive = context.Products.Local;
```

Загружаем `Players` в свойство `team`
```C#
// Explicite loading (явная загрузка)
using (EntityDatabase db = new EntityDatabase())
{
    var team = db.Teams.FirstOrDefault();///1 team
    db.Players.Where(p => p.Team.Id == team.Id).Load();
    
	Console.WriteLine($"Team: {team?.Name}");
    foreach (var p in team.Players)
        Console.WriteLine($"Player: {p.Name}");
}
```

### Когда использовать

- Когда нужно “приклеить” коллекцию к UI и обновлять её автоматически.
    
- Когда удобно работать с `.Local` как с in-memory списком.
    
- Когда не нужен немедленный результат (как с `ToList()`), а важнее, чтобы данные были загружены в `DbContext`