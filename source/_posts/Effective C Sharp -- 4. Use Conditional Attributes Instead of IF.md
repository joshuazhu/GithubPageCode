---
 title: Effective C# -- 4. Use Conditional Attributes Instead of IF
 date: {date}
 categories: C Sharp
---

1. If use a __ #if and #endif __ pragmas in a function for debug model, in release model will have an empty function, which will cause unnecessary cost. 
```cs
    public void Func()
    {
    #if DEBUG
        msg = GetDiagnostics();
    #endif
    //when release, the function body is empty     
    }
```

2. Instead, we can use conditional attribute. __Conditional attribute will invoke the function marked as "DEBUG" only when creating debug builds__.
<!-- more -->
```cs
    [Conditional("DEBUG")]
    private void CheckState()
    {
        // logic
    }
```

3. The Conditional attribute tells the C# compiler that this method should be called __only when the compiler detects the DEBUG environment variable__.
```cs
    //When DEBUG, you get
    public string LastName
    {
    get
    {
        CheckState();
        return lastName;
    }
    set
    {
        CheckState();
        lastName = value;
        CheckState();
    }
    }
    //When Release, the same code get this
    public string LastName
    {
    get
        {
            return lastName;
        }
    set
        {
            lastName = value;
        }
    }
```


4. Easier to create methods depend on more than one environment variable.
```cs
    [Conditional("DEBUG"), Conditional("TRACE")]
    private void CheckState()
```


5. The conditional attribute __can be applied only to entire methods__, and any method with a conditional attribute __must have a return type of void__. You __cannot__ use the Conditional attribute for __blocks of code inside methods or with methods that return values__. 

6. __The best practice__ is to any method marked with the Conditional attribute should __not take any parameters__. __The user could use a method call with side effects to generate those parameters. Those method calls will not take place if the condition is not true__.
```cs
    //assignment will not happen if it is debug
    SomeMethod(item = names.Dequeue());
    Console.WriteLine(item);
    [Conditional("DEBUG")]
    private static void SomeMethod(string param)
    {
    }
```

7. The Conditional attribute generates __more efficient IL than #if/#endif does__. It also has the advantage of being applicable __only at the function level__, which forces you to __better structure your conditional code__.