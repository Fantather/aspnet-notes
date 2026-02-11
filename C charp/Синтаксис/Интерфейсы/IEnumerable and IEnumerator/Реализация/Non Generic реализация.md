**IEnumerable**
```C#
	public class Week : IEnumerable
	{
		string[] days = { "Monday", "Tuesday", "Wednesday", "Thursday", "Friday",
		"Saturday", "Sunday"};
		public IEnumerator GetEnumerator() => days.GetEnumerator();
	} 
```



**IEnumerator**
```C#
using System.Collections;

class WeekEnumerator : IEnumerator
{
	string[] days => this.days = days;// Присваиваем ссылку на переданную коллекцию
	int positon = -1;
	public WeekEnumerator(string[] days) => this.days = days;
	
	public object Current
	{
		get
		{
			if (position == -1 || position >= days.Length)
				throw new ArgumentException();
			return days[position];
		}
	}
	
	public bool MoveNext()
	{
		if(position < days.Length - 1)
		{
			position++;
			return true;
		}
		
		else
			return false;
	}
	
	public void Reset() => position = -1;
}


// Реализация GetEnumerator()
public class Week
{
	string[] days = { "Monday", "Tuesday", "Wednesday", "Thursday", "Friday",
	"Saturday", "Sunday"};
	public IEnumerator GetEnumerator() => new WeekEnumerator(days);
}
```