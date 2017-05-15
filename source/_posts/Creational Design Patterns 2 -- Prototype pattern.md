---
 title: Creational Design Patterns 2 -- Prototype pattern
 date: {date}
 categories: Design Patterns
---

## Definition
Specify the kind of objects to create using a prototypical instance, and create new objects by copying this prototype, which is __used to instantiate a class by copying, or cloning, the properties of an existing object__. 

## When to use it
When objects has a lot of properties which must be filled and the quantity of time you spent on to create this object instance was enormous.
This copy duplicates __all of the properties and fields of original object__. __If a property is reference type, the reference is copied too__.

## Components
* Client：The application object that need the cloned copy of the object
* Prototype：This is an interface or abstract class that defined the method to clone itself, contains a virtual method Clone().
* ConcretePrototype：This is the concrete class that will clone itself.

![Prototype Pattern UML](https://www.codeproject.com/KB/architecture/430590/Prototype.jpg)

<!-- More -->

## Code
```cs
public abstract class IPrototype
{
    public string Id { set; get; }
    public abstract IPrototype Copy();
}

public class ConcretePrototype : IPrototype
{
    public ConcretePrototype(string id)
    {
        Id = id;
    }

    public override IPrototype Copy()
    {
        return (IPrototype) this.MemberwiseClone();
    }
}

//Client class
public class Client
{
    public static void Main()
    {
        var prototype = new ConcretePrototype("1");
        var clone = prototype.Copy();
        Console.WriteLine("Clone: {0}", clone.Id);
        Console.ReadLine();
    }
}
```

## Reference

[Design Patterns 1 of 3 - Creational Design Patterns - CodeProject](https://www.codeproject.com/Articles/430590/Design-Patterns-of-Creational-Design-Patterns)

[Factory Patterns - Abstract Factory Pattern - CodeProject](https://www.codeproject.com/Articles/1137307/Factory-Patterns-Abstract-Factory-Pattern)