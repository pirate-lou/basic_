- Cуть метода - это фильтрация коллекции. Берем коллекцию и превращаем ее в коллекцию поменьше, включая в результирующую коллекцию только те элементы, которые удовлетворяют некоторым условия.
	- коллекция или перечислимый тип - это все, что реализует `IEnumerable`, т.е. все, почему можно пробежаться `foreach`  
	- в реализации сошлось: 
		- дженерик методы
		- extention методы
		- лямбда-выражения
		- делегатные типы
		- yield return 
```c#
public static class IEnumerableExtensions
{
	public static IEnumerable<T> Where<T>(this IEnumerable<T> enumerable,
		Func<T, bool> predicate)
	{
		foreach (var e in enumerable)
			if (predicate(e))
				yield return e;
	}
}
```