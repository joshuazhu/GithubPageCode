---
 title: Effective C# -- 8. Prefer Member Initializers to Assignment Statements
 date: {date}
 categories: .Net
---

1. Declare member variable instead of in the body of every constructor, utilize the initializer syntax for both static and instance variables.
2. The compiler generates code __at the beginning of each constructor__ to execute all the initializers.
<!-- more -->
3. The initializers are added to the __compiler-generated default constructor__.
4. Initializers execute __before the base class constructor__, and they are executed __in the order the variables are declared__ in your class.
5. Three cases should __not__ use initializer.
    * initializing the object to 0, or null: __default__ system initialization sets everything to 0.
    * facilitate exception handling: You cannot wrap the initializers in a try block, You should __move that initialization code into the body__ of your constructors to catch exception.
    * when you create multiple initializations for the __same object__.
    ```cs
      public class MyClass2
      {
            // declare the collection, and initialize it.
            private List<string> labels = new List<string>();
            MyClass2()
            {
            }
            MyClass2(int size)
            {
                //compiler will generate code as labels = new List<string>()
                //which is unnecessary.
                labels = new List<string>(size);
            }
      }
    ```