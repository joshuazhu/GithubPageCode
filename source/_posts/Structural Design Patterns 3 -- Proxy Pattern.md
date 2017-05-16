---
 title: Structural Design Patterns 3 -- Proxy Pattern
 date: {date}
 categories: Design Patterns
---

## Definition

>Provide a surrogate or placeholder for another object to control access to it. This pattern can be used when we don't want to access the resource or subject directly.

## Components

* __Subject__: Provides an interface that both __actual__ and __proxy__ class will implement. In this way the proxy __can easily be used as a substitute for real subject__.
* __Proxy__: This class will be __used by the applications__ and will expose the methods exposed by the Subject.
* __RealSubject__: Real object that contains the actual logic to retrieve the data/functionality, which __represented by the proxy__.

![Proxy Pattern UML](http://upload-images.jianshu.io/upload_images/3131810-508eba3a450b3cd1.gif?imageMogr2/auto-orient/strip)

<!-- More -->

## Code

~~~cs
public interface ISubject
{
    double Add(double x, double y);
    double Sub(double x, double y);
    double Mul(double x, double y);
    double Div(double x, double y);
}

public class Math : ISubject
{
    public double Add(double x, double y) { return x + y; }
    public double Sub(double x, double y) { return x - y; }
    public double Mul(double x, double y) { return x * y; }
    public double Div(double x, double y) { return x / y; }
}

public class MathProxy : ISubject
{
    private Math _math;

    public MathProxy()
    {
        _math = new Math();
    }

    public double Add(double x, double y)
    {
        return _math.Add(x, y);
    }
    public double Sub(double x, double y)
    {
        return _math.Sub(x, y);
    }
    public double Mul(double x, double y)
    {
        return _math.Mul(x, y);
    }
    public double Div(double x, double y)
    {
        return _math.Div(x, y);
    }
}

public class ProxyPatternRunner : IPatterRunner
{
    public void RunPattern()
    {
        var proxy = new MathProxy();

        Console.WriteLine("4 + 2 = " + proxy.Add(4, 2));
        Console.WriteLine("4 - 2 = " + proxy.Sub(4, 2));
        Console.WriteLine("4 * 2 = " + proxy.Mul(4, 2));
        Console.WriteLine("4 / 2 = " + proxy.Div(4, 2));

        Console.ReadKey();
    }
}
~~~

## Proxy Types

* __Remote proxies__: Representing the object located remotely.

* __Virtual proxies__: These proxies will provide some default behaviors if the real objects needs some time to perform these behaviors. Once the real object is done, these proxies push the actual data to the client.

* __Protection proxies__: When an application does not have access to some resource then these proxies will talk to the objects.

## Reference

[Understanding and Implementing Proxy Pattern in C# - CodeProject](https://www.google.com.au/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0ahUKEwitscX1m_TTAhXLFJQKHe6MD7YQFggyMAE&url=https%3A%2F%2Fwww.codeproject.com%2FArticles%2F492594%2FUnderstanding-and-Implementing-Proxy-Pattern-in-Cs&usg=AFQjCNEuPk8k1A4RZH8tfkzUmIiQwdMaeQ&sig2=poTBB6exI7ZeW2X-WmDziQ)

[Design Patterns 2 of 3 - Structural Design Patterns - CodeProject](https://www.codeproject.com/Articles/438922/Design-Patterns-of-Structural-Design-Patterns)

[Head First Design Patterns - O'Reilly Media](https://www.google.com.au/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0ahUKEwjE74WWy7rTAhVEppQKHfqGAjoQFggiMAA&url=http%3A%2F%2Fshop.oreilly.com%2Fproduct%2F9780596007126.do&usg=AFQjCNF91VIwQIeGyXH4xU67GibpAiRKRA&sig2=YcwhV4RTfJRpzWn3xsIcoA)