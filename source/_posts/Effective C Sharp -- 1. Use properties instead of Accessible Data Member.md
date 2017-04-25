---
 title: Effective C# -- 1. Use properties instead of Accessible Data Member
 date: {date}
 categories: .Net
---

1. Public data members are __bad practice__.
2. Properties let you expose data members as part of your public interface and still provide the __encapsulation__ you want in an object-oriented environment.
3. Properties are implemented with methods, adding __multithreaded__ support is easier.
<!--more-->
```cs
    public class Customer
    {
        private object syncHandle = new object();
        private string name;
        public string Name
        {
            get
            {
                lock (syncHandle)
                    return name;
            }
            set
            {
                if (string.IsNullOrEmpty(value))
                    throw new ArgumentException("Name cannot be blank", "Name");
                lock (syncHandle)
                    name = value;
                    // More Elided.
            }
        }
    }
```

4. Properties can be used in __interface__
```cs
    public interface INameValuePair<T>
    {
        string Name { get; }
        T Value { get; set; }
    }
```

5. Can specify different accessibility modifiers to get and set accessors in a property.
```cs
    public class Customer
    {
        public virtual string Name
        {
            get;
            protected set;
        }
        // remaining implementation omitted
    }
```