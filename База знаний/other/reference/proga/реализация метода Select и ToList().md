- Select - это выборка, он обрабатывает список студентов, он берет студентов и по одному вытаскивает из этого списка и далее выполняет преобразование 
```c#
public static IEnumerable<TOut> Select<TIn, TOut>
	(this IEnumerable<TIn> enumerable, Func<TIn, TOut> selector)
{
	foreach (var e in enumerable)
		yield return selector(e);
}
```
- реализация метода ToList()
```c#
public static List<T> ToList<T>(this IEnumerable<T> enumerable)
{
	var list = new List<T>();
	foreach (var e in enumerable)
		list.Add(e);
	return list;
}
```