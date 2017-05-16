---
 title: Behavioral Design Patterns 5 -- Mediator pattern
 date: {date}
 categories: Design Patterns
---

## Definition
>Allows people to __decouple__ the direct communication between objects by introducing a __Meditator__, which is used to implementing the __communication between the objects__.

The mediator is the __communication center__ for the objects. Instead of communicating to other objects directly, __objects call mediator to send their messages to the destination object__.

## When to use Mediator Pattern

1. When N (n >= 2) objects depend on each other.
2. When multiple objects depend on each other and the __dependencies are not clear or may be changed frequently__. Applying Mediator pattern could __reduce the risks raised by depencency changes__.

## Components

1. __MediatorBase__： Abstract class for the mediator object. This class contains methods that can be consumed by colleagues.
    * __ColleagueList__: This is the list of the registered participants.
    * __DistributeMessage(ColleagueBase)__: Send message to all participants.
    * __Register(ColleagueBase)__: Register a colleague to receive the message from mediator.
2. __ConcreateMediator__： This class implements communication methods of the MediatorBase class.
    * __DistributeMessage(ColleagueBase)__: Implement the method from MediatorBase .
    * __Register(ColleagueBase)__: Implement the method from MediatorBase.
3. __ColleagueBase__： This is the abstract class (or interface) for all concrete colleagues. It has a protected field that holds __a reference to the mediator object__.
    * __SendMessage(IMediator)__ : Sends the message to the mediator
    * __ReceiveMessage()__ : Gets the message from the mediator
4. __ConcreteColleague__：this is the implementation of the ColleagueBase class. Concrete colleagues are classes which communicate with each other.
    * __SendMessage(IMediator)__ : Implement the method from ColleagueBase.
    * __ReceiveMessage()__ : Gets the message from the mediator

![Mediator Pattern UML](https://www.codeproject.com/KB/architecture/455228/mediator.jpg)

<!-- More -->

## Code

```cs
public interface IMediator
{
    List<IColleague> ColleagueList { get; set; }

    void DistributeMessage(IColleague sender, string message);

    void Register(IColleague colleague);
}

public class ConcreteMediator : IMediator
{
    public ConcreteMediator()
    {
        ColleagueList = new List<IColleague>();
    }

    public void Register(IColleague colleague)
    {
        ColleagueList.Add(colleague);
    }

    public List<IColleague> ColleagueList { get; set; }

    public void DistributeMessage(IColleague sender, string message)
    {
        foreach (IColleague c in ColleagueList)
            if (c != sender)
            {
                c.ReceiveMessage(message);
            }
    }
}

public class ConcreteColleague : IColleague
{
    private string Name;

    public ConcreteColleague(string name)
    {
        Name = name;
    }

    public void SendMessage(IMediator mediator, string message)
    {
        mediator.DistributeMessage(this, message);
    }

    public void ReceiveMessage(string message)
    {
        Console.WriteLine(Name + " received " + message.ToString());
    }
}

public interface IColleague
{
    void SendMessage(IMediator mediator, string message);

    void ReceiveMessage(string message);
}

public class Client
{
    public static void Main()
    {
        var ColleagueA = new ConcreteColleague("A");
        var ColleagueB = new ConcreteColleague("B");
        var ColleagueC = new ConcreteColleague("C");
        var ColleagueD = new ConcreteColleague("D");

        var mediator1 = new ConcreteMediator();
        mediator1.Register(ColleagueA);
        mediator1.Register(ColleagueB);
        mediator1.Register(ColleagueC);

        ColleagueA.SendMessage(mediator1, "MessageX from ColleagueA");

        var mediator2 = new ConcreteMediator();
        mediator2.Register(ColleagueB);
        mediator2.Register(ColleagueD);

        ColleagueB.SendMessage(mediator2, "MessageX from ColleagueA");
    }
}
```

## Reference

[Design Patterns 1 of 3 - Creational Design Patterns - CodeProject](https://www.codeproject.com/Articles/430590/Design-Patterns-of-Creational-Design-Patterns)

[Mediator Design Pattern - CodeProject](https://www.google.com.au/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&ved=0ahUKEwjOgOey2dPTAhUEu7wKHfTQBLcQFggxMAE&url=https%3A%2F%2Fwww.codeproject.com%2FArticles%2F186187%2FMediator-Design-Pattern&usg=AFQjCNF7YG9XMjJi6qAAQwt8MBLhBLM0lQ&sig2=ltS0Omhy4zV4Hkoh06o7GA&cad=rjt)