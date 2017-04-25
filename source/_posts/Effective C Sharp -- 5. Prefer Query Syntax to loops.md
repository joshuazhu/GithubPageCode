---
 title: Effective C# -- 5. Prefer Query Syntax to loops
 date: {date}
 categories: .Net
---

1. Use the query syntax creates more readable code and enables reuse of different building blocks.
```cs
    //Example 1
    /***************************
    *traditional method syntax
    ****************************/
    int[] foo = new int[100];
    for (int num = 0; num < foo.Length; num++)
        foo[num] = num * num;
    foreach (int i in foo)
        Console.WriteLine(i.ToString());
    /****************************
    *Query syntax
    ****************************/
    int[] foo = (from n in Enumerable.Range(0, 100)
                select n * n).ToArray();

    //Example2
    /***************************
    *traditional method syntax
    ****************************/
    private static IEnumerable<Tuple<int, int>> ProduceIndices2()
    {
        for (int x = 0; x < 100; x++)
            for (int y = 0; y < 100; y++)
                if (x + y < 100)
                    yield return Tuple.Create(x, y);
    }
    /****************************
    *Query syntax
    ****************************/
    private static IEnumerable<Tuple<int, int>> QueryIndices2()
    {
        return from x in Enumerable.Range(0, 100)
            from y in Enumerable.Range(0, 100)
            where x + y < 100
            select Tuple.Create(x, y);
    }
```

<!-- more -->
2. Queries create __a more composable__ API than looping constructs can provide. The deferred execution model for queries enables developers to __compose these single operations into multiple operations__ that can be accomplished in __one enumeration of the sequence__.
```cs
    //Method syntax
    private static IEnumerable<Tuple<int, int>> ProduceIndices3()
    {
        var storage = new List<Tuple<int, int>>();
        for (int x = 0; x < 100; x++)
            for (int y = 0; y < 100; y++)
                if (x + y < 100)
                    storage.Add(Tuple.Create(x, y));
        storage.Sort((point1, point2) =>
            (point2.Item1*point2.Item1 +
            point2.Item2 * point2.Item2).CompareTo(
            point1.Item1 * point1.Item1 +
            point1.Item2 * point1.Item2));
        return storage;
    }
    //Query Syntax
    private static IEnumerable<Tuple<int, int>> QueryIndices3()
    {
        return from x in Enumerable.Range(0, 100)
                from y in Enumerable.Range(0, 100)
                where x + y < 100
                orderby (x*x + y*y) descending
                select Tuple.Create(x, y);
    }
```