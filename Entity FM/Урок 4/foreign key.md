Способы создания
- Дать правильное имя свойству `ИмяКлассаId`
```C#
public class Team {}

public class Player
{
	public int id { get; set; }
	public int Teamid { get; set; }
}
```

- Создать навигационное свойство
  Работает быстрее чем хранение ключа, потому что дочерний объект уже часть родительского объекта, не нужно пытаться получить доступ по ключу
```C#
public class Player
{
	public int id { get; set; }
	public Team team { get; set: }
}
```

- Через атрибуты (data notation)
  Есть возможность указать имя для свойства
```C#
public class Player
{
	public int id { get; set; }
	[ForeignKey("TeamInfoKey")]
	public Team team { get; set; }
}
```

- Через FluentAPI
```C#
modelBuilder.Entity<Player>()
    .HasOne(p => p.Team)
    .WithMany(t => t.Players)
    .HasForeignKey(p => p.MyTeamKey);
```