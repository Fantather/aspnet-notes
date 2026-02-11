Что то о том что мы свойство говорим ипользовать одну и то жу область пмяти
![[Pasted image 20250711195911.png]]


Тут в принимающем данные классе нет двух полей, которые есть в отправляющем
Эти поля сохраняются в переменную-словарь помеченную `[JsonExtensionData]`
![[Pasted image 20250711200040.png]]


`[JsonPropertyOrder]` - Позволяет задать порядок считывания полей
![[Pasted image 20250711200550.png]]


`[JsonIngnore(Condition = JsonIgnoreCondition.Never)]` - Поле никогда не игнорируется
![[Pasted image 20250711200756.png]]

Не записывается в JSON, если содержит значение по умолчанию
![[Pasted image 20250711200855.png]]

`IgnoreReadOnlyProperties` - Говорим не записывать в файл `readonly` свойства
![[Pasted image 20250711200727.png]]

Игнорирует поле если оно null
![[Pasted image 20250711201127.png]]