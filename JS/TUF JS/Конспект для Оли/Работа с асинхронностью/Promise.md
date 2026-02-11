Объект, который обещает вернуть результат задачи в будущем

Возвращает или `resolve` для результата или `reject` для исключений

Можно назначить ему continue-метод с помощью `.then`

```JS
<script>

function fiboSumPromise(n) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			try {
				if (n < 0)
				{
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
				resolve(sum);
			}
			catch (error) {
				reject(error);
			}
		}, 1000);
	});
}



fiboSumPromise(10)
	.then(result => {
		console.log("Результат:", result);
	})
	.catch(error => {
		console.error("Помилка:", error.message);
	});



console.log("Код не блокується");

</script>
```