---
 title: Effective C# -- 3. Prefer the is or as to Casts
 date: {date}
 categories: C Sharp
---

1. Correct choice for type casting is to user the __as__ operator as it is __safer__ and more __efficient__ at runtime (they __succeed only__ if the runtime type matches the sought type, __never construct a new object__).
```cs
    object o = Factory.GetObject();
    // Version one:
    MyType t = o as MyType;
    if (t != null)
    {
        // work with t, it's a MyType.
    }
    else
    {
    }
```

2. When using __cast__ instead of __is__ or __as__, need to __check null__ in addition to catching exceptions, __null can be converted to any reference type using a cast__.

3. The main difference between cast and as operator:
  * As/Is: do not perform any other operations, is object is not the requested type or derived from the requested type, they fail.
  * Casts: Can use conversion operators to convert an object to requested type (casting a long to short can lose information) 
<!-- more -->
4. The user-defined conversion operators may not work for cast as it is only work on the compile-time type of an object, not on the runtime type.
```cs
    public class SecondType
    {
        private MyType _value;
        // other details elided
        // Conversion operator.
        // This converts a SecondType to
        // a MyType, see item 9.
        public static implicit operator
            MyType(SecondType t)
            {
                return t._value;
            }

        public static void Main(string []args)
        {
            
            try
            {
                MyType t1;
                //user-defined operator will not work
                t1 = (MyType)o; // Fails. o is not MyType
                // work with t1, it's a MyType.
            }
            catch (InvalidCastException e)
            {
                Console.WriteLine(e.Message);
                // report the conversion failure.
            }
        }
    }
```

5. The __as__ operator does not work on __value types__. That’s because ints are value types and __can never be null__. Use __is__ operator only when you cannot convert the type using __as__
```cs
    i = (int)o; // will throw an error
    //use is to check if o is an integer
    object o = Factory.GetValue();
    int i = 0;
    if (o is int)
    i = (int)o;
```

6. If you’re about to __convert a type using as__, the is check is simply not necessary. __Check the return from as against null__; it’s simpler.

7. __foreach__ uses a cast operation to perform conversions from an object to the type used in the loop. __The foreach statement__ (which uses a cast) does not examine the casts that are available in the runtime type of the objects in the collection (__user-defined operator__). It examines only the conversions available in the __System.Object class__.
