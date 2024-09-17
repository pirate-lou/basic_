- Метод `Where()` - фильтрует коллекцию по заданному сравненителю.
- Метод `Select()` - применяет к элементу коллекции переданную функцию и возвращает её результат. В итоге получается преобразование элемента коллекции в новый элемент. 

*Реализация методов*
```c#
public static class IEnumerableExtentions
    {
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

        public static List<T> ToList<T>(this IEnumerable<T> enumerable)
        {
            var list = new List<T>();
            foreach (var e in enumerable)
                list.Add(e);
            return list;
        }
    }
```

```c# 
public static void Main()
     {
         var students = new List<Student>
         {
             new Student { LastName="Jones", GroupName="FT-1" },
             new Student { LastName="Adams", GroupName="FT-1" },
             new Student { LastName="Williams", GroupName="KN-1"},
             new Student { LastName="Brown", GroupName="KN-1"}
         };

         var result = students
             .Where(z => z.GroupName == "KN-1")
             .Select(z => z.LastName);
         foreach (var e in result)
             Console.WriteLine(e);
     }
```

- Методы Where и Select - это ленивые методы, потому что в них используется `yield return` и __соответственно их выполнение начинается не тогда когда выполнение дошло до строчек `.Where(...)`  и`.Select(...)`, а когда по результату побежали `foreach`__
Сначала входи в метод `Select`, несмотря на то, что метод `Where` написан перед ним. 
*обычные методы вызываются в том порядке, в котором они идут в точной записи `Math.Sin(1).ToString()`*
	У нас же ленивые методы, поэтому они вызываются в другом порядке.
		Начинаем бежать `foreach` по переменной `result` - эта переменная соответствует перечислению, которая получена с помощью метода `Select()`

вся эта запись
```c#
var result = students
             .Where(z => z.GroupName == "KN-1")
             .Select(z => z.LastName)
             .ToList();
```
соответствует этой
```c#
var r = IEnumerableExtentions.Select(IEnumerableExtentions
	.Where(students, z => z.GroupName == "KN-1"), z => z.LastName);
```
из-за того, что Select - это extention метод, можно передавать первый аргумент не так `.Where(students, z => z.GroupName == "KN-1")`, а вызывать его вот так `.Where(z => z.GroupName == "KN-1")`
- тот объект, который порожден методом `Where()`, то перечисление, которое возвращает `Where()` - когда мы вызывает метод `Select()` из него, реально, это перечисление передается методом `Select()` первым элементом  

==посмотреть extention методы==

бежим по перечислению, по нормальному, не ленивому, в методе `Where()` , получает очередной элемент из списка и в этот момент запускается  `predicate(e)`  - это лямбда-выражение `Where(z => z.GroupName == "KN-1")`
Далее, когда мы получили нужный элемент, мы переходим в метод `Select()`, потом вызываем эту лямбду  `.Select(z => z.LastName)`  и получаем соответствующую строчку, и эту строчку мы возвращаем как очередной элемент перечисления возвращаемого методом `Select()` и попадает в этот `foreach` и выводится на консоль 
```c#
foreach (var e in result)
             Console.WriteLine(e);
```

Запрос элемента из метода `Select()`=> восстанавливаем метод, тк это метод с конструкцией `yield return`, восстановим в том месте где мы его прервали, в этом месте запрашиваем очередной элемент из метода `Where()` => возьмем очередной элемент из списка, проверим => выдадим (или не выдадим и будем искать до посинения) => используем его внутри метода `Select()` => передадим в `selector` (в методе `Main()`) => и вернем из метода `Select()`  

когда не осталось элементов:
запрос элемента => запросим элемент из метода `Select()` =>  запросим элемент из метода`Where`, но элементов нет => выходим из `foreach` => дает сигнал `foreach` из метода `Select()` , что последовательность закончилась, потому что метод `Where()` закончился => выход из `foreach`

==Из-за того, что методы ленивые, они начинают выполняться задом наперед

Ленивые коллекции - пораждают элементы по мере их требований, если мы не требуем их, то мы и не заходим в соответствующие методы.