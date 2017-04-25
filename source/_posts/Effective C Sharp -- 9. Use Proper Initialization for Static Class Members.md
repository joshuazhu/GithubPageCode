---
 title: Effective C# -- 9. Use Proper Initialization for Static Class Members
 date: {date}
 categories: .Net
---

1. A static constructor executes before any other methods to initialize static member variables.

2. __Should not use any instance constructors, private function or idiom to initialize static variables.__

3. Could use  __initializer syntax__ as an __alternative__ to the __static constructor__. If have more complicated logic for initialization, create a static constructor.
```cs
    public class MySingleton2
    {
        private static readonly MySingleton2 theOneAndOnly;

        // static constructor
        static MySingleton2()
        {
            theOneAndOnly = new MySingleton2();
        }
        public static MySingleton2 TheOnly
        {
            get { return theOneAndOnly; }
        }
        private MySingleton2()
        {
        }
        // remainder elided
    }
```

4. Static constructor:
    * Can define __only one__ static constructor.
    * Must __not__ take any __arguments__.
    * Will be called by __CLR__, if an exception escape  a static constructor, __CLR will terminate program__.