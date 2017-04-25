---
 title: Effective C# -- 12. Avoid Creating Unnecessary Objects
 date: {date}
 categories: C Sharp
---

1. All reference types, even local variables, are allocated on the __heap__.
    * One very common bad practice is to allocate GDI objects in a Windows paint handler.

    * Should promote __local variables__ to __member variables__ when they are reference types, they will be used in routines that are called frequently. __Only__ local variables in __routines that are frequently accessed are good candidates__.
    <!--more-->
    ```cs
    private readonly Font myFont = new Font("Arial", 10.0f);
    protected override void OnPaint(PaintEventArgs e)
    {
        e.Graphics.DrawString(DateTime.Now.ToString(),
        myFont, Brushes.Black, new PointF(0, 0));
        base.OnPaint(e);
    }
    ```
  * Create __static member__ variables for __commonly used instances__ of the __reference types__ you need.  
    ```cs
    private static Brush blackBrush;
    public static Brush Black
    {
        get
        {
            if (blackBrush == null)
                blackBrush = new SolidBrush(Color.Black);
            return blackBrush;
            } 
    }
    ```

2. Immutable objects
  * After you construct a string, the contents of that __string cannot be modified__. Whenever you write code to __modify__ the contents of a string, you are __creating a new string object__, which is __inefficient__.
  * __StringBuilder__ let you create and modify text data __before__ you construct an immutable string object.  
    ```cs
    //inefficient
    string msg = "Hello, ";
    msg += thisUser.Name;
    msg += ". Today is ";
    msg += System.DateTime.Now.ToString();
    //better
    StringBuilder msg = new StringBuilder("Hello, ");
    msg.Append(thisUser.Name);
    msg.Append(". Today is ");
    msg.Append(DateTime.Now.ToString());
    string finalMsg = msg.ToString();
    ```
    When your call for __immutable types__, consider __creating builder objects__ to facilitate the multiphase construction of the final object.