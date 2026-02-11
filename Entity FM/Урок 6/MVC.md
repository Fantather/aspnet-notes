Делим проект на Model, Controller, View, Repository

Model - Модель связанная с БД
Controller - Вся логика
View - Интерфейс
Repositiory - Что бы работать по CRUD операциям с каждой моделью

Если View хочет сменить модель, то оно обращается к Controller, который уже меняет Model


![[Pasted image 20250912203655.png]]


Repository класс должен реализовывать заранее созданный нами интерфейс
С CRUD операциями и тем что нам необходимо

На пример `IBaseRepository<T>`
![[Pasted image 20250912203837.png]]