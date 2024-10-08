Инструкция `lock()` обозначает, что в тот момент, когда в каком-то другом треде выполняются инструкции, никакой другой тред, с `lock()` на тот же объект, не может выполнять свои инструкции

```c#
private static readonly List<int> list = new List<int>();

static void MakeWork()
{
    for (int i = 0; ; i++)
    {
        // если не поставить lock, возникнут странные,
        // плохо повторяемые и, возможно, редкие ошибки
        lock (list)
        {
            list.Add(i);
        }
    }
}

static void MakeWorkNoLock()
{
    for (int i = 0; ; i++) list.Add(i);
}

public static void Correct_syncronization()
{
    new Action(MakeWork).BeginInvoke(null, null);
    var sw = Stopwatch.StartNew();
    while (sw.Elapsed < TimeSpan.FromSeconds(10))
    {
        lock (list)
        {
	        // секция 1
            if (list.Count > 0) list.RemoveAt(0);
        }
    }
}

public static void Main()
{
    new Action(MakeWorkNoLock).BeginInvoke(null, null);
    while(true)
    {
	    lock(list)
	    {
		    // секция 2
		    if(list.Count > 100000) 
			    list.Clear();
			else if(list.Count > 1)
				list.RemoveAt(0);
	    }
    }
}
```

Таким образом будет выполняться целиком критическая секция 1 и целиком критическая секция 2. Они не могут выполняться одновременно, должна полностью завершиться одна, перед тем, как начнет работать другая. 

При разработке приложения - нужно продумать архитектуру, его разбиение на треды так, чтобы минимизировать количество общих ресурсов. Приложение должно разбиваться на довольно большие независимые части и эти части должны передавать информацию через некоторое ограниченное, как можно маленькое количество объектов.
В качестве точки сочленения потоков используется очередь - хорошо подходит по смыслу планирования задач.


Что же делает команда `lock` ?
- Команда `lock` помогает заблокировать доступ к непотокобезопасным объектам так, чтобы другие потоки с `lock` не могли к ним обратиться до тех пор, пока другой `lock` не откроет доступ. 