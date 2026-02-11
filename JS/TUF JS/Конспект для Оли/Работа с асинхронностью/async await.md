Ключевое слово `async` используется для того, что бы использовать внутри функции `await` или что бы функция всегда заворачивала результат в `Promise`

Пример раз
```JS
<script>

function fiboSumPromise(n) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			try
			{
				if (n < 0)
				{
					throw new Error("n не може бути від'ємним");
				}
				let a = 0, b = 1;
				let sum = 1;

				for (let i = 2; i <= n; i++)
				{
					let next = a + b;
					sum += next;
					a = b;
					b = next;
				}
				resolve(sum);
			}
			catch (error)
			{
				reject(error);
			}
		}, 1000);
	});
}

async function run() {
	try
	{
		let result = await fiboSumPromise(10);
		console.log("Результат:", result);
	}
	catch (error)
	{
		console.error("Помилка:", error.message);
	}
}

run();

console.log("Код теж не блокується");
</script>
```

Другой пример
![[Pasted image 20251128200021.png]]