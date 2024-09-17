
```c#
var result = students
    .Where(z => z.GroupName == "KN-1")
    .Select(z => z.LastName)
    .ToList();
foreach (var e in result)
    Console.WriteLine(e);
```
метод `ToList()` - это нормальный метод, т.е. когда его позвали, тогда он значение и отдает 
```c#
public static class IEnumerableExtentions
    {
        public static List<T> ToList<T>(this IEnumerable<T> enumerable)
        {
            var list = new List<T>();
            foreach (var e in enumerable)
                list.Add(e);
            return list;
        }
        
        public static IEnumerable<T> Where<T>
	        (this IEnumerable<T> enumerable, Func<T, bool> predicate)
        {
            foreach (var e in enumerable)
                if (predicate(e))
                    yield return e;
        }

        public static IEnumerable<TOut> Select<TIn, TOut>
            (this IEnumerable<TIn> enumerable, Func<TIn, TOut> selector)
        {
            foreach (var e in enumerable)
                yield return selector(e);
        }
    }
```

в метод `ToList()` попадают перечисления, которые выданы методом `Select()` => заходим в метод `Where()` => проверяем => протаскивает элемент через `Where()`, потом через `Select()` => складываем в итоговый `list`, в методе `ToList()` 

- Если в конце LINQ выражения у нас не ленивый метод, то сначала будет выполнен он, а потом уже ленивые методы. 
*когда мы подготавливаем коллекцию, мы должны знать на основании чего мы ее готовим, нам нужен тот объект к которому мы будем применять методы `Where()` и `Select()` 

- Если же в LINQ выражениях используется несколько не ленивых методов, то они будут выполняться последовательно, по очереди.

*можно привести сравнение, ленивые методы - это stack - первый пришел, последний ушел, а не ленивые методы - это queue - первый пришел, первый ушел. 

- когда мы вызываем нормальный метод, мы ожидаем немедленного получения из него значения, поэтому мы сразу вызывает этот метод