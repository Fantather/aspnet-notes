```C#
public class ExampleWithCallback
{
	// Метод, который будет выполняться асинхронно
	public static int LongRunningCalculation(int a, int b)
	{
		return a * b;
	}
	
	
	
	// Callback-метод
	public static int CalculationCompleted(IAsyncResult asyncResult)
	{
		// Получаем делегат из AsyncState
		Func<int, int, int> delegateInstance = (Func<int, int, int>)asyncResult.AsyncState;
		
		
		try
		{
			int result = delegateInstance.EndInvoke(asyncResult);
		}
		
		catch (Exception ex)
		{
			Console.Writeline($"Error in async operation: {ex.Message}");
		}
	}
}


public static void Main(string[] args)
{
	Func<int, int, int> calculator = LongRunningCalculation;
	
	
	IAsyncResult asyncResult = calculator.BeginInvoke(
	10, 15,
	new AsyncCallback(CalculationCompleted),
	calculator);
	
	
}
```