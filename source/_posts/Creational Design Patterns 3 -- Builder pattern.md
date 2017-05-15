---
 title: Creational Design Patterns 3 -- Builder pattern
 date: {date}
 categories: Design Patterns
---

## Definition
Separate the construction of a complex object from its representation so that the same construction process can create different representations, which allows for the step-by-step creation of complex objects using the correct sequence of action.

This pattern is used when a complex object that needs to be created is constructed by __constituent parts__ that must be __created in the same order__ or __by using a specific algorithm__.

## Components
* __Builder__：Interface for creating the actual products. Each step is an abstract method.
* __ConcreteBuilder__：Implementation for builder.
* __Director__：represent class that controls algorithm used for creation of complex object.
* __Product__：complex object that is being built.


![Builder Pattern UML](https://www.codeproject.com/KB/architecture/430590/Builder.jpg)

<!-- More -->

## Code
```cs
public class DirectorCashier
{
    public void BuildFood(Builder builder)
    {
        builder.BuildPart1();
        builder.BuildPart2();
    }
}

public abstract class Builder
{
    public abstract void BuildPart1();

    public abstract void BuildPart2();

    public abstract Product GetProduct();
}

public class ConcreteBuilder1 : Builder
{
    protected Product _product;
    public ConcreteBuilder1()
    {
        _product = new Product();
    }
        
    public override void BuildPart1()
    {
        this._product.Add("Hamburger", "2");
    }
        
    public override void BuildPart2()
    {
        this._product.Add("Drink", "1");
    }

    public override Product GetProduct()
    {
        return this._product;
    }
}

public class Product
{
    public Dictionary<string, string> products = new Dictionary<string, string>();
        
    public void Add(string name, string value)
    {
        products.Add(name, value);
    }
        
    public void ShowToClient()
    {
        foreach (var p in products)
        {
            Console.WriteLine($"{p.Key} {p.Value}");
        }
    }
}

public class BuilderPatternRunner : IPattterRunner
{
    public void RunPattern()
    {
        var Builder1 = new ConcreteBuilder1();
        var Director = new DirectorCashier();

        Director.BuildFood(Builder1);
        Builder1.GetProduct().ShowToClient();
    }
}
```
## Reference
[Design Patterns 1 of 3 - Creational Design Patterns - CodeProject](https://www.codeproject.com/Articles/430590/Design-Patterns-of-Creational-Design-Patterns)

[Factory Patterns - Abstract Factory Pattern - CodeProject](https://www.codeproject.com/Articles/1137307/Factory-Patterns-Abstract-Factory-Pattern)