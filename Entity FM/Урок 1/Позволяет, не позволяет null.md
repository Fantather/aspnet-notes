```C#
public partial class User
{
	public int Id { get; set; }
	public string Name { get; set; } = null!; // Not Null
	public string? Email { get; set; } // Can be Null
}
```

`null!` - Не позволяет присваивать Null значения
string? - Позволяет 
`public int Id` - Entity Framework автоматически сделает поле с такой сигнатурой `PrimaryKey`

Поля должны быть публичными автосвойствами
Имена свойств отобразятся в базе данных, но это настраиваемо