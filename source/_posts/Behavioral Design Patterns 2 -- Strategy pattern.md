---
 title: Behavioral Design Patterns 2 -- Strategy pattern
 date: {date}
 categories: Design Patterns
---

## Definition
Defines a family of algorithms, encapsulates each one, and makes them interchangeable. Strategy lets the __algorithm vary indenpendently__ from clients that use it.


## Components
1. __Client__: this class uses interchangeable algorithms. It maintains __a reference to StrategyBase object__. 
2. __StrategyBase__: declares an __interface__ common to all supported algorithms.
3. __ConcreteStrategy__:__inherited__ from the StrategyBase Class, provides a different algorithm.

![Strategy pattern uml](https://www.codeproject.com/KB/architecture/455228/strategy.jpg)

<!--More-->

## Sample code
~~~cs
public interface Istrategy {
	public void operate();
}

public class Strategy1 implements Istrategy{ 
	public void operate(){
		System.out.println("operate 1");
	}
}

public class Strategy2 implements Istrategy {
	public void operate(){
		System.out.println("operate 2");
	}
}

public class Context {
	private Istrategy strategy;
 
	public Context(Istrategy strategy) {
		this.strategy = strategy;
	}

	public void operate() {
		this.strategy.operate();
	}
}

public class Client {
	public static void main(string args[]) {
		Context context;
		context = new Context (new Strategy1());
		context.operate();

		context = new Context (new Strategy2());
		context.operate();
	}
}
~~~
## Advantages
1. Provides design where __client__ and __strategies__ are __loosely coupled__.
2. __Easy__ to __add__ or __replace__ or __reuse__ clients or strategies.

## Reference 
[Design Patterns 1 of 3 - Creational Design Patterns - CodeProject](https://www.codeproject.com/Articles/430590/Design-Patterns-of-Creational-Design-Patterns)
[Head First Design Patterns - O'Reilly Media](https://www.google.com.au/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0ahUKEwjE74WWy7rTAhVEppQKHfqGAjoQFggiMAA&url=http%3A%2F%2Fshop.oreilly.com%2Fproduct%2F9780596007126.do&usg=AFQjCNF91VIwQIeGyXH4xU67GibpAiRKRA&sig2=YcwhV4RTfJRpzWn3xsIcoA)