Подробнее про [[Where]]

**Where**
```C#
using (Context db = new Context())
{
	var phones = db.Phones.Where(p => p.Company.Name == "Apple");
	foreach (Phone phone in phones)
		Console.WriteLine($"{phone.Name} ({phone.Price}) ");
}

using (Context db = new Context())
{
	var phones = (from phone in db.Phones
				  where phone.Company.Name == "Apple"
				  select phone);
	foreach (Phone phone in phones)
		Console.WriteLine($"{phone.Name} ({phone.Price})");
```


**FirstOrDefault**
```C#
using (Context db = new Context())
{
	Phone? myphone = db.Phones.FirstOrDefault(p => p.Id == 3);

	if (myphone is not null)
		Console.WriteLine(myphone.Name);
}
```