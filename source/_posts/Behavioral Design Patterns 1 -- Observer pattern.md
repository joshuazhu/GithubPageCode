---
 title: Behavioral Design Patterns 1 -- Observer pattern
 date: {date}
 categories: Design Patterns
---

## Definition
Defines a one-to-many dependency between objects so that when one object changes state, all of its dependents are notified and updated automatically.

## Components
1. __SubjectBase__: base class of all subjects.
    a. Contains a __list__ of __observers__ that are subscribed to the subject and methods.
    b. A __Notify__ method loops through the list of observers to call __their notify__ method.
2. __ConcreteSubject__: concrete implementation of base subject.
3. __ObserverBase__: base class of observers, contains methods which is called when a subject's state changes.
4. __ConcreteObserver__: concrete implementation of base observer.
![Observer pattern UML](https://www.codeproject.com/KB/architecture/455228/observer.jpg)

<!--More-->

## Sample code
```cs
using System;
using System.Collections.Generic;
namespace ObserverPattern
{
    static class ObserverPattern
    {
        public static void Main()
        {
            var subject = new ConcreteSubject();
            subject.Attach(new ConcreteObserver(subject, "Observer 1"));
            subject.Attach(new ConcreteObserver(subject, "Observer 2"));
            subject.Attach(new ConcreteObserver(subject, "Observer 3"));
            subject.SetState("STATE");
        }
    }
    public abstract class SubjectBase
    {
        private List<ObserverBase> _observers = new List<ObserverBase>();
        public void Attach(ObserverBase o)
        {
            _observers.Add(o);
        }
        public void Detach(ObserverBase o)
        {
            _observers.Remove(o);
        }
        public void Notify()
        {
            foreach (var o in _observers)
            {
                o.Update();
            }
        }
    }
    public class ConcreteSubject : SubjectBase
    {
        private string _state;
        public string GetState()
        {
            return _state;
        }
        public void SetState(string newState)
        {
            _state = newState;
            Notify();
        }
    }
    public abstract class ObserverBase
    {
        public abstract void Update();
    }
    public class ConcreteObserver : ObserverBase
    {
        private ConcreteSubject _subject;
        private string _name;
        public ConcreteObserver(ConcreteSubject subject, string name)
        {
            _subject = subject;
            _name = name;
        }
        public override void Update()
        {
            string subjectState = _subject.GetState();
            Console.WriteLine(_name + ": " + subjectState);
        }
    }
}
```

## Advantages
1. The __only__ thing the subject knows about an __observer__ is that it __implements__ a certain __interface__.

2. Because subjects only depends on the Observers' interface, we can __add or replace any observer at runtime__.

3. We can __reuse subjects or observers__ independently of each other.

4. __Changes__ to either the subjects or an observer __will not affect the other__.

## Disadvantages
1. If a subject has __many direct or indirect observers__, it may __take a while__ to notice all observers when a state change happens.

2. If observers depends on each other, which may result in a __dependency loop__ and it will __crash the system__.

## Reference
[Design Patterns 1 of 3 - Creational Design Patterns - CodeProject](https://www.codeproject.com/Articles/430590/Design-Patterns-of-Creational-Design-Patterns)
[Head First Design Patterns - O'Reilly Media](https://www.google.com.au/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0ahUKEwjE74WWy7rTAhVEppQKHfqGAjoQFggiMAA&url=http%3A%2F%2Fshop.oreilly.com%2Fproduct%2F9780596007126.do&usg=AFQjCNF91VIwQIeGyXH4xU67GibpAiRKRA&sig2=YcwhV4RTfJRpzWn3xsIcoA)