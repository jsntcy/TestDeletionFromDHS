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

[!code-csharp[ReadOnlyAutoProperty](../../samples/snippets/csharp/new-in-6/newcode.cs#ReadOnlyAutoProperty)]

The `FirstName` and `LastName` properties can be set only in the body of a constructor:

[!code-csharp[ReadOnlyAutoPropertyConstructor](../../samples/snippets/csharp/new-in-6/newcode.cs#ReadOnlyAutoPropertyConstructor)]

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

### Auto-Property Initializers

C# 6 enables you to assign an initial value for the storage used by an
auto-property in the auto-property declaration:

[!code-csharp[Initialization](../../samples/snippets/csharp/new-in-6/newcode.cs#Initialization)]

The `grades` member is initialized where it is declared. That makes it
storage allocation with public interface for `Student` objects.

Property Initializers can be used with read/write properties as well
as read only properties, as shown here.

[!code-csharp[ReadWriteInitialization](../../samples/snippets/csharp/new-in-6/newcode.cs#ReadWriteInitialization)]

## Expression bodied function members

read-only properties." For example, an override of `ToString()` is often
a great candidate:

[!code-csharp[ToStringExpressionMember](../../samples/snippets/csharp/new-in-6/newcode.cs#ToStringExpressionMember)]

You can also use expression bodied members in read only properties as well:

[!code-csharp[FullNameExpressionMember](../../samples/snippets/csharp/new-in-6/newcode.cs#FullNameExpressionMember)]

## using static

The *using static* enhancement enables you to import the static methods
of a single class. Previously, the `using` statement imported all types
in a namespace.

[!code-csharp[UsingStaticMath](../../samples/snippets/csharp/new-in-6/newcode.cs#UsingStaticMath)]

And now, you can use any static method in the @System.Math class without
qualifying the @System.Math class. The @System.Math class is a great use case for
this feature because it does not contain any
instance methods. You can also use `using static` to import a
class' static methods for a class that has both static
and instance methods. One of the most useful examples is @System.String:

[!code-csharp[UsingStatic](../../samples/snippets/csharp/new-in-6/newcode.cs#UsingStatic)]

> [!NOTE]
> You must use the fully qualified class name, `System.String`
> in a static using statement.
> You cannot use the `string` keyword instead.

You can now call static methods defined in the @System.String class without
qualifying those methods as members of that class:

[!code-csharp[UsingStaticString](../../samples/snippets/csharp/new-in-6/newcode.cs#UsingStaticString)]

The `static using` feature and extension methods interact in
interesting ways, and the language design included some rules
that specifically address those interactions. The goal is to
minimize any chances of breaking changes in existing codebases,
including yours.

[!code-csharp[UsingStaticLinq](../../samples/snippets/csharp/new-in-6/newcode.cs#usingStaticLinq)]

This imports all the methods in the @System.Linq.Enumerable class.
However, the extension methods are only in scope when called as extension
methods. They are not in scope if they are called using the static method
syntax:

[!code-csharp[UsingStaticLinqMethod](../../samples/snippets/csharp/new-in-6/newcode.cs#UsingStaticLinkMethod)]

This decision is because extension methods are typically called using
extension method invocation expressions. In the rare case where they are

## String Interpolation

C# 6 contains new syntax for composing strings from a format string
and expressions that can be evaluated to produce other string values.

[!code-csharp[stringInterpolation](../../samples/snippets/csharp/new-in-6/newcode.cs#FullNameExpressionMember)]

This initial example used variable expressions for the substituted
expressions. You can expand on this syntax to use any expression. For
example, you could compute a student's grade point average as part of
the interpolation:

[!code-csharp[stringInterpolationExpression](../../samples/snippets/csharp/new-in-6/newcode.cs#stringInterpolationExpression)]

Running the preceding example, you would find that the output for `Grades.Average()`
might have more decimal places than you would like. The string interpolation
syntax supports all the format strings available using earlier formatting
methods. You add the format strings inside the braces. Add a `:` following
the expression to format:

[!code-csharp[stringInterpolationFormat](../../samples/snippets/csharp/new-in-6/newcode.cs#stringInterpolationFormat)]

The preceding line of code will format the value for `Grades.Average()` as
a floating-point number with two decimal places.

The `:` is always interpreted as the separator between the expression
being formatted and the format string. This can introduce problems when
your expression uses a `:` in another way, such as a conditional operator:

```csharp
public string GetGradePointPercentages() =>
    $"Name: {LastName}, {FirstName}. G.P.A: {Grades.Any() ? Grades.Average() : double.NaN:F2}";
```

In the preceding example, the `:` is parsed as the beginning of the format string, not part
of the conditional operator. In all cases where this happens, you can
surround the expression with parentheses to force the compiler to interpret
the expression as you intend:

[!code-csharp[stringInterpolationConditional](../../samples/snippets/csharp/new-in-6/newcode.cs#stringInterpolationConditional)]

There aren't any limitations on the expressions you can place between
the braces. You can execute a complex LINQ query inside an interpolated
string to perform computations and display the result:

[!code-csharp[stringInterpolationLinq](../../samples/snippets/csharp/new-in-6/newcode.cs#stringInterpolationLinq)]

You can see from this sample that you can even nest a string interpolation
expression inside another string interpolation expression. This example
is very likely more complex than you would want in production code.
Rather, it is illustrative of the breadth of the feature. Any C# expression
can be placed between the curly braces of an interpolated string.

### String interpolation and specific cultures

The @System.FormattableString type contains the format string, and the results
of evaluating the arguments before converting them to strings. You can
use public methods of @System.FormattableString to specify the culture when
formatting a string. For example, the following will produce a string
using German as the language and culture. (It will use the ',' character
for the decimal separator,
and the '.' character as the thousands separator.)

```csharp
FormattableString str = @"Average grade is {s.Grades.Average()}";
var gradeStr = string.Format(null,
    System.Globalization.CultureInfo.CreateSpecificCulture("de-de"),
    str.GetFormat(), str.GetArguments());
```

> [!NOTE]
> The preceding example is not supported in .NET Core version 1.0.1. It is
> only supported in the .NET Framework.

In general, string interpolation expressions produce strings as their
output. However, when you want greater control over the culture used to
format the string, you can specify a specific output.  If this is a capability
you often need, you can create convenience methods, as extension methods,
to enable easy formatting with specific cultures.

## Exception Filters

Another new feature in C# 6 is *exception filters*. Exception Filters
are clauses that determine when a given catch clause should be applied.
If the expression used for an exception filter evaluates to `true`, the
catch clause performs its normal processing on an exception. If the
expression evaluates to `false`, then the `catch` clause is skipped.

After adding this in code, you set your debugger to break on all unhandled
exceptions. Run the program under the debugger, and the debugger breaks
whenever `PerformFailingOperation()` throws a `RecoverableException`.
The debugger breaks your program, because the catch clause won't be executed
due to the false-returning exception filter.

## `nameof` Expressions

The `nameof` expression evaluates to the name of a symbol. It's a great
way to get tools working whenever you need the name of a variable,
a property, or a member field.

One of the most common uses for `nameof` is to provide the name of a symbol
that caused an exception:

[!code-csharp[nameof](../../samples/snippets/csharp/new-in-6/NewCode.cs#UsingStaticString)]

Another use is with XAML based applications that implement the `INotifyPropertyChanged`
interface: