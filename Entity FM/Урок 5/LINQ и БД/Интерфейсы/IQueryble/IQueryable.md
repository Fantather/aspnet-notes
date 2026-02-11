`IQueryable<T>` - это интерфейс из пространства имён System.Linq, который позволяет строить запросы к источнику данных (например, к базе данных) *лениво*, то есть запрос не выполняется, пока мы явно не попросим данные (`ToList()`, `ToArray()`, `First()` и т.д.)
- `IQueryable` не содержит данных сам по себе, это *описание запроса*
- Запрос может быть переведен в SQL, если источник - база данных (например, Entity Framework)


```C#
using (Context db = new Context())
{
    int id = 1;
    IQueryable<Phone> phoneIQuer = db.Phones; // создаётся запрос, который ещё не выполнен
    var phones = phoneIQuer.Where(p => p.Id > id).ToList(); // фильтр выполняется уже на сервере (в SQL)

    foreach (var item in phones)
        Console.WriteLine(item.Name);
}
```
- `IQueryable` создаёт *ленивый запрос* к базе
- `Where` пока не выполняется физически, SQL запрос составляется, но выполняется только при вызове `ToList()`
- Фильтрация идёт уже на уровне SQL сервера, поэтому загружаются только нужные записи


Постепенное добавление условий
```C#
using (Context db = new Context())
{
    IQueryable<Phone> phoneIQuer = db.Phones;
    phoneIQuer = phoneIQuer.Where(p => p.CompanyId == 1); // добавляем условие по компании
    phoneIQuer = phoneIQuer.Where(p => p.Price < 50000); // добавляем условие по цене
    var phones = phoneIQuer.ToList(); // выполнение запроса на сервере
}
```
- Можно пошагово строить запрос, добавляя условия через `Where`
- До вызова `ToList()` запрос *не выполняется*, это называется *ленивым выполнением* (deferred execution)
- Результатом будет только список телефонов компании с `Id == 1` и ценой меньше 50 000, полученный с сервера


Пример с `IEnumerable`
```C#
using (Context db = new Context())
{
    int id = 1;
    IEnumerable<Phone> phoneIEnum = db.Phones; // Все данные загружаются из базы в память (отсутствует ленивое выполнение)
    var phones = phoneIEnum.Where(p => p.Id > id).ToList(); // фильтрация выполняется уже на клиентской стороне

    foreach (var item in phones)
        Console.WriteLine(item.Name);
}
```
- `IEnumerable` используется для работы с коллекцией в памяти.
- `db.Phones` сразу превращается в `IEnumerable`, поэтому *все записи из таблицы загружаются в память*, а фильтр `Where` применяется уже после этого.
- Если таблица большая, это неэффективно, потому что загружается всё, а фильтруются только нужные записи.