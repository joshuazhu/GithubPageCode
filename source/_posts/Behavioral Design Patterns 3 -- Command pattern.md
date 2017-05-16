---
 title: Behavioral Design Patterns 3 -- Command pattern
 date: {date}
 categories: Design Patterns
---

## Definition
>The command pattern is a design pattern that enables __all of the information for a request to be contained within a single object__. The command can then be invoked as required, often as part of a batch of queued commands with rollback capabilities.

## When to use Command pattern

1. When it is required to separate invoker and receiver.
2. When system needs to generate, queue and execute requests in different time.
3. When it is required to provide "undo" and "rockback" command. The common object could store the current state, which could be used to implement undo or redo command.
4. When system need to do batch command。

## Components

* Client: The class is a consumer of the classes of the Command design pattern. It creates the command objects and links them to receivers.
* Receiver: this is the class which knows how to perform the operations associated with carrying out the request.
* CommandBase: this is the abstract class (or interface) for all command objects. It holds information about the receiver which is responsible for executing an operation using information encapsulated within the command object.
* ConcreteCommand: concrete implementation of the CommandBase abstract class or interface.
* Invoker: is the object which decides when to execute the command.

<!-- More -->

![Command Pattern UML](http://my.csdn.net/uploads/201205/09/1336547877_9980.jpg)

## Codes

~~~cs
/*
 * Receiver
 */
public class Document
{
    private List<string> _textArray = new List<string>();

    public void Write(string text)
    {
        _textArray.Add(text);
    }
    public void Erase(string text)
    {
        _textArray.Remove(text);
    }
    public void Erase(int textLevel)
    {
        _textArray.RemoveAt(textLevel);
    }
    public string ReadDocument()
    {
        System.Text.StringBuilder sb = new System.Text.StringBuilder();
        foreach (string text in _textArray)
            sb.Append(text);
        return sb.ToString();
    }
}

/*
 * Command
 */
public abstract class Command
{
    abstract public void Redo();
    abstract public void Undo();
}

public class DocumentEditCommand : Command
{
    private Document _editableDoc;

    //store the current state
    private string _text;

    public DocumentEditCommand(Document doc, string text)
    {
        _editableDoc = doc;
        _text = text;
        _editableDoc.Write(_text);
    }

    public override void Redo()
    {
        _editableDoc.Write(_text);
    }

    public override void Undo()
    {
        _editableDoc.Erase(_text);
    }
}

/*
 * Invoker
 */
public class DocumentInvoker
{
    private List<Command> _commands = new List<Command>();
    private Document _doc = new Document();

    public void Redo(int level)
    {
        Console.WriteLine("---- Redo {0} level ", level);
        ((Command)_commands[level]).Redo();
    }

    public void Undo(int level)
    {
        Console.WriteLine("---- Undo {0} level ", level);
        ((Command)_commands[level]).Undo();
    }

    public void Write(string text)
    {
        DocumentEditCommand cmd = new
            DocumentEditCommand(_doc, text);
        _commands.Add(cmd);
    }

    public void Read()
    {
        Console.WriteLine(_doc.ReadDocument());
    }
}

/*
 * Client
 */
public class Client
{
    public static void Main(String[] args)
    {
        DocumentInvoker instance = new DocumentInvoker();
        instance.Write("This is the original text.");
        instance.Write(" Here is some other text.");
        instance.Read();
        instance.Undo(1);
        instance.Read();
        instance.Redo(1);
        instance.Read();
        instance.Write(" And a little more text.");
        instance.Read();
        instance.Undo(2);
        instance.Read();
        instance.Redo(2);
        instance.Read();
        instance.Undo(1);
        instance.Read();
    }
}
~~~

## Advantage

1. Code Decoupling: Invoker and Receiver are independent, Invoker only needs to call Command's execute function.
2. Easy to extend：The Command subclasses are easy to extend, no need to change existing class.
3. Composing command: easy to compose a group of commands.

## Reference

[Design Patterns 3 of 3 - Behavioral Design Patterns - CodeProject](https://www.codeproject.com/Articles/455228/Design-Patterns-of-Behavioral-Design-Patterns)

[Head First Design Patterns - O'Reilly Media](https://www.google.com.au/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0ahUKEwjE74WWy7rTAhVEppQKHfqGAjoQFggiMAA&url=http%3A%2F%2Fshop.oreilly.com%2Fproduct%2F9780596007126.do&usg=AFQjCNF91VIwQIeGyXH4xU67GibpAiRKRA&sig2=YcwhV4RTfJRpzWn3xsIcoA)