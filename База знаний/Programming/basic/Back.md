1. тип вложенности, вверх видим, вниз нет (про приватные поля)
```c#
class MyClass  
{  
    private int field = 1;  
    public class Nested  
    {  
    // внутри вложенного класа мы можем обраться к закрытому полю через экземляр 
        MyClass instance = new MyClass();  
        public void Method(int a)  
        {            
	        instance.field = a;  
            Console.WriteLine($"method from nested --- {instance.field}");  
        }    
    }
}
```

2. 