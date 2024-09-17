 ### Использование статических членов в нестатических классах

>Статическая переменная – это общая переменная для всех экземпляров класса, которая хранится в объекте.

![[Screenshot 2024-08-15 at 12.10.17 PM.png]]

пример со статической переменной:
	допустим я живу с другом Максимом, мы скинулись и купили 10 яблок, лежат в корзине. Я подошел, взял одно, осталось 9 и их может взять как я, так и он, то бишь доступно 9 яблок. Потом Максим взял два, осталось 7, доступных мне и ему.
	Т.е. эта корзина с яблоками является статической переменной, которая является общей для меня и для Максима.


-----

#### Константы 

>Константа не может быть объявлена как static , поскольку по-своему поведению, уже является статической

>Поле const относится к типу, а не к экземплярам типа. Поэтому к полям const можно обращаться с использованием той же нотации **ИмяКласса.ИмяЧлена**, что и в используемой для статических полей.


---

#### Статические методы и поля

>Статические методы не могут обращаться к нестатическим полям. 
>Статические члены не могут быть virtual, abstract, override. 
>Статические члены поддерживают замещение.
![[Screenshot 2024-08-21 at 12.02.22 PM.png]]

Статические поля readonly должны быть инициализированы в конструкторе.

Если класс содержит статические поля, должен быть предоставлен статический конструктор, который инициализирует эти поля при загрузке класса.

>Единственное назначение статических конструкторов - присваивать исходные значения статическим переменным.


-----

>Если класс содержит статические поля, должен быть предоставлен статический конструктор, который инициализирует эти поля при загрузке класса.

```c#
private static field;

public static Property
{
	get { return field; }
	set { field = value; }
}
```


---

AbstractClass
![[Screenshot 2024-08-21 at 11.49.30 AM.png]]

11 строчку можно заменить на 
```c#
AbstractClass instance = new ConcreteClass();
```


---

 ```c#
abstract class BaseClass
{
	public static void StaticMethod()
	{
		Console.WriteLine("BaseClass.StaticMethod");
	}
} 

class DerivedClass : BaseClass
{
	// одноименные методы, чтобы избавиться от warning в StaticMethod
	// нужно поставить оператор new	
	public static new void StaticMethod()
	{
		Console.WriteLine("DerivedClass.StaticMethod");
	}
}
```



### Статические классы


Статический класс в С#, выражает идею паттерна проектирования - Singleton.

Правила:

1. Экземпляр статического класса нельзя создать. 
2. Static class всегда наследуется от Object (Попытка наследования от чего либо другого приводит к ошибке компиляции) .
3. Static class не реализует интерфейсы. Попытка наследования от интерфейса приводит к ошибке уровня компиляции.
4. Содержит только статические члены (наличие в нем нестатического члена приведет к ошибке компиляции).
5. Статический класс не может содержать конструкторов экземпляров.
6. Статический класс закрыт для наследования от него. Попытка наследования от статического класса приводит к ошибке уровня компиляции.

----

Статика ничего не реализует и ничего не наследует 
Статический класс представляет из себя класс одного объекта, одной какой-то сущности 
 
----


### Nested class

1. Внутри вложенного класса мы можем обратиться к закрытому полю через экземляр  
```c#
class MyClass  
{  
    private int field = 1;  
  
    public class Nested  
    {  
        MyClass instance = new MyClass();  
  
        public void Method(int a)  
        {
            instance.field = a;  
            Console.WriteLine($"method from nested --- {instance.field}");  
        }    
    }
}
```


2. обычные классы могут содержать в себе статические классы и наоборот, статические классы могут в себе содержать обычные классы

```c#
public class MyClass1  
{  
    public static class Nested  
    {  
        public static void StaticClass()  
        {      
              Console.WriteLine("1_NestedMethodFromStaticClass");  
        }  
    }
}    
public static class MyClass2  
{  
    public class Nested  
    {  
        public static void DefClass()  
        {
            Console.WriteLine("2_NestedMethodFromDefClass");  
        }
    }
}
```



----

### Расширяющие методы 

Правила:
1. Расширяющие методы могут быть только статическими и создаваться только в статических классах.
```c#
static class ExtensionClass  
{  
    // this - сообщает компилятору, что данный метод является расширяющим.  
    // расширяем тип string 
    public static void ExtentionMethod(this string value)  
    {        
	    Console.WriteLine(value);  
    }
}
```

2. Аргумент расширения всегда должен быть только один и стоять первым в списке аргументов
```c#
static class ExtensionClass  
{  
	// расширяющий тип только один и только первый
    public static void ExtensionMethod(this string value, int number)  
    {        
	    Console.WriteLine(value + number);  
    }
}
```

3. Использование ref / out аргументов static class ExtensionClass  
```c#
static class ExtensionClass
{  
    public static void Add(this int summand1, ref int summand2, out int sum)  
    {       
	     sum = summand1 + summand2;
	     Console.WriteLine("{0} + {1} = {2}", summand1, summand2, sum);  
    }
}  
class Program  
{  
    static void Main()  
    {        
	    int summand1 = 3;  
        int summand2 = 5;  
        int sum = 0;  
        summand1.Add(ref summand2, out sum);  
    }
}
```