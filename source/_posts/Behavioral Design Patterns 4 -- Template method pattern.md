---
 title: Behavioral Design Patterns 4 -- Template method pattern
 date: {date}
 categories: Design Patterns
---

## Definition
Defines the steps of an algorithm and allows subclasses to provide the implementation for one or more steps.

## Components
1. __AbstractClass__: Defines a group of __basic methods__ and __template methods__. __Template Methods__ define the order of executing basic methods. __Basic Methods__ let subclasses to override or implement。
2. __ConcreteClass__：Inheret from Abstract Class that implements the basic methods which defined by Abstract Class.
![Template Method Pattern UML](https://www.codeproject.com/KB/architecture/455228/template_method.jpg)

<!-- More -->
## Code
~~~cs
//Abstract Class
public abstract class AbstractClass
{
    //basic methods
    protected abstract void SimpleMethod1();

    protected abstract void SimpleMethod2();

    //template methods
    public void templateMethod()
    {
        SimpleMethod1();
        SimpleMethod2();
    }
}

//Concrete methods
public class ConcreteClass : AbstractClass
{

    protected override void SimpleMethod1()
    {
        Console.WriteLine("method1");
    }

    protected override void SimpleMethod2()
    {
        Console.WriteLine("method2");
    }

}

//Client class
public class Client
{
    public static void Main(String[] args)
    {
        ConcreteClass class1 = new ConcreteClass();
        class1.templateMethod();
    }
}
~~~

## Advantages
1. Open Close Principle. 
2. The behavior(orders of executing basic methods) is controlled by parent class, subclasses are only for implementation. Easy to extend.

## Reference
[Design Patterns 1 of 3 - Creational Design Patterns - CodeProject](https://www.codeproject.com/Articles/430590/Design-Patterns-of-Creational-Design-Patterns)

[Head First Design Patterns - O'Reilly Media](https://www.google.com.au/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0ahUKEwjE74WWy7rTAhVEppQKHfqGAjoQFggiMAA&url=http%3A%2F%2Fshop.oreilly.com%2Fproduct%2F9780596007126.do&usg=AFQjCNF91VIwQIeGyXH4xU67GibpAiRKRA&sig2=YcwhV4RTfJRpzWn3xsIcoA)