если есть задача просто асинхронного вызова, можно воспользоваться BeginInvoke / EndInvoke, для того, чтобы код стал более удобным.

```c#
static double MakeWork(int number)
{
	double a = 1;
	for (int i = 0; i < 1000000; i++)
		for (int j = 0; i < 100; j++)
			a /= 0.01;
	Console.WriteLine(number);
	return a;
}

public static void Main()
{
	var func = new Func<int, double>(MakeWork);
	var result = func.BeginInvoke(1, null, null);
	while (!result.IsCompleted)
	Console.WriteLine(".");
	var resultedValue = func.EndInvoke(result);
}
```

когда тред доходит до строчки с командой `EndInvoke` - тред ждет, пока другой поток не закончит свою работу и не вернет результат.