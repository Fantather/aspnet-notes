`.Include()` - это метод *Entity Framework Core*, который позволяет загружать связанные данные из других таблиц (навигационные свойства) вместе с основными объектами

По умолчанию EF Core не подгружает связанные таблицы автоматически (это называется _Lazy Loading_, и по умолчанию оно отключено)
`.Include()` делает _Eager Loading_ - т.е. сразу подтягивает данные

Теперь у каждого объекта `Shop` будут **полные связанные данные**, и не придётся делать отдельные запросы к базе.
```C#
public class Shop
{
    public int Id { get; set; }
    public string Name { get; set; }
    public ICollection<ShopProduct> ShopProducts { get; set; }
}

public class ShopProduct
{
    public int ShopId { get; set; }
    public Shop Shop { get; set; }

    public int ProductId { get; set; }
    public Product Product { get; set; }
}



using (var context = new ShopProductDbContext(options))
{
	var shops = context.Shops
	    .Include(s => s.ShopProducts)
	        .ThenInclude(sp => sp.Product)
	    .ToList();
}
```
