Для использования LINQ нужно что бы коллекция реализовала `IEnumerable`
![[Pasted image 20250708190231.png]]
```C#
var Mas2 = from i in Mas
			where i > 4
			where i < 8
			select i;
```

Если нужно просто перебирать элементы, то легковеснее использовать `Enumerable`
Или его можно преобразовать в нужный нам контейнер
``` C#
Enumerable<int> Mas3 = (from i in Mas
						where i > 4 && i < 8
						select i).ToArray();
```

``` C#
int[] Mas3 = (from i in Mas
						where i > 4 && i < 8
						select i).ToArray();
```



```C#
var selecedTeams2 = from t in teams
					where t.ToUpper().StartWith("Б") // Фильтруем содержимое
					orderby t // Сортируем по возрастанию
					select t; // Возвращаем элемент
```
`where` - Фильтрует элементы `teams`
`orderby` - Сортирует по заданному условию
`select` - Возвращает сам элемент, завершающая часть запроса


Методы расширения `LINQ`
```C#
var selecedTeams3 = teams.Where(t => t.ToUpper().StartsWith("Б")).OrderBy(t => t);
```



**Фильтрация**
Выборка пользователей которым меньше 28 лет, владеющих английским
Сортируем сначала по имени, потом по возрасту
Возвращаем не весь объект, а анонимный тип

`descending` - Фильтрация о спаданию
`escending` - Фильтрация по возрастанию, используется по умолчанию
```C#
var selectedUsers2 = from user in users
					 from lang in user.Languages
					 where user.Age < 28
					 where lang == "английский"
					 orderby user.Name descending, user.Age
					 select new { user.Name, user.Age, lang };
```