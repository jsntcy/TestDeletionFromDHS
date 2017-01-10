# What's New in C# 6

The 6.0 release of C# contained many features that improve
productivity for developers. Features in this release include:
The overall effect of these features is that you write more concise code
that is also more readable. The syntax contains less ceremony for many

The remainder of this topic provides details on each of these features.

Using this syntax, the compiler doesn't ensure that the type really is immutable. It only
enforces that the `FirstName` and `LastName` properties are not modified from any
code outside the class.

Read-only auto-properties enable true read-only behavior. You declare the auto-property
with only a get accessor:

[!code-csharp[ReadOnlyAutoPropertyConstructor](newcode.cs#ReadOnlyAutoPropertyConstructor)]

Trying to set `LastName` in another method generates a `CS0200` compilation error:

```csharp
public class Student
{
    public string LastName { get;  }

    public void ChangeName(string newLastName)
    {
        // Generates CS 0200: Property or indexer cannot be assigned to -- it is read only
        LastName = newLastName;
    }
}
```

This feature enables true language support for creating immutable types and using
the more concise and convenient auto-property syntax.
