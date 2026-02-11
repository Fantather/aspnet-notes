`.Include()` загружает **первый уровень связанных данных**.
`.ThenInclude()` используется **для вложенных связей** - т.е. чтобы подгрузить связи внутри связи

Если для вложенной связи не указать `ThenInclude`, то класс будет null, а структура будет хранить значения по умолчанию

```C#
var shops = context.Shops
    .Include(s => s.ShopProducts)       // Загружаем коллекцию ShopProducts
        .ThenInclude(sp => sp.Product)  // Для каждого ShopProduct подгружаем Product
    .ToList();
```
- `s => s.ShopProducts` - первый уровень (магазин → список товаров в магазине)
- `sp => sp.Product` - второй уровень (каждый ShopProduct → сам продукт)


Если бы у `Product` было, например, `ProductAddition`, можно добавить ещё один уровень:
```C#
var shops = context.Shops     .Include(s => s.ShopProducts)         .ThenInclude(sp => sp.Product)             .ThenInclude(p => p.Addition)     .ToList();
```

Теперь EF подтянет **магазин → ShopProducts → Product → ProductAddition** за один запрос.