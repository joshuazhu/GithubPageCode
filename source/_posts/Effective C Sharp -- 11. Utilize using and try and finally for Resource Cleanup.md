---
 title: Effective C# -- 11. Utilize using and try/finally for Resource Cleanup
 date: {date}
 categories: .Net
---

1. Anytime you use types that have a __Dispose()__ method, it’s your responsibility to __release those resources by calling Dispose()__. __Best way__ to ensure that __Dispose()__ always gets called is to utilize the __using__ statement or a __try/finally block__.

2. 
    * All types own unmanaged resources __implement__ the __IDisposable interface__.
    * They all created a __finalizer__.
    * Nonmemory resources are freed later when finalizer get chance to execute.

3. The using statement ensures that Dispose() is called. Using a using statement, the C# compiler __generates a try/finally block__ around each object.
```cs
    // Example Using clause:
    using (myConnection = new SqlConnection(connString))
    {
        myConnection.Open();
    }
    //
    // example Try / Catch block:
    //
    try
    {
        myConnection = new SqlConnection(connString);
        myConnection.Open();
    }
    finally
    {
        myConnection.Dispose();
    }
```

4. The using statement __works only__ if the compile-time type supports the __IDisposable__ interface, __as__ clause will safely dispose of objects that might/might not implement IDisposable.
<!-- more -->
```cs
    // The correct fix.
    // Object may or may not support IDisposable.
    object obj = Factory.CreateResource();
    using (obj as IDisposable)
        Console.WriteLine(obj.ToString());
```

5. If obj implements IDisposable, the using statement generates the cleanup code. If not, the using statement __degenerates to using(null)__, which is safe but __doesn’t do anything__. 

6. Two disposable object in one method. If use two __using__ clause, it will create a bug, the first call using will never gets disposed if the second one throws an exception.
```cs
    using (myConnection as IDisposable)
    using (mySqlCommand as IDisposable)
    {
        myConnection.Open();
        mySqlCommand.ExecuteNonQuery();
    }
    //the above code will generate the following code when compiling.
    //if myConnection throw an exception, the
    SqlConnection myConnection = null;
    SqlCommand mySqlCommand = null;
    try
    {
        myConnection = new SqlConnection(connString);
        try
        {
            mySqlCommand = new SqlCommand(commandString,
            myConnection);
            myConnection.Open();
            mySqlCommand.ExecuteNonQuery();
        }
        finally
        {
            if (mySqlCommand != null)
                mySqlCommand.Dispose();
        } 
    }
    finally
    {
        if (myConnection != null)
            myConnection.Dispose();
    }
```

7. Some types support both a __Dispose__ method and a __Close__ method to __free resources__.
  * The __Dispose__ method also __notifies__ the Garbage Collector that the object no longer needs to be finalized. Dispose calls GC.SuppressFinalize(). ___Close typically does not__, the object will remain in the finalization queue, even though finalization is not needed.
  * The __Dispose__ does not remove objects from memory, so you can get into trouble by disposing of objects that are still in use. 
    * For example: After you dispose of the __connection__, the __SQLConnection object__ is still in memory, but it is __no longer connected to a database__.
  * Do not dispose of objects that are __still being referenced__ elsewhere in your program.