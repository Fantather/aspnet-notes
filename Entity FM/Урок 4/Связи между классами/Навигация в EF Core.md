```C#
public class Team  ///1:N в одній команді багао гравців
{
	public int Id { get; set; }
	public string Name { get; set; }

	///Навігаційна властивість - колекція гравців  на С#
	public List<Player> Players { get; set; }
}

public class Player
{
	[Key]
	[ForeignKey("UserId")]
	public int Id { get; set; }
	public string Name { get; set; }

	//// Створюємо foreign key з назвою класс TeamId
	//// автоматически создаст foreign key  відобразиться в базі
   public int TeamId { get; set; }

   //[ForeignKey("TeamId")] //// data anotation -- atributes
	public Team Team { get; set; }
}

public class User
{
	[Key]
	[ForeignKey("PlayerId")]
	public int Id { get; set; }
	public string Name { get; set; }
	public string SecondName { get; set; }
}
```



**Навигационные свойства**
Они нужны в C# для удобной работы с объектами и связями
И что бы для работы с самим объектом не приходилось отдельным запросом вытягивать его из таблицы 
```C#
public Team team { get; set; }
```


**Свойство для Внешнего Ключа**
Это обычное свойство которое хранит идентификатор связанной сущности в БД
Нужно что бы
- EF мог правильно выстроить связь в БД
```C#
public int TeamId { get; set; }
```


Enity Framework умеет работать только через Навигационные свойства или только через Внешние ключи, но рекомендуется использовать и то и другое, что бы получать преимущества обоих подходов