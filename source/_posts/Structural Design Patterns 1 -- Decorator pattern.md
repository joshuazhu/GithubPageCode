---
 title: Structural Design Patterns 1 -- Decorator pattern
 date: {date}
 categories: Design Patterns
---

## Definition

>Attaches additional responsibilities to an object dynamically. Decorators provide a flexible __alternative to subclassing__ for __extending functionality__.

## Components

1. __ComponentBase__: base class for all concrete components and decorators.
2. __ConcreteComponent__: inherits from __ComponentBase__. Concrete component classes that may be wrapped by the decorators.
3. __DecoratorBase__: base class for decorators. It has a __constructor__ that accepts a __ComponentsBase__ as its parameters.
4. __ConcreteDecorator__: It may include some additional methods which extend the functionality of components.  

![Decorator pattern uml](https://www.codeproject.com/KB/architecture/438922/Decorator.jpg)

<!-- More -->

## Sample code
```cs
static class Program
{
    static void Main()
    {
        var component = new ConcreteComponent();
        var decorator = new ConcreteDecorator(component);
        decorator.Operation();
    }
}

public abstract class ComponentBase
{
    public abstract void Operation();
}

class ConcreteComponent : ComponentBase
{
    public override void Operation()
    {
        Console.WriteLine("ConcreteComponent.Operation()");
    }
}

public abstract class DecoratorBase : ComponentBase
{
    private readonly ComponentBase _component;

    protected DecoratorBase(ComponentBase component)
    {
        _component = component;
    }

    public override void Operation()
    {
        _component.Operation();
    }
}

public class ConcreteDecorator : DecoratorBase
{
    public ConcreteDecorator(ComponentBase component) : base(component) { }

    public override void Operation()
    {
        base.Operation();
        Console.WriteLine("[ADDITIONAL CODE BLOCK]");
    }
}
```

## Advantages
1. It helps to achieve __extending the components' functionality__ by __avoiding changing components' original logic__.
2. It provides a flexible __alternative to subclassing for extending functionality__, which could change components' behavior at __runtime__.

## Reference
[Design Patterns 2 of 3 - Structural Design Patterns - CodeProject](https://www.codeproject.com/Articles/438922/Design-Patterns-of-Structural-Design-Patterns)

[Head First Design Patterns - O'Reilly Media](https://www.google.com.au/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0ahUKEwjE74WWy7rTAhVEppQKHfqGAjoQFggiMAA&url=http%3A%2F%2Fshop.oreilly.com%2Fproduct%2F9780596007126.do&usg=AFQjCNF91VIwQIeGyXH4xU67GibpAiRKRA&sig2=YcwhV4RTfJRpzWn3xsIcoA)