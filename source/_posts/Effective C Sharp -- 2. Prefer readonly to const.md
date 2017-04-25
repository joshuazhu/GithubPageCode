---
 title: Effective C# -- 2. Prefer readonly to const
 date: {date}
 categories: .Net
---

1. C# has two different version of constants: __compile-time__ constants and __runtime__ constants.
2. Runtime time constants is slower but correct than Compile constants.
```cs
  // Compile time constant:
  public const int Millennium = 2000;
  // Runtime constant:
  public static readonly int ThisYear = 2004;
```
3. Compile-time constants can also be __declared inside methods__. Read-only constants cannot be declared with method scope.

4. A compile-time constant is replaced with the value of that constant in your object code. Runtime constants are evaluated at runtime ( __Read-only values are also constants, in that they cannot be modified after the con- structor has executed.__), The IL generated when you reference a read-only constant references the readonly variable, not the value.

5. Compile-time constants can be used only for __primitive types (built-in integral and floating-point types), enums, or strings__. Runtime constants can be __any type__. You must __initialize them in a constructo__r, or you can use an initializer.