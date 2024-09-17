
### Interface

- интерфейсы с одноименными методами
```csharp
class DerivedClass : Interface1, Interface2
{
	// одноименные методы являются private
	// явное указание модификаторов доступа - невозможно
	
	// если есть одноименные интерфесы и нам нужно реализовать
	// конкретный метод, конкретного интерфейса, то нужно
	// использовать технику явного указания и потом
	// кастимся к конкретному интерфейсу
	void Interface1.Method()
	{
	    Console.WriteLine("interface_1");
	}

	void Interface2.Method()
	{
	    Console.WriteLine("interface_2");
	}
}
```

```csharp
class Program
{
	static void Main()
	{
		DerivedClass instance = new DerivedClass(); // каст к нужному интерфейсу
    Interface1 instance1 = instance as Interface1;
    instance1.Method();

    Interface2 instance2 = instance as Interface2;
    instance2.Method();

    Console.ReadKey();
	}
}
```

- одноименные методы
```c#
interface Interface1
{
    void Method1();
}

interface Interface2
{
    void Method1();
}
```

- множественное наследование

```c#
// множественное наследование допустимо только для интерфейсов
// множественное наследование от двух и более классов или структур - недопустимо
// доступно только множественное наследование от одного класса и многих интерфейсов
class DerivedClass : BaseClass, /*TestClass - добавить еще базовый класс - нельзя*/ Interface1, Interface2
{
    public void Method1()
    {
        Console.WriteLine("interface_1, method_1()");
    }

    public void Method2()
    {
        Console.WriteLine("interface_2, method_2()");
    }
}
```

```c#
class BaseClass
{
    public void Method()
    {
        Console.WriteLine("BaseClass Method()");
    }
}
```

```c#
static void Main()
{
	DerivedClass instance = new DerivedClass();
	instance.Method();
	instance.Method1();
	instance.Method2();
	Console.WriteLine(new string('-', 15));
	
	BaseClass instance1 = instance as BaseClass;
	instance1.Method();
	
	Interface1 instance2 = instance as Interface1;
	instance2.Method1();
	
	Interface2 instance3 = instance as Interface2;
	instance3.Method2();
	
	Console.ReadKey();
 }
```

- наследование интефейсов от интерфейсов
```c#
namespace Interface
{
	// наследование интефейсов от интерфейсов 
	interface IInterface1
	{
	void Method1();
	}
	
	interface IInterface2 : IInterface1
	{
	    void Method2();
	}
	
	class ConcreteClass : IInterface2
	{
	    public void Method1()
	    {
	        Console.WriteLine("interface1 method1");
	    }
	
	    public void Method2()
	    {
	        Console.WriteLine("interface2 method2");
	    }
	}

	class Program
	{
        static void Main()
        {
            ConcreteClass instance = new ConcreteClass();
            instance.Method1();
            instance.Method2();

            Console.WriteLine(new string('-', 15));
            
            IInterface1 instance1 = instance as IInterface1;
            instance1.Method1();

            Console.WriteLine(new string('-', 15));

            IInterface2 instance2 = instance as IInterface2;
            instance2.Method2();
            instance2.Method1();
        }
	}
}
```

- наследование интерфейса от интерфейса, у которых совпадают имена членов
```c#
interface IInterface1
{
    void Method();
}

interface IInterface2 : IInterface1
{
    // без new - ошибки не будет, но будет предупреждение компилятора
    // модификатор new - подсказка, что у нас идет замещение этого метода
    // можно и не указывать, но помогает узнать, что где-то есть одноименный метод
    // т.е. где-то в иерархии наследования есть одноименный метод
    new void Method();
}

class ConcreteClass : IInterface2
{
    void IInterface1.Method()
    {
        Console.WriteLine("interface1 method1");
    }

    void IInterface2.Method()
    {
        Console.WriteLine("interface2 method2");
    }
}

class Program
{
    static void Main()
    {
        ConcreteClass instance = new ConcreteClass();

        IInterface1 instance1 = instance as IInterface1;
        instance1.Method();

        IInterface2 instance2 = instance as IInterface2;
        instance2.Method();
    }
}
```

- Наследование интерфейса от интерфейса, у которых совпадают имена членов Объединение реализации одноименных абстрактных членов.
```csharp
// при касте к любому из интерфейсов - реализация будет одинаковой
interface IInterface1
{
    void Method();
}

interface IInterface2
{
    void Method();
}

class ConcreteClass : IInterface2, IInterface1
{
    public void Method()
    {
        Console.WriteLine("interface1 and interface2 methods 1-2");
    }
}

```

- наследование абстракных классов от интерфейсов
```c#
namespace Interface
{
	// наследование абстракных классов от интерфейсов
	interface IInterface
	{
		void Method();
	}
	abstract class AbstractClass : IInterface
	{
	    // замещение абстрактного метода из интерфейса, в абстрактном классе обязательно
	    public abstract void Method();
	}
	
	class ConcreteClass : AbstractClass
	{
	    // реализация абстрактного метода из абстрактного класса, в конкретном классе обязательна
	    public override void Method()
	    {
	        Console.WriteLine("Метод, реализация интерфейса в абстрактном методе");
	    }
	}
}
```


----

### Array

Массив – именованный набор однотипных переменных, расположенных в памяти непосредственно друг за другом, доступ к которым осуществляется по индексу.

```c# 
int[] array = new int[3];
```
![[Screenshot 2024-08-14 at 11.33.42 AM.png]]

Типы массива являются ==ссылочными типами==, производными от абстрактного базового класса Array

- Индекс массива – целое число, либо значение типа, приводимого к целому, указывающее на конкретный элемент массива.![[Screenshot 2024-08-14 at 11.34.28 AM.png]]

- Одномерный массив – массив, содержащий один индекс.![[Screenshot 2024-08-14 at 11.35.01 AM.png]]

- Многомерные массивы – массивы имеющие более одного индекса![[Screenshot 2024-08-14 at 11.38.21 AM.png]]

- Двумерный массив – прямоугольный массив, содержащий два индекса.
```c#
int[,] array = new int[3,3];
```
	![[Screenshot 2024-08-14 at 11.38.54 AM.png]]

- Массивы в C# ковариантные, но не контравариантные
	Ковариантность – это неявный UpCast всех элементов массива. Контравариантность – это неявный DownCast всех элементов массива.
	
	Массивы элементов ссылочных типов ковариантные но, не контравариантные.
	Массивы элементов структурных типов не ковариантные и не контравариантные.

- Ключевое слово ==params== позволяет задать параметр метода, принимающий переменное количество аргументов. Всегда должно стоять последним аргументом 
	Метод может принимать ==только один== params-аргумент, и он должен быть последним в списке аргументов. 
```c#
class Program
{
	public static void Method(params int[] items)
	{
		for (int i = 0; i < items.Length; i++)
			{
				Console.Write(items[i] + "\t");
			}
	}

	static void Main()
	{
		Method(12, 24, 23, 123, 53, 64, 324);
	}
}
```


-----

###  Инденксаторы 

Индексаторы позволяют индексировать экземпляры класса или структуры так же, как массивы. Индексаторы напоминают свойства, но их методы доступа принимают параметры.
	
Метод set автоматически срабатывает тогда, когда свойству пытаются присвоить значение. Это значение представлено ключевым словом value.
	
Метод get автоматически срабатывает тогда, когда мы пытаемся получить значение.
	
Нужны для создания своих классов коллекций или классов массивов, чтобы в дальнейшем мы могли записывать/извлекать элементы.

```c#
class Program
{
	int[] array = new int[3]
	{
		1,2,3
	};
	// индексатор этого массива
	// ключевое слово this показывает, что это индексатор 
	public int this[/*тип нашего индекса*/int index]
	{
		get { return array[index]; }
		
		set { array[index] = value; }
	}
}
```

