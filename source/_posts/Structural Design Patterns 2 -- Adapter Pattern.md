---
 title: Structural Design Patterns 2 -- Adapter Pattern
 date: {date}
 categories: Design Patterns
---

## Definition
Defines a manner for creating relationships between objects. This pattern translates one interface for a class into another compatible interface.

It is usually implemented when requirements are changed and we must implement some functionality of classes which interfaces __are not compatible with ours__.

## Components
* __Client__: Represents the class which wants to use the incompatible interface.
* __ITarget__: Interface that is used by client.
* __Adaptee__: This is the functionality which the client desires but its interface is not compatible with the client.
* __Adapter__: This is the class which would implement ITarget and would call the Adaptee code which the client wants to call.
![Adapter Pattern UML](https://www.codeproject.com/KB/architecture/774259/extracted-png-image2.png)

<!-- More -->
## Code
```cs
public interface ITarget
{
    void MethodA();
}

public class Client
{
    private ITarget _iTarget { set; get; }

    public Client(ITarget target)
    {
        _iTarget = target;
    }

    public void Request()
    {
        _iTarget.MethodA();
    }

}

public class Adaptee
{
    public void MethodB()
    {
        Console.WriteLine("Adaptee's MethodB called");
    }
}

public class Adapter : ITarget
{
    Adaptee _adaptee = new Adaptee();

    public void MethodA()
    {
        _adaptee.MethodB();
    }

    public static void Main()
    {
        var adapter = new Adapter();
        var client = new Client(adapter);
        client.Request();
    }
}
```
## Reference
[Design Patterns 2 of 3 - Structural Design Patterns - CodeProject](https://www.codeproject.com/Articles/438922/Design-Patterns-of-Structural-Design-Patterns)
[Head First Design Patterns - O'Reilly Media](https://www.google.com.au/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0ahUKEwjE74WWy7rTAhVEppQKHfqGAjoQFggiMAA&url=http%3A%2F%2Fshop.oreilly.com%2Fproduct%2F9780596007126.do&usg=AFQjCNF91VIwQIeGyXH4xU67GibpAiRKRA&sig2=YcwhV4RTfJRpzWn3xsIcoA)