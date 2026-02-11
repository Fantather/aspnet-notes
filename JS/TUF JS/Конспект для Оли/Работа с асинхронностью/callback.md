Метод, который передаётся в другую функцию, что бы она вызвалась когда результат будет готов

Это устаревший метод работы с асинхронностью
Сейчас используют fetch и promise

Первый параметр у callback это исключение, второй это результат работы метода

Если выполнение было успешно, то вместо исключения передаётся null
Если не успешно, то вместо результата передаётся null

```JS
<script>

// Callback — це функція, яка передається в іншу функцію, щоб вона викликала її коли результат буде готовий.

function asyncFiboSum(n, callback) {
	setTimeout(() => {
		try {
			if (n < 0) {
				throw new Error("n не може бути від'ємним");
			}
			let a = 0, b = 1;
			let sum = 1;
			
			for (let i = 2; i <= n; i++) {
				let next = a + b;
				sum += next;
				a = b;
				b = next;
			}
			callback(null, sum); // (помилки немає, повертаємо результат)
		}
		catch (error) {
			callback(error, null); // (сталася помилка)
		}
	}, 1000);
}


asyncFiboSum(10, (error, result) => {
	if (error)
	{
		console.error("Сталася помилка:", error.message);
		return;
	}

	console.log("Сума перших 10 чисел Фібоначчі:", result);
});

console.log("Код після asyncFiboSum");
</script>
```