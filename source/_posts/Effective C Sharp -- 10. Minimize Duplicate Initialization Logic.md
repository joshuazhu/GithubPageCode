---
 title: Effective C# -- 10. Minimize Duplicate Initialization Logic
 date: {date}
 categories: C Sharp
---

1. Find that multiple constructors contain the same logic, factor that logic into a common constructor instead.

2. One constructor all another one.
```cs
public MyClass() : this(0, "")
{
}
public MyClass(int initialCount) : this(initialCount, string.Empty)
{
}
public MyClass(int initialCount, string name)
{
   //logic here
}
```

3. Order of operations for construction the first instance of a type.
    1. Static variable storage is set to 0.
    2. Static variable initializers execute.
    3. Static constructors for the base class execute.
    4. The static constructor executes.
    5. Instance variable storage is set to 0.
    6. Instance variable initializers execute.
    7. The appropriate base class instance constructor executes. 
    8. The instance constructor executes.

    __Subsequent instances__ of the same type start at step 5.

4. Use __initializers__ to initialize __simple resources__. Use __constructors__ to initialize members that require __more sophisticated__ logic.