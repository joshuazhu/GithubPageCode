---
 title: Creational Design Patterns 4 -- Singleton pattern
 date: {date}
 categories: Design Patterns
---

## Definition
>This pattern ensures a class has only one instance, and provides a global point of access to it.

The base idea of singleton pattern is to centralise management of internal or external resources and to provide a global point to access to this resources.

## Components
* __Singleton__: Contains a static method called _GetSingleton__ which returns the single instance held in __private__ variable.

![Singleton UML](https://www.codeproject.com/KB/architecture/430590/Singleton.jpg)

## Code

~~~cs
public sealed class Singleton
{
    private static volatile Singleton _instance;
    private static readonly object _lockThis = new object();

    private Singleton() {}

    public static Singleton GetSingleton()
    {
        if (_instance == null)
        {
            lock (_lockThis)
            {
                if (_instance == null)
                {
                    _instance = new Singleton();
                }
            }
        }
        return _instance;
    }
}

public class SingletonPatternRunner : IPatterRunner
{
    public void RunPattern()
    {
        var singleton1 = Singleton.GetSingleton();
        var singleton2 = Singleton.GetSingleton();
    }
}
~~~
## Reference
[Design Patterns 1 of 3 - Creational Design Patterns - CodeProject](https://www.codeproject.com/Articles/430590/Design-Patterns-of-Creational-Design-Patterns)

[Factory Patterns - Abstract Factory Pattern - CodeProject](https://www.codeproject.com/Articles/1137307/Factory-Patterns-Abstract-Factory-Pattern)