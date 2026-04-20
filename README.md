# C# Basics

> *Click &#9733; if you like the project. Your contributions are heartily ♡ welcome.*

<br/>

## Related Topics

* *[HTML Basics](https://github.com/learning-zone/html-basics)*
* *[CSS Basics](https://github.com/learning-zone/css-basics)*
* *[React Basics](https://github.com/learning-zone/react-basics)*
* *[Angular Basics](https://github.com/learning-zone/angular-basics)*
* *[SQL Basics](https://github.com/learning-zone/sql-basics)*
* *[ASP.NET Core](asp-net-core.md)*
* *[ADO.NET](ado.net.md)*
* *[.NET Multiple Choice Questions](dotnet-mcq.md)*
* *[Unit Testing](dotnet-unit-test.md)*
* *[Design Patterns](dotnet-dp.md)*
* *[Data Structures and Algorithms](dotnet-ds.md)*

<br/>

## Table of Contents

* [Fundamentals](#-1-fundamentals)
* [Operators](#-2-operators)
* [Classes](#-3-classes)
* [Inheritance](#-4-inheritance)
* [Collections](#-5-collections)
* [Multithreading](#-6-multithreading)
* [File Handling](#-7-file-handling)
* [Regular Expression](#-8-regular-expression)
* [Exception Handling](#-9-exception-handling)
* [Events and Delegates](#-10-events-and-delegates)
* [Garbage Collection](#-11-garbage-collection)
* [Lambda Expressions](#-12-lambda-expressions)
* [Language Integrated Query](#-13-language-integrated-query)
* [Microservices](#-14-microservices)
* [Performance and Optimization](#-15-performance-and-optimization)
* [Deployment](#-16-deployment)
* [.NET Core](#-17-net-core)
* [Miscellaneous](#-18-miscellaneous)

<br/>

## # 1. FUNDAMENTALS

<br/>

## Q. What is C# and what are its main features?

C# is a modern, object-oriented, type-safe programming language developed by Microsoft as part of the **.NET platform** (formerly .NET Core). The current stable versions are **.NET 10** (LTS, released November 2025) and **C# 14**, designed for building web, desktop, mobile, cloud, gaming, and AI applications.

Main features of C#:

* **Object-Oriented Programming:** C# supports classes, objects, inheritance, polymorphism, and encapsulation. With **C# 9+ Records** and **C# 12 Primary Constructors**, writing concise, immutable data models is easier than ever.

```cs
// C# 12 Primary Constructor
public class Person(string name, int age)
{
    public string Name { get; } = name;
    public int Age { get; } = age;
}
```

* **Type Safety:** C# enforces compile-time type checking. **Nullable Reference Types** (C# 8+) further eliminate null-reference exceptions by making nullability explicit.

```cs
string? nullableName = null;        // explicitly nullable
string nonNullableName = "Pradeep"; // cannot be null
```

* **Pattern Matching (C# 8–14):** C# has rich pattern matching with `switch` expressions, relational, list, and type patterns.

```cs
object shape = new Circle(5);
string description = shape switch
{
    Circle { Radius: > 10 } => "Large circle",
    Circle c => $"Circle with radius {c.Radius}",
    _ => "Unknown shape"
};
```

* **Records (C# 9+):** Concise, immutable reference types with value-based equality built in.

```cs
public record Product(string Name, decimal Price);
var p1 = new Product("Laptop", 999.99m);
var p2 = p1 with { Price = 899.99m }; // non-destructive mutation
```

* **LINQ (Language Integrated Query):** Unified querying of in-memory collections, databases, XML, and JSON.

* **Asynchronous Programming:** `async`/`await` with `IAsyncEnumerable<T>` (C# 8+) for efficient async streaming.

```cs
await foreach (var item in GetStreamAsync())
    Console.WriteLine(item);
```

* **Generic Math (C# 11+):** Write algorithms that work across all numeric types using `INumber<T>`.

```cs
T Sum<T>(T[] numbers) where T : INumber<T>
    => numbers.Aggregate(T.Zero, (acc, n) => acc + n);
```

* **Native AOT (Ahead-of-Time Compilation, .NET 7+):** Compile to native binaries for fast startup, small footprint, and deployment without the .NET runtime — ideal for microservices and CLI tools.

* **Collection Expressions (C# 12):** Unified syntax for all collection types.

```cs
int[] array = [1, 2, 3];
List<string> list = ["a", "b", "c"];
Span<int> span = [10, 20, 30];
```

* **Cross-Platform Development:** .NET 10 runs on Windows, Linux, macOS, Android, iOS, and WebAssembly (Blazor).

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the different types of data types available in C#?

C# offers a variety of data types categorized as value types, reference types, and pointer types. Value types store data directly, while reference types store memory addresses to the actual data. 

```
                    C# Data Type
                        |
        |-----------------------------------------------------------------------------|
    Value Type                                                                  Reference Type
        |                                                                             |
|------------------|------------|-----------|                                         |
|                  |            |           |                                         |
Simple Types   Enum Types  Struct Type  Nullable Type                                 |
                                                        |-----------------|-----------------|------------|
                                                        |                 |                 |            |
                                                      Class Types   Interface Types   Array Types  Delegate Types
```

**1. Value Types:**

These store data directly and include:

* **Integral types**: `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`
* **Native-sized integers (C# 9+)**: `nint`, `nuint` — size matches the platform\'s pointer size (32 or 64-bit)
* **128-bit integers (.NET 7+)**: `Int128`, `UInt128`
* **Floating-point types**: `float`, `double`
* **Half-precision float (.NET 5+)**: `Half` — 16-bit floating-point
* **Decimal type**: `decimal`
* **Boolean type**: `bool`
* **Structs**: Custom value types (e.g., `DateTime`, `Guid`)
* **Record Structs (C# 10+)**: Immutable value types with value-based equality (e.g., `record struct Point(int X, int Y)`)
* **Enumerations**: `enum`

**2. Reference Types:**

These store references to the actual data:

* **String**: string
* **Objects**: object
* **Arrays**: e.g., int[], string[]
* **Class types**: Custom classes
* **Delegates**
* **Interfaces**

**3. Pointer Types:**

Used in unsafe code for direct memory manipulation (e.g., int*, char*).

**4. Nullable Types:**

Allow value types to represent null (e.g., int?, bool?).

**Example:**

```cs
int number = 10;              // Value type
string name = "Pradeep";      // Reference type
int? age = null;              // Nullable value type
int[] numbers = {10, 20, 30}; // Array (reference type)
nint nativeInt = 42;          // Native-sized integer (C# 9+)
Int128 bigNum = Int128.MaxValue; // 128-bit integer (.NET 7+)
Half half = (Half)3.14f;      // 16-bit float (.NET 5+)
record struct Point(int X, int Y); // Record struct (C# 10+)
Point p = new Point(1, 2);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can primitive data types be stored in heap?

Yes, primitive data types in C# (such as int, float, bool, etc.) are value types and are typically stored on the stack when used as local variables. However, they can be stored on the heap in certain scenarios:

* **When they are part of a reference type** (e.g., fields in a class or elements in an array), the value type is stored on the heap as part of the object.

* **When they are boxed** (i.e., converted to object or an interface type), the value is copied to the heap.

**Example:**

```cs
int x = 10; // Stored on the stack

object obj = x; // Boxing: x is copied to the heap
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. It is possible to store mixed datatypes such as int, string, float, char all in one array?

Yes, you can store multiple data types in a `System.Array` if the array is declared as type `object[]` or `Array` (the base class). This is because all types in C# ultimately derive from `object`. However, this approach loses type safety and requires casting when retrieving values.

**Example:**

```cs
object[] mixedArray = { 10, "Hi", 3.14, true };

foreach (var item in mixedArray)
{
    Console.WriteLine(item);
}
```

**Output:**

```
10
Hi
3.14
True
```

**Note:**

* Arrays like int[], string[], etc., can only store a single data type.
* Using object[] allows mixed types, but it\'s generally better to use generic collections or tuples for type safety and clarity.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is an extension method in C# and how is it implemented?

An extension method adds functionality to an existing type without modifying its source code or creating a subclass. It is defined as a `static` method in a `static` class, with the first parameter prefixed with `this`. C# 14 also introduces **extension members** (a superset of extension methods).

**1. Traditional extension method (all versions):**

```cs
public static class StringExtensions
{
    public static string Reverse(this string input)
    {
        char[] chars = input.ToCharArray();
        Array.Reverse(chars);
        return new string(chars);
    }

    public static bool IsNullOrEmpty(this string? input)
        => string.IsNullOrEmpty(input);
}

// Usage
string text = "Hello";
Console.WriteLine(text.Reverse());       // Output: olleH
Console.WriteLine("".IsNullOrEmpty());   // Output: True
```

**2. Extension methods with generic constraints (C# 11+):**

```cs
public static class NumberExtensions
{
    public static T Clamp<T>(this T value, T min, T max)
        where T : System.Numerics.INumber<T>
        => T.Max(min, T.Min(value, max));
}

Console.WriteLine(15.Clamp(0, 10)); // Output: 10
Console.WriteLine(5.5.Clamp(0.0, 10.0)); // Output: 5.5
```

**3. Extension members (C# 14 — new syntax):**

C# 14 introduces a new `extension` block syntax that allows properties, static methods, and operators as extensions — not just instance methods:

```cs
extension(string s) StringEx
{
    public int WordCount => s.Split(' ').Length;
    public static string Repeat(string text, int times) => string.Concat(Enumerable.Repeat(text, times));
}

string sentence = "Hello World";
Console.WriteLine(sentence.WordCount); // Output: 2 (extension property)
Console.WriteLine(StringEx.Repeat("ha", 3)); // Output: hahaha
```

**Note:** Extension methods are resolved at compile time. They are found only when the namespace of the static class is imported with `using`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a generic type in C# and why is it used?

Generics allow you to write classes, methods, delegates, and interfaces that work with any data type, maintaining compile-time type safety and avoiding boxing/unboxing overhead. With **C# 11 Generic Math**, generic code can now also perform arithmetic operations across numeric types.

**Features:**

* **Type Safety**: Errors are caught at compile time.
* **Code Reusability**: One implementation works for all types.
* **Performance**: No boxing/unboxing — generics use the actual type at runtime.

**1. Generic class:**

```cs
public class Repository<T>
{
    private readonly List<T> _items = new();

    public void Add(T item) => _items.Add(item);
    public IReadOnlyList<T> GetAll() => _items;
}

var repo = new Repository<string>();
repo.Add("Pradeep");
repo.Add("Kumar");
Console.WriteLine(repo.GetAll().Count); // Output: 2
```

**2. Generic method:**

```cs
T Max<T>(T a, T b) where T : IComparable<T>
    => a.CompareTo(b) >= 0 ? a : b;

Console.WriteLine(Max(10, 20));       // Output: 20
Console.WriteLine(Max("apple", "mango")); // Output: mango
```

**3. Generic Math (C# 11+, .NET 7+):**

Write arithmetic algorithms that work over all numeric types using interfaces like `INumber<T>`, `IAdditionOperators<T,T,T>`, etc.

```cs
using System.Numerics;

T Sum<T>(IEnumerable<T> values) where T : INumber<T>
    => values.Aggregate(T.Zero, (acc, v) => acc + v);

Console.WriteLine(Sum(new[] { 1, 2, 3, 4 }));         // Output: 10
Console.WriteLine(Sum(new[] { 1.5, 2.5, 3.0 }));      // Output: 7
Console.WriteLine(Sum(new[] { 1m, 2m, 3.5m }));       // Output: 6.5
```

**4. Generic constraints:**

| Constraint                | Meaning                                        |
|---------------------------|------------------------------------------------|
| `where T : class`         | T must be a reference type                     |
| `where T : struct`        | T must be a value type                         |
| `where T : new()`         | T must have a public parameterless constructor  |
| `where T : IDisposable`   | T must implement `IDisposable`                 |
| `where T : INumber<T>`    | T must be a numeric type (C# 11+)              |
| `where T : notnull`       | T cannot be a nullable type (C# 8+)            |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is an anonymous method in C# and how is it used?

An anonymous method is a named method without a name, defined using the delegate keyword. It\'s used for inline code, particularly when you need to create a delegate instance without explicitly defining a named method first. 

Anonymous methods are helpful when you need a small, one-time-use method and don\'t want to clutter the code with a separate named method. 

**Example:**

```cs
// Declare a delegate
delegate void Greet(string name);

class Program
{
    static void Main()
    {
        // Assign an anonymous method to the delegate
        Greet greet = delegate(string name)
        {
            Console.WriteLine("Hello, " + name + "!");
        };

        greet("Pradeep"); // Output: Hello, Pradeep!
    }
}
```

**Usage:**

* Anonymous methods are commonly used for event handling, callbacks, or passing logic as parameters.
* They can access variables from the enclosing scope (closure).

**Note:**

Lambda expressions are a more concise way to achieve the same functionality as anonymous methods. Lambda expressions are generally preferred over anonymous methods in modern C# code. 

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between a method and a function in C#?

In C#, a function is always part of a class, making it a method. A function is a reusable block of code that can be called to perform a specific task. A method, in object-oriented programming, is associated with an object or class, often operating on the object\'s data or state. 

* **Method**: A block of code that belongs to a class or object and performs a specific action. It can access and modify the state (fields/properties) of the class.

* **Function**: A general term for a block of code that performs a task and returns a value. In C#, standalone functions (not part of a class) do not exist; all functions are methods.

**Example:**

```cs
public class Calculator
{
    // This is a method
    public int Add(int a, int b)
    {
        return a + b;
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the Common Language Runtime (CLR) in C#?

The **Common Language Runtime (CLR)** — known as **CoreCLR** in modern .NET (formerly .NET Core) — is the core execution engine for .NET applications. It manages execution, memory, security, and cross-language interoperability. As of **.NET 10**, CoreCLR is cross-platform (Windows, Linux, macOS, Android, iOS, WebAssembly).

**Key Functions:**

* **Execution and Management:** Manages the full lifecycle of .NET applications, including startup, execution, and shutdown.

* **Memory Management (Garbage Collection):** Automatic, generational GC with **server GC** for high-throughput services and **workstation GC** for desktop apps. .NET 8/9/10 introduce further GC improvements such as the **non-GC heap** (pinned allocations exempt from GC pressure).

* **Just-In-Time (JIT) Compilation:** Converts IL to native machine code at runtime using **Tiered Compilation** and **Dynamic PGO** (Profile-Guided Optimization). Alternatively, **Native AOT** compiles entirely at build time.

* **Security:** Code Access Security was removed in .NET Core. Modern .NET relies on OS-level security, sandboxing, and runtime verification of IL.

* **Exception Handling:** Structured exception handling with `try`/`catch`/`finally`, including `AggregateException` for parallel/async errors.

* **Cross-Language Interoperability:** All .NET languages (C#, F#, VB.NET) share the **Common Type System (CTS)** and compile to IL, enabling seamless interop.

* **Metadata:** Type information embedded in assemblies enables reflection, serialization, and source generators.

**Example — inspecting runtime info (.NET 10):**

```cs
using System.Runtime.InteropServices;

Console.WriteLine(RuntimeInformation.FrameworkDescription); // .NET 10.0.x
Console.WriteLine(RuntimeInformation.OSDescription);        // e.g., Linux 6.x
Console.WriteLine(RuntimeInformation.ProcessArchitecture);  // X64 / Arm64
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of namespaces in C#?

In C#, namespaces primarily serve two crucial purposes: organizing code and preventing naming conflicts. They provide a hierarchical structure for grouping related classes, interfaces, and other types, making code easier to read, maintain, and understand.

**Features:**

* **Avoiding Name Conflicts:** Namespaces help prevent naming collisions by distinguishing between types that may have the same name but are in different namespaces.

* **Code Organization:** They provide a logical structure, making large codebases easier to manage and understand.

* **Improved Readability:** By grouping related functionality, namespaces make code more readable and maintainable.

* **Access Control:** Namespaces can help control the scope of class and method visibility.

**Example:**

```cs
namespace MyCompany.Project.Utilities
{
    public class Logger
    {
        // Logger implementation
    }
}
```

You can then use the using directive to access types within a namespace:

```cs
using MyCompany.Project.Utilities;

Logger logger = new Logger();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `using` statement in C#?

The `using` statement in C# ensures that objects implementing `IDisposable` are properly disposed when they go out of scope, even if an exception is thrown. .NET 10 supports two syntaxes:

**1. Traditional `using` block (all .NET versions):**

```cs
using (var file = new StreamReader("example.txt"))
{
    string content = file.ReadToEnd();
    // file is automatically disposed when the block exits
}
```

**2. `using` declaration (C# 8+, .NET Core 3+):**

No braces needed — the object is disposed at the end of the enclosing scope. This is the preferred modern style.

```cs
using var file = new StreamReader("example.txt");
string content = file.ReadToEnd();
// file is automatically disposed here (end of method/block)
```

**3. `await using` for async disposal (C# 8+):**

For objects implementing `IAsyncDisposable` (e.g., async streams, `HttpClient`, `DbContext`):

```cs
await using var connection = new SqlConnection(connectionString);
await connection.OpenAsync();
// connection is asynchronously disposed at end of scope
```

**Key points:**

* Applies to any type implementing `IDisposable` or `IAsyncDisposable`.
* Prevents resource leaks for files, streams, database connections, HTTP clients, etc.
* `using` declarations (C# 8+) reduce nesting and are generally preferred in modern .NET code.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are properties in C# and how are they used?

Properties in C# are special members that provide a flexible mechanism to read, write, or compute the values of private fields. Modern .NET adds `init`-only properties and `required` properties.

**1. Traditional property with `get`/`set`:**

```cs
public class Person
{
    private string name;

    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    // Auto-implemented property
    public int Age { get; set; }

    // Read-only computed property
    public string Info => $"Name: {Name}, Age: {Age}";
}
```

**2. `init`-only properties (C# 9+):**

Can only be set during object initialization (in constructors or object initializers), making them immutable after construction. Ideal for DTOs and value objects.

```cs
public class Product
{
    public string Name { get; init; }
    public decimal Price { get; init; }
}

var p = new Product { Name = "Laptop", Price = 999.99m };
// p.Name = "Phone"; // Compile error — init-only
```

**3. `required` properties (C# 11+):**

Forces callers to set the property in an object initializer — a compile-time guarantee of initialization.

```cs
public class Employee
{
    public required string Name { get; init; }
    public required int Id { get; init; }
    public string Department { get; init; } = "General";
}

// Must provide Name and Id — compiler enforces it
var emp = new Employee { Name = "Pradeep", Id = 101 };
Console.WriteLine(emp.Info); // Output: Name: Pradeep, Age: 30
```

**4. Primary Constructor properties (C# 12+):**

```cs
public class Point(int x, int y)
{
    public int X { get; } = x;
    public int Y { get; } = y;
    public override string ToString() => $"({X}, {Y})";
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the Arrays in C#.Net?

In C#.NET, an **array** is a data structure that stores a fixed-size sequential collection of elements of the same type. Arrays are used to store multiple values in a single variable, instead of declaring separate variables for each value.

**Key Points:**

- Arrays are zero-indexed (the first element is at index 0).
- All elements must be of the same type.
- The size of an array is specified at the time of its creation and cannot be changed.

**Example:**

```cs
// Declaration
int[] numbers;

// Initialization
numbers = new int[5]; // Array of 5 integers

// Declaration and initialization together
string[] names = { "Alice", "Bob", "Charlie" };
```

**Types of Arrays:**

**1. Single-Dimensional Array:**  
   
A linear array with one row of elements.

```cs
int[] arr = new int[3] { 10, 20, 30 };

foreach(int num in arr) {
    Console.WriteLine(num); // Output: 10, 20, 30
}
```

**2. Multi-Dimensional Array:**  

An array with more than one dimension (e.g., matrix).

```cs
int[,] matrix = new int[2, 3] { {1, 2, 3}, {4, 5, 6} };
```

**3. Jagged Array:**  

An array of arrays, where each inner array can have a different length.

```cs
int[][] jagged = new int[2][];
jagged[0] = new int[3] { 1, 2, 3 };
jagged[1] = new int[2] { 4, 5 };
```

**Example:**

```cs
int[] numbers = { 5, 10, 15 };
for (int i = 0; i < numbers.Length; i++)
{
    Console.WriteLine(numbers[i]);
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between the System.Array.CopyTo() and System.Array.Clone()?

The key difference between `System.Array.CopyTo()` and `System.Array.Clone()` lies in their purpose and how they create a copy of the array. `Array.CopyTo()` copies the elements of an array to a destination array, while `Array.Clone()` creates a new, independent array that is a shallow copy of the original. 

**System.Array.CopyTo():**

- Copies all elements of the current array to another existing array, starting at a specified index in the destination array.
- Requires the destination array to be at least as large as the source array.
- Performs a shallow copy (copies references for reference types, values for value types).

**Example:**

```cs
int[] source = { 10, 20, 30 };
int[] destination = new int[3];

source.CopyTo(destination, 0); // Copy

for (int i = 0; i < destination.Length; i++)
{
    Console.WriteLine(destination[i]); // Output: 10, 20, 30
}
```

**System.Array.Clone():**

- Creates a new array of the same type and length as the original and copies all elements into it.
- Returns an object, so you usually need to cast it to the appropriate array type.
- Also performs a shallow copy.

**Example:**

```cs
int[] source = { 10, 20, 30 };
int[] clone = (int[])source.Clone(); // Clone

for (int i = 0; i < clone.Length; i++)
{
    Console.WriteLine(clone[i]); // Output: 10, 20, 30
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is jagged array in C#.Net?

A jagged array in C#.NET is an array whose elements are arrays themselves, and these inner arrays can have different lengths. It is also known as an "array of arrays." 

Unlike a multidimensional array (e.g., `int[,]`), a jagged array allows each row to have a different number of columns.

**Example:**

```cs
// Declare a jagged array with 2 rows
int[][] jaggedArray = new int[2][];

// Initialize each row with different lengths
jaggedArray[0] = new int[] { 10, 20, 30 };
jaggedArray[1] = new int[] { 40, 50 };

// Accessing elements
Console.WriteLine(jaggedArray[0][1]); // Output: 20
Console.WriteLine(jaggedArray[1][0]); // Output: 40
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can you sort the elements of the array in descending order?

To sort the elements of an array in descending order in C#, you can use the Array.Sort method with a custom comparer, or use LINQ for a more concise approach.

**Example:** Using `Array.Sort` with a comparer

```cs
int[] numbers = { 5, 2, 8, 1, 3 };
Array.Sort(numbers, (a, b) => b.CompareTo(a)); // Sorts in descending order

foreach (int num in numbers)
{
    Console.WriteLine(num); // Output: 8 5 3 2 1
}
```

**Example:** Using LINQ

```cs
int[] numbers = { 5, 2, 8, 1, 3 };
var descending = numbers.OrderByDescending(n => n).ToArray();

foreach (int num in descending)
{
    Console.WriteLine(num); // Output: 8 5 3 2 1
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `var` and `dynamic` types?

The `var` and `dynamic` are keywords used for declaring variables, but they differ significantly in their approach to type checking and type inference. 

`var` is used for implicit typing, where the compiler infers the variable\'s type from the initializer value, while `dynamic` is a type itself that bypasses compile-time type checking, making type verification occur at runtime. 

**`var` (Implicit Typing):**

* **Type inference**: The type of a `var` variable is determined by the compiler at compile time, based on the assigned value.
* **Type safety**: Once assigned, the type cannot change, and all type checks are performed at compile time.
* **Usage**: Useful for anonymous types or when the type is obvious from the right-hand side.

**Example:**

```cs
var number = 10;        // number is inferred as int
var name = "Pradeep";   // name is inferred as string
// number = "Kumar";    // Compile-time error: cannot assign string to int
```

**`dynamic` (Dynamic Typing):**

* **Runtime type resolution**: The type of a dynamic variable is determined at runtime, not at compile time.
* **No compile-time type checking**: Errors related to type usage are only detected at runtime.
* **Flexibility**: Allows operations that may not be valid at compile time, but can cause runtime exceptions if used incorrectly.
* **Usage**: Useful when working with COM objects, dynamic languages, or reflection.

**Example:**

```cs
dynamic value = 10;
value = "Pradeep";     // Allowed: type can change at runtime

Console.WriteLine(value.NonExistentMethod()); // Compiles, but throws runtime exception if method doesn\'t exist
```

**Key Differences:**

|Feature	    |var	            |dynamic
|---------------|-------------------|----------------
|Type Checking	|Compile time	    |Runtime
|Type Determination	|Inferred from initializer	|Determined at runtime
|Type Changes	|Fixed after initialization	    |Allowed to change at runtime
|Error Detection   |Compile time| Runtime

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a `struct` in C#?

A `struct` (structure) is a value type that groups related variables into a single unit. It is stored on the stack (unless boxed or embedded in a reference type). Modern C# introduces **record structs** for immutable value objects.

**Differences from Classes:**

* **Value Type:** Assignment copies all fields; changes to a copy do not affect the original.
* **No Inheritance:** Structs cannot inherit from other structs or classes (only interfaces).
* **Immutability:** Use `readonly struct` to guarantee all fields are read-only.

**1. Traditional struct:**

```cs
public struct Point
{
    public int X;
    public int Y;

    public Point(int x, int y) { X = x; Y = y; }
}

Point p1 = new Point(10, 20);
Point p2 = p1; // copy
p2.X = 30;
Console.WriteLine(p1.X); // Output: 10 (unchanged)
Console.WriteLine(p2.X); // Output: 30
```

**2. `readonly struct` (C# 7.2+):**

All members must be read-only. The compiler enforces immutability and enables performance optimizations (avoids defensive copies).

```cs
public readonly struct Temperature(double celsius)
{
    public double Celsius { get; } = celsius;
    public double Fahrenheit => Celsius * 9 / 5 + 32;
    public override string ToString() => $"{Celsius}°C";
}
```

**3. `record struct` (C# 10+):**

Combines the value semantics of a struct with the conciseness of records. Provides value-based equality and a `ToString()` override automatically.

```cs
public record struct Coordinate(double Latitude, double Longitude);

var c1 = new Coordinate(12.97, 77.59);
var c2 = new Coordinate(12.97, 77.59);

Console.WriteLine(c1 == c2);   // True (value-based equality)
Console.WriteLine(c1);         // Output: Coordinate { Latitude = 12.97, Longitude = 77.59 }

// Non-destructive mutation with 'with'
var c3 = c1 with { Latitude = 28.61 };
```

**4. `readonly record struct` (C# 10+):**

Fully immutable value type with value equality — preferred for small, data-centric types.

```cs
public readonly record struct Money(decimal Amount, string Currency);

var price = new Money(9.99m, "USD");
var discounted = price with { Amount = 7.99m };
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `abstract` and `virtual` methods?

Abstract methods are declared without an implementation, requiring derived classes to provide one. Virtual methods, on the other hand, come with a default implementation that can be overridden by derived classes. 

**Abstract Methods:**

* An abstract method is declared in an abstract class, meaning it doesn\'t have a concrete implementation. 
* Abstract classes, by definition, cannot be instantiated directly. 
* Derived classes that inherit from an abstract class must provide a concrete implementation for all abstract methods to be instantiated. 
* This forces derived classes to implement specific behavior related to the abstract method.

**Virtual Methods:**

* A virtual method has a default implementation in the base class. 
* Derived classes can choose to override the virtual method, providing their own implementation. 
* If a derived class does not override a virtual method, the base class\'s implementation will be used. 
* Virtual methods enable polymorphism, allowing different object types to respond to the same method call in different ways. 

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `out` and `ref` parameters?

The `out` and `ref` keywords are both used to pass arguments by reference to methods, but they have important differences:

**ref parameter:**

* The variable passed as `ref` must be initialized before it is passed to the method.
* The method can read and modify the value.
* Changes made to the parameter inside the method are reflected outside.

**out parameter:**

* The variable passed as `out` does not need to be initialized before being passed.
* The method must assign a value to the `out` parameter before the method returns.
* Used when a method needs to return multiple values.

**Example:**

```cs
public class Program
{
    static void RefExample(ref int x)
    {
      x = x + 10;
    }

    static void OutExample(out int y)
    {
      y = 20; // Must assign before returning
    }

	public static void Main(string[] args)
	{

      // Using 'ref'
      int a = 5;
      RefExample(ref a); // a is now 15

      // Using 'out'
      int b;
      OutExample(out b); // b is now 20

      Console.WriteLine($"a = {a}"); // Output: a = 15
      Console.WriteLine($"b = {b}"); // Output: b = 20
	}
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a `Tuple` in C#?

A Tuple is a data structure that stores a fixed number of elements of potentially different types in a single object. Modern C# uses **value tuples** (C# 7+) with named elements, deconstruction, and `with` expressions.

**Features:**

* Returns multiple values from a method without `out` parameters or a custom class.
* Supports named elements for better readability.
* Supports **deconstruction** to unpack values into individual variables.
* Value tuples (C# 7+) are structs — more performant than old `Tuple<T>` class (heap-allocated).

**1. Named tuple elements (preferred):**

```cs
(string Name, int Age, bool IsEmployed) GetPerson()
{
    return ("Pradeep", 28, true);
}

var person = GetPerson();
Console.WriteLine(person.Name);       // Output: Pradeep
Console.WriteLine(person.Age);        // Output: 28
Console.WriteLine(person.IsEmployed); // Output: True
```

**2. Deconstruction (C# 7+):**

```cs
var (name, age, isEmployed) = GetPerson();
Console.WriteLine($"{name} is {age} years old."); // Output: Pradeep is 28 years old.
```

**3. Inline tuple:**

```cs
var point = (X: 10, Y: 20);
Console.WriteLine($"X={point.X}, Y={point.Y}"); // Output: X=10, Y=20
```

**4. Tuple in LINQ (C# 12 collection expressions):**

```cs
var employees = new[]
{
    (Name: "Alice", Dept: "Engineering"),
    (Name: "Bob",   Dept: "Marketing"),
};

foreach (var (name, dept) in employees)
    Console.WriteLine($"{name} – {dept}");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain byval and byref?

In C#, **byval** (by value) and **byref** (by reference) describe how arguments are passed to methods:

**By Value (byval):**

- The method receives a **copy** of the variable\'s value.
- Changes made to the parameter inside the method **do not affect** the original variable.
- This is the default behavior for method parameters in C#.

**Example:**

```cs
void Increment(int x)
{
    x = x + 1;
}

int a = 10;
Increment(a);
Console.WriteLine(a); // Output: 10 (original value unchanged)
```

**By Reference (byref):**

The method receives a reference to the original variable.
Changes made to the parameter affect the original variable.
In C#, use the ref or out keyword to pass by reference.

**Example:**

```cs
void Increment(ref int x)
{
    x = x + 1;
}

int a = 10;
Increment(ref a);
Console.WriteLine(a); // Output: 11 (original value changed)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is an immutable string?

An **immutable string** in C# is a string whose value cannot be changed after it is created. When you modify a string (such as by concatenation, replacement, or other operations), a new string object is created in memory, and the original string remains unchanged.

**Why are strings immutable?:**

* **Thread Safety:** Immutable objects are inherently-safe because their state can\'t change after creation.
* **Security:** Immutability ensures that once a string is created, it can not be tempered with, reducing the risk of injection attacks. For example security sensitive operation (e.g., file paths, URLs, Database queries).
* **Hashing and Performance:** Since their value doesn\'t change, hash code remain constant, which is crucial for consistent lookups.

**Example:**

```cs
string s1 = "Hello";
string s2 = s1;
s1 = s1 + " World"; // Creates a new string, s1 now points to "Hello World"
Console.WriteLine(s2); // Output: Hello (s2 is unchanged)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the JIT compiler process?

The JIT (Just-In-Time) compiler is a core component of the .NET runtime (CLR/CoreCLR) that converts Intermediate Language (IL) code into native machine code at runtime, just before execution.

**JIT Compilation Process:**

1. **Source → IL:** The C# compiler (`csc` / `dotnet build` using **Roslyn**) compiles source code into **Intermediate Language (IL)** and stores it in assemblies (`.dll` / `.exe`).
2. **Assembly Loading:** The CoreCLR loads the required assemblies at startup.
3. **JIT Compilation:** When a method is called for the first time, the JIT compiler translates its IL to **native machine code** optimized for the current CPU (x64, Arm64, etc.).
4. **Caching:** The native code is cached in memory so subsequent calls execute directly without re-compilation.
5. **Execution:** The CPU runs the native code.

```
Source Code (.cs)
      ↓  Roslyn compiler
IL Code + Metadata (Assembly .dll/.exe)
      ↓  CoreCLR loads assembly
JIT Compiler (per method, first call only)
      ↓
Native Machine Code (cached)
      ↓
Execution by CPU
```

**.NET JIT improvements (.NET 8/9/10):**

* **Tiered Compilation (default on):** Methods start with quick-tier-0 code, then are recompiled with full optimizations (tier-1) if called frequently.
* **Dynamic PGO (Profile-Guided Optimization):** The JIT uses runtime profiling data to make smarter inlining and de-virtualization decisions automatically.
* **AVX-512 / SIMD support:** On .NET 9+, the JIT emits SIMD vector instructions for hardware-accelerated math.

**Alternative: Native AOT (Ahead-of-Time Compilation, .NET 7+):**

Native AOT compiles the entire application to a **self-contained native binary** at build time — no JIT, no .NET runtime required at deployment.

```bash
dotnet publish -r linux-x64 -p:PublishAot=true
```

**Benefits of Native AOT:**
* Instant startup (no JIT warm-up)
* Smaller memory footprint
* Suitable for serverless functions, CLI tools, and containers

```cs
// Program.cs — minimal Native AOT app (.NET 10)
Console.WriteLine("Hello from Native AOT!");
```

**When to use JIT vs. Native AOT:**

| Scenario                   | JIT (Default)     | Native AOT              |
|----------------------------|-------------------|-------------------------|
| Long-running services      | ✅ Preferred       | ✅ Supported             |
| Cold-start sensitive apps  | ⚠️ Warm-up delay  | ✅ Instant start         |
| Reflection-heavy code      | ✅ Full support    | ⚠️ Limited              |
| Smallest binary size       | ⚠️ Runtime needed | ✅ Single file           |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain the characteristics of value-type variables that are supported in the C# programming language.

In C#, value-type variables hold their data directly in memory, unlike reference types which store a reference to the data. When assigned, value-type variables create a copy of the data, meaning changes to one variable do not affect others, except in cases where `ref` or `out` modifiers are used. 

**Characteristics:**

- **Direct Storage:** Value types store their data directly, not as a reference to another memory location.
- **Stack Allocation:** Most value-type variables are allocated on the stack (unless they are part of a reference type or boxed), which allows for fast allocation and deallocation.
- **No Null by Default:** Value types cannot be null unless they are declared as nullable (e.g., `int?`).
- **Copy Semantics:** Assigning one value-type variable to another copies the value, not a reference. Changes to one variable do not affect the other.
- **Predefined and User-Defined:** Value types include built-in types (such as `int`, `float`, `bool`, `char`, `struct`, and `enum`) and user-defined structs and enums.
- **No Inheritance:** Value types cannot inherit from other types (except for interfaces), and they are implicitly sealed.
- **Default Values:** Value types always have a default value (e.g., `0` for numeric types, `false` for `bool`).
- **Boxing and Unboxing:** Value types can be "boxed" to be treated as objects (stored on the heap), and "unboxed" back to value types.

**Example:**

```cs
int number = 42;           // Integral value type
double price = 19.99;      // Floating-point value type
bool isActive = true;      // Boolean value type
char letter = 'A';         // Character value type
DateTime today = DateTime.Now; // Struct (user-defined value type)
```

**Summary Table:**

| Feature                | Value Type Example | Behavior                                      |
|------------------------|-------------------|-----------------------------------------------|
| Storage                | `int x = 5;`      | Stores value directly in variable             |
| Assignment             | `int y = x;`      | Copies value, not reference                   |
| Nullability            | `int? z = null;`  | Nullable only with `?` syntax                 |
| Default Value          | `int x;`          | Defaults to `0`                               |
| Inheritance            | `struct`          | Cannot inherit from another struct/class      |


<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a parameter? Explain the new types of parameters introduced in C# 4.0.

A **parameter** in C# is a variable defined in a method, constructor, or indexer declaration that receives a value (called an argument) when the method is called. Parameters allow you to pass data into methods so they can operate on different values.

C# 4.0 introduced two important features related to method parameters:

**1. Optional Parameters:**

* You can specify default values for parameters in a method declaration.
* If the caller omits an argument, the default value is used

**Example:**

```cs
void PrintMessage(string message, int repeat = 1)
{
    for (int i = 0; i < repeat; i++)
        Console.WriteLine(message);
}

PrintMessage("Hello");      // Uses default repeat = 1
PrintMessage("Hi", 3);      // repeat = 3

// Output
// Hello
// Hi Hi Hi
```

**2. Named Parameters**

* You can specify arguments by parameter name, regardless of their position.
* This improves readability and allows you to skip optional parameters.

**Example:**

```cs
void PrintMessage(string name, int age = 0, string city = "Unknown")
{
    Console.WriteLine($"{name}, {age}, {city}");
}

PrintMessage("Pradeep", city: "Bengaluru"); // age uses default value 0

// Output
// Pradeep, 0, Bengaluru
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the different types of literals?

In C#, **literals** are fixed values assigned directly to variables or constants in code. They represent constant values of various data types.

**Types of literals in C#:**

**1. Integer Literals**
  - Represent whole numbers.
  - Examples: `10`, `-42`, `0xFF` (hexadecimal), `0b1010` (binary), `123U` (unsigned), `123L` (long).
  - Suffixes: `U` (unsigned), `L` (long), `UL` (unsigned long).

**2. Floating-Point Literals**
  - Represent real numbers (with decimals).
  - Examples: `3.14`, `2.5e2` (scientific notation), `1.5F` (float), `2.7D` (double), `1.2M` (decimal).
  - Suffixes: `F` or `f` (float), `D` or `d` (double), `M` or `m` (decimal).

**3. Character Literals**
  - Represent a single Unicode character, enclosed in single quotes.
  - Examples: `'A'`, `'\n'`, `'\u0041'`.

**4. String Literals**
  - Represent a sequence of characters, enclosed in double quotes.
  - Examples: `"Hello"`, `"C#\nBasics"`.
  - **Verbatim string literals**: Start with `@` — preserve escape sequences and line breaks, e.g., `@"C:\Users\Name"`.
  - **Interpolated strings (C# 6+)**: Prefix with `$` — embed expressions, e.g., `$"Hello, {name}!"`.
  - **Raw string literals (C# 11+)**: Start and end with at least three double quotes `"""..."""`. No escape sequences needed, multi-line friendly.

```cs
// Raw string literal (C# 11+)
string json = """
    {
        "name": "Pradeep",
        "age": 30
    }
    """;

// Raw interpolated string
string name = "Pradeep";
string greeting = $"""Hello, {name}! Welcome to "C# 14".""";
```

  - **UTF-8 string literals (C# 11+)**: Suffix with `u8` — produces a `ReadOnlySpan<byte>` for zero-copy UTF-8 data, ideal for networking and file I/O.

```cs
ReadOnlySpan<byte> utf8Hello = "Hello"u8;
```

**5. Boolean Literals**
  - Represent logical values.
  - Only two possible values: `true` and `false`.

**6. Null Literal**
  - Represents a null reference.
  - Only one value: `null`.

**Examples:**

```cs
int age = 25;                // Integer literal
double pi = 3.14159;         // Floating-point literal
char letter = 'A';           // Character literal
string name = "Alice";       // String literal
bool isActive = true;        // Boolean literal
object obj = null;           // Null literal
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the main difference between sub-procedure and function?

In C#, the main difference between a subroutine (which can be a Sub procedure in some languages) and a function is that a function returns a value, while a subroutine (or sub procedure) does not. 

Both perform actions, but functions allow you to use their result elsewhere in your code, while subroutines/sub procedures simply execute and return control. 

**Difference :**

* **Function:** Returns a value to the caller. In C#, this is a method with a non-void return type.
* **Sub-procedure:** Does not return a value. In C#, this is a method with a void return type.

**Example:**

```cs
// Function: returns an int
int Add(int a, int b)
{
    return a + b;
}

// Sub-procedure: returns nothing (void)
void PrintSum(int a, int b)
{
    Console.WriteLine(a + b);
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between string and StringBuilder in C#?

In C#, `string` and `StringBuilder` both handle text, but string is immutable, and StringBuilder is mutable. This means that when you modify a string, a new string object is created, while with StringBuilder, you can modify the object in place without creating new objects. 

**1. string:**

- **Immutable:** Once created, a string cannot be changed. Any operation that appears to modify a string (such as concatenation or replacement) actually creates a new string object in memory.

- **Performance:** Frequent modifications (like concatenation in loops) can lead to performance issues due to repeated allocations and garbage collection.

- **Usage:** Best for scenarios where the text does not change often.

**Example:**

```cs
string s = "Hello";
s += " World"; // Creates a new string object
```

**2. StringBuilder:**

* **Mutable:** Designed for scenarios where you need to modify the text repeatedly. Changes are made to the same object, avoiding unnecessary allocations.

* **Performance:** More efficient for repeated modifications, such as appending or inserting text in loops.

* **Usage:** Recommended when building or modifying large or dynamic strings.

**Example:**

```cs
using System.Text;

StringBuilder sb = new StringBuilder("Hello");
sb.Append(" World"); // Modifies the existing object
string result = sb.ToString();
```

**When to use which?**

* Use `string` for simple, infrequent changes.
* Use `StringBuilder` for complex or repeated string manipulations, especially in loops.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is difference between late binding and early binding in C#?

In C#, early binding (also known as static binding) resolves method calls at compile time, while late binding (also known as dynamic binding) resolves method calls at runtime. 

Early binding offers better performance and type safety due to compile-time checking, while late binding provides more flexibility but with potential runtime overhead, especially when using reflection, according to various sources. 

**Early Binding:**

* **Compilation Time:** The type of object and method to be called is determined at compile time.
* **Type Safety:** The compiler performs type checking during compilation, catching errors early. 
* **Performance:** Generally faster due to direct method resolution and less overhead at runtime. 

**Example:** 

```cs
// Early binding example
MyClass obj = new MyClass();
obj.MyMethod(); // Compiler knows about MyMethod at compile time
```

**Late Binding:**

* **Compilation Time:** The method or property to be invoked is determined at runtime.
* **Type Safety:** Less type safety, more flexible, but slower due to runtime checks.
* **Performance:** Potentially slower due to runtime lookups and potential overhead, especially with reflection. 

**Example:**

```cs
// Late binding using dynamic
dynamic obj = GetSomeObject();
obj.MyMethod(); // Resolved at runtime
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Indexer in C#?

An indexer in C# is a special type of property that allows objects of a class or struct to be indexed just like arrays, using the square bracket `[]` syntax. Indexers enable you to access elements in an object using an index, making custom classes behave like collections.

**Example:**

```cs
public class SampleCollection
{
    private string[] data = new string[5];

    // Indexer declaration
    public string this[int index]
    {
        get { return data[index]; }
        set { data[index] = value; }
    }
}

// Usage
var collection = new SampleCollection();
collection[0] = "Hello";
Console.WriteLine(collection[0]); // Output: Hello
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the differences between Object, Var and Dynamic type?

In C#, object, var and dynamic are three different ways to declare variables, each with distinct behaviors and use cases.

**1. `object`**

- **Description:** The base type of all types in C#. Any type (value or reference) can be assigned to an `object` variable.
- **Type Checking:** Compile-time type is always `object`. You must cast to the actual type to access members.
- **Type Safety:** Type checking is enforced at compile time, but you need explicit casting to use specific members.

**Example:**

```cs
object obj = "Hello";
// Console.WriteLine(obj.Length); // Error: 'object' does not contain 'Length'
Console.WriteLine(((string)obj).Length); // OK after casting
```

**2. `var`:**

* **Description:** Enables implicit typing. The compiler infers the type from the right-hand side at compile time.
* **Type Checking:** Strongly typed at compile time. After initialization, the type cannot change.
* **Type Safety:** Fully type-safe; errors are caught at compile time.

**Example:**

```cs
var message = "Hello"; // message is string
// message = 123; // Error: cannot assign int to string
Console.WriteLine(message.Length); // OK
```

**3. `dynamic`:**

* **Description:** Introduced in C# 4.0. Type checking is deferred until runtime.
* **Type Checking:** No compile-time checking for member access; all checks happen at runtime.
* **Type Safety:** Not type-safe; runtime errors may occur if members do not exist.

**Example:**

```cs
dynamic value = "Hello";
Console.WriteLine(value.Length); // OK at runtime
value = 123;
// Console.WriteLine(value.Length); // Runtime error: 'int' does not contain 'Length'
```

**Differences:** 

| Feature         | object           | var                | dynamic           |
|-----------------|------------------|--------------------|-------------------|
| Type Resolution | Compile time     | Compile time (inferred) | Runtime      |
| Type Safety     | Yes (with casting) | Yes              | No                |
| Flexibility     | High (but verbose) | Medium           | Highest           |
| Member Access   | Requires casting | Direct (after inference) | Direct (runtime) |
| Use Case        | General base type, APIs | When type is obvious or anonymous types | Interop, dynamic scenarios |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between managed and unmanaged code?

Managed code is code that runs under the control of the .NET Common Language Runtime (CLR). The CLR provides services such as automatic memory management (garbage collection), type safety, exception handling, and security. Examples include C# and VB.NET code compiled for the .NET runtime.

Unmanaged code is code that runs directly on the operating system, outside the control of the CLR. It is responsible for its own memory management and resource cleanup. Examples include code written in C or C++ and compiled to native machine code, as well as COM components and Win32 API calls.

**Key Differences:**

| Managed Code                          | Unmanaged Code                      |
|---------------------------------------|-------------------------------------|
| Runs under CLR (.NET runtime)         | Runs directly on OS                 |
| Automatic memory management (GC)      | Manual memory management            |
| Type safety and security checks       | No built-in type safety             |
| Exception handling by CLR             | Must handle exceptions manually     |
| Platform-independent (via CLR)        | Platform-dependent                  |

**Summary:**  

Managed code is safer and easier to maintain, while unmanaged code offers more control and performance but requires careful resource management.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is an Object Pool in C#?

An **Object Pool** in C# is a creational design pattern that improves performance by reusing objects instead of repeatedly creating and destroying them. It involves maintaining a pool of pre-initialized objects, readily available for use when needed. This reduces memory allocation and garbage collection overhead, leading to faster execution, especially when dealing with frequently created and destroyed objects.

**Benefits:**

- Reduces the overhead of frequent object creation and garbage collection.
- Useful for objects like database connections, threads, or large memory buffers.
- Helps improve performance and resource utilization.

**Example:**

```cs
// Simple generic object pool example
public class ObjectPool<T> where T : new()
{
    private readonly Stack<T> _objects = new Stack<T>();

    public T GetObject()
    {
        return _objects.Count > 0 ? _objects.Pop() : new T();
    }

    public void ReturnObject(T item)
    {
        _objects.Push(item);
    }
}

// Usage
var pool = new ObjectPool<StringBuilder>();
StringBuilder sb = pool.GetObject();
sb.Append("Hello, Object Pool!");
pool.ReturnObject(sb);
```

**.NET Built-in Support:**  

.NET Core provides `System.Buffers.ObjectPool<T>` and `ArrayPool<T>` for pooling arrays and other objects.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain the difference between lazy and eager evaluation in C#?

In C#, lazy evaluation defers the execution of code until its result is actually needed, while eager evaluation executes code immediately upon encountering it

**1. Eager Evaluation:** Values or expressions are computed immediately when they are assigned or called.

**Example:** Standard variable assignments and most method calls.
 
```cs
int x = GetValue(); // GetValue() is called immediately
```

- **Pros:** Simple and predictable; useful when you always need the value.
- **Cons:** Can waste resources if the value is expensive to compute and not always needed.

**2. Lazy Evaluation:** Computation is deferred until the value is actually needed (accessed for the first time).

**Example:** Using `Lazy<T>`, `IEnumerable<T>` with `yield return`, or LINQ queries.

```cs
Lazy<int> lazyValue = new Lazy<int>(() => GetValue());
// GetValue() is not called until lazyValue.Value is accessed
```

```cs
IEnumerable<int> GetNumbers()
{
    yield return 1;
    yield return 2;
}
// Numbers are generated as you iterate
```

- **Pros:** Improves performance and resource usage when the value may not be needed.
- **Cons:** Can make debugging harder; deferred exceptions.

**Difference**

| Aspect           | Eager Evaluation           | Lazy Evaluation                |
|------------------|---------------------------|-------------------------------|
| When evaluated   | Immediately               | On first use (on demand)      |
| Example          | `int x = GetValue();`     | `Lazy<int> x = ...;`          |
| Use cases        | Always-needed values      | Expensive/optional values     |
| LINQ             | `.ToList()` (immediate)   | `.Where()` (deferred)         |


<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Mention the two major categories that distinctly classify the variables of C# programs.

In C# programs, variables are primarily categorized into **value types** and **reference types**. Value types directly store the variable\'s value in memory, while reference types store a memory address (reference) to the value\'s location. 

**Value Types:**

These store the actual data directly in the memory location of the variable (Stack). Examples include `int`, `bool`, `float`, `enum`, and `struct` types. When a value type variable is copied, a new copy of the data is created, so changes to one variable don\'t affect others.

**Reference Types:**

These store a memory address to the location where the actual data is stored (Heap). Examples include `string`, `object`, `array`, and `class` types. When a reference type variable is copied, the copy contains the same memory address, meaning both variables point to the same data. Therefore, changes to the data through one reference will be reflected in the other. 

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is checked and unchecked block?

In C#, the `checked` and `unchecked` blocks are used to control how the runtime handles arithmetic overflow for integral types (like `int`, `long`, etc.).

- **checked block:** Forces the runtime to throw an `OverflowException` if an arithmetic operation results in a value outside the range of the data type.

- **unchecked block:** Suppresses overflow checking, so if an overflow occurs, the result wraps around (default behavior in most cases).

**Example:**

```cs
int max = int.MaxValue;

try
{
    // Checked block: will throw OverflowException
    checked
    {
        int result = max + 1;
    }
}
catch (OverflowException)
{
    Console.WriteLine("Overflow detected!");
}

// Unchecked block: will not throw, wraps around
unchecked
{
    int result = max + 1;
    Console.WriteLine(result); // Output: -2147483648
}
```

**When to use:**

- Use `checked` when you want to ensure that overflows are caught and handled.
- Use `unchecked` when performance is critical and you are sure overflows are not an issue.

You can also use the `checked` and `unchecked` keywords as expressions:

```cs
int result = checked(max + 1);    // Throws OverflowException
int result2 = unchecked(max + 1); // Wraps around
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between typeOf() and sizeOf()?

In C#, `typeof()` and `sizeof()` are two different operators used for different purposes:

**1. `typeof()` Operator:**

- Returns the `System.Type` object for a given type.
- Used to get metadata information about a type at compile time.
- Commonly used with reflection.

**Example:**

```cs
Type t = typeof(int); // Gets the Type object for int
Console.WriteLine(t.FullName); // Output: System.Int32
```

**2. `sizeof()` Operator:**

- Returns the size (in bytes) of a value type.
- Used to determine how much memory a type occupies.
- Only works with primitive types (like int, char, float, etc.) unless used in an unsafe context.

**Example:**

```cs
int size = sizeof(int); 
Console.WriteLine(size); // Output: 4
```

**Summary Table:**

| Operator | Purpose                        | Returns         | Usage Example        |
|----------|--------------------------------|-----------------|----------------------|
| typeof   | Get type metadata              | Type object     | typeof(int)          |
| sizeof   | Get size in bytes (value types)| Integer (bytes) | sizeof(int)          |

**Note:**  

- `typeof()` works for all types (value and reference).
- `sizeof()` works only for value types and may require `unsafe` context for custom structs.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is widening and Narrowing conversion in C#?

Widening and narrowing conversions in C# refer to how values are converted between different data types, especially numeric types.

**Widening Conversion (Implicit Conversion):**

- Converts a value to a larger or more general type.
- No data loss; safe and automatic.

**Example:** `int` to `long`, `float` to `double`.

```cs
int a = 100;
long b = a;      // Widening: int to long (implicit)
float f = a;     // Widening: int to float (implicit)

Console.WriteLine(b); // Output: 100
Console.WriteLine(f); // Output: 100
```

**Narrowing Conversion (Explicit Conversion):**

- Converts a value to a smaller or more specific type.
- May cause data loss or overflow; requires explicit cast.

**Example:** `double` to `int`, `long` to `short`.

```cs
double x = 123.45;
int y = (int)x;   // Narrowing: double to int (explicit), y = 123

long big = 1000;
short small = (short)big; // Narrowing: long to short (explicit)

Console.WriteLine(y); // Output: 123
Console.WriteLine(small); // Output: -31072
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How to view an Assembly?

To view an assembly in C#, you can inspect its metadata, types, and IL code using several tools:

**1. Using ILDASM (IL Disassembler):**

ILDASM is a tool provided with the .NET SDK to view the contents of an assembly (DLL or EXE).

**Steps:**

1. Open the Developer Command Prompt for Visual Studio.
2. Run:
```cs
ildasm YourAssembly.dll
```
3. The ILDASM window will open, allowing you to browse namespaces, classes, methods, and view IL code.

**2. Using dotPeek or ILSpy (Third-Party Tools):**

- [dotPeek](https://www.jetbrains.com/decompiler/) and [ILSpy](https://github.com/icsharpcode/ILSpy) are free .NET decompilers.
- Open your `.dll` or `.exe` file in these tools to view C# code, metadata, and resources.

**3. Using Visual Studio:**

- Right-click on a reference in Solution Explorer → "Go to Definition" to view metadata.
- Use "Object Browser" (View → Object Browser) to explore assemblies.

**4. Using Reflection in Code:**

You can use reflection to inspect an assembly programmatically:

```cs
using System;
using System.Reflection;

class Program
{
    static void Main()
    {
        Assembly asm = Assembly.LoadFrom("MyAssembly.dll");
        foreach (Type type in asm.GetTypes())
        {
            Console.WriteLine(type.FullName);
        }
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are MultiLingual Applications?

MultiLingual Applications are software applications designed to support multiple languages, allowing users to interact with the application in their preferred language. In C#, this is typically achieved using resource files (.resx) and the .NET localization framework.

**Key Points:**

- **Localization:** Adapting the application for different languages and regions (e.g., translating UI text, formatting dates/numbers).
- **Resource Files:** Store language-specific strings and resources in separate .resx files (e.g., `Resources.en.resx`, `Resources.fr.resx`).
- **Culture Settings:** The application detects or allows the user to select their culture (language/region), and loads the appropriate resources at runtime.
- **.NET Support:** .NET provides classes like `ResourceManager` and `CultureInfo` to manage localization.

**How it works:**

- Text and UI strings are stored in resource files for each supported language.
- The application loads the appropriate resource file based on the user\'s culture or language preference.
- .NET provides classes like `ResourceManager` and `CultureInfo` to facilitate localization.

**Example:**

Suppose you have two resource files:

- `Resources.en.resx` (for English)
- `Resources.fr.resx` (for French)

You can load the correct string at runtime:

```cs
using System.Globalization;
using System.Resources;

ResourceManager rm = new ResourceManager("Namespace.Resources", typeof(Program).Assembly);
CultureInfo ci = new CultureInfo("fr"); // or "en"

string greeting = rm.GetString("Greeting", ci);

Console.WriteLine(greeting); // Output depends on selected culture
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you describe the process of code compilation in .NET?

In .NET, code compilation involves several stages, including translating source code into Common Intermediate Language (CIL) and then executing it using the Common Language Runtime (CLR) with the Just-In-Time (JIT) compiler.

The compiler (like Roslyn for C#) initially converts the high-level source code into CIL, a CPU-independent set of instructions. This CIL code, along with metadata, is stored in an assembly (PE format). When the program runs, the CLR uses the JIT compiler to convert the CIL into machine code, which is then executed on the specific CPU. 

**Process of Code Compilation**

1. **Source Code to Intermediate Language (IL):**
   - The **Roslyn compiler** (`dotnet build` / `csc`) compiles C# source code into **Common Intermediate Language (CIL/IL)**.
   - The compiled IL, along with metadata, is stored in assemblies (`.dll` or `.exe`).
   - In .NET 10, the compiler supports C# 14 features such as the `field` keyword, extension members, and `params` enhancements.

2. **Assembly Loading:**
   - The **CoreCLR** runtime loads required assemblies when the application starts.

3. **Just-In-Time (JIT) Compilation:**
   - The JIT compiler translates IL to native machine code method-by-method on first call.
   - **.NET 8/9/10 improvements:** Tiered Compilation and Dynamic PGO recompile hot methods with full optimizations at runtime automatically.

4. **Native AOT (Ahead-of-Time, .NET 7+):**
   - With `PublishAot=true`, the entire app is compiled to a self-contained native binary at build time — no JIT or runtime required at deployment.

5. **Execution:**
   - The CPU executes the native code. JIT-compiled code is cached for subsequent calls.

**Summary Diagram:**

```
Source Code (C# 14)
      ↓  Roslyn Compiler
IL Code + Metadata (Assembly)
      ↓  CoreCLR loads assembly
      ├─── JIT (default): method-by-method → native code (cached, PGO-optimized)
      └─── Native AOT: whole-app → single native binary (no runtime needed)
                ↓
           Execution by CPU
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you return multiple values from a function in C#?

Yes, you can return multiple values from a function in C#. There are several common ways to achieve this:

**1. Using Tuples (preferred, C# 7+):**

Tuples allow you to return multiple values of different types in a single return statement.

```cs
(string Name, int Age) GetPerson()
{
    return ("Pradeep", 30);
}

// Usage
var person = GetPerson();
Console.WriteLine(person.Name); // Pradeep
Console.WriteLine(person.Age);  // 30

// Deconstruction (C# 7+)
var (name, age) = GetPerson();
Console.WriteLine($"{name} is {age}"); // Pradeep is 30
```

**2. Using Out Parameters:**

The Out parameters allow a function to modify the values of variables passed as arguments. This is a way to "return" additional values indirectly.

```cs
void GetValues(out int a, out int b)
{
    a = 10;
    b = 20;
}

int x, y;
GetValues(out x, out y);
Console.WriteLine(x); // 10
Console.WriteLine(y); // 20
```

**3. Using a Custom Class or Struct**

You can define a custom class or struct to encapsulate multiple values and return an instance of that type

```cs
class Result
{
    public int Sum { get; set; }
    public int Product { get; set; }
}

// Example usage in a Main method
public class Program
{
    static Result Calculate(int a, int b)
    {
      return new Result { Sum = a + b, Product = a * b };
    }

    public static void Main()
    {
        var result = Calculate(3, 4);
        Console.WriteLine(result.Sum);     // 7
        Console.WriteLine(result.Product); // 12
    }
}
```

**4. Using a Arrays or Lists**

If the values to be returned are of the same type, you can return an array or a list.

```cs
public int[] GetValues()
{
    return new int[] { 10, 20, 30 };
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. In how many ways you can pass parameters to a method?

You can pass parameters to a method in C# in several ways:

**1. By Value (default):**  

The method receives a copy of the argument. Changes inside the method do not affect the original variable.

```cs
Void MyMethod(int x) {
    x = 10 // Only modifies the local copy
}
```

**2. By Reference (`ref`):**  

The method receives a reference to the original variable. The variable must be initialized before passing.

```cs
Void MyMethod(int x) {
    x = 10 // Modifies the original value
}
```

**3. Output Parameter (`out`):**  

The method can assign a value to the parameter and return it to the caller. The variable does not need to be initialized before passing.

```cs
Void MyMethod(out int x) {
    x = 10 // Must assign a value before method ends
}
```

**4. Parameter Array (`params`):**  

Allows passing a variable number of arguments as an array.

```cs
Void MyMethod(params int[] numbers) {
    foreach(int n in numbers) {
        Console.WriteLine(n);
    }
}
```

**5. Optional Parameters:**  

Parameters with default values that can be omitted when calling the method.

```cs
Void MyMethod(int x = 5) {
    Console.WriteLine(x);
}
```

**6. Named Parameters:**  

Allows specifying parameters by name when calling the method, allowing arguments to be passed in any order.

```cs
Void MyMethod(int x, int y) {
    Console.WriteLine(x + y);
}

// call using named parameters
MyMethod(y: 10, x: 5);
```

**7. in Parameters (Read-only Reference):**  

Passes by reference but ensures the method cannot modify the value.

```cs
Void MyMethod(in int x) {
    Console.WriteLine(x); // Cannot modify x
}

// call using named parameters
MyMethod(y: 10, x: 5);
```

**Summary Table:**

| Way                | Keyword    | Description                                 |
|--------------------|------------|---------------------------------------------|
| By Value           | (default)  | Passes a copy of the value                  |
| By Reference       | ref        | Passes a reference to the variable          |
| Output Parameter   | out        | Passes a reference, must assign in method   |
| Parameter Array    | params     | Passes variable number of arguments         |
| Optional Parameter | = value    | Allows omitting arguments with defaults     |
| Named Parameter    | name: val  | Specify argument by name                    |
| In Parameter       | in         | Passes a reference to the variable(cannot modify the value)|

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 2. OPERATORS

<br/>

## Q. What are the different types of operators in C#?
---
C# operators are special symbols that perform operations on operands. They are categorized into several types: arithmetic, comparison, logical, bitwise, assignment, and others.

The different types of operators in C# are:

**1. Arithmetic Operators**

These operators perform standard arithmetic operations on numeric values.

- `+` (Addition)
- `-` (Subtraction)
- `*` (Multiplication)
- `/` (Division)
- `%` (Modulus)
- `++` (Increment)
- `--` (Decrement)

**Example:**
```cs
int a = 10, b = 3;

Console.WriteLine(a + b); // Output: 13
Console.WriteLine(a - b); // Output: 7
Console.WriteLine(a * b); // Output: 30
Console.WriteLine(a / b); // Output: 3
Console.WriteLine(a % b); // Output: 1
```

**2. Relational (Comparison) Operators**

These operators compare two values and return a boolean result (true or false).

- `==` (Equal to)
- `!=` (Not equal to)
- `>` (Greater than)
- `<` (Less than)
- `>=` (Greater than or equal to)
- `<=` (Less than or equal to)

**Example:**
```cs
int a = 5, b = 10;

Console.WriteLine(a == b) // Output: False
Console.WriteLine(a < b) // Output: True
```

**3. Logical Operators**

These operators perform logical operations on boolean expressions.

- `&&` (Logical AND)
- `||` (Logical OR)
- `!` (Logical NOT)

**Example:**
```cs
bool isAdult = true;
bool hasID = false;

Console.WriteLine(isAdult && hasID); // Output: False
Console.WriteLine(isAdult || hasID); // Output: True
```

**4. Assignment Operators**

These operators assign values to variables.

- `=` (Simple assignments)
- `+=`, `-=`, `*=`, `/=`, `%=` (Compound assignments)

**Example:**
```cs
int x = 5;
x += 3; // x = x + 3

Console.WriteLine(x); // Output: 8
```

**5. Bitwise Operators**

These operators work directly on the binary representation of numbers.

- `&` (AND)
- `|` (OR)
- `^` (XOR)
- `~` (NOT)
- `<<` (Left shift)
- `>>` (Right shift)

**Example:**
```cs
int a = 5;
int b = 3;

Console.WriteLine(a & b); // Output: 1 (0001)
Console.WriteLine(a | b); // Output: 7 (0111)
```

**6. Conditional (Ternary) Operator**

- `condition ? expr1 : expr2`

**Example:**
```cs
int age = 18;
String result = (age >= 18) ? "Adult" : "Minor";

Console.WriteLine(result); // Output: Adult
```

**7. Null-Coalescing Operators**

Used to handle null values:

- `??` (Returns the left-hand operand if not null, otherwise right)
- `??=` (Assigns the right-hand operand if the left is null)

**Example:**
```cs
string name = null;
string displayName = name ?? "Guest";

Console.WriteLine(displayName); // Output: Guest
```

**8. Null-Conditional Operator**

- `?.` (Safely access members/methods if the object is not null)

**Example:**
```cs
object?.Member // If object is null, the expression returns null instead od throwing an exception.
```

**9. Type Operators**

Used for type checking and casting:

- `is` (Checks if an object is a specific type)
- `as` (Attempts to cast an object to specific type)
- `typeof` (Returns the type object for a type)
- `sizeof` (Returns the size in bytes of a value type)

**Example:**
```cs
object obj = "Hello World";

if(obj is string)
{
    Console.WriteLine("It\'s a string!");
}
```

**10. Other Operators**

- `new` (Creates objects)
- `nameof` (Gets the name of a variable/type/member as a string)
- `checked` / `unchecked` (Controls overflow checking)
- `await` (Used in asynchronous programming)
- `=>` (Lambda operator)
- `[]` (Array/indexer access)
- `()` (Method call/cast)
- `.` (Member access)


**Example:**
```cs
// Simple Lambda Expression
Func<int, int> square = x => x * x;

Console.WriteLine(square(5)); // Output: 25
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `nameof` operator in C#?
---
The `nameof` operator in C# is used to obtain the **simple (unqualified) string name** of a variable, type, or member. It is evaluated at compile time and helps make code safer and easier to maintain, especially when referring to member names in exceptions, logging, data binding, or attributes.

**Purpose and Benefits:**

- **Refactoring safety:** If you rename a variable, property, or method, `nameof` automatically updates the string, reducing errors from hard-coded strings.
- **Compile-time checking:** Errors are caught at compile time if the referenced name does not exist.
- **Improved readability:** Makes code clearer and less error-prone.


**Typical use cases:**  
- Argument validation: `throw new ArgumentNullException(nameof(parameter));`
- PropertyChanged events in data binding
- Logging and diagnostics

**Example:**

```cs
public class Person
{
    public string FirstName { get; set; }

    public void PrintName()
    {
        Console.WriteLine(nameof(FirstName)); // Output: FirstName
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is difference between const and readonly in C#?
---
In C#, both `const` and `readonly` are used to define values that cannot be changed after initialization, but they have important differences:

| Feature         | const                              | readonly                                 |
|-----------------|------------------------------------|------------------------------------------|
| When assigned   | At compile time                    | At runtime (in constructor or declaration)|
| Type            | Only primitive types, string, enum | Any type (including reference types)     |
| Scope           | Implicitly static (class-level)    | Can be instance-level or static          |
| Value changes   | Cannot be changed anywhere         | Can be assigned once per instance        |
| Usage           | For values known at compile time   | For values known only at runtime         |

**Const**:
- Must be assigned a value at declaration.
- Value is replaced at compile time (literal).
- Always static; cannot be used with instance-specific values.

**Example:**
```cs
public class MyClass
{
    public const double Pi = 3.14159;
}
```

**Readonly**:
- Can be assigned at declaration or in a constructor.
- Value is set at runtime, so it can differ per instance.
- Can be used with reference types and structs.

**Example:**
```cs
public class MyClass
{
    public readonly int Id;

    public MyClass(int id) {
        Id = id; // Allowed in constructor
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How you would use a bitwise operator in C#? 
---
Bitwise operators in C# are used to perform bit level operations on integer types like `int`, `uint`, `long`, `ulong`, `bytes`, etc. These operators treat their operands as a sequence of bits rather than as decimal, hexadecimal, or octal numbers.

**Overview:**

| Operator   | Symbol | Description  |
|------------|--------|--------------|
|AND         |  &     | Sets each bit to 1 if both bits are 1|
|OR          |        | Sets each bit to 1 if at least one of the corresponding bits is 1, otherwise 0.|
|XOR         |  ^     | Sets each bit to 1 if only one of two bits is 1|
|NOT         |  ~     | Inverts all the bits|
|Left Shift  |  <<    | Shifts bits to the left|
|Right Shift |  >>    | Shifts bits to the right|

**Typical use cases:**

* Setting, clearing, or toggling specific bits in flags or masks.
* Efficient storage of multiple boolean values.
* Low-level programming, device control, or performance-critical code.

**Example:**
```cs
int a = 5;      // 0101 in binary
int b = 3;      // 0011 in binary

// Bitwise AND
int and = a & b; // 0001 = 1

// Bitwise OR
int or = a | b;  // 0111 = 7

// Bitwise XOR
int xor = a ^ b; // 0110 = 6

// Bitwise NOT
int notA = ~a;   // Inverts all bits

// Left shift
int leftShift = a << 1; // 1010 = 10

// Right shift
int rightShift = a >> 1; // 0010 = 2

Console.WriteLine($"AND: {and}, OR: {or}, XOR: {xor}, NOT: {notA}, <<: {leftShift}, >>: {rightShift}");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain the use of the `as` operator in C# and the best way to use it?
---
The `as` operator in C# is used for **safe type casting**. It attempts to cast an object to a specified type and returns `null` if the conversion fails, instead of throwing an exception (unlike a direct cast).

**Syntax:**
```cs
object obj = "hello";
string str = obj as string; // str is "hello"
```
**When to Use the `as` Operator:**

- **When you want to avoid exceptions:** Use `as` when you expect that the cast might fail and you want to handle it gracefully.
- **When working with reference types or nullable value types:** `as` only works with these types.

**Best Practices:**

**1. Always check for null after using `as`:**
```cs
object obj = GetObject();
MyClass mc = obj as MyClass;
if (mc != null)
{
    mc.DoSomething();
}
else
{
    // Handle the failed cast
}
```

**2. Use `as` for performance when you need to both check and cast:**
   - Prefer `as` over `is` + cast when you need the casted value, to avoid double type-checking.

**3. Do not use `as` with value types (except nullable):**
   - `as` cannot be used with non-nullable value types.

**Example:**
```cs
class Animal { }
class Dog : Animal
{
    public void Bark() => Console.WriteLine("Woof!");
}

object obj = new Dog();

Dog dog = obj as Dog;
if (dog != null)
{
    dog.Bark(); // Output: Woof!
}
else
{
    Console.WriteLine("Not a Dog");
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the use of Null Coalescing Operator (??) in C#? 
---
The **null coalescing operator(??)** in C# is used to provide a default value when dealing nullable types or potentially null expressions. It helps to write cleaner and more concise code by avoiding explicit null checks.

**Example:**
```cs
string userAge = null;
string age = userAge ?? 18;

Console.WriteLine(userAge); // Output: 18
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is difference between "is" and "as" operator in C#?
---
In C#, the `is` operator and the `as` operator are both used for type checking and type conversion, but they serve different purposes. The `is` operator checks if an object is of a specific type, returning a boolean value (true or false). The `as` operator attempts to convert an object to a specified type, returning the converted object if the conversion is successful, or null if it\'s not. 

**1. `is` Operator:**
- Checks if an object is compatible with a given type.
- Returns a boolean (`true` or `false`).
- Does **not** perform a cast.

**Example:**
```cs
object obj = "hello";
if (obj is string)
{
    Console.WriteLine("obj is a string");
}
```

**2. `as` Operator:**
- Attempts to cast an object to a specified reference type or nullable type.
- Returns the object as the new type if successful, or `null` if the cast fails (no exception thrown).
- Only works with reference types and nullable value types.

**Example:**
```cs
object obj = "hello";
string str = obj as string;
if (str != null)
{
    Console.WriteLine($"String value: {str}");
}
```

**Use Case:**  
- Use `is` when you only need to check the type.
- Use `as` when you want to try casting and handle failure gracefully (by checking for `null`).

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are nullable types in C#?
---
In C#, **nullable types** cover two distinct concepts:

**1. Nullable Value Types (`T?` / `Nullable<T>`) — all .NET versions:**

Allow value types (e.g., `int`, `bool`, `DateTime`) to represent `null`, useful for optional database fields or missing data.

**Key members:**
* `HasValue` — `true` if the variable holds a non-null value.
* `Value` — gets the value (throws `InvalidOperationException` if `null`).
* `GetValueOrDefault()` — returns the value or the type\'s default.

```cs
int? score = null;

if (score.HasValue)
    Console.WriteLine($"Score: {score.Value}");
else
    Console.WriteLine("Score is not set."); // Output: Score is not set.

int fallback = score.GetValueOrDefault(-1); // -1
```

**2. Nullable Reference Types (C# 8+, .NET Core 3+):**

Enable compile-time null safety for **reference types**. Enabled project-wide with `<Nullable>enable</Nullable>` in the `.csproj` (default in .NET 6+ projects).

```cs
// Without nullable context: string can be null silently (old behavior)
// With nullable context:
string nonNullable = "Pradeep"; // Cannot be null — compiler warns if you try
string? nullable = null;        // Explicitly nullable — must check before use

int length = nullable?.Length ?? 0; // Safe with null-conditional + null-coalescing
Console.WriteLine(length); // Output: 0
```

**3. Null-coalescing operators (C# 8+):**

```cs
string? input = null;
string result = input ?? "default";      // "default"
input ??= "assigned if null";            // ??= assigns only if left side is null
Console.WriteLine(input); // Output: assigned if null
```

**4. Null-forgiving operator `!` (C# 8+):**

Suppresses the nullable warning when you know a value isn\'t null:

```cs
string? value = GetMaybeNull();
int len = value!.Length; // tells compiler "trust me, not null"
```
<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Type Casting and what are its types in C#?
---
Type casting in C# is the process of converting a variable from one data type to another. This is often necessary when working with different types of data, such as converting an `int` to a `double`, or casting a base class reference to a derived class.

There are two main types of type casting in C#:

**1. Implicit Casting**
* Automatically performed by the compiler when converting from a smaller to a larger or compatible type.
* Safe because there is no loss of data.

**Examples:**  
```cs
int num = 100;
double d = num; // Implicit casting: int to double
```

**2. Explicit Casting**
* Required when converting from a larger to a smaller or incompatible type.
* May result in data loss or runtime exceptions

**Examples:**  
```cs
double d = 123.45;
int num = (int)d; // Explicit casting: double to int (fractional part lost)
```

**3. Other Casting Types**

- **Boxing and Unboxing:**  
  - *Boxing* converts a value type to an object type.
  - *Unboxing* extracts the value type from the object.

**Example:**
```cs
int x = 10;
object obj = x;      // Boxing
int y = (int)obj;    // Unboxing
```

- **Using `as` and `is` Operators:**  
  - `as` tries to cast and returns `null` if it fails (for reference/nullable types).
  - `is` checks type compatibility.

**Example:**
```cs
object obj = "hello";
    string str = obj as string; // str is "hello"
    if (obj is string) { /* true */ }
```

- **Using Convert Class:**  
    - Provides methods to convert between base types.

**Example:**
```cs
string str = "123";
int num = Convert.ToInt32(str);
```

- **Using Parse() and TryParse()**
    - Converts strings to numeric types
    - TryParse() is safer as it avoids exceptions.

**Example:**
```cs
int result;
bool success = int.TryParse("456", out result);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `==` operator and `.Equals()` method?
---
The `==` operator and `.Equals()` method are both used to compare objects in C#, but they behave differently depending on the type being compared:

**1. `==` Operator:**

- **Default behavior:** For reference types, `==` checks if both references point to the same object in memory (reference equality).
- **Value types:** For built-in value types (like `int`, `double`), `==` compares the actual values (value equality).
- **Can be overloaded:** Classes can overload the `==` operator to provide custom equality logic (e.g., `string` and many .NET types do this).

**2. `.Equals()` Method:**

- **Default behavior:** Inherited from `object`, compares reference equality unless overridden.
- **Override:** Many types (like `string`, value types, and custom classes) override `.Equals()` to compare values.
- **Polymorphic:** Can be overridden in derived classes for custom equality logic.

**Key Differences:**

| Aspect                | `==` Operator                        | `.Equals()` Method                |
|-----------------------|--------------------------------------|-----------------------------------|
| Reference Types       | Reference equality (unless overloaded)| Reference equality (unless overridden) |
| Value Types           | Value equality                       | Value equality (overridden)       |
| Overridable           | Yes (operator overloading)           | Yes (method override)             |
| Null Handling         | Safe (returns false if either is null)| Throws if called on null instance |

**Example:** 
```csharp
class Person {
    public string Name;
    public override bool Equals(object obj) =>
        obj is Person p && Name == p.Name;
    // == is not overloaded, so default is reference equality
}

var p1 = new Person { Name = "Pradeep" };
var p2 = new Person { Name = "Pradeep" };

Console.WriteLine(p1 == p2);        // False (different references)
Console.WriteLine(p1.Equals(p2));   // True  (same value)
```

**Summary:**  
- Use `==` for simple value types and when you know the operator is overloaded for value comparison (like `string`).
- Use `.Equals()` when you want to ensure value-based comparison, especially for custom types.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is short-circuit evaluation in C#?
---
Short-circuit evaluation in C# is a performance optimization technique used with logical operators like `&&` and `||`. It means that the second operand in a logical expression is evaluated only if necessary.

**1. Logical AND (&&):**

- If the first operand is false, the result is always false, so the second operand is not evaluated.

**Example:**

```cs
string s = null;
if (s != null && s.Length > 0)
{
    Console.WriteLine("String is not empty.");
}
```

Here, `s.Length > 0` is only checked if `s != null` is `true`, preventing a possible exception.

**2. Logical OR (||):**

- If the first operand is true, the result is always true, so the second is not evaluated.

**Example:**
```cs
bool result = (x == 0) || (10 / x > 1);
```

If x is 0, the first condition is true, so the second part is not evaluated, again avoiding a divide-by-zero exception.

**Benefits:** 

- **Efficiency:** Avoids unnecessary computation.
- **Safety:** Prevents errors like null reference or divide-by-zero.
- **Control flow:** Lets you write conditions that depend on earlier checks.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. List some different ways for equality check in .Net?
---
In C#, there are several ways to perform equality checks depending on the type of objects comparison. Here are some common ways to check for equality in .NET:

**1. `==` Operator**
- Compares value types by value.
- For reference types, checks reference equality unless overloaded (e.g., `string`).

**Example:**
```cs
int a = 5, b = 5;
bool result = a == b; // true

string s1 = "hello", s2 = "hello";
bool isEqual = s1 == s2; // true (string overloads ==)
```

**2. `.Equals()` Method**
- Checks value equality if overridden; otherwise, checks reference equality.

**Example:**
```cs
object o1 = "Hello";
object o2 = "Hello";

bool isEqual = o1.Equals(o2); // true
```

**3. `Object.ReferenceEquals()`**
- Checks if two references point to the same object.
- Does not consider value equality.

**Example:**
```cs
object o1 = new object();
object o2 = o1;

bool isEqual = ReferenceEquals(o1, o2); // true
```

**4. `Object.Equals(a, b)`**
- Static method; handles nulls safely.
- Calls `.Equals()` internally.

**Example:**
```cs
bool isEqual = Object.Equals(objA, objB);
```

**5. `IEquatable<T>.Equals()`**
- Implement for custom value equality in your types.
- Interface for type-safe equality
- Recommended for value types and collections.

**Example:**
```cs
public class Person : IEquatable<Person>
{
    public string Name;
    public bool Equals(Person other) => Name == other?.Name;
}
```

**6. `SequenceEqual()` (for collections)**
- Compares elements of two sequences.

**Example:**
```cs
var arr1 = new[] { 10, 20, 30 };
var arr2 = new[] { 10, 20, 30 };

bool isEqual = arr1.SequenceEqual(arr2); // true
```

**7. `StructuralComparisons.StructuralEqualityComparer`**
- For arrays and tuples.

**Example:**
```cs
var arr1 = new[] { 1, 2 };
var arr2 = new[] { 1, 2 };

bool isEqual = StructuralComparisons.StructuralEqualityComparer.Equals(arr1, arr2); // true
```

**8. `EqualityComparer<T>.Default.Equals()`**
- Useful in generic code.
- Uses the default quality comparer for the type.

**Example:**
```cs
bool isEqual = EqualityComparer<string>.Default.Equals("Hi", "Hi");
```

**Summary Table:**

| Method                        | Use Case                        |
|-------------------------------|---------------------------------|
| `==`                          | Value types, overloaded types   |
| `.Equals()`                   | Value/reference, can override   |
| `ReferenceEquals()`           | Reference equality only         |
| `Object.Equals(a, b)`         | Safe, handles nulls             |
| `IEquatable<T>.Equals()`      | Custom types, performance       |
| `SequenceEqual()`             | Collections/arrays              |
| `StructuralEqualityComparer`  | Arrays, tuples, structural types|
| `EqualityComparer<T>.Default.Equals()`| generic code            |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is difference between static, readonly, and constant in C#?
---
In C#, `static`, `readonly`, and `const` are modifiers used to define how variables behave in terms of initialization, memory allocation, and mutability. Here\'s a breakdown of the differences:

**1. `const`(Constant)**
- Value must be assigned at declaration and cannot change.
- Value is replaced at compile time (literal).
- Always static (shared across all instances).
- Only primitive types, enums, or strings.

**Example:**
```cs
public const double Pi = 3.14159;
```

**2. `readonly`**
- Value can be assigned at declaration or in the constructor.
- Value can differ per instance (unless also static).
- Value cannot change after construction.
- Can be any type (including reference types).

**Example:**
```cs
public readonly int id;

public MyClass(int id) { 
    this.id = id; 
}
```

**3. `static`**
- Belongs to the type itself, not to any instance.
- Shared across all instances.
- Can be changed at runtime (unless also readonly/const).
- Can be used with fields, methods, constructors, and classes.

**Example:**
```cs
public static int Counter = 0;
```

**Summary:**
|Feature       | const    | readonly      | static |
|--------------|----------|---------------|--------|
|Compile-time  |Yes       | No            | No     |
|Runtime       |No        | Yes           | Yes    |
|Instance-based|No        | Yes           | No     |
|static        |Implicitly| Optional      | Yes    |
|Mutable       |No        | No(after init)| Yes(if not readonly)|

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How to loop through an enum in C#?
---
To loop through an enum in C#, you can use the `Enum.GetValues()` method, which returns an array of the enum\'s values. 

**Example:**
```cs
enum Days
{
    Sunday,
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday
}

class Program
{
    static void Main()
    {
        foreach (Days day in Enum.GetValues(typeof(Days)))
        {
            Console.WriteLine(day);
        }
    }
}
```

**Output:**
```
Sunday
Monday
Tuesday
Wednesday
Thursday
Friday
Saturday
```

- `Enum.GetValues(typeof(Days))` returns an array of all enum values.
- You can also use `Enum.GetNames(typeof(Days))` to get the names as strings.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How to set default value to Property in C#?
---
You can set a **default value** for a property in C# in several ways, depending on the type of property and the context.

**1. Auto-Implemented Property with Default Values:**
- You can assign a default value directly in the property declaration:
```cs
public class Person
{
    public string Name { get; set; } = "Unknown";
    public int Age { get; set; } = 18;
}
```

**2. Using a Constructor:**
- If you need more complex logic or want to support older versions of C#, use a constructor:
```cs
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }

    public Person()
    {
        Name = "Unknown";
        Age = 18;
    }
}
```

**3. Using Read-Only Properties (init-only):**

```csharp
public class Person
{
    public string Name { get; init; } = "Unknown";
}
```

**4. Default values with Nullable Types:**
- For optional values, you can use nullable types and assign defaults:
```cs
public class Person
{
    public int? Age { get; set; } = null;
}
```

**Summary:**  
- Use property initializers for simple default values.
- Use the constructor for more complex logic or when default values depend on other parameters.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How to convert int to enum in C#?
---
You can convert an integer to an enum type in C# using a simple cast. This is useful when you have an integer value (for example, from a database or user input) and want to work with it as an enum.

**Example:**
```cs
public class Program
{
    enum Status
    {
        Pending = 0,
        Approved = 1,
        Rejected = 2
    }
            
    public static void Main(string[] args)
    {
        int value = 1;
        Status status = (Status)value;

        Console.WriteLine(status); // Output: Approved
    }
}
```

**Note:**
- The cast does not check if the integer value is defined in the enum. If the value is not defined, it will still cast, but the result may not be meaningful.
- To check if the value is valid for the enum, use `Enum.IsDefined`:

```cs
if (Enum.IsDefined(typeof(Status), value))
{
    Status status = (Status)value;
    Console.WriteLine(status)
}
else
{
    Console.WriteLine("Invalid status value");
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is BigInteger Data Type in C#?
---
The `BigInteger` data type in C# is a structure provided by the `System.Numerics` namespace that allows you to work with arbitrarily large integers—much larger than the built-in numeric types like `int` or `long`. Unlike these fixed-size types, `BigInteger` can represent numbers of any size and precision, limited only by the available system memory.

**Key Points:**
- `BigInteger` is used when you need to handle numbers larger than `long.MaxValue` (9,223,372,036,854,775,807).
- It supports all standard arithmetic operations (+, -, *, /, %, etc.).
- It is immutable—operations return a new `BigInteger` instance.

**Example:**

```cs
using System;
using System.Numerics;

class Program
{
    static void Main()
    {
        BigInteger big = BigInteger.Parse("123456789012345678901234567890");
        BigInteger result = big * 2;

        Console.WriteLine(result); // Output: 246913578024691357802469135780
    }
}
```

**Note:**  
- To use `BigInteger`, add a reference to `System.Numerics` and include `using System.Numerics;` at the top of your file.
- This is especially useful in scenarios like cryptography, scientific computations, or financial calculations where precision and large values are critical. 

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How to convert String to Enum in C#?
---
To convert a string to an enum in C#, use the `Enum.Parse()` or `Enum.TryParse()` method. `Enum.Parse()` throws an exception if the conversion fails, while `Enum.TryParse()` returns a boolean indicating success. 

This is useful when you have a string value (e.g., from user input or a file) and want to convert it to a strongly-typed enum value.

**1. Using Enum.Parse():**

- This throws an exception if the string doesn\'t match any enum name.

**Example:**
```cs
public enum Status
{
    Active,
    Inactive,
    Pending
}

string input = "Active";
Status status = (status)Enum.Parse(typeof(Status), input);
```

**2. Using Enum.TryParse():**

- This method is safer because it avoids exceptions and let you handle invalid input gracefully.

**Example:**
```cs
public enum Status 
{
    Active,
    Inactive,
    Pending
}

string input = "Inactive";

if(Enum.TryParse(input, out Status status)) 
{
    Console.WriteLine($"Parsed successfully: {status}");
} 
else
{
    Console.WriteLine("Invalid enum value.");
}
```

**3. Case-Insensitive Parsing:**

**Example:**
```cs
Enum.TryParse("pending", ignoreCase: true, out Status status);
```

**Notes:**
- Use `Enum.TryParse` for safer conversion.
- You can pass `true` as the second argument to `TryParse` for case-insensitive matching.
- Always validate user input before parsing to avoid exceptions.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How to convert an Object to JSON in C#?
---
In C#, you can convert an object to JSON string using the `System.Text.Json` namespace or `Newtonsoft.Json` (also known as Json.NET) library. The most common and modern approach is with `System.Text.Json`.

**1. Using System.Text.Json:**

You can convert an object to a JSON string in C# using the built-in `System.Text.Json` namespace:

**Example:** 
```cs
using System.Text.Json;

var person = new { Name = "Pradeep", Age = 30 };
string json = JsonSerializer.Serialize(person);

Console.WriteLine(json); // Output: {"Name":"Pradeep","Age":30}
```

**2. Using Newtonsoft.Json:**

- install the NuGet package:

```cs
Install-Package Newtonsoft.Json
```

**Example:**
```csharp
using Newtonsoft.Json;

var person = new { Name = "Pradeep", Age = 30 };
string json = JsonConvert.SerializeObject(person);

Console.WriteLine(json); // Output: {"Name":"Pradeep","Age":30}
```

**Note:**  
- For `System.Text.Json`, add `using System.Text.Json;`.
- For `Newtonsoft.Json`, install the NuGet package and add `using Newtonsoft.Json;`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How to convert JSON String to Object in C#?
---
To convert a JSON string to an object in C#, you typically use either the built-in `System.Text.Json` namespace or the popular third-party library `Newtonsoft.Json` (Json.NET).

**1. Using System.Text.Json:**

You can convert a JSON string to an object in C# using the built-in `System.Text.Json` namespace:

**Example:**
```cs
using System.Text.Json;

string json = "{\"Name\":\"Pradeep\",\"Age\":30}";

// Define a matching class
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}

// Deserialize the JSON string to an object
Person person = JsonSerializer.Deserialize<Person>(json);

Console.WriteLine(person.Name); // Output: Pradeep
Console.WriteLine(person.Age);  // Output: 30
```

**2. Using Newtonsoft.Json (Json.NET):**

First, install the NuGet package:  
`Install-Package Newtonsoft.Json`

**Example:**
```cs
using Newtonsoft.Json;

string json = "{\"Name\":\"Pradeep\",\"Age\":30}";

public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}

Person person = JsonConvert.DeserializeObject<Person>(json);

Console.WriteLine(person.Name); // Output: Pradeep
Console.WriteLine(person.Age);  // Output: 30
```

**Note:**  
- The class properties must match the JSON keys (case-insensitive by default).
- For dynamic or anonymous types, you can use `JsonDocument` (System.Text.Json) or `JObject` (Newtonsoft.Json).

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How to Pass or Access Command-line Arguments in C#?
---
In C#. you can access command-line arguments using the Main method\'s parameter, typicallydefined as a string[] args.

**Example:**
```cs
using System;

class Program
{
    static void Main(string[] args) 
    {
        Console.WriteLine("Number of arguments:" + args.Length);
        foreach (string arg in args)
        {
            Console.WriteLine("Argument: " + arg);
        }
    }
}
```

Running the Program
```cs
MyApp.exe firstArg secondArg
```
Output
```cs
Number of arguments: 2
Argument: firstArg
Argument: secondArg
```
**2. Alternative Access**

- You can also access command-line arguments using:
```cs
string[] args = Environment.GetCommandLineArgs();
```
This includes the executable name as the first element(args[0]), unlike the Main method\'s args which starts from the first actual argument.

## Q. How to convert date object to string in C#?
---
To convert a date object (DateTime) to a string in C#, use the `ToString()` method. You can specify a format string to control the output.

**1. Default Format:**

```cs
DateTime now = DateTime.Now;
string dateString = now.ToString();
```

**2. Custom Format:**

```cs
string formatted = now.ToString("yyyy-MM-dd HH:mm:ss");
```

**3. Culture-Specific Format:**

```cs
string cultureFormatted = now.ToString("D", new CultureInfo("fr-FR"));
```

**Common formats:**
- `"yyyy-MM-dd"` → 2025-05-26
- `"MM/dd/yyyy"` → 05/26/2025
- `"dddd, MMMM dd, yyyy"` → Monday, May 26, 2025

**Summary:**  
Use `dateTime.ToString()` for default, or `dateTime.ToString("format")` for custom string output.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How to combine two arrays without duplicate values in C#?
---
To combine two arrays without duplicate values in C#, you can use the `Union` method from **LINQ**, which returns the set union of two sequences (removing duplicates). 

**Example:**
```cs
using System;
using System.Linq;

class Program
{
    static void Main()
    {
        int[] array1 = { 10, 20, 30 };
        int[] array2 = { 30, 50, 10 };

        int[] combinedArray = array1.Union(array2).ToArray();

        Console.WriteLine("Combined Array: " + string.Join(", ", combinedArray)); // Output: 10, 20, 30, 50
    }
}
```

**Explanation:**
- `Union` returns the set union of two sequence, which means it removes duplicates.
- `ToArray()` converts the result back to an array.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How to convert string to int in C#?
---
To convert a string to an int in C#, you can use one of the following methods:

**1. int.Parse()**  

This method throws an exception if the string is not a valid integer.

```cs
string str = "123";
int number = int.Parse(str); // number = 123
```

**2. int.TryParse()**  

This is the safest method. It returns true if the conversion is successful, otherwise false.

```cs
string str = "123";
int number;
bool isSuccess = int.TryParse(str, out number);

if (isSuccess)
{
    Console.WriteLine("Conversion successful:" + number);
}
else
{
    Console.WriteLine("Invalid Input");
}
```

**3. Convert.ToInt32()**  

This method throws an exception if the string is not a valid number, but it handles `null` by returning 0.

```cs
string str = "123";
int number = Convert.ToInt32(str); // number = 123
```

**Recommendation:**  

Use `int.TryParse()` for user input or when the string may not be a valid integer, as it avoids exceptions.

**Summary Table:**

|Method           |Throws on invalid input|Handles null | Usage scenario      |
|-----------------|-----------------------|-------------|---------------------|
|int.Parse()      |Yes                    |No           | Trusted input       |
|int.TryParse()   |No                     |Yes          | User/untrusted input|
|Convert.ToInt32()|Yes                    |Yes (returns 0) | Nullable input   |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is boxing and unboxing?
---
In C#, **boxing** and **unboxing** are processes that allow value types (like `int`, `float`, `bool`, `double`, `struct`, etc.) to be treated as reference types (like object).

**1. Boxing:**  

- Boxing is the process of converting a **value type** to a **reference type** (specially, to object or to any interface type implemented by the value type). 
- The value is wrapped inside a System.Object and stored on the heap.

**Example:**
```cs
int num = 42;
object obj = num; // Boxing: num is copied into obj as an object
```

- The value 42 (a value type) is wrapped inside an object (a reference type).
- This involves copying the value and storing it on the heap.

**2. Unboxing:**
 
**Unboxing** is the reverse process: converting a **reference type** back to a **value type**.

**Example:**
```cs
object obj = 42;
int num = (int)obj; // Unboxing: obj is converted back to int
```

- The object obj is unboxed back into an int.
- This requires an explicit cast and can throw an exception if the types don\'t match.

**Notes:**
- Boxing incurs a performance cost due to heap allocation.
- Unboxing requires explicit casting and can throw exceptions if the types do not match.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>


## Q. What effect does boxing and unboxing have on performance?
---
Boxing and unboxing can negatively impact performance in C#.

**Boxing** is the process of converting a value type (like `int`, `double`, or a struct) to a reference type (`object`). This involves allocating memory on the heap and copying the value, which is more expensive than working with value types on the stack.

**Unboxing** is the reverse: extracting the value type from the object. This requires a type check and copying the value back from the heap to the stack.

**Performance Effects:**

**1. Memory Allocation:** 

- Boxing allocates memory in the heap, whereas value types are usually stored on the stack.
- Heap allocations are more expensive and require garbage collection, which can slow down your application.

**2. Garbage collection:** 

- Frequent boxing leads to more objects on the heap, increasing the load on the garbage collector.

**3. CPU overhead:** 

- Boxing and unboxing involve type checking and casting, which adds CPU cycles.

**4. Cache Misses:**

- Value types on the stack are more cache-friendly. Boxed objects on the heap can lead to cache misses, reducing performance.

**Example:**
```cs
int x = 42;
object obj = x;      // Boxing (heap allocation)
int y = (int)obj;    // Unboxing (type check + copy)
```

**When to Avoid Boxing/Unboxing:** 

- In tight loops or performance critical code.
- When working with collections-prefer `List<int>` over `List<object>`.
- Use generics to avoid boxing in data structures.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `==` and `ReferenceEquals` in C#?
---
The `==` and `ReferenceEquals` in C# are both used for comparisons, but they serve different purposes:

- **`==` Operator**:
  - For **value types** (like `int`, `struct`), `==` compares the actual values.
  - For **reference types** (like classes), by default, `==` checks if both references point to the same object (reference equality). However, many classes (like `string`) **override** `==` to compare values instead.
  - Can be **overloaded** by custom types to provide value-based equality.

**Example:**
````cs
object a = new string("hello");
object b = new string("hello");

Console.WriteLine(a == b); // True, because string overrides == for value equality
````

- **`ReferenceEquals` Method**:
  - Always checks if two references point to the **exact same object** in memory (reference equality), regardless of any operator overloading or overrides.
  - Cannot be overloaded.

**Example:**
````cs
object a = new string("hello");
object b = String.Copy(a);

Console.WriteLine(object.ReferenceEquals(a, b)); // False, different objects in memory
````

**Summary:**  
- Use `==` for value comparison (if overridden).
- Use `ReferenceEquals` when you need to know if two variables refer to the exact same object instance.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does operator overloading work in C#?
---
In C#, operator overloading allows developers to extend the functionality of operators (like `+`, `-`, `*`, etc.) to work with user-defined data types (classes and structs). This makes your objects behave more like built-in types, improving readability and usability.

**Example:**

```cs
/**
* + Operator Overloading Example
*/
Public class Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    // Overload the + operator
    public static Point operator +(Point a, Point b) 
    {
        return new Point(a.X + b.X, a.Y + b.Y);
    }
}

// Usage
Point p1 = new Point(1, 2);
Point p2 = new Point(3, 4);
Point result = p1 + p2; // Uses the overload + operator
```

**Key Points:**

* Only certain operators can be overloaded (eg. `+`, `-`, `*`, `/`, `==`, `!=`, `<`, `>`, `++`, `--`, `[]`, `()`).
* Overloaded operators must be static and public.
* You can also overload comparison operators, but they must be overloaded in paris (`==` with `!=`, `<` with `>`).
* Overloading short-circuiting operators (`||` and `&&`) is generally discouraged due to potential confusion. 

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the "=>" operator in C#? Where is it used?
---
The `=>` operator in C# is called the **lambda operator** or **goes to operator**. It is used to define **lambda expressions**, which are anonymous functions that can contain expressions or statements and can be used to create delegates or expression tree types.

**Syntax:**

```cs
(parameter) => expression
```
**Where is it used?**

**1. Lambda Expressions:** Used to define inline functions, especially with LINQ, delegates, and events.

**Examples:**
```csharp
Func<int, int, int> add = (a, b) => a + b;
Console.WriteLine(add(2, 3)); // Output: 5
```

**2. LINQ Queries:** Commonly used in LINQ to filter, project, or transform data.
**Example:**
```csharp
var evens = numbers.Where(n => n % 2 == 0);
```

**3. Event Handlers:** Can be use to define event handlers inline.
**Example:**
```csharp
button.Click += (sender, e) => { Console.WriteLine("Button clicked!"); };
```

**4. Expression Trees:** In advanced scenarios, lambda expressions can be complied into expression trees for dynamic query generation.
**Example:**
```cs
using System;
using System.Linq.Expressions;

class Program
{
    static void Main()
    {
        // Define an expression tree for a Lambda: x => x*x
        Expression<Fun<int, int>> squareExpr = x => x * x;

        // Print the expression
        Console.WriteLine("Expression: " + squareExpr);

        // Compile and invoke the expression 
        Fun<int, int> square = squareExpr.Compile();
        Console.WriteLine("Result of square(5): "+ square(5));
    }
}
```

**Summary:**  
The `=>` operator is used to define inline functions (lambdas) and concise member implementations, making code more readable and expressive, especially in LINQ and functional programming scenarios.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the null-conditional operator (?.) and how does it differ from the null-coalescing operator (??)?
---
The **null-conditional operator** (`?.`) and the **null-coalescing operator** (`??`) are both used in C# to simplify working with potentially null values, but they serve different purposes:

**1. Null-Conditional Operator (`?.`):**

The **null-conditional operator** (`?.`) allows you to safely access a member or method of an object that might be `null`. If the object is `null`, the expression returns `null` instead of throwing a `NullReferenceException`.

**Example:**
```csharp
Person person = null;
string name = person?.Name; // name is null, no exception thrown
```

**2. Null-Coalescing Operator** (`??`) 

The `??` operator provides a **default value** when the left hand operand is `null`. 

**Example:**
```csharp
string name = person?.Name ?? "Unknown"; // If person or Name is null, name is "Unknown"
```

**Usage Together:**
You can combine both:
```csharp
int? length = person?.Name?.Length ?? 0; // If person or Name is null, length is 0
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the default literal in C#?
---
The **default literal** in C# (introduced in C# 7.1) provides a concise way to represent the default value of a type without explicitly specifying the type. It is written simply as `default` (without a type in parentheses).

**Examples:** Without **default** literal (older style)

```csharp
// Before C# 7.1
int number = default(int); // 0
string text = default(string) // null
```

**Example:** With **default** literal (modern style)
```cs
// After C# 7.1
int number = default; // 0
string text = default; // null
```

**In generic methods:**

```cs
public T GetDefaultValue<T>()
{
    return default;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you explain the is not pattern introduced in C# 9.0?
---
The **`is not` pattern** introduced in C# 9.0 is a concise way to check if an object is *not* of a certain type or does *not* match a pattern.

**Syntax:**

Instead of writing:
```csharp
if (!(obj is string)) 
{  
    Console.WriteLine("Obj is not string");
}
```
You can now write:
```csharp
if (obj is not string) 
{  
    Console.WriteLine("Obj is not string");
}
```

This improves readability and reduces the need for extra parentheses.

**Example:**
```csharp
object value = 42;
if (value is not string)
{
    Console.WriteLine("Not a string!"); // Output: Not a string!
}
```

**Example:** Using Pattern Matching

You can also use it with more complex patterns:
```csharp
if (person is not Employee { IsActive: true }) {
    // person is either not an Employee or not active
}
```

**Benefits:**  
- More readable and expressive code.
- Works with all pattern matching scenarios.
- This pattern is especially useful in switch expressions and when working with pattern matching in modern C#.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 3. CLASSES

<br/>

## Q. What is object-oriented programming?

Object-oriented programming (OOP) in C# is a programming paradigm based on the concept of "objects", which are instances of classes. OOP enables developers to structure software in a modular way by organizing code into reusable components.

**Key principles:**

* **Encapsulation**: Bundles data and methods that operate on the data into a single unit called a class, and restricts direct access to some of the object\'s components.

* **Inheritance**: Allows a class to inherit members (fields, methods, properties) from another class, promoting code reuse.

* **Polymorphism**: Enables objects to be treated as instances of their parent class rather than their actual class, allowing for flexible and interchangeable code.

* **Abstraction**: Hides complex implementation details and exposes only the necessary features of an object.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a constructor in C# and what are its different types?

A **constructor** is a special method that is automatically called when an object is instantiated. It initializes the object\'s state. Constructors have the same name as the class and no return type.

**Types of constructors in C# (.NET 10 / C# 14):**

**1. Default (Parameterless) Constructor:**

Automatically provided by the compiler if no constructor is defined.

```cs
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}

var p = new Person(); // default constructor
```

**2. Parameterized Constructor:**

```cs
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }

    public Person(string name, int age)
    {
        Name = name;
        Age = age;
    }
}

var p = new Person("Pradeep", 30);
```

**3. Primary Constructor (C# 12+):**

Parameters declared directly on the class — concise and idiomatic in .NET 8+.

```cs
public class Employee(string name, int id)
{
    public string Name { get; } = name;
    public int Id { get; } = id;
    public override string ToString() => $"{Id}: {Name}";
}

var emp = new Employee("Pradeep", 101);
Console.WriteLine(emp); // Output: 101: Pradeep
```

**4. Copy Constructor:**

Creates a new object as a copy of an existing object.

```cs
public class Point
{
    public int X, Y;
    public Point(int x, int y) { X = x; Y = y; }
    public Point(Point other) { X = other.X; Y = other.Y; } // copy
}
```

**5. Static Constructor:**

Called once before any static members are accessed or any instance is created. Cannot have parameters or access modifiers.

```cs
public class Config
{
    public static readonly string AppName;

    static Config()
    {
        AppName = "MyApp"; // runs once, automatically
        Console.WriteLine("Static constructor called");
    }
}
```

**6. Private Constructor:**

Used in Singleton patterns or factory methods to prevent direct instantiation.

```cs
public class Singleton
{
    private static readonly Singleton _instance = new();
    private Singleton() { }
    public static Singleton Instance => _instance;
}
```

**7. Constructor Chaining (`this()` / `base()`):**

```cs
public class Shape
{
    public string Color { get; }
    public Shape(string color) { Color = color; }
}

public class Circle : Shape
{
    public double Radius { get; }

    public Circle(double radius) : this(radius, "Red") { }

    public Circle(double radius, string color) : base(color)
    {
        Radius = radius;
    }
}

var c = new Circle(5.0);
Console.WriteLine($"{c.Color} circle, radius={c.Radius}"); // Red circle, radius=5
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you explain the use of the "this" keyword in C#?

The `this` keyword refers to the **current instance** of a class. It has several uses:

| Use | Purpose |
|-----|---------|
| Disambiguate | Distinguish between a field and a parameter with the same name |
| Constructor chaining | Call another constructor in the same class via `this(...)` |
| Pass self as argument | Pass the current instance to a method or event |
| Extension methods | First parameter of an extension method refers to `this` |

```cs
// 1. Disambiguate field vs parameter
public class Person
{
    private string name;
    private int age;

    public Person(string name, int age)
    {
        this.name = name; // 'this.name' = field; 'name' = parameter
        this.age  = age;
    }
}

// 2. Constructor chaining — delegate to another constructor
public class Order
{
    public int    Id       { get; }
    public string Product  { get; }
    public int    Quantity { get; }

    public Order(int id, string product) : this(id, product, 1) { }

    public Order(int id, string product, int quantity)
    {
        Id       = id;
        Product  = product;
        Quantity = quantity;
    }
}

var o1 = new Order(1, "Laptop");       // quantity defaults to 1
var o2 = new Order(2, "Phone", 3);
Console.WriteLine($"{o1.Product} x{o1.Quantity}"); // Laptop x1
Console.WriteLine($"{o2.Product} x{o2.Quantity}"); // Phone x3

// 3. Pass current instance as argument
public class Button
{
    public event Action<Button>? Clicked;
    public void Click() => Clicked?.Invoke(this); // passes self
}

// 4. Extension method — 'this' marks the extended type
public static class StringExtensions
{
    public static string Shorten(this string value, int maxLength) =>
        value.Length <= maxLength ? value : value[..maxLength] + "...";
}

Console.WriteLine("Hello, World!".Shorten(5)); // Hello...

// 5. Fluent builder — return 'this' for method chaining
public class QueryBuilder
{
    private readonly List<string> _parts = [];

    public QueryBuilder Select(string cols) { _parts.Add($"SELECT {cols}"); return this; }
    public QueryBuilder From(string table)  { _parts.Add($"FROM {table}");  return this; }
    public QueryBuilder Where(string cond)  { _parts.Add($"WHERE {cond}");  return this; }
    public string Build() => string.Join(" ", _parts);
}

string sql = new QueryBuilder()
    .Select("*")
    .From("Products")
    .Where("Price > 100")
    .Build();
Console.WriteLine(sql); // SELECT * FROM Products WHERE Price > 100
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How are static constructors executed in a Parent/Child class hierarchy?

Static constructors are **type-initializers** — each runs exactly once, the first time its own class is used (accessed or instantiated). The execution order follows a simple rule: **the static constructor of the base class runs before the static constructor of the derived class**.

```cs
public class Base
{
    public static string Config;

    static Base()
    {
        Config = "Base initialized";
        Console.WriteLine("Base static constructor");
    }

    public Base() => Console.WriteLine("Base instance constructor");
}

public class Derived : Base
{
    public static string Extra;

    static Derived()
    {
        Extra = "Derived initialized";
        Console.WriteLine("Derived static constructor");
    }

    public Derived() => Console.WriteLine("Derived instance constructor");
}

// First use of Derived — triggers type initialization
var d = new Derived();
Console.WriteLine(Base.Config);
Console.WriteLine(Derived.Extra);
```

**Output:**
```
Base static constructor
Derived static constructor
Base instance constructor
Derived instance constructor
Base initialized
Derived initialized
```

**Key rules:**
- Static constructors run **before any instance constructor** of the same class.
- Base static constructor always runs **before** the derived static constructor.
- Static constructors are called **at most once** per AppDomain — guaranteed by the CLR.
- They run automatically; you cannot call them explicitly.
- They are thread-safe — the CLR ensures only one thread executes a static constructor.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. If a base class has overloaded constructors, can you enforce a call from an inherited constructor to a specific base constructor?

**Yes.** Use the `base(...)` initializer to chain to a specific base constructor. This is evaluated before the derived constructor body runs.

```cs
public class Vehicle
{
    public string Make  { get; }
    public string Model { get; }
    public int    Year  { get; }

    public Vehicle(string make, string model)
        : this(make, model, DateTime.UtcNow.Year) { }

    public Vehicle(string make, string model, int year)
    {
        Make  = make;
        Model = model;
        Year  = year;
        Console.WriteLine($"Vehicle({make}, {model}, {year})");
    }
}

public class ElectricVehicle : Vehicle
{
    public int BatteryKwh { get; }

    // Explicitly calls Vehicle(make, model) — two-param base constructor
    public ElectricVehicle(string make, string model, int batteryKwh)
        : base(make, model)
    {
        BatteryKwh = batteryKwh;
        Console.WriteLine($"ElectricVehicle battery={batteryKwh} kWh");
    }

    // Explicitly calls Vehicle(make, model, year) — three-param base constructor
    public ElectricVehicle(string make, string model, int year, int batteryKwh)
        : base(make, model, year)
    {
        BatteryKwh = batteryKwh;
        Console.WriteLine($"ElectricVehicle battery={batteryKwh} kWh");
    }
}

var ev1 = new ElectricVehicle("Tesla", "Model 3", 82);
// Vehicle(Tesla, Model 3, 2026)
// ElectricVehicle battery=82 kWh

var ev2 = new ElectricVehicle("Tesla", "Cybertruck", 2023, 123);
// Vehicle(Tesla, Cybertruck, 2023)
// ElectricVehicle battery=123 kWh
```

**Rules:**
- `base(...)` must be the **first** thing executed — before the derived constructor body.
- You can only call **one** base constructor per derived constructor.
- If no `base(...)` is specified, the compiler implicitly calls the **parameterless** base constructor; if none exists, the code will not compile.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you access the constructor of one class from another class?

There are several ways to invoke or chain constructors across classes:

```cs
// 1. base() — call a specific base class constructor from a derived constructor
public class Animal
{
    public string Name { get; }
    public Animal(string name) { Name = name; }
}

public class Dog : Animal
{
    public string Breed { get; }
    public Dog(string name, string breed) : base(name) // calls Animal(string)
    {
        Breed = breed;
    }
}

var dog = new Dog("Rex", "Labrador");
Console.WriteLine($"{dog.Name} — {dog.Breed}"); // Rex — Labrador

// 2. this() — call another constructor in the SAME class
public class Point
{
    public int X { get; }
    public int Y { get; }
    public int Z { get; }

    public Point(int x, int y) : this(x, y, 0) { }       // delegates to 3-param ctor
    public Point(int x, int y, int z) { X = x; Y = y; Z = z; }
}

// 3. Factory method — control instantiation from outside the class
public class Connection
{
    private Connection(string connectionString) { /* init */ }

    public static Connection Create(string connStr) => new Connection(connStr);
    public static Connection CreateDefault()       => new Connection("Server=localhost;");
}

var conn = Connection.Create("Server=prod;Database=MyDb;");

// 4. Activator.CreateInstance — dynamic instantiation via reflection
object instance = Activator.CreateInstance(typeof(Dog), "Buddy", "Poodle")!;
Console.WriteLine(((Dog)instance).Name); // Buddy

// 5. Dependency Injection (Microsoft.Extensions.DependencyInjection)
// The DI container resolves and calls constructors automatically
// services.AddScoped<IRepository, SqlRepository>();
// Constructor of SqlRepository is called by the container when resolved
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the use of static constructors?

A **static constructor** (also called a type initializer) is used to initialize **static fields** or perform **one-time setup logic** that runs before any static member is accessed or any instance of the class is created. It is declared with `static` and no access modifier or parameters.

**Common use cases:**

```cs
// 1. Initialize static fields that require computation
public class MathConstants
{
    public static readonly double GoldenRatio;
    public static readonly double NaturalLog2;

    static MathConstants()
    {
        GoldenRatio = (1 + Math.Sqrt(5)) / 2;
        NaturalLog2 = Math.Log(2);
        Console.WriteLine("MathConstants initialized");
    }
}

Console.WriteLine(MathConstants.GoldenRatio); // 1.618...

// 2. Load configuration or resources once
public class AppConfig
{
    public static readonly Dictionary<string, string> Settings;

    static AppConfig()
    {
        // Load from environment / file on first use — only once
        Settings = new Dictionary<string, string>
        {
            ["AppName"] = Environment.GetEnvironmentVariable("APP_NAME") ?? "MyApp",
            ["Version"] = "1.0.0",
        };
    }
}

// 3. Register types / set up factories
public class SerializerFactory
{
    private static readonly Dictionary<string, Func<string, object>> _parsers;

    static SerializerFactory()
    {
        _parsers = new()
        {
            ["json"] = data => System.Text.Json.JsonDocument.Parse(data),
            ["csv"]  = data => data.Split(','),
        };
    }

    public static object Parse(string format, string data) =>
        _parsers.TryGetValue(format, out var parser)
            ? parser(data)
            : throw new NotSupportedException(format);
}

// 4. Guarantee thread-safe singleton initialization (CLR ensures this automatically)
public class Singleton
{
    public static readonly Singleton Instance;

    static Singleton()
    {
        Instance = new Singleton();
        Console.WriteLine("Singleton created");
    }

    private Singleton() { }
}
```

**Key properties:**
- Runs **at most once** per AppDomain.
- Runs **before** the first instance is created or any static member is accessed.
- Is **thread-safe** — CLR guarantees single execution.
- Cannot have parameters or an access modifier.
- Cannot be called directly.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you explain the difference between a static and an instance method in C#?

| | Static Method | Instance Method |
|-|--------------|----------------|
| **Belongs to** | The type itself | An instance (object) of the type |
| **Access** | Called via `ClassName.Method()` | Called via `objectRef.Method()` |
| **`this` keyword** | Not available | Available — refers to the current object |
| **Instance members** | Cannot access directly | Can access all instance members |
| **Static members** | Can access | Can access |
| **Memory** | One copy per type | Logically one per instance (code shared, state per instance) |
| **Overridable** | Cannot be `virtual`/`override` | Can be `virtual`, `abstract`, `override` |

```cs
public class Counter
{
    // Static field — shared across all instances
    private static int _totalCreated = 0;

    // Instance field — each object has its own copy
    private int _count = 0;
    public string Name { get; }

    public Counter(string name)
    {
        Name = name;
        _totalCreated++;           // modify shared state
    }

    // Instance method — operates on this specific object\'s _count
    public void Increment() => _count++;
    public void Decrement() => _count--;
    public int  GetCount()  => _count;

    // Static method — no 'this', accesses only static members
    public static int GetTotalCreated() => _totalCreated;
    public static Counter Create(string name) => new Counter(name); // factory
}

var a = new Counter("A");
var b = new Counter("B");

a.Increment(); a.Increment(); a.Increment(); // a._count = 3
b.Increment();                               // b._count = 1

Console.WriteLine(a.GetCount());             // 3
Console.WriteLine(b.GetCount());             // 1
Console.WriteLine(Counter.GetTotalCreated()); // 2 (shared across all instances)

// Static utility classes — all static methods, no instance needed
public static class MathHelper
{
    public static double CircleArea(double radius)  => Math.PI * radius * radius;
    public static double HypotenuseLeg(double a, double b) => Math.Sqrt(a * a + b * b);
}

Console.WriteLine(MathHelper.CircleArea(5));       // 78.54...
Console.WriteLine(MathHelper.HypotenuseLeg(3, 4)); // 5
```

**When to choose:**
- **Static:** Pure functions, factory methods, utility/helper methods, operations that don\'t depend on instance state.
- **Instance:** Operations that read or modify object state; methods that should be polymorphic (`virtual`/`override`).

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is an abstract class in C# and when is it used?

An **abstract class** is a class that cannot be instantiated directly. It acts as a base class providing shared implementation while forcing derived classes to implement specific members via `abstract` methods.

**Key characteristics:**

- Declared with the `abstract` keyword.
- Can contain both abstract methods (no body) and concrete methods (with body).
- Can have constructors, fields, properties, and access modifiers.
- A derived class must implement all abstract members unless it is also abstract.

**Example (.NET 10):**

```cs
public abstract class Animal
{
    public string Name { get; }

    protected Animal(string name) => Name = name;

    // Abstract: must be overridden by derived classes
    public abstract string MakeSound();

    // Concrete: shared implementation
    public void Describe() =>
        Console.WriteLine($"{Name} says: {MakeSound()}");
}

public class Dog(string name) : Animal(name)
{
    public override string MakeSound() => "Woof!";
}

public class Cat(string name) : Animal(name)
{
    public override string MakeSound() => "Meow!";
}

Animal[] animals = [new Dog("Rex"), new Cat("Whiskers")];
foreach (var animal in animals)
    animal.Describe();
// Output:
// Rex says: Woof!
// Whiskers says: Meow!
```

**When to use abstract classes:**

- When multiple related classes share a common base implementation.
- When you want to enforce a contract (abstract members) while providing shared code.
- When you need constructors, state (fields), or access modifiers — things interfaces cannot provide (before C# 8).

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between an abstract class and an interface?

Both define contracts for derived types, but they differ in usage, capabilities, and design intent.

| Feature                          | Abstract Class                     | Interface (C# 8+)                      |
|----------------------------------|-------------------------------------|----------------------------------------|
| Instantiation                    | Cannot be instantiated              | Cannot be instantiated                 |
| Multiple inheritance             | Single base class only              | A class can implement many interfaces  |
| Fields / State                   | Can have fields and state           | No fields (only properties/methods)    |
| Constructors                     | Can have constructors               | Cannot have constructors               |
| Access modifiers                 | Supports all modifiers              | Members public by default              |
| Default implementations          | Yes (concrete methods)              | Yes (C# 8+ default interface methods)  |
| Static members                   | Yes                                 | Yes (C# 8+)                            |
| Use case                         | Shared base for related types       | Capability contract (unrelated types)  |

**Abstract class example:**

```cs
public abstract class Logger
{
    private readonly string _prefix = "[LOG]";
    public abstract void Write(string message);
    public void Info(string msg) => Write($"{_prefix} INFO: {msg}");
}
```

**Interface with default implementation (C# 8+):**

```cs
public interface ILogger
{
    void Write(string message);
    void Info(string msg) => Write($"[LOG] INFO: {msg}"); // default impl
}
```

**Guideline:** Use an **interface** to define a capability shared across unrelated types. Use an **abstract class** when related types share code and state.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is reflection in .NET and how would you use it?

**Reflection** is the ability to inspect and interact with type metadata (classes, methods, properties, attributes) at runtime using the `System.Reflection` namespace. It enables dynamic loading, instantiation, and invocation without knowing types at compile time.

**Common uses:** Serialization frameworks, dependency injection containers, ORMs, test runners, and source generators.

**Example — inspecting a type at runtime:**

```cs
using System.Reflection;

public class Product
{
    public required string Name { get; init; }
    public decimal Price { get; init; }
    public void Display() => Console.WriteLine($"{Name}: {Price:C}");
}

// Inspect type
Type type = typeof(Product);
Console.WriteLine($"Type: {type.Name}");

foreach (var prop in type.GetProperties())
    Console.WriteLine($"  Property: {prop.Name} ({prop.PropertyType.Name})");

foreach (var method in type.GetMethods(BindingFlags.Public | BindingFlags.Instance | BindingFlags.DeclaredOnly))
    Console.WriteLine($"  Method: {method.Name}");
```

**Example — dynamic instantiation and method invocation:**

```cs
object instance = Activator.CreateInstance(typeof(Product),
    new object[] { }) ?? throw new InvalidOperationException();

// Set properties via reflection
PropertyInfo? nameProp = typeof(Product).GetProperty("Name");
nameProp?.SetValue(instance, "Laptop");

// Invoke method
MethodInfo? display = typeof(Product).GetMethod("Display");
display?.Invoke(instance, null); // Output: Laptop: $0.00
```

**⚠️ .NET 10 note — prefer Source Generators over Reflection:**

Reflection has runtime overhead and is incompatible with **Native AOT**. In modern .NET, prefer:
- `System.Text.Json` source generators for serialization
- `Microsoft.Extensions.DependencyInjection` for DI
- `[GeneratedRegex]` for compiled regex
- `IIncrementalGenerator` for compile-time code generation

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a sealed class in C# and why is it used?

A **sealed class** is a class that cannot be inherited. Applying the `sealed` modifier prevents other classes from deriving from it.

**Why use it:**
- **Security & correctness:** Prevents unintended overriding that could break invariants.
- **Performance:** The JIT compiler and Native AOT can devirtualize sealed class method calls, improving performance.
- **Design intent:** Signals that the type is complete and not designed for extension.

**Example:**

```cs
public sealed class ImmutablePoint
{
    public int X { get; }
    public int Y { get; }

    public ImmutablePoint(int x, int y) => (X, Y) = (x, y);
    public double DistanceTo(ImmutablePoint other)
        => Math.Sqrt(Math.Pow(X - other.X, 2) + Math.Pow(Y - other.Y, 2));
    public override string ToString() => $"({X}, {Y})";
}

// var p = new DerivedPoint(); // Compile error — cannot inherit from sealed class
```

**Sealed methods in unsealed classes:**

You can seal a specific `override` to stop further overriding while keeping the class inheritable:

```cs
public class Base
{
    public virtual void Render() => Console.WriteLine("Base.Render");
}

public class Derived : Base
{
    public sealed override void Render() => Console.WriteLine("Derived.Render"); // no further override
}
```

**Note:** In .NET, many BCL types like `string`, `StringBuilder`, and `HttpClient` are `sealed` for performance and security.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the benefits of using a sealed class in C#?

**1. Performance (JIT / Native AOT devirtualization):**

When the JIT compiler or Native AOT knows a class is `sealed`, it can replace virtual method dispatch with direct calls — eliminating the vtable lookup overhead.

```cs
public sealed class PriceCalculator
{
    public decimal Calculate(decimal basePrice, decimal taxRate)
        => basePrice * (1 + taxRate);
}

var calc = new PriceCalculator();
// JIT devirtualizes Calculate() — compiled as a direct call, no vtable
Console.WriteLine(calc.Calculate(100m, 0.18m)); // Output: 118
```

**2. Security & design correctness:**

Prevents derived classes from overriding behaviour in ways that break invariants or security contracts.

```cs
public sealed class JwtTokenValidator
{
    private readonly string _secret;
    public JwtTokenValidator(string secret) => _secret = secret;

    public bool Validate(string token)
    {
        // Cannot be overridden and weakened by a subclass
        return token.StartsWith("valid"); // simplified
    }
}
```

**3. Signals clear design intent:**

Tells consumers of your API: *"This type is complete — do not extend it."*

**4. Supports pattern matching optimisation:**

The C# compiler and JIT can generate exhaustiveness checks and optimise `switch` expressions when the type hierarchy is closed (via `sealed`).

```cs
public abstract class Shape { }
public sealed class Circle(double Radius) : Shape;
public sealed class Rectangle(double W, double H) : Shape;

double Area(Shape s) => s switch
{
    Circle c    => Math.PI * c.Radius * c.Radius,
    Rectangle r => r.W * r.H,
    _           => throw new ArgumentOutOfRangeException()
};
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Is it possible for a sealed class to be used as a base class in C#?

**No.** A `sealed` class cannot be used as a base class. Attempting to inherit from it produces a compile-time error.

```cs
public sealed class Logger
{
    public void Log(string message) => Console.WriteLine(message);
}

// Compile error: 'FileLogger' cannot derive from sealed type 'Logger'
// public class FileLogger : Logger { }
```

**Why:** The entire purpose of `sealed` is to prevent inheritance. The compiler enforces this as an error, not a warning.

**Workaround — use composition instead of inheritance:**

```cs
public class FileLogger
{
    private readonly Logger _logger = new Logger(); // compose, don\'t inherit

    public void Log(string path, string message)
    {
        File.AppendAllText(path, message + Environment.NewLine);
        _logger.Log(message); // delegate to sealed class
    }
}
```

**Note:** Many .NET BCL types are sealed for this reason — `string`, `StringBuilder`, `HttpClient`, `DateTime`. You extend their behaviour via extension methods or wrapper classes, not inheritance.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Is it possible for a sealed class in C# to define virtual methods?

**No.** A `sealed` class cannot declare new `virtual` methods. Since the class cannot be inherited, virtual methods would serve no purpose and the compiler rejects them.

```cs
// Compile error: 'SealedClass.Method()' cannot be virtual because 'SealedClass' is sealed
// public sealed class SealedClass
// {
//     public virtual void Method() { }
// }
```

**However**, a `sealed` class **can override** a `virtual` method from a base class, and it can mark that override as `sealed override` to stop further overriding (though once the class itself is sealed, no further derivation is possible anyway).

```cs
public abstract class Animal
{
    public virtual string Speak() => "...";
}

public sealed class Cat : Animal
{
    // OK: overriding a virtual method from the base class
    public override string Speak() => "Meow";
}

var cat = new Cat();
Console.WriteLine(cat.Speak()); // Output: Meow
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Is it possible for a non-child class to define sealed methods in C#?

**No.** The `sealed` modifier on a method is only valid on an **override** method — it means *"stop allowing further overriding of this inherited virtual method"*. A class that is not part of an inheritance chain cannot use `sealed` on a method.

```cs
// Compile error: 'MyClass.Method()' cannot be sealed because it is not an override
// public class MyClass
// {
//     public sealed void Method() { }  // ERROR
// }
```

**Valid use — `sealed override` in a derived class:**

```cs
public class Base
{
    public virtual void Render() => Console.WriteLine("Base.Render");
}

public class Derived : Base
{
    // sealed override: stops any further class from overriding Render()
    public sealed override void Render() => Console.WriteLine("Derived.Render");
}

public class LeafDerived : Derived
{
    // Compile error: cannot override sealed member 'Derived.Render()'
    // public override void Render() { }
}

var d = new Derived();
d.Render(); // Output: Derived.Render
```

**Summary:** `sealed` on a method requires the method to be an `override`. It cannot be applied to brand-new methods or methods in classes that are not part of an inheritance chain.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can abstract classes be used to implement the Template Method design pattern?

The **Template Method** pattern defines the skeleton of an algorithm in a base class, deferring specific steps to derived classes. The base class calls abstract (or virtual) methods in a fixed sequence — derived classes fill in the details without altering the overall flow.

**Abstract classes are ideal here** because they can have both the concrete template method (the skeleton) and abstract hook methods (the steps).

**Example — Report generation pipeline (.NET 10):**

```cs
public abstract class ReportGenerator
{
    // Template method — defines the fixed algorithm skeleton
    public void Generate()
    {
        FetchData();
        FormatData();
        Render();
        SendReport();
    }

    protected abstract void FetchData();   // must be implemented
    protected abstract void FormatData();  // must be implemented
    protected abstract void Render();      // must be implemented

    // Optional hook with default behaviour
    protected virtual void SendReport()
        => Console.WriteLine("Report saved locally.");
}

public class PdfReportGenerator : ReportGenerator
{
    protected override void FetchData()   => Console.WriteLine("Fetching data from SQL...");
    protected override void FormatData()  => Console.WriteLine("Formatting as PDF...");
    protected override void Render()      => Console.WriteLine("Rendering PDF...");
    protected override void SendReport()  => Console.WriteLine("Emailing PDF report.");
}

public class ExcelReportGenerator : ReportGenerator
{
    protected override void FetchData()  => Console.WriteLine("Fetching data from API...");
    protected override void FormatData() => Console.WriteLine("Formatting as Excel...");
    protected override void Render()     => Console.WriteLine("Rendering Excel file...");
    // Uses default SendReport() — saved locally
}

// Usage
ReportGenerator pdf = new PdfReportGenerator();
pdf.Generate();
Console.WriteLine();

ReportGenerator excel = new ExcelReportGenerator();
excel.Generate();
```

**Output:**
```
Fetching data from SQL...
Formatting as PDF...
Rendering PDF...
Emailing PDF report.

Fetching data from API...
Formatting as Excel...
Rendering Excel file...
Report saved locally.
```

**Key takeaway:** The base class (`ReportGenerator`) controls *when* each step runs. Subclasses control *what* each step does.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you give an example of how abstract classes can lead to implementation of anti-patterns such as the God Object pattern?

The **God Object** anti-pattern occurs when a single class takes on too many responsibilities. If a poorly-designed abstract base class accumulates abstractions for unrelated concerns, every subclass inherits this bloat — making the hierarchy rigid, hard to test, and hard to maintain.

**Bad example — God Object abstract class:**

```cs
// BAD: one abstract class handles authentication, logging, emailing, AND data access
public abstract class BaseService
{
    public abstract bool Authenticate(string username, string password);
    public abstract void LogActivity(string message);
    public abstract void SendEmail(string to, string body);
    public abstract IEnumerable<object> GetData(string query);
    public abstract void SaveData(object entity);
    public abstract void GenerateReport();
}

// Every derived class is forced to implement ALL of the above
public class UserService : BaseService
{
    public override bool Authenticate(string u, string p) => true;
    public override void LogActivity(string msg) => Console.WriteLine(msg);
    public override void SendEmail(string to, string body) { /* email logic */ }
    public override IEnumerable<object> GetData(string q) => Enumerable.Empty<object>();
    public override void SaveData(object e) { }
    public override void GenerateReport() { /* report logic */ }
}
```

**Problems:**
- Violates **SRP** — one class owns authentication, logging, email, data, and reports.
- Every subclass is forced to implement unrelated methods.
- Testing `UserService` requires stubbing all six unrelated concerns.

**Good example — Segregated interfaces + abstract class per concern (SOLID):**

```cs
public interface IAuthService   { bool Authenticate(string username, string password); }
public interface IEmailService  { void Send(string to, string body); }
public interface IDataRepository<T> { IEnumerable<T> GetAll(); void Save(T entity); }

public abstract class BaseUserService(IAuthService auth, IEmailService email)
{
    protected readonly IAuthService Auth = auth;
    protected readonly IEmailService Email = email;

    public abstract void OnUserRegistered(string username);
}

public class UserRegistrationService(IAuthService auth, IEmailService email)
    : BaseUserService(auth, email)
{
    public override void OnUserRegistered(string username)
    {
        Email.Send(username, "Welcome!");
        Console.WriteLine($"User {username} registered.");
    }
}
```

**Key takeaway:** Abstract classes should represent a single coherent abstraction. Use interfaces to compose unrelated capabilities rather than cramming them into one base class.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can abstract classes be used to implement the Factory Method pattern?

The **Factory Method** pattern defines an abstract method for creating an object, letting subclasses decide which concrete type to instantiate. The base class orchestrates the workflow but delegates object creation to derived classes.

**Example — Notification system (.NET 10):**

```cs
// Product: the object being created
public abstract class Notification
{
    public abstract void Send(string recipient, string message);
}

public class EmailNotification : Notification
{
    public override void Send(string recipient, string message)
        => Console.WriteLine($"Email to {recipient}: {message}");
}

public class SmsNotification : Notification
{
    public override void Send(string recipient, string message)
        => Console.WriteLine($"SMS to {recipient}: {message}");
}

public class PushNotification : Notification
{
    public override void Send(string recipient, string message)
        => Console.WriteLine($"Push to {recipient}: {message}");
}

// Creator: defines the factory method and uses it in a template
public abstract class NotificationService
{
    // Factory method — subclasses decide what type to create
    protected abstract Notification CreateNotification();

    // Template: uses the factory method
    public void Notify(string recipient, string message)
    {
        var notification = CreateNotification(); // polymorphic creation
        notification.Send(recipient, message);
    }
}

// Concrete creators
public class EmailNotificationService : NotificationService
{
    protected override Notification CreateNotification() => new EmailNotification();
}

public class SmsNotificationService : NotificationService
{
    protected override Notification CreateNotification() => new SmsNotification();
}

public class PushNotificationService : NotificationService
{
    protected override Notification CreateNotification() => new PushNotification();
}

// Usage — client code depends on the abstract creator, not concrete types
NotificationService[] services =
[
    new EmailNotificationService(),
    new SmsNotificationService(),
    new PushNotificationService(),
];

foreach (var service in services)
    service.Notify("pradeep@example.com", "Your order has shipped!");
```

**Output:**
```
Email to pradeep@example.com: Your order has shipped!
SMS to pradeep@example.com: Your order has shipped!
Push to pradeep@example.com: Your order has shipped!
```

**Key benefits:**
- Client code (`Notify`) is decoupled from concrete notification types.
- Adding a new channel (e.g., `WhatsAppNotification`) requires only a new subclass — no changes to existing code (**Open/Closed Principle**).

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the SOLID principles in C#?

**SOLID** is an acronym for five object-oriented design principles that lead to more maintainable, scalable, and testable software.

**1. S — Single Responsibility Principle (SRP)**

A class should have only one reason to change.

```cs
// Bad: one class does everything
public class Report { public void Generate() {} public void Save() {} public void Print() {} }

// Good: separate responsibilities
public class ReportGenerator { public string Generate() => "Report data"; }
public class ReportSaver { public void Save(string data, string path) => File.WriteAllText(path, data); }
```

**2. O — Open/Closed Principle (OCP)**

Open for extension, closed for modification. Use abstractions and polymorphism.

```cs
public abstract class Discount { public abstract decimal Apply(decimal price); }
public class SeasonalDiscount : Discount { public override decimal Apply(decimal p) => p * 0.9m; }
public class LoyaltyDiscount : Discount { public override decimal Apply(decimal p) => p * 0.85m; }
```

**3. L — Liskov Substitution Principle (LSP)**

Derived types must be substitutable for their base types without altering correctness.

```cs
public class Bird { public virtual void Fly() => Console.WriteLine("Flying"); }
public class Eagle : Bird { public override void Fly() => Console.WriteLine("Eagle soaring"); }
// Penguin cannot fly — violates LSP if it inherits Bird with Fly()
```

**4. I — Interface Segregation Principle (ISP)**

Clients should not be forced to depend on interfaces they don\'t use.

```cs
public interface IPrintable { void Print(); }
public interface IScannable { void Scan(); }

// Implement only what you need
public class SimplePrinter : IPrintable { public void Print() => Console.WriteLine("Printing..."); }
public class AllInOne : IPrintable, IScannable
{
    public void Print() => Console.WriteLine("Printing...");
    public void Scan() => Console.WriteLine("Scanning...");
}
```

**5. D — Dependency Inversion Principle (DIP)**

High-level modules should not depend on low-level modules. Both should depend on abstractions.

```cs
public interface IEmailSender { void Send(string to, string body); }
public class SmtpEmailSender : IEmailSender
{
    public void Send(string to, string body) => Console.WriteLine($"SMTP: {to} -> {body}");
}

public class OrderService(IEmailSender emailSender) // DI via primary constructor
{
    public void PlaceOrder(string item)
    {
        Console.WriteLine($"Order placed: {item}");
        emailSender.Send("customer@email.com", $"Your {item} is confirmed!");
    }
}

// Usage (.NET DI container)
// services.AddScoped<IEmailSender, SmtpEmailSender>();
// services.AddScoped<OrderService>();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can abstract classes be used to implement the Dependency Inversion principle?

The **Dependency Inversion Principle (DIP)** states that high-level modules should depend on abstractions (abstract classes or interfaces), not on concrete implementations. Abstract classes act as the abstraction layer.

**Example — payment processing (.NET 10):**

```cs
// Abstraction (abstract class)
public abstract class PaymentProcessor
{
    public abstract bool ProcessPayment(decimal amount);
    public abstract void Refund(decimal amount);

    // Shared template logic
    public bool ExecuteTransaction(decimal amount)
    {
        Console.WriteLine($"Processing transaction: {amount:C}");
        return ProcessPayment(amount);
    }
}

// Low-level modules (concrete implementations)
public class StripeProcessor : PaymentProcessor
{
    public override bool ProcessPayment(decimal amount)
    { Console.WriteLine($"Stripe charge: {amount:C}"); return true; }
    public override void Refund(decimal amount)
        => Console.WriteLine($"Stripe refund: {amount:C}");
}

public class PayPalProcessor : PaymentProcessor
{
    public override bool ProcessPayment(decimal amount)
    { Console.WriteLine($"PayPal charge: {amount:C}"); return true; }
    public override void Refund(decimal amount)
        => Console.WriteLine($"PayPal refund: {amount:C}");
}

// High-level module depends on abstraction, NOT on Stripe or PayPal
public class OrderService(PaymentProcessor processor)
{
    public void PlaceOrder(string item, decimal price)
    {
        Console.WriteLine($"Order: {item}");
        processor.ExecuteTransaction(price);
    }
}

// Usage — swap processor without changing OrderService
var service = new OrderService(new StripeProcessor());
service.PlaceOrder("Laptop", 999m);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between an abstract class and a concrete class in C#, and how does this relate to the Open-Closed principle?

- **Abstract class**: Cannot be instantiated; may have abstract (bodyless) members that subclasses must implement. Defines *what* must be done.
- **Concrete class**: Can be instantiated; provides full implementations for all members.

**Relationship to Open-Closed Principle (OCP):** A system is *open for extension* (add new concrete subclasses) but *closed for modification* (the abstract base class never changes). You extend behavior by adding new subclasses rather than editing existing code.

```cs
// Abstract base — CLOSED for modification
public abstract class TaxCalculator
{
    public abstract decimal GetRate();        // must override
    public decimal Calculate(decimal price) => price * GetRate(); // fixed template
}

// Concrete classes — OPEN for extension (add without touching base)
public class UkTaxCalculator   : TaxCalculator { public override decimal GetRate() => 0.20m; }
public class UsTaxCalculator   : TaxCalculator { public override decimal GetRate() => 0.15m; }
public class IndianTaxCalculator : TaxCalculator { public override decimal GetRate() => 0.18m; }

// Usage
TaxCalculator[] calculators = [new UkTaxCalculator(), new UsTaxCalculator(), new IndianTaxCalculator()];
foreach (var c in calculators)
    Console.WriteLine($"{c.GetType().Name}: {c.Calculate(1000m):C}");
// Output:
// UkTaxCalculator: £200.00 ... etc.
```

Adding a `CanadaTaxCalculator` requires **no changes** to `TaxCalculator` or existing classes — that is OCP in action.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you give an example of how abstract classes can help to enforce the Interface Segregation principle?

The **Interface Segregation Principle (ISP)** says clients should not be forced to depend on methods they don\'t use. Abstract classes enforce ISP by providing small, focused abstractions — each abstract class defines only the operations relevant to one concern.

```cs
// Focused abstract classes (not one God class with 10 abstract methods)
public abstract class DataReader
{
    public abstract IEnumerable<string> Read(string source);
}

public abstract class DataWriter
{
    public abstract void Write(string destination, IEnumerable<string> data);
}

public abstract class DataTransformer
{
    public abstract IEnumerable<string> Transform(IEnumerable<string> input);
}

// Concrete implementations only inherit what they need
public class CsvReader : DataReader
{
    public override IEnumerable<string> Read(string path) =>
        File.ReadAllLines(path);
}

public class UpperCaseTransformer : DataTransformer
{
    public override IEnumerable<string> Transform(IEnumerable<string> input) =>
        input.Select(s => s.ToUpperInvariant());
}

public class ConsoleWriter : DataWriter
{
    public override void Write(string _, IEnumerable<string> data)
    {
        foreach (var line in data) Console.WriteLine(line);
    }
}

// Pipeline: each class only depends on its own focused abstraction
var reader      = new CsvReader();
var transformer = new UpperCaseTransformer();
var writer      = new ConsoleWriter();

var lines = reader.Read("data.csv");
var transformed = transformer.Transform(lines);
writer.Write("", transformed);
```

**Key takeaway:** `CsvReader` never knows about writing; `ConsoleWriter` never knows about reading — each abstract class is small and focused.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can abstract classes be used to implement the Strategy design pattern?

The **Strategy** pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. Abstract classes serve as the strategy contract, while concrete subclasses are the actual strategies.

**Example — sorting strategies (.NET 10):**

```cs
// Strategy contract
public abstract class SortStrategy
{
    public abstract void Sort(List<int> data);
    public void Execute(List<int> data)
    {
        Console.WriteLine($"Before: {string.Join(", ", data)}");
        Sort(data);
        Console.WriteLine($"After:  {string.Join(", ", data)}");
    }
}

// Concrete strategies
public class BubbleSortStrategy : SortStrategy
{
    public override void Sort(List<int> data)
    {
        // simplified bubble sort
        for (int i = 0; i < data.Count - 1; i++)
            for (int j = 0; j < data.Count - 1 - i; j++)
                if (data[j] > data[j + 1])
                    (data[j], data[j + 1]) = (data[j + 1], data[j]);
    }
}

public class LinqSortStrategy : SortStrategy
{
    public override void Sort(List<int> data)
    {
        var sorted = data.OrderBy(x => x).ToList();
        data.Clear();
        data.AddRange(sorted);
    }
}

// Context: accepts any strategy at runtime
public class Sorter(SortStrategy strategy)
{
    private SortStrategy _strategy = strategy;
    public void SetStrategy(SortStrategy strategy) => _strategy = strategy;
    public void Sort(List<int> data) => _strategy.Execute(data);
}

// Usage — swap strategies at runtime
var data = new List<int> { 5, 3, 8, 1, 9, 2 };
var sorter = new Sorter(new BubbleSortStrategy());
sorter.Sort(data);

data = [5, 3, 8, 1, 9, 2];
sorter.SetStrategy(new LinqSortStrategy());
sorter.Sort(data);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Is it possible to declare abstract methods as private in C#?

**No.** Abstract methods cannot be `private`. An abstract method must be overridden by a derived class, but a `private` member is not visible to derived classes — making it impossible to override.

The compiler enforces this with an error:

```
error CS0621: 'MyClass.Method()': virtual or abstract members cannot be private
```

**Valid access modifiers for abstract methods:**

| Modifier             | Allowed on abstract method? |
|----------------------|-----------------------------|
| `public`             | ✅ Yes                      |
| `protected`          | ✅ Yes (most common)        |
| `internal`           | ✅ Yes                      |
| `protected internal` | ✅ Yes                      |
| `private`            | ❌ No — compile error       |
| `private protected`  | ❌ No — compile error       |

```cs
public abstract class Shape
{
    public abstract double Area();       // ✅ public
    protected abstract string Describe(); // ✅ protected
    // private abstract void Init();    // ❌ compile error
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Is it possible for an abstract class to contain a main method in C#?

**Yes.** An abstract class can contain a `static void Main` (or `static Task Main`) method. The `Main` method is `static`, so it belongs to the type itself — not to any instance — and can coexist with abstract instance members.

```cs
public abstract class ApplicationBase
{
    // Abstract instance member — cannot instantiate ApplicationBase directly
    public abstract string GetAppName();

    // Concrete shared logic
    protected void PrintBanner()
        => Console.WriteLine($"=== {GetAppName()} ===");

    // Entry point — perfectly valid on an abstract class
    public static void Main(string[] args)
    {
        // Cannot do: new ApplicationBase() — it\'s abstract
        // But we can instantiate a concrete subclass:
        ApplicationBase app = new ConsoleApp();
        app.PrintBanner();
        Console.WriteLine("Main running in abstract class.");
    }
}

public class ConsoleApp : ApplicationBase
{
    public override string GetAppName() => "MyConsoleApp";
}
// Output:
// === MyConsoleApp ===
// Main running in abstract class.
```

**Note:** In modern .NET (C# 9+), top-level statements (`Program.cs` with no class) are the preferred entry point — you rarely put `Main` in any class, abstract or not.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Is it possible in C# that a class inherit from multiple abstract classes?

**No.** C# does not support multiple class inheritance — a class can inherit from **only one** base class (abstract or concrete). This is by design to avoid the "diamond problem".

```cs
public abstract class Logger    { public abstract void Log(string msg); }
public abstract class Formatter { public abstract string Format(string msg); }

// Compile error: 'MyService' cannot have multiple base classes
// public class MyService : Logger, Formatter { }
```

**Workaround — use multiple interfaces:**

Interfaces (including default interface method implementations in C# 8+) allow a class to implement multiple contracts:

```cs
public interface ILogger    { void Log(string msg); }
public interface IFormatter { string Format(string msg); }

public class MyService : ILogger, IFormatter
{
    public void Log(string msg) => Console.WriteLine($"[LOG] {msg}");
    public string Format(string msg) => msg.ToUpperInvariant();
}

var svc = new MyService();
svc.Log(svc.Format("hello")); // Output: [LOG] HELLO
```

**Summary:** Single class inheritance + multiple interface implementation is the C# pattern for combining multiple contracts.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between a sealed class and an unsealed class in C#?

| Feature              | Sealed class                         | Unsealed class                         |
|----------------------|--------------------------------------|----------------------------------------|
| Inheritance          | Cannot be inherited                  | Can be inherited                       |
| Virtual methods      | Cannot declare new `virtual` methods | Can declare `virtual` methods          |
| Performance          | JIT can devirtualize — faster calls  | Virtual dispatch overhead              |
| Design intent        | Type is complete, extension forbidden| Type is designed to be extended        |
| Example in BCL       | `string`, `HttpClient`, `DateTime`   | `Stream`, `Exception`, `DbContext`     |

```cs
// Sealed — cannot inherit
public sealed class Circle
{
    public double Radius { get; }
    public Circle(double r) => Radius = r;
    public double Area() => Math.PI * Radius * Radius;
}

// Unsealed — can be inherited and extended
public class Shape
{
    public virtual double Area() => 0;
}

public class Rectangle(double w, double h) : Shape
{
    public override double Area() => w * h;
}

Shape s = new Rectangle(4, 5);
Console.WriteLine(s.Area()); // Output: 20
```

**When to seal:** Use `sealed` when you want to prevent unintended subclassing, improve performance, or express that the type is intentionally final.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can you use a sealed class to prevent inheritance in C#?

Apply the `sealed` modifier to a class declaration. The compiler will then reject any attempt to derive from it.

```cs
public sealed class DatabaseConnection
{
    private readonly string _connectionString;

    public DatabaseConnection(string cs) => _connectionString = cs;

    public void Open()  => Console.WriteLine($"Opening: {_connectionString}");
    public void Close() => Console.WriteLine("Closing connection.");
}

// Compile error: cannot derive from sealed type 'DatabaseConnection'
// public class SqlConnection : DatabaseConnection { }
```

**To seal only a specific method** in an otherwise inheritable class, use `sealed override`:

```cs
public class Vehicle
{
    public virtual void StartEngine() => Console.WriteLine("Engine started");
}

public class ElectricCar : Vehicle
{
    // No subclass of ElectricCar can override StartEngine further
    public sealed override void StartEngine() => Console.WriteLine("Silent electric start");
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can we do Multiple inheritance with Abstract classes?

**No.** C# does not support multiple class inheritance — a class can have only **one** base class, whether abstract or concrete. This prevents the diamond-inheritance ambiguity problem.

```cs
public abstract class Flyable  { public abstract void Fly(); }
public abstract class Swimmable { public abstract void Swim(); }

// Compile error: cannot have multiple base classes
// public class Duck : Flyable, Swimmable { }
```

**Solution — use multiple interfaces:**

```cs
public interface IFlyable  { void Fly(); }
public interface ISwimmable { void Swim(); }

public class Duck : IFlyable, ISwimmable
{
    public void Fly()  => Console.WriteLine("Duck flying");
    public void Swim() => Console.WriteLine("Duck swimming");
}

var duck = new Duck();
duck.Fly();   // Output: Duck flying
duck.Swim();  // Output: Duck swimming
```

A class can inherit from **one abstract class** and implement **many interfaces** simultaneously.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between Abstract class and interface?

See the detailed comparison in [What is the difference between an abstract class and an interface?](#q-what-is-the-difference-between-an-abstract-class-and-an-interface) above.

**Quick summary:**

| Aspect            | Abstract Class             | Interface (C# 8+)           |
|-------------------|----------------------------|-----------------------------|
| Inheritance       | Single only                | Implement many              |
| State (fields)    | Yes                        | No                          |
| Constructors      | Yes                        | No                          |
| Default methods   | Yes (concrete methods)     | Yes (C# 8+ default impl)    |
| Use when          | Related types share state/code | Unrelated types share a capability |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Why simple base class replace Abstract class?

A simple (concrete) base class **can** replace an abstract class when:
- Every method has a sensible default implementation (no unimplemented contract needed).
- You want to allow instantiation of the base class itself.
- Derived classes are optional extensions, not mandatory completions.

**When to prefer a simple base class:**

```cs
// All methods have defaults — no abstract needed
public class Logger
{
    public virtual void Log(string msg) => Console.WriteLine($"[INFO] {msg}");
    public virtual void Error(string msg) => Console.WriteLine($"[ERROR] {msg}");
}

public class FileLogger : Logger
{
    public override void Log(string msg) =>
        File.AppendAllText("app.log", msg + Environment.NewLine);
}

// Base class is usable on its own — fine as concrete
var log = new Logger();
log.Log("Starting app");
```

**When abstract is the right choice:** Use `abstract` when the base class cannot meaningfully function on its own and derived classes *must* provide implementation (e.g., `Area()` on `Shape`).

**Rule of thumb:** If the base class can stand alone and all methods have reasonable defaults → concrete base class. If the base class is incomplete without subclasses → abstract class.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are nested classes and when to use them?

A **nested class** is a class defined inside another class. It has access to the outer class\'s private members and is logically tied to it.

**When to use:**
- The nested type is an implementation detail of the outer class only.
- The nested type only makes sense in the context of the outer class (e.g., `Node` inside `LinkedList<T>`).
- Builder or helper types that assist the outer class.

```cs
public class LinkedList<T>
{
    private Node? _head;

    // Nested class — only LinkedList needs to know about Node
    private class Node
    {
        public T Value;
        public Node? Next;
        public Node(T value) { Value = value; }
    }

    public void Add(T value)
    {
        var node = new Node(value);
        node.Next = _head;
        _head = node;
    }

    public void Print()
    {
        for (var n = _head; n != null; n = n.Next)
            Console.Write($"{n.Value} ");
        Console.WriteLine();
    }
}

var list = new LinkedList<int>();
list.Add(1); list.Add(2); list.Add(3);
list.Print(); // Output: 3 2 1
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can Nested class access outer class variables?

**Yes**, a nested class can access `private` and `protected` **static** members of the outer class directly. For **instance** members of the outer class, the nested class needs a reference to the outer instance.

```cs
public class Outer
{
    private static string _staticSecret = "outer-static";
    private string _instanceSecret = "outer-instance";

    public class Inner
    {
        public void ShowStatic()
            => Console.WriteLine(_staticSecret); // ✅ direct access to outer static

        public void ShowInstance(Outer outer)
            => Console.WriteLine(outer._instanceSecret); // ✅ via outer reference
    }
}

var inner = new Outer.Inner();
inner.ShowStatic();               // Output: outer-static
inner.ShowInstance(new Outer());  // Output: outer-instance
```

**Key point:** The nested class has special visibility into the outer class\'s `private` members — this is unlike a regular external class.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can we have public, protected access modifiers in nested class?

**Yes.** A nested class can have any access modifier: `public`, `protected`, `internal`, `private`, `protected internal`, or `private protected`.

```cs
public class OuterClass
{
    // public nested — accessible from anywhere
    public class PublicNested
    {
        public void Hello() => Console.WriteLine("Public nested");
    }

    // protected nested — accessible only in OuterClass and its subclasses
    protected class ProtectedNested
    {
        public void Hello() => Console.WriteLine("Protected nested");
    }

    // private nested — only accessible within OuterClass
    private class PrivateNested
    {
        public void Hello() => Console.WriteLine("Private nested");
    }

    public void Demo()
    {
        new PublicNested().Hello();    // ✅
        new ProtectedNested().Hello(); // ✅
        new PrivateNested().Hello();   // ✅
    }
}

public class Derived : OuterClass
{
    public void Test()
    {
        new PublicNested().Hello();    // ✅
        new ProtectedNested().Hello(); // ✅ — accessible via inheritance
        // new PrivateNested().Hello(); // ❌ not accessible
    }
}

// From outside:
new OuterClass.PublicNested().Hello(); // ✅
// new OuterClass.ProtectedNested();   // ❌
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Are private class members inherited to the derived class?

**Private members are inherited but not accessible** in derived classes. They exist in the derived object\'s memory layout but cannot be referenced by name in the derived class code.

```cs
public class Base
{
    private int _secret = 42;             // inherited but inaccessible
    protected int Protected = 100;        // inherited and accessible
    public string Name { get; set; } = "Base";
}

public class Derived : Base
{
    public void Show()
    {
        Console.WriteLine(Name);      // ✅ public member
        Console.WriteLine(Protected); // ✅ protected member
        // Console.WriteLine(_secret); // ❌ compile error — private
    }
}
```

**Note:** You can access `private` base members indirectly via `public` or `protected` methods/properties of the base class.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Where is a protected class-level variable available?

A `protected` member is available:
1. Within the **same class** where it is declared.
2. In any **derived class** (regardless of assembly).

It is **not** accessible from unrelated classes or from outside the inheritance hierarchy.

```cs
public class Animal
{
    protected string Species = "Unknown"; // available here and in subclasses
}

public class Dog : Animal
{
    public void ShowSpecies()
        => Console.WriteLine(Species); // ✅ accessible in derived class
}

// Outside the hierarchy:
var a = new Animal();
// Console.WriteLine(a.Species); // ❌ compile error — not accessible here
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Are private class-level variables inherited?

Yes, they are part of the object\'s memory, but **not accessible by name** in derived classes. See [Are private class members inherited to the derived class?](#q-are-private-class-members-inherited-to-the-derived-class) above for a full example.

**Quick answer:** `private` members are **inherited (exist in memory)** but **not accessible (compile error if referenced)** in derived classes.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Which class is at the top of .NET class hierarchy?

`System.Object` (`object` in C#) is at the top of the .NET class hierarchy. Every class, struct (when boxed), and record ultimately derives from `System.Object`.

```cs
// All of these inherit from System.Object:
object o    = new object();
string s    = "hello";     // string → object
int boxed   = 42;          // int → ValueType → object (when boxed)
object arr  = new int[5];  // Array → object

Console.WriteLine(typeof(string).BaseType);     // System.Object
Console.WriteLine(typeof(Exception).BaseType);  // System.Object
Console.WriteLine(42.GetType().BaseType);       // System.ValueType

// object provides: Equals(), GetHashCode(), ToString(), GetType(), MemberwiseClone()
Console.WriteLine(o.GetType()); // System.Object
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the .NET collection class that allows an element to be accessed using a unique key?

`Dictionary<TKey, TValue>` is the primary class for key-based element access. It provides O(1) average-time lookups.

```cs
var capitals = new Dictionary<string, string>
{
    ["India"]   = "New Delhi",
    ["France"]  = "Paris",
    ["Japan"]   = "Tokyo",
};

// Access by key
Console.WriteLine(capitals["India"]); // Output: New Delhi

// Safe access
if (capitals.TryGetValue("France", out string? capital))
    Console.WriteLine(capital); // Output: Paris

// Iterate
foreach (var (country, city) in capitals)
    Console.WriteLine($"{country}: {city}");
```

**Other key-based collections in .NET:**

| Type                         | Use case                                   |
|------------------------------|--------------------------------------------|
| `Dictionary<K,V>`            | General mutable key-value store            |
| `ConcurrentDictionary<K,V>`  | Thread-safe key-value store                |
| `FrozenDictionary<K,V>` (.NET 8+) | Immutable, read-optimised dictionary  |
| `SortedDictionary<K,V>`      | Keys kept in sorted order                  |
| `Hashtable`                  | Non-generic legacy (avoid in new code)     |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain the three services model commonly known as a three-tier application?

A **three-tier architecture** separates an application into three logical layers, each with a distinct responsibility:

| Tier                  | Also called       | Responsibility                              |
|-----------------------|-------------------|---------------------------------------------|
| **Presentation Layer**| UI / Front-end    | User interaction, display, input validation |
| **Business Logic Layer** | Application / BLL | Business rules, workflows, calculations  |
| **Data Access Layer** | DAL / Persistence | Database or external service communication  |

**Example — ASP.NET Core Web API (.NET 10):**

```cs
// 1. Data Access Layer (DAL)
public interface IProductRepository
{
    Task<Product?> GetByIdAsync(int id);
}

public class ProductRepository(AppDbContext db) : IProductRepository
{
    public async Task<Product?> GetByIdAsync(int id)
        => await db.Products.FindAsync(id);
}

// 2. Business Logic Layer (BLL)
public class ProductService(IProductRepository repo)
{
    public async Task<string> GetProductNameAsync(int id)
    {
        var product = await repo.GetByIdAsync(id)
            ?? throw new KeyNotFoundException($"Product {id} not found");
        return product.Name.ToUpperInvariant(); // business rule
    }
}

// 3. Presentation Layer (Controller / API endpoint)
[ApiController, Route("api/products")]
public class ProductsController(ProductService service) : ControllerBase
{
    [HttpGet("{id}")]
    public async Task<IActionResult> Get(int id)
    {
        var name = await service.GetProductNameAsync(id);
        return Ok(new { Name = name });
    }
}
```

**Benefits:** Independent scalability, testability, and maintainability of each tier.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you prevent your class from being inherited by another class?

**Yes** — apply the `sealed` modifier to the class:

```cs
public sealed class Singleton
{
    private static readonly Singleton _instance = new();
    private Singleton() { }
    public static Singleton Instance => _instance;

    public void DoWork() => Console.WriteLine("Working...");
}

// Compile error: cannot derive from sealed type 'singleton'
// public class MySingleton : Singleton { }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you allow a class to be inherited, but prevent the method from being over-ridden?

**Yes** — apply `sealed` on a specific `override` method, while keeping the class itself unsealed:

```cs
public class BaseLogger
{
    public virtual void Log(string msg) => Console.WriteLine($"Base: {msg}");
    public virtual void Error(string msg) => Console.WriteLine($"Error: {msg}");
}

public class FileLogger : BaseLogger
{
    // sealed override: subclasses of FileLogger cannot override Log()
    public sealed override void Log(string msg)
        => File.AppendAllText("app.log", msg + Environment.NewLine);

    // Error() is NOT sealed — can be overridden further
    public override void Error(string msg)
        => File.AppendAllText("error.log", msg + Environment.NewLine);
}

public class AdvancedFileLogger : FileLogger
{
    // public override void Log(string msg) { } // ❌ compile error — sealed
    public override void Error(string msg) => Console.WriteLine($"ALERT: {msg}"); // ✅
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. When do you absolutely have to declare a class as abstract?

You **must** declare a class `abstract` when it contains one or more `abstract` members (methods, properties, indexers, or events with no implementation). The compiler requires this because an incomplete class cannot be instantiated.

```cs
// Must be abstract — it has an abstract member
public abstract class Shape
{
    public abstract double Area(); // no body — derived class MUST implement
    public void Describe() => Console.WriteLine($"Area = {Area():F2}");
}

// Compile error if not abstract but contains abstract member:
// public class BadShape { public abstract double Area(); } // ❌
```

**Other scenarios where abstract is the right choice (design decision, not enforced):**
- The class represents a concept that has no meaningful standalone instance (e.g., `Animal`, `Vehicle`).
- You want to enforce a contract while providing shared implementation.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the implicit name of the parameter that gets passed into the set method/property of a class?

The implicit parameter name is **`value`**. It holds the value being assigned to the property.

```cs
public class Person
{
    private string _name = string.Empty;

    public string Name
    {
        get => _name;
        set
        {
            // 'value' is the implicit parameter — holds what the caller assigns
            if (string.IsNullOrWhiteSpace(value))
                throw new ArgumentException("Name cannot be empty.");
            _name = value.Trim();
        }
    }
}

var p = new Person();
p.Name = "  Pradeep  "; // 'value' = "  Pradeep  "
Console.WriteLine(p.Name); // Output: Pradeep
```

In C# 13+, the `field` keyword is also available inside property accessors to reference the auto-generated backing field directly, but `value` remains the parameter name for the setter.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. When you inherit a protected class-level variable, who is it available to?

A `protected` variable is available to:
- The **class that declares it**.
- Any **derived class** in any assembly (unless `private protected`, which restricts to the same assembly).

It is **not** available to unrelated classes, even within the same assembly (use `internal` for that).

```cs
public class Base { protected int Value = 10; }

public class Child : Base
{
    public void Show() => Console.WriteLine(Value); // ✅ accessible
}

public class Unrelated
{
    public void Test(Base b)
    {
        // Console.WriteLine(b.Value); // ❌ not accessible from unrelated class
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the top .NET class that everything is derived from?

`System.Object` (alias `object`). See [Which class is at the top of .NET class hierarchy?](#q-which-class-is-at-the-top-of-net-class-hierarchy) above.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. When do you absolutely have to declare a class as abstract (as opposed to free-willed educated choice or decision based on UML diagram)?

You are **forced** by the compiler to use `abstract` when the class contains **at least one `abstract` member** (a member declared without a body). Without `abstract` on the class, the code will not compile.

```cs
// FORCED: contains abstract member → class must be abstract
public abstract class DataExporter // ← required by compiler
{
    public abstract void Export(string data); // ← forces class to be abstract
    public void Log(string msg) => Console.WriteLine(msg);
}
```

In all other cases, `abstract` is a design choice — you opt in to signal that the type is incomplete by intent.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Is it namespace class or class namespace?

The correct syntax is **`namespace` then `class`** — namespaces contain classes, not the other way around.

```cs
// Correct: namespace wraps the class
namespace MyApp.Services
{
    public class OrderService
    {
        public void Process() => Console.WriteLine("Processing order");
    }
}

// File-scoped namespace (C# 10+) — preferred modern style
namespace MyApp.Services;

public class OrderService
{
    public void Process() => Console.WriteLine("Processing order");
}
```

A class belongs to a namespace; a namespace cannot belong to a class.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the default Access Modifier for the members of the class?

The default access modifier for **class members** (fields, methods, properties, etc.) is **`private`**.

The default for the **class itself** (when declared directly in a namespace) is **`internal`**.

```cs
namespace MyApp;

class MyClass          // default: internal (visible only within the assembly)
{
    int _field;         // default: private
    void Method() { }  // default: private
    string Prop { get; set; } // default: private

    public int PublicField = 0; // explicitly public
}
```

| Context                    | Default modifier |
|----------------------------|-----------------|
| Class in namespace         | `internal`      |
| Class member               | `private`       |
| Interface member (C# 8+)   | `public`        |
| Enum member                | `public`        |
| Struct member              | `private`       |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How to Call the Default constructor of one class with the parameterized constructor of same class?

Use **constructor chaining** with the `this()` keyword:

```cs
public class Person
{
    public string Name { get; }
    public int Age { get; }
    public string Country { get; }

    // Default constructor chains to the parameterized one with defaults
    public Person() : this("Unknown", 0, "India") { }

    // Partial parameterized chains to the full constructor
    public Person(string name) : this(name, 0, "India") { }

    // Full parameterized constructor — all others delegate here
    public Person(string name, int age, string country)
    {
        Name    = name;
        Age     = age;
        Country = country;
    }
}

var p1 = new Person();                    // Name=Unknown, Age=0, Country=India
var p2 = new Person("Pradeep");           // Name=Pradeep, Age=0, Country=India
var p3 = new Person("Pradeep", 30, "IN"); // Name=Pradeep, Age=30, Country=IN

Console.WriteLine($"{p1.Name}, {p1.Country}"); // Unknown, India
Console.WriteLine($"{p2.Name}, {p2.Country}"); // Pradeep, India
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is scope of a Protected Internal member variable of a C# class?

A `protected internal` member is accessible:
- From **anywhere within the same assembly** (like `internal`), AND
- From **derived classes in any assembly** (like `protected`).

It is the **most permissive** combined modifier.

```cs
// Assembly A
public class Base
{
    protected internal string Data = "shared";
}

// Assembly A — unrelated class (same assembly → internal part grants access)
public class Unrelated
{
    public void Test(Base b) => Console.WriteLine(b.Data); // ✅
}

// Assembly B — derived class (protected part grants access)
public class Derived : Base
{
    public void Show() => Console.WriteLine(Data); // ✅
}

// Assembly B — unrelated class
public class External
{
    // Console.WriteLine(new Base().Data); // ❌ not accessible
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between Virtual method and Abstract method?

| Feature             | `virtual` method                       | `abstract` method                      |
|---------------------|----------------------------------------|----------------------------------------|
| Body                | Has a default implementation           | No body — derived class must implement |
| Class requirement   | Class can be concrete or abstract      | Class must be `abstract`               |
| Override required   | Optional — derived class may override  | Mandatory — derived class must override|
| Instantiation       | Containing class can be instantiated   | Containing class cannot be instantiated|

```cs
public abstract class Animal
{
    // abstract — NO body, MUST be overridden
    public abstract string MakeSound();

    // virtual — HAS a body, CAN be overridden
    public virtual string Describe()
        => $"I am a {GetType().Name} and I say {MakeSound()}";
}

public class Dog : Animal
{
    public override string MakeSound() => "Woof"; // required
    // Describe() not overridden — uses base implementation
}

public class Cat : Animal
{
    public override string MakeSound() => "Meow";   // required
    public override string Describe() => "I'm a cat. Meow!"; // optional
}

Animal[] animals = [new Dog(), new Cat()];
foreach (var a in animals)
    Console.WriteLine(a.Describe());
// Output:
// I am a Dog and I say Woof
// I'm a cat. Meow!
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is scope of a Internal member variable of a C# class?

An `internal` member is accessible from **anywhere within the same assembly** (project/DLL), but **not** from external assemblies.

```cs
// Assembly A (MyApp.dll)
public class Configuration
{
    internal string ConnectionString = "Server=localhost;"; // same assembly only
    public string AppName = "MyApp"; // accessible everywhere
}

// Same assembly — OK
public class DatabaseService
{
    public void Connect()
    {
        var config = new Configuration();
        Console.WriteLine(config.ConnectionString); // ✅ same assembly
    }
}

// Assembly B — external project referencing MyApp.dll
// var cfg = new Configuration();
// Console.WriteLine(cfg.ConnectionString); // ❌ not accessible externally
```

**Tip:** Use `[assembly: InternalsVisibleTo("TestProject")]` to expose `internal` members to a test assembly without making them `public`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How would you implement multiple interfaces with the same method name in the same class?

Use **explicit interface implementation** to resolve the naming conflict:

```cs
public interface IDrawable { void Draw(); }
public interface IPrintable { void Draw(); } // same method name

public class Document : IDrawable, IPrintable
{
    // Explicit implementation — called only via the interface reference
    void IDrawable.Draw()  => Console.WriteLine("Drawing to screen");
    void IPrintable.Draw() => Console.WriteLine("Printing to paper");

    // Optional: a public method calling one of them
    public void Render() => ((IDrawable)this).Draw();
}

var doc = new Document();
doc.Render(); // Output: Drawing to screen

// Call via interface reference
IDrawable  drawable  = doc;
IPrintable printable = doc;

drawable.Draw();  // Output: Drawing to screen
printable.Draw(); // Output: Printing to paper
```

**Note:** Explicitly implemented members are not accessible directly on the class instance — only via a cast to the interface type.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can you create a derived class object from a base class?

You cannot directly instantiate an abstract base class, but you can hold a derived class object in a base class reference (polymorphism):

```cs
public abstract class Animal
{
    public abstract string Sound();
}

public class Dog : Animal
{
    public override string Sound() => "Woof";
}

// Base class reference pointing to derived class object
Animal animal = new Dog(); // ✅ upcasting (implicit)
Console.WriteLine(animal.Sound()); // Output: Woof

// Downcast when you need derived-specific members
if (animal is Dog dog)
    Console.WriteLine($"Dog instance: {dog.Sound()}");
```

**With factory / virtual constructor pattern (.NET 10):**

```cs
public abstract class Shape
{
    public abstract double Area();
    public static Shape CreateCircle(double r) => new Circle(r);
}

public sealed class Circle(double r) : Shape
{
    public override double Area() => Math.PI * r * r;
}

Shape s = Shape.CreateCircle(5);
Console.WriteLine(s.Area()); // Output: 78.54...
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Which is the parent class of all classes which we create in C#?

**`System.Object`** (`object`) is the implicit parent of every class in C#. When you define a class without an explicit base, it implicitly inherits from `object`.

```cs
public class MyClass { } // implicitly: public class MyClass : object { }

var obj = new MyClass();
Console.WriteLine(obj.GetType());        // MyClass
Console.WriteLine(obj.ToString());       // MyClass
Console.WriteLine(obj.GetHashCode());    // some hash
Console.WriteLine(obj.Equals(new MyClass())); // False (reference equality by default)
```

Methods inherited from `object`: `Equals()`, `GetHashCode()`, `ToString()`, `GetType()`, `MemberwiseClone()`, `Finalize()`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the base class in .NET framework from which all the classes have been developed?

`System.Object` is the ultimate base class for all types in .NET. In .NET 10 (CoreCLR), this is the same — `System.Object` sits at the root of the entire type hierarchy.

```cs
// Verify any type\'s chain back to System.Object
Type t = typeof(HttpClient);
while (t != null)
{
    Console.WriteLine(t.FullName);
    t = t.BaseType!;
}
// Output (partial):
// System.Net.Http.HttpClient
// System.Object
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement a custom attribute in C#?

Derive a class from `System.Attribute`, mark it with `[AttributeUsage]`, and apply it with `[...]` syntax.

```cs
// 1. Define the attribute
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, AllowMultiple = false)]
public class AuditAttribute : Attribute
{
    public string Author { get; }
    public string Version { get; }

    public AuditAttribute(string author, string version = "1.0")
    {
        Author  = author;
        Version = version;
    }
}

// 2. Apply the attribute
[Audit("Pradeep", "2.0")]
public class OrderService
{
    [Audit("Pradeep")]
    public void PlaceOrder(string item) => Console.WriteLine($"Order: {item}");
}

// 3. Read the attribute via reflection
var attr = typeof(OrderService).GetCustomAttribute<AuditAttribute>();
Console.WriteLine($"Author: {attr?.Author}, Version: {attr?.Version}");
// Output: Author: Pradeep, Version: 2.0
```

**⚠️ Note for .NET 10 / Native AOT:** Reflection-based attribute reading works but is trimmed by the AOT compiler. Prefer source generators or `[DynamicallyAccessedMembers]` annotations when targeting Native AOT.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the `System.String` and `System.Text.StringBuilder` classes?

Both represent text, but with different performance characteristics:

| Feature            | `System.String`                 | `System.Text.StringBuilder`          |
|--------------------|---------------------------------|--------------------------------------|
| Mutability         | **Immutable** — every change creates a new object | **Mutable** — modifies in place |
| Thread safety      | Inherently safe (immutable)     | Not thread-safe                      |
| Performance        | Fine for few concatenations     | Efficient for many concatenations    |
| Memory             | New allocation per modification | Single buffer, grows as needed       |
| Namespace          | `System`                        | `System.Text`                        |

**String (immutable):**

```cs
string s = "Hello";
s += " World"; // creates a new string object
Console.WriteLine(s); // Hello World
```

**StringBuilder (mutable):**

```cs
using System.Text;

var sb = new StringBuilder();
for (int i = 0; i < 5; i++)
    sb.Append($"Line {i}\n");

string result = sb.ToString();
Console.WriteLine(result);
```

**Rule:** Use `string` for a small number of operations. Use `StringBuilder` for loops or building large strings dynamically.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are I/O classes in C#?

The `System.IO` namespace provides classes for reading from and writing to files, directories, and streams.

**Key I/O classes:**

| Class             | Purpose                                      |
|-------------------|----------------------------------------------|
| `File`            | Static methods for quick file operations     |
| `FileInfo`        | Instance-based file metadata and operations  |
| `Directory`       | Static methods for directory operations      |
| `StreamReader`    | Read text from a stream                      |
| `StreamWriter`    | Write text to a stream                       |
| `FileStream`      | Low-level binary file stream                 |
| `MemoryStream`    | In-memory stream (no file system)            |
| `BinaryReader/Writer` | Read/write primitive types as binary   |
| `Path`            | Platform-safe path manipulation              |

**Example — modern async file I/O (.NET 10):**

```cs
using System.IO;

// Write
await File.WriteAllTextAsync("output.txt", "Hello, .NET 10!");

// Read all lines
string[] lines = await File.ReadAllLinesAsync("output.txt");
foreach (var line in lines)
    Console.WriteLine(line);

// Stream-based read (large files)
await using var reader = new StreamReader("output.txt");
while (await reader.ReadLineAsync() is { } line)
    Console.WriteLine(line);

// Directory operations
string dir = Path.Combine("MyApp", "Logs");
Directory.CreateDirectory(dir);
Console.WriteLine(Directory.Exists(dir)); // True
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you add extension methods to an existing static class?

**No** — you cannot add extension methods *to* a static class (i.e., you cannot extend a static class\'s API with new instance-style methods, because static classes cannot be used as a type that has instances or be referenced via `this`).

However, you can **create extension methods in a static class** to extend other types (including non-static classes):

```cs
// Extension methods LIVE IN a static class, but they extend OTHER types
public static class StringExtensions
{
    // Extends 'string' — not the static class itself
    public static bool IsPalindrome(this string s)
    {
        var reversed = new string(s.Reverse().ToArray());
        return s.Equals(reversed, StringComparison.OrdinalIgnoreCase);
    }
}

Console.WriteLine("racecar".IsPalindrome()); // True
Console.WriteLine("hello".IsPalindrome());   // False
```

**Attempting to extend a static class itself:**

```cs
public static class MathHelper { }

// ❌ Cannot write: public static void NewMethod(this MathHelper m) { }
// MathHelper has no instances — 'this MathHelper' is meaningless
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you create sealed abstract class in C#?

**No.** `sealed` and `abstract` are contradictory modifiers:
- `abstract` means the class *must* be inherited (it cannot be instantiated directly).
- `sealed` means the class *cannot* be inherited.

Combining them produces a compile error:

```cs
// Compile error: abstract and sealed cannot be combined
// public sealed abstract class MyClass { }
```

The closest valid pattern is a `static` class — it cannot be instantiated or inherited and can contain only static members:

```cs
public static class Utilities
{
    public static string Reverse(string s) => new string(s.Reverse().ToArray());
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you inherit multiple interfaces?

**Yes.** A class (or struct) can implement any number of interfaces:

```cs
public interface IReadable  { string Read(); }
public interface IWritable  { void Write(string data); }
public interface ICloseable { void Close(); }

public class FileHandler : IReadable, IWritable, ICloseable
{
    private readonly List<string> _buffer = [];

    public string Read()           => string.Join("\n", _buffer);
    public void Write(string data) => _buffer.Add(data);
    public void Close()            => _buffer.Clear();
}

var fh = new FileHandler();
fh.Write("Hello");
fh.Write("World");
Console.WriteLine(fh.Read()); // Hello\nWorld
fh.Close();
```

An interface can also inherit from multiple interfaces:

```cs
public interface IReadWritable : IReadable, IWritable { }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What interface should your data structure implement to make the "Where" method work?

Your type must implement **`IEnumerable<T>`** (from `System.Collections.Generic`). LINQ extension methods including `Where`, `Select`, `OrderBy` etc. operate on any `IEnumerable<T>`.

```cs
using System.Collections;
using System.Collections.Generic;

public class NumberBag : IEnumerable<int>
{
    private readonly List<int> _numbers = [];

    public void Add(int n) => _numbers.Add(n);

    // Required by IEnumerable<int>
    public IEnumerator<int> GetEnumerator() => _numbers.GetEnumerator();
    IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
}

var bag = new NumberBag();
foreach (var n in new[] { 1, 2, 3, 4, 5, 6 }) bag.Add(n);

// Where works because NumberBag implements IEnumerable<int>
var evens = bag.Where(n => n % 2 == 0).ToList();
Console.WriteLine(string.Join(", ", evens)); // Output: 2, 4, 6
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can we define methods as private in interface?

**Yes** — since **C# 8 / .NET Core 3**, interfaces can define `private` methods. Private interface methods can only be called from within the interface itself (typically from default implementations) and cannot be overridden by implementing classes.

```cs
public interface IGreeter
{
    void Greet(string name);

    // Private helper — only callable within this interface
    private string FormatName(string name)
        => name.Trim().ToUpperInvariant();

    // Default implementation calls the private helper
    void GreetFormal(string name)
        => Console.WriteLine($"Good day, {FormatName(name)}.");
}

public class EnglishGreeter : IGreeter
{
    public void Greet(string name) => Console.WriteLine($"Hello, {name}!");
    // GreetFormal comes from the default implementation
}

IGreeter g = new EnglishGreeter();
g.Greet("pradeep");        // Output: Hello, pradeep!
g.GreetFormal("pradeep");  // Output: Good day, PRADEEP.
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. If I want to change an interface, what\'s the best practice?

Changing a published interface is a **breaking change** — all implementing classes must be updated. The safest strategies:

**1. Default interface methods (C# 8+) — non-breaking addition:**

Add new methods with a default implementation. Existing implementors don\'t need to change.

```cs
public interface ILogger
{
    void Log(string message);

    // New method added without breaking existing implementations
    void LogWarning(string message) => Log($"[WARNING] {message}");
}
```

**2. Interface versioning — create a new interface:**

```cs
public interface ILogger    { void Log(string message); }
public interface ILogger2 : ILogger { void LogWarning(string message); }

// New implementations use ILogger2; old ones still work with ILogger
```

**3. Use an abstract base class for extensible contracts:**

If you anticipate change, an abstract class with virtual methods is easier to evolve without breaking consumers.

**4. Avoid changing interface signatures** if the interface is in a public NuGet package or shared library — use extension methods or wrapper interfaces instead.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can we create instance of interface?

**No.** You cannot instantiate an interface directly with `new`. An interface is a contract — it has no concrete implementation or constructor.

```cs
// Compile error: cannot create an instance of an abstract type/interface
// ILogger logger = new ILogger();
```

**What you can do:** Create an instance of a class that implements the interface, and hold it in an interface-typed variable:

```cs
public interface ILogger { void Log(string msg); }
public class ConsoleLogger : ILogger
{
    public void Log(string msg) => Console.WriteLine(msg);
}

ILogger logger = new ConsoleLogger(); // ✅ interface reference to concrete instance
logger.Log("Hello!"); // Output: Hello!
```

**Exception:** Anonymous types implementing interfaces via `static` methods (not common), or using mock frameworks in tests that generate proxy implementations at runtime.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain the differences between covariance and contravariance in C# for delegates and interfaces.

**Covariance** allows a more derived type to be used where a less derived type is expected (output positions — return types).  
**Contravariance** allows a less derived type to be used where a more derived type is expected (input positions — parameter types).

Enabled with `out` (covariance) and `in` (contravariance) on generic type parameters.

**Covariance (`out`) — return type can be more derived:**

```cs
public class Animal { }
public class Dog : Animal { }

// IEnumerable<T> is covariant (out T)
IEnumerable<Dog> dogs = new List<Dog> { new Dog() };
IEnumerable<Animal> animals = dogs; // ✅ Dog is more derived than Animal

// Func<T> is covariant in TResult
Func<Dog> getDog = () => new Dog();
Func<Animal> getAnimal = getDog; // ✅ covariant
```

**Contravariance (`in`) — parameter type can be less derived:**

```cs
// Action<T> is contravariant (in T)
Action<Animal> processAnimal = a => Console.WriteLine("Processing animal");
Action<Dog> processDog = processAnimal; // ✅ contravariant — can handle Dog via Animal handler

processDog(new Dog()); // Output: Processing animal
```

**Custom covariant interface:**

```cs
public interface IProducer<out T> { T Produce(); }

public class DogProducer : IProducer<Dog>
{
    public Dog Produce() => new Dog();
}

IProducer<Animal> producer = new DogProducer(); // ✅ covariant
```

**Summary:**

| Keyword | Type parameter | Allowed direction | Example                       |
|---------|---------------|-------------------|-------------------------------|
| `out`   | Return type    | More derived → base | `IEnumerable<Dog>` → `IEnumerable<Animal>` |
| `in`    | Parameter type | Base → more derived | `Action<Animal>` → `Action<Dog>` |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is an abstraction?

**Abstraction** is the OOP principle of exposing only the **relevant details** of an object and hiding the internal complexity. It allows you to work with high-level concepts without knowing the underlying implementation.

In C#, abstraction is achieved through:
- **Abstract classes** (partial implementation, enforced contract)
- **Interfaces** (pure contract, no implementation)
- **Access modifiers** (hide internal details)

```cs
// Abstraction: caller uses IPayment without knowing Stripe or PayPal internals
public interface IPayment
{
    Task<bool> ChargeAsync(decimal amount, string token);
}

public class StripePayment : IPayment
{
    public async Task<bool> ChargeAsync(decimal amount, string token)
    {
        // Complex Stripe API logic hidden here
        Console.WriteLine($"Stripe: charging {amount:C} with token {token}");
        await Task.Delay(10); // simulate async call
        return true;
    }
}

// High-level code only knows IPayment — abstracted from Stripe details
public class CheckoutService(IPayment payment)
{
    public async Task Checkout(decimal total)
    {
        bool success = await payment.ChargeAsync(total, "tok_123");
        Console.WriteLine(success ? "Payment succeeded" : "Payment failed");
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Is it compulsory to implement Abstract methods?

**Yes** — any non-abstract class that inherits from an abstract class **must** implement all abstract methods. Failure to do so is a compile error.

```cs
public abstract class Shape
{
    public abstract double Area();    // must be implemented
    public abstract double Perimeter(); // must be implemented
    public virtual void Print() => Console.WriteLine($"Area={Area():F2}"); // optional
}

// Compile error — Area() and Perimeter() are not implemented:
// public class BadShape : Shape { }

// OK — all abstract members implemented:
public class Square(double side) : Shape
{
    public override double Area()      => side * side;
    public override double Perimeter() => 4 * side;
}

var sq = new Square(5);
sq.Print(); // Output: Area=25.00
```

**Exception:** If a derived class is itself `abstract`, it does not need to implement the abstract methods — the responsibility is passed to the next concrete class in the chain.

```cs
public abstract class AbstractDerived : Shape
{
    // Still abstract — no need to implement Area() or Perimeter() yet
    public abstract string Name();
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the method MemberwiseClone() doing?

`MemberwiseClone()` is a `protected` method inherited from `System.Object`. It creates a **shallow copy** of the current object — all value-type fields are copied by value, but reference-type fields are copied by reference (both objects share the same referenced object).

```cs
public class Address
{
    public string City = "Mumbai";
}

public class Person
{
    public string Name = "Pradeep";
    public int Age = 30;
    public Address Location = new Address();

    public Person ShallowCopy() => (Person)MemberwiseClone();
}

var original = new Person();
var copy     = original.ShallowCopy();

// Value types are independent copies
copy.Name = "Ravi";
copy.Age  = 25;

// Reference types point to the SAME object (shallow copy!)
copy.Location.City = "Delhi";

Console.WriteLine(original.Name);          // Pradeep (independent copy)
Console.WriteLine(original.Location.City); // Delhi (shared reference!)
```

**For a deep copy**, implement `ICloneable` or manually copy each reference-type field.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain the difference between destructor, dispose and finalize method?

| Concept        | Declaration              | Called by          | Use case                            |
|----------------|--------------------------|--------------------|-------------------------------------|
| `Destructor`   | `~ClassName() { }`       | GC (non-deterministic) | Alias for `Finalize()` in C#     |
| `Finalize()`   | `protected override void Finalize()` | GC | Last resort cleanup of unmanaged resources |
| `Dispose()`    | `IDisposable.Dispose()`  | Developer / `using` | Deterministic cleanup of managed + unmanaged resources |

**Destructor / Finalize** (non-deterministic — GC decides when):

```cs
public class ResourceHolder
{
    ~ResourceHolder() // Destructor — compiles to override Finalize()
    {
        Console.WriteLine("Finalizer called by GC");
        // Release unmanaged resources
    }
}
```

**IDisposable / Dispose** (deterministic — called explicitly or via `using`):

```cs
public class FileHandler : IDisposable
{
    private FileStream? _stream;
    private bool _disposed;

    public FileHandler(string path)
        => _stream = new FileStream(path, FileMode.OpenOrCreate);

    public void Dispose()
    {
        if (!_disposed)
        {
            _stream?.Dispose();
            _stream = null;
            _disposed = true;
            GC.SuppressFinalize(this); // prevent double-cleanup
        }
    }

    ~FileHandler() => Dispose(); // fallback if Dispose() not called
}

// Deterministic cleanup with using:
using var fh = new FileHandler("data.txt");
// Dispose() called automatically at end of scope
```

**Best practice (.NET 10):** Implement `IDisposable` for deterministic cleanup. Use `IAsyncDisposable` + `await using` for async resources. Always call `GC.SuppressFinalize(this)` in `Dispose()` if you have a finalizer.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Could you explain the difference between Func vs. Action vs. Predicate?

All three are built-in generic delegate types in `System`:

| Type          | Signature                          | Returns    | Use case                            |
|---------------|------------------------------------|------------|-------------------------------------|
| `Action<T>`   | `void (T arg)`                     | `void`     | Perform an action, no return value  |
| `Func<T, R>`  | `R (T arg)`                        | `R`        | Transform/compute, returns a value  |
| `Predicate<T>`| `bool (T arg)`                     | `bool`     | Test a condition (subset of Func)   |

```cs
// Action<T> — does something, returns nothing
Action<string> print = msg => Console.WriteLine(msg);
print("Hello Action"); // Output: Hello Action

// Func<T, TResult> — transforms and returns
Func<int, int, int> add = (a, b) => a + b;
Console.WriteLine(add(3, 4)); // Output: 7

Func<string, string> toUpper = s => s.ToUpperInvariant();
Console.WriteLine(toUpper("hello")); // Output: HELLO

// Predicate<T> — returns bool (used in List<T>.FindAll, RemoveAll, etc.)
Predicate<int> isEven = n => n % 2 == 0;
var numbers = new List<int> { 1, 2, 3, 4, 5, 6 };
var evens = numbers.FindAll(isEven);
Console.WriteLine(string.Join(", ", evens)); // Output: 2, 4, 6

// Predicate<T> is equivalent to Func<T, bool>
Func<int, bool> isEvenFunc = n => n % 2 == 0;
```

**Key takeaway:** `Predicate<T>` is essentially `Func<T, bool>` — use `Func` in LINQ, and `Predicate` with older `List<T>` APIs.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a namespace and is it compulsory?

A **namespace** is a logical container that organises types (classes, interfaces, enums, etc.) and prevents naming conflicts between different libraries or code areas.

```cs
namespace MyApp.Services;  // file-scoped namespace (C# 10+)

public class OrderService { }
public class ProductService { }
```

**Is it compulsory?** No. Types declared without a namespace go into the **global namespace** and can be accessed without qualification. However, omitting namespaces is strongly discouraged in production code because:
- Name collisions become likely as a project grows.
- IntelliSense, tooling, and `using` directives rely on namespaces.

**Usage:**

```cs
using MyApp.Services;

var svc = new OrderService(); // resolved via namespace
```

**Global namespace access** (when needed):

```cs
global::System.Console.WriteLine("Using global namespace");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What do you think about empty destructor?

An **empty destructor** is harmful and should be avoided. Even with no code, it has a negative performance impact:

**Why it\'s harmful:**
- The GC moves objects with finalizers to the **finalization queue**, requiring an extra GC cycle to collect them.
- An empty destructor causes this overhead without any benefit.
- Objects survive an extra GC generation, increasing memory pressure.

```cs
// BAD — empty destructor adds GC overhead with zero benefit
public class MyClass
{
    ~MyClass() { } // ❌ Remove this
}

// GOOD — no destructor needed if there are no unmanaged resources
public class MyClass
{
    // No destructor — GC collects it efficiently in one cycle
}
```

**Rule:** Only add a destructor/finalizer if the class **directly owns unmanaged resources** (e.g., native handles, COM objects). And if you have a finalizer, always also implement `IDisposable` and call `GC.SuppressFinalize(this)` in `Dispose()`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the different types of "USING/HAS A" relationship?

"HAS-A" is a **composition** relationship — one class contains or uses an instance of another class. C# supports three levels of HAS-A:

**1. Association** — objects are related but have independent lifecycles.

```cs
// Teacher knows about a Student but doesn\'t own it
public class Teacher { public void Teach(Student s) => Console.WriteLine($"Teaching {s.Name}"); }
public class Student { public string Name { get; set; } = ""; }
```

**2. Aggregation** — a "whole-part" relationship where the part can exist independently.

```cs
// Department contains Employees, but Employees can exist without a Department
public class Employee { public string Name { get; set; } = ""; }
public class Department
{
    private readonly List<Employee> _employees = [];
    public void Add(Employee e) => _employees.Add(e);
}
```

**3. Composition** — a strong "whole-part" relationship; the part cannot exist without the whole.

```cs
// Engine only exists as part of a Car — created and destroyed with it
public class Car
{
    private readonly Engine _engine = new Engine(); // Car owns Engine\'s lifecycle

    public void Start() => _engine.Start();
}

public class Engine { public void Start() => Console.WriteLine("Engine started"); }
```

**Summary:**

| Relationship  | Lifecycle dependency     | Example              |
|---------------|--------------------------|----------------------|
| Association   | Independent              | Teacher ↔ Student    |
| Aggregation   | Part survives whole      | Department → Employee|
| Composition   | Part dies with whole     | Car → Engine         |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Differentiate between Composition vs Aggregation vs Association?

See the [USING/HAS A relationship](#q-what-are-the-different-types-of-usinghas-a-relationship) question above for the full breakdown.

**Quick differentiation:**

- **Association**: Knows-about. Objects interact but neither owns the other.
- **Aggregation**: Has-a. Whole holds parts, but parts can exist independently (weak ownership).
- **Composition**: Contains-a. Whole creates and owns parts; parts cannot exist alone (strong ownership).

```cs
// Association
public class Order { public void Process(Customer c) { } }

// Aggregation — customer exists outside the order
public class ShoppingCart
{
    public List<Customer> SharedCustomers { get; } = [];
}

// Composition — address is created and owned by customer
public class Customer
{
    private readonly Address _address; // owned, created here
    public Customer(string city) => _address = new Address(city);
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are circular references?

A **circular reference** occurs when two or more objects reference each other, forming a cycle (A → B → A, or A → B → C → A). In .NET, the GC handles circular references via tracing (mark-and-sweep), so they don\'t cause memory leaks in managed code by themselves.

However, circular references cause problems in:
- **Serialization** (JSON/XML infinite loop)
- **Dependency injection** (circular dependency between services)
- **COM / unmanaged interop** (reference counting can\'t break cycles)

```cs
public class Parent
{
    public Child? Child { get; set; }
    public string Name = "Parent";
}

public class Child
{
    public Parent? Parent { get; set; } // circular reference
    public string Name = "Child";
}

var parent = new Parent();
var child  = new Child();
parent.Child  = child;
child.Parent  = parent; // circle: parent → child → parent

// Serialization issue:
// JsonSerializer.Serialize(parent); // throws JsonException: cycle detected

// Fix with ReferenceHandler.Preserve or break the cycle:
var options = new System.Text.Json.JsonSerializerOptions
{
    ReferenceHandler = System.Text.Json.Serialization.ReferenceHandler.Preserve
};
string json = System.Text.Json.JsonSerializer.Serialize(parent, options);
Console.WriteLine(json);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is weak reference in C#?

A **weak reference** (`WeakReference<T>`) allows you to hold a reference to an object without preventing the garbage collector from collecting it. If there are no other (strong) references to the object, the GC can reclaim its memory even though your weak reference still exists.

```cs
var data = new byte[1024 * 1024]; // 1 MB object
var weakRef = new WeakReference<byte[]>(data);

// data still has a strong reference, so it's alive
Console.WriteLine(weakRef.TryGetTarget(out _)); // True

// Remove the strong reference
data = null!;
GC.Collect(); // force GC for demo purposes

if (weakRef.TryGetTarget(out byte[]? recovered))
    Console.WriteLine("Still alive");
else
    Console.WriteLine("Collected by GC"); // likely after GC
```

**Use cases:**
- **Caches** — hold entries without preventing GC from reclaiming them under memory pressure.
- **Event handlers** — avoid memory leaks when the publisher outlives the subscriber.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain weak and strong references?

- **Strong reference**: Any normal variable holding an object. Keeps the object alive — the GC will not collect it.
- **Weak reference**: A `WeakReference<T>` that does not prevent GC from collecting the object.

```cs
// Strong reference — object stays alive
var obj = new List<int> { 1, 2, 3 }; // strong

// Weak reference — object can be collected if no strong ref exists
var weak = new WeakReference<List<int>>(obj);

Console.WriteLine(weak.TryGetTarget(out _)); // True — obj is alive (strong ref exists)

obj = null!; // remove strong reference
GC.Collect();

Console.WriteLine(weak.TryGetTarget(out _)); // False — likely collected
```

**Key point:** As long as at least one **strong reference** exists, the GC will not collect the object. When all strong references are removed, the GC may collect it — and any `WeakReference<T>` pointing to it will return `false` from `TryGetTarget`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can you use Interfaces in C# to reduce coupling and improve maintainability?

Interfaces define contracts that decouple **what** a component does from **how** it does it. By depending on interfaces instead of concrete classes, your code becomes easier to test, extend, and maintain.

```cs
// Interface (contract)
public interface IEmailService
{
    Task SendAsync(string to, string subject, string body);
}

// Two concrete implementations — easily swappable
public class SmtpEmailService : IEmailService
{
    public async Task SendAsync(string to, string subject, string body)
    {
        Console.WriteLine($"SMTP → {to}: {subject}");
        await Task.CompletedTask;
    }
}

public class SendGridEmailService : IEmailService
{
    public async Task SendAsync(string to, string subject, string body)
    {
        Console.WriteLine($"SendGrid → {to}: {subject}");
        await Task.CompletedTask;
    }
}

// Consumer depends on IEmailService, NOT on Smtp or SendGrid
public class UserRegistrationService(IEmailService emailService)
{
    public async Task RegisterAsync(string email)
    {
        Console.WriteLine($"User {email} registered.");
        await emailService.SendAsync(email, "Welcome!", "Thanks for joining.");
    }
}

// Easy to swap: change the DI registration, not the service code
var service = new UserRegistrationService(new SmtpEmailService());
await service.RegisterAsync("user@example.com");
```

**Benefits:**
- **Testability:** Inject a mock `IEmailService` in unit tests — no real email sent.
- **Extensibility:** Add `TwilioEmailService` without touching `UserRegistrationService`.
- **Reduced coupling:** `UserRegistrationService` doesn\'t know or care which email provider is used.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What happens if the inherited interfaces have conflicting method names?

When a class implements multiple interfaces with the same method signature, you can use **explicit interface implementation** to provide a separate implementation for each.

```cs
public interface ILogger  { void Log(string msg); }
public interface IAuditor { void Log(string msg); } // same method name

public class ActivityTracker : ILogger, IAuditor
{
    // Explicit: only callable via ILogger reference
    void ILogger.Log(string msg)  => Console.WriteLine($"[LOG]   {msg}");

    // Explicit: only callable via IAuditor reference
    void IAuditor.Log(string msg) => Console.WriteLine($"[AUDIT] {msg}");

    // Optional: a public method on the class itself
    public void TrackActivity(string activity)
    {
        ((ILogger)this).Log(activity);
        ((IAuditor)this).Log(activity);
    }
}

var tracker = new ActivityTracker();
tracker.TrackActivity("User login");
// Output:
// [LOG]   User login
// [AUDIT] User login

// Disambiguate via interface reference
ILogger  logger  = tracker;
IAuditor auditor = tracker;

logger.Log("Via ILogger");   // Output: [LOG]   Via ILogger
auditor.Log("Via IAuditor"); // Output: [AUDIT] Via IAuditor
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `volatile` keyword in C#?

The `volatile` keyword indicates that a field may be modified by multiple threads concurrently. It instructs the compiler and runtime to always read from and write to the field\'s actual memory location, preventing CPU caching or instruction reordering optimisations that could cause stale reads.

```cs
public class SharedState
{
    private volatile bool _running = true; // ensures all threads see the latest value

    public void Stop() => _running = false;

    public void WorkLoop()
    {
        while (_running) // each iteration re-reads from memory
        {
            // do work
        }
        Console.WriteLine("Loop stopped.");
    }
}

var state = new SharedState();
var worker = Task.Run(state.WorkLoop);

await Task.Delay(100);
state.Stop(); // another thread sets _running = false
await worker;
```

**When to use:** Simple flag variables read/written by multiple threads where full `lock` or `Interlocked` is overkill. For compound operations or incrementing, use `Interlocked` instead.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `checked` keyword in C#?

The `checked` keyword enables **overflow checking** for arithmetic operations on integer types. Without `checked`, integer overflow wraps around silently; with `checked`, it throws an `OverflowException`.

```cs
int max = int.MaxValue; // 2,147,483,647

// Unchecked (default) — wraps around silently
int overflowed = max + 1;
Console.WriteLine(overflowed); // Output: -2147483648 (wrong!)

// Checked — throws OverflowException
try
{
    int result = checked(max + 1); // ❌ throws OverflowException
}
catch (OverflowException ex)
{
    Console.WriteLine($"Overflow caught: {ex.Message}");
}

// Checked block — applies to all arithmetic in the block
checked
{
    int a = int.MaxValue;
    int b = a + 1; // ❌ throws OverflowException
}
```

**`unchecked` keyword** explicitly suppresses overflow checking (useful when you want wraparound by design, e.g., hash code calculations):

```cs
int hash = unchecked(int.MaxValue + 1); // -2147483648 — intentional wrap
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `this` keyword in C#?

The `this` keyword refers to the **current instance** of a class. It is used to:

1. **Disambiguate** between fields and parameters with the same name.
2. **Chain constructors** within the same class.
3. **Pass the current instance** as an argument to a method.
4. **Invoke extension methods** explicitly.
5. **Return the current instance** (fluent builder pattern).

```cs
public class Builder
{
    private string _name = "";
    private int _age;

    // 1. Disambiguate field vs. parameter
    public Builder SetName(string name) { this._name = name; return this; }

    // 5. Return current instance (fluent API)
    public Builder SetAge(int age)  { _age = age; return this; }

    public override string ToString() => $"{_name}, {_age}";

    // 2. Constructor chaining
    public Builder() : this("Unknown", 0) { }
    public Builder(string name, int age) { _name = name; _age = age; }
}

var result = new Builder()
    .SetName("Pradeep")
    .SetAge(30)
    .ToString();

Console.WriteLine(result); // Output: Pradeep, 30
```

**In primary constructors (C# 12+):** `this` still refers to the current instance and can be used in method bodies.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `base` keyword in C#?

The `base` keyword refers to the **base class** of the current class. It is used to:

1. **Call a base class constructor** from a derived class constructor.
2. **Call a base class method** that has been overridden.

```cs
public class Vehicle
{
    protected string Model { get; }

    public Vehicle(string model)
    {
        Model = model;
        Console.WriteLine($"Vehicle created: {model}");
    }

    public virtual void Describe()
        => Console.WriteLine($"Vehicle: {Model}");
}

public class Car : Vehicle
{
    public int Doors { get; }

    // 1. Call base constructor
    public Car(string model, int doors) : base(model)
    {
        Doors = doors;
        Console.WriteLine($"Car created: {model}, {doors} doors");
    }

    // 2. Call overridden base method
    public override void Describe()
    {
        base.Describe(); // calls Vehicle.Describe()
        Console.WriteLine($"Doors: {Doors}");
    }
}

var car = new Car("Tesla Model 3", 4);
car.Describe();
// Output:
// Vehicle created: Tesla Model 3
// Car created: Tesla Model 3, 4 doors
// Vehicle: Tesla Model 3
// Doors: 4
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `params` keyword in C#?

The `params` keyword allows a method to accept a **variable number of arguments** of the same type. The caller can pass a comma-separated list, an array, or nothing at all.

**C# 13 enhancement:** `params` now works with any collection type (`IEnumerable<T>`, `Span<T>`, `ReadOnlySpan<T>`, `List<T>`, etc.) — not just arrays.

**Traditional `params` (all versions):**

```cs
public int Sum(params int[] numbers)
    => numbers.Sum();

Console.WriteLine(Sum(1, 2, 3));           // Output: 6
Console.WriteLine(Sum(10, 20, 30, 40));    // Output: 100
Console.WriteLine(Sum());                  // Output: 0
Console.WriteLine(Sum(new[] { 5, 5, 5 })); // Output: 15
```

**`params ReadOnlySpan<T>` (C# 13 / .NET 9+) — zero allocation:**

```cs
public static double Average(params ReadOnlySpan<double> values)
{
    if (values.IsEmpty) return 0;
    double sum = 0;
    foreach (var v in values) sum += v;
    return sum / values.Length;
}

Console.WriteLine(Average(10.0, 20.0, 30.0)); // Output: 20
```

**Rules:**
- Only one `params` parameter per method.
- Must be the last parameter.
- Cannot be combined with `ref`/`out`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `yield` keyword in C#?

The `yield` keyword enables **lazy iterator methods** that return elements one at a time on demand. See the detailed answer in the [yield with iterator example](#q-what-is-the-purpose-of-the-yield-keyword-in-c-provide-an-example-of-using-it-with-an-iterator) section below.

**Quick example:**

```cs
public IEnumerable<int> Squares(int n)
{
    for (int i = 1; i <= n; i++)
        yield return i * i;
}

foreach (var s in Squares(5))
    Console.Write(s + " "); // Output: 1 4 9 16 25
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `async` and `await` keyword in C#?

`async` and `await` are keywords that enable **asynchronous programming** in C# without blocking the calling thread. They are built on top of `Task` and `Task<T>` (and `ValueTask`/`ValueTask<T>` for low-allocation scenarios).

- `async` marks a method as asynchronous; it must return `void`, `Task`, `Task<T>`, `ValueTask`, or `ValueTask<T>`.
- `await` suspends the current method until the awaited operation completes, freeing the thread for other work.

**Basic example:**

```cs
public async Task<string> FetchDataAsync(string url)
{
    using var client = new HttpClient();
    string content = await client.GetStringAsync(url);
    return content;
}

// Caller
string data = await FetchDataAsync("https://api.example.com/data");
Console.WriteLine(data);
```

**`IAsyncEnumerable<T>` — async streaming (C# 8+):**

Stream data asynchronously without loading everything into memory.

```cs
public async IAsyncEnumerable<int> GenerateAsync()
{
    for (int i = 1; i <= 5; i++)
    {
        await Task.Delay(100); // simulate async work
        yield return i;
    }
}

await foreach (var value in GenerateAsync())
    Console.Write(value + " "); // Output: 1 2 3 4 5
```

**`Task.WhenAll` and `Task.WhenAny`:**

```cs
// Run multiple async tasks in parallel
var t1 = FetchDataAsync("https://api1.example.com");
var t2 = FetchDataAsync("https://api2.example.com");

string[] results = await Task.WhenAll(t1, t2);
```

**`ValueTask<T>` for performance (.NET 5+):**

Use when a method often completes synchronously (avoids Task heap allocation).

```cs
public async ValueTask<int> GetCachedValueAsync(string key)
{
    if (_cache.TryGetValue(key, out int val))
        return val; // synchronous fast-path, no heap alloc
    return await LoadFromDbAsync(key);
}
```

**Cancellation support:**

```cs
public async Task ProcessAsync(CancellationToken ct = default)
{
    for (int i = 0; i < 100; i++)
    {
        ct.ThrowIfCancellationRequested();
        await DoWorkAsync(i, ct);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `override` and `new` keywords?

Both `override` and `new` affect how a derived class method relates to a base class method, but they behave very differently at runtime.

| Aspect              | `override`                                   | `new`                                           |
|---------------------|----------------------------------------------|-------------------------------------------------|
| Requires `virtual`  | Yes — base method must be `virtual`/`abstract`| No — works on any base method                  |
| Runtime dispatch    | Polymorphic (dynamic) — actual type decides  | Static — reference type decides                |
| Intent              | Replace the base implementation              | Hide the base method (method hiding)            |
| Best practice       | Use for intentional polymorphism             | Rarely needed; often a design smell             |

**`override` — polymorphic dispatch (runtime type wins):**

```cs
public class Animal
{
    public virtual string Speak() => "...";
}

public class Dog : Animal
{
    public override string Speak() => "Woof"; // replaces Animal.Speak
}

Animal a = new Dog();
Console.WriteLine(a.Speak()); // Output: Woof (Dog\'s version — runtime type wins)
```

**`new` — method hiding (reference type wins):**

```cs
public class Cat : Animal
{
    public new string Speak() => "Meow"; // hides Animal.Speak — NOT polymorphic
}

Animal a = new Cat();
Console.WriteLine(a.Speak()); // Output: ... (Animal\'s version — reference type wins!)

Cat c = new Cat();
Console.WriteLine(c.Speak()); // Output: Meow (Cat reference — Cat\'s version)
```

**Key takeaway:** Use `override` for true polymorphism. Use `new` only when you intentionally want to hide a base member without polymorphic dispatch — and document why.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Why do we need the `out` keyword?

The `out` keyword allows a method to **return multiple values** by passing parameters by reference. Unlike `ref`, the variable passed as `out` does **not** need to be initialised before the call — the method *must* assign a value to it before returning.

**Primary use cases:**
1. Return multiple values from a single method (before tuples were preferred).
2. The `TryXxx` pattern — return a `bool` and an output value simultaneously.

```cs
// Basic out parameter
public bool TryDivide(int a, int b, out int result)
{
    if (b == 0) { result = 0; return false; }
    result = a / b;
    return true;
}

// Caller — variable declared inline (C# 7+)
if (TryDivide(10, 2, out int quotient))
    Console.WriteLine(quotient); // Output: 5

if (!TryDivide(10, 0, out _)) // discard with _
    Console.WriteLine("Cannot divide by zero");
```

**Built-in TryParse pattern (.NET):**

```cs
string input = "42";
if (int.TryParse(input, out int value))
    Console.WriteLine($"Parsed: {value}"); // Output: Parsed: 42

// Discard when you only need the bool
bool isValid = int.TryParse("abc", out _); // false
```

**Multiple out parameters:**

```cs
public void GetMinMax(int[] arr, out int min, out int max)
{
    min = arr.Min();
    max = arr.Max();
}

GetMinMax(new[] { 3, 1, 4, 1, 5, 9 }, out int mn, out int mx);
Console.WriteLine($"Min={mn}, Max={mx}"); // Min=1, Max=9
```

**Note:** For new code, prefer **tuples** (`(int min, int max) GetMinMax(...)`) over multiple `out` parameters — they are more readable and don\'t require pre-declaration.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `params` keyword in C#?

The `params` keyword lets a method accept a **variable number of arguments** without the caller needing to create an array explicitly. It must be the last parameter in the signature.

**C# 13 / .NET 9+ enhancement:** `params` now supports any collection type — `ReadOnlySpan<T>`, `IEnumerable<T>`, `List<T>`, etc. — not just arrays.

```cs
// Traditional params array (all .NET versions)
public static int Sum(params int[] numbers) => numbers.Sum();

Console.WriteLine(Sum(1, 2, 3));        // Output: 6
Console.WriteLine(Sum(10, 20));         // Output: 30
Console.WriteLine(Sum());               // Output: 0
Console.WriteLine(Sum([5, 5, 5]));      // Output: 15 (pass array directly)

// params ReadOnlySpan<T> (C# 13 / .NET 9+) — zero heap allocation
public static double Average(params ReadOnlySpan<double> values)
{
    if (values.IsEmpty) return 0;
    double total = 0;
    foreach (var v in values) total += v;
    return total / values.Length;
}

Console.WriteLine(Average(10.0, 20.0, 30.0)); // Output: 20
```

**Rules:**
- Only **one** `params` parameter per method.
- Must be the **last** parameter.
- Cannot be combined with `ref`, `out`, or `in`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `yield` keyword in C#? Provide an example of using it with an iterator?

The `yield` keyword is used inside an **iterator method** to return elements one at a time, enabling lazy evaluation. The method\'s state is preserved between calls, so execution resumes where it left off.

- `yield return` — returns the next value to the caller.
- `yield break` — stops the iteration.

**Key benefits:**
- Lazy evaluation: elements are generated on demand, not all at once.
- Memory efficient: no need to build a full collection in memory.
- Works with `foreach`, LINQ, and `await foreach` (when returning `IAsyncEnumerable<T>`).

**Example — synchronous iterator:**

```cs
public IEnumerable<int> EvenNumbers(int max)
{
    for (int i = 0; i <= max; i += 2)
    {
        Console.WriteLine($"Yielding {i}");
        yield return i;
    }
}

foreach (var n in EvenNumbers(10))
    Console.Write(n + " "); // Output: 0 2 4 6 8 10
```

**Example — infinite sequence with `yield`:**

```cs
public IEnumerable<int> Fibonacci()
{
    int a = 0, b = 1;
    while (true)
    {
        yield return a;
        (a, b) = (b, a + b);
    }
}

var first10 = Fibonacci().Take(10).ToList();
Console.WriteLine(string.Join(", ", first10));
// Output: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34
```

**Example — async iterator (C# 8+, .NET Core 3+):**

```cs
public async IAsyncEnumerable<string> ReadLinesAsync(string path)
{
    await foreach (var line in File.ReadLinesAsync(path))
        yield return line.ToUpperInvariant();
}

await foreach (var line in ReadLinesAsync("data.txt"))
    Console.WriteLine(line);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the "yield" keyword used for in C#?

The `yield` keyword is used in an **iterator method** to lazily produce a sequence of values one at a time. See the full answer with examples in the [`yield` with iterator](#q-what-is-the-purpose-of-the-yield-keyword-in-c-provide-an-example-of-using-it-with-an-iterator) section above.

**Key points:**
- `yield return <value>` — returns the next element and suspends execution until the next iteration.
- `yield break` — ends the sequence early.
- The method return type must be `IEnumerable<T>`, `IEnumerator<T>`, or `IAsyncEnumerable<T>`.
- State is preserved between `yield return` calls — no manual state machine needed.

```cs
// Lazy sequence — elements generated only when consumed
public IEnumerable<int> Countdown(int from)
{
    for (int i = from; i >= 0; i--)
        yield return i;
    yield return -1; // sentinel — indicates sequence ended
}

foreach (var n in Countdown(3))
    Console.Write(n + " "); // Output: 3 2 1 0 -1

// Short-circuit: yield break stops iteration
public IEnumerable<int> TakeWhilePositive(IEnumerable<int> source)
{
    foreach (var item in source)
    {
        if (item <= 0) yield break;
        yield return item;
    }
}

var result = TakeWhilePositive([5, 3, 1, -2, 4]).ToList();
Console.WriteLine(string.Join(", ", result)); // Output: 5, 3, 1
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the importance of "this" keyword?

The `this` keyword refers to the **current instance** of a class or struct. 

**Summary of uses:**

| Use | Description |
|-----|-------------|
| Disambiguate | Distinguish between a field and a parameter with the same name |
| Constructor chaining | `this(...)` calls another constructor in the same class |
| Pass current instance | Pass `this` as an argument to other methods |
| Fluent API | Return `this` from methods to enable method chaining |
| Extension methods | The extended type\'s instance is accessible as `this` in the extension |

```cs
public class Counter
{
    private int _count;

    // 1. Disambiguate field vs. parameter
    public Counter(int count) => this._count = count;

    // 4. Fluent API — return this
    public Counter Increment() { _count++; return this; }
    public Counter Add(int n)  { _count += n; return this; }

    public int Value => _count;

    // 2. Constructor chaining
    public Counter() : this(0) { } // delegates to Counter(int)
}

var result = new Counter()
    .Increment()
    .Increment()
    .Add(5)
    .Value;

Console.WriteLine(result); // Output: 7
```

**`this` in primary constructors (C# 12+):**

```cs
public class Order(int id, string customer)
{
    public int Id { get; } = id;
    public string Customer { get; } = customer;

    // this refers to the current instance in methods
    public Order WithCustomer(string newCustomer) => new Order(this.Id, newCustomer);
}

var o1 = new Order(1, "Alice");
var o2 = o1.WithCustomer("Bob");
Console.WriteLine(o2.Customer); // Output: Bob
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `ref` and `out` keywords?

Both `ref` and `out` pass arguments **by reference** (the method receives a pointer to the original variable, not a copy), but they have different rules around initialization.

| Aspect | `ref` | `out` |
|--------|-------|-------|
| Must be initialized before call | **Yes** — caller must assign first | **No** — can be uninitialized |
| Method must assign before return | No — method may or may not modify it | **Yes** — method must assign before returning |
| Primary use | Two-way data exchange | Return multiple values from a method |
| Inline declaration (C# 7+) | No | Yes — `out int result` |
| Discard allowed | No | Yes — `out _` |

**`ref` — read AND write by caller and method:**

```cs
public void Double(ref int value)
{
    value *= 2; // modifies the caller\'s variable
}

int x = 5;
Double(ref x);
Console.WriteLine(x); // Output: 10
```

**`out` — write-only output; method must assign:**

```cs
public bool TryParse(string s, out int result)
{
    if (int.TryParse(s, out result))
        return true;
    result = 0; // must assign before return
    return false;
}

// Inline declaration (C# 7+)
if (TryParse("42", out int value))
    Console.WriteLine(value); // Output: 42

// Discard when result not needed
if (TryParse("abc", out _))
    Console.WriteLine("valid");
else
    Console.WriteLine("invalid"); // Output: invalid
```

**Side-by-side comparison:**

```cs
// ref — x must be assigned before passing
int x = 10;
Multiply(ref x, 3);
Console.WriteLine(x); // Output: 30

static void Multiply(ref int n, int factor) => n *= factor;

// out — y does NOT need to be assigned before passing
GetSquare(5, out int y);
Console.WriteLine(y); // Output: 25

static void GetSquare(int n, out int result) => result = n * n;
```

**Modern alternative — prefer tuples for multiple return values:**

```cs
// Instead of multiple out parameters:
public (int Min, int Max) GetMinMax(int[] arr) =>
    (arr.Min(), arr.Max());

var (min, max) = GetMinMax([3, 1, 4, 1, 5, 9]);
Console.WriteLine($"Min={min}, Max={max}"); // Min=1, Max=9
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a `partial` class in C#?

A **partial class** splits the definition of a class, struct, interface, or record across multiple files. All parts are combined into a single type at compile time.

**Use cases:**
- Machine-generated code (e.g., source generators, designers, scaffolding) in one file + human-written logic in another.
- Large classes split for organizational clarity.

**Example:**

```cs
// File: Order.cs
public partial class Order
{
    public int Id { get; init; }
    public string CustomerName { get; init; } = string.Empty;
}

// File: Order.Validation.cs
public partial class Order
{
    public bool IsValid() =>
        Id > 0 && !string.IsNullOrWhiteSpace(CustomerName);
}

// File: Order.Display.cs
public partial class Order
{
    public override string ToString() => $"Order #{Id} for {CustomerName}";
}

// Usage
var order = new Order { Id = 1, CustomerName = "Pradeep" };
Console.WriteLine(order);           // Output: Order #1 for Pradeep
Console.WriteLine(order.IsValid()); // Output: True
```

**Partial methods (C# 9+):**

Allows one part to declare a method signature and another to optionally implement it. If not implemented, calls are removed by the compiler.

```cs
public partial class DataProcessor
{
    public partial void OnDataLoaded(string data); // declaration
}

public partial class DataProcessor
{
    public partial void OnDataLoaded(string data) // implementation
        => Console.WriteLine($"Loaded: {data}");
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a `static` class in C#?

A **static class** is a class that cannot be instantiated or inherited. All its members must also be `static`. It is sealed by default.

**Use cases:**
- Utility/helper methods (e.g., `Math`, `File`, `Path`).
- Extension method containers.
- Factory methods or constants shared globally.

**Example:**

```cs
public static class MathHelper
{
    public const double GoldenRatio = 1.6180339887;

    public static double CircleArea(double radius) => Math.PI * radius * radius;

    public static int Clamp(int value, int min, int max)
        => Math.Max(min, Math.Min(max, value));
}

Console.WriteLine(MathHelper.CircleArea(5));   // Output: 78.53...
Console.WriteLine(MathHelper.Clamp(15, 0, 10)); // Output: 10
```

**Extension method host (must be static class):**

```cs
public static class StringExtensions
{
    public static bool IsEmail(this string s) =>
        s.Contains('@') && s.Contains('.');
}

Console.WriteLine("user@example.com".IsEmail()); // True
```

**Key rules:**

| Rule | Detail |
|------|--------|
| Cannot be instantiated | `new MathHelper()` is a compile error |
| Cannot be inherited | Implicitly `sealed` |
| All members must be `static` | Including nested types |
| Can have `static` constructor | Runs once before first access |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the different types of classes in C#?

C# supports several class types, each serving a distinct purpose:

| Class type | Description |
|---|---|
| **Concrete class** | The default — can be instantiated directly |
| **Abstract class** | Cannot be instantiated; may contain abstract members that derived classes must implement |
| **Sealed class** | Cannot be inherited (e.g., `string`, `StringBuilder`) |
| **Static class** | Cannot be instantiated or inherited; all members are `static` |
| **Partial class** | Definition split across multiple files; combined at compile time |
| **Generic class** | Parameterised by one or more type arguments (e.g., `List<T>`) |
| **Record class** | Immutable reference type with value-based equality (C# 9+) |
| **Nested class** | Declared inside another class |

**Examples:**

```cs
// Concrete
public class Car { public string Model { get; set; } = ""; }

// Abstract
public abstract class Shape { public abstract double Area(); }

// Sealed
public sealed class Singleton
{
    public static Singleton Instance { get; } = new();
    private Singleton() { }
}

// Static
public static class MathUtils
{
    public static double Square(double x) => x * x;
}

// Generic
public class Repository<T> where T : class
{
    private readonly List<T> _store = [];
    public void Add(T item) => _store.Add(item);
    public IEnumerable<T> GetAll() => _store;
}

// Record (C# 9+) — immutable, value equality
public record class Product(string Name, decimal Price);

var p1 = new Product("Laptop", 999m);
var p2 = new Product("Laptop", 999m);
Console.WriteLine(p1 == p2); // True (value equality)

// Partial (split across files)
public partial class Order { public int Id { get; init; } }
public partial class Order { public bool IsValid() => Id > 0; }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Are private class members inherited to the derived class?

**Yes — private members are inherited (they exist in memory), but they are not accessible in derived classes.**

The derived class contains the private members of the base class as part of its object layout, but the compiler prevents direct access to them from derived class code.

```cs
public class Base
{
    private int _secret = 42;      // private field
    private void PrivateHelper()   // private method
        => Console.WriteLine("Base private method");

    public int GetSecret() => _secret;    // public accessor
    protected void CallHelper() => PrivateHelper(); // can call internally
}

public class Derived : Base
{
    public void Show()
    {
        // Console.WriteLine(_secret);  // ❌ compile error — not accessible
        // PrivateHelper();             // ❌ compile error — not accessible

        Console.WriteLine(GetSecret()); // ✅ access via public method
        CallHelper();                   // ✅ access via protected method
    }
}

var d = new Derived();
d.Show();
// Output:
// 42
// Base private method
```

**Proof that private members exist in derived objects:**

```cs
// Reflection can reveal them
var fields = typeof(Derived)
    .GetFields(System.Reflection.BindingFlags.NonPublic |
               System.Reflection.BindingFlags.Instance);

foreach (var f in fields)
    Console.WriteLine(f.Name); // Output: _secret (inherited but private)
```

**Summary:**

| Member access | Inherited (exists in memory)? | Accessible in derived class? |
|---|---|---|
| `public` | ✅ | ✅ |
| `protected` | ✅ | ✅ |
| `internal` | ✅ | ✅ (same assembly) |
| `private` | ✅ | ❌ |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are partial classes?

A **partial class** splits a single class definition across multiple source files. The `partial` keyword is required on all parts. The compiler merges them into one class at compile time.

**Use cases:**
- **Code generators** — one file is machine-generated (e.g., EF Core scaffold, WinForms Designer), the other is hand-written.
- **Large classes** — split for organizational clarity without breaking encapsulation.
- **Source generators** (C# 9+) — generators add members to a partial class you define.

```cs
// File: Customer.cs  — hand-written
public partial class Customer
{
    public int Id { get; init; }
    public string Name { get; init; } = string.Empty;
    public string Email { get; init; } = string.Empty;
}

// File: Customer.Validation.cs — hand-written
public partial class Customer
{
    public bool IsValid() =>
        Id > 0 &&
        !string.IsNullOrWhiteSpace(Name) &&
        Email.Contains('@');
}

// File: Customer.g.cs — could be machine-generated
public partial class Customer
{
    public override string ToString() => $"[{Id}] {Name} <{Email}>";
}

// Usage — all parts are merged into one Customer type
var c = new Customer { Id = 1, Name = "Pradeep", Email = "p@example.com" };
Console.WriteLine(c);           // Output: [1] Pradeep <p@example.com>
Console.WriteLine(c.IsValid()); // Output: True
```

**Partial methods (C# 9+ — must have access modifiers):**

```cs
public partial class DataPipeline
{
    // Declared in one part — implementation is optional in older C#
    // In C# 9+, partial methods with access modifiers MUST be implemented
    public partial void OnProcessed(string data);
}

public partial class DataPipeline
{
    public partial void OnProcessed(string data)
        => Console.WriteLine($"Processed: {data}");
}

var pipeline = new DataPipeline();
pipeline.OnProcessed("record-1"); // Output: Processed: record-1
```

**Rules:**
- All parts must have the same access modifier.
- All parts must be in the same assembly and namespace.
- Works on classes, structs, interfaces, and records.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between a struct and a class in C#?

Both `struct` and `class` define types with members, but they have fundamental behavioral differences.

| Feature                 | `class`                              | `struct`                              |
|-------------------------|--------------------------------------|---------------------------------------|
| Type kind               | Reference type (heap)                | Value type (stack / inline in object) |
| Null                    | Can be `null`                        | Cannot be `null` (unless `Nullable<T>`)|
| Assignment              | Copies the reference                 | Copies the entire value               |
| Inheritance             | Supports full inheritance            | Cannot inherit (only interfaces)      |
| Default constructor     | Compiler generates one               | Auto-provided; custom allowed (C# 10+)|
| `record` syntax         | `record class` (C# 9+)              | `record struct` (C# 10+)             |
| Performance             | Heap allocation + GC pressure        | Stack-allocated, GC-friendly          |

**Class example (reference semantics):**

```cs
public class PointClass { public int X; public int Y; }

var a = new PointClass { X = 1, Y = 2 };
var b = a;       // b references the same object
b.X = 99;
Console.WriteLine(a.X); // Output: 99 (same object)
```

**Struct example (value semantics):**

```cs
public struct PointStruct { public int X; public int Y; }

var a = new PointStruct { X = 1, Y = 2 };
var b = a;       // b is a copy
b.X = 99;
Console.WriteLine(a.X); // Output: 1 (original unchanged)
```

**`record struct` (C# 10+) — immutable value type with equality:**

```cs
public readonly record struct Vector2D(double X, double Y)
{
    public double Length => Math.Sqrt(X * X + Y * Y);
}

var v = new Vector2D(3, 4);
Console.WriteLine(v.Length); // Output: 5
```

**Guidelines:**
- Use `class` for complex objects with behavior, identity, or mutable state.
- Use `struct` for small, immutable, frequently copied data (e.g., coordinates, colors, currency).
- Prefer `readonly record struct` for immutable value objects in modern .NET.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `public`, `private`, `protected`, and `internal` access modifiers?

Access modifiers control the visibility and accessibility of types and members in C#.

| Modifier              | Accessible from                                      |
|-----------------------|------------------------------------------------------|
| `public`              | Anywhere (same assembly + other assemblies)          |
| `private`             | Only within the same class or struct                 |
| `protected`           | Same class + derived classes (any assembly)          |
| `internal`            | Anywhere within the **same assembly**                |
| `protected internal`  | Same assembly **or** derived classes in any assembly |
| `private protected`   | Same class + derived classes within the **same assembly** (C# 7.2+) |
| `file` (C# 11+)       | Only within the same **source file**                 |

**Example:**

```cs
public class BankAccount
{
    private decimal _balance;          // only this class
    protected string OwnerId { get; }  // this class + subclasses
    internal string BranchCode { get; } // same assembly

    public BankAccount(string ownerId, string branchCode)
    {
        OwnerId = ownerId;
        BranchCode = branchCode;
    }

    public void Deposit(decimal amount)
    {
        if (amount <= 0) throw new ArgumentException("Must be positive");
        _balance += amount;
    }

    public decimal GetBalance() => _balance;
}

public class PremiumAccount : BankAccount
{
    public PremiumAccount(string ownerId) : base(ownerId, "PREM")
    {
        Console.WriteLine(OwnerId);    // OK: protected
        Console.WriteLine(BranchCode); // OK: internal (same assembly)
        // Console.WriteLine(_balance); // Error: private
    }
}
```

**`file` modifier (C# 11+):**

```cs
// Only usable within this .cs file — useful for source generators
file class InternalHelper
{
    public static void DoWork() => Console.WriteLine("Working...");
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. C# provides a default constructor for me. I write a constructor that takes a string as a parameter, but want to keep the no parameter one. How many constructors should I write?

**You need to write 2 constructors explicitly.**

When you add any constructor with parameters, the compiler stops generating the default (no-parameter) constructor automatically. To keep both, you must declare both explicitly:

```cs
public class Person
{
    public string Name { get; }
    public int Age { get; }

    // 1. Explicit no-parameter constructor (compiler no longer auto-generates this)
    public Person()
    {
        Name = "Unknown";
        Age  = 0;
    }

    // 2. Parameterized constructor
    public Person(string name)
    {
        Name = name;
        Age  = 0;
    }
}

var p1 = new Person();          // uses no-param constructor
var p2 = new Person("Pradeep"); // uses string constructor

Console.WriteLine(p1.Name); // Output: Unknown
Console.WriteLine(p2.Name); // Output: Pradeep
```

**Tip:** Use constructor chaining (`this()`) to avoid duplication:

```cs
public class Person
{
    public string Name { get; }
    public int Age { get; }

    public Person() : this("Unknown") { }            // chains to string ctor
    public Person(string name) : this(name, 0) { }   // chains to full ctor
    public Person(string name, int age) { Name = name; Age = age; }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Why do we need constructors?

A **constructor** is a special method that initialises an object\'s state when it is created. Without constructors, fields would be left at default values and the object might be in an invalid or inconsistent state.

**Reasons we need constructors:**

| Purpose | Description |
|---------|-------------|
| **Initialisation** | Set fields/properties to meaningful initial values |
| **Validation** | Enforce invariants — reject invalid state at creation time |
| **Dependency injection** | Receive required dependencies when the object is created |
| **Encapsulation** | Control how an object is constructed |

```cs
public class BankAccount
{
    public string Owner { get; }
    public decimal Balance { get; private set; }

    // Constructor enforces invariants
    public BankAccount(string owner, decimal initialDeposit)
    {
        if (string.IsNullOrWhiteSpace(owner))
            throw new ArgumentException("Owner name required");
        if (initialDeposit < 0)
            throw new ArgumentOutOfRangeException(nameof(initialDeposit), "Must be >= 0");

        Owner   = owner;
        Balance = initialDeposit;
    }

    public void Deposit(decimal amount) => Balance += amount;
}

var acc = new BankAccount("Pradeep", 1000m);
Console.WriteLine($"{acc.Owner}: {acc.Balance:C}"); // Pradeep: ₹1,000.00

// var bad = new BankAccount("", -100); // ❌ throws at construction
```

**Primary constructors (C# 12 / .NET 8+):**

```cs
// Concise — parameters available throughout the class
public class Product(string name, decimal price)
{
    public string Name  { get; } = name;
    public decimal Price { get; } = price > 0 ? price
        : throw new ArgumentException("Price must be positive");
}

var p = new Product("Laptop", 999m);
Console.WriteLine($"{p.Name}: {p.Price:C}"); // Laptop: ₹999.00
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. In parent child which constructor fires first?

The **base (parent) class constructor always fires first**, before the derived (child) class constructor. This guarantees that the base part of the object is fully initialised before derived initialisation runs.

```cs
public class GrandParent
{
    public GrandParent() => Console.WriteLine("1. GrandParent constructor");
}

public class Parent : GrandParent
{
    public Parent() => Console.WriteLine("2. Parent constructor");
}

public class Child : Parent
{
    public Child() => Console.WriteLine("3. Child constructor");
}

var c = new Child();
// Output:
// 1. GrandParent constructor
// 2. Parent constructor
// 3. Child constructor
```

**With parameterized constructors and `base()`:**

```cs
public class Animal
{
    public string Name { get; }
    public Animal(string name)
    {
        Name = name;
        Console.WriteLine($"Animal created: {name}");
    }
}

public class Dog : Animal
{
    public string Breed { get; }
    public Dog(string name, string breed) : base(name) // base runs first
    {
        Breed = breed;
        Console.WriteLine($"Dog created: {breed}");
    }
}

var d = new Dog("Rex", "Labrador");
// Output:
// Animal created: Rex      ← base runs first
// Dog created: Labrador    ← derived runs second
```

**Rule:** The `base()` call (explicit or implicit) always executes before the body of the derived constructor.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the Constructor Chaining in C#?

**Constructor chaining** is calling one constructor from another in the same class using `this(...)` or from a derived class using `base(...)`. It avoids code duplication and centralises initialisation logic.

**`this(...)` — chain within the same class:**

```cs
public class Order
{
    public int Id { get; }
    public string Customer { get; }
    public decimal Discount { get; }

    // Most specific constructor — all initialisation here
    public Order(int id, string customer, decimal discount)
    {
        Id       = id;
        Customer = customer;
        Discount = discount;
        Console.WriteLine($"Order {id} for {customer}, discount {discount:P}");
    }

    // Chains to the full constructor with defaults
    public Order(int id, string customer) : this(id, customer, 0m) { }

    // Chains further
    public Order(int id) : this(id, "Guest") { }
}

new Order(1);                  // Order 1 for Guest, discount 0%
new Order(2, "Pradeep");       // Order 2 for Pradeep, discount 0%
new Order(3, "Alice", 0.15m);  // Order 3 for Alice, discount 15%
```

**`base(...)` — chain to parent constructor:**

```cs
public class Vehicle
{
    public string Brand { get; }
    public Vehicle(string brand)
    {
        Brand = brand;
        Console.WriteLine($"Vehicle: {brand}");
    }
}

public class Car : Vehicle
{
    public int Doors { get; }
    public Car(string brand, int doors) : base(brand) // calls Vehicle(string)
    {
        Doors = doors;
        Console.WriteLine($"Car: {doors} doors");
    }
    public Car(string brand) : this(brand, 4) { } // chains to Car(string, int)
}

new Car("Tesla");         // Vehicle: Tesla → Car: 4 doors
new Car("BMW", 2);        // Vehicle: BMW   → Car: 2 doors
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the different ways a method can be overloaded?

**Method overloading** means defining multiple methods in the same class with the same name but different **parameter lists**. The compiler picks the correct overload at compile time based on argument types.

**Ways to overload:**

| Variation | Example |
|-----------|---------|
| Different number of parameters | `Add(int a)` vs `Add(int a, int b)` |
| Different parameter types | `Print(int n)` vs `Print(string s)` |
| Different parameter order | `Log(string msg, int level)` vs `Log(int level, string msg)` |
| `params` vs explicit | `Sum(int a, int b)` vs `Sum(params int[] nums)` |

**❌ NOT valid for overloading:**
- Different return type only
- Different parameter names only
- `ref`/`out` alone (compiler cannot always distinguish)

```cs
public class Converter
{
    // Different number of parameters
    public string Format(int n) => $"{n}";
    public string Format(int n, string prefix) => $"{prefix}{n}";

    // Different parameter types
    public double Round(double value) => Math.Round(value);
    public decimal Round(decimal value) => Math.Round(value);

    // Different parameter order
    public string Build(string name, int age) => $"{name}, {age}";
    public string Build(int age, string name) => $"{age}: {name}";

    // params overload
    public int Sum(int a, int b) => a + b;
    public int Sum(params int[] nums) => nums.Sum();
}

var c = new Converter();
Console.WriteLine(c.Format(42));          // 42
Console.WriteLine(c.Format(42, "ID-")); // ID-42
Console.WriteLine(c.Round(3.567));        // 4
Console.WriteLine(c.Sum(1, 2));           // 3
Console.WriteLine(c.Sum(1, 2, 3, 4, 5)); // 15
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. When and why to use method overloading?

Use method overloading when you want to provide **multiple convenient ways to call the same logical operation** with different inputs — without forcing callers to remember different method names.

**When to use:**
- Same operation, different input types (e.g., `Draw(Circle)`, `Draw(Rectangle)`)
- Optional parameters with meaningful defaults (prefer overloads over default params when callers must be clear)
- API evolution — add a new overload without breaking existing callers

**Why it improves code:**
- **Readability** — callers use one intuitive method name
- **Discoverability** — IntelliSense shows all variants together
- **Type safety** — each overload can handle its input correctly without casting

```cs
// Logging API — callers don\'t need to know about formatting details
public class Logger
{
    public void Log(string message)
        => Console.WriteLine($"[INFO]  {message}");

    public void Log(string message, LogLevel level)
        => Console.WriteLine($"[{level}] {message}");

    public void Log(Exception ex)
        => Console.WriteLine($"[ERROR] {ex.GetType().Name}: {ex.Message}");

    public void Log(string message, Exception ex)
        => Console.WriteLine($"[ERROR] {message}: {ex.Message}");
}

public enum LogLevel { Info, Warning, Error }

var log = new Logger();
log.Log("Application started");
log.Log("High memory usage", LogLevel.Warning);
log.Log(new InvalidOperationException("State error"));
log.Log("Unhandled exception", new Exception("Boom"));
```

**Prefer overloading over optional parameters when:**
- Different overloads need different validation logic
- You want to avoid a single method with many nullable parameters
- The combinations are meaningful and not just "add more defaults"

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Is operator overloading supported in C#?

**Yes.** C# allows you to overload most built-in operators for custom types using the `operator` keyword on a `public static` method. This lets instances of your types work naturally with `+`, `-`, `==`, `<`, etc.

**Overloadable operators:** `+`, `-`, `*`, `/`, `%`, `==`, `!=`, `<`, `>`, `<=`, `>=`, `!`, `~`, `++`, `--`, `&`, `|`, `^`, `<<`, `>>`, `true`, `false`

**Example — `Vector2D` with operator overloading:**

```cs
public readonly record struct Vector2D(double X, double Y)
{
    // Arithmetic
    public static Vector2D operator +(Vector2D a, Vector2D b) => new(a.X + b.X, a.Y + b.Y);
    public static Vector2D operator -(Vector2D a, Vector2D b) => new(a.X - b.X, a.Y - b.Y);
    public static Vector2D operator *(Vector2D v, double scalar) => new(v.X * scalar, v.Y * scalar);

    // Equality (record already provides == and != via value equality)
    public double Length => Math.Sqrt(X * X + Y * Y);

    public override string ToString() => $"({X}, {Y})";
}

var v1 = new Vector2D(1, 2);
var v2 = new Vector2D(3, 4);

Console.WriteLine(v1 + v2);   // Output: (4, 6)
Console.WriteLine(v2 - v1);   // Output: (2, 2)
Console.WriteLine(v1 * 3);    // Output: (3, 6)
Console.WriteLine(v1 == new Vector2D(1, 2)); // True (record equality)
Console.WriteLine(v1.Length); // Output: 2.236...
```

**Comparison operators — must be overloaded in pairs:**

```cs
public class Temperature : IComparable<Temperature>
{
    public double Celsius { get; }
    public Temperature(double c) => Celsius = c;

    public static bool operator <(Temperature a, Temperature b) => a.Celsius < b.Celsius;
    public static bool operator >(Temperature a, Temperature b) => a.Celsius > b.Celsius;
    public static bool operator ==(Temperature a, Temperature b) => a.Celsius == b.Celsius;
    public static bool operator !=(Temperature a, Temperature b) => a.Celsius != b.Celsius;

    public int CompareTo(Temperature? other) => Celsius.CompareTo(other?.Celsius);
    public override bool Equals(object? obj) => obj is Temperature t && t.Celsius == Celsius;
    public override int GetHashCode() => Celsius.GetHashCode();
}

var t1 = new Temperature(100);
var t2 = new Temperature(37);
Console.WriteLine(t1 > t2);  // True
Console.WriteLine(t1 == t2); // False
```

**Note:** `&&` and `||` cannot be overloaded directly — overload `true`/`false`/`&`/`|` instead.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can `this` be used within a static method?

**No.** `this` refers to the **current instance** of a class, but static methods do not belong to any instance — they belong to the type itself. Therefore, `this` has no meaning inside a static method and the compiler will produce an error.

```cs
public class Counter
{
    private int _count = 0;

    public void Increment() => _count++;  // instance method — 'this' available

    public static Counter Create()
    {
        // Console.WriteLine(this._count); // ❌ Compile error: CS0026
        // 'this' is not valid in a static member
        return new Counter();              // ✅ create a new instance instead
    }

    public static int Compare(Counter a, Counter b) =>
        a._count.CompareTo(b._count); // ✅ work with explicit instances
}

var c = Counter.Create();
c.Increment();
```

**Why:** Static methods are resolved at compile time on the type, not a runtime instance. There is no object to which `this` could refer.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you declare an override method to be static if the original method is not static?

**No.** You cannot change the `static`/instance nature of a method when overriding. An `override` method must match the base method\'s signature exactly — same name, same parameter types, same staticness.

```cs
public class Base
{
    public virtual void Show() => Console.WriteLine("Base");
}

public class Derived : Base
{
    // ❌ Compile error CS0106: cannot change 'virtual' to 'static' in override
    // public static override void Show() => Console.WriteLine("Derived");

    // ✅ Correct override — non-static like the base
    public override void Show() => Console.WriteLine("Derived");
}
```

**Summary of override rules:**

| Attempt | Allowed? |
|---------|----------|
| `virtual` → `override` (non-static) | ✅ |
| `virtual` non-static → `static override` | ❌ |
| `static` method → `static override` | ❌ (static methods cannot be virtual) |
| `abstract` → `override` | ✅ |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you declare the override method static while the original method is non-static?

**No** — same answer as above. `override` requires the method to be non-static if the base method is non-static. Attempting to combine `static` and `override` on a method that overrides a virtual instance method is a compile error.

The only way to introduce a static method with the same name is to use `new` (method hiding), which is a completely separate method that is not polymorphic:

```cs
public class Base
{
    public virtual void Process() => Console.WriteLine("Base instance Process");
}

public class Derived : Base
{
    // ✅ Hides (does NOT override) the base virtual method
    public new static void Process() => Console.WriteLine("Derived static Process");

    // ✅ Still override the virtual instance method separately if needed
    public override void Process() => Console.WriteLine("Derived instance Process");
}

// Warning: hiding causes confusion — use with care and explicit casts:
Derived.Process();                   // Output: Derived static Process
Base b = new Derived();
b.Process();                         // Output: Derived instance Process (override wins)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Why can you have only one static constructor?

A **static constructor** initialises the type itself (not instances). Because there is exactly one copy of the type, it needs to be initialised exactly once — so only one static constructor is allowed and it takes **no parameters**.

```cs
public class AppConfig
{
    public static string ConnectionString { get; }
    public static int MaxRetries { get; }

    // One static constructor — no parameters, no access modifier
    static AppConfig()
    {
        ConnectionString = Environment.GetEnvironmentVariable("DB_CONN")
                           ?? "Server=localhost;Database=App";
        MaxRetries = 3;
        Console.WriteLine("AppConfig initialised once");
    }
}

// Accessed multiple times — static constructor runs only once
Console.WriteLine(AppConfig.ConnectionString); // "initialised once" printed here
Console.WriteLine(AppConfig.MaxRetries);       // no re-initialisation
```

**Reasons only one is allowed:**
1. **No parameters** — you can\'t distinguish overloads without parameters.
2. **Single initialisation** — the CLR guarantees it runs exactly once before any static member is first accessed.
3. **Thread safety** — the CLR makes static constructor execution thread-safe automatically; multiple constructors would complicate this guarantee.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. When does the static constructor fire?

The static constructor fires **automatically, once, before the type is first used** — either when a static member is first accessed or when the first instance of the class is created. You cannot call it directly.

```cs
public class Sensor
{
    public static string Model { get; }

    static Sensor()
    {
        Model = "SensorX-2000";
        Console.WriteLine("Static constructor ran");
    }

    public Sensor() => Console.WriteLine("Instance constructor ran");
}

// --- First static member access ---
Console.WriteLine(Sensor.Model);  // triggers static constructor
// Output:
// Static constructor ran
// SensorX-2000

// --- First instance creation (if static ctor not yet run) ---
var s = new Sensor();
// Output:
// Static constructor ran   ← runs first
// Instance constructor ran ← then instance ctor
```

**Timing guarantees:**
- Runs at most **once** per application domain.
- Runs before any instance constructors or static method calls.
- The CLR ensures thread safety — even if multiple threads access the type simultaneously, the static constructor runs only once.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. When will the static constructor be called?

The static constructor is called by the **CLR** (not by developer code) and is guaranteed to run **before the first use of the type** — whichever of these occurs first:

1. **First access to a static member** (field, property, or method)
2. **First instantiation** of the class

```cs
public class Registry
{
    public static Dictionary<string, string> Entries { get; } = new();

    static Registry()
    {
        // Called before Entries is first accessed or Registry is first instantiated
        Entries["version"] = "1.0";
        Entries["env"]     = "production";
        Console.WriteLine("Registry loaded");
    }
}

// Triggered by first static member access:
Console.WriteLine(Registry.Entries["version"]);
// Output:
// Registry loaded
// 1.0

// Second access — static constructor does NOT run again:
Console.WriteLine(Registry.Entries["env"]); // Output: production
```

**Key rules:**
- Cannot be called explicitly.
- No parameters, no access modifier.
- Runs exactly **once** per application domain.
- Exceptions in static constructors cause a `TypeInitializationException` on every subsequent access.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can we declare a Public access modifier for a static constructor?

**No.** Static constructors **cannot have any access modifier** (`public`, `private`, `protected`, `internal`). The CLR controls when it is called; restricting or exposing access would be meaningless. Adding an access modifier is a compile error.

```cs
public class MyClass
{
    // ❌ Compile error CS0515: access modifiers are not allowed on static constructors
    // public static MyClass() { }

    // ✅ Correct — no access modifier
    static MyClass()
    {
        Console.WriteLine("Type initialised");
    }
}
```

**Summary of static constructor rules:**

| Rule | Value |
|------|-------|
| Access modifier | None (not allowed) |
| Parameters | None (not allowed) |
| Return type | None |
| Can be overloaded | No — only one allowed |
| Called by | CLR automatically |
| Called how many times | Once per AppDomain |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. If we declare Main() and a static constructor in the same class, which one will be called first?

The **static constructor fires first**, before `Main()` executes.

When the CLR starts the application, it needs to load and initialise the entry-point class before calling `Main()`. Type initialisation (the static constructor) happens as part of that loading process.

```cs
public class Program
{
    static Program()
    {
        Console.WriteLine("1. Static constructor");
    }

    public static void Main(string[] args)
    {
        Console.WriteLine("2. Main method");
    }
}
// Output:
// 1. Static constructor
// 2. Main method
```

**Why:** The CLR guarantees that all static members are initialised before any method of the type runs. Since `Main()` is a static method of `Program`, the CLR initialises `Program` (runs its static constructor) before entering `Main()`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does dependency inversion benefit? Show with an example.

The **Dependency Inversion Principle (DIP)** — the D in SOLID — states:
1. High-level modules should not depend on low-level modules. Both should depend on **abstractions**.
2. Abstractions should not depend on details. Details should depend on abstractions.

**Benefits:**
- Reduces coupling between layers.
- Makes code easier to test (inject mocks).
- Allows swapping implementations without changing consumers.
- Enables parallel development of components.

**Without DIP (tightly coupled — bad):**

```cs
// High-level module depends directly on low-level SqlServer class
public class OrderService
{
    private readonly SqlOrderRepository _repo = new SqlOrderRepository(); // hard dependency

    public void PlaceOrder(string item)
    {
        _repo.Save(item); // cannot swap to a different repo without changing OrderService
    }
}
```

**With DIP (loosely coupled — good):**

```cs
// 1. Abstraction (interface) — both layers depend on this
public interface IOrderRepository
{
    void Save(string item);
}

// 2. Low-level detail implements the abstraction
public class SqlOrderRepository : IOrderRepository
{
    public void Save(string item) => Console.WriteLine($"SQL: Saved '{item}'");
}

public class InMemoryOrderRepository : IOrderRepository
{
    private readonly List<string> _orders = [];
    public void Save(string item) { _orders.Add(item); Console.WriteLine($"Memory: Saved '{item}'"); }
}

// 3. High-level module depends on abstraction, not concrete class
public class OrderService(IOrderRepository repo) // injected via constructor
{
    public void PlaceOrder(string item) => repo.Save(item);
}

// Production — use SQL
var prodService = new OrderService(new SqlOrderRepository());
prodService.PlaceOrder("Laptop"); // SQL: Saved 'Laptop'

// Test — swap to in-memory without touching OrderService
var testService = new OrderService(new InMemoryOrderRepository());
testService.PlaceOrder("Monitor"); // Memory: Saved 'Monitor'
```

**With ASP.NET Core DI (.NET 10):**

```cs
builder.Services.AddScoped<IOrderRepository, SqlOrderRepository>();
// Swap to InMemoryOrderRepository by changing ONE line — no code changes elsewhere
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Will only Dependency Inversion solve the decoupling problem?

**No.** DIP is essential but not sufficient on its own. Full decoupling requires applying several complementary principles and patterns together.

**What DIP solves:**
- Removes direct class-to-class dependencies (high-level → abstraction ← low-level).
- Enables dependency injection.

**What DIP alone does NOT solve:**

| Gap | Solution |
|-----|---------|
| Too many responsibilities in one class | **SRP** — Single Responsibility Principle |
| Open to breaking changes when extending | **OCP** — Open/Closed Principle |
| Interfaces that are too broad (fat interfaces) | **ISP** — Interface Segregation Principle |
| Subclasses violating base-class contracts | **LSP** — Liskov Substitution Principle |
| Runtime coupling via events | **Observer / Event Aggregator** pattern |
| Service location anti-pattern | **Constructor injection** (not `ServiceLocator.Get<T>()`) |
| Circular dependencies | **Mediator** pattern or redesign |

**Example — DIP alone is not enough:**

```cs
// DIP applied: depends on interface ✅
// BUT violates SRP: does ordering, emailing, AND logging in one class ❌
public class OrderService(IOrderRepository repo, IEmailService email, ILogger logger)
{
    public void PlaceOrder(string item)
    {
        repo.Save(item);            // ordering
        email.Send("Confirmation"); // emailing — should be separate concern
        logger.Log("Order placed"); // logging — cross-cutting concern
    }
}
```

**Better — combine DIP + SRP + OCP:**

```cs
// Separate concerns, each depending on abstractions
public class OrderService(IOrderRepository repo, IOrderEventPublisher events)
{
    public void PlaceOrder(string item)
    {
        repo.Save(item);
        events.Publish(new OrderPlacedEvent(item)); // decoupled via event
    }
}

// Email and logging react to the event — OrderService doesn\'t know about them
```

**Takeaway:** Apply all five SOLID principles together, use dependency injection frameworks (like .NET\'s built-in DI), and consider patterns like Mediator, Observer, and Event Aggregator for full decoupling.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between a value type and a reference type in C#?

This is one of the most fundamental distinctions in C#. It affects memory layout, assignment behavior, equality semantics, and performance.

| Feature | Value type | Reference type |
|---------|------------|----------------|
| Stored in | Stack (or inline in containing object) | Heap |
| Assignment | Copies the value | Copies the reference (both point to same object) |
| Default value | Zero/`false`/`\0` etc. | `null` |
| Equality (default) | Value-based | Reference-based (same object?) |
| Can be `null` | Only via `Nullable<T>` / `T?` | Yes |
| Inheritance | Inherits from `ValueType` → `object` | Inherits from `object` |
| Examples | `int`, `double`, `bool`, `struct`, `enum`, `record struct` | `class`, `string`, `array`, `interface`, `delegate`, `record class` |

**Assignment behavior:**

```cs
// Value type — assignment copies the value
int a = 10;
int b = a;
b = 99;
Console.WriteLine(a); // Output: 10  (a is unchanged — b got a copy)

// Reference type — assignment copies the reference
var list1 = new List<int> { 1, 2, 3 };
var list2 = list1;   // both point to the same List
list2.Add(4);
Console.WriteLine(list1.Count); // Output: 4  (list1 was modified via list2)
```

**Custom struct (value) vs class (reference):**

```cs
public struct PointV { public int X, Y; }    // value type
public class  PointR { public int X, Y; }    // reference type

var sv = new PointV { X = 1, Y = 2 };
var sv2 = sv;  // full copy
sv2.X = 99;
Console.WriteLine(sv.X);  // Output: 1 — copy is independent

var rv = new PointR { X = 1, Y = 2 };
var rv2 = rv;  // reference copy
rv2.X = 99;
Console.WriteLine(rv.X);  // Output: 99 — same object!
```

**Boxing — value type wrapped in reference:**

```cs
int n = 42;
object boxed = n;     // boxing: value type → heap allocation
int unboxed = (int)boxed; // unboxing
Console.WriteLine(unboxed); // Output: 42
```

**Modern `readonly record struct` (C# 10+) — value type with immutability and value equality:**

```cs
public readonly record struct Money(decimal Amount, string Currency)
{
    public override string ToString() => $"{Amount:F2} {Currency}";
}

var m1 = new Money(100m, "USD");
var m2 = new Money(100m, "USD");
Console.WriteLine(m1 == m2);  // Output: True  (value equality)
Console.WriteLine(m1);        // Output: 100.00 USD
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is encapsulation in C# and why is it important?

**Encapsulation** is the OOP principle of bundling data (fields) and the methods that operate on that data into a single unit (class), and restricting direct access to internal state through access modifiers and properties.

**Why it is important:**
- Protects internal state from invalid changes.
- Hides implementation details (abstraction).
- Enables validation logic in setters.
- Makes code easier to maintain and refactor.

**Example — without encapsulation (bad):**

```cs
public class Temperature
{
    public double Celsius; // anyone can set any value
}

var t = new Temperature();
t.Celsius = -9999; // invalid, no protection
```

**Example — with encapsulation (good):**

```cs
public class Temperature
{
    private double _celsius;

    public double Celsius
    {
        get => _celsius;
        set
        {
            if (value < -273.15)
                throw new ArgumentOutOfRangeException(nameof(value), "Below absolute zero!");
            _celsius = value;
        }
    }

    public double Fahrenheit => _celsius * 9 / 5 + 32;

    public override string ToString() => $"{_celsius}°C / {Fahrenheit}°F";
}

var temp = new Temperature { Celsius = 100 };
Console.WriteLine(temp); // Output: 100°C / 212°F
```

**Modern .NET — encapsulation with records and `init`:**

```cs
// Immutable by design — state set once at construction
public record class Product
{
    public required string Name { get; init; }
    public required decimal Price { get; init; }

    // Validation via constructor
    public Product
    {
        if (Price < 0) throw new ArgumentException("Price cannot be negative");
    }
}

var p = new Product { Name = "Laptop", Price = 999m };
// p.Price = -1; // Compile error — init-only
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement encapsulation in C#?

Encapsulation is implemented in C# through three main mechanisms:

1. **Access modifiers** — restrict who can see or modify members.
2. **Properties** — expose controlled read/write access to private fields, with optional validation.
3. **Methods** — expose behaviour while hiding the internal steps.

```cs
public class BankAccount
{
    // 1. Private backing field — hidden from outside
    private decimal _balance;
    private readonly List<string> _transactions = [];

    public string Owner { get; }  // read-only property (init via constructor)

    public BankAccount(string owner, decimal initialDeposit)
    {
        if (string.IsNullOrWhiteSpace(owner))
            throw new ArgumentException("Owner required");
        Owner = owner;
        Deposit(initialDeposit); // use method, not direct field access
    }

    // 2. Property with validation — encapsulates the balance field
    public decimal Balance => _balance; // read-only externally

    // 3. Methods expose controlled behavior
    public void Deposit(decimal amount)
    {
        if (amount <= 0) throw new ArgumentException("Amount must be positive");
        _balance += amount;
        _transactions.Add($"+{amount:C}");
    }

    public void Withdraw(decimal amount)
    {
        if (amount <= 0) throw new ArgumentException("Amount must be positive");
        if (amount > _balance) throw new InvalidOperationException("Insufficient funds");
        _balance -= amount;
        _transactions.Add($"-{amount:C}");
    }

    public IReadOnlyList<string> GetHistory() => _transactions.AsReadOnly();
}

var acc = new BankAccount("Pradeep", 1000m);
acc.Deposit(500m);
acc.Withdraw(200m);

Console.WriteLine(acc.Balance); // Output: 1300
foreach (var t in acc.GetHistory())
    Console.WriteLine(t); // +£1,000.00, +£500.00, -£200.00
```

**Modern approach — `required` + `init` properties (C# 11 / .NET 7+):**

```cs
public class Product
{
    public required string Name  { get; init; }     // set once at construction
    public required decimal Price { get; init; }

    public string Description => $"{Name}: {Price:C}";
}

var p = new Product { Name = "Laptop", Price = 999m };
Console.WriteLine(p.Description); // Laptop: £999.00
// p.Price = 500m; // ❌ compile error — init-only
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you provide an example of encapsulation in C#?

**Example — `Temperature` class with validated property:**

```cs
public class Temperature
{
    private double _celsius;

    public double Celsius
    {
        get => _celsius;
        set
        {
            if (value < -273.15)
                throw new ArgumentOutOfRangeException(nameof(value),
                    "Temperature cannot be below absolute zero (-273.15°C)");
            _celsius = value;
        }
    }

    // Derived property — read-only, computed from internal state
    public double Fahrenheit => _celsius * 9.0 / 5.0 + 32;
    public double Kelvin     => _celsius + 273.15;

    public override string ToString() =>
        $"{_celsius:F1}°C / {Fahrenheit:F1}°F / {Kelvin:F2}K";
}

var t = new Temperature { Celsius = 100 };
Console.WriteLine(t); // Output: 100.0°C / 212.0°F / 373.15K

t.Celsius = -10;
Console.WriteLine(t.Fahrenheit); // Output: 14.0

try
{
    t.Celsius = -300; // ❌ below absolute zero
}
catch (ArgumentOutOfRangeException ex)
{
    Console.WriteLine(ex.Message); // Temperature cannot be below absolute zero
}
```

**Key encapsulation points in this example:**
- `_celsius` is `private` — nobody sets it directly.
- The `set` accessor validates the value before storing.
- `Fahrenheit` and `Kelvin` are computed read-only properties — callers cannot set them.
- The class *owns* the conversion logic; the caller only provides Celsius.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the benefits of using encapsulation in object-oriented programming?

| Benefit | Explanation |
|---------|-------------|
| **Data protection** | Private fields cannot be set to invalid values from outside the class |
| **Validation** | Property setters and constructors enforce invariants at the boundary |
| **Maintainability** | Internal implementation can change without affecting external callers |
| **Reduced coupling** | Consumers depend on the public API (interface), not internals |
| **Testability** | Encapsulated classes are self-contained and easier to unit-test |
| **Readability** | Clear public surface separates "what to use" from "how it works" |

**Example — changing internals without breaking callers:**

```cs
// Version 1 — stores Celsius internally
public class Temperature
{
    private double _celsius;
    public double Celsius { get => _celsius; set => _celsius = value; }
}

// Version 2 — internally switched to Kelvin storage (implementation detail)
// Callers still use .Celsius — nothing breaks!
public class Temperature
{
    private double _kelvin; // changed internal representation

    public double Celsius
    {
        get => _kelvin - 273.15;
        set => _kelvin = value + 273.15; // validation can be added here
    }
}

// Caller — unchanged in both versions
var t = new Temperature { Celsius = 100 };
Console.WriteLine(t.Celsius); // Output: 100
```

The caller never knew about `_celsius` or `_kelvin` — encapsulation made the change invisible.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does encapsulation help in maintaining code?

Encapsulation supports maintainability in four key ways:

**1. Change-safe internals** — You can refactor, optimise, or fix the internal implementation of a class without touching any of its consumers, as long as the public API stays the same.

**2. Single point of change** — Validation logic lives in one place (the property or constructor). Fix a bug once; every caller benefits.

**3. Prevents invalid state propagation** — Bugs caused by one part of the code corrupting another\'s data are stopped at the class boundary.

**4. Self-documenting intent** — A `private` field signals "implementation detail"; a `public` property signals "intended API". Reading the public members is enough to understand how to use the class.

```cs
// Before: no encapsulation — Order total calculated in 12 different places
public class Order { public decimal Total; } // anyone writes to Total

// After: encapsulation — total is always correct, calculated in one place
public class Order
{
    private readonly List<decimal> _lineItems = [];

    public void AddItem(decimal price)
    {
        if (price < 0) throw new ArgumentException("Price cannot be negative");
        _lineItems.Add(price);
    }

    // Total is always consistent — derived from the single source of truth
    public decimal Total => _lineItems.Sum();
    public int ItemCount => _lineItems.Count;
}

var order = new Order();
order.AddItem(49.99m);
order.AddItem(19.99m);

Console.WriteLine(order.Total);     // Output: 69.98
Console.WriteLine(order.ItemCount); // Output: 2

// order.Total = 0; // ❌ compile error — read-only, prevents accidental zeroing
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between encapsulation and abstraction in C#?

Both are OOP pillars and are often confused, but they address different concerns:

| Aspect | Encapsulation | Abstraction |
|--------|---------------|-------------|
| **What it hides** | Internal *data and state* (how the object stores things) | Internal *complexity and implementation* (how the object does things) |
| **Achieved via** | Access modifiers, properties, private fields | Abstract classes, interfaces, method signatures |
| **Who benefits** | Protects the object from misuse | Simplifies what the caller needs to know |
| **Level** | Within a single class | Across the design (class hierarchy, module boundary) |
| **Question answered** | "Who can see/change this?" | "What can I do with this?" |

**Encapsulation example — hiding state:**

```cs
public class Stack<T>
{
    private readonly List<T> _items = []; // hidden — caller doesn\'t know it\'s a List

    public void Push(T item) => _items.Add(item);
    public T Pop()
    {
        if (_items.Count == 0) throw new InvalidOperationException("Stack is empty");
        var top = _items[^1];
        _items.RemoveAt(_items.Count - 1);
        return top;
    }
    public int Count => _items.Count;
}
```

**Abstraction example — hiding how things work:**

```cs
// Caller only knows "I can send notifications" — doesn\'t know SMTP vs push vs SMS
public interface INotificationService
{
    Task NotifyAsync(string userId, string message);
}

public class PushNotificationService : INotificationService
{
    public async Task NotifyAsync(string userId, string message)
    {
        // Complex push notification logic hidden here
        Console.WriteLine($"Push → {userId}: {message}");
        await Task.CompletedTask;
    }
}

// Consumer works with the abstraction, not the detail
public class AlertService(INotificationService notifier)
{
    public Task SendAlertAsync(string userId, string msg) =>
        notifier.NotifyAsync(userId, msg);
}
```

**Together:** Encapsulation protects the *state* inside `PushNotificationService`; abstraction hides *what service is used* from `AlertService`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can encapsulation be violated in C#?

Encapsulation is violated when internal state is exposed or bypassed in ways that allow invalid or unexpected modifications. Common violations:

**1. Public fields — no validation possible:**

```cs
// ❌ Violation — direct public field
public class Circle { public double Radius; } // can be set to -1

// ✅ Fix — property with validation
public class Circle
{
    private double _radius;
    public double Radius
    {
        get => _radius;
        set => _radius = value >= 0 ? value
               : throw new ArgumentException("Radius must be ≥ 0");
    }
}
```

**2. Returning mutable collections directly:**

```cs
// ❌ Violation — caller can mutate internal list
public class Roster
{
    private readonly List<string> _names = ["Alice", "Bob"];
    public List<string> Names => _names; // exposes the internal list
}

var r = new Roster();
r.Names.Clear(); // corrupts internal state!

// ✅ Fix — return read-only view
public IReadOnlyList<string> Names => _names.AsReadOnly();
```

**3. Reflection — bypasses access modifiers (use only in exceptional scenarios):**

```cs
public class Secret { private int _code = 42; }

var s = new Secret();
var field = typeof(Secret).GetField("_code",
    System.Reflection.BindingFlags.NonPublic |
    System.Reflection.BindingFlags.Instance);

field!.SetValue(s, 999); // ⚠️ bypasses encapsulation via reflection
```

**4. Overly broad access modifiers:**

```cs
// ❌ Making implementation details public/internal unnecessarily
public class PaymentProcessor
{
    public string _internalToken = "abc123"; // should be private
}
```

**5. Mutable default property setters without validation:**

```cs
// ❌ Auto-property with public setter — no opportunity to validate
public class Person
{
    public int Age { get; set; } // can be set to -1 or 999
}

// ✅ Validate in setter or use init + constructor validation
public class Person
{
    private int _age;
    public int Age
    {
        get => _age;
        set => _age = value is >= 0 and <= 150 ? value
               : throw new ArgumentOutOfRangeException(nameof(value));
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do access modifiers (public, private, protected, internal) relate to encapsulation in C#?

Access modifiers are the **primary tool for implementing encapsulation** in C#. They define the *visibility boundary* of each member, controlling which code can read or modify internal state.

| Modifier | Visibility | Encapsulation role |
|---|---|---|
| `private` | Same class only | Strongest encapsulation — fields should almost always be `private` |
| `protected` | Same class + derived classes | Shares internals with subclasses while hiding from the outside world |
| `internal` | Same assembly | Shares internals within a module/library, hidden from other assemblies |
| `private protected` | Same class + derived (same assembly) | Most restrictive combination |
| `protected internal` | Derived classes (any) + same assembly | Widest combined modifier |
| `public` | Everywhere | No encapsulation — intentionally exposed API surface |

**How they work together in a well-encapsulated class:**

```cs
public class Employee
{
    // private — never exposed: internal state
    private decimal _salary;
    private readonly List<string> _auditLog = [];

    // public — the intentional API surface
    public string Name { get; }
    public string Department { get; private set; } // readable everywhere, settable here

    // protected — available to payroll sub-hierarchy
    protected decimal BaseSalary => _salary;

    // internal — visible within the HR assembly for reporting
    internal DateTime HireDate { get; }

    public Employee(string name, string department, decimal salary, DateTime hireDate)
    {
        Name       = name;
        Department = department;
        HireDate   = hireDate;
        SetSalary(salary); // use private method to enforce rules
    }

    // private method — implementation detail, not part of public API
    private void SetSalary(decimal value)
    {
        if (value < 0) throw new ArgumentException("Salary must be non-negative");
        _salary = value;
        _auditLog.Add($"{DateTime.UtcNow:u}: Salary set to {value:C}");
    }

    public void AdjustSalary(decimal percentage)
    {
        SetSalary(_salary * (1 + percentage / 100));
    }

    public IReadOnlyList<string> GetAuditLog() => _auditLog.AsReadOnly();
}

var emp = new Employee("Pradeep", "Engineering", 80_000m, DateTime.Today);
emp.AdjustSalary(10); // 10% raise

foreach (var entry in emp.GetAuditLog())
    Console.WriteLine(entry);
```

**Rule of thumb:** Start with `private`. Only widen the access modifier when there is a clear need.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain method hiding?

**Method hiding** occurs when a derived class defines a method with the same name and signature as a base class method using the `new` keyword. The derived method *hides* the base method rather than *overriding* it — it breaks the polymorphic chain.

**Key difference from `override`:**
- `override` — the derived method replaces the base: called even through a base reference.
- `new` (hiding) — the derived method only applies when accessed via the derived type reference.

```cs
public class Animal
{
    public virtual string Sound() => "...";  // virtual — intended for override
    public string Name()  => "Animal";        // non-virtual — hiding candidate
}

public class Dog : Animal
{
    public override string Sound() => "Woof"; // override — polymorphic
    public new string Name()  => "Dog";        // new — hides Animal.Name()
}

// ---- Polymorphism with override ----
Animal a1 = new Dog();
Console.WriteLine(a1.Sound()); // Output: Woof  ← Dog\'s version (override wins)

// ---- Method hiding with new ----
Animal a2 = new Dog();
Console.WriteLine(a2.Name()); // Output: Animal ← reference type decides (base wins!)

Dog d = new Dog();
Console.WriteLine(d.Name());  // Output: Dog    ← derived reference → Dog\'s version
```

**Why method hiding exists:**
- Versioning — a base class added a new method AFTER the derived class defined one with the same name; `new` prevents a compile warning while acknowledging the conflict.
- Intentional API divergence — rare, but sometimes a derived class needs a completely separate method with the same name.

**Warning:** Method hiding breaks the **Liskov Substitution Principle** — code that holds a `Dog` via an `Animal` reference will call the base `Name()` unexpectedly. Prefer `override` for polymorphic behavior.

```cs
// Practical demonstration of the problem
void PrintName(Animal a) => Console.WriteLine(a.Name());

PrintName(new Animal()); // Output: Animal
PrintName(new Dog());    // Output: Animal  ← NOT Dog! Hiding breaks polymorphism
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is polymorphism?

**Polymorphism** ("many forms") is the OOP ability for objects of different types to be treated as instances of a common base type, with behavior resolved at runtime based on the actual type.

C# supports two main forms:

**1. Compile-time (static) polymorphism — method overloading:**

Multiple methods with the same name but different parameter signatures.

```cs
public class Calculator
{
    public int Add(int a, int b) => a + b;
    public double Add(double a, double b) => a + b;
    public string Add(string a, string b) => a + b;
}

var calc = new Calculator();
Console.WriteLine(calc.Add(1, 2));          // 3
Console.WriteLine(calc.Add(1.5, 2.5));      // 4
Console.WriteLine(calc.Add("Hello ", "World")); // Hello World
```

**2. Runtime (dynamic) polymorphism — method overriding:**

Derived classes override `virtual` or `abstract` methods; the correct implementation is chosen at runtime.

```cs
public abstract class Shape
{
    public abstract double Area();
    public void Print() => Console.WriteLine($"{GetType().Name} area: {Area():F2}");
}

public class Circle(double radius) : Shape
{
    public override double Area() => Math.PI * radius * radius;
}

public class Rectangle(double w, double h) : Shape
{
    public override double Area() => w * h;
}

Shape[] shapes = [new Circle(5), new Rectangle(4, 6)];
foreach (var shape in shapes)
    shape.Print();
// Output:
// Circle area: 78.54
// Rectangle area: 24.00
```

**3. Pattern matching polymorphism (C# 8+):**

Another modern way to handle type-based dispatch without inheritance:

```cs
double GetArea(object shape) => shape switch
{
    Circle c       => Math.PI * c.radius * c.radius,
    Rectangle r    => r.w * r.h,
    _              => throw new ArgumentException("Unknown shape")
};
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain the four pillars of OOP in C#?

**1. Encapsulation**

Bundling data and behavior together while hiding internal details via access modifiers and properties.

```cs
public class BankAccount
{
    private decimal _balance;
    public void Deposit(decimal amount) { if (amount > 0) _balance += amount; }
    public decimal Balance => _balance; // read-only access
}
```

**2. Abstraction**

Exposing only relevant details and hiding complexity. Achieved via abstract classes, interfaces, and encapsulation.

```cs
public interface IPaymentGateway
{
    Task<bool> ChargeAsync(decimal amount, string cardToken);
}
// Caller only depends on the interface, not Stripe/PayPal internals
```

**3. Inheritance**

A class inherits members from a base class, promoting code reuse.

```cs
public class Vehicle
{
    public string Brand { get; }
    public Vehicle(string brand) => Brand = brand;
    public virtual string Describe() => $"{Brand} vehicle";
}

public class Car(string brand, int doors) : Vehicle(brand)
{
    public override string Describe() => $"{Brand} car with {doors} doors";
}

Vehicle v = new Car("Toyota", 4);
Console.WriteLine(v.Describe()); // Toyota car with 4 doors
```

**4. Polymorphism**

Objects of different types behave differently through a common interface (method overriding and overloading).

```cs
public abstract class Notification
{
    public abstract void Send(string message);
}

public class EmailNotification : Notification
{
    public override void Send(string message) =>
        Console.WriteLine($"Email: {message}");
}

public class SmsNotification : Notification
{
    public override void Send(string message) =>
        Console.WriteLine($"SMS: {message}");
}

Notification[] notifications = [new EmailNotification(), new SmsNotification()];
foreach (var n in notifications)
    n.Send("Your order is shipped!");
// Email: Your order is shipped!
// SMS: Your order is shipped!
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 4. INHERITANCE

<br/>

## Q. What is inheritance in C# and how does it work?

**Inheritance** allows a class (derived/child class) to acquire the members (fields, properties, methods) of another class (base/parent class) using the `:` syntax. It promotes code reuse and enables polymorphism.

**Key rules:**
- C# supports **single class inheritance** (one direct base class) but **multiple interface implementation**.
- Use `base` to call base class constructors or methods.
- Use `virtual`/`override` for polymorphic behavior.
- Use `sealed` on a class or method to prevent further inheritance/overriding.

**Example (.NET 10 / C# 14):**

```cs
public class Person(string name, int age)
{
    public string Name { get; } = name;
    public int Age { get; } = age;
    public virtual string Describe() => $"{Name}, age {Age}";
}

public class Employee(string name, int age, string department)
    : Person(name, age)
{
    public string Department { get; } = department;
    public override string Describe() =>
        $"{base.Describe()}, {Department} dept";
}

public class Manager(string name, int age, string department, int reports)
    : Employee(name, age, department)
{
    public int DirectReports { get; } = reports;
    public override string Describe() =>
        $"{base.Describe()}, manages {DirectReports} people";
}

Person[] people =
[
    new Person("Alice", 30),
    new Employee("Bob", 35, "Engineering"),
    new Manager("Carol", 45, "Engineering", 10),
];

foreach (var p in people)
    Console.WriteLine(p.Describe());
// Alice, age 30
// Bob, age 35, Engineering dept
// Carol, age 45, Engineering dept, manages 10 people
```

**Constructor chaining with `base`:**

```cs
public class Animal(string name)
{
    public string Name { get; } = name;
}

public class Dog(string name, string breed) : Animal(name)
{
    public string Breed { get; } = breed;
    public override string ToString() => $"{Name} ({Breed})";
}

var d = new Dog("Rex", "Labrador");
Console.WriteLine(d); // Output: Rex (Labrador)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between single inheritance and multiple inheritance in C#?

**Single inheritance** means a class can inherit from exactly **one** base class. C# supports only single class inheritance.

**Multiple inheritance** (inheriting from more than one class simultaneously) is **not supported** for classes in C#. However, a class can implement **multiple interfaces**, which achieves a similar design goal.

| Feature | Single inheritance | Multiple inheritance |
|---|---|---|
| Class | ✅ Supported | ❌ Not supported |
| Interface | ✅ | ✅ (multiple interfaces allowed) |
| Diamond problem | Not possible | Avoided by design |

**Single class inheritance:**

```cs
public class Animal
{
    public string Name { get; }
    public Animal(string name) => Name = name;
    public virtual string Describe() => $"Animal: {Name}";
}

public class Dog(string name) : Animal(name) // single base class
{
    public override string Describe() => $"Dog: {Name}";
}

var d = new Dog("Rex");
Console.WriteLine(d.Describe()); // Output: Dog: Rex
```

**Multiple interface implementation (C# alternative to multiple inheritance):**

```cs
public interface IFlyable  { void Fly(); }
public interface ISwimmable { void Swim(); }

// A class can implement multiple interfaces
public class Duck(string name) : Animal(name), IFlyable, ISwimmable
{
    public void Fly()  => Console.WriteLine($"{Name} is flying");
    public void Swim() => Console.WriteLine($"{Name} is swimming");
    public override string Describe() => $"Duck: {Name}";
}

var duck = new Duck("Donald");
duck.Fly();   // Output: Donald is flying
duck.Swim();  // Output: Donald is swimming

// Multiple interface references
IFlyable  flyer   = duck;
ISwimmable swimmer = duck;
flyer.Fly();   // Donald is flying
swimmer.Swim(); // Donald is swimming
```

**Why C# avoids multiple class inheritance:** The **diamond problem** — if two base classes define the same method, the compiler cannot determine which version to call. Interfaces (with default implementations in C# 8+) sidestep this via explicit interface implementation.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can you use the "base" keyword in C# to call the base class constructor?

The `base` keyword in a constructor\'s initialiser list calls a specific base class constructor **before** the derived constructor body executes.

**Syntax:**
```cs
public DerivedClass(args) : base(args_for_base) { }
```

**Example — calling parameterised base constructors:**

```cs
public class Vehicle
{
    public string Brand { get; }
    public int Year { get; }

    public Vehicle(string brand, int year)
    {
        Brand = brand;
        Year  = year;
        Console.WriteLine($"Vehicle created: {Brand} ({Year})");
    }
}

public class Car : Vehicle
{
    public int Doors { get; }

    // Calls Vehicle(string, int) before Car\'s body runs
    public Car(string brand, int year, int doors) : base(brand, year)
    {
        Doors = doors;
        Console.WriteLine($"Car created: {Doors} doors");
    }
}

public class ElectricCar : Car
{
    public int RangeKm { get; }

    // Chains all the way up: Vehicle → Car → ElectricCar
    public ElectricCar(string brand, int year, int doors, int rangeKm)
        : base(brand, year, doors)
    {
        RangeKm = rangeKm;
        Console.WriteLine($"ElectricCar created: {RangeKm}km range");
    }
}

var ec = new ElectricCar("Tesla", 2025, 4, 500);
// Output (top-down, base first):
// Vehicle created: Tesla (2025)
// Car created: 4 doors
// ElectricCar created: 500km range

Console.WriteLine($"{ec.Brand}, {ec.Doors} doors, {ec.RangeKm}km");
// Output: Tesla, 4 doors, 500km
```

**With primary constructors (C# 12):**

```cs
public class Animal(string name)
{
    public string Name { get; } = name;
}

// Primary constructor + base call
public class Dog(string name, string breed) : Animal(name)
{
    public string Breed { get; } = breed;
    public override string ToString() => $"{Name} ({Breed})";
}

Console.WriteLine(new Dog("Rex", "Labrador")); // Output: Rex (Labrador)
```

**`base` in method calls:**

```cs
public class Logger
{
    public virtual void Log(string msg) => Console.WriteLine($"[LOG] {msg}");
}

public class TimestampLogger : Logger
{
    public override void Log(string msg)
    {
        base.Log(msg); // call base implementation first
        Console.WriteLine($"[{DateTime.UtcNow:HH:mm:ss}]");
    }
}

new TimestampLogger().Log("started");
// [LOG] started
// [14:32:01]
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you explain polymorphism and how it is achieved in C#?

**Polymorphism** ("many forms") is the ability for a single interface or method name to represent different behaviors depending on the runtime type of the object. C# supports three forms:

**1. Compile-time polymorphism — method overloading:**

```cs
public class Printer
{
    public void Print(int value)    => Console.WriteLine($"int: {value}");
    public void Print(string value) => Console.WriteLine($"string: {value}");
    public void Print(double value) => Console.WriteLine($"double: {value}");
}

var p = new Printer();
p.Print(42);      // int: 42
p.Print("Hello"); // string: Hello
p.Print(3.14);    // double: 3.14
```

**2. Runtime polymorphism — method overriding (`virtual`/`override`):**

```cs
public abstract class Shape
{
    public abstract double Area();
    public virtual  string Describe() => $"{GetType().Name}: Area = {Area():F2}";
}

public class Circle(double radius) : Shape
{
    public override double Area() => Math.PI * radius * radius;
}

public class Rectangle(double w, double h) : Shape
{
    public override double Area() => w * h;
}

public class Triangle(double b, double height) : Shape
{
    public override double Area() => 0.5 * b * height;
}

// All three accessed through Shape reference — runtime decides which Area() to call
Shape[] shapes = [new Circle(5), new Rectangle(4, 6), new Triangle(3, 8)];

foreach (var shape in shapes)
    Console.WriteLine(shape.Describe());
// Circle: Area = 78.54
// Rectangle: Area = 24.00
// Triangle: Area = 12.00
```

**3. Interface polymorphism:**

```cs
public interface IAnimal { string Sound(); }

public class Cat : IAnimal { public string Sound() => "Meow"; }
public class Dog : IAnimal { public string Sound() => "Woof"; }
public class Cow : IAnimal { public string Sound() => "Moo"; }

IAnimal[] animals = [new Cat(), new Dog(), new Cow()];
foreach (var a in animals)
    Console.WriteLine(a.Sound()); // Meow / Woof / Moo
```

**4. Pattern matching polymorphism (C# 8+):**

```cs
static double GetArea(Shape shape) => shape switch
{
    Circle c      => Math.PI * 5 * 5,  // type pattern
    Rectangle r   => 4 * 6,
    _             => throw new ArgumentException("Unknown shape")
};
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can you use inheritance and polymorphism together to achieve dynamic dispatch in C#?

**Dynamic dispatch** means the correct method implementation is selected at **runtime** based on the actual type of the object, not the declared reference type. It is achieved by combining inheritance (`virtual`/`override`) with base-type references.

```cs
public abstract class Notification
{
    public string Recipient { get; }
    protected Notification(string recipient) => Recipient = recipient;

    // virtual method — subclasses override; dispatch is dynamic
    public abstract Task SendAsync(string message);

    // Template method pattern — fixed algorithm, dynamic steps
    public async Task NotifyAsync(string message)
    {
        Console.WriteLine($"Preparing notification for {Recipient}...");
        await SendAsync(message); // dynamic dispatch — runtime type decides
        Console.WriteLine("Done.");
    }
}

public class EmailNotification(string recipient, string smtpServer)
    : Notification(recipient)
{
    public override Task SendAsync(string message)
    {
        Console.WriteLine($"[SMTP:{smtpServer}] Email → {Recipient}: {message}");
        return Task.CompletedTask;
    }
}

public class SmsNotification(string recipient, string phoneNumber)
    : Notification(recipient)
{
    public override Task SendAsync(string message)
    {
        Console.WriteLine($"[SMS:{phoneNumber}] → {Recipient}: {message}");
        return Task.CompletedTask;
    }
}

public class PushNotification(string recipient, string deviceToken)
    : Notification(recipient)
{
    public override Task SendAsync(string message)
    {
        Console.WriteLine($"[Push:{deviceToken}] → {Recipient}: {message}");
        return Task.CompletedTask;
    }
}

// Dynamic dispatch — all through the Notification base reference
Notification[] notifications =
[
    new EmailNotification("alice@example.com", "smtp.gmail.com"),
    new SmsNotification("Bob", "+1-555-1234"),
    new PushNotification("Carol", "tok_abc123"),
];

foreach (var n in notifications)
    await n.NotifyAsync("Your order has shipped!"); // runtime picks the right SendAsync
```

**Output:**
```
Preparing notification for alice@example.com...
[SMTP:smtp.gmail.com] Email → alice@example.com: Your order has shipped!
Done.
Preparing notification for Bob...
[SMS:+1-555-1234] → Bob: Your order has shipped!
Done.
Preparing notification for Carol...
[Push:tok_abc123] → Carol: Your order has shipped!
Done.
```

**Key point:** `NotifyAsync` is defined once in the base class. It calls `SendAsync`, which is dispatched dynamically to whichever derived class is actually stored in `n` at runtime. Adding a new notification type (e.g., `SlackNotification`) requires zero changes to `NotifyAsync`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the base keyword in C#, and how is it used in inheritance?

The `base` keyword provides access to members of the **immediate base class** from within a derived class. It has two main uses:

**1. Call a base class constructor (`base(...)` in initialiser list):**

```cs
public class Person(string name, int age)
{
    public string Name { get; } = name;
    public int Age { get; } = age;
}

public class Employee : Person
{
    public string Department { get; }

    // base(name, age) calls Person\'s primary constructor
    public Employee(string name, int age, string department)
        : base(name, age)
    {
        Department = department;
    }

    public override string ToString() => $"{Name} ({Age}) — {Department}";
}

Console.WriteLine(new Employee("Pradeep", 30, "Engineering"));
// Output: Pradeep (30) — Engineering
```

**2. Call a base class method from an overriding method (`base.Method()`):**

```cs
public class Logger
{
    public virtual void Log(string message)
        => Console.WriteLine($"[LOG] {message}");
}

public class AuditLogger : Logger
{
    private readonly List<string> _audit = [];

    public override void Log(string message)
    {
        base.Log(message);            // run the base behavior first
        _audit.Add(message);          // then add audit-specific logic
        Console.WriteLine($"[AUDIT] Recorded: {message}");
    }
}

new AuditLogger().Log("User logged in");
// [LOG] User logged in
// [AUDIT] Recorded: User logged in
```

**3. Access base class properties:**

```cs
public class Shape
{
    public string Color { get; set; } = "Black";
}

public class Circle : Shape
{
    public double Radius { get; }
    public Circle(double radius) => Radius = radius;

    public override string ToString() =>
        $"Circle(r={Radius}, color={base.Color})"; // base.Color
}

Console.WriteLine(new Circle(5) { Color = "Red" });
// Output: Circle(r=5, color=Red)
```

**What `base` cannot do:**
- Access members more than one level up (only the **immediate** parent).
- Be used in static methods (no instance context).
- Call `base.base` — to reach grandparent, restructure the class hierarchy.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the differences between virtual methods and abstract methods in C#, and when each should be used?

| Feature | `virtual` | `abstract` |
|---------|-----------|------------|
| Has a body | ✅ Yes — provides a default implementation | ❌ No body (in non-default-interface scenarios) |
| Must be overridden | ❌ Optional | ✅ Mandatory in concrete derived classes |
| Class requirement | Can be in any non-sealed class | Class **must** be `abstract` |
| Can be instantiated directly | ✅ (if class is concrete) | ❌ Abstract class cannot be instantiated |
| Purpose | Provide sensible default, allow customisation | Define a contract with no default |

**`virtual` — default behavior, optionally overridden:**

```cs
public class Animal
{
    // Provides a default — derived classes may or may not override
    public virtual string Sound() => "...";
    public virtual string Describe() => $"I am a {GetType().Name}";
}

public class Dog : Animal
{
    public override string Sound() => "Woof"; // overrides
    // Describe() not overridden — uses base default
}

public class Cat : Animal
{
    public override string Sound() => "Meow";
    public override string Describe() => "I am a mysterious cat"; // custom
}

Animal[] animals = [new Dog(), new Cat(), new Animal()];
foreach (var a in animals)
    Console.WriteLine($"{a.Describe()} — {a.Sound()}");
// I am a Dog — Woof
// I am a mysterious cat — Meow
// I am a Animal — ...
```

**`abstract` — no default, must be implemented:**

```cs
public abstract class Shape
{
    // No default — every concrete shape must provide its own Area
    public abstract double Area();
    public abstract double Perimeter();

    // Can mix virtual with abstract in the same class
    public virtual string Describe() =>
        $"{GetType().Name}: Area={Area():F2}, P={Perimeter():F2}";
}

public class Circle(double radius) : Shape
{
    public override double Area()      => Math.PI * radius * radius;
    public override double Perimeter() => 2 * Math.PI * radius;
}

public class Square(double side) : Shape
{
    public override double Area()      => side * side;
    public override double Perimeter() => 4 * side;
}

Shape[] shapes = [new Circle(5), new Square(4)];
foreach (var s in shapes)
    Console.WriteLine(s.Describe());
// Circle: Area=78.54, P=31.42
// Square: Area=16.00, P=16.00
```

**When to use each:**
- Use `virtual` when there is a **sensible default** and derived classes may optionally specialise.
- Use `abstract` when **no meaningful default exists** and every subclass must provide its own implementation.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle constructor inheritance in C#, and what is the role of the base keyword in constructor inheritance?

Constructors are **not inherited** in C#. Each class must define its own constructors. However, a derived class constructor can (and often must) call a base class constructor using `base(...)` to initialise inherited members.

**If no `base(...)` is specified**, the compiler automatically calls the base class\'s **parameterless constructor**. If no such constructor exists, it is a compile error.

```cs
public class Vehicle
{
    public string Brand { get; }
    public int Year { get; }

    // No parameterless constructor — derived class MUST call base(...)
    public Vehicle(string brand, int year)
    {
        Brand = brand;
        Year  = year;
    }
}

public class Car : Vehicle
{
    public int Doors { get; }

    // Must chain to Vehicle(string, int) via base(...)
    public Car(string brand, int year, int doors)
        : base(brand, year)             // ← base constructor called first
    {
        Doors = doors;
    }

    // Convenience overload — chains through Car\'s own constructors
    public Car(string brand, int year) : this(brand, year, 4) { }
}

public class ElectricCar : Car
{
    public int RangeKm { get; }

    public ElectricCar(string brand, int year, int doors, int rangeKm)
        : base(brand, year, doors)      // ← calls Car → Vehicle
    {
        RangeKm = rangeKm;
    }
}

var ec = new ElectricCar("Tesla", 2025, 4, 500);
Console.WriteLine($"{ec.Brand} {ec.Year}, {ec.Doors}d, {ec.RangeKm}km");
// Output: Tesla 2025, 4d, 500km
```

**Execution order — always base first:**

```cs
public class A { public A() => Console.WriteLine("A"); }
public class B : A { public B() => Console.WriteLine("B"); }
public class C : B { public C() => Console.WriteLine("C"); }

new C();
// Output:
// A   ← grandparent first
// B
// C   ← derived last
```

**Primary constructors (C# 12) with base:**

```cs
public class Person(string name) { public string Name { get; } = name; }
public class Employee(string name, string dept) : Person(name)
{
    public string Department { get; } = dept;
}

var e = new Employee("Pradeep", "Engineering");
Console.WriteLine($"{e.Name} — {e.Department}");
// Output: Pradeep — Engineering
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between inheritance and composition in C# and when should you use each one?

**Inheritance** models an **"is-a"** relationship: the derived class *is* a specialised version of the base class.

**Composition** models a **"has-a"** relationship: a class *contains* one or more objects of other types to delegate work to them.

| Aspect | Inheritance | Composition |
|--------|-------------|-------------|
| Relationship | "is-a" | "has-a" |
| Coupling | Tight — derived depends on base internals | Loose — depends on a public interface/contract |
| Flexibility | Base changes affect all derived classes | Composed behavior can be swapped at runtime |
| Reuse | Reuse via subclassing | Reuse via delegation |
| Polymorphism | Achieved via `virtual`/`override` | Achieved via interface + injection |
| Preferred when | True specialisation hierarchy | Behaviors that vary independently |

**Inheritance (use when "is-a" is genuine):**

```cs
public class Animal(string name)
{
    public string Name { get; } = name;
    public virtual string Sound() => "...";
}

public class Dog(string name) : Animal(name)       // Dog IS-A Animal ✅
{
    public override string Sound() => "Woof";
}
```

**Composition (prefer for behavior reuse):**

```cs
// Behaviors defined as interfaces
public interface ILogger    { void Log(string msg); }
public interface IEmailSender { void Send(string to, string body); }

// Implementations (can be swapped)
public class ConsoleLogger : ILogger
{
    public void Log(string msg) => Console.WriteLine($"[LOG] {msg}");
}

public class SmtpEmailSender : IEmailSender
{
    public void Send(string to, string body) =>
        Console.WriteLine($"[SMTP] → {to}: {body}");
}

// OrderService HAS-A logger and emailer — not inheriting from them
public class OrderService(ILogger logger, IEmailSender emailSender)
{
    public void PlaceOrder(string item, string customerEmail)
    {
        logger.Log($"Order placed: {item}");
        emailSender.Send(customerEmail, $"Your order for {item} is confirmed!");
    }
}

var service = new OrderService(new ConsoleLogger(), new SmtpEmailSender());
service.PlaceOrder("Laptop", "pradeep@example.com");
// [LOG] Order placed: Laptop
// [SMTP] → pradeep@example.com: Your order for Laptop is confirmed!
```

**Swapping behavior at runtime (composition wins):**

```cs
// For tests — swap to a no-op logger without changing OrderService
public class NullLogger : ILogger { public void Log(string msg) { } }

var testService = new OrderService(new NullLogger(), new SmtpEmailSender());
```

**Rule:** Prefer composition over inheritance (Effective Java principle applies equally in C#). Use inheritance only when the "is-a" relationship is stable and meaningful across the hierarchy.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you override private virtual methods?

**No.** A `private` method is not accessible in derived classes, so it cannot be overridden. The compiler will produce error **CS0621** if you try to mark a `private` method as `virtual`.

```cs
public class Base
{
    // ❌ Compile error CS0621: 'Base.DoWork()' cannot be declared virtual
    // because it is private
    // private virtual void DoWork() { }

    // To allow overriding, minimum access must be protected (or higher):
    protected virtual void DoWork() => Console.WriteLine("Base.DoWork");
}

public class Derived : Base
{
    protected override void DoWork() => Console.WriteLine("Derived.DoWork");
}
```

**Why:** `private` means "visible only in this class". Since derived classes cannot see it, they have no way to provide an overriding implementation. The C# compiler enforces this at compile time.

**What you can do instead — Template Method Pattern:**

If you want a private step to be "customisable" while keeping it hidden, expose a `protected virtual` hook and call it from a private or public method:

```cs
public class DataProcessor
{
    // Public entry point — not overridable
    public void Process(string data)
    {
        Validate(data);
        Transform(data); // calls the protected virtual hook
        Save(data);
    }

    private static void Validate(string data)
    {
        if (string.IsNullOrEmpty(data))
            throw new ArgumentException("Data cannot be empty");
    }

    private static void Save(string data) =>
        Console.WriteLine($"Saved: {data}");

    // Protected virtual — derived classes customise only this step
    protected virtual void Transform(string data) =>
        Console.WriteLine($"Default transform: {data}");
}

public class UpperCaseProcessor : DataProcessor
{
    protected override void Transform(string data) =>
        Console.WriteLine($"Upper transform: {data.ToUpper()}");
}

new UpperCaseProcessor().Process("hello");
// Saved: hello
// Upper transform: HELLO
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What does the keyword virtual mean in the method definition?

The `virtual` keyword marks a method as **overridable** — derived classes may provide their own implementation using the `override` keyword. Without `virtual`, the method is non-virtual and cannot be overridden (only hidden with `new`).

```cs
public class Shape
{
    // virtual — derived classes can replace this implementation
    public virtual double Area() => 0;

    // non-virtual — cannot be overridden, only hidden
    public string TypeName() => "Shape";
}

public class Circle(double radius) : Shape
{
    // override replaces the virtual method for Circle instances
    public override double Area() => Math.PI * radius * radius;
}

Shape s = new Circle(5);
Console.WriteLine(s.Area());     // Output: 78.54... (Circle.Area — override wins)
Console.WriteLine(s.TypeName()); // Output: Shape (non-virtual — base always called)
```

**Key properties of `virtual`:**
- Enables **runtime polymorphism (dynamic dispatch)** — the actual method called is determined at runtime based on the object\'s type, not the reference type.
- A `virtual` method may have a body (default implementation). Derived classes call `base.Method()` to access it.
- Can be sealed in a derived class to prevent further overriding: `public sealed override double Area()`.
- `abstract` methods are implicitly virtual — they also participate in dynamic dispatch but have no body.

**Sealing a virtual override:**

```cs
public class Square(double side) : Shape
{
    // sealed prevents further overriding in classes that inherit from Square
    public sealed override double Area() => side * side;
}

// public class SpecialSquare : Square
// {
//     public override double Area() => 999; // ❌ compile error — sealed
// }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a Virtual Method in C#?

A **virtual method** is a method declared with the `virtual` keyword that allows derived classes to **override** it with their own implementation. The correct version is selected at **runtime** based on the actual object type (dynamic dispatch).

```cs
public class Animal
{
    // Virtual method with a default implementation
    public virtual string MakeSound() => $"{GetType().Name} makes a generic sound";

    // Virtual property
    public virtual string Category => "Unknown";
}

public class Dog : Animal
{
    public override string MakeSound() => "Woof!";
    public override string Category    => "Mammal";
}

public class Eagle : Animal
{
    public override string MakeSound() => "Screech!";
    public override string Category    => "Bird";
}

public class Fish : Animal
{
    // Does NOT override — uses base default
}

// Dynamic dispatch — runtime picks the right MakeSound()
Animal[] animals = [new Dog(), new Eagle(), new Fish()];

foreach (var a in animals)
    Console.WriteLine($"{a.GetType().Name} [{a.Category}]: {a.MakeSound()}");

// Output:
// Dog [Mammal]: Woof!
// Eagle [Bird]: Screech!
// Fish [Unknown]: Fish makes a generic sound
```

**Virtual methods in abstract classes (mix of abstract + virtual):**

```cs
public abstract class Report
{
    // abstract — must be implemented (no default)
    public abstract string GenerateContent();

    // virtual — default header, can be customised
    public virtual string GenerateHeader() => $"=== Report ({DateTime.Today:d}) ===";

    // non-virtual — fixed footer, never changes
    public string GenerateFooter() => "=== End of Report ===";

    public void Print()
    {
        Console.WriteLine(GenerateHeader());
        Console.WriteLine(GenerateContent()); // dynamic dispatch
        Console.WriteLine(GenerateFooter());
    }
}

public class SalesReport : Report
{
    public override string GenerateContent() => "Sales: £50,000 this month";
    // Uses default header (virtual, not overridden)
}

public class AnnualReport : Report
{
    public override string GenerateContent() => "Annual Revenue: £600,000";
    public override string GenerateHeader()  => "=== Annual Report 2025 ===";
}

new SalesReport().Print();
// === Report (19/04/2026) ===
// Sales: £50,000 this month
// === End of Report ===

new AnnualReport().Print();
// === Annual Report 2025 ===
// Annual Revenue: £600,000
// === End of Report ===
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Why a private virtual method cannot be overridden in C#?

A `private virtual` method is a **contradiction** — and the C# compiler rejects it with **CS0621**.

**Two reasons:**

**1. Accessibility:** `private` means the method is visible **only within the declaring class**. Derived classes cannot see private members — they cannot reference them, let alone override them.

**2. Virtual dispatch requires visibility:** For `override` to work, the derived class must be able to see the method signature and declare it with `override`. Since `private` blocks that visibility, override is impossible.

```cs
public class Base
{
    // ❌ CS0621 — cannot be both private and virtual
    // private virtual void Compute() { }

    // ✅ To be overridable, minimum access is protected
    protected virtual void Compute() => Console.WriteLine("Base.Compute");

    // ✅ Private method can be called via a protected/public virtual hook
    private void InternalWork() => Console.WriteLine("Internal work");

    protected virtual void DoWork()
    {
        InternalWork(); // private helper — called from overridable method
        Compute();
    }
}

public class Derived : Base
{
    protected override void Compute() => Console.WriteLine("Derived.Compute");
    // Cannot access InternalWork — it\'s private to Base
}
```

**Summary of which access modifiers can be combined with `virtual`:**

| Access modifier | Can be `virtual`? |
|---|---|
| `private` | ❌ No (CS0621) |
| `protected` | ✅ Yes |
| `internal` | ✅ Yes |
| `protected internal` | ✅ Yes |
| `public` | ✅ Yes |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you override a method in C#?

To override a method:

1. The base class method must be marked `virtual`, `abstract`, or `override`.
2. The derived class method must use the `override` keyword.
3. The signature (name, parameter types, return type) must match exactly.

```cs
public class Animal
{
    public virtual string Speak() => "...";               // virtual — can be overridden
    public virtual string Describe() => $"I am an Animal";
}

public class Dog : Animal
{
    public override string Speak() => "Woof!";             // overrides Animal.Speak
    // Describe() not overridden — Dog uses Animal\'s version
}

public class GoldenRetriever : Dog
{
    // Override again — must still match signature
    public override string Speak() => "Woof Woof!";
    public override string Describe() => "I am a Golden Retriever";
}

Animal a = new GoldenRetriever();
Console.WriteLine(a.Speak());    // Output: Woof Woof!   (runtime type decides)
Console.WriteLine(a.Describe()); // Output: I am a Golden Retriever
```

**Calling the base implementation from an override:**

```cs
public class TimestampLogger
{
    public virtual void Log(string message) =>
        Console.WriteLine($"[LOG] {message}");
}

public class PrefixLogger : TimestampLogger
{
    public override void Log(string message)
    {
        base.Log(message);             // call base first
        Console.WriteLine($"  at {DateTime.UtcNow:HH:mm:ss UTC}");
    }
}

new PrefixLogger().Log("App started");
// [LOG] App started
//   at 14:32:01 UTC
```

**Preventing further override with `sealed override`:**

```cs
public class SpecialDog : Dog
{
    // sealed — no class deriving from SpecialDog can override Speak again
    public sealed override string Speak() => "Bark Bark!";
}
```

**Abstract method override (mandatory):**

```cs
public abstract class Shape
{
    public abstract double Area(); // no body — must be overridden
}

public class Circle(double r) : Shape
{
    public override double Area() => Math.PI * r * r; // required
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `override` and `new` keywords in C#?

Both `override` and `new` let a derived class define a method with the same name as a base class method, but they have fundamentally different behavior:

| Aspect | `override` | `new` (hiding) |
|--------|-----------|----------------|
| Base requirement | Base method must be `virtual`/`abstract`/`override` | Any base method |
| Polymorphism | ✅ Yes — runtime type decides | ❌ No — reference type decides |
| Dispatch | Dynamic (runtime) | Static (compile-time) |
| LSP compliance | ✅ | ❌ (often breaks it) |
| Compiler warning if omitted | Error | Warning CS0108 (shadowing) |

```cs
public class Animal
{
    public virtual string Sound()   => "...";    // virtual
    public string Category() => "Animal";         // non-virtual
}

public class Dog : Animal
{
    public override string Sound()   => "Woof";  // polymorphic
    public new string Category() => "Dog";        // hiding
}

// ---- Test via base reference ----
Animal a = new Dog();

// override: runtime type (Dog) decides
Console.WriteLine(a.Sound());    // Output: Woof   ← Dog.Sound

// new (hiding): reference type (Animal) decides
Console.WriteLine(a.Category()); // Output: Animal ← Animal.Category (NOT Dog!)

// ---- Test via derived reference ----
Dog d = new Dog();
Console.WriteLine(d.Sound());    // Output: Woof
Console.WriteLine(d.Category()); // Output: Dog
```

**Practical impact — hiding breaks polymorphism:**

```cs
void Describe(Animal a)
{
    Console.WriteLine($"Sound: {a.Sound()}, Category: {a.Category()}");
}

Describe(new Dog());
// Sound: Woof       ← override works correctly (Dog\'s Sound)
// Category: Animal  ← hiding is wrong here (expected "Dog", got "Animal")
```

**Rule:** Use `override` for polymorphic behavior. Use `new` only for intentional version breaking (e.g., a base class added a method that conflicts with an existing derived method) — and document it clearly.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does inheritance promote code reusability?

Inheritance promotes reusability by allowing derived classes to **inherit and reuse** all non-private members from the base class without rewriting them — while adding or customising only what differs.

**1. Shared implementation — write once, reuse many times:**

```cs
public class Vehicle
{
    public string Brand { get; }
    public int Year { get; }
    public string Registration { get; }

    public Vehicle(string brand, int year, string reg)
    {
        Brand = brand;
        Year  = year;
        Registration = reg;
    }

    // Shared behavior — ALL vehicles log the same way
    public void LogUsage(string action) =>
        Console.WriteLine($"[{Registration}] {Brand} ({Year}): {action}");

    public virtual string FuelType => "Unknown";
}

public class Car(string brand, int year, string reg) : Vehicle(brand, year, reg)
{
    public override string FuelType => "Petrol";
    public int Doors { get; init; } = 4;
}

public class ElectricCar(string brand, int year, string reg) : Vehicle(brand, year, reg)
{
    public override string FuelType => "Electric";
    public int RangeKm { get; init; }
}

// Both reuse LogUsage, Brand, Year, Registration without rewriting
var car = new Car("BMW", 2023, "AB12 CDE") { Doors = 2 };
var ev  = new ElectricCar("Tesla", 2025, "EV99 XYZ") { RangeKm = 500 };

car.LogUsage("started"); // [AB12 CDE] BMW (2023): started
ev.LogUsage("charged");  // [EV99 XYZ] Tesla (2025): charged

Console.WriteLine(car.FuelType); // Petrol
Console.WriteLine(ev.FuelType);  // Electric
```

**2. Hierarchical reuse — build progressively:**

```cs
public class Employee(string name, decimal salary)
{
    public string Name { get; } = name;
    protected decimal Salary { get; set; } = salary;

    public virtual decimal CalculatePay() => Salary;
    public override string ToString() => $"{Name}: {CalculatePay():C}";
}

public class Manager(string name, decimal salary, decimal bonus)
    : Employee(name, salary)
{
    private readonly decimal _bonus = bonus;
    public override decimal CalculatePay() => base.CalculatePay() + _bonus;
}

public class ContractEmployee(string name, decimal hourlyRate, int hours)
    : Employee(name, hourlyRate * hours)
{
    // CalculatePay() reused from Employee unchanged
}

Employee[] staff =
[
    new Employee("Alice", 4000m),
    new Manager("Bob", 4000m, 1000m),
    new ContractEmployee("Carol", 50m, 100),
];

foreach (var e in staff)
    Console.WriteLine(e); // uses reused ToString + polymorphic CalculatePay
// Alice: £4,000.00
// Bob: £5,000.00
// Carol: £5,000.00
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the potential pitfalls of using inheritance?

Inheritance is powerful but easily misused. Common pitfalls:

**1. Fragile base class problem — base changes break derived classes:**

```cs
public class Collection
{
    private int _addCount = 0;

    public virtual void Add(string item) { _addCount++; /* logic */ }

    // Adding a new virtual method that calls Add internally
    public virtual void AddAll(string[] items)
    {
        foreach (var item in items) Add(item); // calls virtual Add
    }
}

public class InstrumentedCollection : Collection
{
    private int _count = 0;
    public override void Add(string item)    { _count++; base.Add(item); }
    public override void AddAll(string[] items) { _count += items.Length; base.AddAll(items); }
    // AddAll calls base.AddAll which calls virtual Add → _count incremented TWICE per item!
}
```

**2. Tight coupling — derived depends on base internals:**

```cs
// If Base changes internal logic, Derived silently breaks
public class Base
{
    protected int _value;
    public virtual void Set(int v) { _value = v * 2; } // secret: doubles internally
}

public class Derived : Base
{
    public override void Set(int v) { base.Set(v); Console.WriteLine(_value); }
    // Derived assumes _value == v, but gets v*2 — unexpected dependency
}
```

**3. Violation of Liskov Substitution Principle (LSP):**

```cs
public class Rectangle
{
    public virtual int Width  { get; set; }
    public virtual int Height { get; set; }
    public int Area() => Width * Height;
}

public class Square : Rectangle // ❌ Square IS-NOT substitutable for Rectangle
{
    public override int Width  { set { base.Width  = value; base.Height = value; } }
    public override int Height { set { base.Height = value; base.Width  = value; } }
}

Rectangle r = new Square();
r.Width = 4; r.Height = 5;
Console.WriteLine(r.Area()); // Expected 20, got 25 — LSP violated!
```

**4. Deep inheritance hierarchies — hard to understand and maintain:**

```cs
// 6-level hierarchy — changing Animal ripples through everything
Animal → Vertebrate → Mammal → Carnivore → Feline → Cat
```

**5. Inheritance for code reuse only (not "is-a"):**

```cs
// ❌ Stack should NOT inherit List just to reuse its storage
public class Stack<T> : List<T> // exposes Add, Remove, Insert — breaks Stack semantics
```

**Mitigations:**
- Prefer **composition over inheritance** for behavior reuse.
- Follow **LSP** — a derived class must be fully substitutable for its base.
- Seal classes or methods when the hierarchy is intended to be closed.
- Keep hierarchies shallow (2-3 levels max).
- Program to interfaces, not concrete base classes.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you prevent a class from being inherited in C#?

Apply the `sealed` keyword to the class. A sealed class cannot be used as a base class — any attempt to inherit from it is a **compile error (CS0509)**.

```cs
public sealed class DatabaseConnection
{
    private readonly string _connectionString;

    public DatabaseConnection(string connectionString)
    {
        if (string.IsNullOrWhiteSpace(connectionString))
            throw new ArgumentException("Connection string required");
        _connectionString = connectionString;
    }

    public void Open()  => Console.WriteLine($"Opening: {_connectionString}");
    public void Close() => Console.WriteLine("Connection closed");
}

// ❌ CS0509: 'MyConnection' cannot derive from sealed type 'DatabaseConnection'
// public class MyConnection : DatabaseConnection { }
```

**Sealing a specific override (not the whole class):**

```cs
public class Animal
{
    public virtual string Sound() => "...";
}

public class Dog : Animal
{
    // sealed on the override — Dog can be inherited, but Sound() cannot be overridden further
    public sealed override string Sound() => "Woof";
}

public class GoldenRetriever : Dog
{
    // ❌ CS0239: cannot override inherited member 'Dog.Sound()' because it is sealed
    // public override string Sound() => "Woof Woof";
}
```

**Why seal a class?**
- Security — prevent subclasses from altering security-critical behaviour.
- Performance — the JIT compiler can optimise sealed class method calls (devirtualisation).
- Design intent — signal that the type is complete and not designed for extension.
- Immutability — `record` types in C# seal `==` and `Equals` by design; adding `sealed` to a record prevents positional cloning surprises.

**Common examples of sealed classes in .NET:**
- `System.String` — `sealed` to protect immutability guarantees.
- `System.Int32`, `System.Boolean` — value types are implicitly sealed.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 5. COLLECTIONS

<br/>

## Q. What are collections in C# and why are they used?

Collections are data structures that store, manage, and manipulate groups of related objects. Unlike arrays (fixed size), most .NET collections resize dynamically and provide rich APIs for searching, sorting, filtering, and thread-safe access.

**Main collection namespaces:**
- `System.Collections.Generic` — strongly typed, preferred in modern .NET
- `System.Collections.Concurrent` — thread-safe collections
- `System.Collections.Immutable` — immutable collections (.NET 5+)
- `System.Collections.Frozen` — read-optimized frozen sets/dictionaries (.NET 8+)

**Commonly used collections:**

| Collection               | Key Feature                                 |
|--------------------------|---------------------------------------------|
| `List<T>`                | Ordered, resizable, index access            |
| `Dictionary<K,V>`        | Key-value pairs, O(1) lookup                |
| `HashSet<T>`             | Unique elements, fast membership check      |
| `Queue<T>`               | FIFO order                                  |
| `Stack<T>`               | LIFO order                                  |
| `LinkedList<T>`          | Doubly linked, efficient insert/remove      |
| `SortedDictionary<K,V>`  | Key-sorted dictionary                       |
| `ConcurrentDictionary<K,V>` | Thread-safe key-value                   |
| `ImmutableList<T>`       | Immutable, safe to share across threads     |
| `FrozenDictionary<K,V>`  | Optimized read-only dictionary (.NET 8+)    |

**Example — List<T>:**

```cs
var names = new List<string> { "Alice", "Bob", "Carol" };
names.Add("Dave");
names.Remove("Bob");

foreach (var name in names)
    Console.WriteLine(name);
// Output: Alice Carol Dave
```

**Example — Dictionary<K,V>:**

```cs
var scores = new Dictionary<string, int>
{
    ["Alice"] = 95,
    ["Bob"]   = 87,
};

scores["Carol"] = 92;

if (scores.TryGetValue("Alice", out int score))
    Console.WriteLine($"Alice: {score}"); // Alice: 95
```

**Example — FrozenDictionary (.NET 8+) for read-heavy scenarios:**

```cs
using System.Collections.Frozen;

var lookup = new Dictionary<string, int>
{
    ["red"]   = 0xFF0000,
    ["green"] = 0x00FF00,
    ["blue"]  = 0x0000FF,
}.ToFrozenDictionary();

Console.WriteLine(lookup["red"].ToString("X")); // FF0000
```

**Example — Collection expressions (C# 12):**

```cs
List<int> numbers = [1, 2, 3, 4, 5];
int[] arr = [.. numbers, 6, 7];  // spread operator
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between an array and a collection in C#?

| Feature | Array (`T[]`) | Collection (e.g., `List<T>`) |
|---------|--------------|------------------------------|
| Size | Fixed at creation | Dynamic (grows/shrinks) |
| Type safety | Always typed (`int[]`) | Generic collections are typed |
| Performance | Fastest element access (O(1)) | Slightly more overhead |
| Insertion/deletion | Not supported (fixed) | Efficient (`Add`, `Remove`) |
| Interface | `IEnumerable`, `IList` | Richer API (LINQ, sort, search) |
| Null safety | Length is always known | `Count` property |

```cs
// Array — fixed size, fast direct access
int[] scores = [10, 20, 30, 40, 50];
Console.WriteLine(scores[2]); // 30
// scores.Add(60); // ❌ arrays have no Add — fixed size

// List<T> — dynamic size, full API
var names = new List<string> { "Alice", "Bob" };
names.Add("Carol");
names.Remove("Bob");
Console.WriteLine(names.Count); // 2

// Modern collection expressions (C# 12)
List<int> nums = [1, 2, 3];
int[] arr     = [.. nums, 4, 5]; // spread into array
```

**When to use arrays:**
- Size is known upfront and fixed.
- Maximum performance for index access.
- Interop with native or unsafe code.

**When to use collections:**
- Size is unknown or changes at runtime.
- You need built-in searching, sorting, filtering, or thread-safety.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the different types of collections available in C#?

.NET provides collections across several namespaces:

**`System.Collections.Generic` (strongly typed — preferred):**

| Type | Description |
|------|-------------|
| `List<T>` | Ordered, resizable, index access |
| `Dictionary<K,V>` | Key-value, O(1) lookup |
| `HashSet<T>` | Unique elements, fast membership |
| `Queue<T>` | FIFO |
| `Stack<T>` | LIFO |
| `LinkedList<T>` | Doubly linked list |
| `SortedList<K,V>` | Key-sorted, backed by array |
| `SortedDictionary<K,V>` | Key-sorted, backed by BST |
| `SortedSet<T>` | Unique sorted elements |

**`System.Collections.Concurrent` (thread-safe):**

| Type | Description |
|------|-------------|
| `ConcurrentDictionary<K,V>` | Thread-safe dictionary |
| `ConcurrentQueue<T>` | Thread-safe FIFO |
| `ConcurrentStack<T>` | Thread-safe LIFO |
| `ConcurrentBag<T>` | Unordered, thread-safe bag |
| `BlockingCollection<T>` | Bounded producer-consumer |

**`System.Collections.Immutable` (.NET 5+):**

`ImmutableList<T>`, `ImmutableDictionary<K,V>`, `ImmutableArray<T>`, etc.

**`System.Collections.Frozen` (.NET 8+):**

`FrozenDictionary<K,V>`, `FrozenSet<T>` — read-optimized, ideal for static lookup tables.

```cs
// Generic
var list  = new List<int> { 1, 2, 3 };
var dict  = new Dictionary<string, int> { ["a"] = 1 };
var set   = new HashSet<string> { "x", "y", "z" };

// Frozen (read-only optimized — .NET 8+)
using System.Collections.Frozen;
var frozen = new Dictionary<string, int> { ["one"] = 1, ["two"] = 2 }
    .ToFrozenDictionary();
Console.WriteLine(frozen["one"]); // 1

// Immutable
using System.Collections.Immutable;
var immList = ImmutableList.Create(1, 2, 3);
var added   = immList.Add(4); // returns NEW list
Console.WriteLine(immList.Count); // 3 (original unchanged)
Console.WriteLine(added.Count);   // 4
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are Concurrent Collection Classes?

Concurrent collection classes in the `System.Collections.Concurrent` namespace are **thread-safe** without requiring external locks. They use fine-grained locking or lock-free algorithms for better performance in multi-threaded scenarios.

| Class | Description | Thread-safe operation |
|-------|-------------|----------------------|
| `ConcurrentDictionary<K,V>` | Thread-safe key-value store | `TryAdd`, `TryUpdate`, `GetOrAdd` |
| `ConcurrentQueue<T>` | Thread-safe FIFO | `Enqueue`, `TryDequeue` |
| `ConcurrentStack<T>` | Thread-safe LIFO | `Push`, `TryPop` |
| `ConcurrentBag<T>` | Unordered thread-safe collection | `Add`, `TryTake` |
| `BlockingCollection<T>` | Bounded producer-consumer | `Add`, `Take` (blocks when empty/full) |

```cs
// ConcurrentDictionary — safe multi-threaded add/update
var cache = new ConcurrentDictionary<string, int>();

// Multiple threads can add/read simultaneously
await Task.WhenAll(Enumerable.Range(0, 10).Select(i =>
    Task.Run(() => cache.TryAdd($"key{i}", i))));

Console.WriteLine(cache.Count); // 10 (always correct, no race conditions)

// GetOrAdd — atomic: get existing or add new
int val = cache.GetOrAdd("key5", k => 99);
Console.WriteLine(val); // 5 (already existed)

// AddOrUpdate — atomic increment
cache.AddOrUpdate("counter", 1, (_, existing) => existing + 1);

// ConcurrentQueue — thread-safe producer/consumer
var queue = new ConcurrentQueue<string>();
queue.Enqueue("task1");
queue.Enqueue("task2");

if (queue.TryDequeue(out string? item))
    Console.WriteLine(item); // task1

// BlockingCollection — bounded buffer (blocks producer when full)
var buffer = new BlockingCollection<int>(boundedCapacity: 5);

var producer = Task.Run(() =>
{
    for (int i = 0; i < 10; i++)
    {
        buffer.Add(i); // blocks when buffer is full
        Console.WriteLine($"Produced: {i}");
    }
    buffer.CompleteAdding();
});

var consumer = Task.Run(() =>
{
    foreach (var n in buffer.GetConsumingEnumerable())
        Console.WriteLine($"Consumed: {n}");
});

await Task.WhenAll(producer, consumer);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain Hashtable in C#?

`Hashtable` is a **non-generic** key-value collection in `System.Collections` that stores objects as `object`/`object` pairs. It was the primary associative collection before generics were introduced in .NET 2.0. In modern .NET, `Dictionary<K,V>` is always preferred.

```cs
using System.Collections;

var table = new Hashtable();
table["name"]    = "Pradeep";
table["age"]     = 30;
table["active"]  = true;

Console.WriteLine(table["name"]); // Pradeep

// Iteration — key order is not guaranteed
foreach (DictionaryEntry entry in table)
    Console.WriteLine($"{entry.Key}: {entry.Value}");

// Check existence
Console.WriteLine(table.ContainsKey("age"));   // True
Console.WriteLine(table.ContainsValue(30));    // True

table.Remove("active");
Console.WriteLine(table.Count); // 2
```

**Why `Dictionary<K,V>` is better than `Hashtable`:**

| Feature | `Hashtable` | `Dictionary<K,V>` |
|---------|------------|-------------------|
| Type safety | ❌ `object` — boxing/unboxing | ✅ Strongly typed |
| Performance | Slower (boxing overhead) | Faster (no boxing for value types) |
| Null keys | ❌ Not allowed | ❌ Not allowed (same) |
| Thread safety | Thread-safe for reads only | Use `ConcurrentDictionary` for writes |
| Recommended | Legacy code only | ✅ Always prefer |

```cs
// Modern equivalent — Dictionary<K,V>
var dict = new Dictionary<string, object?>
{
    ["name"]   = "Pradeep",
    ["age"]    = 30,
    ["active"] = true,
};

if (dict.TryGetValue("age", out object? age))
    Console.WriteLine((int)age); // 30
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is IEnumerable<> in C#?

`IEnumerable<T>` is the fundamental interface for all enumerable sequences in .NET. It exposes a single method `GetEnumerator()` that allows iterating through a collection with `foreach`.

```cs
// IEnumerable<T> definition (simplified)
public interface IEnumerable<out T> : IEnumerable
{
    IEnumerator<T> GetEnumerator();
}
```

**Key characteristics:**
- **Forward-only, read-only** — no index access, no mutation.
- **Deferred execution** — LINQ queries on `IEnumerable<T>` are not evaluated until iterated.
- All .NET collections implement it (`List<T>`, `Array`, `Dictionary<K,V>`, etc.).
- `yield return` creates custom `IEnumerable<T>` iterators.

```cs
// Any type implementing IEnumerable<T> works with foreach + LINQ
IEnumerable<int> numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

var evens = numbers
    .Where(n => n % 2 == 0)   // deferred — not executed yet
    .Select(n => n * n);       // deferred — not executed yet

foreach (var n in evens)       // execution happens here
    Console.Write($"{n} ");    // 4 16 36 64 100

// Custom iterator with yield return
IEnumerable<int> FibonacciSequence(int count)
{
    int a = 0, b = 1;
    for (int i = 0; i < count; i++)
    {
        yield return a;
        (a, b) = (b, a + b);
    }
}

foreach (var f in FibonacciSequence(8))
    Console.Write($"{f} "); // 0 1 1 2 3 5 8 13

// IAsyncEnumerable<T> — async streaming (C# 8+)
async IAsyncEnumerable<int> GetDataAsync()
{
    for (int i = 0; i < 5; i++)
    {
        await Task.Delay(10); // simulate async I/O
        yield return i;
    }
}

await foreach (var item in GetDataAsync())
    Console.Write($"{item} "); // 0 1 2 3 4
```

**When to use `IEnumerable<T>` as a parameter/return type:**
- Return it when you want to expose a lazy/streaming sequence.
- Accept it as a parameter when you only need to iterate (most flexible).
- Return `IReadOnlyList<T>` or `IReadOnlyCollection<T>` when callers need `Count` or index access.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between a BlockingCollection and a ConcurrentQueue or ConcurrentStack? In which scenarios would you choose to use the BlockingCollection, and why?

| Feature | `ConcurrentQueue<T>` / `ConcurrentStack<T>` | `BlockingCollection<T>` |
|---------|----------------------------------------------|------------------------|
| Blocking | ❌ Non-blocking (`TryDequeue` returns false) | ✅ Blocks consumer until item available |
| Bounded capacity | ❌ Unlimited | ✅ Optional upper bound |
| Completion signal | ❌ No | ✅ `CompleteAdding()` signals end-of-stream |
| Ordering | Queue=FIFO, Stack=LIFO | Wraps any `IProducerConsumerCollection<T>` |
| Use case | Fire-and-forget, polling | Classic producer-consumer pipelines |

**`ConcurrentQueue` — non-blocking, just thread-safe:**

```cs
var queue = new ConcurrentQueue<int>();
queue.Enqueue(1);

// Returns false immediately if empty — caller must handle
if (!queue.TryDequeue(out int item))
    Console.WriteLine("Queue empty — try again later");
```

**`BlockingCollection` — blocks consumer, supports bounded buffer and completion:**

```cs
// Bounded buffer: producer blocks when capacity reached
var pipeline = new BlockingCollection<string>(boundedCapacity: 3);

var producer = Task.Run(async () =>
{
    string[] items = ["A", "B", "C", "D", "E"];
    foreach (var item in items)
    {
        pipeline.Add(item); // blocks if buffer is full
        Console.WriteLine($"Produced: {item}");
        await Task.Delay(50);
    }
    pipeline.CompleteAdding(); // signals no more items
});

var consumer = Task.Run(() =>
{
    // GetConsumingEnumerable blocks when empty, exits when CompleteAdding() called
    foreach (var item in pipeline.GetConsumingEnumerable())
        Console.WriteLine($"Consumed: {item}");
});

await Task.WhenAll(producer, consumer);
```

**`BlockingCollection` wrapping a `ConcurrentStack` (LIFO behavior):**

```cs
// Default is ConcurrentQueue (FIFO). Override with ConcurrentStack for LIFO:
var lifoCollection = new BlockingCollection<int>(new ConcurrentStack<int>(), boundedCapacity: 10);
```

**Choose `BlockingCollection` when:**
- You need a classic **bounded producer-consumer** pattern.
- Consumers should **block** rather than poll.
- You need a **completion signal** (`CompleteAdding`).
- You want to swap the underlying data structure (FIFO/LIFO/Bag).

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain the difference between IQueryable, ICollection, IList & IDictionary interfaces?

| Interface | Namespace | Key purpose | Adds over parent |
|-----------|-----------|-------------|-----------------|
| `IEnumerable<T>` | `System.Collections.Generic` | Forward-only iteration | (base) |
| `ICollection<T>` | `System.Collections.Generic` | Count + Add/Remove | `Count`, `Add`, `Remove`, `Contains` |
| `IList<T>` | `System.Collections.Generic` | Index access | `this[index]`, `Insert`, `RemoveAt` |
| `IDictionary<K,V>` | `System.Collections.Generic` | Key-value mapping | `this[key]`, `Keys`, `Values`, `TryGetValue` |
| `IQueryable<T>` | `System.Linq` | Remote/deferred queries | `Expression`, `Provider` — translates to SQL/etc. |

```cs
// ICollection<T> — knows its count, can add/remove
ICollection<string> col = new List<string> { "a", "b" };
col.Add("c");
Console.WriteLine(col.Count); // 3

// IList<T> — index access + ordered insertion
IList<int> list = new List<int> { 10, 20, 30 };
Console.WriteLine(list[1]); // 20
list.Insert(1, 15);         // [10, 15, 20, 30]
list.RemoveAt(0);            // [15, 20, 30]

// IDictionary<K,V> — key-value access
IDictionary<string, int> dict = new Dictionary<string, int>
{
    ["apple"] = 3,
    ["banana"] = 5,
};
dict["cherry"] = 2;
Console.WriteLine(dict["banana"]); // 5

if (dict.TryGetValue("apple", out int count))
    Console.WriteLine($"apple: {count}"); // apple: 3

// IQueryable<T> — translates to SQL via EF Core
// (requires a DbContext)
// IQueryable<Product> query = dbContext.Products
//     .Where(p => p.Price > 100)   // translated to SQL WHERE clause
//     .OrderBy(p => p.Name);       // translated to SQL ORDER BY
// var results = await query.ToListAsync(); // SQL executes here
```

**`IQueryable<T>` vs `IEnumerable<T>` key distinction:**
- `IEnumerable<T>`: executes in-memory (C# code runs).
- `IQueryable<T>`: translates expression tree to the remote query language (SQL, OData) — filtering happens at the database, not in memory.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is difference between Hashtable and Dictionary?

| Feature | `Hashtable` | `Dictionary<K,V>` |
|---------|------------|-------------------|
| Namespace | `System.Collections` | `System.Collections.Generic` |
| Type safety | ❌ Non-generic (`object`) | ✅ Generic — strongly typed |
| Performance | Slower (boxing for value types) | Faster (no boxing) |
| Null key | ❌ Not allowed | ❌ Not allowed |
| Null value | ✅ Allowed | ✅ Allowed |
| Thread safety | Thread-safe for multiple readers | Not thread-safe (use `ConcurrentDictionary`) |
| Ordering | Not guaranteed | Not guaranteed (insertion order in .NET 5+) |
| Introduced | .NET 1.0 | .NET 2.0 (generics era) |

```cs
// Hashtable — non-generic, stores object/object
var ht = new Hashtable();
ht["name"] = "Pradeep"; // boxing if value type
ht[1]      = 42;
Console.WriteLine((string)ht["name"]!); // requires cast

// Dictionary<K,V> — generic, type-safe, faster
var dict = new Dictionary<string, int>
{
    ["apples"]  = 5,
    ["bananas"] = 3,
};

dict["cherries"] = 8;

if (dict.TryGetValue("apples", out int qty))
    Console.WriteLine($"Apples: {qty}"); // Apples: 5

// Iterate
foreach (var (key, value) in dict)
    Console.WriteLine($"{key}: {value}");

// Thread-safe alternative
var safe = new System.Collections.Concurrent.ConcurrentDictionary<string, int>();
safe.TryAdd("x", 1);
```

**Rule:** Always use `Dictionary<K,V>` in new code. Use `Hashtable` only when maintaining legacy code.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is difference between SortedList and SortedDictionary in C#?

Both maintain key-value pairs **sorted by key**, but differ in their internal data structure and performance characteristics.

| Feature | `SortedList<K,V>` | `SortedDictionary<K,V>` |
|---------|-------------------|------------------------|
| Internal structure | Two parallel arrays (keys + values) | Red-black BST |
| Memory | Lower (arrays are compact) | Higher (BST nodes have overhead) |
| Lookup by index | ✅ `Keys[i]`, `Values[i]` | ❌ Not supported |
| Insert / Remove | O(n) — shifts array | O(log n) — BST rebalance |
| Lookup by key | O(log n) binary search | O(log n) BST traversal |
| Best for | Read-heavy, sorted enumeration | Frequent insert/delete |

```cs
// SortedList<K,V> — array-backed, index access
var sortedList = new SortedList<string, int>
{
    ["banana"] = 3,
    ["apple"]  = 5,
    ["cherry"] = 2,
};

// Keys are always sorted
foreach (var (k, v) in sortedList)
    Console.WriteLine($"{k}: {v}");
// apple: 5
// banana: 3
// cherry: 2

// Index access (unique to SortedList)
Console.WriteLine(sortedList.Keys[0]);   // apple
Console.WriteLine(sortedList.Values[0]); // 5

// SortedDictionary<K,V> — BST-backed, faster insert/delete
var sortedDict = new SortedDictionary<string, int>
{
    ["banana"] = 3,
    ["apple"]  = 5,
    ["cherry"] = 2,
};

sortedDict.Add("date", 7); // O(log n) — faster than SortedList for frequent inserts

foreach (var (k, v) in sortedDict)
    Console.WriteLine($"{k}: {v}");
// apple: 5, banana: 3, cherry: 2, date: 7 (sorted)
```

**Decision guide:**
- Many reads, few insertions, need index access → `SortedList<K,V>`
- Frequent insertions/deletions → `SortedDictionary<K,V>`

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between Array and ArrayList?

| Feature | `Array` (`T[]`) | `ArrayList` |
|---------|----------------|-------------|
| Type safety | ✅ Strongly typed | ❌ Stores `object` — no type safety |
| Size | Fixed | Dynamic |
| Performance | Fast — no boxing for value types | Slower — boxing/unboxing for value types |
| Namespace | Built-in | `System.Collections` |
| Generics | N/A | Non-generic — superseded by `List<T>` |
| LINQ support | ✅ | ✅ (via cast to `IEnumerable`) |

```cs
// Array — fixed size, typed
int[] arr = new int[3] { 1, 2, 3 };
arr[0] = 10;
Console.WriteLine(arr.Length); // 3
// arr[3] = 4; // ❌ IndexOutOfRangeException

// ArrayList — dynamic, but loses type safety
var al = new System.Collections.ArrayList();
al.Add(1);        // boxing int → object
al.Add("hello");  // mixes types — no compile error!
al.Add(3.14);

foreach (object item in al)
    Console.WriteLine(item); // 1 / hello / 3.14

// ❌ Runtime error possible:
// int x = (int)al[1]; // InvalidCastException — "hello" is not int

// ✅ Modern replacement — List<T>
var list = new List<int> { 1, 2, 3 };
list.Add(4);
Console.WriteLine(list.Count); // 4
```

**Rule:** Never use `ArrayList` in new code. Use `List<T>` — it is type-safe, faster (no boxing), and has a richer API.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Whose performance is better array or arraylist?

**Array (`T[]`) is significantly faster than `ArrayList`** for value types because:

1. **No boxing** — arrays store value types directly; `ArrayList` boxes every value type into `object` (heap allocation per element).
2. **No casting** — array element access is typed; `ArrayList` requires a cast on retrieval.
3. **Better cache locality** — typed arrays are contiguous memory; `ArrayList` elements are object references scattered on the heap.

```cs
using System.Diagnostics;

const int N = 1_000_000;

// Array benchmark
var sw = Stopwatch.StartNew();
int[] intArray = new int[N];
for (int i = 0; i < N; i++) intArray[i] = i;
long sum1 = 0;
foreach (int x in intArray) sum1 += x;
sw.Stop();
Console.WriteLine($"Array:     {sw.ElapsedMilliseconds}ms, Sum={sum1}");

// ArrayList benchmark
sw.Restart();
var al = new System.Collections.ArrayList(N);
for (int i = 0; i < N; i++) al.Add(i);         // boxing!
long sum2 = 0;
foreach (object x in al) sum2 += (int)x;        // unboxing!
sw.Stop();
Console.WriteLine($"ArrayList: {sw.ElapsedMilliseconds}ms, Sum={sum2}");

// List<int> — same speed as array (no boxing)
sw.Restart();
var list = new List<int>(N);
for (int i = 0; i < N; i++) list.Add(i);
long sum3 = 0;
foreach (int x in list) sum3 += x;
sw.Stop();
Console.WriteLine($"List<int>: {sw.ElapsedMilliseconds}ms, Sum={sum3}");
// Typical: Array ≈ List<int> >> ArrayList
```

**Performance ranking (value types):** `T[]` ≈ `List<T>` >> `ArrayList`

For **reference types** (classes), the boxing penalty disappears, so the gap narrows — but `List<T>` is still preferred for type safety.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What class is underneath the Sorted List class?

`SortedList<K,V>` is backed internally by **two parallel arrays**: one for keys and one for values. It is not backed by a separate public class — it manages the arrays directly.

The internal implementation uses:
- `TKey[] keys` — sorted array of keys (binary search for lookups).
- `TValue[] values` — parallel array of corresponding values.

```cs
var sl = new SortedList<string, int>
{
    ["banana"] = 2,
    ["apple"]  = 1,
    ["cherry"] = 3,
};

// Internally, after insertion:
// keys:   ["apple", "banana", "cherry"]   ← sorted array
// values: [1,       2,        3]           ← parallel array

// You can directly access the underlying key/value collections
IList<string> keys   = sl.Keys;    // IList<TKey> view of the key array
IList<int>    values = sl.Values;  // IList<TValue> view of the value array

Console.WriteLine(sl.Keys[0]);   // apple   (index access — unique to SortedList)
Console.WriteLine(sl.Values[0]); // 1

// Index of a key
int idx = sl.IndexOfKey("banana"); // 1 (binary search on the key array)
Console.WriteLine(sl.Keys[idx]);   // banana
```

**Consequence of the array backing:**
- Lookup: O(log n) binary search.
- Insert/Remove: O(n) — elements must be shifted.
- Memory: efficient, compact (no BST node overhead).

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. IEnumerable vs List - What to Use? How do they work?

**`IEnumerable<T>`** — the minimal interface for any forward-only sequence. Provides `GetEnumerator()` only. May be lazy (deferred execution).

**`List<T>`** — a concrete, in-memory, ordered, resizable collection implementing `IList<T>`, `ICollection<T>`, and `IEnumerable<T>`. All items are materialised in memory immediately.

| Aspect | `IEnumerable<T>` | `List<T>` |
|--------|-----------------|-----------|
| Memory | Lazy — items produced on demand | Eager — all items in memory |
| Execution | Deferred (LINQ queries) | Immediate |
| `Count` / `Length` | ❌ Not available (enumerate to count) | ✅ O(1) `.Count` |
| Index access | ❌ | ✅ `list[i]` |
| Mutation | ❌ | ✅ `Add`, `Remove`, `Sort` |
| Re-enumeration | May re-execute the query | ✅ Safe — always same data |
| Best for | Method parameters (widest compatibility) | In-memory data management |

```cs
// IEnumerable<T> — lazy, deferred
IEnumerable<int> LazySquares(int n)
{
    for (int i = 1; i <= n; i++)
    {
        Console.WriteLine($"Computing {i}²");
        yield return i * i;
    }
}

var seq = LazySquares(5); // nothing computed yet
var first = seq.First();   // computes 1² only, stops
Console.WriteLine(first);  // 1

// List<T> — eager, in-memory
List<int> squares = LazySquares(5).ToList(); // ALL 5 computed immediately
Console.WriteLine(squares[2]); // 9 (O(1) index access)
squares.Add(36);
squares.Sort();

// Use IEnumerable<T> as parameter type for maximum flexibility:
void PrintAll(IEnumerable<int> items) // accepts List, array, query, etc.
{
    foreach (var item in items)
        Console.Write($"{item} ");
}

PrintAll(squares);             // works
PrintAll([1, 2, 3]);           // works (array)
PrintAll(Enumerable.Range(1, 3)); // works (query)
```

**Decision:**
- Use `IEnumerable<T>` for **method parameters** (accepts any sequence).
- Use `List<T>` when you need **mutation, indexing, or multiple iterations**.
- Call `.ToList()` to materialise a lazy query into a concrete list.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. When to use ArrayList over array[] in C#?

**Answer: Almost never in modern .NET.** `ArrayList` was introduced in .NET 1.0 before generics existed. Since .NET 2.0, `List<T>` supersedes it completely.

**The only remaining legitimate use** of `ArrayList` is when maintaining **legacy code** that predates generics (.NET 1.x era) and cannot be refactored.

| Scenario | Recommendation |
|----------|---------------|
| New code — heterogeneous types | Use `List<object>` or a discriminated union/interface |
| New code — homogeneous types | Use `List<T>` or `T[]` |
| Interop with very old APIs expecting `ArrayList` | Use `ArrayList` only at the boundary |
| Legacy maintenance | Keep `ArrayList`, refactor when possible |

```cs
// ❌ Old approach — ArrayList loses type safety
var al = new System.Collections.ArrayList();
al.Add(42);       // boxing
al.Add("hello");  // mixed types — no compile error
int x = (int)al[0]; // unboxing — runtime error if wrong type

// ✅ Modern replacement
var list = new List<int> { 42, 99 };
list.Add(100); // type-safe, no boxing

// For truly heterogeneous data, use object or an interface:
var mixed = new List<object> { 42, "hello", 3.14, true };

// Or better yet, a discriminated union via a sealed hierarchy / oneOf:
sealed record IntValue(int Value);
sealed record StringValue(string Value);
var typed = new List<object> { new IntValue(42), new StringValue("hi") };
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the differences between IEnumerable and IQueryable?

| Feature | `IEnumerable<T>` | `IQueryable<T>` |
|---------|-----------------|----------------|
| Execution location | **In-memory** (C# code) | **Remote** (SQL, OData, etc.) |
| Query translation | No — LINQ methods run as C# delegates | Yes — expression trees translated to SQL |
| Data retrieval | Pulls all data from source first | Sends only necessary query to the data source |
| Provider | None (local iteration) | Requires a `QueryProvider` (e.g., EF Core) |
| Filtering | After fetching (expensive for large data) | At the source (efficient — WHERE in SQL) |
| Inherits from | `IEnumerable` | `IEnumerable<T>` + `IQueryable` |
| Use case | In-memory collections (List, Array) | ORM queries (EF Core, LINQ to SQL) |

```cs
var products = new List<Product>
{
    new("Laptop",  1200m),
    new("Mouse",   25m),
    new("Monitor", 400m),
    new("Keyboard",75m),
};

// IEnumerable<T> — filters IN MEMORY (all 4 rows loaded first)
IEnumerable<Product> expensiveEnum = products
    .Where(p => p.Price > 100); // C# delegate, runs in-memory

// IQueryable<T> — with EF Core, WHERE translated to SQL (only matching rows fetched)
// IQueryable<Product> expensiveQuery = dbContext.Products
//     .Where(p => p.Price > 100); // becomes: SELECT * FROM Products WHERE Price > 100
// var result = await expensiveQuery.ToListAsync(); // SQL executes here

// Mixing: IQueryable → AsEnumerable() forces in-memory from that point
// dbContext.Products
//     .Where(p => p.Price > 100)   // SQL
//     .AsEnumerable()              // switch to in-memory
//     .Where(p => p.Name.Contains("a")); // C# in-memory filter

record Product(string Name, decimal Price);
```

**Key rule:** Use `IQueryable<T>` when the data lives in a remote store (database). Use `IEnumerable<T>` for in-memory data. Never filter a large database table with `IEnumerable` — it loads every row into memory before filtering.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How to sort the generic SortedList in the descending order?

`SortedList<K,V>` always keeps keys in **ascending order** by default. To sort in descending order, pass a custom `IComparer<K>` that reverses the comparison.

```cs
// Ascending (default)
var ascending = new SortedList<int, string>
{
    [3] = "three",
    [1] = "one",
    [2] = "two",
};

foreach (var (k, v) in ascending)
    Console.WriteLine($"{k}: {v}"); // 1 / 2 / 3

// Descending — use Comparer<T>.Create with reversed comparison
var descending = new SortedList<int, string>(
    Comparer<int>.Create((a, b) => b.CompareTo(a))); // reverse!

descending[3] = "three";
descending[1] = "one";
descending[2] = "two";

foreach (var (k, v) in descending)
    Console.WriteLine($"{k}: {v}"); // 3 / 2 / 1

// For strings — descending alphabetically
var names = new SortedList<string, int>(
    StringComparer.OrdinalIgnoreCase, // case-insensitive
    Comparer<string>.Create((a, b) => string.Compare(b, a, StringComparison.OrdinalIgnoreCase)));

// Simpler: collect into a List and sort descending with LINQ
var sl = new SortedList<int, string> { [1] = "a", [3] = "c", [2] = "b" };

var sortedDesc = sl.OrderByDescending(kv => kv.Key).ToList();
foreach (var (k, v) in sortedDesc)
    Console.WriteLine($"{k}: {v}"); // 3 / 2 / 1
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a `HashSet` and when would you use it?

`HashSet<T>` is an **unordered collection of unique elements** backed by a hash table. It provides O(1) average-case operations for `Add`, `Remove`, and `Contains`.

**Use it when:**
- You need **uniqueness** enforcement automatically.
- You need fast **membership testing** (`Contains`).
- You need **set operations** (union, intersection, difference).

```cs
// Basic usage
var fruits = new HashSet<string> { "apple", "banana", "cherry" };
fruits.Add("apple");    // ❌ duplicate ignored — returns false
fruits.Add("date");     // ✅ added

Console.WriteLine(fruits.Contains("banana")); // True  — O(1)
Console.WriteLine(fruits.Count);              // 4

// Set operations
var a = new HashSet<int> { 1, 2, 3, 4, 5 };
var b = new HashSet<int> { 3, 4, 5, 6, 7 };

// Union — all elements from both
var union = new HashSet<int>(a);
union.UnionWith(b);
Console.WriteLine(string.Join(",", union)); // 1,2,3,4,5,6,7

// Intersection — elements in both
var intersection = new HashSet<int>(a);
intersection.IntersectWith(b);
Console.WriteLine(string.Join(",", intersection)); // 3,4,5

// Difference — elements in a but not b
var diff = new HashSet<int>(a);
diff.ExceptWith(b);
Console.WriteLine(string.Join(",", diff)); // 1,2

// Remove duplicates from a list — common pattern
var withDuplicates = new List<int> { 1, 2, 2, 3, 3, 3, 4 };
var unique = new HashSet<int>(withDuplicates);
Console.WriteLine(string.Join(",", unique)); // 1,2,3,4

// Frozen variant (.NET 8+) — for read-only lookup tables
using System.Collections.Frozen;
FrozenSet<string> reserved = FrozenSet.ToFrozenSet(
    new[] { "class", "void", "public", "private" });
Console.WriteLine(reserved.Contains("void")); // True
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How is LinkedList used in C#?

`LinkedList<T>` is a **doubly linked list** where each node (`LinkedListNode<T>`) holds a value and references to the previous and next nodes. It excels at **O(1) insertion and removal** at any position when you already have a reference to the node.

```cs
var ll = new LinkedList<string>();

// Add to end / beginning
ll.AddLast("B");
ll.AddFirst("A");
ll.AddLast("C");
// List: A <-> B <-> C

Console.WriteLine(string.Join(" -> ", ll)); // A -> B -> C

// Find a node and insert before/after it
var nodeB = ll.Find("B")!;
ll.AddBefore(nodeB, "A.5");
ll.AddAfter(nodeB, "B.5");
// List: A <-> A.5 <-> B <-> B.5 <-> C

Console.WriteLine(string.Join(" -> ", ll)); // A -> A.5 -> B -> B.5 -> C

// Remove by value or node
ll.Remove("A.5");
ll.Remove(nodeB); // O(1) — already have the node reference
Console.WriteLine(string.Join(" -> ", ll)); // A -> B.5 -> C

// Traverse forward and backward
var node = ll.First;
while (node is not null)
{
    Console.Write($"{node.Value} ");
    node = node.Next;
}
// A B.5 C

node = ll.Last;
while (node is not null)
{
    Console.Write($"{node.Value} ");
    node = node.Previous;
}
// C B.5 A
```

**When to use `LinkedList<T>`:**
- Frequent **insertions/removals in the middle** of a sequence (O(1) if node ref known).
- Implementing **LRU cache** (move-to-front pattern).
- **Undo/redo** lists, browser history navigation.

**When NOT to use:**
- Random index access is needed (O(n) vs O(1) for arrays).
- Cache performance matters (non-contiguous memory, poor locality).

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is difference between Stack and Heap?

In .NET, **stack** and **heap** are the two primary memory regions used during program execution.

| Aspect | Stack | Heap |
|--------|-------|------|
| Contents | Value types, method call frames, local variable references | Reference type objects, boxed values |
| Allocation | Automatic — push on call, pop on return | Managed by garbage collector |
| Access speed | Very fast (contiguous, CPU-cached) | Slower (random access, GC overhead) |
| Size | Limited (~1 MB per thread) | Large (limited by available RAM) |
| Lifetime | Limited to the scope/method | Until GC collects it |
| Thread | Each thread has its own stack | Shared across all threads |
| Fragmentation | None (LIFO) | Can fragment over time |

```cs
void Method()
{
    int x = 42;              // value type — stored on stack
    double y = 3.14;         // value type — stored on stack

    var list = new List<int>(); // reference type — object on heap
                                // `list` variable (reference) on stack

    var point = new Point(1, 2); // struct — on stack (value type)
    var obj   = new object();    // class  — on heap (reference type)
}

public record struct Point(int X, int Y); // struct = value type
```

**Visual:**

```
STACK (per thread)            HEAP (shared)
┌─────────────────┐           ┌────────────────────────┐
│ x = 42          │           │ List<int> object        │
│ y = 3.14        │           │  [_items array, Count]  │
│ list ──────────────────────►│                         │
│ point = (1,2)   │           │ object {}               │
│ obj ────────────────────────►                         │
└─────────────────┘           └────────────────────────┘
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Are string allocated on stack or heap?

**Strings are reference types — their content is allocated on the heap.** The variable holding the reference lives on the stack (or inline in an object), but the actual string data is on the heap.

However, `string` has special behavior:
- **String interning** — the CLR interns string literals; identical literals share the same heap object.
- **Immutability** — strings are immutable; every "modification" creates a new heap object.

```cs
string s1 = "hello"; // literal — interned on heap, reference on stack
string s2 = "hello"; // same interned object — s1 and s2 point to same heap address

Console.WriteLine(object.ReferenceEquals(s1, s2)); // True (interned)

string s3 = new string("hello".ToCharArray()); // forced new object — NOT interned
Console.WriteLine(object.ReferenceEquals(s1, s3)); // False

// Manual interning
string s4 = string.Intern(s3); // returns the interned version
Console.WriteLine(object.ReferenceEquals(s1, s4)); // True

// String immutability — each operation allocates a new heap object
string original = "Hello";
string modified = original + " World"; // new heap string
Console.WriteLine(object.ReferenceEquals(original, modified)); // False

// For many string operations, use StringBuilder (single buffer)
var sb = new System.Text.StringBuilder();
for (int i = 0; i < 1000; i++)
    sb.Append(i);
string result = sb.ToString(); // single heap allocation
```

**Summary:** String variable → stack (or field in object); String content → always heap.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How many stack and heaps are created for an application?

- **Stack:** One stack per **thread**. Each thread gets its own private stack (default ~1 MB, configurable).
- **Heap:** One **managed heap** per **process** (AppDomain), shared across all threads.

.NET\'s managed heap is internally divided into:
- **Small Object Heap (SOH)** — Generation 0, 1, 2 for objects < 85,000 bytes.
- **Large Object Heap (LOH)** — objects ≥ 85,000 bytes (arrays, large strings).
- **Pinned Object Heap (POH)** — .NET 5+ — pinned objects that must not be moved by GC.

```cs
// Each new thread = new stack
var t1 = new Thread(() =>
{
    int x = 10; // on t1\'s own stack
    Console.WriteLine($"Thread {Thread.CurrentThread.ManagedThreadId}: x={x}");
});

var t2 = new Thread(() =>
{
    int x = 20; // on t2\'s own stack — different from t1\'s x
    Console.WriteLine($"Thread {Thread.CurrentThread.ManagedThreadId}: x={x}");
});

t1.Start(); t2.Start();
t1.Join();  t2.Join();

// Heap is shared — reference types visible across threads
var shared = new List<int>();
var t3 = new Thread(() => shared.Add(1)); // modifies shared heap object
var t4 = new Thread(() => shared.Add(2)); // same heap object
t3.Start(); t4.Start();
t3.Join();  t4.Join();
Console.WriteLine(shared.Count); // 2 (both threads wrote to same heap list)
// Note: List<T> is NOT thread-safe — use ConcurrentBag or lock in production
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How are stack and heap memory deallocated?

**Stack deallocation — automatic and immediate:**

Stack memory is released as soon as a method returns. The stack pointer moves back (stack "unwinds"). No GC involvement.

```cs
void Outer()
{
    Inner();
    // After Inner() returns, all of Inner\'s stack frame is gone
}

void Inner()
{
    int a = 10;       // allocated on stack
    double b = 3.14;  // allocated on stack
} // stack frame released here — a and b are gone
```

**Heap deallocation — by the Garbage Collector (GC):**

Objects on the heap are not freed immediately. The GC traces live references and collects unreachable objects in generations (Gen 0, 1, 2).

```cs
void CreateObject()
{
    var obj = new byte[1000]; // allocated on heap
    // use obj...
} // obj reference is gone from stack — object is GC-eligible
// GC may collect it at any later point

// Force collection (for demonstration only — avoid in production)
GC.Collect();
GC.WaitForPendingFinalizers();
```

**`IDisposable` / `using` — release unmanaged resources deterministically:**

```cs
using var stream = new FileStream("file.txt", FileMode.OpenOrCreate);
// stream is disposed (file handle released) here — deterministic, not waiting for GC
```

| Memory | Released by | When |
|--------|------------|------|
| Stack | CPU (stack pointer) | When method returns |
| Heap (managed) | GC | Non-deterministically (Gen 0/1/2 collections) |
| Heap (unmanaged) | `Dispose()` / finalizer | `using` block or GC finalizer queue |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Who clears the heap memory?

The **.NET Garbage Collector (GC)** is responsible for clearing heap memory. It:

1. **Traces** all live object references starting from GC roots (static fields, local variables, CPU registers, GC handles).
2. **Marks** reachable objects as alive.
3. **Collects** (sweeps) objects not marked — their memory is reclaimed.
4. **Compacts** the heap to remove fragmentation (for Gen 0/1/2; LOH is not compacted by default).

```cs
public class Resource
{
    private readonly string _name;
    public Resource(string name) => _name = name;

    // Finalizer — called by GC on a separate finalizer thread
    ~Resource() => Console.WriteLine($"GC collected: {_name}");
}

// Demonstrate GC collection
var r = new Resource("MyResource");
r = null!; // remove the only reference — object becomes unreachable

GC.Collect();               // request a GC (not guaranteed immediate)
GC.WaitForPendingFinalizers(); // wait for finalizer to run
// Output: GC collected: MyResource

// For deterministic cleanup of unmanaged resources:
public class ManagedResource : IDisposable
{
    private bool _disposed;

    public void Dispose()
    {
        if (_disposed) return;
        // Free unmanaged resources here (file handles, network connections, etc.)
        Console.WriteLine("Resource disposed");
        _disposed = true;
        GC.SuppressFinalize(this); // tell GC not to call finalizer
    }

    ~ManagedResource() => Dispose(); // fallback if Dispose not called
}

using var res = new ManagedResource(); // Dispose called at end of using block
```

**GC generations:**
- **Gen 0** — short-lived objects (most common). Collected frequently.
- **Gen 1** — survived Gen 0. Buffer between Gen 0 and Gen 2.
- **Gen 2** — long-lived objects. Collected infrequently (full GC).
- **LOH** — large objects (≥ 85 KB). Collected with Gen 2.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Where is structure allocated Stack or Heap?

**It depends on context** — not always the stack:

| Where struct is used | Allocation location |
|---------------------|---------------------|
| Local variable in a method | **Stack** |
| Field of a class (reference type) | **Heap** (inside the class object) |
| Field of another struct | Same location as the containing struct |
| Boxed (cast to `object` or interface) | **Heap** |
| Array element (`int[]`, `Point[]`) | **Heap** (arrays are reference types) |

```cs
public struct Point { public int X, Y; }
public class Container { public Point Location; } // Point field — on heap inside Container

void Demo()
{
    Point p1 = new Point { X = 1, Y = 2 }; // local var — STACK

    var c = new Container();
    c.Location = new Point { X = 3, Y = 4 }; // field in class — HEAP

    Point[] points = [new(1, 2), new(3, 4)]; // array — HEAP (contiguous)

    // Boxing — struct moved to heap
    object boxed = p1;               // heap allocation!
    Point unboxed = (Point)boxed;    // copy back from heap
}

// readonly record struct (C# 10) — best practice for immutable value types
public readonly record struct Vector2D(double X, double Y)
{
    public double Length => Math.Sqrt(X * X + Y * Y);
}

void VectorDemo()
{
    Vector2D v = new(3, 4); // STACK — local variable
    Console.WriteLine(v.Length); // 5
}
```

**Key point:** The common saying "structs are on the stack" is an oversimplification. Structs follow the same allocation rules as their container.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can structures get created on Heap?

**Yes.** Structs can live on the heap in several scenarios:

1. **Boxed** — cast to `object` or an interface.
2. **Field of a class** — embedded in the class object\'s heap memory.
3. **Element of an array** — arrays are reference types (heap).
4. **Captured by a closure** — if a struct local variable is captured by a lambda/delegate, it may be promoted to the heap.
5. **`async` state machine** — local struct variables in async methods are stored in the heap-allocated state machine object.

```cs
public struct Counter { public int Value; }

// 1. Boxing — struct moves to heap
Counter c = new Counter { Value = 5 };
object boxed = c;  // heap allocation, copy of c

// 2. Class field — struct embedded in heap object
public class Wrapper { public Counter Counter; }
var w = new Wrapper(); // w.Counter lives on heap (inside Wrapper)

// 3. Array — heap
Counter[] arr = new Counter[3]; // 3 Counter structs, all on heap

// 4. Closure capture
int x = 10; // local int (struct) captured by lambda → promoted to heap
Action inc = () => x++; // x is now on the heap inside a display class
inc();
Console.WriteLine(x); // 11

// 5. async method — locals stored in heap state machine
async Task AsyncDemo()
{
    Counter local = new Counter { Value = 1 }; // stored in async state machine (heap)
    await Task.Delay(1);
    Console.WriteLine(local.Value);
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Is there a way we can see this Heap memory?

Yes — several tools and APIs expose the managed heap:

**1. Via code — `GC` APIs:**

```cs
// Total memory currently used by managed heap
long before = GC.GetTotalMemory(forceFullCollection: false);

var list = new List<byte[]>();
for (int i = 0; i < 100; i++)
    list.Add(new byte[1024]); // allocate 100 KB

long after = GC.GetTotalMemory(forceFullCollection: false);
Console.WriteLine($"Heap grew by: {(after - before) / 1024} KB");

// GC memory info (.NET 5+)
GCMemoryInfo info = GC.GetGCMemoryInfo();
Console.WriteLine($"Heap size: {info.HeapSizeBytes / 1_048_576:F1} MB");
Console.WriteLine($"Committed: {info.TotalCommittedBytes / 1_048_576:F1} MB");
Console.WriteLine($"Gen0 size: {info.GenerationInfo[0].SizeAfterBytes} bytes");
Console.WriteLine($"Gen2 size: {info.GenerationInfo[2].SizeAfterBytes} bytes");

// Generation of a specific object
var obj = new object();
Console.WriteLine($"obj is in Gen{GC.GetGeneration(obj)}"); // 0 (just allocated)
GC.Collect();
Console.WriteLine($"obj is in Gen{GC.GetGeneration(obj)}"); // 1 (survived collection)
```

**2. External tools:**
- **dotMemory** (JetBrains) — heap snapshots, object retention paths.
- **Visual Studio Diagnostic Tools** — heap allocation timeline, snapshot comparison.
- **PerfView** — GC events, allocation stacks.
- **dotnet-dump** — capture and analyze memory dumps (`dotnet-dump collect`, `dotnet-dump analyze`).
- **dotnet-gcdump** — GC heap dump in `.gcdump` format.

```powershell
# Capture a GC heap dump from command line
dotnet-gcdump collect -p <pid>
# Open in Visual Studio or dotnet-gcdump report for analysis
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain stack and Heap?

**Stack** is a **LIFO (Last-In, First-Out)** memory region used for method call frames and local variables. Each thread has its own stack. Memory is allocated and deallocated automatically and instantly as methods are called and return.

**Heap** is a **large, shared memory region** managed by the .NET GC. Reference type objects are allocated here. The GC periodically reclaims memory from unreachable objects.

```
 Stack (per thread)                  Heap (shared, GC managed)
 ┌─────────────────────────┐         ┌─────────────────────────┐
 │ Main() frame            │         │ ┌─────────────────────┐ │
 │   args reference ───────────────► │ │ string[] args       │ │
 │   person ref ───────────────────► │ └─────────────────────┘ │
 ├─────────────────────────┤         │ ┌─────────────────────┐ │
 │ CreatePerson() frame    │         │ │ Person object       │ │
 │   name = "Pradeep"      │         │ │  Name: "Pradeep"    │ │
 │   age  = 30             │         │ │  Age:  30           │ │
 │   p ref ────────────────────────► │ └─────────────────────┘ │
 └─────────────────────────┘         └─────────────────────────┘
```

```cs
class Person { public string Name { get; } = ""; public int Age; }

void Demo()
{
    int age = 30;                  // value type → stack
    string name = "Pradeep";      // reference → stack, content → heap
    Person p = new Person();       // reference → stack, object → heap

    Console.WriteLine(GC.GetGeneration(p)); // 0 — just allocated
} // stack frame released; p reference gone; Person object GC-eligible
```

**Summary:**

| Stack | Heap |
|-------|------|
| Fast, automatic, LIFO | Slower, GC-managed |
| Value types + references | Reference type objects |
| Per-thread | Shared across threads |
| Limited size (~1 MB) | Large (GB range) |
| No fragmentation | Can fragment (GC compacts) |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Where are stack and heap stored?

Both stack and heap are stored in **RAM (physical and virtual memory)**, managed by the OS and the .NET runtime:

- **Stack** — each thread\'s stack is allocated as a contiguous block of virtual memory by the OS when the thread is created. The CPU\'s stack pointer register (`RSP` on x64) tracks the current top.
- **Managed heap** — the .NET runtime requests virtual memory pages from the OS via VirtualAlloc (Windows) or mmap (Linux/macOS). The GC manages segments within this virtual memory.

```cs
// You can observe memory allocations via the Environment class
Console.WriteLine($"64-bit process: {Environment.Is64BitProcess}");

// Working set and virtual memory
using var proc = System.Diagnostics.Process.GetCurrentProcess();
Console.WriteLine($"Working set:   {proc.WorkingSet64 / 1_048_576} MB");
Console.WriteLine($"Virtual memory:{proc.VirtualMemorySize64 / 1_048_576} MB");
Console.WriteLine($"Private memory:{proc.PrivateMemorySize64 / 1_048_576} MB");

// Heap info via GC
GCMemoryInfo info = GC.GetGCMemoryInfo();
Console.WriteLine($"Total heap committed: {info.TotalCommittedBytes / 1_048_576} MB");
```

**Conceptually:**
```
Physical RAM
  ├── OS kernel code + data
  ├── Thread 1 stack (virtual address range, ~1 MB reserved)
  ├── Thread 2 stack (separate range)
  ├── Managed heap segments (GC-managed, can grow)
  │     ├── Gen 0 segment
  │     ├── Gen 1 segment
  │     ├── Gen 2 segment
  │     └── LOH segment
  └── Native heaps (unmanaged, DLL code, etc.)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What goes on stack and what goes on heap?

**Stack:**
- Local value type variables (`int`, `double`, `bool`, `char`, `struct`, `enum`)
- Method parameters (value types passed by value)
- References (pointers) to heap objects
- Return addresses for method calls

**Heap:**
- All reference type objects (`class`, `string`, `array`, `delegate`, `interface` instances)
- Static fields (stored in a special "high-frequency heap" / static area)
- Boxed value types
- Large objects (≥ 85,000 bytes go to LOH)

```cs
public struct Point { public int X, Y; }   // value type
public class Circle { public double R; }   // reference type

void Example()
{
    // ── STACK ──────────────────────────────────────────
    int age     = 30;          // int → stack
    bool active = true;        // bool → stack
    Point pt    = new(3, 4);   // struct → stack (inline)
    Circle c    = new() { R = 5 }; // c (reference) → stack; object → heap

    // ── HEAP ───────────────────────────────────────────
    // new Circle() object is on heap, 'c' on stack points to it
    var list = new List<int>(); // List object on heap; 'list' ref on stack
    list.Add(42);               // int 42 stored inside the List\'s backing array (heap)

    // Boxing — value type moved to heap
    object boxed = age;         // new heap allocation; 'boxed' ref on stack

    // Array — always heap (array is a reference type)
    int[] arr = [1, 2, 3];      // array object on heap; 'arr' ref on stack
}
```

**Rule of thumb:**

| Category | Where |
|----------|-------|
| `int`, `double`, `bool`, `char`, `decimal` | Stack (as locals) |
| `struct` | Stack (as locals) |
| `class` instance | Heap |
| `string` content | Heap |
| `array` | Heap |
| Reference variable | Stack |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How is the stack memory address arranged?

The stack grows **downward** in memory on x86/x64 architectures. When a new method frame is pushed, the stack pointer (RSP) is **decremented** (moves toward lower addresses). When the method returns, RSP is **incremented** back (moves toward higher addresses).

```
High addresses
  ┌─────────────────────────┐ ← Stack base (thread start)
  │ Main() frame            │
  │   [return address]      │
  │   [saved registers]     │
  │   args = ...            │ RSP points here when in Main()
  ├─────────────────────────┤
  │ Method1() frame         │ ← RSP decremented when Method1() called
  │   [return address]      │
  │   local int a = 10      │
  │   local double b = 3.14 │
  ├─────────────────────────┤
  │ Method2() frame         │ ← RSP decremented again
  │   [return address]      │
  │   local int x = 5       │ ← RSP points here (top of stack)
  └─────────────────────────┘
Low addresses (stack grows downward ↓)
```

```cs
// You can observe stack behavior via StackTrace
void InnerMethod()
{
    var trace = new System.Diagnostics.StackTrace(fNeedFileInfo: true);
    Console.WriteLine(trace);
    // Output shows call stack: InnerMethod → OuterMethod → Main
}

void OuterMethod() => InnerMethod();
OuterMethod();
```

**Key points:**
- Stack memory is **contiguous** — excellent CPU cache behavior.
- **Stack overflow** occurs when too many frames are pushed (e.g., infinite recursion) and RSP goes past the stack\'s reserved limit.
- Each thread\'s stack is independent — different threads have stacks at different address ranges.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How is stack memory deallocated LIFO or FIFO?

**LIFO (Last-In, First-Out).** Stack memory deallocation is strictly LIFO — the most recently pushed frame is always released first (when its method returns).

```cs
void A()
{
    int x = 1; // pushed when A() called
    Console.WriteLine("A: entered");
    B();       // B\'s frame pushed ON TOP of A\'s frame
    Console.WriteLine("A: B returned"); // A\'s frame still alive
} // A\'s frame popped — x released

void B()
{
    int y = 2; // pushed when B() called
    Console.WriteLine("B: entered");
    C();       // C\'s frame pushed on top
    Console.WriteLine("B: C returned");
} // B\'s frame popped — y released

void C()
{
    int z = 3; // pushed last
    Console.WriteLine("C: entered");
} // C\'s frame popped FIRST — z released first (LIFO)

A();
// Output:
// A: entered
// B: entered
// C: entered   ← C allocated last
// B: C returned
// A: B returned
// Stack release order: C first, then B, then A (LIFO)
```

**Why LIFO works:** Methods call each other in a nested (tree) structure. A callee always returns before its caller — so the callee\'s frame is always on top and is always freed first. This makes a simple stack pointer increment sufficient for deallocation — no GC, no scanning, no bookkeeping.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you choose between `List<T>` and `ArrayList` in C#?

**Always choose `List<T>` in new code.** `ArrayList` is a legacy type that predates generics and should not be used in modern .NET.

| Decision factor | `ArrayList` | `List<T>` |
|----------------|------------|-----------|
| Type safety | ❌ Stores `object` — runtime cast errors | ✅ Compile-time type checking |
| Performance (value types) | ❌ Boxing/unboxing on every operation | ✅ No boxing — direct storage |
| IntelliSense / tooling | ❌ All methods return `object` | ✅ Full typed IntelliSense |
| LINQ | Requires cast | ✅ Native LINQ support |
| Recommended | Legacy code only | ✅ Always |

```cs
// ❌ ArrayList — avoid in new code
var al = new System.Collections.ArrayList();
al.Add(42);       // boxing int → object
al.Add("mixed");  // no compile error — mixed types allowed
int n = (int)al[0]; // unboxing, runtime error if wrong type

// ✅ List<T> — always prefer
var list = new List<int> { 1, 2, 3 };
list.Add(4);         // no boxing
list.Add(5);
Console.WriteLine(list.Count); // 5

// List<T> with LINQ
var evens = list.Where(x => x % 2 == 0).ToList();
Console.WriteLine(string.Join(",", evens)); // 2,4

// List<T> capacity pre-allocation for performance
var large = new List<int>(capacity: 1_000_000);
for (int i = 0; i < 1_000_000; i++)
    large.Add(i); // no re-allocation needed — capacity was pre-set
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you iterate over a collection in C#?

```cs
var numbers = new List<int> { 1, 2, 3, 4, 5 };

// 1. foreach — simplest, works on any IEnumerable<T>
foreach (int n in numbers)
    Console.Write($"{n} "); // 1 2 3 4 5

// 2. for loop — when you need the index
for (int i = 0; i < numbers.Count; i++)
    Console.Write($"[{i}]={numbers[i]} ");

// 3. LINQ — declarative, composable
numbers.ForEach(Console.WriteLine);    // List<T>.ForEach
var doubled = numbers.Select(n => n * 2).ToList();

// 4. while with enumerator — manual control
using var enumerator = numbers.GetEnumerator();
while (enumerator.MoveNext())
    Console.Write($"{enumerator.Current} ");

// 5. Span<T> / ReadOnlySpan<T> — zero-allocation iteration (C# 7.2+)
int[] arr = [10, 20, 30, 40];
ReadOnlySpan<int> span = arr;
foreach (ref readonly int item in span)
    Console.Write($"{item} ");

// 6. Dictionary iteration
var dict = new Dictionary<string, int> { ["a"] = 1, ["b"] = 2 };
foreach (var (key, value) in dict) // deconstruction (C# 7+)
    Console.WriteLine($"{key}={value}");

// 7. Parallel iteration (CPU-bound work)
Parallel.ForEach(numbers, n => Console.Write($"{n * n} ")); // order not guaranteed

// 8. Async iteration — IAsyncEnumerable<T> (C# 8+)
async IAsyncEnumerable<int> GetAsync()
{
    foreach (var n in numbers)
    {
        await Task.Delay(1);
        yield return n;
    }
}

await foreach (int n in GetAsync())
    Console.Write($"{n} ");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `IEnumerable` and `ICollection` in C#?

`ICollection<T>` extends `IEnumerable<T>` by adding **size awareness** and **modification** capabilities.

| Member | `IEnumerable<T>` | `ICollection<T>` |
|--------|-----------------|-----------------|
| `GetEnumerator()` | ✅ | ✅ (inherited) |
| `Count` | ❌ | ✅ |
| `IsReadOnly` | ❌ | ✅ |
| `Add(T)` | ❌ | ✅ |
| `Remove(T)` | ❌ | ✅ |
| `Contains(T)` | ❌ | ✅ |
| `Clear()` | ❌ | ✅ |
| `CopyTo(T[], int)` | ❌ | ✅ |

```cs
// IEnumerable<T> — iterate only
IEnumerable<int> seq = [1, 2, 3, 4, 5];
foreach (var n in seq) Console.Write($"{n} ");
// seq.Count(); // works via LINQ extension, but O(n)
// seq.Add(6);  // ❌ no Add on IEnumerable

// ICollection<T> — add, remove, count
ICollection<string> col = new List<string> { "Alice", "Bob" };
col.Add("Carol");
col.Remove("Bob");
Console.WriteLine(col.Count);          // 2
Console.WriteLine(col.Contains("Alice")); // True

// Implementing ICollection<T>
public class FixedSizeCollection<T> : ICollection<T>
{
    private readonly List<T> _inner = [];
    private readonly int _maxSize;

    public FixedSizeCollection(int maxSize) => _maxSize = maxSize;

    public int Count => _inner.Count;
    public bool IsReadOnly => false;

    public void Add(T item)
    {
        if (_inner.Count >= _maxSize)
            throw new InvalidOperationException("Collection is full");
        _inner.Add(item);
    }

    public bool Remove(T item)      => _inner.Remove(item);
    public bool Contains(T item)    => _inner.Contains(item);
    public void Clear()             => _inner.Clear();
    public void CopyTo(T[] arr, int i) => _inner.CopyTo(arr, i);
    public IEnumerator<T> GetEnumerator() => _inner.GetEnumerator();
    System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() => GetEnumerator();
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you sort a collection in C#?

```cs
// 1. List<T>.Sort() — in-place, uses default comparer
var numbers = new List<int> { 5, 3, 1, 4, 2 };
numbers.Sort();
Console.WriteLine(string.Join(",", numbers)); // 1,2,3,4,5

// 2. List<T>.Sort() with custom comparer — descending
numbers.Sort((a, b) => b.CompareTo(a));
Console.WriteLine(string.Join(",", numbers)); // 5,4,3,2,1

// 3. Array.Sort()
int[] arr = [5, 3, 1, 4, 2];
Array.Sort(arr);
Console.WriteLine(string.Join(",", arr)); // 1,2,3,4,5

// 4. LINQ OrderBy / OrderByDescending — returns new sequence (non-destructive)
var names = new List<string> { "Charlie", "Alice", "Bob" };
var sorted = names.OrderBy(n => n).ToList();
var desc   = names.OrderByDescending(n => n).ToList();
Console.WriteLine(string.Join(",", sorted)); // Alice,Bob,Charlie
Console.WriteLine(string.Join(",", desc));   // Charlie,Bob,Alice

// 5. Sort complex objects
record Product(string Name, decimal Price);

var products = new List<Product>
{
    new("Laptop", 999m),
    new("Mouse", 25m),
    new("Monitor", 400m),
};

// By single property
var byPrice = products.OrderBy(p => p.Price).ToList();

// By multiple properties
var sorted2 = products
    .OrderBy(p => p.Name.Length) // primary
    .ThenBy(p => p.Price)        // secondary
    .ToList();

foreach (var p in byPrice)
    Console.WriteLine($"{p.Name}: {p.Price:C}");

// 6. IComparer<T> — reusable custom sort logic
var byNameDesc = new List<string> { "Charlie", "Alice", "Bob" };
byNameDesc.Sort(StringComparer.OrdinalIgnoreCase); // case-insensitive ascending

// 7. CollectionsMarshal / Span sort (.NET 5+) — zero-allocation in-place
var span = System.Runtime.InteropServices.CollectionsMarshal.AsSpan(numbers);
span.Sort(); // sorts the List\'s backing array directly
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you remove duplicates from a collection in C#?

```cs
var withDups = new List<int> { 1, 2, 2, 3, 3, 3, 4, 4, 5 };

// 1. HashSet<T> — fastest, loses order
var unique1 = new HashSet<int>(withDups);
Console.WriteLine(string.Join(",", unique1)); // 1,2,3,4,5 (order may vary)

// 2. LINQ Distinct() — preserves first occurrence order
var unique2 = withDups.Distinct().ToList();
Console.WriteLine(string.Join(",", unique2)); // 1,2,3,4,5 (ordered)

// 3. LINQ DistinctBy (C# 6 / .NET 6+) — by property
record Product(string Name, decimal Price);

var products = new List<Product>
{
    new("Laptop",  999m),
    new("Mouse",   25m),
    new("Laptop",  899m), // duplicate name
    new("Monitor", 400m),
};

var distinctByName = products.DistinctBy(p => p.Name).ToList();
foreach (var p in distinctByName)
    Console.WriteLine($"{p.Name}: {p.Price:C}");
// Laptop: £999.00, Mouse: £25.00, Monitor: £400.00

// 4. GroupBy — group then take first (more control)
var deduplicated = products
    .GroupBy(p => p.Name)
    .Select(g => g.OrderBy(p => p.Price).First()) // take cheapest per name
    .ToList();

// 5. ToHashSet() extension — convenient
var strings = new[] { "apple", "banana", "apple", "cherry", "banana" };
var uniqueStrings = strings.ToHashSet();
Console.WriteLine(string.Join(",", uniqueStrings)); // apple,banana,cherry

// 6. Custom equality — using IEqualityComparer
var uniqueIgnoreCase = strings
    .Distinct(StringComparer.OrdinalIgnoreCase)
    .ToList();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `Queue` and `Stack` in C#?

| Feature | `Queue<T>` | `Stack<T>` |
|---------|-----------|-----------|
| Order | **FIFO** — First In, First Out | **LIFO** — Last In, First Out |
| Add | `Enqueue(item)` | `Push(item)` |
| Remove | `Dequeue()` | `Pop()` |
| Peek (no remove) | `Peek()` | `Peek()` |
| Non-removing try | `TryDequeue(out T)` | `TryPop(out T)` |
| Use case | Task queues, BFS, print spoolers | Undo/redo, call stack simulation, DFS |

```cs
// Queue<T> — FIFO
var queue = new Queue<string>();
queue.Enqueue("First");
queue.Enqueue("Second");
queue.Enqueue("Third");

Console.WriteLine(queue.Dequeue()); // First  ← oldest item out
Console.WriteLine(queue.Peek());    // Second ← next without removing
Console.WriteLine(queue.Count);     // 2

// Practical: task processing queue
var taskQueue = new Queue<Func<Task>>();
taskQueue.Enqueue(() => Task.Run(() => Console.WriteLine("Task A")));
taskQueue.Enqueue(() => Task.Run(() => Console.WriteLine("Task B")));

while (taskQueue.TryDequeue(out var task))
    await task();

// Stack<T> — LIFO
var stack = new Stack<string>();
stack.Push("First");
stack.Push("Second");
stack.Push("Third");

Console.WriteLine(stack.Pop());  // Third  ← most recent item out
Console.WriteLine(stack.Peek()); // Second ← next without removing
Console.WriteLine(stack.Count);  // 2

// Practical: undo history
var undoStack = new Stack<string>();
undoStack.Push("Type 'Hello'");
undoStack.Push("Type ' World'");
undoStack.Push("Delete 'World'");

Console.WriteLine($"Undo: {undoStack.Pop()}"); // Undo: Delete 'World'
Console.WriteLine($"Undo: {undoStack.Pop()}"); // Undo: Type ' World'
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you convert an array to a collection in C#?

```cs
int[] arr = [1, 2, 3, 4, 5];

// 1. To List<T>
var list = arr.ToList();             // LINQ extension — O(n) copy
var list2 = new List<int>(arr);      // constructor overload
Console.WriteLine(list.GetType().Name); // List`1

// 2. To HashSet<T> — deduplicate while converting
var set = arr.ToHashSet();
int[] withDups = [1, 2, 2, 3, 3];
var uniqueSet = withDups.ToHashSet(); // { 1, 2, 3 }

// 3. To Queue<T>
var queue = new Queue<int>(arr);     // FIFO order preserved

// 4. To Stack<T>
var stack = new Stack<int>(arr);     // note: iteration order is reversed

// 5. To Dictionary<K,V>
string[] words = ["apple", "banana", "cherry"];
var wordDict = words.ToDictionary(w => w, w => w.Length);
// { "apple": 5, "banana": 6, "cherry": 6 }

// 6. To ImmutableList<T>
using System.Collections.Immutable;
var immutable = arr.ToImmutableList();
var added = immutable.Add(6); // returns new list; original unchanged

// 7. To Span<T> / Memory<T> — zero-copy (for performance-sensitive code)
Span<int> span     = arr.AsSpan();
Memory<int> memory = arr.AsMemory();
span[0] = 99; // modifies the original array

// 8. Collection expressions (C# 12) — spread operator
List<int> merged = [.. arr, 6, 7, 8]; // spread arr + append literals

// 9. As IEnumerable<T> — no copy (already implements it)
IEnumerable<int> enumerable = arr;
foreach (var n in enumerable) Console.Write($"{n} ");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the thread-safe collection classes available in C#?

Thread-safe collections in `System.Collections.Concurrent` use fine-grained locking or lock-free algorithms (CAS — compare-and-swap) instead of requiring external `lock` statements.

| Class | Thread-safe equivalent of | Key operations |
|-------|--------------------------|----------------|
| `ConcurrentDictionary<K,V>` | `Dictionary<K,V>` | `TryAdd`, `TryUpdate`, `GetOrAdd`, `AddOrUpdate` |
| `ConcurrentQueue<T>` | `Queue<T>` | `Enqueue`, `TryDequeue`, `TryPeek` |
| `ConcurrentStack<T>` | `Stack<T>` | `Push`, `TryPop`, `TryPeek` |
| `ConcurrentBag<T>` | (unordered) | `Add`, `TryTake`, `TryPeek` |
| `BlockingCollection<T>` | (producer-consumer) | `Add`, `Take`, `GetConsumingEnumerable` |

```cs
// ConcurrentDictionary — atomic operations
var counter = new ConcurrentDictionary<string, int>();

await Task.WhenAll(Enumerable.Range(0, 1000).Select(_ =>
    Task.Run(() => counter.AddOrUpdate("hits", 1, (_, old) => old + 1))));

Console.WriteLine(counter["hits"]); // 1000 (always correct)

// ConcurrentQueue — thread-safe task distribution
var workQueue = new ConcurrentQueue<int>();
for (int i = 0; i < 10; i++) workQueue.Enqueue(i);

var workers = Enumerable.Range(0, 3).Select(_ => Task.Run(() =>
{
    while (workQueue.TryDequeue(out int item))
        Console.WriteLine($"Thread {Thread.CurrentThread.ManagedThreadId} processed {item}");
}));

await Task.WhenAll(workers);

// ConcurrentBag — unordered, thread-local storage (good for object pools)
var bag = new ConcurrentBag<int>();
await Task.WhenAll(Enumerable.Range(0, 5).Select(i =>
    Task.Run(() => bag.Add(i))));
Console.WriteLine(bag.Count); // 5

// ImmutableList — safe to share across threads (read-only after creation)
using System.Collections.Immutable;
var shared = ImmutableList.Create(1, 2, 3);
// Any thread can read — no synchronisation needed (immutable)
var modified = shared.Add(4); // returns NEW list, original unchanged
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use LINQ with collections in C#?

LINQ (Language Integrated Query) provides a rich set of extension methods on `IEnumerable<T>` for querying and transforming collections declaratively.

```cs
record Product(string Name, string Category, decimal Price, int Stock);

var products = new List<Product>
{
    new("Laptop",   "Electronics", 999m,  15),
    new("Mouse",    "Electronics", 25m,   200),
    new("Desk",     "Furniture",   350m,  30),
    new("Monitor",  "Electronics", 450m,  50),
    new("Chair",    "Furniture",   250m,  40),
    new("Keyboard", "Electronics", 75m,   150),
};

// Filter (Where)
var electronics = products.Where(p => p.Category == "Electronics").ToList();

// Project (Select)
var names = products.Select(p => p.Name).ToList();

// Sort (OrderBy / ThenBy)
var sorted = products
    .OrderBy(p => p.Category)
    .ThenByDescending(p => p.Price)
    .ToList();

// Aggregate
decimal total = products.Sum(p => p.Price);
decimal avg   = products.Average(p => p.Price);
int     count = products.Count(p => p.Price > 100);

Console.WriteLine($"Total: {total:C}, Avg: {avg:C}, Count>100: {count}");

// Group
var byCategory = products
    .GroupBy(p => p.Category)
    .Select(g => new { Category = g.Key, Count = g.Count(), TotalValue = g.Sum(p => p.Price) })
    .ToList();

foreach (var g in byCategory)
    Console.WriteLine($"{g.Category}: {g.Count} items, £{g.TotalValue}");

// Flat map (SelectMany)
var tags = new[] { new[] { "a", "b" }, new[] { "c", "d" } };
var allTags = tags.SelectMany(t => t).ToList(); // [a, b, c, d]

// First / Single / Any / All
var cheapest = products.MinBy(p => p.Price);
bool anyOutOfStock = products.Any(p => p.Stock == 0);
bool allInStock    = products.All(p => p.Stock > 0);

// Distinct / DistinctBy (.NET 6+)
var categories = products.DistinctBy(p => p.Category).Select(p => p.Category).ToList();

// Take / Skip / Chunk
var page1 = products.OrderBy(p => p.Name).Take(3).ToList();
var page2 = products.OrderBy(p => p.Name).Skip(3).Take(3).ToList();
var pages = products.Chunk(2).ToList(); // groups of 2

// Query syntax (equivalent to method syntax above)
var expensiveElectronics =
    from p in products
    where p.Category == "Electronics" && p.Price > 100
    orderby p.Price descending
    select p;

foreach (var p in expensiveElectronics)
    Console.WriteLine($"{p.Name}: {p.Price:C}");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `Array` and `List` in C#?

| Feature | `T[]` (Array) | `List<T>` |
|---------|--------------|-----------|
| Size | Fixed at creation | Dynamic (auto-resizes) |
| Type | Value/reference — contiguous memory | Backed by `T[]`, resized on demand |
| Index access | ✅ O(1) | ✅ O(1) |
| Add / Remove | ❌ Not supported | ✅ `Add`, `Remove`, `Insert` |
| `Length` / `Count` | `Length` | `Count` |
| Memory | Slightly more efficient (no metadata) | Small overhead for capacity tracking |
| Multi-dimensional | ✅ (`int[,]`, `int[][]`) | ❌ (use `List<List<T>>` instead) |
| LINQ | ✅ | ✅ |
| Span/Memory | ✅ `AsSpan()` | ✅ `CollectionsMarshal.AsSpan()` |
| Interop (P/Invoke, etc.) | ✅ Preferred | ❌ Usually requires `.ToArray()` |

```cs
// Array — fixed size
int[] arr = new int[5];
arr[0] = 10;
// arr[5] = 60; // ❌ IndexOutOfRangeException

// List<T> — dynamic
var list = new List<int> { 1, 2, 3 };
list.Add(4);          // grows automatically
list.Insert(0, 0);    // [0, 1, 2, 3, 4]
list.Remove(2);       // [0, 1, 3, 4]
list.RemoveAt(0);     // [1, 3, 4]

Console.WriteLine(list.Count); // 3

// Convert between them
int[] fromList = list.ToArray();
List<int> fromArr = arr.ToList();

// Performance — pre-allocate List capacity to avoid re-allocations
var preAllocated = new List<int>(capacity: 1_000_000);
for (int i = 0; i < 1_000_000; i++)
    preAllocated.Add(i); // no re-allocations needed
```

**Decision:** Use `T[]` when size is fixed and performance/interop is critical. Use `List<T>` when you need dynamic sizing and rich collection APIs.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between Array and Collections?

| Aspect | Array (`T[]`) | Collections (e.g., `List<T>`, `Dictionary<K,V>`) |
|--------|--------------|--------------------------------------------------|
| Size | Fixed | Dynamic |
| Type | Single type, contiguous memory | Generic (strongly typed), backed by arrays/BSTs |
| Flexibility | Minimal — only index access | Rich API: search, sort, filter, thread-safe variants |
| Interfaces | `IList<T>`, `IEnumerable<T>` | Same + `ICollection<T>` and more |
| Overhead | Minimal | Small metadata overhead |
| Multi-dim | ✅ (`int[,]`) | ❌ (nest collections) |
| Nullability | Can hold nulls | Depends on type |

```cs
// Array — tight, fixed, minimal API
int[] arr = [10, 20, 30];
Console.WriteLine(arr.Length); // 3
// arr[3] = 40; // ❌ cannot resize

// List<T> — flexible, rich
var list = new List<int>([10, 20, 30]);
list.Add(40);
list.Remove(20);
list.Sort();
Console.WriteLine(list.Count); // 3

// Dictionary<K,V> — key-value, O(1) lookup
var dict = new Dictionary<string, int>
{
    ["one"] = 1, ["two"] = 2, ["three"] = 3,
};
Console.WriteLine(dict["two"]); // 2

// HashSet<T> — unique values, O(1) membership
var set = new HashSet<int> { 1, 2, 3, 2, 1 };
Console.WriteLine(set.Count); // 3 (duplicates removed)
```

**In summary:** Arrays are the primitive building block. Collections are higher-level abstractions that wrap arrays (or other structures) and add functionality. Most collections internally use arrays.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are generic collections?

**Generic collections** are type-parameterised collection classes in `System.Collections.Generic` that enforce type safety at compile time and avoid boxing/unboxing for value types. They were introduced in .NET 2.0 and replaced the non-generic collections (`ArrayList`, `Hashtable`, etc.).

**Benefits:**
- Compile-time type checking — no accidental mixing of types.
- Better performance — no boxing for value types.
- Cleaner API — all methods are typed.
- IntelliSense works correctly.

```cs
// Generic List<T>
var numbers = new List<int> { 1, 2, 3 };
numbers.Add(4);
// numbers.Add("text"); // ❌ compile error — type-safe

// Generic Dictionary<K,V>
var scores = new Dictionary<string, int>
{
    ["Alice"] = 95,
    ["Bob"]   = 87,
};
scores["Carol"] = 92;

if (scores.TryGetValue("Alice", out int score))
    Console.WriteLine($"Alice: {score}"); // Alice: 95

// Generic HashSet<T>
var unique = new HashSet<string> { "apple", "banana", "apple" };
Console.WriteLine(unique.Count); // 2

// Generic Queue<T> and Stack<T>
var queue = new Queue<int>();
queue.Enqueue(1); queue.Enqueue(2);
Console.WriteLine(queue.Dequeue()); // 1

var stack = new Stack<string>();
stack.Push("first"); stack.Push("second");
Console.WriteLine(stack.Pop()); // second

// Generic custom class
public class Repository<T> where T : class
{
    private readonly List<T> _items = [];
    public void Add(T item) => _items.Add(item);
    public IReadOnlyList<T> GetAll() => _items;
}

var repo = new Repository<string>();
repo.Add("hello");
repo.Add("world");
Console.WriteLine(repo.GetAll().Count); // 2
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can you optimize memory usage and performance when working with large data collections in C#?

**Key optimisations:**

**1. Pre-allocate capacity:**

```cs
// ❌ No capacity — triggers multiple re-allocations as list grows
var list = new List<int>();
for (int i = 0; i < 1_000_000; i++) list.Add(i);

// ✅ Pre-allocate — single backing array allocation
var list2 = new List<int>(capacity: 1_000_000);
for (int i = 0; i < 1_000_000; i++) list2.Add(i);
```

**2. Use `Span<T>` and `Memory<T>` for zero-allocation slicing:**

```cs
int[] arr = Enumerable.Range(0, 1_000_000).ToArray();

// ❌ Creates a new array copy
int[] slice = arr[100..200];

// ✅ Zero-copy view
ReadOnlySpan<int> span = arr.AsSpan(100, 100);
int sum = 0;
foreach (ref readonly int n in span) sum += n;
```

**3. Use `ArrayPool<T>` for temporary buffers:**

```cs
using System.Buffers;

// ❌ Allocates a new array each time (GC pressure)
void ProcessData(byte[] data) { /* ... */ }

// ✅ Rent from pool — no GC allocation
byte[] buffer = ArrayPool<byte>.Shared.Rent(4096);
try
{
    // use buffer
    ProcessData(buffer);
}
finally
{
    ArrayPool<byte>.Shared.Return(buffer); // return to pool
}
```

**4. Use `IEnumerable<T>` with `yield` for streaming (avoid loading all data):**

```cs
// ❌ Loads all 10M records into memory
List<int> LoadAll() => Enumerable.Range(0, 10_000_000).ToList();

// ✅ Streams one at a time — O(1) memory
IEnumerable<int> StreamAll()
{
    for (int i = 0; i < 10_000_000; i++)
        yield return i;
}

// Process without holding all in memory
foreach (var item in StreamAll().Where(n => n % 1000 == 0))
    Console.WriteLine(item);
```

**5. Use `FrozenDictionary` / `FrozenSet` for read-only lookup (.NET 8+):**

```cs
using System.Collections.Frozen;

// FrozenDictionary is optimised for frequent reads — faster than Dictionary for lookups
var lookup = new Dictionary<string, int> { ["a"] = 1, ["b"] = 2 }
    .ToFrozenDictionary();

// Very fast Contains/lookup — ideal for static configuration, keyword lists
Console.WriteLine(lookup.ContainsKey("a")); // True
```

**6. Use `LINQ Chunk()` to process in batches:**

```cs
var allItems = Enumerable.Range(1, 100_000);

foreach (var batch in allItems.Chunk(1000))
{
    // Process 1000 items at a time — limits peak memory
    await ProcessBatchAsync(batch);
}
```

**7. Avoid LINQ materialisation when not needed:**

```cs
// ❌ Materialises to List unnecessarily
var list = items.Where(x => x > 0).ToList().Count;

// ✅ Count without materialising
var count = items.Count(x => x > 0); // single pass, no intermediate list
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 6. MULTITHREADING

<br/>

## Q. What is multithreading in C# and why is it important?

**Multithreading** is the ability to execute multiple threads concurrently within a single process, enabling parallelism and better CPU utilization. In modern .NET, the preferred abstraction is `Task` and `async`/`await` (via the **Task Parallel Library, TPL**) rather than raw `Thread` management.

**Why it matters:**
- Improves responsiveness (UI stays fluid while background work runs).
- Maximizes CPU utilization on multi-core processors.
- Enables concurrent I/O (e.g., multiple HTTP requests simultaneously).

**1. `Task.Run` — run CPU-bound work on the thread pool:**

```cs
var result = await Task.Run(() =>
{
    // CPU-intensive work (runs on thread pool thread)
    return Enumerable.Range(1, 1_000_000).Sum();
});
Console.WriteLine(result); // Output: 500000500000
```

**2. `async`/`await` — non-blocking async I/O (preferred for I/O-bound):**

```cs
public async Task<string[]> FetchAllAsync(string[] urls)
{
    using var client = new HttpClient();
    var tasks = urls.Select(url => client.GetStringAsync(url));
    return await Task.WhenAll(tasks); // all in parallel
}
```

**3. `Parallel.ForEachAsync` (.NET 6+) — async parallel processing:**

```cs
var urls = new[] { "https://api1.example.com", "https://api2.example.com" };

await Parallel.ForEachAsync(urls,
    new ParallelOptions { MaxDegreeOfParallelism = 4 },
    async (url, ct) =>
    {
        using var client = new HttpClient();
        var data = await client.GetStringAsync(url, ct);
        Console.WriteLine($"Fetched {data.Length} chars from {url}");
    });
```

**4. Thread-safe shared state with `Interlocked`:**

```cs
int counter = 0;
await Task.WhenAll(Enumerable.Range(0, 100).Select(_ =>
    Task.Run(() => Interlocked.Increment(ref counter))));
Console.WriteLine(counter); // Output: 100 (always correct)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Multithreading with .NET, and what is a thread in C#?

A **thread** is the smallest unit of execution within a process. A **process** can have multiple threads running concurrently, sharing the same memory space.

**Multithreading** is the ability to run multiple threads simultaneously to perform work in parallel, improving responsiveness and throughput.

In .NET, threads are managed by the **CLR** and scheduled by the **OS**. Modern .NET (5+) recommends using `Task` and `async/await` over raw `Thread` for most scenarios.

```cs
// A thread in .NET = lightweight unit of execution
Console.WriteLine($"Main thread ID: {Thread.CurrentThread.ManagedThreadId}");
Console.WriteLine($"Is background: {Thread.CurrentThread.IsBackground}");
Console.WriteLine($"Is thread pool: {Thread.CurrentThread.IsThreadPoolThread}");
Console.WriteLine($"State: {Thread.CurrentThread.ThreadState}");
```

**Ways to implement multithreading in .NET 10:**

```cs
// 1. Thread (low-level — use only for dedicated long-running work)
var t = new Thread(() => Console.WriteLine($"Thread {Thread.CurrentThread.ManagedThreadId}"));
t.IsBackground = true;
t.Start();
t.Join();

// 2. ThreadPool (managed pool — underlying mechanism for Tasks)
ThreadPool.QueueUserWorkItem(_ => Console.WriteLine("ThreadPool work item"));

// 3. Task (preferred — async, return values, exception propagation)
await Task.Run(() => Console.WriteLine("Task on thread pool"));

// 4. Parallel class (data parallelism)
Parallel.For(0, 4, i => Console.WriteLine($"Parallel item {i}"));

// 5. async/await (I/O-bound work without blocking threads)
async Task<string> FetchDataAsync(string url)
{
    using var client = new HttpClient();
    return await client.GetStringAsync(url);
}

// 6. PLINQ (parallel LINQ)
var results = Enumerable.Range(1, 100).AsParallel().Where(n => n % 2 == 0).ToList();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between a thread and a process?

| | **Process** | **Thread** |
|-|------------|-----------|
| Definition | An isolated running instance of a program | A unit of execution within a process |
| Memory | Has its own address space | Shares the process address space |
| Communication | IPC (pipes, sockets, shared memory) | Shared memory — fast but needs synchronization |
| Isolation | Crash in one process doesn\'t affect others | Crash in one thread can crash the whole process |
| Creation cost | High (separate memory, handles, etc.) | Lower (shares process resources) |
| Switching cost | Expensive (context switch across processes) | Less expensive (same address space) |

```cs
// Process info
var current = System.Diagnostics.Process.GetCurrentProcess();
Console.WriteLine($"PID: {current.Id}");
Console.WriteLine($"Name: {current.ProcessName}");
Console.WriteLine($"Threads: {current.Threads.Count}");
Console.WriteLine($"Memory: {current.WorkingSet64 / 1024 / 1024} MB");

// Spawn a child process
using var proc = System.Diagnostics.Process.Start(new System.Diagnostics.ProcessStartInfo
{
    FileName  = "dotnet",
    Arguments = "--version",
    RedirectStandardOutput = true,
    UseShellExecute = false,
});
await proc!.WaitForExitAsync();
Console.WriteLine(await proc.StandardOutput.ReadToEndAsync());

// Thread info
var thread = new Thread(() =>
{
    Console.WriteLine($"Thread ID: {Thread.CurrentThread.ManagedThreadId}");
    Console.WriteLine($"Is pool: {Thread.CurrentThread.IsThreadPoolThread}");
});
thread.Start();
thread.Join();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a new thread in C#?

```cs
// 1. Thread with ThreadStart delegate (no parameters)
var t1 = new Thread(DoWork);
t1.Name         = "WorkerThread";
t1.IsBackground = true; // daemon — terminates when main thread exits
t1.Priority     = ThreadPriority.Normal;
t1.Start();
t1.Join(); // block caller until t1 finishes

void DoWork() => Console.WriteLine($"Running on thread {Thread.CurrentThread.ManagedThreadId}");

// 2. Thread with lambda
var t2 = new Thread(() =>
{
    Console.WriteLine("Lambda thread");
    Thread.Sleep(100); // simulate work
});
t2.Start();

// 3. ParameterizedThreadStart — pass a single object parameter
var t3 = new Thread(param =>
{
    string msg = (string)param!;
    Console.WriteLine($"Message: {msg}");
});
t3.Start("Hello from parameter");

// 4. Type-safe parameter passing via closure (preferred over ParameterizedThreadStart)
int workerId = 42;
string taskName = "ImportJob";
var t4 = new Thread(() =>
{
    // captures workerId and taskName — fully type-safe
    Console.WriteLine($"Worker {workerId}: {taskName}");
});
t4.Start();

// 5. Foreground vs background threads
// Foreground (default): app stays alive until ALL foreground threads finish
// Background: app can exit even if background threads are still running
var fg = new Thread(() => Thread.Sleep(5000)) { IsBackground = false }; // keeps app alive
var bg = new Thread(() => Thread.Sleep(5000)) { IsBackground = true  }; // doesn\'t block exit

// 6. Preferred modern alternative: Task.Run
await Task.Run(() => Console.WriteLine("Preferred: Task on thread pool"));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Why does a delegate need to be passed to the Thread constructor, and how do you pass parameters type-safely?

The `Thread` constructor requires a **delegate** (`ThreadStart` or `ParameterizedThreadStart`) because a thread needs to know *which method to execute*. The delegate is the entry point.

```cs
// ThreadStart — no parameters, no return value
ThreadStart start = DoWork;
var t1 = new Thread(start);
t1.Start();

void DoWork() => Console.WriteLine("No params");

// ParameterizedThreadStart — one object parameter (not type-safe)
ParameterizedThreadStart paramStart = obj =>
{
    int value = (int)obj!; // manual cast — runtime error if wrong type
    Console.WriteLine($"Value: {value}");
};
var t2 = new Thread(paramStart);
t2.Start(100); // pass object

// ✅ Type-safe approach — closure over strongly-typed variables
int id    = 7;
string name = "Alice";
var t3 = new Thread(() =>
{
    // id and name captured by reference — fully type-safe, no casting
    Console.WriteLine($"Worker {id}: {name}");
});
t3.Start();

// ✅ Pass a typed object via closure
record WorkItem(int Id, string Name, DateTime Due);
var item = new WorkItem(1, "Report", DateTime.Today);
var t4 = new Thread(() =>
{
    Console.WriteLine($"Processing {item.Name} (due {item.Due:d})");
});
t4.Start();
t4.Join();

// Retrieving data from a thread — use a shared variable + lock, or Task<T>
int result = 0;
var t5 = new Thread(() => result = Compute()); // write result inside thread
t5.Start();
t5.Join();
Console.WriteLine($"Result: {result}"); // safe to read after Join()

int Compute() => 42;

// ✅ Preferred: Task<T> — return values built-in, no shared variable needed
int taskResult = await Task.Run(() => Compute());
Console.WriteLine($"Task result: {taskResult}");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `Thread.Join` and `Thread.Sleep`? What are `Thread.IsAlive` and `Thread.Join`?

| | `Thread.Join` | `Thread.Sleep` |
|-|--------------|---------------|
| **Blocks** | The **calling** thread | The **current** thread |
| **Until** | The target thread finishes | The timeout elapses |
| **Purpose** | Wait for another thread | Pause execution temporarily |
| **Returns** | `bool` (overload with timeout) | `void` |

```cs
var worker = new Thread(() =>
{
    Console.WriteLine("Worker started");
    Thread.Sleep(500); // pause this thread for 500 ms
    Console.WriteLine("Worker done");
});

worker.Start();
Console.WriteLine($"Worker alive: {worker.IsAlive}"); // true

// Join() — main thread blocks here until worker finishes
bool finished = worker.Join(timeout: TimeSpan.FromSeconds(2));
Console.WriteLine($"Finished in time: {finished}");   // true
Console.WriteLine($"Worker alive: {worker.IsAlive}"); // false

// Thread.Sleep(0) — yield to other threads of equal or higher priority
Thread.Sleep(0);

// Thread.Sleep(Timeout.Infinite) — sleep until interrupted
// Thread.Interrupt() — throws ThreadInterruptedException in sleeping/waiting thread

// IsAlive — true after Start() and before the thread method returns
var t = new Thread(() => Thread.Sleep(200));
Console.WriteLine(t.IsAlive); // false — not started yet
t.Start();
Console.WriteLine(t.IsAlive); // true  — running
t.Join();
Console.WriteLine(t.IsAlive); // false — completed
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the different states of a Thread in C#?

`Thread.ThreadState` is a flags enum — a thread can be in multiple states simultaneously.

| State | Meaning |
|-------|---------|
| `Unstarted` | Created but `Start()` not yet called |
| `Running` | Actively executing |
| `WaitSleepJoin` | Blocked in `Sleep`, `Wait`, `Join`, or a lock |
| `Background` | `IsBackground = true` |
| `Stopped` | Completed or terminated |
| `AbortRequested` | `Abort()` was called (removed in .NET Core) |
| `Suspended` | `Suspend()` was called (removed in .NET Core) |

```cs
var t = new Thread(() =>
{
    Console.WriteLine("Working...");
    Thread.Sleep(300);
});

Console.WriteLine(t.ThreadState); // Unstarted
t.Start();
Console.WriteLine(t.ThreadState); // Running | (possibly Background)
Thread.Sleep(50);
Console.WriteLine(t.ThreadState); // WaitSleepJoin
t.Join();
Console.WriteLine(t.ThreadState); // Stopped

// Prefer checking IsAlive over ThreadState for simple checks
// ThreadState is mostly useful for diagnostics
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `ThreadPool` class and how is it used?

The **ThreadPool** is a pool of pre-created worker threads managed by the CLR. It avoids the overhead of creating and destroying threads for each short-lived task.

```cs
// 1. QueueUserWorkItem — fire and forget (avoid in modern code)
ThreadPool.QueueUserWorkItem(_ => Console.WriteLine("Pool work item"));

// 2. Get/set pool limits
ThreadPool.GetMinThreads(out int minWorker, out int minIo);
ThreadPool.GetMaxThreads(out int maxWorker, out int maxIo);
Console.WriteLine($"Min workers: {minWorker}, Max workers: {maxWorker}");

// Set minimum threads (pre-warm the pool to avoid ramp-up latency)
ThreadPool.SetMinThreads(workerThreads: 8, completionPortThreads: 8);

// 3. Task.Run — the modern way to queue work on the thread pool
var task = Task.Run(() =>
{
    Console.WriteLine($"Pool thread: {Thread.CurrentThread.IsThreadPoolThread}"); // true
    return 42;
});
int result = await task;

// 4. Parallel.ForEach — distributes iterations across pool threads
Parallel.ForEach(Enumerable.Range(1, 10), i =>
    Console.WriteLine($"Item {i} on thread {Thread.CurrentThread.ManagedThreadId}"));

// 5. Long-running work should NOT use the thread pool
// Use TaskCreationOptions.LongRunning to get a dedicated thread instead
var longTask = Task.Factory.StartNew(() =>
{
    while (true) { /* background service */ Thread.Sleep(1000); }
}, TaskCreationOptions.LongRunning);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are `Task` and `async/await` in C#?

A **`Task`** represents an asynchronous operation that may return a value (`Task<T>`). **`async/await`** is syntactic sugar that lets you write asynchronous code in a sequential, readable style.

```cs
// Task — represents an ongoing or completed operation
Task t = Task.Run(() => Console.WriteLine("Fire and forget"));
Task<int> t2 = Task.Run(() => 42);
int value = await t2; // await suspends the caller, not the thread

// async/await — I/O-bound (no thread blocked)
async Task<string> GetDataAsync(string url)
{
    using var client = new HttpClient();
    return await client.GetStringAsync(url); // no thread blocked during HTTP call
}

// CPU-bound — offload to thread pool via Task.Run
async Task<int> ComputeAsync(int n)
{
    return await Task.Run(() =>
    {
        int sum = 0;
        for (int i = 0; i < n; i++) sum += i;
        return sum;
    });
}

// Run multiple tasks concurrently
var tasks = new[] { GetDataAsync("https://httpbin.org/get"), GetDataAsync("https://example.com") };
string[] results = await Task.WhenAll(tasks);

// Task.WhenAny — proceed when the first completes
Task<string> first = await Task.WhenAny(tasks);
Console.WriteLine("First done");

// Return types
// Task        — async void equivalent (no result)
// Task<T>     — async with result
// ValueTask<T>— struct, avoids heap alloc for hot paths that often complete synchronously
// IAsyncEnumerable<T> — async stream

async IAsyncEnumerable<int> GenerateAsync()
{
    for (int i = 0; i < 5; i++)
    {
        await Task.Delay(100);
        yield return i;
    }
}

await foreach (int n in GenerateAsync())
    Console.Write($"{n} "); // 0 1 2 3 4
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `Task.Run` and `Task.Factory.StartNew`?

| | `Task.Run` | `Task.Factory.StartNew` |
|-|-----------|------------------------|
| **Introduced** | .NET 4.5 | .NET 4.0 |
| **Unwraps nested tasks** | ✅ Automatically | ❌ Must call `.Unwrap()` manually |
| **Default scheduler** | `ThreadPool` | Current `TaskScheduler` |
| **LongRunning option** | ❌ Not supported | ✅ `TaskCreationOptions.LongRunning` |
| **Recommended for** | CPU-bound short tasks | Long-running or custom scheduler tasks |
| **Simplicity** | Simpler, safer | More flexible but verbose |

```cs
// Task.Run — preferred for CPU-bound work on the thread pool
int result = await Task.Run(() =>
{
    int sum = Enumerable.Range(1, 1_000_000).Sum();
    return sum;
});
Console.WriteLine(result);

// Task.Factory.StartNew — needed for LongRunning
var longTask = Task.Factory.StartNew(() =>
{
    while (true)
    {
        Console.WriteLine("Background service tick");
        Thread.Sleep(1000);
    }
}, TaskCreationOptions.LongRunning | TaskCreationOptions.DenyChildAttach);

// Task.Run + async lambda — automatically unwraps Task<Task>
int asyncResult = await Task.Run(async () =>
{
    await Task.Delay(100);
    return 42;
});

// Task.Factory.StartNew + async lambda — must unwrap manually
int manualResult = await await Task.Factory.StartNew(async () =>
{
    await Task.Delay(100);
    return 42;
}); // double-await because StartNew returns Task<Task<int>>

// Custom TaskScheduler (advanced — e.g., UI thread, limited concurrency)
var scheduler = new LimitedConcurrencyLevelTaskScheduler(maxDegreeOfParallelism: 2);
var factory   = new TaskFactory(scheduler);
await factory.StartNew(() => Console.WriteLine("Limited concurrency task"));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle exceptions in multithreaded applications?

```cs
// 1. await — exceptions propagate naturally
async Task ProcessAsync()
{
    try
    {
        await Task.Run(() => throw new InvalidOperationException("Task error"));
    }
    catch (InvalidOperationException ex)
    {
        Console.WriteLine($"Caught: {ex.Message}");
    }
}
await ProcessAsync();

// 2. Task.WhenAll — AggregateException wraps all exceptions
var tasks = new[]
{
    Task.Run(() => throw new Exception("Error 1")),
    Task.Run(() => throw new Exception("Error 2")),
    Task.Run(() => Console.WriteLine("OK")),
};
try
{
    await Task.WhenAll(tasks);
}
catch // await unwraps first exception
{
    // Inspect all exceptions via the tasks themselves
    foreach (var t in tasks.Where(t => t.IsFaulted))
        Console.WriteLine(t.Exception!.InnerException!.Message);
}

// 3. Unhandled exceptions on raw Thread — must catch inside the thread
var thread = new Thread(() =>
{
    try
    {
        throw new Exception("Thread crash");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Thread caught: {ex.Message}");
    }
});
thread.Start();

// 4. Global unhandled exception handler (last resort)
AppDomain.CurrentDomain.UnhandledException += (_, e) =>
    Console.WriteLine($"Unhandled: {(e.ExceptionObject as Exception)?.Message}");

TaskScheduler.UnobservedTaskException += (_, e) =>
{
    Console.WriteLine($"Unobserved task exception: {e.Exception.Message}");
    e.SetObserved(); // prevent crash
};

// 5. CancellationToken — not an exception per se, but related
var cts = new CancellationTokenSource();
try
{
    await Task.Run(() =>
    {
        cts.Token.ThrowIfCancellationRequested();
    }, cts.Token);
}
catch (OperationCanceledException)
{
    Console.WriteLine("Task was cancelled");
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a deadlock and how can it be avoided? What are the four necessary conditions for deadlock?

A **deadlock** occurs when two or more threads are permanently blocked, each waiting for a resource held by the other.

**Four necessary conditions (Coffman conditions):**

| Condition | Meaning |
|-----------|---------|
| **Mutual Exclusion** | A resource is held exclusively by one thread |
| **Hold and Wait** | A thread holds a resource while waiting for another |
| **No Preemption** | Resources cannot be forcibly taken away |
| **Circular Wait** | Thread A waits for Thread B, which waits for Thread A |

```cs
// Classic deadlock — two threads lock in opposite orders
object lockA = new(), lockB = new();

var t1 = new Thread(() =>
{
    lock (lockA) { Thread.Sleep(50); lock (lockB) { Console.WriteLine("T1 done"); } }
});
var t2 = new Thread(() =>
{
    lock (lockB) { Thread.Sleep(50); lock (lockA) { Console.WriteLine("T2 done"); } }
});
// t1.Start(); t2.Start(); // would deadlock!

// Prevention strategies:

// 1. Consistent lock ordering — always acquire locks in the same order
var t3 = new Thread(() => { lock (lockA) { lock (lockB) { Console.WriteLine("T3 done"); } } });
var t4 = new Thread(() => { lock (lockA) { lock (lockB) { Console.WriteLine("T4 done"); } } });
t3.Start(); t4.Start();

// 2. Monitor.TryEnter with timeout — fail fast instead of blocking forever
bool got = false;
Monitor.TryEnter(lockA, TimeSpan.FromSeconds(1), ref got);
if (got)
{
    try { /* work */ }
    finally { Monitor.Exit(lockA); }
}
else
{
    Console.WriteLine("Could not acquire lock — skip or retry");
}

// 3. Avoid nested locks — redesign to use a single lock or lock-free structures

// 4. Use async/await — no thread is blocked waiting; deadlock risk eliminated
await Task.Run(() => { /* lock-free async work */ });
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is LiveLock?

A **livelock** occurs when two or more threads keep reacting to each other\'s actions — they are actively executing but making no progress. Unlike a deadlock, threads are not blocked; they just keep changing state in response to each other indefinitely.

```cs
// Simulated livelock — two threads keep "politely yielding" to each other
int sharedFlag = 0;
bool thread1Done = false, thread2Done = false;

var t1 = new Thread(() =>
{
    while (!thread1Done)
    {
        if (Interlocked.CompareExchange(ref sharedFlag, 1, 0) == 0)
        {
            Console.WriteLine("T1: doing work");
            Thread.Sleep(50);
            Interlocked.Exchange(ref sharedFlag, 0);
            thread1Done = true;
        }
        else
        {
            Console.WriteLine("T1: yielding"); // keeps yielding to T2
            Thread.Sleep(10);
        }
    }
});

var t2 = new Thread(() =>
{
    while (!thread2Done)
    {
        if (Interlocked.CompareExchange(ref sharedFlag, 2, 0) == 0)
        {
            Console.WriteLine("T2: doing work");
            Thread.Sleep(50);
            Interlocked.Exchange(ref sharedFlag, 0);
            thread2Done = true;
        }
        else
        {
            Console.WriteLine("T2: yielding"); // keeps yielding to T1
            Thread.Sleep(10);
        }
    }
});

// Prevention:
// - Add randomized back-off delays (Thread.Sleep(Random.Next(10, 100)))
// - Use a priority scheme — one thread gets precedence
// - Use proper lock-free algorithms (e.g., Interlocked operations)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `lock` statement in C#? What is the difference between `lock` and `Interlocked`?

The `lock` statement ensures **mutual exclusion** — only one thread can execute a guarded block at a time. It is syntactic sugar over `Monitor.Enter` / `Monitor.Exit`.

```cs
public class SafeCounter
{
    private readonly object _syncRoot = new();
    private int _count;

    public void Increment()
    {
        lock (_syncRoot) // only one thread at a time
        {
            _count++;
        }
    }

    public int Count
    {
        get { lock (_syncRoot) { return _count; } }
    }
}

var counter = new SafeCounter();
await Task.WhenAll(Enumerable.Range(0, 1000).Select(_ =>
    Task.Run(counter.Increment)));
Console.WriteLine(counter.Count); // always 1000

// What lock compiles to:
// Monitor.Enter(obj, ref lockTaken);
// try { ... } finally { if (lockTaken) Monitor.Exit(obj); }

// ⚠️ Rules:
// - Lock on a private readonly object, never on 'this', string literals, or Type objects
// - Keep locked sections short
// - Never call unknown code inside a lock (can cause deadlock)
```

**`lock` vs `Interlocked`:**

| | `lock` | `Interlocked` |
|-|--------|--------------|
| **Use case** | Guard multi-statement critical sections | Atomic operations on single variables |
| **Overhead** | Higher (OS kernel object) | Very low (CPU atomic instruction) |
| **Operations** | Any code | `Increment`, `Decrement`, `Add`, `Exchange`, `CompareExchange`, `Read` |

```cs
// Interlocked — atomic operations, no lock needed for single-variable updates
int value = 0;

await Task.WhenAll(Enumerable.Range(0, 1000).Select(_ =>
    Task.Run(() => Interlocked.Increment(ref value))));
Console.WriteLine(value); // always 1000

// CompareExchange — optimistic locking / spin loop
int current, updated;
do
{
    current = value;
    updated = current + 10;
} while (Interlocked.CompareExchange(ref value, updated, current) != current);
Console.WriteLine(value);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `Monitor` and `lock` in C#? How do you use the `Monitor` class?

`lock` is shorthand for `Monitor.Enter`/`Monitor.Exit`. `Monitor` gives you additional control: `TryEnter` with timeout, `Wait`, `Pulse`, and `PulseAll` for thread signalling.

```cs
object sync = new();

// lock (compiles to Monitor internally)
lock (sync) { /* critical section */ }

// Monitor.Enter / Exit — explicit equivalent of lock
bool lockTaken = false;
try
{
    Monitor.Enter(sync, ref lockTaken);
    // critical section
}
finally
{
    if (lockTaken) Monitor.Exit(sync);
}

// Monitor.TryEnter — non-blocking, with timeout
bool acquired = Monitor.TryEnter(sync, TimeSpan.FromMilliseconds(500));
if (acquired)
{
    try { /* work */ }
    finally { Monitor.Exit(sync); }
}

// Monitor.Wait / Pulse — producer-consumer signalling
object buffer = new();
Queue<int> queue = new();

var producer = new Thread(() =>
{
    for (int i = 0; i < 5; i++)
    {
        lock (buffer)
        {
            queue.Enqueue(i);
            Console.WriteLine($"Produced: {i}");
            Monitor.Pulse(buffer); // wake one waiting thread
        }
        Thread.Sleep(100);
    }
});

var consumer = new Thread(() =>
{
    for (int i = 0; i < 5; i++)
    {
        lock (buffer)
        {
            while (queue.Count == 0)
                Monitor.Wait(buffer); // releases lock + waits for Pulse

            int item = queue.Dequeue();
            Console.WriteLine($"Consumed: {item}");
        }
    }
});

consumer.Start(); producer.Start();
consumer.Join(); producer.Join();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the Race condition?

A **race condition** occurs when the outcome of a program depends on the timing or ordering of thread execution. When two or more threads access shared data concurrently and at least one modifies it, without proper synchronization, the result is unpredictable.

```cs
// Race condition — unsynchronized increment
int counter = 0;

var tasks = Enumerable.Range(0, 1000)
    .Select(_ => Task.Run(() => counter++)) // NOT atomic: read + add + write
    .ToArray();
await Task.WhenAll(tasks);
Console.WriteLine(counter); // may be < 1000 — race condition!

// Strategies to prevent race conditions:

// 1. lock — guard the critical section
int safeCounter = 0;
object sync = new();
await Task.WhenAll(Enumerable.Range(0, 1000)
    .Select(_ => Task.Run(() => { lock (sync) safeCounter++; })));
Console.WriteLine(safeCounter); // always 1000

// 2. Interlocked — atomic update for simple variables
int atomicCounter = 0;
await Task.WhenAll(Enumerable.Range(0, 1000)
    .Select(_ => Task.Run(() => Interlocked.Increment(ref atomicCounter))));
Console.WriteLine(atomicCounter); // always 1000

// 3. Concurrent collections — thread-safe without manual locking
var bag = new System.Collections.Concurrent.ConcurrentBag<int>();
await Task.WhenAll(Enumerable.Range(0, 1000)
    .Select(i => Task.Run(() => bag.Add(i))));
Console.WriteLine(bag.Count); // always 1000

// 4. Immutable data / local variables — no sharing = no race
var results = await Task.WhenAll(
    Enumerable.Range(1, 4).Select(i => Task.Run(() => i * i)));
Console.WriteLine(string.Join(", ", results)); // 1, 4, 9, 16
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What happens if shared resources are not protected from concurrent access? How do you protect shared resources?

**Without protection:** data corruption, torn reads/writes, stale caches, non-deterministic results.

```cs
// Unprotected — torn write (int64 may not be atomically written on 32-bit)
long shared = 0;
// multiple threads writing concurrently = undefined behavior

// Protection options (choose based on scenario):

// 1. lock — simplest, general purpose
private readonly object _lock = new();
private int _state;
public void Update(int value) { lock (_lock) { _state = value; } }
public int  Read()            { lock (_lock) { return _state;  } }

// 2. Interlocked — atomic single-variable ops (fastest)
private int _count;
public void Increment() => Interlocked.Increment(ref _count);
public int  Count       => Interlocked.CompareExchange(ref _count, 0, 0); // atomic read

// 3. ReaderWriterLockSlim — multiple readers OR one writer
private readonly ReaderWriterLockSlim _rwLock = new();
private Dictionary<int, string> _cache = new();

public string? Get(int key)
{
    _rwLock.EnterReadLock();
    try { return _cache.TryGetValue(key, out var v) ? v : null; }
    finally { _rwLock.ExitReadLock(); }
}
public void Set(int key, string val)
{
    _rwLock.EnterWriteLock();
    try { _cache[key] = val; }
    finally { _rwLock.ExitWriteLock(); }
}

// 4. Concurrent collections — thread-safe without explicit locks
var dict  = new System.Collections.Concurrent.ConcurrentDictionary<int, string>();
var queue = new System.Collections.Concurrent.ConcurrentQueue<int>();
var stack = new System.Collections.Concurrent.ConcurrentStack<int>();
var bag   = new System.Collections.Concurrent.ConcurrentBag<int>();

// 5. Volatile — prevent CPU/compiler reordering for simple flags
private volatile bool _running = true;
public void Stop() => _running = false; // visible immediately to all threads

// 6. Channels (System.Threading.Channels) — async-friendly message passing
var channel = System.Threading.Channels.Channel.CreateBounded<int>(100);
await channel.Writer.WriteAsync(42);
int item = await channel.Reader.ReadAsync();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is synchronization and why is it important? Can you name the synchronization primitives in .NET?

**Synchronization** is the coordination of threads to ensure correct access to shared resources, prevent race conditions, and establish ordering guarantees.

**Why it matters:** Without synchronization, concurrent threads can produce corrupted data, deadlocks, or non-deterministic behaviour.

**Synchronization primitives in .NET:**

| Primitive | Use case |
|-----------|---------|
| `lock` / `Monitor` | Mutual exclusion for any code block |
| `Mutex` | Cross-process mutual exclusion |
| `Semaphore` / `SemaphoreSlim` | Limit concurrent access to N threads |
| `ManualResetEvent` / `ManualResetEventSlim` | Signal multiple waiting threads at once |
| `AutoResetEvent` | Signal one waiting thread, then auto-reset |
| `CountdownEvent` | Wait until N operations have completed |
| `Barrier` | Synchronize N threads at a phase boundary |
| `ReaderWriterLockSlim` | Multiple readers / exclusive writer |
| `SpinLock` | Busy-wait for very short critical sections |
| `SpinWait` | Spinning with back-off before yielding |
| `Interlocked` | Atomic operations on primitive variables |
| `volatile` | Visibility guarantee for simple flags |
| `SemaphoreSlim` (async) | `WaitAsync()` — async-friendly throttling |
| `Channel<T>` | Async-safe producer/consumer messaging |

```cs
// Choosing the right primitive:
// Short critical section on same machine → lock
// Need timeout / TryEnter             → Monitor.TryEnter
// Limit concurrency (e.g., DB pool)   → SemaphoreSlim
// Signal all waiting threads           → ManualResetEventSlim
// Signal one thread, auto-reset        → AutoResetEvent
// Count-down to zero                   → CountdownEvent
// Phase-by-phase parallel work         → Barrier
// Concurrent reads, rare writes        → ReaderWriterLockSlim
// Nanosecond-critical inner loops      → SpinLock
// Cross-process lock                   → Mutex

using var slim = new SemaphoreSlim(initialCount: 3, maxCount: 3);
var tasks = Enumerable.Range(0, 10).Select(async i =>
{
    await slim.WaitAsync();
    try
    {
        Console.WriteLine($"Task {i} running (max 3 concurrent)");
        await Task.Delay(200);
    }
    finally { slim.Release(); }
});
await Task.WhenAll(tasks);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is AutoResetEvent and how is it different from ManualResetEvent?

Both derive from `EventWaitHandle` and allow threads to signal each other.

| | `AutoResetEvent` | `ManualResetEvent` / `ManualResetEventSlim` |
|-|-----------------|---------------------------------------------|
| **Reset** | Automatically after releasing **one** waiting thread | Must call `Reset()` manually |
| **Releases** | Exactly **one** thread per `Set()` call | **All** waiting threads when `Set()` is called |
| **State** | Like a turnstile — one thread passes, gate closes | Like a gate — open for all until closed |
| **Use case** | Worker thread signalling (one-at-a-time) | Broadcast event (all threads proceed) |

```cs
// AutoResetEvent — one producer signals one consumer at a time
using var are = new AutoResetEvent(initialState: false);

var producer = new Thread(() =>
{
    for (int i = 0; i < 3; i++)
    {
        Thread.Sleep(300);
        Console.WriteLine($"Produced {i}");
        are.Set(); // releases exactly one waiting thread
    }
});

var consumer = new Thread(() =>
{
    for (int i = 0; i < 3; i++)
    {
        are.WaitOne(); // blocks until Set() — auto-resets after waking
        Console.WriteLine($"Consumed {i}");
    }
});

producer.Start(); consumer.Start();
producer.Join();  consumer.Join();

// ManualResetEventSlim — broadcast to ALL waiting threads
using var mre = new ManualResetEventSlim(initialState: false);

var workers = Enumerable.Range(0, 4).Select(i => new Thread(() =>
{
    mre.Wait(); // all four threads block here
    Console.WriteLine($"Worker {i} released");
})).ToList();

workers.ForEach(w => w.Start());
Thread.Sleep(200);
mre.Set();  // releases ALL four workers simultaneously
mre.Reset(); // close gate again for next round
workers.ForEach(w => w.Join());
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the Semaphore? What is Mutex and how does it differ from other synchronization mechanisms?

**Semaphore** limits how many threads can access a resource simultaneously. `SemaphoreSlim` is the lightweight, async-friendly version recommended for most in-process scenarios.

**Mutex** is like a `lock` but works **across processes** and is owned by the thread that acquired it.

```cs
// SemaphoreSlim — limit concurrency to N threads (async-friendly)
using var sem = new SemaphoreSlim(initialCount: 2, maxCount: 2);

var tasks = Enumerable.Range(0, 6).Select(async i =>
{
    await sem.WaitAsync();
    try
    {
        Console.WriteLine($"  [{i}] entered (max 2 concurrent)");
        await Task.Delay(300);
        Console.WriteLine($"  [{i}] leaving");
    }
    finally { sem.Release(); }
});
await Task.WhenAll(tasks);

// Semaphore (kernel-level, cross-thread/process)
using var kernelSem = new Semaphore(initialCount: 1, maximumCount: 1, name: "MyAppSemaphore");
kernelSem.WaitOne();
try { /* exclusive access */ }
finally { kernelSem.Release(); }

// Mutex — cross-process mutual exclusion
using var mutex = new Mutex(initiallyOwned: false, name: "Global\\MyAppMutex");

// Single-instance app pattern
bool createdNew;
using var singleInstance = new Mutex(initiallyOwned: true, name: "Global\\MyApp", createdNew: out createdNew);
if (!createdNew)
{
    Console.WriteLine("Another instance is already running.");
    return;
}
// Only one instance reaches here
```

**Comparison:**

| | `lock` | `Mutex` | `SemaphoreSlim` |
|-|--------|---------|----------------|
| **Scope** | In-process | Cross-process | In-process |
| **Max holders** | 1 | 1 | N (configurable) |
| **Async** | ❌ | ❌ | ✅ `WaitAsync` |
| **Overhead** | Low | High (kernel) | Low |
| **Thread-affinity** | Yes | Yes | No |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `volatile` keyword?

`volatile` tells the compiler and CPU that a field may be changed by multiple threads, preventing **caching** of the variable in a CPU register and disabling certain compiler/CPU **reordering** optimisations.

```cs
// Without volatile — compiler may cache _running in a register
// and the loop never sees the update from another thread
public class Processor
{
    private volatile bool _running = true; // volatile ensures visibility

    public void Run()
    {
        while (_running) // reads from memory each iteration, not a register
        {
            // process work...
        }
        Console.WriteLine("Stopped cleanly");
    }

    public void Stop() => _running = false; // immediately visible to Run()
}

// volatile is appropriate for:
// - Simple flags (bool, int, reference)
// - Sentinel values checked in a spin loop

// volatile is NOT appropriate for:
// - Compound operations (check + set, read + increment) — use Interlocked or lock
// - Complex objects — use lock or Concurrent collections

// Difference: volatile vs Interlocked vs lock
//   volatile: prevents caching/reordering; does NOT make compound ops atomic
//   Interlocked: atomic operations on single primitives (Increment, CompareExchange)
//   lock: exclusive section — any code, any type, highest overhead

// Thread.MemoryBarrier — explicit full memory fence (advanced, rarely needed)
private int _data;
private volatile bool _ready;

public void Producer()
{
    _data = 42;
    Thread.MemoryBarrier(); // ensure _data write is visible before _ready write
    _ready = true;
}

public int Consumer()
{
    while (!_ready) Thread.SpinWait(1);
    Thread.MemoryBarrier();
    return _data; // guaranteed to see 42
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the Interlocked functions?

`Interlocked` provides **atomic** operations on shared variables — safe without `lock` and with minimal overhead (single CPU instruction).

```cs
int counter = 0;
long total   = 0;

// Increment / Decrement — thread-safe ++ and --
Interlocked.Increment(ref counter);       // counter++
Interlocked.Decrement(ref counter);       // counter--
Console.WriteLine(counter);               // 0

// Add — thread-safe +=
Interlocked.Add(ref counter, 10);
Console.WriteLine(counter);               // 10

// Exchange — atomically sets value, returns old value
int previous = Interlocked.Exchange(ref counter, 100);
Console.WriteLine($"Was {previous}, now {counter}"); // Was 10, now 100

// CompareExchange — atomically: if (counter == expected) counter = newValue
// Returns the original value
int original = Interlocked.CompareExchange(ref counter, newValue: 200, comparand: 100);
Console.WriteLine($"Original: {original}, Counter: {counter}"); // Original: 100, Counter: 200

// Read — atomic read of a long on 32-bit systems
long atomicRead = Interlocked.Read(ref total);

// Or (C# 9+)
Interlocked.Or(ref counter,  0b1111); // bitwise OR
Interlocked.And(ref counter, 0b1010); // bitwise AND

// Practical: lock-free spin-based update
int value = 0;
int current, newVal;
do
{
    current = value;
    newVal  = current * 2 + 1;
} while (Interlocked.CompareExchange(ref value, newVal, current) != current);
Console.WriteLine(value); // 1

// Practical: reference swap
string? sharedRef = "initial";
string? old = Interlocked.Exchange(ref sharedRef, "updated");
Console.WriteLine($"Was '{old}', now '{sharedRef}'");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can you share data between multiple threads?

```cs
// 1. Shared field with lock — simplest and most common
public class SharedState
{
    private readonly object _lock = new();
    private List<string> _items = [];

    public void Add(string item)    { lock (_lock) { _items.Add(item); } }
    public List<string> Snapshot()  { lock (_lock) { return [.._items]; } }
}

// 2. Concurrent collections — no manual lock needed
var dict  = new System.Collections.Concurrent.ConcurrentDictionary<string, int>();
var queue = new System.Collections.Concurrent.ConcurrentQueue<string>();
var bag   = new System.Collections.Concurrent.ConcurrentBag<int>();

await Task.WhenAll(
    Task.Run(() => dict.TryAdd("key1", 1)),
    Task.Run(() => dict.TryAdd("key2", 2)));

// 3. Channel<T> — async-safe producer/consumer (preferred in .NET 5+)
var channel = System.Threading.Channels.Channel.CreateUnbounded<int>();

var producer = Task.Run(async () =>
{
    for (int i = 0; i < 5; i++)
    {
        await channel.Writer.WriteAsync(i);
        Console.WriteLine($"Sent: {i}");
    }
    channel.Writer.Complete();
});

var consumer = Task.Run(async () =>
{
    await foreach (int item in channel.Reader.ReadAllAsync())
        Console.WriteLine($"Received: {item}");
});

await Task.WhenAll(producer, consumer);

// 4. ThreadLocal<T> — per-thread copy (not shared, but partitions data)
var localRng = new ThreadLocal<Random>(() => new Random());
await Task.WhenAll(Enumerable.Range(0, 4).Select(_ =>
    Task.Run(() => Console.WriteLine(localRng.Value!.Next(100)))));

// 5. Immutable shared data — safest (no synchronization needed)
// Prefer record types and ImmutableList<T>, ImmutableDictionary<T,V>
using System.Collections.Immutable;
ImmutableList<int> immutable = ImmutableList<int>.Empty.Add(1).Add(2);
// Any thread can read immutable safely; Add() returns a new list
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement a producer-consumer scenario in C#?

```cs
using System.Threading.Channels;

// ✅ Modern approach: Channel<T> (preferred in .NET 5+)
var channel = Channel.CreateBounded<int>(capacity: 10);

async Task ProduceAsync()
{
    for (int i = 0; i < 20; i++)
    {
        await channel.Writer.WriteAsync(i);
        Console.WriteLine($"Produced: {i}");
        await Task.Delay(50);
    }
    channel.Writer.Complete();
}

async Task ConsumeAsync(int id)
{
    await foreach (int item in channel.Reader.ReadAllAsync())
    {
        Console.WriteLine($"Consumer {id} got: {item}");
        await Task.Delay(120);
    }
}

// One producer, two consumers
await Task.WhenAll(
    ProduceAsync(),
    ConsumeAsync(1),
    ConsumeAsync(2));

// Alternative: BlockingCollection<T> (older, synchronous API)
var collection = new System.Collections.Concurrent.BlockingCollection<int>(boundedCapacity: 5);

var producer = Task.Run(() =>
{
    for (int i = 0; i < 10; i++)
    {
        collection.Add(i); // blocks if full
        Console.WriteLine($"Added: {i}");
    }
    collection.CompleteAdding();
});

var consumer = Task.Run(() =>
{
    foreach (int item in collection.GetConsumingEnumerable()) // blocks if empty
        Console.WriteLine($"Consumed: {item}");
});

await Task.WhenAll(producer, consumer);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `CancellationToken` and how is it used in multithreading?

`CancellationToken` provides a cooperative cancellation model — the producer (caller) signals cancellation; the consumer (worker) checks and responds to it. No thread is forcibly aborted.

```cs
// 1. Basic usage
using var cts = new CancellationTokenSource();
CancellationToken token = cts.Token;

var task = Task.Run(async () =>
{
    for (int i = 0; i < 100; i++)
    {
        token.ThrowIfCancellationRequested(); // throws OperationCanceledException
        Console.WriteLine($"Working {i}");
        await Task.Delay(100, token); // also cancellable
    }
}, token);

await Task.Delay(350);
cts.Cancel(); // signal cancellation

try   { await task; }
catch (OperationCanceledException) { Console.WriteLine("Task cancelled"); }

// 2. Timeout cancellation
using var timeoutCts = new CancellationTokenSource(TimeSpan.FromSeconds(2));
// CancellationTokenSource.CreateLinkedTokenSource — combine multiple tokens
using var linkedCts = CancellationTokenSource.CreateLinkedTokenSource(
    cts.Token, timeoutCts.Token);

// 3. Register a callback on cancellation
linkedCts.Token.Register(() => Console.WriteLine("Cleanup on cancellation"));

// 4. Check without throwing
if (token.IsCancellationRequested)
{
    Console.WriteLine("Cancelled (non-throwing check)");
    return;
}

// 5. Pass to .NET APIs — most async methods accept CancellationToken
using var httpClient = new HttpClient();
try
{
    using var newCts = new CancellationTokenSource(TimeSpan.FromSeconds(5));
    string data = await httpClient.GetStringAsync("https://example.com", newCts.Token);
}
catch (TaskCanceledException) { Console.WriteLine("HTTP request timed out"); }

// 6. Thread-based (non-async) polling
void LongWork(CancellationToken ct)
{
    while (!ct.IsCancellationRequested)
    {
        Thread.Sleep(100); // do work
    }
    ct.ThrowIfCancellationRequested();
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use `Concurrent` collections in C#?

`System.Collections.Concurrent` provides thread-safe collections that avoid explicit `lock` statements.

```cs
using System.Collections.Concurrent;

// ConcurrentDictionary<TKey, TValue>
var dict = new ConcurrentDictionary<string, int>();
dict.TryAdd("Alice", 100);
dict.AddOrUpdate("Alice", 100, (key, old) => old + 50); // atomic update
int val = dict.GetOrAdd("Bob", key => 200);             // atomic get-or-add
Console.WriteLine(dict["Alice"]); // 150

// ConcurrentQueue<T> — FIFO, lock-free
var queue = new ConcurrentQueue<int>();
Parallel.For(0, 10, i => queue.Enqueue(i));
while (queue.TryDequeue(out int item))
    Console.Write($"{item} ");
Console.WriteLine();

// ConcurrentStack<T> — LIFO
var stack = new ConcurrentStack<int>();
stack.PushRange([1, 2, 3, 4, 5]);
if (stack.TryPop(out int top)) Console.WriteLine($"Popped: {top}"); // 5

// ConcurrentBag<T> — unordered, optimised for same-thread add/take
var bag = new ConcurrentBag<int>();
await Task.WhenAll(Enumerable.Range(0, 100).Select(i =>
    Task.Run(() => bag.Add(i))));
Console.WriteLine($"Bag count: {bag.Count}"); // 100

// BlockingCollection<T> — bounded buffer with blocking Add/Take
var bounded = new BlockingCollection<int>(boundedCapacity: 5);

var prod = Task.Run(() =>
{
    for (int i = 0; i < 10; i++)
    {
        bounded.Add(i);                         // blocks when full
        Console.WriteLine($"Produced: {i}");
    }
    bounded.CompleteAdding();
});

var cons = Task.Run(() =>
{
    foreach (int n in bounded.GetConsumingEnumerable()) // blocks when empty
        Console.WriteLine($"Consumed: {n}");
});

await Task.WhenAll(prod, cons);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `Parallel.For` and `Task.Run`?

| | `Parallel.For` / `Parallel.ForEach` | `Task.Run` |
|-|-------------------------------------|-----------|
| **Purpose** | Data parallelism — divide a collection across cores | Run a single unit of work asynchronously |
| **Blocking** | Blocks the calling thread until all iterations complete | Non-blocking — returns a `Task` |
| **Partitioning** | Automatic (Partitioner) | Manual |
| **Degree of parallelism** | `MaxDegreeOfParallelism` option | Manual via `SemaphoreSlim` |
| **Use case** | CPU-bound loops over data | Single async or CPU-bound job |

```cs
// Parallel.For — best for CPU-bound data processing
var results = new int[10];
Parallel.For(0, 10, new ParallelOptions { MaxDegreeOfParallelism = 4 }, i =>
{
    results[i] = i * i; // safe because each i writes to a different index
    Console.WriteLine($"i={i} on thread {Thread.CurrentThread.ManagedThreadId}");
});
Console.WriteLine(string.Join(", ", results));

// Parallel.ForEach
var files = Directory.GetFiles(".", "*.cs");
Parallel.ForEach(files, new ParallelOptions { MaxDegreeOfParallelism = 2 }, file =>
{
    int lines = File.ReadLines(file).Count();
    Console.WriteLine($"{Path.GetFileName(file)}: {lines} lines");
});

// Task.Run — single async unit of work
var task = Task.Run(() =>
{
    long sum = 0;
    for (long i = 0; i < 1_000_000; i++) sum += i;
    return sum;
});
Console.WriteLine(await task);

// ⚠️ Parallel.For with async — use Parallel.ForEachAsync (.NET 6+)
await Parallel.ForEachAsync(files, new ParallelOptions { MaxDegreeOfParallelism = 4 },
    async (file, ct) =>
    {
        string content = await File.ReadAllTextAsync(file, ct);
        Console.WriteLine($"{Path.GetFileName(file)}: {content.Length} chars");
    });
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the advantages and disadvantages of multithreading?

**Advantages:**

| Advantage | Detail |
|-----------|--------|
| **Improved throughput** | Utilize multiple CPU cores for CPU-bound work |
| **Responsiveness** | UI thread stays responsive while background work runs |
| **Parallelism** | Independent tasks run simultaneously |
| **Better resource utilisation** | Threads run while others wait on I/O |
| **Scalability** | Scale to available hardware cores |

**Disadvantages:**

| Disadvantage | Detail |
|-------------|--------|
| **Complexity** | Harder to design, debug, and reason about |
| **Race conditions** | Unsynchronized shared state leads to bugs |
| **Deadlocks / livelocks** | Threads block each other permanently |
| **Overhead** | Context switches, synchronization, memory |
| **Difficult testing** | Bugs are timing-dependent and non-reproducible |
| **Priority inversion** | High-priority thread blocked by low-priority one |

```cs
// When to use multithreading:
// ✅ CPU-bound: image processing, data crunching, compression
// ✅ Parallel independent tasks: batch file processing
// ✅ Background work: keep UI responsive
// ✅ I/O-bound: async/await without dedicated threads

// When to AVOID:
// ❌ Simple sequential logic — adds complexity with no benefit
// ❌ Shared state that\'s complex to synchronize
// ❌ Very short tasks — thread creation overhead exceeds benefit

// Modern guideline:
// CPU-bound: Parallel.For, Parallel.ForEachAsync, Task.Run
// I/O-bound: async/await (no extra threads needed)
// Producer/consumer: Channel<T>
// Avoid raw Thread() — use Task-based APIs instead
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does multithreading improve performance over a single-threaded solution?

```cs
// Single-threaded: tasks run sequentially — total time = sum of each
var sw = System.Diagnostics.Stopwatch.StartNew();

int r1 = HeavyCompute(1);
int r2 = HeavyCompute(2);
int r3 = HeavyCompute(3);
int r4 = HeavyCompute(4);

sw.Stop();
Console.WriteLine($"Sequential: {sw.ElapsedMilliseconds} ms, results: {r1+r2+r3+r4}");

// Multi-threaded: tasks run in parallel — total time ≈ max of each
sw.Restart();

int[] results = await Task.WhenAll(
    Task.Run(() => HeavyCompute(1)),
    Task.Run(() => HeavyCompute(2)),
    Task.Run(() => HeavyCompute(3)),
    Task.Run(() => HeavyCompute(4)));

sw.Stop();
Console.WriteLine($"Parallel:   {sw.ElapsedMilliseconds} ms, results: {results.Sum()}");

int HeavyCompute(int seed)
{
    Thread.Sleep(500); // simulate 500 ms CPU work
    return seed * seed;
}
// Sequential: ~2000 ms
// Parallel:   ~500 ms  (4x speedup on 4+ cores)

// I/O-bound: async/await saves threads entirely
sw.Restart();
var fetches = Enumerable.Range(1, 4).Select(i =>
    Task.Run(() => { Thread.Sleep(300); return i; })); // simulate I/O
int[] ioResults = await Task.WhenAll(fetches);
sw.Stop();
Console.WriteLine($"Async I/O: {sw.ElapsedMilliseconds} ms"); // ~300 ms

// Amdahl\'s Law: speedup is limited by the sequential portion
// If 20% of code is sequential, max speedup = 1 / 0.2 = 5x regardless of cores
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. When should multithreading be used and when should it be avoided in C#?

```cs
// ✅ USE multithreading when:

// 1. CPU-bound parallel work — multiple independent CPU-intensive tasks
var primes = await Task.Run(() =>
    Enumerable.Range(2, 1_000_000)
              .AsParallel()
              .Where(IsPrime)
              .Count());

// 2. UI responsiveness — background work while UI stays responsive
// (WPF/MAUI: always run long work off the UI thread)
await Task.Run(() => ProcessLargeFile("data.csv")); // off UI thread

// 3. I/O-bound parallelism — multiple concurrent HTTP/DB calls
var responses = await Task.WhenAll(
    httpClient.GetStringAsync("https://api1.example.com"),
    httpClient.GetStringAsync("https://api2.example.com"));

// 4. Background services — polling, cleanup, monitoring
var cts = new CancellationTokenSource();
Task bgService = Task.Factory.StartNew(async () =>
{
    while (!cts.Token.IsCancellationRequested)
    {
        await DoMaintenanceAsync();
        await Task.Delay(TimeSpan.FromMinutes(5), cts.Token);
    }
}, TaskCreationOptions.LongRunning);

// ❌ AVOID multithreading when:

// 1. Simple sequential logic — no gain, only complexity
// BAD:
int badResult = await Task.Run(() => 2 + 2);

// GOOD:
int goodResult = 2 + 2;

// 2. Tasks are too short — thread overhead > benefit
// BAD: threading a 1 µs operation
// GOOD: batch small items, then parallelize the batch

// 3. Heavy shared state — if everything needs a lock, parallelism is lost

// 4. Ordering matters — parallel tasks don\'t preserve order

bool IsPrime(int n)
{
    if (n < 2) return false;
    for (int i = 2; i * i <= n; i++)
        if (n % i == 0) return false;
    return true;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can you ensure mutual exclusion without using `lock` or `Monitor`?

```cs
// 1. SemaphoreSlim(1,1) — async-compatible mutual exclusion
var sem = new SemaphoreSlim(1, 1);

async Task CriticalSectionAsync()
{
    await sem.WaitAsync(); // async — doesn\'t block a thread
    try { /* exclusive work */ await Task.Delay(100); }
    finally { sem.Release(); }
}

// 2. Mutex — cross-process mutual exclusion
using var mutex = new Mutex(false, "Global\\MyAppMutex");
mutex.WaitOne();
try { /* exclusive work */ }
finally { mutex.ReleaseMutex(); }

// 3. SpinLock — busy-wait for very short sections (no kernel transition)
var spinLock = new SpinLock(enableThreadOwnerTracking: false);
bool taken = false;
try
{
    spinLock.Enter(ref taken);
    // ultra-short critical section
    Console.WriteLine("SpinLock acquired");
}
finally { if (taken) spinLock.Exit(); }

// 4. Interlocked.CompareExchange — optimistic lock-free CAS
int lockFlag = 0;
while (Interlocked.CompareExchange(ref lockFlag, 1, 0) != 0)
    Thread.SpinWait(1); // spin until we set flag 0→1
try { /* exclusive work */ }
finally { Interlocked.Exchange(ref lockFlag, 0); }

// 5. ReaderWriterLockSlim — multiple readers, exclusive writer
var rwLock = new ReaderWriterLockSlim();
// Writer
rwLock.EnterWriteLock();
try { /* exclusive write */ }
finally { rwLock.ExitWriteLock(); }
// Reader
rwLock.EnterReadLock();
try { /* concurrent reads */ }
finally { rwLock.ExitReadLock(); }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain the difference between `Barrier` and `CountdownEvent`. Provide a real-world scenario for each.

| | `Barrier` | `CountdownEvent` |
|-|-----------|-----------------|
| **Purpose** | Synchronize N threads at each **phase boundary** | Wait until N operations have signalled completion |
| **Reusable** | ✅ Automatically resets for each phase | ❌ One-shot (or manually reset) |
| **Participants** | Fixed at creation (can be added/removed) | Count set at creation |
| **Direction** | All threads wait for each other | One thread waits; many threads signal |

```cs
// Barrier — pipeline with phases
// Real-world: parallel rendering pipeline where all threads must finish
// Phase 1 (geometry) before any starts Phase 2 (shading)

int workers = 4;
using var barrier = new Barrier(participants: workers, postPhaseAction: b =>
    Console.WriteLine($"\n--- Phase {b.CurrentPhaseNumber + 1} complete ---\n"));

var tasks = Enumerable.Range(0, workers).Select(id => Task.Run(() =>
{
    Console.WriteLine($"Worker {id}: Phase 1 (geometry)");
    Thread.Sleep(Random.Shared.Next(100, 400));
    barrier.SignalAndWait(); // wait for all to finish Phase 1

    Console.WriteLine($"Worker {id}: Phase 2 (shading)");
    Thread.Sleep(Random.Shared.Next(100, 300));
    barrier.SignalAndWait(); // wait for all to finish Phase 2

    Console.WriteLine($"Worker {id}: Phase 3 (output)");
}));
await Task.WhenAll(tasks);

// CountdownEvent — wait for N async completions
// Real-world: download N files concurrently; proceed only when all are done

int fileCount = 5;
using var countdown = new CountdownEvent(initialCount: fileCount);

for (int i = 0; i < fileCount; i++)
{
    int fileId = i;
    Task.Run(() =>
    {
        Thread.Sleep(Random.Shared.Next(200, 600)); // simulate download
        Console.WriteLine($"File {fileId} downloaded");
        countdown.Signal(); // decrement the count
    });
}

countdown.Wait(); // block until count reaches 0
Console.WriteLine("All files downloaded — proceeding with processing");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the issues with `Thread.Abort()`? How do you gracefully stop a thread?

**`Thread.Abort()` is removed in .NET Core / .NET 5+.** It was unsafe because it injected a `ThreadAbortException` at an arbitrary point, potentially corrupting state, leaving locks acquired, or skipping `finally` blocks.

```cs
// ❌ Thread.Abort — NOT available in .NET 5+
// var t = new Thread(...);
// t.Abort(); // throws PlatformNotSupportedException on .NET 5+

// ✅ Graceful cancellation via CancellationToken (recommended)
using var cts = new CancellationTokenSource();

var worker = Task.Run(async () =>
{
    while (!cts.Token.IsCancellationRequested)
    {
        Console.WriteLine("Working...");
        await Task.Delay(300, cts.Token);
    }
    Console.WriteLine("Gracefully stopped");
}, cts.Token);

await Task.Delay(1000);
cts.Cancel(); // cooperative cancellation
try   { await worker; }
catch (OperationCanceledException) { Console.WriteLine("Task cancelled"); }

// ✅ Volatile flag — simple polling (no Task)
public class BackgroundWorker
{
    private volatile bool _stop;
    private Thread? _thread;

    public void Start()
    {
        _thread = new Thread(() =>
        {
            while (!_stop)
            {
                Console.WriteLine("Tick");
                Thread.Sleep(200);
            }
            Console.WriteLine("Worker stopped");
        }) { IsBackground = true };
        _thread.Start();
    }

    public void Stop()
    {
        _stop = true;
        _thread?.Join(timeout: TimeSpan.FromSeconds(2));
    }
}

var bw = new BackgroundWorker();
bw.Start();
await Task.Delay(700);
bw.Stop();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you achieve thread synchronization using `ReaderWriterLockSlim`? What are its advantages over `ReaderWriterLock`?

`ReaderWriterLockSlim` allows **multiple concurrent readers** and **exclusive writers**, improving throughput for read-heavy workloads.

| | `ReaderWriterLock` | `ReaderWriterLockSlim` |
|-|-------------------|----------------------|
| **Performance** | Slower | Faster (optimised internals) |
| **Recursive support** | Via flags | Opt-in (`LockRecursionPolicy`) |
| **Upgradeable read lock** | ❌ | ✅ `EnterUpgradeableReadLock` |
| **Recommendation** | Legacy (avoid) | ✅ Use this |

```cs
public class ThreadSafeCache<TKey, TValue> where TKey : notnull
{
    private readonly Dictionary<TKey, TValue> _dict = new();
    private readonly ReaderWriterLockSlim _lock = new();

    public TValue? Get(TKey key)
    {
        _lock.EnterReadLock(); // multiple readers concurrently
        try
        {
            return _dict.TryGetValue(key, out var val) ? val : default;
        }
        finally { _lock.ExitReadLock(); }
    }

    public void Set(TKey key, TValue value)
    {
        _lock.EnterWriteLock(); // exclusive — blocks all readers and writers
        try { _dict[key] = value; }
        finally { _lock.ExitWriteLock(); }
    }

    // Upgradeable lock — check then conditionally write (no double-locking)
    public TValue GetOrAdd(TKey key, Func<TKey, TValue> factory)
    {
        _lock.EnterUpgradeableReadLock();
        try
        {
            if (_dict.TryGetValue(key, out var existing)) return existing;

            _lock.EnterWriteLock(); // upgrade to write
            try
            {
                var value = factory(key);
                _dict[key] = value;
                return value;
            }
            finally { _lock.ExitWriteLock(); }
        }
        finally { _lock.ExitUpgradeableReadLock(); }
    }

    public void Dispose() => _lock.Dispose();
}

// Usage
var cache = new ThreadSafeCache<string, int>();
await Task.WhenAll(
    Task.Run(() => cache.Set("x", 42)),
    Task.Run(() => Console.WriteLine(cache.Get("x"))),
    Task.Run(() => Console.WriteLine(cache.GetOrAdd("y", _ => 99))));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Discuss the differences between `volatile`, `Interlocked`, and `Thread.MemoryBarrier`. When should each be used?

| | `volatile` | `Interlocked` | `Thread.MemoryBarrier` |
|-|-----------|--------------|----------------------|
| **Prevents caching** | ✅ | ✅ (implicit) | ✅ (explicit fence) |
| **Prevents reordering** | Partial (acquire/release) | ✅ | ✅ (full fence) |
| **Atomic compound ops** | ❌ | ✅ | ❌ |
| **Overhead** | Minimal | Low (single CPU instruction) | Low–Medium |
| **Use case** | Simple flags; visibility | Atomic read/modify/write | Custom lock-free algorithms |

```cs
// volatile — prevent caching of a simple flag
private volatile bool _shutdown = false;

void Worker()
{
    while (!_shutdown) { /* work */ }  // always reads from memory
}
void Stop() => _shutdown = true; // immediately visible

// Interlocked — atomic compound operation on a single variable
int counter = 0;
Interlocked.Increment(ref counter);               // atomic read + add + write
int old = Interlocked.Exchange(ref counter, 100); // atomic swap
int orig = Interlocked.CompareExchange(ref counter, 200, 100); // CAS

// Thread.MemoryBarrier — full memory fence in custom lock-free code
private int _data;
private int _flag;

void Produce()
{
    _data = 99;
    Thread.MemoryBarrier(); // STORE fence: _data write visible before _flag write
    _flag = 1;
}

int Consume()
{
    while (Volatile.Read(ref _flag) == 0) { } // spin
    Thread.MemoryBarrier(); // LOAD fence: _flag read before _data read
    return _data; // guaranteed to see 99
}

// Volatile.Read / Volatile.Write — explicit volatile semantics without field keyword
int val = Volatile.Read(ref _flag);
Volatile.Write(ref _flag, 1);

// Rule of thumb:
// One thread writes, one thread reads a simple flag  → volatile
// Atomic increment / compare-and-swap               → Interlocked
// Custom lock-free algorithm with ordering needs    → MemoryBarrier / Volatile.Read/Write
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain thread-local storage and data partitioning in C# multithreading.

**Thread-local storage (TLS)** gives each thread its own private copy of a variable — no sharing, no synchronization needed.  
**Data partitioning** divides a dataset into independent chunks and assigns each chunk to a separate thread.

```cs
// 1. ThreadLocal<T> — per-thread instance
var localRng = new ThreadLocal<Random>(() => new Random(), trackAllValues: true);

await Task.WhenAll(Enumerable.Range(0, 4).Select(i => Task.Run(() =>
{
    // Each thread has its own Random — no lock needed
    int roll = localRng.Value!.Next(1, 7);
    Console.WriteLine($"Thread {Thread.CurrentThread.ManagedThreadId}: rolled {roll}");
})));

// See all per-thread values
Console.WriteLine($"Instances created: {localRng.Values.Count}");
localRng.Dispose();

// 2. [ThreadStatic] — simpler but no initializer
[ThreadStatic] private static int _threadId;

// 3. Data partitioning — PLINQ
var numbers = Enumerable.Range(1, 10_000_000);
long sum = numbers.AsParallel()
                  .WithDegreeOfParallelism(4)
                  .Where(n => n % 2 == 0)
                  .Select(n => (long)n)
                  .Sum();
Console.WriteLine($"Sum of evens: {sum}");

// 4. Data partitioning — Parallel.For with thread-local accumulator (no shared state)
long total = 0;
Parallel.For(
    fromInclusive: 0L,
    toExclusive:   10_000_000L,
    localInit:    () => 0L,                          // per-thread local
    body:         (i, _, local) => local + i,        // accumulate locally
    localFinally: local => Interlocked.Add(ref total, local) // merge once
);
Console.WriteLine($"Parallel total: {total}");

// 5. Partitioner — custom partition strategy
var partitioner = Partitioner.Create(0, 10_000_000, rangeSize: 500_000);
Parallel.ForEach(partitioner, range =>
{
    long localSum = 0;
    for (long i = range.Item1; i < range.Item2; i++) localSum += i;
    Interlocked.Add(ref total, localSum);
});
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you combine async/await with multithreading? How does `TaskScheduler` fit in?

`async/await` is primarily an **I/O-bound** model — it doesn\'t create new threads. When you need CPU-bound work alongside async, combine `Task.Run` with `await`. `TaskScheduler` controls *where* tasks execute.

```cs
// 1. CPU-bound work + async I/O together
async Task<int> ProcessFileAsync(string path, CancellationToken ct)
{
    // I/O-bound: no thread blocked
    string content = await File.ReadAllTextAsync(path, ct);

    // CPU-bound: offload to thread pool, don\'t block the async context
    int wordCount = await Task.Run(() => content.Split().Length, ct);
    return wordCount;
}

// 2. Concurrent CPU + I/O
var tasks = Directory.GetFiles(".", "*.cs")
    .Select(f => ProcessFileAsync(f, CancellationToken.None));
int[] counts = await Task.WhenAll(tasks);
Console.WriteLine($"Total words: {counts.Sum()}");

// 3. TaskScheduler — controls execution context
// Default: ThreadPoolTaskScheduler (Task.Run uses this)
// CurrentThread: runs on the current thread (synchronous; testing)
// LimitedConcurrency: caps concurrent tasks

public class LimitedConcurrencyLevelTaskScheduler(int maxParallelism)
    : TaskScheduler
{
    private readonly LinkedList<Task> _tasks = new();
    private int _running;

    protected override void QueueTask(Task task)
    {
        lock (_tasks) _tasks.AddLast(task);
        TryExecuteNextTask();
    }

    private void TryExecuteNextTask()
    {
        lock (_tasks)
        {
            if (_running >= maxParallelism || _tasks.Count == 0) return;
            _running++;
            var task = _tasks.First!.Value;
            _tasks.RemoveFirst();
            ThreadPool.QueueUserWorkItem(_ =>
            {
                TryExecuteTask(task);
                lock (_tasks) { _running--; TryExecuteNextTask(); }
            });
        }
    }

    protected override bool TryExecuteTaskInline(Task task, bool prev) => false;
    protected override IEnumerable<Task> GetScheduledTasks() { lock (_tasks) return [.._tasks]; }
}

var scheduler = new LimitedConcurrencyLevelTaskScheduler(maxParallelism: 2);
var factory   = new TaskFactory(CancellationToken.None,
    TaskCreationOptions.None, TaskContinuationOptions.None, scheduler);

await Task.WhenAll(Enumerable.Range(0, 6)
    .Select(i => factory.StartNew(() =>
    {
        Console.WriteLine($"Task {i} on thread {Thread.CurrentThread.ManagedThreadId}");
        Thread.Sleep(200);
    })));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is parallelism? How do you control the degree of parallelism using the `Parallel` class?

**Parallelism** is executing multiple operations simultaneously on multiple CPU cores. **Degree of parallelism (DOP)** is how many threads/tasks run concurrently.

```cs
// Parallel.For with MaxDegreeOfParallelism
var options = new ParallelOptions
{
    MaxDegreeOfParallelism = 4,       // at most 4 threads
    CancellationToken      = CancellationToken.None,
};

var results = new int[20];
Parallel.For(0, 20, options, i =>
{
    results[i] = i * i;
    Console.WriteLine($"  [{i}] on thread {Thread.CurrentThread.ManagedThreadId}");
});
Console.WriteLine(string.Join(", ", results));

// Parallel.ForEach — over collections
var files = Directory.EnumerateFiles(".", "*.cs").ToList();
Parallel.ForEach(files, new ParallelOptions { MaxDegreeOfParallelism = 2 }, file =>
    Console.WriteLine($"{Path.GetFileName(file)} — {new FileInfo(file).Length} bytes"));

// Parallel.ForEachAsync — async-compatible (.NET 6+)
await Parallel.ForEachAsync(files, new ParallelOptions { MaxDegreeOfParallelism = 3 },
    async (file, ct) =>
    {
        string content = await File.ReadAllTextAsync(file, ct);
        Console.WriteLine($"{Path.GetFileName(file)}: {content.Length} chars");
    });

// PLINQ — parallel LINQ
long sum = Enumerable.Range(1, 10_000_000)
    .AsParallel()
    .WithDegreeOfParallelism(Environment.ProcessorCount)
    .Where(n => n % 2 == 0)
    .Select(n => (long)n)
    .Sum();
Console.WriteLine($"Sum: {sum}");

// Choosing DOP:
// CPU-bound: Environment.ProcessorCount  (fully utilise all cores)
// I/O-bound: higher than CPU count is fine (threads spend time waiting)
// Mixed:     experiment; start with 2 × ProcessorCount for I/O

Console.WriteLine($"CPU cores: {Environment.ProcessorCount}");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Describe lock contention and how to mitigate it.

**Lock contention** occurs when multiple threads compete to acquire the same lock. The thread that can\'t acquire the lock blocks, waiting — wasting CPU time and reducing throughput.

```cs
// High contention — all threads fight for one lock
object sharedLock = new();
int counter = 0;

// BAD: all 1000 tasks contend on a single lock
await Task.WhenAll(Enumerable.Range(0, 1000).Select(_ =>
    Task.Run(() => { lock (sharedLock) counter++; })));

// ✅ Mitigation 1: Interlocked — no lock needed for simple atomic ops
int atomicCounter = 0;
await Task.WhenAll(Enumerable.Range(0, 1000).Select(_ =>
    Task.Run(() => Interlocked.Increment(ref atomicCounter))));

// ✅ Mitigation 2: Lock striping — partition data across multiple locks
const int Stripes = 16;
var locks    = Enumerable.Range(0, Stripes).Select(_ => new object()).ToArray();
var counters = new int[Stripes];

await Task.WhenAll(Enumerable.Range(0, 1000).Select(i =>
    Task.Run(() =>
    {
        int stripe = i % Stripes;
        lock (locks[stripe]) counters[stripe]++;
    })));
Console.WriteLine($"Total: {counters.Sum()}"); // 1000

// ✅ Mitigation 3: ReaderWriterLockSlim — allow concurrent reads
var rwl = new ReaderWriterLockSlim();
var dict = new Dictionary<string, int> { ["key"] = 0 };

// Many readers can proceed simultaneously
await Task.WhenAll(Enumerable.Range(0, 100).Select(_ => Task.Run(() =>
{
    rwl.EnterReadLock();
    try { _ = dict["key"]; }
    finally { rwl.ExitReadLock(); }
})));

// ✅ Mitigation 4: Reduce lock scope — keep critical section minimal
int result;
lock (sharedLock) result = counter; // read fast under lock
Console.WriteLine(ExpensiveProcess(result)); // heavy work OUTSIDE lock

// ✅ Mitigation 5: ConcurrentDictionary — built-in lock striping
var cd = new System.Collections.Concurrent.ConcurrentDictionary<int, int>();
await Task.WhenAll(Enumerable.Range(0, 1000).Select(i =>
    Task.Run(() => cd.AddOrUpdate(i % 10, 1, (_, v) => v + 1))));

int ExpensiveProcess(int v) => v * 2;
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is lazy initialization in C# multithreading and how does it affect startup performance?

**Lazy initialization** defers the creation of an expensive object until it is first accessed. This reduces startup time and avoids allocating resources that may never be needed.

```cs
// 1. Lazy<T> — thread-safe by default (LazyThreadSafetyMode.ExecutionAndPublication)
var heavyService = new Lazy<DatabaseService>(() =>
{
    Console.WriteLine("Initializing DatabaseService...");
    return new DatabaseService("Server=localhost;");
});

Console.WriteLine("App started (no DB init yet)");
// DB not initialized until first .Value access
Console.WriteLine(heavyService.Value.Query("SELECT 1")); // initialized here
Console.WriteLine(heavyService.Value.Query("SELECT 2")); // reuses same instance

// 2. Thread-safety modes
var lazy1 = new Lazy<int>(() => 42,
    LazyThreadSafetyMode.ExecutionAndPublication); // default — safe, single init
var lazy2 = new Lazy<int>(() => 42,
    LazyThreadSafetyMode.PublicationOnly);          // allows multiple inits, first wins
var lazy3 = new Lazy<int>(() => 42,
    LazyThreadSafetyMode.None);                     // no thread safety — fastest, single-thread only

// 3. Lazy<T> in a service / singleton
public sealed class AppServices
{
    private static readonly Lazy<AppServices> _instance =
        new(() => new AppServices(), LazyThreadSafetyMode.ExecutionAndPublication);

    public static AppServices Instance => _instance.Value;
    private AppServices() { Console.WriteLine("AppServices initialized"); }
    public void DoWork() => Console.WriteLine("Working");
}

AppServices.Instance.DoWork(); // initialized on first access

// 4. LazyInitializer — static helper, struct-friendly (no wrapper object)
DatabaseService? _db = null;
DatabaseService db = LazyInitializer.EnsureInitialized(
    ref _db, () => new DatabaseService("Server=prod;"));

// 5. Impact on startup
// Without lazy: all services created at startup — slow, wastes memory for unused services
// With lazy:    only what\'s needed is created — faster startup, lower memory footprint

class DatabaseService(string connStr)
{
    public string Query(string sql)
    {
        Console.WriteLine($"Query [{sql}] on {connStr}");
        return "result";
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain `SpinLock` in C# multithreading. How does it differ from `lock` / `Monitor`?

`SpinLock` is a mutual exclusion primitive that **busy-waits** (spins) in a tight loop rather than yielding the thread to the OS. This avoids kernel transitions, making it faster for **very short** critical sections — but wasteful for longer ones.

| | `lock` / `Monitor` | `SpinLock` |
|-|-------------------|-----------|
| **Blocking** | Suspends thread (kernel sleep) | Busy-wait (CPU spinning) |
| **Best for** | Sections taking > ~1 µs | Sections taking < ~1 µs |
| **CPU usage while waiting** | Low (thread suspended) | High (continuous spin) |
| **Overhead per acquire** | Higher (kernel transition) | Lower (no kernel call) |
| **Struct** | Class | `struct` — avoid copying |
| **Thread affinity** | No | Must release on same thread |

```cs
// SpinLock usage
var spinLock = new SpinLock(enableThreadOwnerTracking: false);
int sharedCounter = 0;

await Task.WhenAll(Enumerable.Range(0, 1000).Select(_ => Task.Run(() =>
{
    bool taken = false;
    try
    {
        spinLock.Enter(ref taken); // busy-wait until acquired
        sharedCounter++;           // very short critical section
    }
    finally
    {
        if (taken) spinLock.Exit(useMemoryBarrier: false);
    }
})));

Console.WriteLine(sharedCounter); // 1000

// SpinWait — adaptive spinning with back-off (yield after many spins)
var sw = new SpinWait();
volatile bool ready = false;
Task.Run(() => { Thread.Sleep(100); ready = true; });

while (!ready)
    sw.SpinOnce(); // spins first, then yields, then sleeps
Console.WriteLine("Ready!");

// TryEnter — non-blocking
bool acquired = false;
spinLock.TryEnter(ref acquired);
if (acquired)
{
    try { /* work */ }
    finally { spinLock.Exit(); }
}

// ⚠️ Rules:
// - Never use SpinLock for I/O-bound or blocking code
// - Never await inside a SpinLock (deadlock risk on thread pool)
// - Don\'t copy the SpinLock struct — always pass by ref
// - Use Interlocked instead when operating on a single variable
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How are threads different from TPL?

| | Raw `Thread` | Task Parallel Library (TPL) |
|-|-------------|----------------------------|
| **Abstraction** | Low-level OS thread | High-level task abstraction |
| **Thread reuse** | No — new thread each time | Yes — reuses thread pool threads |
| **Return values** | Not built-in | `Task<T>` returns results |
| **Exception handling** | Manual (inside thread body) | Propagated via `await` / `.Result` |
| **Cancellation** | Manual flag/volatile | `CancellationToken` built-in |
| **Async/await** | Not supported | Native support |
| **Composition** | Manual `Join`, no chaining | `WhenAll`, `WhenAny`, continuations |
| **Parallel loops** | Manual partitioning | `Parallel.For`, `Parallel.ForEach` |
| **Best for** | Long-running, dedicated background threads | Everything else |

```cs
// Thread — low-level, full control
var thread = new Thread(() =>
{
    Console.WriteLine($"Raw thread: {Thread.CurrentThread.ManagedThreadId}");
    Thread.Sleep(200);
    Console.WriteLine("Thread done");
});
thread.IsBackground = true;
thread.Start();
thread.Join();

// TPL — high-level, composable, async-friendly
int result = await Task.Run(() =>
{
    Console.WriteLine($"TPL thread: {Thread.CurrentThread.ManagedThreadId}");
    Thread.Sleep(200);
    return 42;
});
Console.WriteLine($"Task result: {result}");

// TPL continuation chaining
var pipeline = Task.Run(() => "raw data")
    .ContinueWith(t => t.Result.ToUpper())
    .ContinueWith(t => $"Processed: {t.Result}");
Console.WriteLine(await pipeline);

// TPL: parallel loop — 4 cores, no manual thread management
await Parallel.ForEachAsync(Enumerable.Range(0, 8), async (i, ct) =>
{
    await Task.Delay(100, ct);
    Console.WriteLine($"Item {i} done");
});
```
## Q. What is the difference between Task and Thread in C#?

`Thread` is a low-level OS construct for concurrent execution. `Task` is a higher-level abstraction from the Task Parallel Library (TPL) that runs work on the **thread pool** and supports `async`/`await`.

| Feature                   | `Thread`                         | `Task`                              |
|---------------------------|----------------------------------|-------------------------------------|
| Abstraction level         | Low-level (OS thread)            | High-level (thread pool / async)    |
| Creation cost             | High (new OS thread each time)   | Low (reuses thread pool threads)    |
| Return value              | No built-in support              | `Task<T>` returns a result          |
| Exception handling        | Manual (unhandled = crash)       | Propagated via `await` / `.Result`  |
| Cancellation              | Manual (`Thread.Abort` removed)  | `CancellationToken` built-in        |
| Async/await               | Not supported                    | Native support                      |
| Recommended for           | Long-running dedicated work      | Everything else (preferred)         |

**Thread example (rare in modern .NET):**

```cs
var thread = new Thread(() =>
    Console.WriteLine($"Thread: {Thread.CurrentThread.ManagedThreadId}"));
thread.IsBackground = true;
thread.Start();
thread.Join();
```

**Task example (preferred):**

```cs
var result = await Task.Run(() =>
{
    Console.WriteLine($"Thread pool ID: {Thread.CurrentThread.ManagedThreadId}");
    return 42;
});
Console.WriteLine(result); // Output: 42
```

**Long-running task (equivalent to a dedicated thread):**

```cs
var longRunning = Task.Factory.StartNew(() =>
{
    while (true) { /* background service loop */ }
}, TaskCreationOptions.LongRunning);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 7. FILE HANDLING

<br/>

## Q. What is File Handling in C#.Net?

**File handling** is the ability to create, read, write, append, copy, move, and delete files and directories from C# code. The primary namespaces are:

| Namespace | Contents |
|-----------|---------|
| `System.IO` | `File`, `FileInfo`, `Directory`, `DirectoryInfo`, `Stream`, `StreamReader`, `StreamWriter`, `FileStream`, `MemoryStream`, `BinaryReader`, `BinaryWriter` |
| `System.Text.Json` | `JsonSerializer`, `Utf8JsonReader`, `Utf8JsonWriter` |
| `System.Xml` | `XmlReader`, `XmlWriter`, `XDocument` |

```cs
// Quick overview of the main file API
using System.IO;

// Static helper class — convenient for one-off operations
File.WriteAllText("note.txt", "Hello, .NET 10!");
string content = File.ReadAllText("note.txt");
Console.WriteLine(content); // Hello, .NET 10!

// Async versions (preferred in modern code)
await File.WriteAllTextAsync("note.txt", "Hello async!");
string text = await File.ReadAllTextAsync("note.txt");

// Check existence before operating
if (File.Exists("note.txt"))
    File.Delete("note.txt");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you read a file in C#?

```cs
// 1. File.ReadAllText — small files, reads entire file as one string
string text = await File.ReadAllTextAsync("data.txt");

// 2. File.ReadAllLines — reads into string array (one element per line)
string[] lines = await File.ReadAllLinesAsync("data.txt");
foreach (string line in lines) Console.WriteLine(line);

// 3. File.ReadAllBytes — binary content
byte[] bytes = await File.ReadAllBytesAsync("image.png");

// 4. StreamReader — large files, line-by-line streaming (low memory)
await using var reader = new StreamReader("large.csv");
string? line;
while ((line = await reader.ReadLineAsync()) is not null)
    Console.WriteLine(line);

// 5. File.ReadLines — lazy IEnumerable<string>, never loads entire file
foreach (string l in File.ReadLines("data.txt"))
    Console.WriteLine(l); // reads one line at a time

// 6. FileStream with buffer — lowest level, maximum control
await using var fs = new FileStream("data.bin",
    FileMode.Open, FileAccess.Read, FileShare.Read, bufferSize: 4096, useAsync: true);
byte[] buf = new byte[4096];
int bytesRead;
while ((bytesRead = await fs.ReadAsync(buf)) > 0)
    Console.WriteLine($"Read {bytesRead} bytes");

// 7. Pipes — zero-copy for high-throughput reading (.NET 5+)
using System.IO.Pipelines;
await using var pipeFs = File.OpenRead("data.bin");
var reader2 = PipeReader.Create(pipeFs);
while (true)
{
    var result = await reader2.ReadAsync();
    reader2.AdvanceTo(result.Buffer.End);
    if (result.IsCompleted) break;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you write to a file in C#?

```cs
// 1. File.WriteAllText — overwrites (or creates) the file
await File.WriteAllTextAsync("output.txt", "Hello, .NET 10!");

// 2. File.WriteAllLines — writes each string as a line
string[] lines = ["Line 1", "Line 2", "Line 3"];
await File.WriteAllLinesAsync("output.txt", lines);

// 3. File.WriteAllBytes — binary data
byte[] data = [0x50, 0x4B, 0x03, 0x04]; // ZIP magic bytes
await File.WriteAllBytesAsync("archive.bin", data);

// 4. StreamWriter — write line-by-line (useful for logging)
await using var writer = new StreamWriter("log.txt", append: false);
await writer.WriteLineAsync($"{DateTime.UtcNow:O} - App started");
await writer.WriteLineAsync("Processing...");
// Flushed and closed at end of using block

// 5. FileStream — raw bytes, full control
await using var fs = new FileStream("data.bin",
    FileMode.Create, FileAccess.Write, FileShare.None, 4096, useAsync: true);
byte[] bytes = System.Text.Encoding.UTF8.GetBytes("Hello binary world");
await fs.WriteAsync(bytes);

// 6. File.OpenWrite / File.Create — quick FileStream shortcuts
await using var quick = File.Create("temp.txt");
await quick.WriteAsync("quick write"u8.ToArray());

// 7. Atomic write pattern — write to temp, then rename (avoids partial writes)
string target = "config.json";
string temp   = target + ".tmp";
await File.WriteAllTextAsync(temp, System.Text.Json.JsonSerializer.Serialize(new { key = "val" }));
File.Move(temp, target, overwrite: true); // atomic on same volume
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `File.ReadAllText` and `File.ReadAllLines` in C#?

| | `File.ReadAllText` | `File.ReadAllLines` |
|-|-------------------|---------------------|
| **Returns** | `string` (entire file) | `string[]` (one element per line) |
| **Memory** | Single string allocation | Array of strings |
| **Line endings** | Preserved in string | Stripped (used as delimiter) |
| **Use case** | Small config/text files | CSV, log files, line-by-line processing |
| **Async** | `ReadAllTextAsync` | `ReadAllLinesAsync` |

```cs
// File content: "Hello\nWorld\nFoo"

// ReadAllText — one string with newlines intact
string all = await File.ReadAllTextAsync("data.txt");
Console.WriteLine(all.Length);      // includes \n characters
Console.WriteLine(all.Contains('\n')); // true

// ReadAllLines — array, newlines stripped
string[] lines = await File.ReadAllLinesAsync("data.txt");
Console.WriteLine(lines.Length);    // 3
Console.WriteLine(lines[0]);        // Hello
Console.WriteLine(lines[1]);        // World

// File.ReadLines — lazy (no full load) — best for large files
int lineCount = 0;
foreach (string line in File.ReadLines("data.txt"))
    lineCount++;
Console.WriteLine(lineCount); // 3

// When to choose:
// ReadAllText  → parse JSON/XML/config as a whole string
// ReadAllLines → process CSV/log line by line but file fits in memory
// ReadLines    → large files that don\'t fit in memory (streaming)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you append text to an existing file in C#?

```cs
// 1. File.AppendAllText — simplest
await File.AppendAllTextAsync("log.txt", $"{DateTime.UtcNow:O} - event\n");

// 2. File.AppendAllLines — appends multiple lines
string[] newLines = ["Entry 1", "Entry 2"];
await File.AppendAllLinesAsync("log.txt", newLines);

// 3. StreamWriter with append: true
await using var writer = new StreamWriter("log.txt", append: true);
await writer.WriteLineAsync($"[{DateTime.UtcNow:HH:mm:ss}] Application started");

// 4. FileStream with FileMode.Append
await using var fs = new FileStream("log.txt",
    FileMode.Append, FileAccess.Write, FileShare.None, 4096, useAsync: true);
byte[] entry = System.Text.Encoding.UTF8.GetBytes("appended line\n");
await fs.WriteAsync(entry);

// 5. High-frequency logging — keep StreamWriter open (don\'t reopen each write)
public sealed class FileLogger : IAsyncDisposable
{
    private readonly StreamWriter _writer;
    public FileLogger(string path) =>
        _writer = new StreamWriter(path, append: true) { AutoFlush = false };

    public async Task LogAsync(string message)
    {
        await _writer.WriteLineAsync($"{DateTime.UtcNow:O} {message}");
        await _writer.FlushAsync(); // or batch and flush periodically
    }

    public async ValueTask DisposeAsync() => await _writer.DisposeAsync();
}

await using var logger = new FileLogger("app.log");
await logger.LogAsync("Server started");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you check if a file exists in C#?

```cs
// 1. File.Exists — synchronous check (thread-safe to call)
if (File.Exists("config.json"))
{
    string config = await File.ReadAllTextAsync("config.json");
    Console.WriteLine(config);
}
else
{
    Console.WriteLine("Config not found, using defaults");
}

// 2. Directory.Exists — for directories
if (!Directory.Exists("logs"))
    Directory.CreateDirectory("logs");

// 3. FileInfo.Exists — when you need other file metadata too
var fi = new FileInfo("data.csv");
if (fi.Exists)
    Console.WriteLine($"Size: {fi.Length} bytes, Modified: {fi.LastWriteTimeUtc}");

// 4. ⚠️ TOCTOU race condition — check-then-use is not atomic
// Between Exists() check and the Open(), file could be deleted.
// Safer: just open and handle the exception
try
{
    string content = await File.ReadAllTextAsync("data.txt");
    // process...
}
catch (FileNotFoundException)
{
    Console.WriteLine("File not found — skipping");
}
catch (UnauthorizedAccessException)
{
    Console.WriteLine("No permission to read file");
}

// 5. Path combiners — avoid hardcoded separators
string basePath = AppContext.BaseDirectory;
string filePath = Path.Combine(basePath, "data", "config.json");
bool exists     = File.Exists(filePath);
Console.WriteLine($"{filePath} exists: {exists}");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `StreamReader` and `StreamWriter` classes in C#?

`StreamReader` and `StreamWriter` are text-oriented wrappers around a `Stream` that handle **character encoding** automatically. They read/write decoded text rather than raw bytes.

```cs
// StreamReader — read text from any Stream
// 1. From file path (convenience constructor)
await using var reader = new StreamReader("data.txt", System.Text.Encoding.UTF8);

// Read entire content
string all = await reader.ReadToEndAsync();

// Read line by line
string? line;
while ((line = await reader.ReadLineAsync()) is not null)
    Console.WriteLine(line);

// Check end-of-stream
Console.WriteLine($"AtEnd: {reader.EndOfStream}");

// 2. From any Stream (e.g., HTTP response, MemoryStream)
using var response = await new HttpClient().GetStreamAsync("https://example.com");
using var sr       = new StreamReader(response);
string html        = await sr.ReadToEndAsync();

// StreamWriter — write text to any Stream
// 1. To file
await using var writer = new StreamWriter("output.txt",
    append: false,
    encoding: System.Text.Encoding.UTF8);

await writer.WriteAsync("Hello ");
await writer.WriteLineAsync("World");
await writer.WriteLineAsync($"Time: {DateTime.UtcNow}");
// writer.FlushAsync() called automatically on dispose

// 2. AutoFlush — flush after every write (useful for log files)
var logWriter = new StreamWriter("log.txt", append: true) { AutoFlush = true };
await logWriter.WriteLineAsync("Started");
await logWriter.DisposeAsync();

// 3. Write to MemoryStream (no file I/O)
await using var ms      = new MemoryStream();
await using var msWriter = new StreamWriter(ms, leaveOpen: true);
await msWriter.WriteLineAsync("in-memory text");
await msWriter.FlushAsync();
ms.Position = 0;
using var msReader = new StreamReader(ms);
Console.WriteLine(await msReader.ReadToEndAsync()); // in-memory text
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle exceptions when working with files in C#?

```cs
// Common file I/O exceptions:
// FileNotFoundException    — file does not exist
// DirectoryNotFoundException — directory does not exist
// UnauthorizedAccessException — no read/write permission
// IOException              — disk full, file locked, I/O error
// PathTooLongException     — path exceeds OS limit
// NotSupportedException    — invalid path format

public async Task<string?> ReadFileSafeAsync(string path)
{
    try
    {
        return await File.ReadAllTextAsync(path);
    }
    catch (FileNotFoundException)
    {
        Console.WriteLine($"File not found: {path}");
        return null;
    }
    catch (UnauthorizedAccessException)
    {
        Console.WriteLine($"Access denied: {path}");
        return null;
    }
    catch (IOException ex)
    {
        Console.WriteLine($"I/O error reading {path}: {ex.Message}");
        return null;
    }
}

// Ensure streams are always closed — use 'await using' or 'using'
public async Task WriteFileSafeAsync(string path, string content)
{
    await using var writer = new StreamWriter(path); // disposed even on exception
    await writer.WriteAsync(content);
}

// File sharing conflicts — retry with backoff
public async Task<string> ReadWithRetryAsync(string path, int maxAttempts = 3)
{
    for (int attempt = 1; attempt <= maxAttempts; attempt++)
    {
        try
        {
            return await File.ReadAllTextAsync(path);
        }
        catch (IOException) when (attempt < maxAttempts)
        {
            await Task.Delay(200 * attempt); // exponential backoff
        }
    }
    throw new IOException($"Could not read {path} after {maxAttempts} attempts");
}

// Check before delete (avoids exception for common case)
public void DeleteIfExists(string path)
{
    try { File.Delete(path); }
    catch (FileNotFoundException) { /* already gone — fine */ }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you delete a file in C#?

```cs
// 1. File.Delete — throws if path is a directory or access denied; silent if not found
File.Delete("temp.txt");

// 2. Safe delete — suppress FileNotFoundException
public static void DeleteIfExists(string path)
{
    try { File.Delete(path); }
    catch (FileNotFoundException) { }
}

// 3. Check + delete (note: TOCTOU race — prefer try/catch above)
if (File.Exists("temp.txt"))
    File.Delete("temp.txt");

// 4. FileInfo.Delete
var fi = new FileInfo("old.log");
if (fi.Exists) fi.Delete();

// 5. Delete directory (empty)
Directory.Delete("emptyFolder");

// 6. Delete directory and all contents recursively
Directory.Delete("outputFolder", recursive: true);

// 7. Move to recycle bin (Windows only — via P/Invoke or FileSystem.DeleteFile)
// dotnet add package Microsoft.VisualBasic (included in .NET)
Microsoft.VisualBasic.FileIO.FileSystem.DeleteFile(
    "file.txt",
    Microsoft.VisualBasic.FileIO.UIOption.OnlyErrorDialogs,
    Microsoft.VisualBasic.FileIO.RecycleOption.SendToRecycleBin);

// 8. Delete multiple files matching a pattern
foreach (string file in Directory.GetFiles("logs", "*.log"))
    File.Delete(file);

// Or with LINQ
Directory.EnumerateFiles("logs", "*.tmp")
         .ToList()
         .ForEach(File.Delete);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `FileStream` and `MemoryStream` in C#?

| | `FileStream` | `MemoryStream` |
|-|-------------|----------------|
| **Backing store** | File on disk | In-memory byte array |
| **Persistence** | Data persists after app exits | Lost when stream is disposed/app exits |
| **Size limit** | Disk capacity | Available RAM |
| **Performance** | Slower (disk I/O) | Very fast (RAM) |
| **Async** | ✅ `useAsync: true` | ✅ (but completes synchronously) |
| **Use case** | Read/write actual files | Temporary buffers, unit testing, serialisation |
| **Seek** | ✅ (seekable) | ✅ (seekable) |

```cs
// FileStream — backed by disk
await using var fs = new FileStream("data.bin",
    FileMode.Create, FileAccess.ReadWrite, FileShare.None, 4096, useAsync: true);
await fs.WriteAsync("Hello file"u8.ToArray());
fs.Position = 0;
var buf = new byte[10];
await fs.ReadAsync(buf);
Console.WriteLine(System.Text.Encoding.UTF8.GetString(buf)); // Hello file

// MemoryStream — in-memory, no file
await using var ms = new MemoryStream(capacity: 256);
await ms.WriteAsync("Hello memory"u8.ToArray());
ms.Position = 0;
var mbuf = new byte[12];
await ms.ReadAsync(mbuf);
Console.WriteLine(System.Text.Encoding.UTF8.GetString(mbuf)); // Hello memory

// Common pattern: serialise to MemoryStream, then copy to FileStream
var obj = new { Name = "Alice", Age = 30 };
await using var output = new MemoryStream();
await System.Text.Json.JsonSerializer.SerializeAsync(output, obj);
output.Position = 0;
await using var file = File.Create("person.json");
await output.CopyToAsync(file);

// MemoryStream.ToArray() — get all bytes
byte[] allBytes = ms.ToArray(); // independent copy
// GetBuffer() — get underlying buffer (may have extra bytes past Length)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you copy a file in C#?

```cs
// 1. File.Copy — simplest
File.Copy("source.txt", "destination.txt", overwrite: true);

// 2. Async copy via streams (for large files with progress)
public static async Task CopyFileAsync(
    string source, string dest, IProgress<long>? progress = null,
    CancellationToken ct = default)
{
    await using var src  = new FileStream(source, FileMode.Open,   FileAccess.Read,  FileShare.Read,  65536, useAsync: true);
    await using var dst  = new FileStream(dest,   FileMode.Create, FileAccess.Write, FileShare.None, 65536, useAsync: true);
    var buffer = new byte[65536];
    long total = 0;
    int  read;
    while ((read = await src.ReadAsync(buffer, ct)) > 0)
    {
        await dst.WriteAsync(buffer.AsMemory(0, read), ct);
        total += read;
        progress?.Report(total);
    }
}

// Usage
await CopyFileAsync("big.iso", "backup.iso",
    new Progress<long>(bytes => Console.Write($"\r{bytes / 1_048_576} MB")));

// 3. FileInfo.CopyTo
var fi = new FileInfo("source.txt");
fi.CopyTo("destination.txt", overwrite: true);

// 4. Copy a directory recursively (.NET 7+)
// dotnet: Directory has no built-in recursive copy; use helper
static void CopyDirectory(string src, string dst)
{
    Directory.CreateDirectory(dst);
    foreach (string file in Directory.GetFiles(src))
        File.Copy(file, Path.Combine(dst, Path.GetFileName(file)), overwrite: true);
    foreach (string dir in Directory.GetDirectories(src))
        CopyDirectory(dir, Path.Combine(dst, Path.GetFileName(dir)));
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you move a file in C#?

```cs
// 1. File.Move — rename or move; throws if destination exists (use overwrite param)
File.Move("old.txt", "new.txt");                     // error if new.txt exists
File.Move("old.txt", "new.txt", overwrite: true);    // overwrites if exists

// 2. Move to a different directory
File.Move("C:/source/report.pdf", "D:/archive/report.pdf", overwrite: true);

// 3. FileInfo.MoveTo
var fi = new FileInfo("data.csv");
fi.MoveTo("archive/data.csv", overwrite: true);

// 4. Move directory
Directory.Move("OldFolder", "NewFolder"); // must be on same volume

// 5. Atomic move (same volume — rename is atomic on Windows/Linux)
File.Move("config.json.tmp", "config.json", overwrite: true);
// On same filesystem this is a rename — atomic, no partial-write window

// 6. Cross-volume move (copy + delete)
public static async Task MoveAcrossVolumesAsync(string src, string dst, CancellationToken ct = default)
{
    await using var source = new FileStream(src, FileMode.Open, FileAccess.Read, FileShare.None, 65536, useAsync: true);
    await using var dest   = new FileStream(dst, FileMode.Create, FileAccess.Write, FileShare.None, 65536, useAsync: true);
    await source.CopyToAsync(dest, ct);
    source.Close(); // close before deleting
    File.Delete(src);
}

// 7. Rename all files in a folder
foreach (string file in Directory.EnumerateFiles("reports", "*.txt"))
{
    string newName = Path.ChangeExtension(file, ".md");
    File.Move(file, newName, overwrite: false);
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `FileInfo` class and how is it used?

`FileInfo` provides **instance-based** file operations and rich metadata about a single file. Unlike the static `File` class, it performs only one security check at construction time — useful when you need to perform multiple operations on the same file.

```cs
var fi = new FileInfo("report.csv");

// Metadata — no security check on each property
Console.WriteLine($"Name:       {fi.Name}");           // report.csv
Console.WriteLine($"Full path:  {fi.FullName}");
Console.WriteLine($"Directory:  {fi.DirectoryName}");
Console.WriteLine($"Extension:  {fi.Extension}");      // .csv
Console.WriteLine($"Size:       {fi.Length} bytes");
Console.WriteLine($"Created:    {fi.CreationTimeUtc}");
Console.WriteLine($"Modified:   {fi.LastWriteTimeUtc}");
Console.WriteLine($"Read-only:  {fi.IsReadOnly}");
Console.WriteLine($"Exists:     {fi.Exists}");

// Operations
if (fi.Exists)
{
    // Open for reading
    await using StreamReader reader = fi.OpenText();
    string content = await reader.ReadToEndAsync();

    // Copy
    FileInfo copy = fi.CopyTo("report_backup.csv", overwrite: true);
    Console.WriteLine($"Copy size: {copy.Length}");

    // Move / rename
    fi.MoveTo("archive/report.csv", overwrite: true);

    // Delete
    fi.Delete();
}

// Create / open with specific FileStream options
await using FileStream fs = fi.Open(FileMode.OpenOrCreate, FileAccess.ReadWrite);

// Refresh metadata cache (if file changed externally)
fi.Refresh();
Console.WriteLine($"Updated size: {fi.Length}");

// DirectoryInfo — same concept for directories
var di = new DirectoryInfo("logs");
di.Create(); // no-op if already exists
foreach (FileInfo logFile in di.GetFiles("*.log"))
    Console.WriteLine($"{logFile.Name}: {logFile.Length} bytes");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you read and write binary files in C#?

```cs
// ── Writing binary data ──────────────────────────────────────────────

// 1. File.WriteAllBytes
byte[] raw = [0x89, 0x50, 0x4E, 0x47]; // PNG magic bytes
await File.WriteAllBytesAsync("header.bin", raw);

// 2. BinaryWriter — write typed primitives
await using var fs     = new FileStream("data.bin", FileMode.Create, FileAccess.Write, FileShare.None, 4096, true);
await using var writer = new BinaryWriter(fs, System.Text.Encoding.UTF8, leaveOpen: true);

writer.Write(42);              // int   (4 bytes)
writer.Write(3.14);            // double (8 bytes)
writer.Write("Hello");         // length-prefixed string
writer.Write(true);            // bool   (1 byte)
writer.Write((byte)0xFF);      // byte

// 3. Span<byte> with FileStream (best performance, .NET 5+)
await using var write = File.OpenWrite("span.bin");
Span<byte> span = stackalloc byte[8];
System.Buffers.Binary.BinaryPrimitives.WriteInt64LittleEndian(span, 123456789L);
await write.WriteAsync(span.ToArray());

// ── Reading binary data ──────────────────────────────────────────────

// 1. File.ReadAllBytes
byte[] bytes = await File.ReadAllBytesAsync("data.bin");

// 2. BinaryReader — read matching typed data
await using var rfs    = new FileStream("data.bin", FileMode.Open, FileAccess.Read, FileShare.Read, 4096, true);
await using var reader = new BinaryReader(rfs, System.Text.Encoding.UTF8, leaveOpen: true);

int    number = reader.ReadInt32();    // 42
double pi     = reader.ReadDouble();   // 3.14
string text   = reader.ReadString();   // Hello
bool   flag   = reader.ReadBoolean();  // true
byte   b      = reader.ReadByte();     // 0xFF

Console.WriteLine($"{number}, {pi}, {text}, {flag}, {b:X2}");

// 3. Struct serialisation via MemoryMarshal (zero-copy)
[System.Runtime.InteropServices.StructLayout(
    System.Runtime.InteropServices.LayoutKind.Sequential, Pack = 1)]
struct Header { public int Magic; public short Version; public long Timestamp; }

await using var hfs = File.OpenRead("header.bin");
byte[] hbuf = new byte[System.Runtime.InteropServices.Marshal.SizeOf<Header>()];
await hfs.ReadExactlyAsync(hbuf);
Header header = System.Runtime.InteropServices.MemoryMarshal
    .Read<Header>(hbuf);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you work with directories in C#?

```cs
// 1. Create directory (and parents)
Directory.CreateDirectory("logs/2026/april"); // creates entire path, no error if exists

// 2. Check existence
bool exists = Directory.Exists("logs");

// 3. Delete
Directory.Delete("emptyFolder");                  // must be empty
Directory.Delete("fullFolder", recursive: true);  // deletes all contents

// 4. List contents
string[] files   = Directory.GetFiles("logs");               // non-recursive
string[] allCsvs = Directory.GetFiles("data", "*.csv", SearchOption.AllDirectories);

// Lazy enumeration (better for large directories)
foreach (string file in Directory.EnumerateFiles("logs", "*.log", new EnumerationOptions
{
    RecurseSubdirectories = true,
    IgnoreInaccessible    = true,
    MatchCasing           = MatchCasing.CaseInsensitive,
}))
{
    Console.WriteLine(file);
}

// 5. Move / rename directory
Directory.Move("OldName", "NewName");

// 6. Get directory info
var di = new DirectoryInfo("logs");
Console.WriteLine($"Created: {di.CreationTimeUtc}");
Console.WriteLine($"Files:   {di.GetFiles().Length}");
Console.WriteLine($"Parent:  {di.Parent?.FullName}");

// 7. Special folders
string desktop  = Environment.GetFolderPath(Environment.SpecialFolder.Desktop);
string appData  = Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData);
string tempPath = Path.GetTempPath();
string tempFile = Path.GetTempFileName(); // creates a 0-byte temp file

Console.WriteLine($"Temp:    {tempPath}");
Console.WriteLine($"AppData: {appData}");

// 8. Path utilities
string full    = Path.GetFullPath("../relative/path");
string combined = Path.Combine("C:/data", "reports", "2026.csv");
string dir     = Path.GetDirectoryName(combined)!;
string name    = Path.GetFileNameWithoutExtension(combined); // 2026
string ext     = Path.GetExtension(combined);                // .csv
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you get the size of a file in C#?

```cs
// 1. FileInfo.Length — most common
var fi = new FileInfo("video.mp4");
Console.WriteLine($"Size: {fi.Length:N0} bytes");
Console.WriteLine($"Size: {fi.Length / 1_048_576.0:F2} MB");

// 2. File.OpenRead + Stream.Length
await using var fs = File.OpenRead("video.mp4");
Console.WriteLine($"Stream length: {fs.Length:N0} bytes");

// 3. Static helper that formats size
static string FormatSize(long bytes) => bytes switch
{
    < 1_024                 => $"{bytes} B",
    < 1_048_576             => $"{bytes / 1_024.0:F1} KB",
    < 1_073_741_824         => $"{bytes / 1_048_576.0:F2} MB",
    _                       => $"{bytes / 1_073_741_824.0:F2} GB",
};
Console.WriteLine(FormatSize(new FileInfo("video.mp4").Length));

// 4. Total size of a directory (recursive)
static long GetDirectorySize(string path) =>
    Directory.EnumerateFiles(path, "*", SearchOption.AllDirectories)
             .Sum(f => new FileInfo(f).Length);

Console.WriteLine(FormatSize(GetDirectorySize("C:/Users/me/Documents")));

// 5. Check before read (avoid loading huge files)
const long MaxAllowedBytes = 10 * 1_048_576; // 10 MB
var info = new FileInfo("upload.bin");
if (!info.Exists)         throw new FileNotFoundException();
if (info.Length > MaxAllowedBytes)
    throw new InvalidOperationException($"File too large: {FormatSize(info.Length)}");
string content = await File.ReadAllTextAsync(info.FullName);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are DLL files, and what are the advantages of using them?

A **DLL (Dynamic Link Library)** is a compiled binary (`.dll`) containing reusable code — types, methods, resources — that can be loaded and used by multiple applications at runtime.

In .NET, every class library project compiles to a DLL. The runtime loads assemblies on demand via the CLR.

```cs
// Create a class library (DLL)
// dotnet new classlib -n MathLibrary
namespace MathLibrary;

public static class Calculator
{
    public static int Add(int a, int b)      => a + b;
    public static double Sqrt(double value)  => Math.Sqrt(value);
}

// Reference it in another project
// dotnet add reference ../MathLibrary/MathLibrary.csproj
// Or via NuGet: dotnet add package MathLibrary

using MathLibrary;
Console.WriteLine(Calculator.Add(3, 4));  // 7
Console.WriteLine(Calculator.Sqrt(16));   // 4

// Load a DLL at runtime (plugin architecture)
using System.Reflection;

var assembly = Assembly.LoadFrom("plugins/MyPlugin.dll");
var type     = assembly.GetType("MyPlugin.PluginEntry")!;
var instance = Activator.CreateInstance(type)!;
var method   = type.GetMethod("Run")!;
method.Invoke(instance, null);
```

**Advantages of DLL files:**

| Advantage | Detail |
|-----------|--------|
| **Code reuse** | Share library across multiple apps without copying source |
| **Separation of concerns** | Isolate layers (data, business, UI) into separate assemblies |
| **Versioning** | Update a DLL independently without rebuilding all consumers |
| **Lazy loading** | CLR loads DLLs on first use — faster startup |
| **Plugin architecture** | Load DLLs dynamically at runtime |
| **Reduced memory** | OS can share pages of the same DLL across processes |
| **Encapsulation** | `internal` types hidden from consumers |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a `MemoryStream` in C#?

`MemoryStream` is a `Stream` implementation that stores data in **in-memory byte arrays** — no file system or network I/O. It is seekable, readable, and writable.

```cs
// 1. Basic write and read
await using var ms = new MemoryStream();

// Write
byte[] data = System.Text.Encoding.UTF8.GetBytes("Hello MemoryStream!");
await ms.WriteAsync(data);

// Reset position to read from beginning
ms.Position = 0;
using var reader = new StreamReader(ms, leaveOpen: true);
Console.WriteLine(await reader.ReadToEndAsync()); // Hello MemoryStream!

// 2. Pre-populated (wraps existing byte array — read-only)
byte[] existing = [1, 2, 3, 4, 5];
using var ro = new MemoryStream(existing);
Console.WriteLine(ro.ReadByte()); // 1

// 3. Serialise an object to bytes
var product = new { Id = 1, Name = "Laptop" };
await using var output = new MemoryStream();
await System.Text.Json.JsonSerializer.SerializeAsync(output, product);
byte[] json = output.ToArray(); // independent copy
Console.WriteLine(System.Text.Encoding.UTF8.GetString(json));
// {"Id":1,"Name":"Laptop"}

// 4. Use as pipe between two operations (no temp file)
await using var compressed = new MemoryStream();
await using (var gzip = new System.IO.Compression.GZipStream(
    compressed, System.IO.Compression.CompressionLevel.Fastest, leaveOpen: true))
{
    await gzip.WriteAsync("large text data to compress..."u8.ToArray());
}
Console.WriteLine($"Compressed: {compressed.Length} bytes");

// 5. ToArray vs GetBuffer
// ToArray()  — allocates a new byte[] trimmed to Length
// GetBuffer()— returns internal buffer (may have excess capacity, no copy)
byte[] trimmed  = ms.ToArray();          // safe, trimmed
byte[] raw      = ms.GetBuffer();        // fast, may be larger than ms.Length
long   capacity = ms.Capacity;
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `FileStream` class in C#?

`FileStream` provides **low-level, byte-oriented access** to files on disk. It is the foundation for all higher-level file classes (`StreamReader`, `StreamWriter`, `BinaryReader`, etc.) and offers fine-grained control over file mode, access, sharing, buffering, and async I/O.

```cs
// Constructor options
var fs = new FileStream(
    path:        "data.bin",
    mode:        FileMode.OpenOrCreate,  // Create, Open, Append, Truncate, CreateNew
    access:      FileAccess.ReadWrite,   // Read, Write, ReadWrite
    share:       FileShare.None,         // None, Read, Write, ReadWrite, Delete
    bufferSize:  4096,
    useAsync:    true);                  // enable async I/O (important on Windows)
await using var _ = fs;

// Write bytes
await fs.WriteAsync("Hello FileStream"u8.ToArray());

// Seek to start
fs.Seek(0, SeekOrigin.Begin);
// or: fs.Position = 0;

// Read bytes
var buf = new byte[16];
int read = await fs.ReadAsync(buf);
Console.WriteLine(System.Text.Encoding.UTF8.GetString(buf, 0, read)); // Hello FileStream

// Properties
Console.WriteLine($"Length:   {fs.Length}");
Console.WriteLine($"Position: {fs.Position}");
Console.WriteLine($"CanRead:  {fs.CanRead}");
Console.WriteLine($"CanWrite: {fs.CanWrite}");
Console.WriteLine($"CanSeek:  {fs.CanSeek}");

// Flush to disk — ensure OS buffers are written
await fs.FlushAsync();

// Practical: copy file with progress
static async Task CopyWithProgressAsync(string src, string dst, IProgress<double>? p = null)
{
    await using var r = new FileStream(src, FileMode.Open, FileAccess.Read, FileShare.Read, 65536, true);
    await using var w = new FileStream(dst, FileMode.Create, FileAccess.Write, FileShare.None, 65536, true);
    var buf = new byte[65536];
    long total = 0, len = r.Length;
    int n;
    while ((n = await r.ReadAsync(buf)) > 0)
    {
        await w.WriteAsync(buf.AsMemory(0, n));
        total += n;
        p?.Report((double)total / len * 100);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `StreamReader` and `StreamWriter`?

| | `StreamReader` | `StreamWriter` |
|-|---------------|----------------|
| **Direction** | Read text from a Stream | Write text to a Stream |
| **Base class** | `TextReader` | `TextWriter` |
| **Key methods** | `Read`, `ReadLine`, `ReadToEnd`, `ReadLineAsync` | `Write`, `WriteLine`, `WriteAsync`, `WriteLineAsync` |
| **Encoding** | Detects BOM or uses specified encoding | Uses specified encoding (default UTF-8 with BOM) |
| **AutoFlush** | N/A | `AutoFlush` property — flush on every write |
| **EndOfStream** | `EndOfStream` property | N/A |

```cs
// StreamReader — reading
await using var reader = new StreamReader("data.txt", System.Text.Encoding.UTF8);

// Read one character
int ch = reader.Read(); // returns -1 at end

// Read one line
string? line = await reader.ReadLineAsync();

// Read entire file
string all = await reader.ReadToEndAsync();

// Iterate lines
while (!reader.EndOfStream)
    Console.WriteLine(await reader.ReadLineAsync());

// StreamWriter — writing
await using var writer = new StreamWriter("output.txt",
    append: false, encoding: System.Text.Encoding.UTF8)
{
    AutoFlush = false, // batch writes for performance
    NewLine   = "\n",  // Unix-style line endings
};

await writer.WriteAsync("no newline");
await writer.WriteLineAsync("with newline"); // appends NewLine
await writer.FlushAsync(); // explicit flush when AutoFlush = false

// Both wrap the same FileStream
await using var shared = new FileStream("rw.txt", FileMode.OpenOrCreate, FileAccess.ReadWrite);
await using var sw = new StreamWriter(shared, leaveOpen: true);
await sw.WriteLineAsync("written");
await sw.FlushAsync();
shared.Position = 0;
using var sr = new StreamReader(shared, leaveOpen: true);
Console.WriteLine(await sr.ReadToEndAsync()); // written
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a `BinaryReader` in C#?

`BinaryReader` reads **primitive types** from a stream in binary format — the exact byte representation of `int`, `double`, `string`, `bool`, etc. It is the reading counterpart of `BinaryWriter`.

```cs
// Typical workflow: write with BinaryWriter, read with BinaryReader
// Writing
await using var writeFs = new FileStream("record.bin", FileMode.Create);
using var bw = new BinaryWriter(writeFs, System.Text.Encoding.UTF8);

bw.Write(1001);             // int   — 4 bytes
bw.Write("Alice");          // string — length prefix + UTF-8 bytes
bw.Write(95_000.50m);       // decimal — 16 bytes
bw.Write(true);             // bool   — 1 byte
bw.Write(new byte[] { 0xDE, 0xAD, 0xBE, 0xEF }); // raw bytes

// Reading — must read in the SAME order as written
await using var readFs = new FileStream("record.bin", FileMode.Open);
using var br = new BinaryReader(readFs, System.Text.Encoding.UTF8);

int     id      = br.ReadInt32();       // 1001
string  name    = br.ReadString();      // Alice
decimal salary  = br.ReadDecimal();     // 95000.50
bool    active  = br.ReadBoolean();     // true
byte[]  magic   = br.ReadBytes(4);      // { 0xDE, 0xAD, 0xBE, 0xEF }

Console.WriteLine($"Id={id}, Name={name}, Salary={salary:C}, Active={active}");

// Available Read methods:
// ReadByte / ReadBytes(n)
// ReadInt16 / ReadInt32 / ReadInt64
// ReadUInt16 / ReadUInt32 / ReadUInt64
// ReadSingle (float) / ReadDouble
// ReadDecimal
// ReadBoolean
// ReadChar / ReadChars(n)
// ReadString (length-prefixed)
// PeekChar — look without advancing position

// Safe read with bounds check
try { int val = br.ReadInt32(); }
catch (EndOfStreamException) { Console.WriteLine("Unexpected end of file"); }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `BinaryWriter` class in C#?

`BinaryWriter` writes **primitive types** to a stream as their raw binary representation — compact, fast, and order-dependent. Ideal for custom binary file formats and inter-process/network protocols.

```cs
await using var fs = new FileStream("data.bin", FileMode.Create, FileAccess.Write);
using var bw = new BinaryWriter(fs, System.Text.Encoding.UTF8, leaveOpen: false);

// Write different types
bw.Write((byte)0xFF);          // 1 byte
bw.Write((short)32767);        // 2 bytes
bw.Write(42);                  // int — 4 bytes
bw.Write(123456789L);          // long — 8 bytes
bw.Write(3.14f);               // float — 4 bytes
bw.Write(3.141592653589793);   // double — 8 bytes
bw.Write(9999.99m);            // decimal — 16 bytes
bw.Write(true);                // bool — 1 byte
bw.Write('A');                 // char — 2 bytes (UTF-16)
bw.Write("Hello");             // length-prefixed string
bw.Write(new byte[] { 1, 2, 3 }); // raw byte array (no length prefix)
bw.Write(new byte[] { 10, 20, 30 }, offset: 0, count: 2); // partial array

// Flush and inspect size
bw.Flush();
Console.WriteLine($"File size: {fs.Length} bytes");

// Endianness: BinaryWriter/Reader use little-endian by default
// For big-endian, use BinaryPrimitives:
Span<byte> buf = stackalloc byte[4];
System.Buffers.Binary.BinaryPrimitives.WriteInt32BigEndian(buf, 42);
fs.Write(buf);

// Practical: write a custom binary file header
public static void WriteFileHeader(BinaryWriter bw, int version, int recordCount)
{
    bw.Write("MYAPP"u8.ToArray());    // 5-byte magic
    bw.Write((byte)version);           // format version
    bw.Write(recordCount);             // number of records
    bw.Write(DateTimeOffset.UtcNow.ToUnixTimeSeconds()); // timestamp
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `TextReader` and `TextWriter`?

`TextReader` and `TextWriter` are **abstract base classes** for reading/writing sequences of characters. They decouple code from the underlying I/O source.

| | `TextReader` | `TextWriter` |
|-|-------------|-------------|
| **Direction** | Read characters/strings | Write characters/strings |
| **Key methods** | `Read`, `ReadLine`, `ReadToEnd`, `ReadBlock` | `Write`, `WriteLine`, `Flush` |
| **Concrete types** | `StreamReader`, `StringReader` | `StreamWriter`, `StringWriter` |
| **Async** | `ReadAsync`, `ReadLineAsync`, `ReadToEndAsync` | `WriteAsync`, `WriteLineAsync`, `FlushAsync` |
| **Null** | `TextReader.Null` (discards reads) | `TextWriter.Null` (discards writes) |

```cs
// TextReader — accept any source
static async Task ProcessTextAsync(TextReader reader)
{
    string? line;
    while ((line = await reader.ReadLineAsync()) is not null)
        Console.WriteLine(line.ToUpper());
}

// Works with StreamReader (file)
await using var fileReader = new StreamReader("data.txt");
await ProcessTextAsync(fileReader);

// Works with StringReader (in-memory string)
using var stringReader = new StringReader("Hello\nWorld\n.NET 10");
await ProcessTextAsync(stringReader);

// TextWriter — accept any destination
static async Task WriteOutputAsync(TextWriter writer, IEnumerable<string> lines)
{
    foreach (var line in lines)
        await writer.WriteLineAsync(line);
    await writer.FlushAsync();
}

// Write to file
await using var fileWriter = new StreamWriter("output.txt");
await WriteOutputAsync(fileWriter, ["Line 1", "Line 2"]);

// Write to string (in-memory)
await using var stringWriter = new StringWriter();
await WriteOutputAsync(stringWriter, ["Line 1", "Line 2"]);
Console.WriteLine(stringWriter.ToString()); // Line 1\nLine 2\n

// Console.Out and Console.In are TextWriter/TextReader
await WriteOutputAsync(Console.Out, ["Hello console"]);
await ProcessTextAsync(Console.In); // reads from stdin
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a `StringReader` in C#?

`StringReader` is a `TextReader` that reads from an **in-memory string** instead of a file or network stream. It\'s useful for parsing strings using reader-based APIs without temporary files.

```cs
// 1. Basic line-by-line reading
string multiline = "Line 1\nLine 2\nLine 3";
using var reader = new StringReader(multiline);

string? line;
while ((line = reader.ReadLine()) is not null)
    Console.WriteLine(line);
// Line 1
// Line 2
// Line 3

// 2. Use async API (StringReader.ReadLineAsync returns immediately — no I/O)
using var asyncReader = new StringReader("Hello\nWorld");
while ((line = await asyncReader.ReadLineAsync()) is not null)
    Console.WriteLine(line);

// 3. Read one character at a time
using var cr = new StringReader("ABC");
int ch;
while ((ch = cr.Read()) != -1)
    Console.Write((char)ch); // A B C

// 4. PeekChar — look without advancing
using var pr = new StringReader("XYZ");
Console.WriteLine((char)pr.Peek()); // X — not consumed
Console.WriteLine((char)pr.Read()); // X — consumed

// 5. Parse CSV-like content without creating a temp file
static IEnumerable<string[]> ParseCsv(string csv)
{
    using var reader = new StringReader(csv);
    string? line;
    while ((line = reader.ReadLine()) is not null)
        yield return line.Split(',');
}

string data = "Alice,30,Engineer\nBob,25,Designer\nCarol,35,Manager";
foreach (var row in ParseCsv(data))
    Console.WriteLine($"Name={row[0]}, Age={row[1]}, Role={row[2]}");

// 6. Feed into XML/JSON parsers that accept TextReader
using var xmlReader = System.Xml.XmlReader.Create(new StringReader("<root><item>1</item></root>"));
while (xmlReader.Read())
    if (xmlReader.NodeType == System.Xml.XmlNodeType.Text)
        Console.WriteLine(xmlReader.Value); // 1
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `StringWriter` class in C#?

`StringWriter` is a `TextWriter` that writes to an **in-memory `StringBuilder`**. It lets you build strings using writer-based APIs without creating temporary files.

```cs
// 1. Build a string line by line
await using var sw = new StringWriter();
await sw.WriteLineAsync("Dear Alice,");
await sw.WriteLineAsync("Your order has shipped.");
await sw.WriteLineAsync("Regards, Shop");
string email = sw.ToString();
Console.WriteLine(email);

// 2. Use as a drop-in for any TextWriter parameter
static void GenerateReport(TextWriter writer, IEnumerable<string> items)
{
    writer.WriteLine("=== Report ===");
    int i = 1;
    foreach (var item in items)
        writer.WriteLine($"{i++}. {item}");
}

// Write to console
GenerateReport(Console.Out, ["Apple", "Banana"]);

// Write to string
await using var capture = new StringWriter();
GenerateReport(capture, ["Apple", "Banana"]);
string report = capture.ToString();

// Write to file
await using var file = new StreamWriter("report.txt");
GenerateReport(file, ["Apple", "Banana"]);

// 3. Serialise XML to string
await using var xmlSw = new StringWriter();
using var xmlWriter = System.Xml.XmlWriter.Create(xmlSw,
    new System.Xml.XmlWriterSettings { Indent = true, Async = true });
await xmlWriter.WriteStartElementAsync(null, "root", null);
await xmlWriter.WriteElementStringAsync(null, "name", null, "Alice");
await xmlWriter.WriteEndElementAsync();
await xmlWriter.FlushAsync();
Console.WriteLine(xmlSw.ToString());
// <root>
//   <name>Alice</name>
// </root>

// 4. Access the underlying StringBuilder directly
await using var sbw = new StringWriter();
await sbw.WriteAsync("Hello");
sbw.GetStringBuilder().Append(" World"); // direct access
Console.WriteLine(sbw.ToString()); // Hello World
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `XmlReader` and `XmlWriter`?

| | `XmlReader` | `XmlWriter` |
|-|------------|-------------|
| **Direction** | Forward-only read | Forward-only write |
| **Model** | Pull-parser (streaming, low memory) | Streaming writer |
| **Memory** | O(1) — reads one node at a time | O(1) — writes one node at a time |
| **Random access** | ❌ — forward only | ❌ — forward only |
| **Alternatives** | `XDocument.Load` (LINQ to XML, in-memory) | `XDocument.Save` (LINQ to XML) |

```cs
// XmlWriter — generate XML
var settings = new System.Xml.XmlWriterSettings
{
    Indent     = true,
    Async      = true,
    Encoding   = System.Text.Encoding.UTF8,
    OmitXmlDeclaration = false,
};

await using var sw = new StringWriter();
await using (var xw = System.Xml.XmlWriter.Create(sw, settings))
{
    await xw.WriteStartDocumentAsync();
    await xw.WriteStartElementAsync(null, "catalog", null);

    await xw.WriteStartElementAsync(null, "product", null);
    await xw.WriteAttributeStringAsync(null, "id", null, "1");
    await xw.WriteElementStringAsync(null, "name",  null, "Laptop");
    await xw.WriteElementStringAsync(null, "price", null, "1200");
    await xw.WriteEndElementAsync(); // product

    await xw.WriteEndElementAsync(); // catalog
    await xw.WriteEndDocumentAsync();
}
Console.WriteLine(sw.ToString());

// XmlReader — parse XML (streaming, low memory)
string xml = """
    <catalog>
      <product id="1"><name>Laptop</name><price>1200</price></product>
      <product id="2"><name>Phone</name><price>800</price></product>
    </catalog>
    """;

using var xmlReader = System.Xml.XmlReader.Create(
    new StringReader(xml),
    new System.Xml.XmlReaderSettings { Async = true });

while (await xmlReader.ReadAsync())
{
    if (xmlReader.NodeType == System.Xml.XmlNodeType.Element
        && xmlReader.Name == "name")
    {
        await xmlReader.ReadAsync(); // move to text node
        Console.WriteLine(xmlReader.Value); // Laptop, Phone
    }
}

// For simple scenarios, LINQ to XML (XDocument) is easier
var doc = System.Xml.Linq.XDocument.Parse(xml);
var names = doc.Descendants("name").Select(e => e.Value);
foreach (var n in names) Console.WriteLine(n); // Laptop, Phone
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a `JsonReader` in C#?

In modern .NET 10, the JSON reader is **`System.Text.Json.Utf8JsonReader`** — a high-performance, forward-only, ref struct that parses UTF-8 JSON without allocations. (`Newtonsoft.Json.JsonReader` is the legacy alternative.)

```cs
using System.Text.Json;

// 1. Utf8JsonReader — low-level, zero-allocation streaming
byte[] jsonBytes = """{"id":1,"name":"Alice","scores":[95,87,92]}"""u8.ToArray();
var reader = new Utf8JsonReader(jsonBytes, isFinalBlock: true, state: default);

while (reader.Read())
{
    switch (reader.TokenType)
    {
        case JsonTokenType.PropertyName:
            Console.Write($"{reader.GetString()}: ");
            break;
        case JsonTokenType.String:
            Console.WriteLine(reader.GetString());
            break;
        case JsonTokenType.Number:
            Console.WriteLine(reader.GetInt32());
            break;
        case JsonTokenType.StartArray:
        case JsonTokenType.EndArray:
        case JsonTokenType.StartObject:
        case JsonTokenType.EndObject:
            Console.WriteLine(reader.TokenType);
            break;
    }
}

// 2. JsonSerializer.Deserialize — high-level (most common)
string json = """{"id":1,"name":"Alice"}""";
var person = JsonSerializer.Deserialize<Person>(json);
Console.WriteLine($"{person!.Id}: {person.Name}"); // 1: Alice

// 3. JsonDocument — DOM-style read without strong typing
using var doc = JsonDocument.Parse(json);
JsonElement root = doc.RootElement;
Console.WriteLine(root.GetProperty("name").GetString()); // Alice

// 4. Streaming deserialisation for large JSON arrays
await using var stream = File.OpenRead("products.json");
await foreach (var product in JsonSerializer.DeserializeAsyncEnumerable<Product>(stream))
    Console.WriteLine(product!.Name);

record Person(int Id, string Name);
record Product(int Id, string Name, decimal Price);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `JsonWriter` class in C#?

In .NET 10, the JSON writer is **`System.Text.Json.Utf8JsonWriter`** — a high-performance, forward-only writer that produces UTF-8 JSON directly into a buffer or stream without intermediate string allocations.

```cs
using System.Text.Json;

// 1. Utf8JsonWriter — low-level, high-performance
await using var ms = new System.IO.MemoryStream();
await using var writer = new Utf8JsonWriter(ms,
    new JsonWriterOptions { Indented = true });

writer.WriteStartObject();
writer.WriteNumber("id", 42);
writer.WriteString("name", "Alice");
writer.WriteBoolean("active", true);
writer.WriteNull("middleName");

writer.WriteStartArray("scores");
writer.WriteNumberValue(95);
writer.WriteNumberValue(87);
writer.WriteNumberValue(92);
writer.WriteEndArray();

writer.WriteStartObject("address");
writer.WriteString("city",    "London");
writer.WriteString("country", "UK");
writer.WriteEndObject();

writer.WriteEndObject();
await writer.FlushAsync();

string json = System.Text.Encoding.UTF8.GetString(ms.ToArray());
Console.WriteLine(json);

// 2. JsonSerializer.Serialize — high-level (most common)
var person = new { Id = 42, Name = "Alice", Scores = new[] { 95, 87, 92 } };
string json2 = JsonSerializer.Serialize(person,
    new JsonSerializerOptions { WriteIndented = true });
Console.WriteLine(json2);

// 3. Source-generated serialiser (.NET 10) — best performance, trimming-safe
[JsonSerializable(typeof(Product))]
internal partial class ProductContext : JsonSerializerContext { }

var product = new Product(1, "Laptop", 1200m);
string json3 = JsonSerializer.Serialize(product, ProductContext.Default.Product);
Console.WriteLine(json3); // {"Id":1,"Name":"Laptop","Price":1200}

// 4. Write directly to a file stream
await using var file = File.Create("output.json");
await using var fw = new Utf8JsonWriter(file, new JsonWriterOptions { Indented = true });
fw.WriteStartObject();
fw.WriteString("generated", DateTime.UtcNow.ToString("O"));
fw.WriteEndObject();
await fw.FlushAsync();

record Product(int Id, string Name, decimal Price);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `DataContractSerializer` and `XmlSerializer`?

| | `XmlSerializer` | `DataContractSerializer` |
|-|----------------|--------------------------|
| **Namespace** | `System.Xml.Serialization` | `System.Runtime.Serialization` |
| **Opt-in/out** | Opt-out (`[XmlIgnore]`) | Opt-in (`[DataMember]`) |
| **Private members** | ❌ Not serialised | ✅ With `[DataMember]` |
| **Inheritance** | Uses `[XmlInclude]` | Uses `[KnownType]` |
| **XML output** | More customisable (element names, attributes) | Less customisable, more strict |
| **Performance** | Slower (reflection-based) | Faster (generated code) |
| **Null handling** | Omits null elements by default | Serialises null with `xsi:nil="true"` |
| **Interfaces** | ❌ Cannot serialise | ❌ Cannot serialise |
| **Modern recommendation** | Use `System.Text.Json` instead | Use `System.Text.Json` instead |

```cs
using System.Xml.Serialization;
using System.Runtime.Serialization;

// XmlSerializer — attribute-heavy, customisable output
[Serializable]
public class ProductXml
{
    [XmlAttribute("product-id")]   public int Id { get; set; }
    [XmlElement("product-name")]   public string Name { get; set; } = "";
    [XmlIgnore]                    public string InternalCode { get; set; } = "";
}

var xmlSer = new XmlSerializer(typeof(ProductXml));
await using var sw = new StringWriter();
xmlSer.Serialize(sw, new ProductXml { Id = 1, Name = "Laptop" });
Console.WriteLine(sw.ToString());
// <ProductXml product-id="1"><product-name>Laptop</product-name></ProductXml>

// DataContractSerializer — WCF-style, opt-in
[DataContract]
public class ProductDcs
{
    [DataMember(Order = 1)] public int    Id   { get; set; }
    [DataMember(Order = 2)] public string Name { get; set; } = "";
    /* Not decorated — NOT serialised */   public string Internal { get; set; } = "";
}

var dcSer = new DataContractSerializer(typeof(ProductDcs));
await using var ms = new MemoryStream();
dcSer.WriteObject(ms, new ProductDcs { Id = 1, Name = "Laptop" });
ms.Position = 0;
var restored = (ProductDcs)dcSer.ReadObject(ms)!;
Console.WriteLine($"{restored.Id}: {restored.Name}");

// ✅ Modern recommendation: use System.Text.Json for new code
var json = System.Text.Json.JsonSerializer.Serialize(new { Id = 1, Name = "Laptop" });
Console.WriteLine(json); // {"Id":1,"Name":"Laptop"}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a `BinaryFormatter` in C#?

> ⚠️ **`BinaryFormatter` is obsolete and disabled by default since .NET 5, removed in .NET 9.** It had critical security vulnerabilities (arbitrary code execution via deserialization gadget chains). Do not use it in new or existing code.

```cs
// ❌ BinaryFormatter — OBSOLETE, INSECURE, REMOVED in .NET 9
// DO NOT USE:
// var formatter = new System.Runtime.Serialization.Formatters.Binary.BinaryFormatter();
// formatter.Serialize(stream, obj);   // throws NotSupportedException in .NET 9

// ✅ Modern replacements:

// 1. System.Text.Json — JSON (recommended for most scenarios)
var obj = new { Id = 1, Name = "Alice" };
string json = System.Text.Json.JsonSerializer.Serialize(obj);
byte[] jsonBytes = System.Text.Json.JsonSerializer.SerializeToUtf8Bytes(obj);

// 2. MessagePack — binary, compact, fast
// dotnet add package MessagePack
// byte[] msgpack = MessagePackSerializer.Serialize(obj);

// 3. System.Runtime.Serialization with DataContractSerializer (XML or JSON)
// (still available but verbose — prefer System.Text.Json)

// 4. MemoryPack — zero-encoding binary (.NET 10 friendly)
// dotnet add package MemoryPack
// byte[] packed = MemoryPackSerializer.Serialize(obj);

// 5. Custom binary with BinaryWriter (controlled, secure)
await using var ms = new MemoryStream();
using var bw = new BinaryWriter(ms);
bw.Write(1);       // Id
bw.Write("Alice"); // Name

ms.Position = 0;
using var br = new BinaryReader(ms);
Console.WriteLine($"{br.ReadInt32()}: {br.ReadString()}"); // 1: Alice
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `SoapFormatter` class in C#?

> ⚠️ **`SoapFormatter` is obsolete and removed in .NET Core / .NET 5+.** It was a WCF/SOAP-era serialiser that formatted object graphs as SOAP XML. Like `BinaryFormatter`, it had security vulnerabilities.

```cs
// ❌ SoapFormatter — .NET Framework only, OBSOLETE
// System.Runtime.Serialization.Formatters.Soap.SoapFormatter
// Not available in .NET 5+ / .NET Core at all

// ✅ Modern alternatives for SOAP/XML scenarios:

// 1. XmlSerializer — clean XML output (see above)
var ser = new System.Xml.Serialization.XmlSerializer(typeof(MyDto));
await using var sw = new StringWriter();
ser.Serialize(sw, new MyDto { Id = 1, Name = "Alice" });
Console.WriteLine(sw.ToString());

// 2. DataContractSerializer — WCF-compatible XML
var dcSer = new System.Runtime.Serialization.DataContractSerializer(typeof(MyDto));
await using var ms = new MemoryStream();
dcSer.WriteObject(ms, new MyDto { Id = 1, Name = "Alice" });

// 3. System.Text.Json — modern preferred serialiser
string json = System.Text.Json.JsonSerializer.Serialize(new MyDto { Id = 1, Name = "Alice" });

// 4. gRPC (Protobuf) — for service-to-service communication replacing SOAP
// dotnet add package Google.Protobuf Grpc.AspNetCore

public class MyDto
{
    public int    Id   { get; set; }
    public string Name { get; set; } = "";
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `BinaryFormatter` and `SoapFormatter`?

| | `BinaryFormatter` | `SoapFormatter` |
|-|-------------------|-----------------|
| **Format** | Compact binary (proprietary) | SOAP XML (verbose) |
| **Interop** | .NET only | Somewhat interoperable via SOAP |
| **Performance** | Faster, smaller payload | Slower, larger payload |
| **Security** | ❌ Critical vulnerabilities | ❌ Critical vulnerabilities |
| **Status in .NET 9+** | Removed | Removed (never in .NET Core) |
| **Replacement** | `System.Text.Json`, `BinaryWriter`, MessagePack | `XmlSerializer`, `DataContractSerializer`, gRPC |

```cs
// ❌ Both are obsolete — DO NOT USE in new code.

// ✅ Choose the right modern serialiser based on needs:

// Scenario → Recommended serialiser
// REST API payloads         → System.Text.Json
// Configuration files       → System.Text.Json / YAML
// Compact binary IPC        → MessagePack / MemoryPack
// Custom binary protocol    → BinaryWriter + BinaryReader
// XML interop / SOAP legacy → XmlSerializer / DataContractSerializer
// Service-to-service RPC    → gRPC (Protobuf)

// Example: MessagePack (compact binary, fast, secure)
// dotnet add package MessagePack

// [MessagePackObject]
// public class Person
// {
//     [Key(0)] public int    Id   { get; set; }
//     [Key(1)] public string Name { get; set; } = "";
// }
// byte[] bytes   = MessagePackSerializer.Serialize(new Person { Id = 1, Name = "Alice" });
// Person person  = MessagePackSerializer.Deserialize<Person>(bytes);
// Console.WriteLine($"{person.Id}: {person.Name}"); // 1: Alice
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Serialization?

**Serialization** is the process of converting an object\'s state into a format (bytes, JSON, XML, binary) that can be stored or transmitted. **Deserialization** is the reverse — reconstructing the object from that format.

```cs
// ── JSON Serialization (recommended in .NET 10) ────────────────────
using System.Text.Json;

record Person(int Id, string Name, DateTime BirthDate, List<string> Hobbies);

var person = new Person(1, "Alice", new DateTime(1995, 6, 15), ["Hiking", "Reading"]);

// Serialise to JSON string
string json = JsonSerializer.Serialize(person,
    new JsonSerializerOptions { WriteIndented = true });
Console.WriteLine(json);
// {
//   "Id": 1,
//   "Name": "Alice",
//   "BirthDate": "1995-06-15T00:00:00",
//   "Hobbies": ["Hiking","Reading"]
// }

// Deserialise from JSON string
Person restored = JsonSerializer.Deserialize<Person>(json)!;
Console.WriteLine($"{restored.Id}: {restored.Name}");

// Serialise to bytes (more efficient — no intermediate string)
byte[] bytes = JsonSerializer.SerializeToUtf8Bytes(person);
Person fromBytes = JsonSerializer.Deserialize<Person>(bytes)!;

// Async serialisation to file
await using var file = File.Create("person.json");
await JsonSerializer.SerializeAsync(file, person);

// Async deserialisation from file
await using var readFile = File.OpenRead("person.json");
Person fromFile = (await JsonSerializer.DeserializeAsync<Person>(readFile))!;

// ── XML Serialization ───────────────────────────────────────────────
[Serializable]
public class ProductXml { public int Id; public string Name = ""; }

var xmlSer = new System.Xml.Serialization.XmlSerializer(typeof(ProductXml));
await using var sw = new StringWriter();
xmlSer.Serialize(sw, new ProductXml { Id = 1, Name = "Laptop" });
Console.WriteLine(sw.ToString());

// ── Custom binary (no third-party, no security risk) ────────────────
await using var ms = new MemoryStream();
using var bw = new BinaryWriter(ms);
bw.Write(person.Id);
bw.Write(person.Name);
bw.Write(person.BirthDate.ToBinary());
bw.Write(person.Hobbies.Count);
foreach (var h in person.Hobbies) bw.Write(h);

ms.Position = 0;
using var br = new BinaryReader(ms);
int      id      = br.ReadInt32();
string   name    = br.ReadString();
DateTime birth   = DateTime.FromBinary(br.ReadInt64());
var hobbies      = Enumerable.Range(0, br.ReadInt32()).Select(_ => br.ReadString()).ToList();
Console.WriteLine($"Restored: {id} {name} [{string.Join(", ", hobbies)}]");
```

**Serialization types:**

| Type | Format | Use case |
|------|--------|---------|
| JSON (`System.Text.Json`) | Text — human readable | REST APIs, config, storage |
| XML (`XmlSerializer`) | Text — human readable | Interop, legacy SOAP |
| Binary (`BinaryWriter`) | Binary — compact | Custom protocols, file formats |
| MessagePack | Binary — compact | High-performance IPC, game data |
| Protobuf (gRPC) | Binary — compact | Service-to-service communication |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 8. REGULAR EXPRESSION

<br/>

## Q. What is a regular expression in C# and what is it used for?

A **regular expression (regex)** is a sequence of characters that defines a search pattern. In C#, regexes are provided by the `System.Text.RegularExpressions` namespace.

**Common use cases:**

| Use case | Example pattern |
|----------|----------------|
| Validate email | `^[\w\.-]+@[\w\.-]+\.\w{2,}$` |
| Validate phone | `^\+?[\d\s\-()]{7,15}$` |
| Extract dates | `\d{4}-\d{2}-\d{2}` |
| Parse log files | Named capture groups |
| Find/replace text | `Regex.Replace` |
| Split on patterns | `Regex.Split` |
| Scrape HTML/JSON | (use with caution) |

```cs
using System.Text.RegularExpressions;

string text = "Order #1234 placed on 2026-04-19 for $99.99";

// Check if a date exists
bool hasDate = Regex.IsMatch(text, @"\d{4}-\d{2}-\d{2}");
Console.WriteLine(hasDate); // True

// Extract the date
Match m = Regex.Match(text, @"\d{4}-\d{2}-\d{2}");
Console.WriteLine(m.Value);  // 2026-04-19

// Extract the order number
Match order = Regex.Match(text, @"#(\d+)");
Console.WriteLine(order.Groups[1].Value); // 1234

// Replace price format
string cleaned = Regex.Replace(text, @"\$[\d.]+", "[PRICE]");
Console.WriteLine(cleaned); // Order #1234 placed on 2026-04-19 for [PRICE]
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `Regex` class in C#? How do you create and use a regular expression?

The `Regex` class in `System.Text.RegularExpressions` is the primary API for working with regular expressions in .NET. It supports matching, replacing, splitting, and extracting.

```cs
using System.Text.RegularExpressions;

// 1. Static methods — convenient for one-off patterns
bool isMatch   = Regex.IsMatch("hello123", @"\d+");          // true — contains digits
Match match    = Regex.Match("hello123", @"\d+");             // first match
MatchCollection all = Regex.Matches("a1 b2 c3", @"\d");      // all matches
string replaced = Regex.Replace("foo bar", @"\s+", "_");      // foo_bar
string[] parts  = Regex.Split("one,two,,three", @",+");       // ["one","two","three"]

// 2. Instance Regex — reuse compiled pattern (faster for repeated use)
var re = new Regex(@"\d{4}-\d{2}-\d{2}", RegexOptions.Compiled);
Console.WriteLine(re.IsMatch("Today is 2026-04-19")); // true

// 3. Source-generated Regex — best performance (.NET 7+, AOT-safe)
// Place in a partial class:
Console.WriteLine(DatePattern().IsMatch("2026-04-19")); // true

// Key methods summary:
// IsMatch(input)       → bool — does pattern occur?
// Match(input)         → Match — first occurrence
// Matches(input)       → MatchCollection — all occurrences
// Replace(input, repl) → string — replace matches
// Split(input)         → string[] — split on pattern

[GeneratedRegex(@"\d{4}-\d{2}-\d{2}", RegexOptions.None)]
static partial Regex DatePattern();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you match a pattern in a string using regular expressions in C#?

```cs
using System.Text.RegularExpressions;

string input = "Phone: +44-20-7946-0958, Alt: 01632-960-500";

// 1. IsMatch — check if pattern exists anywhere
bool found = Regex.IsMatch(input, @"\d{4}");
Console.WriteLine(found); // true

// 2. Match — get the FIRST match
Match first = Regex.Match(input, @"\d[\d\-]+\d");
if (first.Success)
    Console.WriteLine($"First: {first.Value}"); // 44-20-7946-0958

// 3. Matches — get ALL matches (lazy enumeration)
foreach (Match m in Regex.Matches(input, @"\d[\d\-]+\d"))
    Console.WriteLine(m.Value);
// 44-20-7946-0958
// 01632-960-500

// 4. NextMatch — iterate manually
Match m2 = Regex.Match(input, @"\+?\d[\d\-]+\d");
while (m2.Success)
{
    Console.WriteLine(m2.Value);
    m2 = m2.NextMatch();
}

// 5. Match position and length
Match pos = Regex.Match(input, @"\d{4,}");
Console.WriteLine($"Value='{pos.Value}' at index {pos.Index}, length {pos.Length}");
// Value='7946' at index ...

// 6. Anchors — ^ (start), $ (end), \b (word boundary)
Console.WriteLine(Regex.IsMatch("hello", @"^\w+$")); // true — entire string is word chars
Console.WriteLine(Regex.IsMatch("hello world", @"^\w+$")); // false — contains space

// 7. Multiline matching
string multiline = "line1\nline2\nline3";
var lines = Regex.Matches(multiline, @"^\w+$", RegexOptions.Multiline);
foreach (Match l in lines) Console.WriteLine(l.Value); // line1, line2, line3
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you replace text in a string using regular expressions in C#?

```cs
using System.Text.RegularExpressions;

// 1. Simple replacement
string text = "The price is $12.50 and $7.99";
string result = Regex.Replace(text, @"\$[\d.]+", "[PRICE]");
Console.WriteLine(result); // The price is [PRICE] and [PRICE]

// 2. Back-references — reuse matched groups in replacement
string csv = "Smith, John; Doe, Jane";
// Reorder "Last, First" → "First Last"
string reordered = Regex.Replace(csv, @"(\w+),\s*(\w+)", "$2 $1");
Console.WriteLine(reordered); // John Smith; Jane Doe

// 3. Named groups in replacement
string log = "2026-04-19 ERROR something failed";
string reformatted = Regex.Replace(log,
    @"(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})",
    "${day}/${month}/${year}");
Console.WriteLine(reformatted); // 19/04/2026 ERROR something failed

// 4. MatchEvaluator — dynamic replacement via delegate
string sentence = "hello world foo bar";
string titleCase = Regex.Replace(sentence, @"\b\w+\b", m =>
    char.ToUpper(m.Value[0]) + m.Value[1..]);
Console.WriteLine(titleCase); // Hello World Foo Bar

// 5. Replace with count limit
string repeated = "aaa bbb ccc";
string limited = Regex.Replace(repeated, @"\b\w+\b", "X", count: 2);
Console.WriteLine(limited); // X X ccc

// 6. Source-generated replace (best performance)
string masked = MaskDigits().Replace("Card: 4111-1111-1111-1111", "*");
Console.WriteLine(masked); // Card: ****-****-****-****

[GeneratedRegex(@"\d")]
static partial Regex MaskDigits();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are some common use cases for regular expressions in C#?

```cs
using System.Text.RegularExpressions;

// 1. Email validation
bool IsValidEmail(string email) =>
    Regex.IsMatch(email, @"^[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}$");

Console.WriteLine(IsValidEmail("user@example.com")); // true
Console.WriteLine(IsValidEmail("bad@.com"));          // false

// 2. Phone number extraction
string text = "Call us: +1-800-555-0100 or 020 7946 0958";
foreach (Match m in Regex.Matches(text, @"\+?[\d][\d\s\-]{6,}\d"))
    Console.WriteLine(m.Value); // +1-800-555-0100, 020 7946 0958

// 3. URL extraction
string html = "<a href='https://example.com'>link</a> <a href='http://foo.org/path'>other</a>";
foreach (Match m in Regex.Matches(html, @"https?://[^\s'""]+"))
    Console.WriteLine(m.Value);

// 4. Password strength validation
bool IsStrongPassword(string pwd) =>
    Regex.IsMatch(pwd, @"^(?=.*[A-Z])(?=.*[a-z])(?=.*\d)(?=.*[\W_]).{8,}$");

Console.WriteLine(IsStrongPassword("Passw0rd!"));  // true
Console.WriteLine(IsStrongPassword("password"));   // false

// 5. Log file parsing with named groups
string log = "2026-04-19 14:30:55 ERROR [App] NullReferenceException";
Match logMatch = Regex.Match(log,
    @"(?<date>\d{4}-\d{2}-\d{2}) (?<time>[\d:]+) (?<level>\w+) \[(?<source>[^\]]+)\] (?<message>.+)");

Console.WriteLine($"Date:    {logMatch.Groups["date"].Value}");
Console.WriteLine($"Level:   {logMatch.Groups["level"].Value}");
Console.WriteLine($"Message: {logMatch.Groups["message"].Value}");

// 6. Sanitise / strip HTML tags
string stripped = Regex.Replace("<p>Hello <b>World</b></p>", @"<[^>]+>", "");
Console.WriteLine(stripped); // Hello World

// 7. Split on multiple delimiters
string[] tokens = Regex.Split("one,two;three|four", @"[,;|]");
Console.WriteLine(string.Join(" | ", tokens)); // one | two | three | four

// 8. Find duplicate words
string dupes = "the the cat sat on on the mat";
foreach (Match m in Regex.Matches(dupes, @"\b(\w+)\s+\1\b", RegexOptions.IgnoreCase))
    Console.WriteLine($"Duplicate: '{m.Value}'");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you validate an email address using regular expressions in C#?

```cs
using System.Text.RegularExpressions;

// 1. Source-generated (best — .NET 7+, AOT-safe, zero overhead)
public static partial class EmailValidator
{
    // RFC 5322 simplified — covers >99% of real-world addresses
    [GeneratedRegex(
        @"^[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}$",
        RegexOptions.IgnoreCase | RegexOptions.ExplicitCapture)]
    private static partial Regex EmailRegex();

    public static bool IsValid(string email) =>
        !string.IsNullOrWhiteSpace(email) && EmailRegex().IsMatch(email);
}

// Test
string[] emails =
[
    "user@example.com",        // ✅
    "user.name+tag@domain.co", // ✅
    "user@sub.domain.org",     // ✅
    "bad@.com",                // ❌
    "@nodomain",               // ❌
    "noDomainExtension@abc",   // ❌
    "spaces in@email.com",     // ❌
];

foreach (string email in emails)
    Console.WriteLine($"{email,-35} → {(EmailValidator.IsValid(email) ? "✅" : "❌")}");

// 2. Static Regex (cached instance — fine for most code)
var emailRegex = new Regex(
    @"^[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}$",
    RegexOptions.Compiled | RegexOptions.IgnoreCase);

bool valid = emailRegex.IsMatch("user@example.com");

// 3. MailAddress parse — .NET built-in alternative (handles more edge cases)
static bool IsValidEmail2(string email)
{
    try
    {
        var addr = new System.Net.Mail.MailAddress(email);
        return addr.Address == email.Trim();
    }
    catch { return false; }
}
Console.WriteLine(IsValidEmail2("user@example.com")); // true
Console.WriteLine(IsValidEmail2("notanemail"));        // false
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `Regex.Match` and `Regex.IsMatch` in C#?

| | `Regex.IsMatch` | `Regex.Match` |
|-|----------------|--------------|
| **Returns** | `bool` — does pattern exist? | `Match` object — first occurrence |
| **Performance** | Slightly faster (stops at first match, no object) | Allocates a `Match` object |
| **Use when** | You only need yes/no | You need position, value, or groups |
| **No match** | Returns `false` | Returns `Match` with `Success = false` |

```cs
using System.Text.RegularExpressions;

string input = "Order #4271 shipped on 2026-04-19";

// IsMatch — fastest, just need to know if pattern exists
bool hasOrder = Regex.IsMatch(input, @"#\d+");
Console.WriteLine(hasOrder); // true

// Match — need the actual value or position
Match m = Regex.Match(input, @"#(\d+)");
if (m.Success)
{
    Console.WriteLine($"Full match:  {m.Value}");          // #4271
    Console.WriteLine($"Group 1:     {m.Groups[1].Value}"); // 4271
    Console.WriteLine($"Index:       {m.Index}");           // position in string
    Console.WriteLine($"Length:      {m.Length}");
}

// Match with no match — always check Success before accessing Value
Match noMatch = Regex.Match(input, @"\d{8}");
Console.WriteLine(noMatch.Success); // false
// Console.WriteLine(noMatch.Value); // "" — safe to call but meaningless

// Matches — all occurrences
MatchCollection all = Regex.Matches(input, @"\d+");
foreach (Match each in all)
    Console.Write($"{each.Value} "); // 4271 2026 04 19
Console.WriteLine();

// Rule of thumb:
// Just validating?        → IsMatch
// Need value/groups?      → Match
// Need all occurrences?   → Matches
// Need replace/transform? → Replace + MatchEvaluator
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you extract groups from a match using regular expressions in C#?

```cs
using System.Text.RegularExpressions;

// 1. Numbered capture groups — ()
string log = "2026-04-19 14:32:05 ERROR NullReferenceException";
Match m = Regex.Match(log, @"(\d{4}-\d{2}-\d{2}) (\d{2}:\d{2}:\d{2}) (\w+) (.+)");

if (m.Success)
{
    Console.WriteLine($"Date:    {m.Groups[1].Value}"); // 2026-04-19
    Console.WriteLine($"Time:    {m.Groups[2].Value}"); // 14:32:05
    Console.WriteLine($"Level:   {m.Groups[3].Value}"); // ERROR
    Console.WriteLine($"Message: {m.Groups[4].Value}"); // NullReferenceException
}

// 2. Named capture groups — (?<name>...)  ← recommended
Match named = Regex.Match(log,
    @"(?<date>\d{4}-\d{2}-\d{2}) (?<time>[\d:]+) (?<level>\w+) (?<msg>.+)");

Console.WriteLine($"Date:  {named.Groups["date"].Value}");
Console.WriteLine($"Level: {named.Groups["level"].Value}");
Console.WriteLine($"Msg:   {named.Groups["msg"].Value}");

// 3. Multiple matches with groups
string data = "Alice:30, Bob:25, Carol:35";
foreach (Match person in Regex.Matches(data, @"(?<name>[A-Z]\w+):(?<age>\d+)"))
{
    Console.WriteLine($"{person.Groups["name"].Value} is {person.Groups["age"].Value}");
}
// Alice is 30
// Bob is 25
// Carol is 35

// 4. Optional groups — check Success on the group
Match optional = Regex.Match("+44 20 7946 0958",
    @"(?<country>\+\d{1,3})?\s?(?<number>[\d\s]{7,})");
Console.WriteLine(optional.Groups["country"].Success
    ? optional.Groups["country"].Value
    : "(no country code)");

// 5. Non-capturing group — (?:...) — group without capturing
// Used for alternation or quantifiers without a group slot
Match nc = Regex.Match("colour or color",
    @"colo(?:u)?r"); // (?:u)? — optional 'u', not captured
Console.WriteLine(nc.Value); // colour
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle case sensitivity in regular expressions in C#?

```cs
using System.Text.RegularExpressions;

string input = "Hello WORLD hello world";

// 1. Case-sensitive by default
MatchCollection sensitive = Regex.Matches(input, @"hello");
Console.WriteLine(sensitive.Count); // 1 — only lowercase "hello"

// 2. Case-insensitive via RegexOptions.IgnoreCase
MatchCollection insensitive = Regex.Matches(input, @"hello", RegexOptions.IgnoreCase);
Console.WriteLine(insensitive.Count); // 2 — "Hello" and "hello" (lowercase only, HELLO matches too)

// Wait — actually "Hello" and "hello" and "HELLO" would all match
MatchCollection all = Regex.Matches(input, @"hello", RegexOptions.IgnoreCase);
foreach (Match m in all)
    Console.WriteLine($"'{m.Value}' at {m.Index}"); // Hello, hello

// 3. Inline flag (?i) — embed in pattern (useful for partial case-insensitivity)
Match m1 = Regex.Match("HTTP/1.1 200 OK", @"(?i)http");
Console.WriteLine(m1.Success); // true

// Disable case-insensitive for part of pattern with (?-i)
Match m2 = Regex.Match("fooBAR", @"(?i)foo(?-i)BAR"); // foo case-insensitive, BAR must be exact
Console.WriteLine(m2.Success); // true
Match m3 = Regex.Match("foobar", @"(?i)foo(?-i)BAR");
Console.WriteLine(m3.Success); // false — "bar" doesn\'t match "BAR"

// 4. Source-generated with IgnoreCase
string email = "User@Example.COM";
Console.WriteLine(ValidEmail().IsMatch(email)); // true

[GeneratedRegex(@"^[\w.%+\-]+@[\w.\-]+\.[a-z]{2,}$", RegexOptions.IgnoreCase)]
static partial Regex ValidEmail();

// 5. Combined options
var re = new Regex(@"hello\s+world",
    RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.Singleline);
Console.WriteLine(re.IsMatch("HELLO\nWORLD")); // true (Singleline: . matches \n)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a precompiled Regex object in .NET?

There are three levels of "precompiled" regex in modern .NET:

| Approach | Compilation | Performance | AOT-safe |
|----------|------------|-------------|----------|
| `new Regex(pattern)` | Interpreted (default) | Baseline | ✅ |
| `new Regex(pattern, RegexOptions.Compiled)` | JIT-compiled to IL | ~2–3× faster | ✅ |
| `[GeneratedRegex]` source generator (.NET 7+) | Compiled at build time | Fastest, no startup cost | ✅ |

```cs
using System.Text.RegularExpressions;

// 1. RegexOptions.Compiled — JIT-compiles to IL on first use
// Best for patterns used many times in a hot path
private static readonly Regex _emailRegex = new Regex(
    @"^[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}$",
    RegexOptions.Compiled | RegexOptions.IgnoreCase);

// Store as static readonly so compilation happens once per AppDomain
Console.WriteLine(_emailRegex.IsMatch("user@example.com")); // true

// 2. Source-generated Regex — [GeneratedRegex] (.NET 7+)
// Generates optimised C# code at BUILD TIME — no runtime compilation
// AOT-compatible, trim-safe, zero startup cost
public static partial class Patterns
{
    [GeneratedRegex(
        @"^[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}$",
        RegexOptions.IgnoreCase | RegexOptions.ExplicitCapture,
        matchTimeoutMilliseconds: 1000)]
    public static partial Regex Email();

    [GeneratedRegex(@"\d{4}-\d{2}-\d{2}")]
    public static partial Regex IsoDate();

    [GeneratedRegex(@"(?<scheme>https?)://(?<host>[^/\s]+)(?<path>/[^\s]*)?")]
    public static partial Regex Url();
}

// Usage
Console.WriteLine(Patterns.Email().IsMatch("user@example.com")); // true
Console.WriteLine(Patterns.IsoDate().IsMatch("2026-04-19"));     // true

Match url = Patterns.Url().Match("https://example.com/path?q=1");
Console.WriteLine(url.Groups["scheme"].Value); // https
Console.WriteLine(url.Groups["host"].Value);   // example.com
Console.WriteLine(url.Groups["path"].Value);   // /path?q=1

// 3. Performance comparison
// Interpreted (~1×) → Compiled (~3×) → GeneratedRegex (~5×+)
// Use GeneratedRegex for new code targeting .NET 7+
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 9. EXCEPTION HANDLING

<br/>

## Q. What is exception handling in C# and why is it important?

**Exception handling** is the mechanism for responding to runtime errors in a controlled way, preventing application crashes and allowing graceful recovery or meaningful error reporting.

**Why it matters:**
- Prevents unhandled crashes from terminating the application
- Separates error-handling code from normal logic
- Provides structured information (stack trace, message, inner exception) for debugging
- Enables resource cleanup via `finally` / `using`

```cs
// Without exception handling — crash on bad input
int.Parse("abc"); // FormatException — app crashes

// With exception handling — graceful degradation
try
{
    int value = int.Parse("abc");
    Console.WriteLine(value);
}
catch (FormatException ex)
{
    Console.WriteLine($"Invalid number format: {ex.Message}");
}
finally
{
    Console.WriteLine("Always runs — clean up resources here");
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle exceptions in C#? What is the difference between `try`, `catch`, and `finally` blocks?

```cs
// try   — code that might throw
// catch — handles a specific exception type
// finally — always runs (cleanup), whether or not an exception occurred

try
{
    string[] lines = await File.ReadAllLinesAsync("data.csv"); // may throw
    int count = lines.Length;
    Console.WriteLine($"Lines: {count}");
}
catch (FileNotFoundException ex)
{
    Console.WriteLine($"File not found: {ex.FileName}");
}
catch (UnauthorizedAccessException ex)
{
    Console.WriteLine($"No permission: {ex.Message}");
}
catch (IOException ex)
{
    Console.WriteLine($"I/O error: {ex.Message}");
}
catch (Exception ex) // catch-all — least specific last
{
    Console.WriteLine($"Unexpected: {ex.GetType().Name} — {ex.Message}");
}
finally
{
    // always executed — even if return or exception in catch
    Console.WriteLine("Cleanup complete");
}

// Exception filters — when clause (.NET 6+ idiomatic)
try
{
    using var client = new HttpClient();
    string data = await client.GetStringAsync("https://api.example.com/data");
}
catch (HttpRequestException ex) when (ex.StatusCode == System.Net.HttpStatusCode.NotFound)
{
    Console.WriteLine("Resource not found (404)");
}
catch (HttpRequestException ex) when (ex.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
    Console.WriteLine("Unauthorized (401)");
}
catch (TaskCanceledException)
{
    Console.WriteLine("Request timed out");
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `Exception` class, its key properties, and which class is the base for all exceptions?

`System.Exception` is the **base class for all exceptions** in .NET. Every exception ultimately derives from it.

```
System.Exception
├── System.SystemException          (CLR/runtime exceptions)
│   ├── NullReferenceException
│   ├── IndexOutOfRangeException
│   ├── InvalidOperationException
│   ├── ArgumentException
│   │   ├── ArgumentNullException
│   │   └── ArgumentOutOfRangeException
│   ├── IOException
│   │   └── FileNotFoundException
│   ├── OverflowException
│   ├── FormatException
│   ├── StackOverflowException
│   └── OutOfMemoryException
└── System.ApplicationException     (user/app exceptions — rarely used directly)
```

**Key properties:**

```cs
try
{
    throw new InvalidOperationException("Cannot process empty order",
        innerException: new ArgumentNullException("orderId"));
}
catch (Exception ex)
{
    Console.WriteLine(ex.Message);           // Cannot process empty order
    Console.WriteLine(ex.GetType().Name);    // InvalidOperationException
    Console.WriteLine(ex.StackTrace);        // full call stack
    Console.WriteLine(ex.Source);            // assembly where exception originated
    Console.WriteLine(ex.HResult);           // HRESULT error code (interop)
    Console.WriteLine(ex.HelpLink);          // optional URL for more info
    Console.WriteLine(ex.Data["key"]);       // custom key-value pairs
    Console.WriteLine(ex.InnerException?.Message); // ArgumentNullException message
    Console.WriteLine(ex.ToString());        // full exception string (type + message + stack)
}

// Adding custom data to an exception
var ex2 = new InvalidOperationException("Order failed");
ex2.Data["OrderId"]  = 1234;
ex2.Data["UserId"]   = "alice";
ex2.Data["Timestamp"] = DateTime.UtcNow;
throw ex2;
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `InnerException` property in C#?

`InnerException` preserves the **original cause** of an exception when it is caught and re-thrown inside a higher-level exception. This allows you to inspect the full exception chain.

```cs
// Wrapping a low-level exception with a high-level one
public async Task<Order> LoadOrderAsync(int id)
{
    try
    {
        return await _repository.GetByIdAsync(id);
    }
    catch (SqlException ex) // low-level DB exception
    {
        // Wrap with domain-level exception, preserving original as InnerException
        throw new OrderNotFoundException($"Order {id} could not be loaded.", innerException: ex);
    }
}

// Inspecting the chain
try
{
    await LoadOrderAsync(999);
}
catch (Exception ex)
{
    Console.WriteLine($"Top-level: {ex.Message}");

    Exception? inner = ex.InnerException;
    while (inner is not null)
    {
        Console.WriteLine($"  Caused by: [{inner.GetType().Name}] {inner.Message}");
        inner = inner.InnerException;
    }
}

// Flatten AggregateException inner exceptions
try
{
    await Task.WhenAll(
        Task.Run(() => throw new Exception("Task 1 failed")),
        Task.Run(() => throw new Exception("Task 2 failed")));
}
catch (AggregateException ae)
{
    foreach (var inner in ae.Flatten().InnerExceptions)
        Console.WriteLine($"  Inner: {inner.Message}");
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `throw` keyword? What is the difference between `throw`, `throw ex`, and `throw new`?

| | `throw` | `throw ex` | `throw new ExType(...)` |
|-|---------|-----------|------------------------|
| **Stack trace** | Preserved ✅ | Reset to current line ❌ | New exception, new trace |
| **Use when** | Re-throwing the same exception | ❌ Avoid — loses origin | Wrapping with context |
| **InnerException** | N/A | N/A | Preserve original as inner |

```cs
// throw — re-throw preserving original stack trace (ALWAYS prefer this)
try
{
    await File.ReadAllTextAsync("missing.txt");
}
catch (FileNotFoundException)
{
    // log, then re-throw — stack trace points to the original throw site
    Console.WriteLine("Logged the error");
    throw; // ✅ preserves full stack trace
}

// throw ex — resets stack trace (AVOID)
// catch (FileNotFoundException ex)
// {
//     throw ex; // ❌ stack trace now starts HERE, original location lost
// }

// throw new — wrap with context (preserve original as InnerException)
try
{
    await LoadConfigAsync("appsettings.json");
}
catch (IOException ex)
{
    // Add domain context while preserving original exception
    throw new ApplicationException("Failed to start: config unavailable.", innerException: ex); // ✅
}

// throw new without inner — only use when starting a fresh exception
static void ValidateAge(int age)
{
    if (age < 0) throw new ArgumentOutOfRangeException(nameof(age), "Age cannot be negative.");
    if (age > 150) throw new ArgumentOutOfRangeException(nameof(age), "Age is unrealistically large.");
}

// ExceptionDispatchInfo — re-throw from a different context preserving stack trace
using System.Runtime.ExceptionServices;

Exception? captured = null;
var t = new Thread(() =>
{
    try { throw new InvalidOperationException("From thread"); }
    catch (Exception ex) { captured = ex; }
});
t.Start(); t.Join();

if (captured is not null)
    ExceptionDispatchInfo.Capture(captured).Throw(); // re-throws with original stack trace
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a custom exception in C#?

```cs
// Best practice custom exception — sealed, with standard constructors
[Serializable]
public sealed class OrderNotFoundException : Exception
{
    public int OrderId { get; }

    // Standard constructors (required for full compatibility)
    public OrderNotFoundException()
        : base("Order was not found.") { }

    public OrderNotFoundException(string message)
        : base(message) { }

    public OrderNotFoundException(string message, Exception innerException)
        : base(message, innerException) { }

    // Domain-specific constructor
    public OrderNotFoundException(int orderId)
        : base($"Order {orderId} was not found.")
    {
        OrderId = orderId;
    }

    public OrderNotFoundException(int orderId, Exception innerException)
        : base($"Order {orderId} was not found.", innerException)
    {
        OrderId = orderId;
    }
}

// Exception hierarchy for a domain
public abstract class DomainException(string message, Exception? inner = null)
    : Exception(message, inner);

public sealed class InsufficientInventoryException(string sku, int requested, int available)
    : DomainException($"Not enough stock for '{sku}': requested {requested}, available {available}")
{
    public string Sku       { get; } = sku;
    public int Requested    { get; } = requested;
    public int Available    { get; } = available;
}

// Usage
try
{
    throw new InsufficientInventoryException("LAPTOP-001", requested: 5, available: 2);
}
catch (InsufficientInventoryException ex)
{
    Console.WriteLine(ex.Message);   // Not enough stock for 'LAPTOP-001': requested 5, available 2
    Console.WriteLine(ex.Sku);       // LAPTOP-001
    Console.WriteLine(ex.Requested); // 5
    Console.WriteLine(ex.Available); // 2
}
catch (DomainException ex)
{
    Console.WriteLine($"Domain error: {ex.Message}");
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle multiple exceptions in a single `catch` block in C#?

```cs
// 1. Multiple catch blocks — most specific first
try
{
    ProcessData();
}
catch (ArgumentNullException ex)
{
    Console.WriteLine($"Null argument: {ex.ParamName}");
}
catch (ArgumentOutOfRangeException ex)
{
    Console.WriteLine($"Out of range: {ex.ParamName} = {ex.ActualValue}");
}
catch (ArgumentException ex) // catches both above if not already caught
{
    Console.WriteLine($"Bad argument: {ex.Message}");
}
catch (IOException ex)
{
    Console.WriteLine($"I/O error: {ex.Message}");
}

// 2. Multi-catch (C# 6+) — handle multiple types with the same logic
try
{
    ProcessData();
}
catch (FormatException ex) when (true) // pattern: always matches
{
    Console.WriteLine(ex.Message);
}
// Actually the idiomatic multi-type catch:
catch (Exception ex) when (ex is FormatException or OverflowException or InvalidCastException)
{
    Console.WriteLine($"Conversion error: {ex.Message}");
}

// 3. Exception filter with when — handle subset of one type
try
{
    using var client = new HttpClient();
    string result = await client.GetStringAsync("https://api.example.com");
}
catch (HttpRequestException ex) when ((int?)ex.StatusCode >= 500)
{
    Console.WriteLine("Server error — retry later");
}
catch (HttpRequestException ex) when ((int?)ex.StatusCode is 400 or 404)
{
    Console.WriteLine("Client error — check the request");
}

// 4. Catch-all (use sparingly)
try { ProcessData(); }
catch (Exception ex)
{
    Console.WriteLine($"Unhandled: {ex.GetType().Name} — {ex.Message}");
    // log and possibly rethrow
    throw;
}

void ProcessData() => throw new FormatException("Bad data format");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you log exceptions in C#?

```cs
// 1. Microsoft.Extensions.Logging (recommended — built into ASP.NET Core)
public class OrderService(ILogger<OrderService> logger)
{
    public async Task<Order?> GetOrderAsync(int id, CancellationToken ct = default)
    {
        try
        {
            return await _repository.GetByIdAsync(id, ct);
        }
        catch (OperationCanceledException)
        {
            logger.LogWarning("GetOrder {OrderId} was cancelled", id);
            throw;
        }
        catch (Exception ex)
        {
            // LogError overload with exception automatically captures type + stack trace
            logger.LogError(ex, "Failed to retrieve order {OrderId}", id);
            return null;
        }
    }
}

// 2. Serilog (popular structured logging library)
// dotnet add package Serilog.Sinks.Console Serilog.Sinks.File
using Serilog;

Log.Logger = new LoggerConfiguration()
    .WriteTo.Console()
    .WriteTo.File("logs/app-.log", rollingInterval: RollingInterval.Day)
    .CreateLogger();

try
{
    throw new InvalidOperationException("Something went wrong");
}
catch (Exception ex)
{
    Log.Error(ex, "Processing failed for {Context}", "OrderService");
}
finally
{
    await Log.CloseAndFlushAsync();
}

// 3. Global exception handlers (ASP.NET Core)
// In Program.cs:
// app.UseExceptionHandler(errorApp => errorApp.Run(async context => { ... }));

// 4. AppDomain / TaskScheduler unhandled handlers (console/worker apps)
AppDomain.CurrentDomain.UnhandledException += (_, e) =>
{
    var ex = e.ExceptionObject as Exception;
    Console.Error.WriteLine($"Fatal: {ex?.Message}");
    // log to file, flush sinks, etc.
};

TaskScheduler.UnobservedTaskException += (_, e) =>
{
    Console.Error.WriteLine($"Unobserved task exception: {e.Exception.Message}");
    e.SetObserved(); // prevent crash
};
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use the `using` statement to handle exceptions in C#?

The `using` statement ensures `IDisposable.Dispose()` is called even if an exception is thrown — equivalent to a `try/finally` block.

```cs
// 1. Classic using block
using (var stream = new FileStream("data.bin", FileMode.Open))
using (var reader = new BinaryReader(stream))
{
    int value = reader.ReadInt32();
    Console.WriteLine(value);
} // Dispose called here, even if ReadInt32 throws

// 2. Using declaration (C# 8+) — disposed at end of enclosing scope
await using var writer = new StreamWriter("output.txt");
await writer.WriteLineAsync("Hello");
// writer.Dispose() called when scope exits (exception or normal)

// 3. Using + try/catch — handle exception AND ensure disposal
StreamReader? reader2 = null;
try
{
    reader2 = new StreamReader("data.txt");
    string content = await reader2.ReadToEndAsync();
    ProcessContent(content);
}
catch (FileNotFoundException ex)
{
    Console.WriteLine($"File not found: {ex.FileName}");
}
finally
{
    reader2?.Dispose(); // manual disposal if not using 'using'
}

// Preferred: using + catch via nesting
try
{
    await using var fs = File.OpenRead("data.txt");
    // process...
}
catch (FileNotFoundException ex)
{
    Console.WriteLine($"Missing: {ex.Message}");
}

// 4. IAsyncDisposable — await using
public class AsyncResource : IAsyncDisposable
{
    public async ValueTask DisposeAsync()
    {
        await Task.Delay(10); // flush async
        Console.WriteLine("AsyncResource disposed");
    }
}

await using var res = new AsyncResource();
// ... use res
// DisposeAsync called at scope end

void ProcessContent(string s) { }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between checked and unchecked exceptions in C#?

**Note:** C# does not have checked/unchecked exceptions in the Java sense (forced `throws` declarations). Instead, `checked`/`unchecked` in C# refer to **arithmetic overflow behaviour**.

```cs
int max = int.MaxValue; // 2,147,483,647

// checked — throws OverflowException on arithmetic overflow
try
{
    checked
    {
        int result = max + 1; // throws OverflowException
        Console.WriteLine(result);
    }
}
catch (OverflowException ex)
{
    Console.WriteLine($"Overflow caught: {ex.Message}");
}

// checked expression
int safe = checked(max + 1); // also throws OverflowException

// unchecked — wraps around silently (default behaviour)
unchecked
{
    int wrapped = max + 1;
    Console.WriteLine(wrapped); // -2,147,483,648 (wraps around)
}

int wrapped2 = unchecked(max + 1); // expression form

// Default is unchecked for performance
int x = int.MaxValue + 1; // silently wraps to int.MinValue — no exception

// Enable checked globally via project setting:
// <CheckForOverflowUnderflow>true</CheckForOverflowUnderflow> in .csproj

// For floating point — overflow produces Infinity, not an exception
double bigDouble = double.MaxValue * 2;
Console.WriteLine(bigDouble);          // Infinity (no exception)
Console.WriteLine(double.IsInfinity(bigDouble)); // true
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle exceptions in asynchronous methods and in methods that return a Task?

```cs
// 1. async/await — exceptions propagate naturally via await
async Task<string> LoadDataAsync(string url, CancellationToken ct = default)
{
    try
    {
        using var client = new HttpClient();
        return await client.GetStringAsync(url, ct);
    }
    catch (HttpRequestException ex)
    {
        Console.WriteLine($"HTTP error {ex.StatusCode}: {ex.Message}");
        return string.Empty;
    }
    catch (OperationCanceledException)
    {
        Console.WriteLine("Request cancelled");
        return string.Empty;
    }
}

// Caller catches like synchronous code
try
{
    string data = await LoadDataAsync("https://api.example.com");
}
catch (Exception ex)
{
    Console.WriteLine($"Outer catch: {ex.Message}");
}

// 2. Task.WhenAll — AggregateException wraps all failures
Task[] tasks =
[
    Task.Run(() => throw new Exception("T1 failed")),
    Task.Run(() => throw new Exception("T2 failed")),
    Task.Run(() => Console.WriteLine("T3 OK")),
];

try
{
    await Task.WhenAll(tasks); // await re-throws first exception
}
catch (Exception ex)
{
    Console.WriteLine($"First error: {ex.Message}");
    // Inspect all faulted tasks
    foreach (var t in tasks.Where(t => t.IsFaulted))
        Console.WriteLine($"  {t.Exception!.InnerException!.Message}");
}

// 3. Task returning method — exception stored in Task, thrown on await
Task<int> ComputeAsync()
{
    return Task.Run(() =>
    {
        if (true) throw new InvalidOperationException("Compute failed");
        return 42;
    });
}

var task = ComputeAsync(); // exception not thrown yet — stored in task
try { int r = await task; } // exception thrown here
catch (InvalidOperationException ex) { Console.WriteLine(ex.Message); }

// 4. Fire-and-forget — must handle internally (no await = no propagation)
_ = Task.Run(async () =>
{
    try { await SomeBackgroundWorkAsync(); }
    catch (Exception ex) { Console.WriteLine($"Background error: {ex.Message}"); }
});

async Task SomeBackgroundWorkAsync() => await Task.Delay(100);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `AggregateException` class and when is it used?

`AggregateException` wraps one or more exceptions that occur during parallel or multi-task operations. It is thrown by `Task.WaitAll`, `Task.WhenAll` (when not awaited), and `Parallel.For` / `Parallel.ForEach`.

```cs
// 1. Task.WhenAll — use await to get individual exceptions via faulted tasks
Task[] tasks = [
    Task.Run(() => throw new ArgumentException("Bad arg")),
    Task.Run(() => throw new IOException("Disk error")),
    Task.Run(() => Console.WriteLine("OK")),
];

try
{
    await Task.WhenAll(tasks);
}
catch // await re-throws first exception — check all tasks for the rest
{
    var allErrors = tasks
        .Where(t => t.IsFaulted)
        .SelectMany(t => t.Exception!.InnerExceptions)
        .ToList();

    foreach (var ex in allErrors)
        Console.WriteLine($"{ex.GetType().Name}: {ex.Message}");
}

// 2. Task.WaitAll (synchronous) — throws AggregateException directly
try
{
    Task.WaitAll(tasks);
}
catch (AggregateException ae)
{
    ae.Handle(ex =>
    {
        if (ex is IOException ioEx)
        {
            Console.WriteLine($"I/O handled: {ioEx.Message}");
            return true; // handled
        }
        return false; // rethrow unhandled
    });
}

// 3. Flatten — collapse nested AggregateExceptions
try { Task.WaitAll(tasks); }
catch (AggregateException ae)
{
    foreach (var inner in ae.Flatten().InnerExceptions)
        Console.WriteLine($"  {inner.GetType().Name}: {inner.Message}");
}

// 4. Parallel.For — wraps iteration exceptions in AggregateException
try
{
    Parallel.For(0, 5, i =>
    {
        if (i == 2) throw new Exception($"Error at {i}");
    });
}
catch (AggregateException ae)
{
    foreach (var ex in ae.Flatten().InnerExceptions)
        Console.WriteLine(ex.Message);
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Will the `finally` block execute if an exception has not occurred? What is the C# syntax to catch any possible exception?

```cs
// finally ALWAYS runs — whether or not an exception occurred
// Exceptions: StackOverflowException, process kill, Environment.FailFast

// Case 1: No exception — finally still runs
try
{
    Console.WriteLine("try: no exception");
}
catch (Exception ex)
{
    Console.WriteLine($"catch: {ex.Message}"); // NOT reached
}
finally
{
    Console.WriteLine("finally: always runs"); // ✅ runs
}

// Case 2: Exception handled — finally runs after catch
try
{
    throw new InvalidOperationException("oops");
}
catch (InvalidOperationException ex)
{
    Console.WriteLine($"catch: {ex.Message}");
}
finally
{
    Console.WriteLine("finally: runs after catch"); // ✅ runs
}

// Case 3: Exception NOT caught — finally still runs, then exception propagates
try
{
    try { throw new Exception("unhandled"); }
    finally { Console.WriteLine("finally: runs before propagation"); } // ✅ runs
}
catch (Exception ex) { Console.WriteLine($"outer catch: {ex.Message}"); }

// Case 4: return in try — finally still runs before the method returns
int Calculate()
{
    try    { return 42; }
    finally { Console.WriteLine("finally: runs even with return"); } // ✅
}
Console.WriteLine(Calculate()); // finally runs first, then returns 42

// Syntax to catch ANY exception:
try { /* ... */ }
catch (Exception ex) // catches all managed exceptions
{
    Console.WriteLine($"Caught: {ex.Message}");
}

// Or bare catch (legacy — don\'t use, doesn\'t capture exception reference):
// try { } catch { }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `System.ApplicationException` and `System.SystemException`?

| | `SystemException` | `ApplicationException` |
|-|-----------------|----------------------|
| **Thrown by** | The CLR / .NET runtime | User / application code |
| **Examples** | `NullReferenceException`, `IOException`, `OverflowException` | Custom exceptions (historically) |
| **Modern guidance** | Do not catch directly — catch specific types | ⚠️ **Obsolete pattern** — avoid |
| **Recommended now** | Catch specific `SystemException` subtypes | Derive directly from `Exception` |

```cs
// ⚠️ ApplicationException was originally meant as the base for app exceptions
// — this guidance was ABANDONED in .NET 2.0 — the pattern is now discouraged

// ❌ Old pattern (avoid)
// public class MyException : ApplicationException { }

// ✅ Modern pattern — derive directly from Exception
public sealed class OrderNotFoundException(int orderId)
    : Exception($"Order {orderId} not found.")
{
    public int OrderId { get; } = orderId;
}

// SystemException examples (thrown by CLR):
try
{
    string? s = null;
    _ = s!.Length;                   // NullReferenceException : SystemException
}
catch (NullReferenceException ex)    // specific subtype — preferred
{
    Console.WriteLine(ex.Message);
}

// Catching SystemException directly is an anti-pattern — too broad
// catch (SystemException ex) { } ← catches way too much

// Bottom line:
// - Catching 'Exception' is the correct catch-all
// - Catching 'systemException' or 'ApplicationException' directly — avoid
// - Derive custom exceptions from 'Exception' directly
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `StackOverflowException` and `OutOfMemoryException`?

| | `StackOverflowException` | `OutOfMemoryException` |
|-|------------------------|----------------------|
| **Cause** | Call stack exhausted (infinite/deep recursion) | Heap exhausted — no memory for allocation |
| **Recoverable** | ❌ Not catchable (terminates process in .NET) | ⚠️ Sometimes catchable but rarely recoverable |
| **Common trigger** | Infinite recursion, very deep call chains | Large allocations, memory leaks, huge arrays |
| **Prevention** | Add base cases, use iteration, `Span<T>` | Pool objects, use `ArrayPool`, reduce allocations |

```cs
// StackOverflowException — infinite recursion
// int Factorial(int n) => n == 0 ? 1 : n * Factorial(n); // BUG: missing base case
// → StackOverflowException — process terminates, cannot be caught!

// Correct: proper base case
int Factorial(int n) => n <= 1 ? 1 : n * Factorial(n - 1); // ✅

// Better for deep recursion: iterative or explicit stack
int FactorialIterative(int n)
{
    int result = 1;
    for (int i = 2; i <= n; i++) result *= i;
    return result;
}

// OutOfMemoryException — allocation failure
try
{
    // Allocating 10 GB array — likely to fail
    var huge = new byte[10L * 1024 * 1024 * 1024];
}
catch (OutOfMemoryException ex)
{
    Console.WriteLine($"OOM: {ex.Message}");
    // Rarely recoverable — GC.Collect() + trim memory pools then retry
}

// Preventing OOM:
// Use ArrayPool<T> for large temporary buffers
byte[] buffer = System.Buffers.ArrayPool<byte>.Shared.Rent(1024 * 1024);
try { /* use buffer */ }
finally { System.Buffers.ArrayPool<byte>.Shared.Return(buffer); }

// Stream large files instead of loading all into memory
await foreach (string line in File.ReadLinesAsync("huge.csv"))
    Console.WriteLine(line); // never loads full file
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Name different types of errors that can occur during program execution.

| Error type | When | Examples | Detectable at |
|-----------|------|---------|--------------|
| **Syntax error** | Build time | Missing `;`, mismatched `{}` | Compile time |
| **Semantic error** | Build time | Wrong types, undefined variable | Compile time |
| **Runtime exception** | Execution | `NullReferenceException`, `DivideByZeroException` | Runtime |
| **Logic error** | Execution | Wrong algorithm, off-by-one | Testing/runtime |
| **Stack overflow** | Execution | Infinite recursion | Runtime (fatal) |
| **OutOfMemory** | Execution | Huge allocation, memory leak | Runtime |
| **I/O error** | Execution | File not found, network down | Runtime |
| **Concurrency error** | Execution | Race conditions, deadlocks | Runtime (intermittent) |
| **Configuration error** | Startup | Missing appsettings, bad connstr | Runtime |

```cs
// Runtime exception — NullReferenceException
string? s = null;
try { _ = s!.Length; }
catch (NullReferenceException ex) { Console.WriteLine($"Runtime: {ex.Message}"); }

// Logic error — no exception, wrong result
int Average(int[] nums) => nums.Sum(); // BUG: forgot to divide by nums.Length
Console.WriteLine(Average([1, 2, 3])); // 6 instead of 2 — logic error

// Arithmetic error
try { int x = 10 / 0; }
catch (DivideByZeroException ex) { Console.WriteLine($"DivideByZero: {ex.Message}"); }

// Type conversion error
try { int x = int.Parse("abc"); }
catch (FormatException ex) { Console.WriteLine($"Format: {ex.Message}"); }

// Index out of range
try { var arr = new int[3]; _ = arr[5]; }
catch (IndexOutOfRangeException ex) { Console.WriteLine($"Index: {ex.Message}"); }

// Null argument
try { string.IsNullOrEmpty(null!); } // safe here, but:
try { ArgumentNullException.ThrowIfNull(null, "param"); }
catch (ArgumentNullException ex) { Console.WriteLine($"NullArg: {ex.Message}"); }

// I/O error
try { File.ReadAllText("missing.txt"); }
catch (FileNotFoundException ex) { Console.WriteLine($"File: {ex.Message}"); }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain the difference between the `finally` block and the `Finalize` method.

| | `finally` block | `Finalize` / destructor |
|-|----------------|------------------------|
| **Type** | Exception-handling construct | Object lifecycle method (GC) |
| **When runs** | End of `try/catch` block (deterministic) | When GC collects object (non-deterministic) |
| **Called by** | Developer code flow | Garbage Collector |
| **Purpose** | Cleanup after a try block | Release unmanaged resources as a safety net |
| **Control** | Full developer control | Non-deterministic timing |
| **Preferred alternative** | Inherent to `try` structure | `IDisposable.Dispose` + `using` |

```cs
// finally — deterministic cleanup, runs at end of try/catch
void ReadFile(string path)
{
    StreamReader? reader = null;
    try
    {
        reader = new StreamReader(path);
        Console.WriteLine(reader.ReadToEnd());
    }
    catch (IOException ex)
    {
        Console.WriteLine($"Error: {ex.Message}");
    }
    finally
    {
        reader?.Dispose(); // always runs
        Console.WriteLine("finally: reader disposed");
    }
}

// Finalize / destructor — GC safety net for unmanaged resources
public class UnmanagedWrapper
{
    private IntPtr _handle;

    public UnmanagedWrapper() => _handle = AllocateResource();

    // Finalizer — called by GC if Dispose was not called
    ~UnmanagedWrapper()
    {
        // Safety net only — do NOT rely on timing
        FreeResource(_handle);
        Console.WriteLine("Finalizer ran");
    }

    // ✅ Implement IDisposable for deterministic cleanup
    public void Dispose()
    {
        FreeResource(_handle);
        GC.SuppressFinalize(this); // tell GC: no need to finalize
        Console.WriteLine("Dispose called");
    }

    private static IntPtr AllocateResource() => new IntPtr(1);
    private static void FreeResource(IntPtr h) { }
}

// ✅ Always prefer using/Dispose over relying on finalizer
using var wrapper = new UnmanagedWrapper(); // Dispose called deterministically
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can multiple catch blocks execute for a single exception in C#? What are the most commonly used exception types in .NET?

**No.** Only **one** catch block executes per exception — the first matching block. Subsequent catch blocks are skipped.

```cs
// Only the FIRST matching catch block runs
try
{
    throw new ArgumentNullException("param");
}
catch (ArgumentNullException ex)
{
    Console.WriteLine($"1st: {ex.GetType().Name}"); // ✅ This runs
}
catch (ArgumentException ex)
{
    Console.WriteLine($"2nd: {ex.GetType().Name}"); // ❌ NEVER reached
}
catch (Exception ex)
{
    Console.WriteLine($"3rd: {ex.GetType().Name}"); // ❌ NEVER reached
}

// Correct ordering: most specific → most general
try { /* ... */ }
catch (FileNotFoundException ex) { /* most specific */ }
catch (IOException ex)           { /* less specific */ }
catch (Exception ex)             { /* catch-all last */ }
```

**Most commonly used exception types in .NET:**

| Exception | Cause |
|-----------|-------|
| `NullReferenceException` | Accessing member of null reference |
| `ArgumentNullException` | `null` passed where not allowed |
| `ArgumentException` | Invalid method argument |
| `ArgumentOutOfRangeException` | Argument outside valid range |
| `InvalidOperationException` | Object in wrong state for operation |
| `NotSupportedException` | Operation not supported |
| `NotImplementedException` | Method not yet implemented |
| `IndexOutOfRangeException` | Array index out of bounds |
| `KeyNotFoundException` | Dictionary key not found |
| `FormatException` | String format is invalid (e.g., `int.Parse`) |
| `OverflowException` | Arithmetic overflow in `checked` context |
| `DivideByZeroException` | Division by zero |
| `StackOverflowException` | Call stack exhausted |
| `OutOfMemoryException` | Heap exhausted |
| `IOException` | I/O operation failure |
| `FileNotFoundException` | File not found |
| `UnauthorizedAccessException` | No permission for operation |
| `TimeoutException` | Operation timed out |
| `TaskCanceledException` | Task was cancelled |
| `OperationCanceledException` | Async operation was cancelled |
| `HttpRequestException` | HTTP request failure |
| `SqlException` | SQL Server error |
| `AggregateException` | Multiple Task/Parallel exceptions |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the different ways to handle errors in C#?

```cs
// 1. try/catch/finally — standard structured handling
try { ProcessOrder(order); }
catch (OrderNotFoundException ex) { logger.LogWarning(ex, "Order not found"); }
catch (Exception ex) { logger.LogError(ex, "Unexpected error"); throw; }
finally { /* cleanup */ }

// 2. Exception filters — when clause
try { await CallExternalApiAsync(); }
catch (HttpRequestException ex) when ((int?)ex.StatusCode >= 500)
    { Console.WriteLine("Server error — retry"); }

// 3. Result pattern — no exceptions for expected failures (functional style)
public readonly record struct Result<T>(T? Value, string? Error, bool IsSuccess)
{
    public static Result<T> Ok(T value)    => new(value, null, true);
    public static Result<T> Fail(string e) => new(default, e, false);
}

Result<Order> result = TryGetOrder(id);
if (result.IsSuccess) Process(result.Value!);
else Console.WriteLine(result.Error);

// 4. Nullable return / null-coalescing — for optional data
Order? order = await _repo.FindAsync(id);
Order resolved = order ?? Order.Default;

// 5. TryXxx pattern — like int.TryParse
if (int.TryParse(input, out int value))
    Console.WriteLine(value);
else
    Console.WriteLine("Invalid number");

// 6. ArgumentException helpers (.NET 6+)
void Process(string name, IEnumerable<int> items, int count)
{
    ArgumentNullException.ThrowIfNull(name);
    ArgumentException.ThrowIfNullOrWhiteSpace(name);
    ArgumentNullException.ThrowIfNull(items);
    ArgumentOutOfRangeException.ThrowIfNegative(count);
    ArgumentOutOfRangeException.ThrowIfGreaterThan(count, 1000);
}

// 7. Global handlers — last-resort logging
AppDomain.CurrentDomain.UnhandledException += (_, e) =>
    Console.Error.WriteLine($"Fatal: {(e.ExceptionObject as Exception)?.Message}");

TaskScheduler.UnobservedTaskException += (_, e) =>
{
    Console.Error.WriteLine($"Unobserved: {e.Exception.Message}");
    e.SetObserved();
};

// 8. Polly — resilience policies (retry, circuit breaker, timeout)
// dotnet add package Polly
// var pipeline = new ResiliencePipelineBuilder()
//     .AddRetry(new RetryStrategyOptions { MaxRetryAttempts = 3 })
//     .AddTimeout(TimeSpan.FromSeconds(10))
//     .Build();
// await pipeline.ExecuteAsync(async ct => await CallApiAsync(ct));

Result<Order> TryGetOrder(int id) =>
    id > 0 ? Result<Order>.Ok(new Order()) : Result<Order>.Fail("Invalid id");
void Process(Order o) { }
record Order { public static Order Default { get; } = new(); }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 10. EVENTS AND DELEGATES

<br/>

## Q. What are delegates in C# and why are they used?

A **delegate** is a type-safe function pointer — a reference to a method with a specific signature. It allows methods to be passed as parameters, stored as variables, and invoked dynamically.

```cs
// Declare a delegate type
delegate int MathOp(int a, int b);

// Methods matching the signature
int Add(int a, int b) => a + b;
int Multiply(int a, int b) => a * b;

// Assign and invoke
MathOp op = Add;
Console.WriteLine(op(3, 4));  // 7

op = Multiply;
Console.WriteLine(op(3, 4));  // 12

// Pass delegate as parameter
void ApplyOp(int x, int y, MathOp operation)
    => Console.WriteLine($"Result: {operation(x, y)}");

ApplyOp(5, 6, Add);       // Result: 11
ApplyOp(5, 6, Multiply);  // Result: 30

// Lambda as delegate
MathOp subtract = (a, b) => a - b;
Console.WriteLine(subtract(10, 3)); // 7

// Built-in generic delegates (prefer over custom)
Func<int, int, int> divide = (a, b) => a / b;
Action<string> print = msg => Console.WriteLine(msg);
Predicate<int> isEven = n => n % 2 == 0;

Console.WriteLine(divide(10, 2));   // 5
print("Hello delegates");
Console.WriteLine(isEven(4));       // True
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are Events? What is the difference between a delegate and an event?

An **event** is a mechanism for a class to notify subscribers when something happens. It is built on top of a delegate but restricted so only the declaring class can invoke it.

| | Delegate | Event |
|-|----------|-------|
| **Invocation** | Anyone can invoke | Only the declaring class |
| **Assignment** | `=` allowed externally | Only `+=` / `-=` externally |
| **Purpose** | General function reference | Publisher-subscriber notifications |
| **Null check** | Caller\'s responsibility | Raised via `?.Invoke` pattern |

```cs
// Delegate — anyone can invoke (no encapsulation)
public delegate void AlertHandler(string message);
public AlertHandler OnAlert;  // public field delegate — not recommended

// Event — only the declaring class can invoke
public class Button
{
    // event keyword wraps the delegate with access restrictions
    public event EventHandler<ButtonClickedEventArgs>? Clicked;

    public void Click()
    {
        // Only Button can raise the event
        Clicked?.Invoke(this, new ButtonClickedEventArgs("Left"));
    }
}

public class ButtonClickedEventArgs(string button) : EventArgs
{
    public string Button { get; } = button;
}

// Subscribe
var btn = new Button();
btn.Clicked += (sender, e) => Console.WriteLine($"Button clicked: {e.Button}");
btn.Click(); // Button clicked: Left

// btn.Clicked?.Invoke(...)  // ❌ compile error — external code cannot invoke event
// btn.Clicked = null;       // ❌ compile error — cannot assign externally
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you declare and raise an event? What is the purpose of the `EventHandler` delegate? How do you subscribe and unsubscribe?

```cs
// EventHandler<TEventArgs> — standard delegate for events:
//   void Handler(object? sender, TEventArgs e)

public class OrderProcessor
{
    // 1. Declare event using EventHandler<T> (standard pattern)
    public event EventHandler<OrderEventArgs>? OrderPlaced;
    public event EventHandler<OrderEventArgs>? OrderFailed;
    public event EventHandler? ProcessingCompleted; // no custom args

    // 2. Raise event — protected virtual method (allows derived class override)
    protected virtual void OnOrderPlaced(OrderEventArgs e)
        => OrderPlaced?.Invoke(this, e);

    protected virtual void OnOrderFailed(OrderEventArgs e)
        => OrderFailed?.Invoke(this, e);

    public async Task ProcessAsync(Order order)
    {
        try
        {
            await Task.Delay(100); // simulate work
            OnOrderPlaced(new OrderEventArgs(order.Id, "Placed successfully"));
        }
        catch (Exception ex)
        {
            OnOrderFailed(new OrderEventArgs(order.Id, ex.Message));
        }
        finally
        {
            ProcessingCompleted?.Invoke(this, EventArgs.Empty);
        }
    }
}

public class OrderEventArgs(int orderId, string message) : EventArgs
{
    public int OrderId  { get; } = orderId;
    public string Message { get; } = message;
}

// 3. Subscribe (+= ) and unsubscribe (-=)
var processor = new OrderProcessor();

void OnPlaced(object? sender, OrderEventArgs e)
    => Console.WriteLine($"Order {e.OrderId}: {e.Message}");

void OnCompleted(object? sender, EventArgs e)
    => Console.WriteLine("Processing completed");

processor.OrderPlaced          += OnPlaced;      // subscribe
processor.ProcessingCompleted  += OnCompleted;

await processor.ProcessAsync(new Order(1));

processor.OrderPlaced          -= OnPlaced;      // unsubscribe
processor.ProcessingCompleted  -= OnCompleted;

// Lambda subscription (keep reference to unsubscribe later)
EventHandler<OrderEventArgs>? failedHandler = null;
failedHandler = (_, e) => Console.WriteLine($"Failed: {e.Message}");
processor.OrderFailed += failedHandler;
processor.OrderFailed -= failedHandler; // unsubscribe using reference

record Order(int Id);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a multicast delegate (combinable delegate) in C#?

A **multicast delegate** holds references to **multiple methods** in an invocation list. Each `+=` adds a method; `-=` removes it. All methods are invoked in order when the delegate is called.

```cs
delegate void Notify(string message);

void Logger(string msg)  => Console.WriteLine($"[Log] {msg}");
void Emailer(string msg) => Console.WriteLine($"[Email] {msg}");
void Sms(string msg)     => Console.WriteLine($"[SMS] {msg}");

// Combine delegates
Notify notify = Logger;
notify += Emailer;
notify += Sms;

notify("Order shipped"); // all three called in order
// [Log] Order shipped
// [Email] Order shipped
// [SMS] Order shipped

// Inspect invocation list
foreach (var d in notify.GetInvocationList())
    Console.WriteLine(d.Method.Name); // Logger, Emailer, Sms

// Remove a handler
notify -= Emailer;
notify("Order delivered");
// [Log] Order delivered
// [SMS] Order delivered

// Return value — only LAST invoked delegate\'s return value is returned
delegate int Transform(int x);
Transform chain = x => x + 1;
chain += x => x * 2;
chain += x => x - 3;
int result = chain(5); // (5+1)=6, (5*2)=10, (5-3)=2 → only last: 2
Console.WriteLine(result); // 2

// Combine with Delegate.Combine
Notify a = Logger;
Notify b = Sms;
Notify combined = (Notify)Delegate.Combine(a, b)!;
combined("Test"); // Logger + Sms
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use anonymous methods with delegates in C#?

```cs
// Anonymous method (C# 2.0 syntax — mostly replaced by lambdas)
Func<int, int, int> add = delegate(int a, int b) { return a + b; };
Console.WriteLine(add(3, 4)); // 7

// Parameterless anonymous method
Action greet = delegate { Console.WriteLine("Hello!"); };
greet();

// With event subscription
button.Clicked += delegate(object? sender, EventArgs e)
{
    Console.WriteLine("Button clicked via anonymous method");
};

// Captures outer variable (closure)
int multiplier = 3;
Func<int, int> multiply = delegate(int x) { return x * multiplier; };
Console.WriteLine(multiply(5)); // 15
multiplier = 10;
Console.WriteLine(multiply(5)); // 50 — captures reference, not value!

// Modern equivalent — lambda (preferred)
Func<int, int, int> addLambda = (a, b) => a + b;
Action greetLambda = () => Console.WriteLine("Hello via lambda!");

// Anonymous method vs lambda comparison
Func<int, bool> isPositiveAnon   = delegate(int x) { return x > 0; };
Func<int, bool> isPositiveLambda = x => x > 0; // ← preferred

Button button = new();
record Button { public event EventHandler? Clicked; }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the advantages of using events and delegates? What is the difference between Event and Method?

**Advantages:**

| Feature | Benefit |
|---------|---------|
| **Loose coupling** | Publisher doesn\'t know about subscribers |
| **Multiple subscribers** | Multicast — many handlers for one event |
| **Type safety** | Delegate signature enforced at compile time |
| **Extensibility** | Add/remove handlers without changing publisher |
| **Callback pattern** | Pass methods as arguments (strategy pattern) |

```cs
// Without delegates — tight coupling
class OrderService
{
    private readonly EmailService _email = new();
    private readonly SmsService _sms = new();

    public void PlaceOrder(int id)
    {
        _email.Send($"Order {id} placed"); // tightly coupled
        _sms.Send($"Order {id} placed");
    }
}

// With delegates/events — loose coupling
class OrderServiceDecoupled
{
    public event EventHandler<int>? OrderPlaced;

    public void PlaceOrder(int id)
        => OrderPlaced?.Invoke(this, id); // doesn\'t know about Email/SMS
}

// Subscribers register independently
var svc = new OrderServiceDecoupled();
svc.OrderPlaced += (_, id) => new EmailService().Send($"Order {id}");
svc.OrderPlaced += (_, id) => new SmsService().Send($"Order {id}");

// Event vs Method
// Method — direct call, caller knows callee
void SendEmail(string msg) => Console.WriteLine($"Email: {msg}");
SendEmail("Direct call"); // caller must know method exists

// Event — indirect notification, publisher doesn\'t know subscribers
svc.OrderPlaced += (_, id) => Console.WriteLine($"Notification: order {id}");
svc.PlaceOrder(42); // publisher just fires — doesn\'t know who listens

class EmailService { public void Send(string m) => Console.WriteLine($"Email: {m}"); }
class SmsService   { public void Send(string m) => Console.WriteLine($"SMS: {m}"); }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use lambda expressions with delegates in C#? What is the difference between lambdas and delegates?

```cs
// Lambda IS a delegate — it satisfies any compatible delegate type
Func<int, int, int> sum      = (a, b) => a + b;
Action<string>      log      = msg  => Console.WriteLine(msg);
Predicate<string>   notEmpty = s   => !string.IsNullOrEmpty(s);

// Expression lambda (single expression, no return keyword)
Func<int, int> square = x => x * x;

// Statement lambda (block body)
Func<int, int> absoluteValue = x =>
{
    if (x < 0) return -x;
    return x;
};

// Custom delegate type
delegate bool Validator<T>(T value);
Validator<string> lengthOk = s => s.Length <= 100;
Console.WriteLine(lengthOk("hello")); // True

// Differences — lambda vs delegate
// Delegate: named type declaration
delegate int BinaryOp(int a, int b);
BinaryOp multiply = (a, b) => a * b;

// Lambda: anonymous inline expression — syntactic sugar over delegates
Func<int, int, int> multiply2 = (a, b) => a * b; // same as above

// Key differences:
// 1. Delegates can be non-generic; lambdas always need a target delegate type
// 2. Lambdas can be expression trees (Expression<Func<...>>); anonymous methods cannot
// 3. Lambdas are shorter and more readable

// Expression tree — lambda captured as data (used by EF Core, etc.)
using System.Linq.Expressions;
Expression<Func<int, bool>> expr = x => x > 5;
Console.WriteLine(expr);                  // x => (x > 5)
Console.WriteLine(expr.Compile()(10));    // True — compile and invoke

// Lambdas passed to LINQ
int[] numbers = [1, 2, 3, 4, 5, 6];
var evens = numbers.Where(n => n % 2 == 0).Select(n => n * n).ToList();
Console.WriteLine(string.Join(", ", evens)); // 4, 16, 36
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `Action`, `Func`, and `Predicate` delegates in C#?

| Delegate | Signature | Returns | Use when |
|----------|-----------|---------|----------|
| `Action` | `Action<T1, T2, …>` | `void` | Side-effect operations (print, log, update) |
| `Func` | `Func<T1, T2, …, TResult>` | `TResult` | Transformations, computations |
| `Predicate<T>` | `Predicate<T>` | `bool` | Tests a condition — equivalent to `Func<T, bool>` |

```cs
// Action — no return value
Action<string, int> repeat = (msg, n) =>
{
    for (int i = 0; i < n; i++) Console.WriteLine(msg);
};
repeat("Hello", 3);

Action<Exception> logError = ex => Console.Error.WriteLine(ex.Message);
logError(new Exception("Oops"));

// Func — returns a value (last type param is return type)
Func<string, int>          length   = s => s.Length;
Func<int, int, int>        add      = (a, b) => a + b;
Func<string, string, bool> contains = (s, sub) => s.Contains(sub);

Console.WriteLine(length("dotnet")); // 6
Console.WriteLine(add(3, 7));        // 10
Console.WriteLine(contains("hello world", "world")); // True

// Predicate<T> — equivalent to Func<T, bool>
Predicate<string> isLong      = s => s.Length > 10;
Predicate<int>    isPositive  = n => n > 0;

Console.WriteLine(isLong("Hello World!")); // True
Console.WriteLine(isPositive(-1));         // False

// List.FindAll uses Predicate<T>
var numbers = new List<int> { -3, -1, 0, 2, 5, 8 };
List<int> positives = numbers.FindAll(isPositive);
Console.WriteLine(string.Join(", ", positives)); // 2, 5, 8

// Func<string,string> vs custom delegate
// Func<string,string> — generic, no custom type needed
Func<string, string> toUpper = s => s.ToUpper();

// Custom delegate — allows named type with documentation
delegate string StringTransform(string input);
StringTransform transform = s => s.Trim().ToLower();

// Both work the same way — Func is preferred for brevity
Console.WriteLine(toUpper("hello")); // HELLO
Console.WriteLine(transform("  World  ")); // world
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement custom event accessors in C#?

Custom event accessors (`add` / `remove`) give explicit control over subscription — useful for thread-safety, filtering, or backing stores.

```cs
using System.Collections.Concurrent;

public class EventBus
{
    // Custom backing store — thread-safe dictionary of handlers
    private readonly ConcurrentDictionary<string, EventHandler> _handlers = new();

    // Custom event accessor
    public event EventHandler MessageReceived
    {
        add    => _handlers.AddOrUpdate("msg", value, (_, existing) => existing + value);
        remove => _handlers.AddOrUpdate("msg", null!, (_, existing) => existing - value);
    }

    public void Publish(string message)
    {
        if (_handlers.TryGetValue("msg", out var handler))
            handler?.Invoke(this, EventArgs.Empty);
    }
}

// Thread-safe event with lock (classic pattern)
public class SafeButton
{
    private readonly object _lock = new();
    private EventHandler? _clicked;

    public event EventHandler Clicked
    {
        add    { lock (_lock) { _clicked += value; } }
        remove { lock (_lock) { _clicked -= value; } }
    }

    // Raise safely
    protected virtual void OnClicked()
    {
        EventHandler? handler;
        lock (_lock) { handler = _clicked; } // copy under lock
        handler?.Invoke(this, EventArgs.Empty); // invoke outside lock
    }

    public void Click() => OnClicked();
}

// Usage
var btn = new SafeButton();
btn.Clicked += (_, _) => Console.WriteLine("Safe click handler 1");
btn.Clicked += (_, _) => Console.WriteLine("Safe click handler 2");
btn.Click();
// Safe click handler 1
// Safe click handler 2
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle events in a derived class in C#?

```cs
public class Shape
{
    // Declare event with protected virtual raise method
    public event EventHandler<ShapeEventArgs>? Drawn;

    protected virtual void OnDrawn(ShapeEventArgs e)
        => Drawn?.Invoke(this, e);

    public virtual void Draw()
    {
        Console.WriteLine("Drawing Shape");
        OnDrawn(new ShapeEventArgs("Shape"));
    }
}

public class Circle : Shape
{
    // Override the raise method to add derived-class behaviour
    protected override void OnDrawn(ShapeEventArgs e)
    {
        Console.WriteLine("Circle-specific drawing logic");
        base.OnDrawn(e); // fire the event from base
    }

    public override void Draw()
    {
        Console.WriteLine("Drawing Circle");
        base.Draw();
    }
}

public class ShapeEventArgs(string shapeName) : EventArgs
{
    public string ShapeName { get; } = shapeName;
}

// Subscribe at base-class level — works for derived types too
Shape shape = new Circle();
shape.Drawn += (_, e) => Console.WriteLine($"Event: {e.ShapeName} was drawn");
shape.Draw();
// Drawing Circle
// Circle-specific drawing logic
// Drawing Shape
// Event: Shape was drawn
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the best practices for using events and delegates in C#?

```cs
// 1. Always use EventHandler<TEventArgs> — avoids custom delegate declarations
public event EventHandler<OrderEventArgs>? OrderPlaced; // ✅
// public delegate void OrderHandler(Order o);          // ❌ unnecessary

// 2. Null-conditional invoke — thread-safe raise
OrderPlaced?.Invoke(this, new OrderEventArgs(1, "Placed")); // ✅
// if (OrderPlaced != null) OrderPlaced(this, ...);          // ❌ race condition

// 3. Protected virtual raise method — enables derived class extension
protected virtual void OnOrderPlaced(OrderEventArgs e)
    => OrderPlaced?.Invoke(this, e);

// 4. Always unsubscribe to prevent memory leaks
class Subscriber : IDisposable
{
    private readonly OrderProcessor _processor;

    public Subscriber(OrderProcessor processor)
    {
        _processor = processor;
        _processor.OrderPlaced += HandleOrderPlaced;
    }

    private void HandleOrderPlaced(object? sender, OrderEventArgs e)
        => Console.WriteLine($"Order {e.OrderId}: {e.Message}");

    public void Dispose()
        => _processor.OrderPlaced -= HandleOrderPlaced; // ✅ unsubscribe
}

// 5. Prefer Func/Action over custom delegates for simple cases
Func<int, bool>  isValid = x => x > 0;   // ✅ concise
Action<string>   log     = Console.WriteLine; // ✅

// 6. Use weak event patterns for long-lived publishers / short-lived subscribers
// (WeakEventManager in WPF; or manual WeakReference<T> in other scenarios)

// 7. EventArgs should be immutable (read-only properties)
public sealed class OrderEventArgs(int orderId, string message) : EventArgs
{
    public int OrderId    { get; } = orderId;   // ✅ read-only
    public string Message { get; } = message;
}

// 8. Do NOT raise events in constructors — subscribers may not be attached yet
// 9. Do NOT throw exceptions in event handlers — use try/catch inside handler
// 10. Name events with verb or verb-noun pairs: OrderPlaced, DataReceived, ErrorOccurred

class OrderProcessor
{
    public event EventHandler<OrderEventArgs>? OrderPlaced;
    protected virtual void OnOrderPlaced(OrderEventArgs e)
        => OrderPlaced?.Invoke(this, e);
}
class OrderEventArgs(int orderId, string message) : EventArgs
{
    public int OrderId { get; } = orderId;
    public string Message { get; } = message;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 11. GARBAGE COLLECTION

<br/>

## Q. What is garbage collection in .NET and how does it work?

The **Garbage Collector (GC)** is an automatic memory manager in the .NET runtime that allocates and reclaims heap memory for managed objects, eliminating the need for manual `free`/`delete` calls.

**How it works:**
1. Objects are allocated on the **managed heap**
2. The GC periodically checks which objects are **reachable** (via roots: stack variables, static fields, GC handles)
3. **Unreachable** objects are swept — their memory is reclaimed
4. **Surviving** objects are **compacted** (defragmentation) and promoted to higher generations

```cs
// Objects on managed heap — GC manages lifetime automatically
var list = new List<string>();          // heap allocation
list.Add("item");                       // more heap
list = null;                           // now unreachable → eligible for GC

// You never need to free managed objects — GC handles it
string s = new string('x', 1000);
s = null; // GC will reclaim when it runs next collection

// GC roots — objects reachable from these are NOT collected:
// - Local variables on the stack
// - Static fields
// - CPU registers
// - GC handles (pinned, strong, weak)

// Check GC memory info
var gcInfo = GC.GetGCMemoryInfo();
Console.WriteLine($"Heap size: {gcInfo.HeapSizeBytes:N0} bytes");
Console.WriteLine($"Fragmented: {gcInfo.FragmentedBytes:N0} bytes");
Console.WriteLine($"Total available: {gcInfo.TotalAvailableMemoryBytes:N0} bytes");

// GC notifications (server scenarios)
GC.RegisterForFullGCNotification(10, 10);
Console.WriteLine($"GC latency mode: {System.Runtime.GCSettings.LatencyMode}");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are Generation 0, Generation 1, and Generation 2 in garbage collection? How does the GC know when to clean up?

The GC uses a **generational** model based on the observation that recently allocated objects tend to die young. Objects are promoted through generations as they survive collections.

| Generation | Contains | GC frequency | Notes |
|-----------|---------|------------|-------|
| **Gen 0** | Newly allocated objects | Very frequent (ms) | Cheapest collection |
| **Gen 1** | Survived one Gen 0 | Less frequent | Buffer between Gen 0 and Gen 2 |
| **Gen 2** | Long-lived objects | Infrequent (seconds) | Static fields, caches, singletons |
| **LOH** | Objects ≥ 85,000 bytes | With Gen 2 | Large Object Heap — not compacted by default |

```cs
// Objects start in Gen 0
var obj = new object();
Console.WriteLine(GC.GetGeneration(obj)); // 0

// Force promotion for demonstration
GC.Collect(0); // collect Gen 0
GC.WaitForPendingFinalizers();
Console.WriteLine(GC.GetGeneration(obj)); // 1 (survived → promoted)

GC.Collect(1);
GC.WaitForPendingFinalizers();
Console.WriteLine(GC.GetGeneration(obj)); // 2 (survived again)

// Large objects go straight to LOH (Gen 2)
var large = new byte[100_000]; // ≥ 85KB → LOH
Console.WriteLine(GC.GetGeneration(large)); // 2

// How GC knows when to collect:
// 1. Gen 0 budget exhausted (allocations exceed threshold)
// 2. System memory pressure
// 3. Explicit GC.Collect() call
// 4. AppDomain unload

// GC phases: Mark → Sweep → Compact
// Mark   — traverse from roots, mark all reachable objects
// Sweep  — identify unreachable objects
// Compact — slide live objects together, update references

// Gen 0 metrics
Console.WriteLine($"Gen 0 collections: {GC.CollectionCount(0)}");
Console.WriteLine($"Gen 1 collections: {GC.CollectionCount(1)}");
Console.WriteLine($"Gen 2 collections: {GC.CollectionCount(2)}");
Console.WriteLine($"Total memory: {GC.GetTotalMemory(false):N0} bytes");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Does the garbage collector clean up primitive types?

**Primitive types** (value types like `int`, `double`, `bool`, `struct`) allocated on the **stack** are NOT managed by the GC — they are freed automatically when the stack frame is popped.

Only **reference types** on the **managed heap** are managed by the GC.

```cs
// Value types on the stack — NO GC involvement
int x = 42;          // stack — freed when method returns
double d = 3.14;     // stack
bool flag = true;    // stack

// Value types inside a class — ON the heap (as part of the object)
class DataHolder
{
    public int Count;     // on heap because DataHolder is a reference type
    public double Value;  // on heap
}
var holder = new DataHolder(); // holder reference on stack, object on heap → GC manages

// Struct on the stack
struct Point { public int X, Y; }
Point p = new Point { X = 1, Y = 2 }; // entirely on stack — NO GC

// Struct on the heap (boxed or inside a class/array)
object boxed = p;          // boxing — copied to heap → GC manages
Point[] points = new Point[10]; // array on heap, but Point values inline in array

// Summary:
// Primitive/value types on stack → freed by stack unwind (no GC)
// Reference types on heap → GC manages
// Boxed value types on heap → GC manages
// Value types as fields of heap objects → GC manages (as part of parent object)

Console.WriteLine($"Is value type: {typeof(int).IsValueType}");    // True
Console.WriteLine($"Is value type: {typeof(string).IsValueType}"); // False
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does the garbage collector behave when a class has a destructor (finalizer)?

Objects with **finalizers** are placed on the **finalization queue** when they become unreachable. The GC must run the finalizer before reclaiming memory, which requires **at least two GC cycles**.

```cs
// Object WITH finalizer — two-cycle collection
public class ResourceWithFinalizer
{
    public ResourceWithFinalizer() => Console.WriteLine("Created");

    // Finalizer (destructor syntax) — called by GC on a dedicated finalizer thread
    ~ResourceWithFinalizer()
    {
        Console.WriteLine("Finalized by GC");
        // GC thread — do NOT use Thread.CurrentThread, allocate large objects, etc.
    }
}

// Cycle 1: object found unreachable → moved to finalization queue (NOT reclaimed yet)
// Cycle 2: finalizer thread runs, GC reclaims memory

// ✅ Dispose pattern — call GC.SuppressFinalize to skip the second cycle
public class ManagedResource : IDisposable
{
    private bool _disposed;

    protected virtual void Dispose(bool disposing)
    {
        if (_disposed) return;
        if (disposing)
        {
            // free managed resources
        }
        // free unmanaged resources
        _disposed = true;
    }

    ~ManagedResource()
    {
        Dispose(disposing: false); // safety net — unmanaged only
    }

    public void Dispose()
    {
        Dispose(disposing: true);
        GC.SuppressFinalize(this); // remove from finalization queue → single cycle
    }
}

// Always use 'using' to call Dispose deterministically
using var res = new ManagedResource();
// Dispose called here — GC.SuppressFinalize prevents finalizer run

// Impact on GC:
// Without Dispose: 2 GC cycles, finalizer thread overhead, delayed reclamation
// With Dispose + SuppressFinalize: 1 GC cycle, no finalizer overhead
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can the garbage collector reclaim unmanaged resources? Can you force garbage collection? Is it a good practice?

**Unmanaged resources** (file handles, sockets, native memory via `Marshal.AllocHGlobal`, COM objects) are **NOT managed by the GC**. They must be released explicitly via `IDisposable` / finalizers.

```cs
// ❌ GC cannot free unmanaged resources — you must do it
var handle = System.Runtime.InteropServices.Marshal.AllocHGlobal(1024);
// ... use handle
System.Runtime.InteropServices.Marshal.FreeHGlobal(handle); // manual cleanup required

// ✅ Wrap in SafeHandle or IDisposable for automatic cleanup
public class NativeBuffer : IDisposable
{
    private IntPtr _ptr;
    private bool _disposed;

    public NativeBuffer(int size)
        => _ptr = System.Runtime.InteropServices.Marshal.AllocHGlobal(size);

    public void Dispose()
    {
        if (!_disposed)
        {
            System.Runtime.InteropServices.Marshal.FreeHGlobal(_ptr);
            _ptr = IntPtr.Zero;
            _disposed = true;
            GC.SuppressFinalize(this);
        }
    }

    ~NativeBuffer() => Dispose(); // safety net
}

using var buf = new NativeBuffer(1024);

// Forcing GC — GC.Collect()
GC.Collect();                         // collect all generations
GC.Collect(0);                        // collect Gen 0 only
GC.Collect(2, GCCollectionMode.Forced); // forced full collection
GC.WaitForPendingFinalizers();        // wait for finalizer thread to complete
GC.Collect();                         // collect finalizable objects

// ⚠️ Is it good practice to force GC?
// ❌ Almost never — reasons:
// - Promotes objects to higher generations unnecessarily (Gen 0 → Gen 1 → Gen 2)
// - Disrupts GC\'s self-tuning heuristics
// - Causes latency spikes (stop-the-world pause)
// - Rarely improves performance; often makes it worse

// ✅ Acceptable rare cases:
// 1. After a known large allocation is no longer needed
// 2. In unit tests verifying finalizer behaviour
// 3. Before performance-sensitive benchmarks (baseline memory)
// 4. Out-of-process tooling / diagnostics

void ProcessLargeBatch()
{
    LoadLargeDataSet();
    GC.Collect(2, GCCollectionMode.Aggressive, blocking: true, compacting: true); // rare justified case
}

void LoadLargeDataSet() { }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you detect memory leaks in .NET applications?

```cs
// Common causes of managed memory leaks:
// 1. Event handlers not unsubscribed (most common)
// 2. Static fields holding object references
// 3. Caches with no eviction policy
// 4. Closures capturing large objects
// 5. Long-lived collections growing unbounded

// 1. Event leak — subscriber held alive by publisher\'s event
public class Publisher
{
    public event EventHandler? Updated;
}

public class Subscriber
{
    public Subscriber(Publisher pub)
        => pub.Updated += OnUpdated; // pub holds reference to 'this'

    private void OnUpdated(object? sender, EventArgs e) { }

    // Fix: implement IDisposable and unsubscribe
}

// 2. Detect with GC.GetTotalMemory
long before = GC.GetTotalMemory(forceFullCollection: true);
var list = new List<byte[]>();
for (int i = 0; i < 100; i++) list.Add(new byte[1024 * 1024]); // 100 MB
long after = GC.GetTotalMemory(false);
Console.WriteLine($"Leaked: {(after - before) / 1024 / 1024} MB");
list.Clear();

// 3. WeakReference — holds reference without preventing GC
var weakRef = new WeakReference<byte[]>(new byte[1024]);
GC.Collect();
if (weakRef.TryGetTarget(out var target))
    Console.WriteLine("Still alive");
else
    Console.WriteLine("Collected");

// 4. Tools for detecting leaks:
// - dotnet-counters: dotnet counters monitor --process-id <pid>
// - dotnet-dump:     dotnet dump collect --process-id <pid>
// - Visual Studio Diagnostic Tools → Memory Usage → Snapshots
// - JetBrains dotMemory, Redgate ANTS, PerfView

// 5. MemoryDiagnoser in BenchmarkDotNet
// [MemoryDiagnoser]
// public class MyBenchmark { ... }

// 6. ObjectPooling to reduce pressure
var pool = System.Buffers.ArrayPool<byte>.Shared;
byte[] rented = pool.Rent(1024);
try { /* use buffer */ }
finally { pool.Return(rented); } // returned to pool — no GC pressure

byte[] target2 = [];
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a finalizer in C#? What is `GC.SuppressFinalize` and when should you use it?

```cs
// Finalizer — called by GC before reclaiming object memory
// Syntax: ~ClassName() { }
// - Runs on the dedicated finalizer thread
// - Non-deterministic timing
// - Do NOT call managed code that may have been collected
// - Only for unmanaged resource cleanup as a SAFETY NET

public class FileWrapper : IDisposable
{
    private IntPtr _fileHandle;
    private bool _disposed;

    public FileWrapper(string path)
        => _fileHandle = OpenFile(path); // OS handle

    // Finalizer — safety net if Dispose was not called
    ~FileWrapper()
    {
        Console.WriteLine("Finalizer: cleaning up (Dispose was not called!)");
        Dispose(disposing: false);
    }

    protected virtual void Dispose(bool disposing)
    {
        if (_disposed) return;
        if (disposing)
        {
            // safe to access managed objects here
        }
        CloseFile(_fileHandle); // unmanaged cleanup always
        _fileHandle = IntPtr.Zero;
        _disposed = true;
    }

    // GC.SuppressFinalize — tells GC: "finalizer not needed, skip finalization queue"
    public void Dispose()
    {
        Dispose(disposing: true);
        GC.SuppressFinalize(this); // ✅ skip finalizer — memory reclaimed in ONE cycle
    }

    private static IntPtr OpenFile(string path) => new(1);
    private static void CloseFile(IntPtr h) { }
}

// Always call Dispose with 'using'
using var fw = new FileWrapper("data.bin");
// Dispose called → GC.SuppressFinalize → no finalizer overhead

// GC.ReRegisterForFinalize — re-register for finalization (rare use: resurrection pattern)
public class ResurrectableResource : IDisposable
{
    ~ResurrectableResource() => Console.WriteLine("Finalized");

    public void Reset()
    {
        GC.ReRegisterForFinalize(this); // will finalize again when unreachable
    }

    public void Dispose()
    {
        GC.SuppressFinalize(this);
        Console.WriteLine("Disposed");
    }
}

// GC.KeepAlive — prevents GC from collecting object before a certain point
void UseHandle(IntPtr handle)
{
    var resource = new FileWrapper("file");
    _ = handle; // use handle
    GC.KeepAlive(resource); // ensure resource is NOT collected before this point
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is `GC.Collect` vs `GC.WaitForPendingFinalizers`?

```cs
// GC.Collect — triggers a garbage collection
GC.Collect();         // collect all generations (0, 1, 2)
GC.Collect(0);        // collect Gen 0 only
GC.Collect(2, GCCollectionMode.Forced, blocking: true, compacting: true);
// GCCollectionMode: Default, Forced, Optimized, Aggressive (.NET 6+)

// GC.WaitForPendingFinalizers — blocks until finalizer thread completes all queued finalizers
GC.WaitForPendingFinalizers();

// Why use both together?
// After Collect() — unreachable finalizable objects are queued for finalization
// WaitForPendingFinalizers() — waits for finalizer thread to process that queue
// Second Collect() — reclaims the now-finalized objects

// Standard pattern when you MUST force GC (tests, benchmarks):
GC.Collect();
GC.WaitForPendingFinalizers();
GC.Collect(); // reclaim objects that were waiting for finalization

// Example — verifying finalizer runs in tests
bool finalized = false;

void CreateObject()
{
    var obj = new FinalizableObj(() => finalized = true);
}

CreateObject();           // obj goes out of scope
GC.Collect();
GC.WaitForPendingFinalizers();
Console.WriteLine($"Finalized: {finalized}"); // True

// GC.GetTotalMemory(forceFullCollection: true) — combines Collect + WaitForPendingFinalizers
long memory = GC.GetTotalMemory(forceFullCollection: true);
Console.WriteLine($"Memory after full GC: {memory:N0} bytes");

class FinalizableObj(Action onFinalize)
{
    ~FinalizableObj() => onFinalize();
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a `WeakReference` in C#?

A **`WeakReference<T>`** holds a reference to an object without preventing it from being garbage collected. Useful for caches and observer patterns where you don\'t want to force objects to stay alive.

```cs
// WeakReference<T> — does NOT prevent GC collection
var data = new byte[1024 * 1024]; // 1 MB
var weak = new WeakReference<byte[]>(data);

data = null!; // remove strong reference
GC.Collect();

if (weak.TryGetTarget(out byte[]? target))
    Console.WriteLine($"Still alive: {target.Length} bytes");
else
    Console.WriteLine("Collected by GC");

// WeakReference cache pattern — auto-evicts entries under memory pressure
public class WeakCache<TKey, TValue> where TKey : notnull where TValue : class
{
    private readonly Dictionary<TKey, WeakReference<TValue>> _cache = new();

    public void Set(TKey key, TValue value)
        => _cache[key] = new WeakReference<TValue>(value);

    public TValue? Get(TKey key)
    {
        if (_cache.TryGetValue(key, out var wr) && wr.TryGetTarget(out var val))
            return val;
        _cache.Remove(key); // clean up dead entry
        return null;
    }
}

var cache = new WeakCache<int, string>();
cache.Set(1, "hello");

string? val = cache.Get(1);
Console.WriteLine(val ?? "not found"); // hello

// WeakReference (non-generic, legacy) — avoid, use WeakReference<T> instead
object obj = new { Name = "test" };
var legacyWeak = new WeakReference(obj);
Console.WriteLine(legacyWeak.IsAlive); // True
obj = null!;
GC.Collect();
Console.WriteLine(legacyWeak.IsAlive); // False
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `Lazy<T>` class? What is the difference between `Lazy` and `Lazy<T>`?

`Lazy<T>` defers creation of an expensive object until it is first accessed. It is thread-safe by default.

```cs
// Lazy<T> — deferred, thread-safe initialization
var lazyConfig = new Lazy<AppConfig>(() =>
{
    Console.WriteLine("Loading config..."); // only runs on first access
    return new AppConfig { Timeout = 30 };
});

// Value not yet created
Console.WriteLine(lazyConfig.IsValueCreated); // False

// First access — triggers initialization
AppConfig config = lazyConfig.Value;           // "Loading config..."
Console.WriteLine(lazyConfig.IsValueCreated); // True
Console.WriteLine(lazyConfig.Value.Timeout);  // 30 — second access, no re-init

// Thread safety modes
var lazy1 = new Lazy<ExpensiveObject>(LazyThreadSafetyMode.ExecutionAndPublication); // default — lock on init
var lazy2 = new Lazy<ExpensiveObject>(LazyThreadSafetyMode.PublicationOnly);         // race: one winner
var lazy3 = new Lazy<ExpensiveObject>(LazyThreadSafetyMode.None);                    // no thread safety

// Common pattern: lazy singleton in a class
public class DataService
{
    private static readonly Lazy<DataService> _instance
        = new(() => new DataService());

    public static DataService Instance => _instance.Value;

    private DataService() { }
    public void Query() => Console.WriteLine("Querying data");
}

DataService.Instance.Query();

// Lazy (non-generic) — does NOT exist as a public API
// 'Lazy' by itself is not a type — always use Lazy<T>
// The question likely refers to:
// • Lazy<T>  — built-in BCL class
// • Custom lazy patterns (lazy fields, lazy properties)

// Lazy property pattern (no Lazy<T> class)
private ExpensiveObject? _resource;
ExpensiveObject Resource => _resource ??= new ExpensiveObject();

record AppConfig { public int Timeout { get; init; } }
class ExpensiveObject { }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a `MemoryCache` in C#?

`MemoryCache` is an in-process, thread-safe cache provided by `Microsoft.Extensions.Caching.Memory`. It stores key-value pairs in memory with optional expiration, size limits, and eviction callbacks.

```cs
using Microsoft.Extensions.Caching.Memory;

// Create cache
var cache = new MemoryCache(new MemoryCacheOptions
{
    SizeLimit = 1024 // max entries (in size units you define)
});

// Set with absolute expiration
cache.Set("user:1", new User(1, "Alice"), new MemoryCacheEntryOptions
{
    AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(5),
    Size = 1 // count this entry as 1 unit toward SizeLimit
});

// Set with sliding expiration (refreshed on each access)
cache.Set("session:abc", new SessionData(), new MemoryCacheEntryOptions
{
    SlidingExpiration = TimeSpan.FromMinutes(20),
    Size = 1
});

// Get
if (cache.TryGetValue("user:1", out User? user))
    Console.WriteLine($"From cache: {user?.Name}");

// GetOrCreate — atomic check-and-create
User cachedUser = cache.GetOrCreate("user:2", entry =>
{
    entry.AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(10);
    entry.Size = 1;
    return new User(2, "Bob"); // factory — called only on cache miss
})!;

// Async GetOrCreateAsync
User cachedUser2 = await cache.GetOrCreateAsync("user:3", async entry =>
{
    entry.AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(10);
    entry.Size = 1;
    return await LoadUserFromDbAsync(3);
}) ?? throw new Exception("Not found");

// Eviction callback
cache.Set("temp:key", "value", new MemoryCacheEntryOptions
{
    AbsoluteExpirationRelativeToNow = TimeSpan.FromSeconds(30),
    Size = 1
}.RegisterPostEvictionCallback((key, value, reason, state) =>
    Console.WriteLine($"Evicted '{key}': {reason}")));

// Remove manually
cache.Remove("user:1");

// In ASP.NET Core — register via DI
// services.AddMemoryCache();
// Then inject IMemoryCache into your service

static Task<User> LoadUserFromDbAsync(int id) => Task.FromResult(new User(id, $"User{id}"));
record User(int Id, string Name);
record SessionData;
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a `Mutex` in C#?

A `Mutex` (mutual exclusion) is a synchronization primitive that restricts access to a resource to **one thread at a time**, and uniquely supports **cross-process** synchronization via a named mutex.

```cs
// 1. Local mutex — single-process synchronization
using var mutex = new Mutex();

Thread t1 = new(() =>
{
    mutex.WaitOne(); // acquire
    try   { Console.WriteLine("T1 in critical section"); Thread.Sleep(100); }
    finally { mutex.ReleaseMutex(); }
});

Thread t2 = new(() =>
{
    mutex.WaitOne();
    try   { Console.WriteLine("T2 in critical section"); }
    finally { mutex.ReleaseMutex(); }
});

t1.Start(); t2.Start(); t1.Join(); t2.Join();

// 2. Named mutex — cross-process (e.g., single-instance application)
const string MutexName = "Global\\MyApp_SingleInstance";

bool createdNew;
using var globalMutex = new Mutex(initiallyOwned: true, MutexName, out createdNew);

if (!createdNew)
{
    Console.WriteLine("Another instance is already running.");
    return;
}

try
{
    Console.WriteLine("Application running...");
    Thread.Sleep(5000); // simulate work
}
finally
{
    globalMutex.ReleaseMutex();
}

// 3. Mutex with timeout
using var timedMutex = new Mutex();
bool acquired = timedMutex.WaitOne(TimeSpan.FromSeconds(5));
if (acquired)
{
    try { Console.WriteLine("Acquired with timeout"); }
    finally { timedMutex.ReleaseMutex(); }
}
else
{
    Console.WriteLine("Timed out waiting for mutex");
}

// Note: for single-process scenarios prefer lock/Monitor or SemaphoreSlim
// Mutex is heavier — use only when cross-process sync is needed
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is `Semaphore` vs `SemaphoreSlim` in C#?

| | `Semaphore` | `SemaphoreSlim` |
|-|------------|----------------|
| **Cross-process** | ✅ Named semaphores | ❌ In-process only |
| **Async support** | ❌ | ✅ `WaitAsync()` |
| **Performance** | Heavier (OS kernel) | Lighter (user-mode) |
| **Use when** | Cross-process throttling | In-process async throttling |

```cs
// SemaphoreSlim — preferred for async in-process scenarios
var semaphore = new SemaphoreSlim(initialCount: 3, maxCount: 3); // allow 3 concurrent

var tasks = Enumerable.Range(1, 10).Select(async i =>
{
    await semaphore.WaitAsync(); // acquire slot (async — no thread blocking)
    try
    {
        Console.WriteLine($"Task {i} running (slots left: {semaphore.CurrentCount})");
        await Task.Delay(500); // simulate work
    }
    finally
    {
        semaphore.Release(); // release slot
        Console.WriteLine($"Task {i} done");
    }
});

await Task.WhenAll(tasks); // max 3 tasks run concurrently

// Named Semaphore — cross-process throttling
using var namedSemaphore = new Semaphore(initialCount: 2, maximumCount: 2, name: "Global\\MySemaphore");
bool entered = namedSemaphore.WaitOne(TimeSpan.FromSeconds(5));
if (entered)
{
    try { Console.WriteLine("Entered semaphore"); }
    finally { namedSemaphore.Release(); }
}

// Rate-limiting with SemaphoreSlim (throttle API calls)
var throttle = new SemaphoreSlim(5); // max 5 concurrent HTTP calls
async Task<string> FetchAsync(HttpClient client, string url)
{
    await throttle.WaitAsync();
    try   { return await client.GetStringAsync(url); }
    finally { throttle.Release(); }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a deadlock in C#?

A **deadlock** occurs when two or more threads each hold a resource that the other needs, causing all threads to wait indefinitely.

```cs
// Classic deadlock — two threads, two locks acquired in opposite order
object lock1 = new();
object lock2 = new();

Thread t1 = new(() =>
{
    lock (lock1)
    {
        Thread.Sleep(50); // give t2 time to acquire lock2
        lock (lock2) { Console.WriteLine("T1: acquired both locks"); }
    }
});

Thread t2 = new(() =>
{
    lock (lock2)
    {
        Thread.Sleep(50);
        lock (lock1) { Console.WriteLine("T2: acquired both locks"); }
    }
});

// t1.Start(); t2.Start(); ← DEADLOCK! Both threads wait forever

// Prevention 1: consistent lock ordering
Thread safe1 = new(() => { lock (lock1) { lock (lock2) { Console.WriteLine("safe1"); } } });
Thread safe2 = new(() => { lock (lock1) { lock (lock2) { Console.WriteLine("safe2"); } } });
safe1.Start(); safe2.Start(); safe1.Join(); safe2.Join();

// Prevention 2: use Monitor.TryEnter with timeout
Thread tryLock = new(() =>
{
    if (Monitor.TryEnter(lock1, TimeSpan.FromSeconds(1)))
    {
        try
        {
            if (Monitor.TryEnter(lock2, TimeSpan.FromSeconds(1)))
            {
                try { Console.WriteLine("Acquired both"); }
                finally { Monitor.Exit(lock2); }
            }
            else { Console.WriteLine("Could not acquire lock2 — backoff"); }
        }
        finally { Monitor.Exit(lock1); }
    }
});
tryLock.Start(); tryLock.Join();

// Prevention 3: prefer async/await + SemaphoreSlim over blocking locks
// Prevention 4: CancellationToken in async operations prevents indefinite waits
// Prevention 5: use higher-level concurrency primitives (Channel<T>, Dataflow)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `Interlocked` class in C#?

`Interlocked` provides **atomic** operations on shared variables — thread-safe without locks, using CPU atomic instructions.

```cs
// Interlocked.Increment / Decrement — atomic ++ and --
int counter = 0;
var threads = Enumerable.Range(0, 10).Select(_ => new Thread(() =>
{
    for (int i = 0; i < 1000; i++)
        Interlocked.Increment(ref counter); // atomic — no race condition
})).ToList();

threads.ForEach(t => t.Start());
threads.ForEach(t => t.Join());
Console.WriteLine(counter); // always 10,000

// Without Interlocked: counter++ is NOT atomic (read-modify-write race)
// counter++ → IL: ldloc, ldc.i4.1, add, stloc — three non-atomic operations

// Interlocked.Add — atomic addition
long total = 0;
Interlocked.Add(ref total, 100);
Console.WriteLine(total); // 100

// Interlocked.Exchange — atomically set and return old value
int state = 0;
int oldState = Interlocked.Exchange(ref state, 1);
Console.WriteLine($"Old: {oldState}, New: {state}"); // Old: 0, New: 1

// Interlocked.CompareExchange — set if current value equals expected (CAS)
int value = 5;
int original = Interlocked.CompareExchange(ref value, newValue: 10, comparand: 5);
Console.WriteLine($"Original: {original}, Value: {value}"); // Original: 5, Value: 10

// CAS loop — lock-free update pattern
long sharedLong = 0;
void AddLockFree(long amount)
{
    long current, updated;
    do
    {
        current = Interlocked.Read(ref sharedLong);
        updated = current + amount;
    } while (Interlocked.CompareExchange(ref sharedLong, updated, current) != current);
}

// Interlocked.Read — atomic 64-bit read on 32-bit systems
long safeRead = Interlocked.Read(ref sharedLong);

// Interlocked.MemoryBarrier / MemoryBarrierProcessWide — memory fences
Interlocked.MemoryBarrier(); // full fence — prevents CPU/compiler reordering
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `Task` and `ValueTask`?

| | `Task` / `Task<T>` | `ValueTask` / `ValueTask<T>` |
|-|-------------------|---------------------------|
| **Allocation** | Always heap allocated | No allocation if synchronous |
| **Caching** | Can be cached/reused | Single await only |
| **Overhead** | Higher for hot paths | Lower for sync-fast paths |
| **Use when** | General async work | High-throughput, often-sync methods |

```cs
// Task — standard async, always allocates
async Task<int> GetCountAsync()
{
    await Task.Delay(100); // genuinely async
    return 42;
}

// ValueTask — avoids allocation when result is immediately available
async ValueTask<int> GetCachedCountAsync()
{
    if (_cache.TryGetValue("count", out int cached))
        return cached; // synchronous fast path — NO Task allocation

    int value = await LoadFromDbAsync(); // async slow path
    _cache["count"] = value;
    return value;
}

// Using ValueTask
int count = await GetCachedCountAsync();

// Rules for ValueTask:
// ✅ Await it exactly once
// ✅ Don\'t store and await later (use AsTask() first)
// ✅ Don\'t await from multiple consumers
// ✅ Use when method frequently returns synchronously

// Converting ValueTask to Task when you need to share/store
ValueTask<int> vt = GetCachedCountAsync();
Task<int> task = vt.AsTask(); // convert — now safely multi-awaitable
int r1 = await task;
int r2 = await task; // ✅ safe after AsTask()

// IValueTaskSource — advanced: reuse ValueTask with pool (avoid this unless profiling shows need)

Dictionary<string, int> _cache = new();
Task<int> LoadFromDbAsync() => Task.FromResult(100);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is `CancellationToken` and `CancellationTokenSource` in C#?

```cs
// CancellationTokenSource — creates and controls cancellation
using var cts = new CancellationTokenSource();
CancellationToken token = cts.Token;

// Cancel after timeout
using var timeoutCts = new CancellationTokenSource(TimeSpan.FromSeconds(10));

// Cancel after delay
cts.CancelAfter(TimeSpan.FromSeconds(5));

// Manual cancel
// cts.Cancel(); // triggers cancellation

// CancellationToken — passed to async methods to observe cancellation
async Task<string> FetchDataAsync(string url, CancellationToken ct = default)
{
    using var client = new HttpClient();
    // Pass token to async I/O — cancels automatically
    return await client.GetStringAsync(url, ct);
}

// Full usage example
using var source = new CancellationTokenSource();
CancellationToken ct = source.Token;

// Register a callback on cancellation
ct.Register(() => Console.WriteLine("Operation was cancelled"));

Task workTask = Task.Run(async () =>
{
    for (int i = 0; i < 100; i++)
    {
        ct.ThrowIfCancellationRequested(); // poll and throw OperationCanceledException
        await Task.Delay(100, ct);         // also respects cancellation
        Console.WriteLine($"Step {i}");
    }
}, ct);

await Task.Delay(350);
source.Cancel(); // cancel after ~350ms

try
{
    await workTask;
}
catch (OperationCanceledException)
{
    Console.WriteLine("Task was cancelled gracefully");
}

// Linked tokens — cancel when ANY source fires
using var userCts    = new CancellationTokenSource();
using var timeoutCts2 = new CancellationTokenSource(TimeSpan.FromSeconds(30));
using var linked     = CancellationTokenSource.CreateLinkedTokenSource(
    userCts.Token, timeoutCts2.Token);

await FetchDataAsync("https://example.com", linked.Token);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `Task.WhenAll` and `Task.WhenAny`?

| | `Task.WhenAll` | `Task.WhenAny` |
|-|---------------|---------------|
| **Completes when** | ALL tasks complete | FIRST task completes |
| **Exception** | Waits for all; aggregates | Returns immediately; others keep running |
| **Use for** | Fan-out parallel work | Timeout, race, first-success patterns |

```cs
// Task.WhenAll — wait for all, collect all results
async Task WhenAllExample()
{
    Task<string> task1 = FetchAsync("https://api.example.com/users");
    Task<string> task2 = FetchAsync("https://api.example.com/orders");
    Task<string> task3 = FetchAsync("https://api.example.com/products");

    string[] results = await Task.WhenAll(task1, task2, task3); // parallel fetch
    Console.WriteLine($"Users: {results[0].Length} chars");
    Console.WriteLine($"Orders: {results[1].Length} chars");
}

// Task.WhenAny — first to complete wins
async Task WhenAnyExample()
{
    // Pattern 1: timeout
    using var cts = new CancellationTokenSource();
    Task<string> fetch   = FetchAsync("https://slow-api.example.com");
    Task<string> timeout = Task.Delay(TimeSpan.FromSeconds(5)).ContinueWith(_ => "timeout");

    Task<string> first = await Task.WhenAny(fetch, timeout);
    string result = await first;
    Console.WriteLine(result == "timeout" ? "Request timed out" : $"Got: {result.Length} chars");

    // Pattern 2: first successful result from multiple endpoints
    var endpoints = new[]
    {
        FetchAsync("https://api1.example.com/data"),
        FetchAsync("https://api2.example.com/data"),
        FetchAsync("https://api3.example.com/data"),
    };
    Task<string> winner = await Task.WhenAny(endpoints);
    Console.WriteLine($"Fastest result: {(await winner).Length} chars");
}

// WhenAll exception handling — see all failures
Task[] failingTasks =
[
    Task.Run(() => throw new Exception("Task 1")),
    Task.Run(() => throw new Exception("Task 2")),
];

try { await Task.WhenAll(failingTasks); }
catch
{
    foreach (var t in failingTasks.Where(t => t.IsFaulted))
        Console.WriteLine(t.Exception!.InnerException!.Message);
}

static Task<string> FetchAsync(string url) =>
    Task.FromResult($"data-from-{url}");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is `ConcurrentDictionary` in C#?

`ConcurrentDictionary<TKey, TValue>` is a thread-safe dictionary in `System.Collections.Concurrent` that allows multiple threads to read and write concurrently without external locking.

```cs
using System.Collections.Concurrent;

var dict = new ConcurrentDictionary<string, int>(StringComparer.OrdinalIgnoreCase);

// Thread-safe add or update
dict["count"] = 0;

// TryAdd — adds only if key doesn\'t exist
bool added = dict.TryAdd("item1", 10);

// AddOrUpdate — atomic add-or-update
dict.AddOrUpdate(
    key: "count",
    addValue: 1,
    updateValueFactory: (_, current) => current + 1);

// GetOrAdd — atomic get-or-create
int value = dict.GetOrAdd("hits", key =>
{
    Console.WriteLine($"Creating default for {key}");
    return 0;
});

// GetOrAdd with factory object (avoid closure allocation)
int value2 = dict.GetOrAdd("hits", static (key, seed) => seed, addValueFactoryArgument: 42);

// Parallel increment
var counter = new ConcurrentDictionary<string, long>();
var tasks = Enumerable.Range(0, 100).Select(_ => Task.Run(() =>
{
    for (int i = 0; i < 1000; i++)
        counter.AddOrUpdate("total", 1L, (_, v) => v + 1L);
}));
await Task.WhenAll(tasks);
Console.WriteLine(counter["total"]); // always 100,000

// TryGetValue / TryRemove / TryUpdate
if (dict.TryGetValue("count", out int count))
    Console.WriteLine($"count = {count}");

dict.TryRemove("item1", out _);

// Snapshot iteration (safe but may not be perfectly consistent)
foreach (var (key, val) in dict)
    Console.WriteLine($"{key} = {val}");

// Keys / Values — snapshot copies
ICollection<string> keys = dict.Keys;
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is `BlockingCollection` in C#? What is the difference between `ConcurrentBag` and `ConcurrentQueue`?

**`BlockingCollection<T>`** provides bounded, blocking producer-consumer patterns. It wraps any `IProducerConsumerCollection<T>` (defaults to `ConcurrentQueue<T>`).

| | `ConcurrentQueue<T>` | `ConcurrentBag<T>` |
|-|---------------------|-------------------|
| **Order** | FIFO | Unordered |
| **Best for** | Producer-consumer pipelines | Work-stealing (same thread adds+removes) |
| **Thread affinity** | None | Optimized for thread-local access |

```cs
// BlockingCollection — bounded producer-consumer queue
var collection = new BlockingCollection<int>(boundedCapacity: 100);

// Producer — blocks when collection is full
Task producer = Task.Run(() =>
{
    for (int i = 0; i < 20; i++)
    {
        collection.Add(i);                 // blocks if at capacity
        Console.WriteLine($"Produced: {i}");
    }
    collection.CompleteAdding();           // signal no more items
});

// Consumer — blocks when collection is empty
Task consumer = Task.Run(() =>
{
    foreach (int item in collection.GetConsumingEnumerable()) // blocks until item or completed
        Console.WriteLine($"Consumed: {item}");
});

await Task.WhenAll(producer, consumer);

// ConcurrentQueue — FIFO, producer-consumer pipeline
var queue = new ConcurrentQueue<string>();
queue.Enqueue("first");
queue.Enqueue("second");

if (queue.TryDequeue(out string? item)) Console.WriteLine(item); // first
if (queue.TryPeek(out string? next))    Console.WriteLine(next); // second

// ConcurrentBag — unordered, thread-local optimization
var bag = new ConcurrentBag<int>();
Parallel.For(0, 10, i => bag.Add(i)); // each thread adds to local bag

while (bag.TryTake(out int bagItem))
    Console.Write($"{bagItem} "); // unordered output
Console.WriteLine();

// ConcurrentStack — LIFO
var stack = new ConcurrentStack<int>();
stack.Push(1); stack.Push(2); stack.Push(3);
if (stack.TryPop(out int top)) Console.WriteLine(top); // 3
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between server GC and workstation GC? How do you configure GC with `GCSettings`?

| | **Workstation GC** | **Server GC** |
|-|------------------|--------------|
| **Default for** | Desktop, single-process apps | ASP.NET Core, server workloads |
| **Threads** | 1 GC thread | 1 GC thread per CPU core |
| **Heap** | 1 heap | 1 heap per CPU core |
| **Throughput** | Lower | Higher |
| **Latency** | Lower pauses | Higher pauses (more work per GC) |
| **Memory** | Lower | Higher |

```cs
using System.Runtime;

// Check current mode
Console.WriteLine($"Server GC: {GCSettings.IsServerGC}");
Console.WriteLine($"Latency mode: {GCSettings.LatencyMode}");

// GCLatencyMode — balance throughput vs pause time
// Configure for interactive/low-latency scenario
GCSettings.LatencyMode = GCLatencyMode.SustainedLowLatency;
// Minimizes Gen 2 collections — good for UI, real-time

// Briefly suppress GC during critical section
GC.TryStartNoGCRegion(1024 * 1024 * 10); // request 10 MB no-GC region
try
{
    // Critical path — GC will not run here if memory is available
    PerformLatencySensitiveWork();
}
finally
{
    GC.EndNoGCRegion();
    GCSettings.LatencyMode = GCLatencyMode.Interactive; // restore
}

// Configure in runtimeconfig.json (preferred over code):
// {
//   "runtimeOptions": {
//     "configProperties": {
//       "System.GC.Server": true,
//       "System.GC.Concurrent": true,
//       "System.GC.HeapHardLimit": 1073741824
//     }
//   }
// }

// Or in .csproj:
// <ServerGarbageCollection>true</ServerGarbageCollection>
// <GarbageCollectionAdaptationMode>0</GarbageCollectionAdaptationMode>

// Optimize GC in .NET:
// 1. Reduce allocations — use stackalloc, Span<T>, ArrayPool<T>
// 2. Avoid boxing — use generics instead of object
// 3. Dispose IDisposable objects promptly (using statement)
// 4. Use object pooling for large, frequently-allocated objects
// 5. Prefer value types (struct) for small, short-lived data
// 6. Avoid large object heap (LOH) fragmentation — pool large arrays

void PerformLatencySensitiveWork() { }

```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 12. LAMBDA EXPRESSIONS

<br/>

## Q. What is a lambda expression in C# and why is it used? How do you declare one?

A **lambda expression** is an anonymous inline function defined with the `=>` (goes-to) operator. It creates a concise way to write delegate implementations, LINQ queries, callbacks, and event handlers without declaring a separate named method.

**Syntax forms:**

```cs
// Expression lambda — single expression, implicit return
(parameters) => expression

// Statement lambda — block body with explicit return
(parameters) => { statements; return value; }

// Zero parameters
() => Console.WriteLine("Hello")

// One parameter — parentheses optional
x => x * x

// Multiple parameters
(x, y) => x + y

// Explicitly typed parameters
(int x, string y) => $"{y} = {x}"
```

**Why use lambdas:**
- No need to declare a separate method for one-off logic
- Enables LINQ query expressions
- Captures local variables (closures)
- Replaces verbose anonymous methods (C# 2.0 syntax)

```cs
// Without lambda — verbose
static bool IsEvenMethod(int n) => n % 2 == 0;
var evens1 = new List<int> { 1, 2, 3, 4, 5 }.FindAll(IsEvenMethod);

// With lambda — concise, inline
var evens2 = new List<int> { 1, 2, 3, 4, 5 }.FindAll(n => n % 2 == 0);

// Delegate types satisfied by lambdas
Func<int, int>      square  = x => x * x;
Action<string>      print   = msg => Console.WriteLine(msg);
Predicate<string>   isShort = s => s.Length < 5;
Comparison<int>     desc    = (a, b) => b.CompareTo(a);

Console.WriteLine(square(6));    // 36
print("Lambda!");
Console.WriteLine(isShort("Hi")); // True

// C# 14: natural type for lambdas (compiler infers delegate type)
var add = (int a, int b) => a + b; // inferred as Func<int,int,int>
Console.WriteLine(add(3, 4));  // 7

// Static lambda (.NET 5+) — prevents accidental capture
var multiplier = 10;
Func<int, int> staticLambda = static x => x * 2; // ✅ cannot capture 'multiplier'
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between expression lambdas, statement lambdas, and anonymous methods?

```cs
// 1. Expression lambda — single expression, implicit return
Func<int, int> square = x => x * x;
Func<int, int, int> sum = (a, b) => a + b;
Console.WriteLine(square(5));  // 25
Console.WriteLine(sum(3, 4));  // 7

// 2. Statement lambda — block body, explicit return
Func<int, string> classify = n =>
{
    if (n < 0) return "negative";
    if (n == 0) return "zero";
    return "positive";
};
Console.WriteLine(classify(-5)); // negative

// 3. Anonymous method (C# 2.0 — pre-lambda) — verbose, limited
Func<int, int> squareAnon = delegate(int x) { return x * x; };
Console.WriteLine(squareAnon(5)); // 25

// Key differences:
// Expression lambda  — concise, can be used as Expression<Func<...>> (EF Core, etc.)
// Statement lambda   — more complex logic, cannot be Expression<T>
// Anonymous method   — legacy, no expression tree support, no implicit typing

// Expression tree — only expression lambdas work
using System.Linq.Expressions;
Expression<Func<int, bool>> expr  = x => x > 5;   // ✅ expression lambda
// Expression<Func<int, bool>> fail = x => { return x > 5; }; // ❌ compile error (statement)

Console.WriteLine(expr.Compile()(10)); // True — compiled and invoked
Console.WriteLine(expr);              // x => (x > 5) — inspectable as data

// IQueryable uses expression trees to build SQL
// var users = dbContext.Users.Where(u => u.Age > 18); // EF translates to SQL WHERE
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use lambda expressions with LINQ in C#?

```cs
int[] numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Where — filter
var evens = numbers.Where(n => n % 2 == 0);
// [2, 4, 6, 8, 10]

// Select — transform (map)
var squares = numbers.Select(n => n * n);
// [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

// Where + Select chained
var largeSquares = numbers.Where(n => n > 5).Select(n => n * n);
// [36, 49, 64, 81, 100]

// OrderBy / OrderByDescending
var sorted = numbers.OrderByDescending(n => n);
// [10, 9, 8, ...]

// GroupBy
var words = new[] { "apple", "ant", "banana", "avocado", "blueberry" };
var byLetter = words.GroupBy(w => w[0]);
foreach (var g in byLetter)
    Console.WriteLine($"{g.Key}: {string.Join(", ", g)}");
// a: apple, ant, avocado
// b: banana, blueberry

// Aggregate (fold/reduce)
int product = numbers.Aggregate(1, (acc, n) => acc * n);
Console.WriteLine(product); // 3628800 (10!)

// SelectMany — flatten nested sequences
string[] sentences = ["hello world", "foo bar baz"];
var allWords = sentences.SelectMany(s => s.Split(' '));
// [hello, world, foo, bar, baz]

// Complex object example
var orders = new[]
{
    new { Id = 1, Customer = "Alice", Amount = 250m, Year = 2024 },
    new { Id = 2, Customer = "Bob",   Amount = 100m, Year = 2024 },
    new { Id = 3, Customer = "Alice", Amount = 400m, Year = 2025 },
};

var report = orders
    .Where(o => o.Amount > 150)
    .GroupBy(o => o.Customer)
    .Select(g => new { Customer = g.Key, Total = g.Sum(o => o.Amount), Count = g.Count() })
    .OrderByDescending(r => r.Total);

foreach (var r in report)
    Console.WriteLine($"{r.Customer}: {r.Total:C} ({r.Count} orders)");
// Alice: $650.00 (2 orders)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the advantages of using lambda expressions in C#?

| Advantage | Description |
|-----------|-------------|
| **Concise** | Eliminates boilerplate method declarations |
| **Inline** | Logic defined at the usage site — easier to read |
| **Closure** | Captures surrounding variables naturally |
| **LINQ** | Enables readable query composition |
| **Expression trees** | Lambdas can be inspected/translated (EF Core → SQL) |
| **First-class** | Passed as arguments, stored in variables, returned from methods |
| **Composable** | Build pipelines with Func<T> chains |

```cs
// Composable pipeline
Func<int, int> doubleIt    = x => x * 2;
Func<int, int> addTen      = x => x + 10;
Func<int, bool> isPositive = x => x > 0;

Func<int, int> pipeline = x => addTen(doubleIt(x));
Console.WriteLine(pipeline(5)); // (5*2)+10 = 20

// Higher-order functions
static Func<int, int> MultiplyBy(int factor) => x => x * factor;
var triple = MultiplyBy(3);
var quadruple = MultiplyBy(4);
Console.WriteLine(triple(7));    // 21
Console.WriteLine(quadruple(7)); // 28

// Storing and reusing
var validators = new List<Func<string, bool>>
{
    s => s.Length >= 8,
    s => s.Any(char.IsUpper),
    s => s.Any(char.IsDigit),
};

string password = "MyPass1!";
bool valid = validators.All(v => v(password));
Console.WriteLine($"Password valid: {valid}"); // True

// Lazy evaluation — lambda delays execution
Func<string> expensive = () =>
{
    Thread.Sleep(1000); // only runs when called
    return "computed";
};

// Called only when needed
if (ShouldCompute())
    Console.WriteLine(expensive());

static bool ShouldCompute() => true;
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you capture variables in a lambda expression (closures) in C#?

Lambdas **capture variables by reference**, not by value. The lambda and outer code share the **same variable**.

```cs
// Basic closure — captures outer variable
int multiplier = 3;
Func<int, int> multiply = x => x * multiplier;
Console.WriteLine(multiply(5)); // 15

multiplier = 10; // change outer variable
Console.WriteLine(multiply(5)); // 50 — lambda sees updated value!

// ⚠️ Classic closure trap in loops
var funcs = new List<Func<int>>();
for (int i = 0; i < 5; i++)
    funcs.Add(() => i); // captures the VARIABLE i, not its current value

funcs.ForEach(f => Console.Write(f() + " ")); // 5 5 5 5 5 — all see final i

// ✅ Fix: capture a copy with a local variable
funcs.Clear();
for (int i = 0; i < 5; i++)
{
    int copy = i;
    funcs.Add(() => copy); // captures 'copy' — each iteration\'s own variable
}
funcs.ForEach(f => Console.Write(f() + " ")); // 0 1 2 3 4 — ✅ correct

// ✅ C# foreach — loop variable is NOT shared (safe by design)
int[] items = [10, 20, 30];
var itemFuncs = items.Select(item => (Func<int>)(() => item)).ToList();
itemFuncs.ForEach(f => Console.Write(f() + " ")); // 10 20 30 ✅

// Closure lifetime — captured variable stays alive as long as lambda exists
Func<int> counter = MakeCounter();
Console.WriteLine(counter()); // 1
Console.WriteLine(counter()); // 2
Console.WriteLine(counter()); // 3

Func<int> MakeCounter()
{
    int count = 0;               // 'count' lives as long as the lambda
    return () => ++count;        // lambda captures 'count'
}

// Memory implication — large objects captured in lambdas stay alive
// Store lambdas minimally; don\'t capture large datasets unnecessarily
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use lambda expressions with delegates? What are the limitations of lambda expressions?

```cs
// Lambda with built-in delegates
Func<string, int>   parse   = int.Parse;             // method group
Func<int, int, int> add     = (a, b) => a + b;
Action<string>      log     = Console.WriteLine;
Predicate<int>      isEven  = n => n % 2 == 0;
Comparison<string>  byLen   = (a, b) => a.Length.CompareTo(b.Length);

// Lambda with custom delegate
delegate bool NumberTest(int n, int threshold);
NumberTest greaterThan = (n, t) => n > t;
Console.WriteLine(greaterThan(10, 5)); // True

// Lambda as method argument
int[] nums = [3, 1, 4, 1, 5, 9, 2, 6];
Array.Sort(nums, (a, b) => b - a); // sort descending
Console.WriteLine(string.Join(", ", nums)); // 9, 6, 5, 4, 3, 2, 1, 1

// -------- LIMITATIONS --------

// 1. Cannot use ref/out/in parameters in expression lambdas (statement lambdas only)
Func<int, int> refLambda = (int x) =>       // ✅
{
    // ref int y = ref x; // ❌ lambdas cannot yield ref returns via Func<>
    return x + 1;
};

// 2. Cannot use 'yield return' inside lambdas
// Func<IEnumerable<int>> gen = () => { yield return 1; }; // ❌ compile error

// 3. Cannot use 'goto', 'break', 'continue' to jump outside the lambda
// 4. Cannot use unsafe code (pointers) inside lambdas

// 5. Statement lambdas cannot be expression trees
using System.Linq.Expressions;
// Expression<Func<int,int>> e = x => { return x; }; // ❌ compile error
Expression<Func<int, int>> e = x => x; // ✅ expression only

// 6. Debugging is harder — stack traces show generated names like <MethodName>b__0_0

// 7. Performance — each lambda declaration creates a delegate allocation
//    Use 'static' lambda or cache delegate to avoid repeated allocations
static Func<int, int>? _cached;
_cached ??= static x => x * 2; // allocated once

// 8. Cannot be used as default argument values
// void Method(Func<int, int> f = x => x) { } // ❌ (but null default is fine)
// void Method(Func<int, int>? f = null) { }  // ✅
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle exceptions in lambda expressions? Can you use `async`/`await` with lambdas? How do you use lambdas in event handling?

```cs
// Exception handling inside lambda — use try/catch within body
Func<int, int, int> safeDivide = (a, b) =>
{
    try { return a / b; }
    catch (DivideByZeroException) { return 0; }
};
Console.WriteLine(safeDivide(10, 0)); // 0

// Exception in lambda passed to LINQ — propagates to calling code
int[] numbers = [1, 2, 0, 4];
try
{
    var results = numbers.Select(n => 100 / n).ToList(); // throws at n=0
}
catch (DivideByZeroException ex)
{
    Console.WriteLine($"Lambda threw: {ex.Message}");
}

// ---- ASYNC LAMBDA ----
// async lambda with Action<>-like void
Func<Task> asyncAction = async () =>
{
    await Task.Delay(100);
    Console.WriteLine("Async lambda done");
};
await asyncAction();

// async lambda returning Task<T>
Func<string, Task<int>> asyncFunc = async url =>
{
    using var client = new HttpClient();
    string data = await client.GetStringAsync(url);
    return data.Length;
};
int length = await asyncFunc("https://example.com");

// ⚠️ Avoid async void lambda (no way to await/observe exceptions)
// Action asyncVoid = async () => { await Task.Delay(100); }; // ❌ fire-and-forget, exception lost
// Instead use Func<Task>:
Func<Task> safeAsyncAction = async () => { await Task.Delay(100); }; // ✅

// ---- EVENT HANDLING ----
var button = new Button();

// Lambda event handler — concise
button.Clicked += (sender, e) => Console.WriteLine($"Clicked: {e.Label}");

// Store reference to unsubscribe later
EventHandler<ClickEventArgs>? handler = null;
handler = (sender, e) =>
{
    Console.WriteLine($"Handler: {e.Label}");
    button.Clicked -= handler; // unsubscribe after one click
};
button.Clicked += handler;

button.DoClick("OK");
button.DoClick("OK"); // second click — handler already removed

class Button
{
    public event EventHandler<ClickEventArgs>? Clicked;
    public void DoClick(string label) => Clicked?.Invoke(this, new ClickEventArgs(label));
}
class ClickEventArgs(string label) : EventArgs { public string Label { get; } = label; }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the syntax for a lambda with multiple parameters? How do you use lambdas with generic types?

```cs
// Zero parameters
Func<string> greeting = () => "Hello, World!";
Console.WriteLine(greeting()); // Hello, World!

// One parameter (parentheses optional)
Func<int, int> square = x => x * x;
Func<int, int> cube   = (x) => x * x * x;

// Two parameters
Func<int, int, int>     add  = (a, b) => a + b;
Func<string, int, bool> fits = (str, max) => str.Length <= max;

// Three+ parameters
Func<int, int, int, int> clamp = (val, min, max) => Math.Clamp(val, min, max);
Console.WriteLine(clamp(15, 0, 10)); // 10

// Explicitly typed (required when compiler can\'t infer)
Func<IEnumerable<int>, int, bool> hasMore = (IEnumerable<int> items, int count) =>
    items.Count() > count;

// ---- GENERIC TYPES ----

// Lambda with generic method
static TResult Transform<T, TResult>(T value, Func<T, TResult> transform)
    => transform(value);

int length = Transform("hello", s => s.Length);      // 5
string upper = Transform(42, n => n.ToString("X"));   // 2A
Console.WriteLine(length);  // 5
Console.WriteLine(upper);   // 2A

// Generic delegate type
Func<T, T> Identity<T>() => x => x;
var intIdentity    = Identity<int>();
var stringIdentity = Identity<string>();
Console.WriteLine(intIdentity(42));        // 42
Console.WriteLine(stringIdentity("hi"));   // hi

// Generic pipeline / chain
static Func<T, TResult2> Compose<T, TResult1, TResult2>(
    Func<T, TResult1> first,
    Func<TResult1, TResult2> second) => x => second(first(x));

Func<string, int>    parseLength = s => s.Length;
Func<int, string>    describe    = n => $"length is {n}";
Func<string, string> combined    = Compose(parseLength, describe);

Console.WriteLine(combined("hello"));   // length is 5
Console.WriteLine(combined("dotnet"));  // length is 6

// LINQ with generic types
List<T> Filter<T>(IEnumerable<T> items, Func<T, bool> predicate)
    => items.Where(predicate).ToList();

var numbers = Filter(Enumerable.Range(1, 20), n => n % 3 == 0);
var names   = Filter(new[] { "Alice", "Bob", "Charlie" }, n => n.StartsWith('A'));
Console.WriteLine(string.Join(", ", numbers)); // 3, 6, 9, 12, 15, 18
Console.WriteLine(string.Join(", ", names));   // Alice
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 13. Language Integrated Query

<br/>

## Q. What is Expression Tree In C#?

An **Expression Tree** in C# is a data structure that represents code in a tree-like format, where each node is an expression (such as a method call, operation, or value). Expression trees are part of the `System.Linq.Expressions` namespace and are mainly used to represent code in a way that can be inspected, modified, or executed at runtime.

**Key Points:**

- Expression trees allow code to be represented as data, enabling dynamic query generation, compilation, and interpretation.
- They are widely used in LINQ providers (like Entity Framework) to translate C# queries into SQL or other query languages.
- Expression trees are built from lambda expressions using the `Expression<>` type.

**Example:**

```cs
using System;
using System.Linq.Expressions;

class Program
{
    static void Main()
    {
        // Create an expression tree for: x => x * 2
        Expression<Func<int, int>> expr = x => x * 2;

        // Compile and execute the expression tree
        var func = expr.Compile();
        Console.WriteLine(func(5)); // Output: 10

        // Inspect the expression tree
        Console.WriteLine(expr.Body); // Output: (x * 2)
    }
}
```

**Use Cases:**

- Building dynamic queries (e.g., in ORMs like Entity Framework)
- Creating dynamic code at runtime
- Analyzing or transforming code before execution

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is LINQ in C# and why is it used?

**LINQ (Language Integrated Query)** is a feature introduced in C# 3.0 that provides a uniform syntax for querying data from different sources — in-memory collections, databases (EF Core), XML, and more — directly inside C# code.

```cs
// Without LINQ
var result = new List<int>();
foreach (var n in new[] { 1, 2, 3, 4, 5, 6 })
    if (n % 2 == 0)
        result.Add(n * n);

// With LINQ — expressive, readable, composable
var result2 = new[] { 1, 2, 3, 4, 5, 6 }
    .Where(n => n % 2 == 0)
    .Select(n => n * n)
    .ToList(); // [4, 16, 36]
```

**Why use LINQ?**
- **Readability** — declarative style, reads like English
- **Type safety** — compile-time checking, IntelliSense
- **Composability** — chain operators freely
- **Unified API** — same syntax for arrays, `List<T>`, `IQueryable` (EF Core), XML
- **Deferred execution** — queries are lazy; no work until enumerated

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the main advantages of using LINQ in C#?

| Advantage | Description |
|-----------|-------------|
| **Concise syntax** | Replaces verbose loops with declarative one-liners |
| **Type safety** | Errors caught at compile time |
| **IntelliSense** | Full IDE support for auto-complete |
| **Deferred execution** | Query runs only when iterated — avoid wasted work |
| **Composable** | Chain multiple operators without intermediate collections |
| **Cross-source** | Same API for in-memory, SQL, XML, REST |
| **Maintainable** | Less boilerplate, intent is clear |
| **Parallel support** | PLINQ enables easy parallelism |

```cs
var products = new List<Product>
{
    new("Laptop",  1200, "Electronics"),
    new("Phone",    800, "Electronics"),
    new("Notebook",  10, "Stationery"),
    new("Pen",        2, "Stationery"),
};

// Composable pipeline — each step is a lazy transformation
var summary = products
    .Where(p => p.Price > 5)                            // filter
    .GroupBy(p => p.Category)                           // group
    .Select(g => new {                                  // project
        Category = g.Key,
        Count    = g.Count(),
        Total    = g.Sum(p => p.Price),
        Average  = g.Average(p => p.Price),
    })
    .OrderByDescending(x => x.Total);                  // sort

foreach (var s in summary)
    Console.WriteLine($"{s.Category}: {s.Count} items, total ${s.Total}");
// Electronics: 2 items, total $2000
// Stationery:  1 items, total $10

record Product(string Name, decimal Price, string Category);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you write a basic LINQ query in C#?

LINQ queries can be written in two equivalent syntaxes: **query syntax** (SQL-like) and **method syntax** (lambda chains). Method syntax is more commonly used in modern C#.

```cs
int[] numbers = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5];

// ── Query syntax ──────────────────────────────────────
var queryResult =
    from n in numbers
    where n > 3
    orderby n descending
    select n * 2;

// ── Equivalent method syntax ──────────────────────────
var methodResult = numbers
    .Where(n => n > 3)
    .OrderByDescending(n => n)
    .Select(n => n * 2);

foreach (int n in methodResult)
    Console.Write($"{n} "); // 18 12 10 10 10 8

// Basic operators used in most queries:
var data = new List<string> { "Alice", "Bob", "Charlie", "David", "Eve" };

// Filter
var longNames = data.Where(s => s.Length > 4);              // Charlie, David

// Project
var upper = data.Select(s => s.ToUpper());                  // ALICE, BOB ...

// Sort
var sorted = data.OrderBy(s => s.Length).ThenBy(s => s);    // Bob, Eve, Alice ...

// First match
string first = data.First(s => s.StartsWith('C'));          // Charlie

// Aggregate
int totalChars = data.Sum(s => s.Length);                   // 25
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between LINQ to Objects, LINQ to SQL, and LINQ to XML?

| | LINQ to Objects | LINQ to SQL | LINQ to XML |
|-|----------------|-------------|-------------|
| **Data source** | In-memory collections (`IEnumerable<T>`) | SQL Server tables | XML documents |
| **Interface** | `IEnumerable<T>` | `IQueryable<T>` | `IEnumerable<XElement>` |
| **Execution** | Always in-process | Translated to SQL, runs at DB | In-process DOM |
| **Namespace** | `System.Linq` | `System.Linq` + EF Core | `System.Xml.Linq` |
| **Translation** | None — pure C# | C# → SQL | C# → XPath-style |

```cs
// 1. LINQ to Objects — in-memory
int[] nums = [1, 2, 3, 4, 5];
var evens = nums.Where(n => n % 2 == 0).ToArray(); // [2, 4]

// 2. LINQ to SQL / EF Core — translated to SQL
// SELECT p.* FROM Products p WHERE p.Price > 100 ORDER BY p.Name
var products = await db.Products
    .Where(p => p.Price > 100)
    .OrderBy(p => p.Name)
    .ToListAsync(); // IQueryable<T> → SQL query at DB

// 3. LINQ to XML — query XML documents
var xml = XDocument.Parse("""
    <products>
      <product id="1"><name>Laptop</name><price>1200</price></product>
      <product id="2"><name>Phone</name><price>800</price></product>
    </products>
    """);

var names = xml.Root!
    .Elements("product")
    .Where(e => (decimal)e.Element("price")! > 900)
    .Select(e => (string)e.Element("name")!);

foreach (var name in names)
    Console.WriteLine(name); // Laptop
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you provide an example of a LINQ query that filters and sorts data?

```cs
var employees = new List<Employee>
{
    new(1, "Alice",   "Engineering", 95_000),
    new(2, "Bob",     "Marketing",   60_000),
    new(3, "Charlie", "Engineering", 85_000),
    new(4, "Diana",   "Engineering", 110_000),
    new(5, "Eve",     "HR",          55_000),
    new(6, "Frank",   "Marketing",   70_000),
};

// Filter: Engineering dept with salary > 80k; sort by salary descending
var senior = employees
    .Where(e => e.Department == "Engineering" && e.Salary > 80_000)
    .OrderByDescending(e => e.Salary)
    .ThenBy(e => e.Name)
    .Select(e => new { e.Name, e.Salary });

foreach (var e in senior)
    Console.WriteLine($"{e.Name}: ${e.Salary:N0}");
// Diana: $110,000
// Alice: $95,000
// Charlie: $85,000

// Query syntax equivalent
var seniorQuery =
    from e in employees
    where e.Department == "Engineering" && e.Salary > 80_000
    orderby e.Salary descending, e.Name
    select new { e.Name, e.Salary };

// Pagination
var page2 = employees
    .OrderBy(e => e.Name)
    .Skip(2)          // skip first 2
    .Take(2)          // take next 2
    .ToList();

record Employee(int Id, string Name, string Department, decimal Salary);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the different types of LINQ operators in C#?

LINQ operators are categorised by their function:

| Category | Operators |
|----------|-----------|
| **Filtering** | `Where`, `OfType` |
| **Projection** | `Select`, `SelectMany` |
| **Sorting** | `OrderBy`, `OrderByDescending`, `ThenBy`, `ThenByDescending`, `Reverse` |
| **Grouping** | `GroupBy`, `ToLookup` |
| **Joining** | `Join`, `GroupJoin`, `Zip` |
| **Set** | `Distinct`, `DistinctBy`, `Union`, `Intersect`, `Except`, `ExceptBy` |
| **Aggregation** | `Count`, `LongCount`, `Sum`, `Min`, `Max`, `Average`, `Aggregate` |
| **Element** | `First`, `FirstOrDefault`, `Last`, `LastOrDefault`, `Single`, `SingleOrDefault`, `ElementAt` |
| **Quantifiers** | `Any`, `All`, `Contains` |
| **Partitioning** | `Skip`, `Take`, `SkipWhile`, `TakeWhile`, `SkipLast`, `TakeLast` |
| **Conversion** | `ToList`, `ToArray`, `ToDictionary`, `ToHashSet`, `AsEnumerable`, `Cast` |
| **Concatenation** | `Concat`, `Append`, `Prepend` |
| **Generation** | `Range`, `Repeat`, `Empty` |

```cs
int[] a = [1, 2, 3, 4, 5];
int[] b = [3, 4, 5, 6, 7];

// Set operators
var union     = a.Union(b);           // [1,2,3,4,5,6,7]
var intersect = a.Intersect(b);       // [3,4,5]
var except    = a.Except(b);          // [1,2]

// Generation
var range  = Enumerable.Range(1, 5);  // [1,2,3,4,5]
var repeat = Enumerable.Repeat("x", 3); // ["x","x","x"]

// Partitioning
var page = a.Skip(1).Take(3);         // [2,3,4]
var tail = a.TakeLast(2);             // [4,5]

// Quantifiers
bool anyEven = a.Any(n => n % 2 == 0);  // true
bool allPos  = a.All(n => n > 0);       // true

// DistinctBy / MinBy / MaxBy (.NET 6+)
var words = new[] { "apple", "ant", "banana", "avocado" };
var byFirstLetter = words.DistinctBy(w => w[0]); // apple, banana
var shortest = words.MinBy(w => w.Length);         // ant
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use the `Select` and `Where` operators in LINQ?

- **`Where`** — filters a sequence (predicate returns `bool`)
- **`Select`** — projects/transforms each element into a new shape

```cs
var people = new List<Person>
{
    new("Alice", 30, "alice@example.com"),
    new("Bob",   17, "bob@example.com"),
    new("Carol", 25, "carol@example.com"),
    new("Dave",  15, "dave@example.com"),
};

// Where — filter adults
var adults = people.Where(p => p.Age >= 18);
// Alice, Carol

// Select — project to a new type
var emails = people.Select(p => p.Email);
// ["alice@example.com", ...]

// Combine: filter then project
var adultEmails = people
    .Where(p => p.Age >= 18)
    .Select(p => new { p.Name, p.Email });

// Select with index
var indexed = people
    .Select((p, i) => $"{i + 1}. {p.Name}");
// ["1. Alice", "2. Bob", ...]

// SelectMany — flatten nested collections
var orders = new List<Order>
{
    new("Alice", ["Laptop", "Mouse"]),
    new("Bob",   ["Phone"]),
};

var allItems = orders.SelectMany(o => o.Items);
// ["Laptop", "Mouse", "Phone"]

// SelectMany with result selector
var orderItems = orders.SelectMany(
    o => o.Items,
    (order, item) => new { order.Customer, Item = item });
// { Customer: "Alice", Item: "Laptop" }, ...

record Person(string Name, int Age, string Email);
record Order(string Customer, List<string> Items);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `GroupBy` operator in LINQ?

`GroupBy` partitions a sequence into groups by a key. Each group is an `IGrouping<TKey, TElement>` which exposes the `Key` and the matching elements.

```cs
var orders = new List<Order>
{
    new(1, "Alice", "Electronics", 1200m),
    new(2, "Bob",   "Electronics",  800m),
    new(3, "Alice", "Stationery",    15m),
    new(4, "Carol", "Electronics",  500m),
    new(5, "Bob",   "Stationery",     8m),
};

// Group by category
var byCategory = orders.GroupBy(o => o.Category);

foreach (var group in byCategory)
{
    Console.WriteLine($"Category: {group.Key} ({group.Count()} orders)");
    foreach (var o in group)
        Console.WriteLine($"  {o.Customer}: ${o.Amount}");
}
// Category: Electronics (3 orders)
//   Alice: $1200  Bob: $800  Carol: $500
// Category: Stationery (2 orders)
//   Alice: $15  Bob: $8

// GroupBy with aggregation (most common use)
var summary = orders
    .GroupBy(o => o.Category)
    .Select(g => new
    {
        Category = g.Key,
        Count    = g.Count(),
        Total    = g.Sum(o => o.Amount),
        Avg      = g.Average(o => o.Amount),
        Max      = g.Max(o => o.Amount),
    })
    .OrderByDescending(x => x.Total);

// Multi-key grouping (anonymous type as key)
var byCustomerAndCategory = orders
    .GroupBy(o => new { o.Customer, o.Category })
    .Select(g => new { g.Key.Customer, g.Key.Category, Total = g.Sum(o => o.Amount) });

// ToLookup — eager, indexed; GroupBy is lazy
var lookup = orders.ToLookup(o => o.Category);
var elecOrders = lookup["Electronics"]; // O(1) access

record Order(int Id, string Customer, string Category, decimal Amount);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you perform a join operation using LINQ?

```cs
var customers = new List<Customer>
{
    new(1, "Alice"),
    new(2, "Bob"),
    new(3, "Carol"),
};

var orders = new List<Order>
{
    new(101, 1, "Laptop",  1200m),
    new(102, 1, "Mouse",     25m),
    new(103, 2, "Phone",    800m),
    new(104, 2, "Case",      20m),
};

// 1. Inner Join — only matching rows (like SQL INNER JOIN)
var joined = customers.Join(
    orders,
    c => c.Id,          // outer key
    o => o.CustomerId,  // inner key
    (c, o) => new { c.Name, o.Product, o.Amount });

// Alice-Laptop, Alice-Mouse, Bob-Phone, Bob-Case

// 2. Group Join — like SQL LEFT OUTER JOIN
var grouped = customers.GroupJoin(
    orders,
    c => c.Id,
    o => o.CustomerId,
    (c, orderGroup) => new
    {
        c.Name,
        Orders = orderGroup.ToList(),
        Total  = orderGroup.Sum(o => o.Amount),
    });
// Carol has empty Orders list (no matching orders)

// 3. Left outer join using GroupJoin + SelectMany
var leftJoin = customers
    .GroupJoin(orders, c => c.Id, o => o.CustomerId,
        (c, os) => new { Customer = c, Orders = os })
    .SelectMany(
        x => x.Orders.DefaultIfEmpty(),
        (x, o) => new { x.Customer.Name, Product = o?.Product ?? "No orders" });
// Carol: No orders

// 4. Query syntax join
var queryJoin =
    from c in customers
    join o in orders on c.Id equals o.CustomerId
    select new { c.Name, o.Product, o.Amount };

// 5. Zip — pair elements by position
var letters = new[] { "A", "B", "C" };
var nums    = new[] { 1, 2, 3 };
var zipped  = letters.Zip(nums, (l, n) => $"{l}{n}"); // A1, B2, C3

record Customer(int Id, string Name);
record Order(int Id, int CustomerId, string Product, decimal Amount);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between deferred execution and immediate execution in LINQ?

| | Deferred Execution | Immediate Execution |
|-|--------------------|---------------------|
| **When runs** | When iterated (`foreach`, `ToList`, etc.) | Immediately when called |
| **Re-runs** | Every time you iterate | Result captured once |
| **Operators** | `Where`, `Select`, `OrderBy`, `GroupBy`, `Skip`, `Take`… | `ToList`, `ToArray`, `Count`, `First`, `Sum`, `Any`, `ToDictionary`… |

```cs
var data = new List<int> { 1, 2, 3, 4, 5 };

// Deferred — query is a recipe, not a result
var query = data.Where(n => n > 2); // nothing runs here

data.Add(6); // modifying source AFTER query definition

foreach (int n in query) // runs NOW — sees the added 6
    Console.Write($"{n} "); // 3 4 5 6

// Immediate — snapshot taken now
var snapshot = data.Where(n => n > 2).ToList();
data.Add(7);
Console.WriteLine(snapshot.Count); // 4 — does NOT see 7

// Deferred: re-evaluated each iteration
var counter = 0;
var lazy = data.Select(n => { counter++; return n; });
_ = lazy.ToList(); // counter = 7
_ = lazy.ToList(); // counter = 14 — ran again!

var eager = data.Select(n => { counter++; return n; }).ToList();
_ = eager.Count; // counter doesn\'t increase — already materialised

// Practical implication with EF Core
IQueryable<Product> query2 = db.Products.Where(p => p.Price > 100);
// No SQL sent yet

if (filterByCategory)
    query2 = query2.Where(p => p.Category == "Electronics"); // compose

var results = await query2.ToListAsync(); // ONE SQL query with both conditions
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use the `Aggregate` operator in LINQ?

`Aggregate` applies an accumulator function over a sequence, allowing arbitrary fold operations that built-in operators like `Sum` or `Max` don\'t cover.

```cs
int[] nums = [1, 2, 3, 4, 5];

// 1. Basic — sum (equivalent to nums.Sum())
int sum = nums.Aggregate((acc, n) => acc + n); // 15
// acc starts with first element: ((((1+2)+3)+4)+5)

// 2. With seed
int sumFromTen = nums.Aggregate(10, (acc, n) => acc + n); // 25

// 3. With seed + result selector
string result = nums.Aggregate(
    seed: new System.Text.StringBuilder(),
    func: (sb, n) => { sb.Append(n); sb.Append(','); return sb; },
    resultSelector: sb => sb.ToString().TrimEnd(','));
Console.WriteLine(result); // 1,2,3,4,5

// 4. Product
long product = nums.Aggregate(1L, (acc, n) => acc * n); // 120

// 5. Build a running max
int runMax = nums.Aggregate(int.MinValue, Math.Max); // 5

// 6. Aggregate words into a sentence
string[] words = ["The", "quick", "brown", "fox"];
string sentence = words.Aggregate((a, b) => $"{a} {b}");
Console.WriteLine(sentence); // The quick brown fox

// 7. Group counts with Aggregate (for illustration)
var freq = "abracadabra".Aggregate(
    new Dictionary<char, int>(),
    (dict, ch) =>
    {
        dict[ch] = dict.GetValueOrDefault(ch) + 1;
        return dict;
    });
// a:5, b:2, r:2, c:1, d:1

// Note: Aggregate is sequential. For parallel fold, use PLINQ .Aggregate()
// with partition-aware overloads.
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `Let` keyword in LINQ and how is it used?

`let` is a **query syntax** keyword that introduces a sub-expression (computed value) and gives it a name, which can then be reused in the rest of the query without recomputing it.

```cs
var words = new[] { "Hello", "World", "LINQ", "Rocks", "C#" };

// Without let — .Length evaluated twice
var longUpper =
    from w in words
    where w.Length > 4
    select w.ToUpper();

// With let — computed once and reused
var withLet =
    from w in words
    let upper  = w.ToUpper()        // computed once
    let length = upper.Length       // computed once
    where length > 4
    orderby length descending
    select $"{upper} ({length})";

foreach (var s in withLet)
    Console.WriteLine(s);
// HELLO (5)
// WORLD (5)
// ROCKS (5)

// Useful when sub-expression is expensive (e.g., regex match)
using System.Text.RegularExpressions;
var emails = new[] { "alice@example.com", "notanemail", "bob@test.org" };

var validDomains =
    from e in emails
    let m = Regex.Match(e, @"@(.+)$")
    where m.Success
    let domain = m.Groups[1].Value
    select domain;
// example.com, test.org

// Method syntax equivalent — use intermediate Select
var methodEquivalent = words
    .Select(w => (word: w, upper: w.ToUpper(), length: w.Length))
    .Where(x => x.length > 4)
    .OrderByDescending(x => x.length)
    .Select(x => $"{x.upper} ({x.length})");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle null values in LINQ queries?

```cs
var names = new List<string?> { "Alice", null, "Bob", null, "Carol" };

// 1. Filter out nulls
var nonNull = names.Where(n => n is not null); // Alice, Bob, Carol

// 2. OfType<T> — filters and casts (removes nulls from nullable sequences)
var nonNullOfType = names.OfType<string>(); // Alice, Bob, Carol

// 3. Null-coalescing in Select
var safe = names.Select(n => n ?? "Unknown"); // Alice, Unknown, Bob, ...

// 4. Null-conditional in nested navigation
var orders = new List<Order?> { new(1, null), null, new(3, "Laptop") };

var products = orders
    .Where(o => o?.Product is not null)
    .Select(o => o!.Product!.ToUpper());

// 5. FirstOrDefault / SingleOrDefault safely
var people = new List<Person> { new("Alice", 30), new("Bob", 25) };
Person? found = people.FirstOrDefault(p => p.Name == "Unknown");
string display = found?.Name ?? "Not found"; // Not found

// 6. DefaultIfEmpty — avoid empty sequence exceptions
var numbers = new List<int>();
int maxOrDefault = numbers.DefaultIfEmpty(0).Max(); // 0 instead of exception

// 7. Null guards in GroupBy / Join
var items = new List<Item> { new("x", null), new("y", "cat"), new("z", "cat") };

var grouped = items
    .Where(i => i.Category is not null)
    .GroupBy(i => i.Category!);

// 8. Nullable reference types + LINQ (.NET 10 / C# 14)
IEnumerable<string> guaranteed = names.Where(n => n != null)!; // suppress warning

record Person(string Name, int Age);
record Order(int Id, string? Product);
record Item(string Name, string? Category);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are anonymous types in LINQ and how are they used?

**Anonymous types** are compiler-generated, read-only reference types created with `new { ... }`. They are commonly used in LINQ `Select` projections to shape query results without defining an explicit class.

```cs
var products = new List<Product>
{
    new(1, "Laptop",  1200m, "Electronics"),
    new(2, "Phone",    800m, "Electronics"),
    new(3, "Notebook",  10m, "Stationery"),
};

// 1. Basic projection into anonymous type
var projections = products.Select(p => new
{
    p.Name,            // member name inferred from property
    p.Price,
    PriceWithTax = p.Price * 1.2m,  // computed member with explicit name
});

foreach (var item in projections)
    Console.WriteLine($"{item.Name}: ${item.PriceWithTax:F2}");
// Laptop: $1440.00 ...

// 2. Grouping result using anonymous type
var grouped = products
    .GroupBy(p => p.Category)
    .Select(g => new
    {
        Category = g.Key,
        Count    = g.Count(),
        Total    = g.Sum(p => p.Price),
    });

// 3. Multi-key grouping
var multiKey = products.GroupBy(p => new { p.Category })
    .Select(g => new { g.Key.Category, Avg = g.Average(p => p.Price) });

// 4. Anonymous types are structurally equal (compiler generates Equals/GetHashCode)
var a = new { Name = "Alice", Age = 30 };
var b = new { Name = "Alice", Age = 30 };
Console.WriteLine(a.Equals(b)); // true — value equality based on properties

// 5. Limitation: cannot return anonymous types from methods
// Use record types when you need to cross method boundaries
var dto = products.Select(p => new ProductDto(p.Id, p.Name, p.Price)).ToList();

// Prefer record over anonymous type in .NET 10
record ProductDto(int Id, string Name, decimal Price);
record Product(int Id, string Name, decimal Price, string Category);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use LINQ with collections in C#?

LINQ works with any type implementing `IEnumerable<T>` — arrays, `List<T>`, `Dictionary<K,V>`, `HashSet<T>`, `Stack<T>`, custom collections, etc.

```cs
// Arrays
int[] primes = [2, 3, 5, 7, 11, 13];
var largePrimes = primes.Where(n => n > 5).ToArray(); // [7, 11, 13]

// List<T>
var names = new List<string> { "Charlie", "Alice", "Bob" };
var sorted = names.OrderBy(n => n).ToList(); // [Alice, Bob, Charlie]

// Dictionary<K,V> — query KeyValuePairs
var scores = new Dictionary<string, int>
{
    ["Alice"] = 90, ["Bob"] = 75, ["Carol"] = 88
};
var topStudents = scores
    .Where(kv => kv.Value >= 80)
    .OrderByDescending(kv => kv.Value)
    .Select(kv => $"{kv.Key}: {kv.Value}");
// Alice: 90, Carol: 88

// HashSet<T>
var set1 = new HashSet<int> { 1, 2, 3, 4 };
var set2 = new HashSet<int> { 3, 4, 5, 6 };
var common = set1.Intersect(set2).ToHashSet(); // {3, 4}

// Stack<T> / Queue<T>
var stack = new Stack<int>(new[] { 1, 2, 3, 4, 5 });
var top3  = stack.Take(3).ToList(); // [5, 4, 3]

// Nested collections with SelectMany
var departments = new List<Department>
{
    new("Eng",  [new("Alice"), new("Bob")]),
    new("HR",   [new("Carol")]),
};

var allEmployees = departments.SelectMany(d => d.Employees); // flat list

// Convert LINQ result back to collection types
List<int>            list = primes.Where(n => n > 3).ToList();
int[]                arr  = primes.Where(n => n > 3).ToArray();
HashSet<int>         hs   = primes.ToHashSet();
Dictionary<int, int> dict = primes.ToDictionary(n => n, n => n * n);

record Department(string Name, List<Employee> Employees);
record Employee(string Name);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you optimize LINQ queries for performance?

```cs
// 1. Materialise once — avoid re-enumerating IEnumerable
// ❌ Query re-executes on each iteration
IEnumerable<int> query = data.Where(n => n > 0);
int count = query.Count(); // first iteration
int sum   = query.Sum();   // second iteration

// ✅ Materialise once
var list  = data.Where(n => n > 0).ToList();
int count2 = list.Count;
int sum2   = list.Sum();

// 2. AsNoTracking with EF Core (covered in DB section)
// 3. Use Any() not Count() > 0
// ❌ Counts all elements
if (data.Count() > 0) { }
// ✅ Stops at first match
if (data.Any()) { }

// 4. Filter early — Where before Select/OrderBy
// ❌ Sort all 1M items, then filter
data.OrderBy(n => n).Where(n => n > 1000);
// ✅ Filter first, sort fewer items
data.Where(n => n > 1000).OrderBy(n => n);

// 5. Use concrete collection operators over LINQ when possible
// ❌ LINQ on List when you know it\'s a List
int last = list.Last();
// ✅ Direct property
int last2 = list[^1]; // O(1) index vs O(n) LINQ Last()

// 6. PLINQ for CPU-intensive data processing
var result = data.AsParallel()
    .WithDegreeOfParallelism(Environment.ProcessorCount)
    .Where(ExpensivePredicate)
    .Select(ExpensiveTransform)
    .ToList();

// 7. Compiled LINQ for EF Core hot paths (see AsNoTracking section)
// 8. Avoid closures that capture large objects in query lambdas
int threshold = 100; // captured by value — fine
var filtered = data.Where(n => n > threshold);

// 9. Use FrozenDictionary for static lookup tables (.NET 8+)
using System.Collections.Frozen;
FrozenSet<int> allowedIds = new[] { 1, 5, 10, 42 }.ToFrozenSet();
var found = data.Where(n => allowedIds.Contains(n)); // O(1) lookup per element

// 10. Chunk for batched processing (.NET 6+)
foreach (int[] batch in data.Chunk(100))
    ProcessBatch(batch); // process in batches of 100

static bool ExpensivePredicate(int n) => n > 0;
static int  ExpensiveTransform(int n) => n * 2;
static void ProcessBatch(int[] batch) { }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain the differences between Parallel.ForEach and PLINQ (Parallel LINQ)?

| | `Parallel.ForEach` | PLINQ (`AsParallel()`) |
|-|--------------------|------------------------|
| **Style** | Imperative (action-based) | Declarative (LINQ pipeline) |
| **Result** | Side effects only | Returns `IEnumerable<T>` |
| **Ordering** | Not preserved | Use `AsOrdered()` to preserve |
| **Exception** | `AggregateException` | `AggregateException` |
| **Cancellation** | `ParallelOptions.CancellationToken` | `.WithCancellation(ct)` |
| **Best for** | Fire-and-forget parallel work | Data transformation pipelines |

```cs
int[] data = Enumerable.Range(1, 1_000_000).ToArray();

// 1. Parallel.ForEach — side effects, no return
var bag = new System.Collections.Concurrent.ConcurrentBag<int>();
Parallel.ForEach(data,
    new ParallelOptions { MaxDegreeOfParallelism = 4 },
    n => { if (n % 2 == 0) bag.Add(n * n); });
Console.WriteLine(bag.Count); // ~500,000

// 2. PLINQ — transforms and returns
var results = data.AsParallel()
    .WithDegreeOfParallelism(4)
    .Where(n => n % 2 == 0)
    .Select(n => n * n)
    .ToList(); // order not guaranteed

// 3. PLINQ with preserved order
var ordered = data.AsParallel()
    .AsOrdered()
    .Where(n => n % 2 == 0)
    .Select(n => n * n)
    .ToList(); // slower but maintains input order

// 4. PLINQ with cancellation
using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(5));
try
{
    var r = data.AsParallel()
        .WithCancellation(cts.Token)
        .Select(HeavyWork)
        .ToList();
}
catch (OperationCanceledException) { Console.WriteLine("Cancelled"); }

// 5. ForEachAsync (.NET 6+) — async parallel (best for I/O)
await Parallel.ForEachAsync(data.Take(100),
    new ParallelOptions { MaxDegreeOfParallelism = 10 },
    async (n, ct) => await ProcessAsync(n, ct));

// Recommendation:
// I/O-bound parallel work  → Parallel.ForEachAsync
// CPU-bound transformation → PLINQ
// CPU-bound side effects   → Parallel.ForEach

static int HeavyWork(int n) => n * n;
static async Task ProcessAsync(int n, CancellationToken ct) => await Task.Delay(1, ct);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does LINQ\'s "Where" method work?

`Where` is a **deferred, lazy** extension method on `IEnumerable<T>` that uses an iterator (via `yield return`) to evaluate the predicate for each element only as the sequence is consumed.

```cs
// Conceptual implementation of Where
public static IEnumerable<T> Where<T>(this IEnumerable<T> source, Func<T, bool> predicate)
{
    foreach (T item in source)
        if (predicate(item))
            yield return item; // caller receives control here
}

// 1. Basic usage
var nums = new[] { 1, 2, 3, 4, 5, 6 };
var evens = nums.Where(n => n % 2 == 0);
// Nothing evaluated yet — evens is an IEnumerable<int>

foreach (int n in evens)      // evaluation starts here
    Console.Write($"{n} ");   // 2 4 6

// 2. Predicate with index overload
var withIndex = nums.Where((n, i) => i % 2 == 0); // elements at even indices
// 1 (index 0), 3 (index 2), 5 (index 4)

// 3. Chained Where — each adds a filter (all evaluated in one pass)
var result = nums
    .Where(n => n > 2)    // first predicate
    .Where(n => n < 6);   // second predicate — single iteration total

// 4. Where with IQueryable<T> (EF Core)
// The lambda is NOT a delegate — it\'s an Expression<Func<T, bool>>
// EF Core translates it to SQL: WHERE Price > 100
var products = await db.Products
    .Where(p => p.Price > 100)
    .ToListAsync();

// 5. Short-circuit — Where stops iterating once caller stops
string? firstEven = nums.Where(n => n % 2 == 0).FirstOrDefault()?.ToString();
// Only evaluates until the first even number is found — does not scan the rest

// 6. Side effects and lazy evaluation
int callCount = 0;
var tracked = nums.Where(n => { callCount++; return n > 3; });
Console.WriteLine(callCount); // 0 — not yet evaluated
_ = tracked.ToList();
Console.WriteLine(callCount); // 6 — evaluated all 6 elements
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the benefits of a Deferred Execution in LINQ?

| Benefit | Description |
|---------|-------------|
| **Composability** | Build complex queries step by step without intermediate allocations |
| **Efficiency** | Only processes elements that reach the end of the pipeline |
| **Live data** | Query always sees the current state of the source |
| **EF Core integration** | Conditions added after query definition still generate one SQL statement |
| **Short-circuiting** | Operators like `First`, `Any` stop as soon as a match is found |

```cs
var data = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

// 1. Composability — no intermediate List allocations
var pipeline = data
    .Where(n => n % 2 == 0)   // IEnumerable — lazy
    .Select(n => n * n)        // IEnumerable — lazy
    .TakeWhile(n => n < 50);   // IEnumerable — lazy
// All three combined into ONE pass when iterated

foreach (int n in pipeline) Console.Write($"{n} "); // 4 16 36

// 2. Live view of source
var query = data.Where(n => n > 7);
data.Add(11);                  // added AFTER query defined
data.Add(12);
Console.WriteLine(string.Join(",", query)); // 8 9 10 11 12 — sees new items

// 3. EF Core — compose before executing
IQueryable<Product> q = db.Products.AsQueryable();
if (minPrice.HasValue) q = q.Where(p => p.Price >= minPrice.Value);
if (category != null) q = q.Where(p => p.Category == category);
var result = await q.ToListAsync(); // ONE SQL query with all conditions

// 4. Short-circuiting saves work
int checks = 0;
bool found = data
    .Where(n => { checks++; return n > 5; })
    .Any();  // stops at first match (n=6)
Console.WriteLine($"Checks: {checks}"); // 6 — not all 10 elements

// ⚠️ Caveat: re-enumeration re-executes the query
var expensive = data.Where(n => Expensive(n)); // avoid calling twice
var list = expensive.ToList(); // materialise once
Console.WriteLine(list.Count);
Console.WriteLine(list.Sum());

static bool Expensive(int n) => n > 0;
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you explain the difference between a query expression and a method chain in LINQ?

Both forms are equivalent — the compiler transforms query syntax into method calls during compilation. Method syntax is more powerful (supports all operators); query syntax is more readable for complex joins and grouping.

```cs
var products = new List<Product>
{
    new("Laptop",  1200m, "Electronics"),
    new("Phone",    800m, "Electronics"),
    new("Notebook",  10m, "Stationery"),
    new("Pen",        2m, "Stationery"),
};

// ── Query syntax (SQL-like) ────────────────────────────────────────────
var queryExpr =
    from p in products
    where p.Price > 50
    orderby p.Category, p.Price descending
    select new { p.Name, p.Price };

// ── Equivalent method chain ────────────────────────────────────────────
var methodChain = products
    .Where(p => p.Price > 50)
    .OrderBy(p => p.Category)
    .ThenByDescending(p => p.Price)
    .Select(p => new { p.Name, p.Price });

// ── Join — query syntax is more readable ──────────────────────────────
var customers = new List<(int Id, string Name)> { (1, "Alice"), (2, "Bob") };
var orders    = new List<(int CId, string Item)> { (1, "Laptop"), (1, "Mouse"), (2, "Phone") };

// Query syntax
var joinQuery =
    from c in customers
    join o in orders on c.Id equals o.CId
    select new { c.Name, o.Item };

// Method syntax (equivalent)
var joinMethod = customers.Join(
    orders, c => c.Id, o => o.CId,
    (c, o) => new { c.Name, o.Item });

// ── Operators ONLY available in method syntax ─────────────────────────
// (no query syntax equivalent)
var count = products.Count(p => p.Price > 100);
var first = products.FirstOrDefault(p => p.Price > 100);
var dist  = products.DistinctBy(p => p.Category);
var chunk = products.Chunk(2);

// ── let in query syntax = intermediate Select in method syntax ────────
var qLet =
    from p in products
    let discounted = p.Price * 0.9m
    where discounted > 100
    select new { p.Name, discounted };

var mLet = products
    .Select(p => (p, discounted: p.Price * 0.9m))
    .Where(x => x.discounted > 100)
    .Select(x => new { x.p.Name, x.discounted });

record Product(string Name, decimal Price, string Category);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you give an example of using LINQ to filter data in a collection?

```cs
var employees = new List<Employee>
{
    new(1, "Alice",   "Engineering", 95_000, new DateTime(2019, 3, 15)),
    new(2, "Bob",     "Marketing",   60_000, new DateTime(2021, 6,  1)),
    new(3, "Charlie", "Engineering", 85_000, new DateTime(2018, 1, 20)),
    new(4, "Diana",   "Engineering", 110_000, new DateTime(2015, 9, 10)),
    new(5, "Eve",     "HR",          55_000, new DateTime(2022, 4, 30)),
    new(6, "Frank",   "Marketing",   70_000, new DateTime(2020, 11, 5)),
};

// Filter by department
var engineers = employees.Where(e => e.Department == "Engineering");

// Filter by salary range
var midRange = employees.Where(e => e.Salary is >= 60_000 and <= 90_000);

// Filter by hire date (joined before 2020)
var senior = employees.Where(e => e.HireDate.Year < 2020);

// Multiple conditions
var seniorEngineers = employees
    .Where(e => e.Department == "Engineering"
             && e.Salary > 80_000
             && e.HireDate.Year < 2021);

// Filter with string operations
var alice = employees.Where(e => e.Name.StartsWith("A", StringComparison.OrdinalIgnoreCase));

// Filter in query syntax
var mktQuery =
    from e in employees
    where e.Department == "Marketing" && e.Salary >= 65_000
    select e;

// Complex filter — employees who joined in the last 3 years OR earn over 100k
var complex = employees.Where(e =>
    e.HireDate >= DateTime.UtcNow.AddYears(-3) || e.Salary > 100_000);

// Chained filters (same as AND)
var chained = employees
    .Where(e => e.Department == "Engineering")
    .Where(e => e.Salary > 80_000);

foreach (var e in seniorEngineers)
    Console.WriteLine($"{e.Name}: ${e.Salary:N0}, hired {e.HireDate:yyyy-MM-dd}");
// Diana: $110,000, hired 2015-09-10
// Charlie: $85,000, hired 2018-01-20

record Employee(int Id, string Name, string Department, decimal Salary, DateTime HireDate);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can LINQ be used to perform grouping and aggregation operations on data?

```cs
var sales = new List<Sale>
{
    new("Alice",   "Electronics", "Laptop",  1200m, new DateTime(2026, 1, 15)),
    new("Bob",     "Electronics", "Phone",    800m, new DateTime(2026, 1, 20)),
    new("Alice",   "Stationery",  "Notebook",  10m, new DateTime(2026, 2, 3)),
    new("Carol",   "Electronics", "Tablet",   600m, new DateTime(2026, 2, 10)),
    new("Bob",     "Stationery",  "Pens",       5m, new DateTime(2026, 2, 15)),
    new("Alice",   "Electronics", "Headphones",150m,new DateTime(2026, 3, 1)),
};

// 1. Group by single key with aggregate
var byCategory = sales
    .GroupBy(s => s.Category)
    .Select(g => new
    {
        Category = g.Key,
        Count    = g.Count(),
        Total    = g.Sum(s => s.Amount),
        Average  = g.Average(s => s.Amount),
        Min      = g.Min(s => s.Amount),
        Max      = g.Max(s => s.Amount),
    });

// 2. Group by multiple keys
var bySalesperson = sales
    .GroupBy(s => new { s.Salesperson, s.Category })
    .Select(g => new
    {
        g.Key.Salesperson,
        g.Key.Category,
        Total = g.Sum(s => s.Amount),
    })
    .OrderBy(x => x.Salesperson).ThenByDescending(x => x.Total);

// 3. Group by time period (month)
var byMonth = sales
    .GroupBy(s => new { s.Date.Year, s.Date.Month })
    .Select(g => new
    {
        Month = $"{g.Key.Year}-{g.Key.Month:D2}",
        Revenue = g.Sum(s => s.Amount),
        Orders  = g.Count(),
    })
    .OrderBy(x => x.Month);

// 4. Running totals with Aggregate
decimal running = 0;
var runningTotals = sales
    .OrderBy(s => s.Date)
    .Select(s => { running += s.Amount; return new { s.Salesperson, s.Amount, Running = running }; });

// 5. Top N per group
var topSalePerCategory = sales
    .GroupBy(s => s.Category)
    .SelectMany(g => g.OrderByDescending(s => s.Amount).Take(1));

foreach (var row in byCategory)
    Console.WriteLine($"{row.Category}: {row.Count} sales, total ${row.Total}");
// Electronics: 4 sales, total $2750
// Stationery: 2 sales, total $15

record Sale(string Salesperson, string Category, string Product, decimal Amount, DateTime Date);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does the Single Responsibility Principle (SRP) apply to LINQ code?

**SRP** states that a class/method should have only one reason to change. In LINQ: each query or method should do **one thing** — don\'t mix filtering, transforming, and persisting in the same expression.

```cs
// ❌ Violates SRP — one method filters, transforms, logs, AND saves
public async Task ProcessOrdersAsync(List<Order> orders, AppDbContext db)
{
    var result = orders
        .Where(o => o.Status == "Pending" && o.Amount > 100)
        .Select(o => { Console.WriteLine($"Processing {o.Id}"); return o; }) // side effect!
        .Select(o => new InvoiceDto(o.Id, o.Amount * 1.1m));

    db.Invoices.AddRange(result.Select(dto => new Invoice(dto)));
    await db.SaveChangesAsync();
}

// ✅ SRP — each method has one responsibility
public IEnumerable<Order> FilterEligibleOrders(IEnumerable<Order> orders) =>
    orders.Where(o => o.Status == "Pending" && o.Amount > 100);

public IEnumerable<InvoiceDto> ProjectToInvoiceDtos(IEnumerable<Order> orders) =>
    orders.Select(o => new InvoiceDto(o.Id, o.Amount * 1.1m));

public async Task SaveInvoicesAsync(IEnumerable<InvoiceDto> dtos, AppDbContext db)
{
    db.Invoices.AddRange(dtos.Select(dto => new Invoice(dto)));
    await db.SaveChangesAsync();
}

// Compose at call site
public async Task RunAsync(List<Order> orders, AppDbContext db)
{
    var eligible = FilterEligibleOrders(orders);
    var dtos     = ProjectToInvoiceDtos(eligible);
    await SaveInvoicesAsync(dtos, db);
}

record Order(int Id, string Status, decimal Amount);
record InvoiceDto(int OrderId, decimal Total);
record Invoice(InvoiceDto dto);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you demonstrate the use of LINQ to implement the Open-Closed Principle (OCP)?

**OCP** — open for extension, closed for modification. Represent query logic as injectable `Func<T, bool>` / `Expression<Func<T, bool>>` predicates so new filters can be added without modifying existing query code.

```cs
// Specification pattern — encapsulates query logic, open to extension
public interface ISpecification<T>
{
    Expression<Func<T, bool>> Criteria { get; }
}

public class PriceAboveSpecification(decimal min) : ISpecification<Product>
{
    public Expression<Func<Product, bool>> Criteria => p => p.Price > min;
}

public class CategorySpecification(string category) : ISpecification<Product>
{
    public Expression<Func<Product, bool>> Criteria => p => p.Category == category;
}

// Repository — does NOT change when new specs are added
public class ProductRepository(AppDbContext db)
{
    public async Task<List<Product>> FindAsync(ISpecification<Product> spec) =>
        await db.Products.Where(spec.Criteria).ToListAsync();
}

// Combine specs without modifying either
public static class SpecificationExtensions
{
    public static ISpecification<T> And<T>(
        this ISpecification<T> left, ISpecification<T> right) =>
        new AndSpecification<T>(left, right);
}

public class AndSpecification<T>(ISpecification<T> left, ISpecification<T> right)
    : ISpecification<T>
{
    public Expression<Func<T, bool>> Criteria
    {
        get
        {
            // Combine two expressions: left.Criteria AND right.Criteria
            var param = Expression.Parameter(typeof(T), "x");
            var body  = Expression.AndAlso(
                Expression.Invoke(left.Criteria,  param),
                Expression.Invoke(right.Criteria, param));
            return Expression.Lambda<Func<T, bool>>(body, param);
        }
    }
}

// Usage — extend by composing, not by modifying
var repo = new ProductRepository(db);
var spec = new PriceAboveSpecification(100)
    .And(new CategorySpecification("Electronics"));

var results = await repo.FindAsync(spec);

record Product(int Id, string Name, decimal Price, string Category);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does the Liskov Substitution Principle (LSP) apply to LINQ code?

**LSP** — a derived type must be substitutable for its base type. In LINQ: any `IEnumerable<T>` implementation (array, `List<T>`, EF Core `IQueryable<T>`) should be usable interchangeably in LINQ pipelines.

```cs
// ✅ Methods accept IEnumerable<T> — substitutable with any collection type
public static IEnumerable<Product> FilterExpensive(
    IEnumerable<Product> products, decimal threshold) =>
    products.Where(p => p.Price > threshold);

// All of these are substitutable — no code change required
Product[] array      = [new("Laptop", 1200m), new("Pen", 2m)];
List<Product> list   = [new("Laptop", 1200m), new("Pen", 2m)];
IQueryable<Product> query = db.Products; // EF Core

var r1 = FilterExpensive(array, 100);   // array
var r2 = FilterExpensive(list, 100);    // List<T>
var r3 = FilterExpensive(query, 100);   // IQueryable<T> (executes as SQL via EF)

// ❌ LSP violation — casting to concrete type breaks substitutability
public static List<Product> FilterBad(IEnumerable<Product> products, decimal t)
{
    var list = (List<Product>)products; // throws if array or IQueryable
    return list.Where(p => p.Price > t).ToList();
}

// ✅ Custom IEnumerable<T> that behaves like a sequence
public class ProductCatalog : IEnumerable<Product>
{
    private readonly List<Product> _items = [];
    public void Add(Product p) => _items.Add(p);
    public IEnumerator<Product> GetEnumerator() => _items.GetEnumerator();
    System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() =>
        GetEnumerator();
}

var catalog = new ProductCatalog();
catalog.Add(new("Laptop", 1200m));
var expensive = FilterExpensive(catalog, 100); // substitutable — works!

record Product(string Name, decimal Price);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you give an example of using LINQ to implement the Interface Segregation Principle (ISP)?

**ISP** — clients should not be forced to depend on interfaces they don\'t use. Split large data-source interfaces into focused ones; LINQ queries program against only what they need.

```cs
// ❌ Fat interface — query code must depend on write operations it doesn\'t use
public interface IProductRepository
{
    IQueryable<Product> Query();
    Task AddAsync(Product p);
    Task UpdateAsync(Product p);
    Task DeleteAsync(int id);
    Task SaveAsync();
}

// ✅ Segregated interfaces
public interface IProductReader     { IQueryable<Product> Query(); }
public interface IProductWriter
{
    Task AddAsync(Product p);
    Task UpdateAsync(Product p);
    Task DeleteAsync(int id);
    Task SaveAsync();
}

// LINQ service depends only on the reader
public class ProductQueryService(IProductReader reader)
{
    public Task<List<Product>> GetExpensiveAsync(decimal min) =>
        reader.Query()
              .Where(p => p.Price > min)
              .AsNoTracking()
              .ToListAsync();

    public Task<Dictionary<string, decimal>> GetAverageByCategory() =>
        reader.Query()
              .AsNoTracking()
              .GroupBy(p => p.Category)
              .Select(g => new { g.Key, Avg = g.Average(p => p.Price) })
              .ToDictionaryAsync(x => x.Key, x => x.Avg);
}

// Write service depends only on the writer
public class ProductWriteService(IProductWriter writer)
{
    public Task CreateAsync(Product p) => writer.AddAsync(p);
}

// Concrete repository implements both (no ISP violation here)
public class ProductRepository(AppDbContext db) : IProductReader, IProductWriter
{
    public IQueryable<Product> Query() => db.Products;
    public async Task AddAsync(Product p) { db.Products.Add(p); await db.SaveChangesAsync(); }
    public async Task UpdateAsync(Product p) { db.Products.Update(p); await db.SaveChangesAsync(); }
    public async Task DeleteAsync(int id) { /* ... */ }
    public Task SaveAsync() => db.SaveChangesAsync();
}

record Product(int Id, string Name, decimal Price, string Category);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you explain the Dependency Inversion Principle (DIP) and how it relates to LINQ code?

**DIP** — high-level modules should depend on abstractions, not concrete implementations. In LINQ: depend on `IEnumerable<T>` / `IQueryable<T>` abstractions, not on `List<T>`, `DbSet<T>`, or SQL.

```cs
// ❌ Violates DIP — high-level class depends on concrete EF Core DbSet
public class ReportService(AppDbContext db)
{
    public List<string> GetTopProductNames(int count) =>
        db.Products                    // concrete EF Core dependency
            .OrderByDescending(p => p.Price)
            .Take(count)
            .Select(p => p.Name)
            .ToList();
}

// ✅ DIP — depend on abstraction (IQueryable<T> or IProductReader)
public interface IProductReader
{
    IQueryable<Product> Query();
}

public class ReportService(IProductReader reader)  // depends on abstraction
{
    public async Task<List<string>> GetTopProductNamesAsync(int count) =>
        await reader.Query()
            .OrderByDescending(p => p.Price)
            .Take(count)
            .Select(p => p.Name)
            .ToListAsync();
}

// Production implementation — EF Core
public class EfProductReader(AppDbContext db) : IProductReader
{
    public IQueryable<Product> Query() => db.Products;
}

// Test implementation — in-memory
public class FakeProductReader(IEnumerable<Product> products) : IProductReader
{
    public IQueryable<Product> Query() => products.AsQueryable();
}

// Composition root (Program.cs)
builder.Services.AddScoped<IProductReader, EfProductReader>();
builder.Services.AddScoped<ReportService>();

// Unit test — no database needed
var fakeReader = new FakeProductReader(
[
    new(1, "Laptop", 1200m),
    new(2, "Phone",   800m),
    new(3, "Pen",       2m),
]);
var service = new ReportService(fakeReader);
var top2 = await service.GetTopProductNamesAsync(2);
// ["Laptop", "Phone"] — fully testable, no DB required

record Product(int Id, string Name, decimal Price);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain the difference between Select and Where?

| | `Where` | `Select` |
|-|---------|---------|
| **Purpose** | Filter — removes elements | Project — transforms elements |
| **Output count** | ≤ input count | = input count |
| **Element type** | Same `T` | Can change to any `TResult` |
| **Predicate** | `Func<T, bool>` | `Func<T, TResult>` |

```cs
var products = new List<Product>
{
    new("Laptop",  1200m, "Electronics"),
    new("Phone",    800m, "Electronics"),
    new("Notebook",  10m, "Stationery"),
};

// Where — filter (keeps same type, reduces count)
var electronics = products.Where(p => p.Category == "Electronics");
// [Laptop, Phone]  — still Product objects, count: 2

// Select — project (transforms type, same count)
var names = products.Select(p => p.Name);
// ["Laptop", "Phone", "Notebook"] — string objects, count: 3

// Select into a different type
var dtos = products.Select(p => new { p.Name, Discounted = p.Price * 0.9m });
// [{Laptop, 1080}, {Phone, 720}, {Notebook, 9}]

// Typical pattern: Where first, then Select (filter then project)
var expensiveNames = products
    .Where(p => p.Price > 100)       // 2 elements remain
    .Select(p => p.Name.ToUpper());  // ["LAPTOP", "PHONE"]

// Select does NOT filter — null projection requires Where
var allMaybeNull = products.Select(p => p.Price > 100 ? p.Name : null);
// ["Laptop", "Phone", null] — 3 elements, one null

// Filter nulls with Where
var filtered = allMaybeNull.Where(n => n is not null);

record Product(string Name, decimal Price, string Category);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `IEnumerable` and `IQueryable`?

| | `IEnumerable<T>` | `IQueryable<T>` |
|-|-----------------|-----------------|
| **Namespace** | `System.Collections.Generic` | `System.Linq` |
| **Execution** | In-process (CLR) | Translated to provider query (SQL, etc.) |
| **Expression** | Delegate (`Func<T, bool>`) | Expression tree (`Expression<Func<T, bool>>`) |
| **Data pulled** | All data from source, then filtered in memory | Only filtered data returned from DB |
| **Use case** | In-memory collections | EF Core, ORMs, remote data sources |
| **Extends** | `IEnumerable` | `IEnumerable<T>` + `IQueryable` |

```cs
// IEnumerable — in-memory filtering (pulls all rows first)
IEnumerable<Product> memProducts = db.Products.ToList(); // ALL rows loaded
var cheap = memProducts.Where(p => p.Price < 100);       // filtered in CLR
// SQL: SELECT * FROM Products  (no WHERE clause)

// IQueryable — DB-side filtering (only matching rows returned)
IQueryable<Product> dbProducts = db.Products;            // no SQL yet
var cheapQ = dbProducts.Where(p => p.Price < 100);       // builds expression tree
var result = await cheapQ.ToListAsync();                  // NOW executes SQL
// SQL: SELECT * FROM Products WHERE Price < 100

// Practical impact on performance
// Table with 1M rows, 10 match filter:
// IEnumerable: loads 1M rows → filters → 10 objects
// IQueryable:  DB filters → loads only 10 rows

// The Where predicate is different internally
IEnumerable<Product> e = [new("Laptop", 1200m)];
// Takes Func<Product, bool> — a compiled delegate
e.Where(p => p.Price > 100);

IQueryable<Product> q = e.AsQueryable();
// Takes Expression<Func<Product, bool>> — an expression tree
q.Where(p => p.Price > 100); // can be inspected and translated to SQL

// AsEnumerable — switch from IQueryable to IEnumerable mid-pipeline
// Useful when the final transform can\'t be translated to SQL
var data = await db.Products
    .Where(p => p.Price > 100)      // SQL WHERE
    .AsEnumerable()                 // switch to in-memory
    .Select(p => new { p.Name, Tag = FormatTag(p) }) // CLR method, no SQL translation needed
    .ToListAsync();                 // ❌ ToListAsync only on IQueryable; use ToList() here

var data2 = db.Products
    .Where(p => p.Price > 100)      // SQL WHERE
    .AsEnumerable()
    .Select(p => new { p.Name, Tag = FormatTag(p) })
    .ToList(); // ✅

static string FormatTag(Product p) => $"[{p.Name}]";
record Product(string Name, decimal Price);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 14. MICROSERVICES

<br/>

## Q. What are microservices and why are they used?

**Microservices** is an architectural style where an application is composed of small, independently deployable services, each responsible for a specific business capability and communicating via APIs.

```
Monolith                         Microservices
┌─────────────────────────┐      ┌───────────┐  ┌───────────┐  ┌───────────┐
│  UI + Business + Data   │      │  Order    │  │  Catalog  │  │  Payment  │
│  (all in one process)   │  →   │  Service  │  │  Service  │  │  Service  │
└─────────────────────────┘      └───────────┘  └───────────┘  └───────────┘
                                      ↑               ↑              ↑
                                 Each has its own DB, deploy, scale, team
```

**Why use microservices?**

| Benefit | Detail |
|---------|--------|
| **Independent deployment** | Deploy Order Service without touching Payment Service |
| **Independent scaling** | Scale only the Catalog Service during a sale |
| **Technology diversity** | Each service can use a different language/DB |
| **Fault isolation** | Catalog failure doesn\'t bring down Orders |
| **Team autonomy** | Small teams own end-to-end services |
| **Faster release cycles** | Smaller, focused deployments |

**When NOT to use microservices:**
- Small teams / early-stage products — start with a monolith
- When services need very frequent synchronous coordination (distributed monolith anti-pattern)
- When operational complexity (containers, service mesh, distributed tracing) outweighs benefits

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement microservices in .NET Core?

Each microservice is a separate ASP.NET Core Web API project with its own database, deployed independently as a Docker container.

```cs
// 1. Create a minimal microservice (OrderService)
// dotnet new webapi -n OrderService

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddDbContext<OrderDbContext>(opt =>
    opt.UseNpgsql(builder.Configuration.GetConnectionString("Orders")));
builder.Services.AddScoped<IOrderRepository, OrderRepository>();
builder.Services.AddScoped<OrderService>();

// Register HttpClient for inter-service calls
builder.Services.AddHttpClient<ICatalogClient, CatalogClient>(client =>
    client.BaseAddress = new Uri(builder.Configuration["Services:Catalog"]!));

var app = builder.Build();
app.MapOrderEndpoints(); // feature-sliced minimal API endpoints
app.Run();

// 2. Typed HttpClient for service-to-service communication
public class CatalogClient(HttpClient client)
{
    public async Task<CatalogItem?> GetItemAsync(int id, CancellationToken ct = default)
    {
        var response = await client.GetAsync($"/api/catalog/{id}", ct);
        response.EnsureSuccessStatusCode();
        return await response.Content.ReadFromJsonAsync<CatalogItem>(cancellationToken: ct);
    }
}

// 3. Simple endpoint
app.MapPost("/orders", async (CreateOrderRequest req, OrderService svc, CancellationToken ct) =>
{
    var order = await svc.CreateAsync(req, ct);
    return Results.Created($"/orders/{order.Id}", order);
});

// 4. Health checks — required for Kubernetes probes
builder.Services.AddHealthChecks()
    .AddNpgSql(connStr, name: "database")
    .AddUrlGroup(new Uri("http://catalog-service/health"), name: "catalog");

app.MapHealthChecks("/health");
app.MapHealthChecks("/health/ready", new() { Predicate = r => r.Tags.Contains("ready") });
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the main advantages of using microservices architecture?

| Advantage | Description |
|-----------|-------------|
| **Independent scaling** | Scale only bottleneck services |
| **Independent deployment** | Deploy/rollback one service without affecting others |
| **Technology flexibility** | Python ML model + C# API + Go service in same system |
| **Fault isolation** | Circuit breakers prevent cascade failures |
| **Team autonomy** | Teams own and deploy their service end-to-end |
| **Smaller codebases** | Easier to understand, test, and onboard |
| **Faster iteration** | Frequent small releases without full-system regression |
| **Horizontal scalability** | Run 10 instances of Order Service, 2 of Admin |

```
Example: E-Commerce Platform

┌─────────────┐   ┌──────────────┐   ┌─────────────┐   ┌─────────────┐
│  API Gateway │──▶│ Order Service│──▶│Catalog Svc  │   │Payment Svc  │
│ (YARP/Ocelot)│   │  (C# + PG)  │   │(C# + PG)    │   │(C# + Redis) │
└─────────────┘   └──────────────┘   └─────────────┘   └─────────────┘
                         │                                      │
                  ┌──────▼──────┐                    ┌─────────▼──────┐
                  │ RabbitMQ /  │                    │  Notification  │
                  │ Azure SB    │──────────────────▶│  Service       │
                  └─────────────┘                    └────────────────┘

Each service:
 - Owns its database (no shared DB)
 - Has its own Docker image
 - Has its own CI/CD pipeline
 - Scales independently in Kubernetes
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle communication between microservices in .NET Core?

**Synchronous** (request-response): HTTP/REST, gRPC
**Asynchronous** (event-driven): message queues (RabbitMQ, Azure Service Bus, Kafka)

```cs
// 1. HTTP REST — typed HttpClient via IHttpClientFactory
builder.Services.AddHttpClient<ICatalogClient, CatalogClient>(client =>
    client.BaseAddress = new Uri("http://catalog-service"));

public class CatalogClient(HttpClient client)
{
    public Task<Product?> GetProductAsync(int id) =>
        client.GetFromJsonAsync<Product>($"/products/{id}");
}

// 2. gRPC — binary protocol, strongly-typed contracts (.proto files)
// dotnet add package Grpc.AspNetCore
// Service: catalog.proto → generated CatalogService.CatalogServiceClient

builder.Services.AddGrpcClient<CatalogService.CatalogServiceClient>(opts =>
    opts.Address = new Uri("https://catalog-service:5001"));

public class OrderService(CatalogService.CatalogServiceClient grpcClient)
{
    public async Task<ProductInfo> GetProductInfoAsync(int id)
    {
        var reply = await grpcClient.GetProductAsync(new ProductRequest { Id = id });
        return new ProductInfo(reply.Name, reply.Price);
    }
}

// 3. Async messaging — MassTransit + RabbitMQ
// dotnet add package MassTransit.RabbitMQ
builder.Services.AddMassTransit(x =>
{
    x.AddConsumer<OrderCreatedConsumer>();
    x.UsingRabbitMq((ctx, cfg) =>
    {
        cfg.Host("rabbitmq://localhost");
        cfg.ConfigureEndpoints(ctx);
    });
});

// Publisher
public class OrderService(IPublishEndpoint publish)
{
    public async Task CreateOrderAsync(CreateOrderRequest req)
    {
        // ... create order in DB ...
        await publish.Publish(new OrderCreated(orderId, req.Items));
    }
}

// Consumer in another service
public class OrderCreatedConsumer : IConsumer<OrderCreated>
{
    public async Task Consume(ConsumeContext<OrderCreated> ctx)
    {
        var msg = ctx.Message;
        // process the event (e.g., send confirmation email)
    }
}

record OrderCreated(int OrderId, List<OrderItem> Items);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the role of API Gateway in microservices architecture?

The **API Gateway** is the single entry point for all clients. It handles routing, authentication, rate limiting, SSL termination, and request aggregation — preventing clients from knowing about individual services.

```
Client (React App / Mobile)
          │
          ▼
  ┌───────────────┐
  │  API Gateway  │  ← YARP / Ocelot / Azure API Management
  │               │    - Route /orders → OrderService
  │  Auth (JWT)   │    - Route /catalog → CatalogService
  │  Rate Limit   │    - Aggregate /dashboard → multiple services
  │  Load Balance │    - Strip/add headers
  └───────────────┘
     /      |      \
Order   Catalog  Payment
Service Service  Service
```

```cs
// YARP (Yet Another Reverse Proxy) — Microsoft\'s API Gateway (.NET 10)
// dotnet add package Yarp.ReverseProxy

builder.Services.AddReverseProxy()
    .LoadFromConfig(builder.Configuration.GetSection("ReverseProxy"));

app.MapReverseProxy();

// appsettings.json
{
  "ReverseProxy": {
    "Routes": {
      "orders-route": {
        "ClusterId": "orders-cluster",
        "Match": { "Path": "/api/orders/{**catch-all}" },
        "AuthorizationPolicy": "default"
      },
      "catalog-route": {
        "ClusterId": "catalog-cluster",
        "Match": { "Path": "/api/catalog/{**catch-all}" }
      }
    },
    "Clusters": {
      "orders-cluster": {
        "Destinations": {
          "primary": { "Address": "http://order-service:8080/" }
        }
      },
      "catalog-cluster": {
        "Destinations": {
          "primary":   { "Address": "http://catalog-service:8080/" },
          "secondary": { "Address": "http://catalog-service-2:8080/" }
        },
        "LoadBalancingPolicy": "RoundRobin"
      }
    }
  }
}

// Add rate limiting
builder.Services.AddRateLimiter(opts =>
    opts.AddFixedWindowLimiter("api", o =>
    {
        o.PermitLimit = 100;
        o.Window = TimeSpan.FromMinutes(1);
    }));
app.UseRateLimiter();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you manage data consistency in microservices?

Each microservice owns its database — no shared DB. Consistency is maintained through **eventual consistency** patterns.

```cs
// 1. Saga Pattern (Choreography) — services react to events
// OrderService publishes → InventoryService and PaymentService consume

// OrderService
await publishEndpoint.Publish(new OrderPlaced(orderId, customerId, items));

// InventoryService consumer
public class OrderPlacedConsumer : IConsumer<OrderPlaced>
{
    public async Task Consume(ConsumeContext<OrderPlaced> ctx)
    {
        var reserved = await inventory.ReserveAsync(ctx.Message.Items);
        if (reserved)
            await ctx.Publish(new InventoryReserved(ctx.Message.OrderId));
        else
            await ctx.Publish(new InventoryFailed(ctx.Message.OrderId));
    }
}

// 2. Saga Pattern (Orchestration) — MassTransit StateMachine
public class OrderStateMachine : MassTransitStateMachine<OrderState>
{
    public OrderStateMachine()
    {
        Initially(
            When(OrderPlacedEvent)
                .Activity(x => x.OfInstanceType<ReserveInventoryActivity>())
                .TransitionTo(AwaitingInventory));

        During(AwaitingInventory,
            When(InventoryReservedEvent)
                .Activity(x => x.OfInstanceType<ChargePaymentActivity>())
                .TransitionTo(AwaitingPayment),
            When(InventoryFailedEvent)
                .TransitionTo(Cancelled));
    }

    public State AwaitingInventory { get; private set; } = default!;
    public State AwaitingPayment   { get; private set; } = default!;
    public State Cancelled         { get; private set; } = default!;
    public Event<OrderPlaced>         OrderPlacedEvent         { get; private set; } = default!;
    public Event<InventoryReserved>   InventoryReservedEvent   { get; private set; } = default!;
    public Event<InventoryFailed>     InventoryFailedEvent     { get; private set; } = default!;
}

// 3. Outbox Pattern — guarantee event delivery even if service crashes
// Store event in DB (same transaction as business data), then publish
await using var tx = await db.Database.BeginTransactionAsync();
db.Orders.Add(newOrder);
db.OutboxMessages.Add(new OutboxMessage(
    nameof(OrderCreated),
    JsonSerializer.Serialize(new OrderCreated(newOrder.Id))));
await db.SaveChangesAsync(); // atomic: order + outbox message
await tx.CommitAsync();
// Background worker reads outbox and publishes to message bus
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are some common challenges when working with microservices?

| Challenge | Description | Solution |
|-----------|-------------|---------|
| **Distributed tracing** | Requests span multiple services | OpenTelemetry + Jaeger/Zipkin |
| **Data consistency** | No shared DB, eventual consistency | Saga pattern, outbox |
| **Network failures** | Inter-service calls can fail | Retry, circuit breaker (Polly) |
| **Service discovery** | Services need to find each other | Kubernetes DNS, Consul |
| **Testing complexity** | Integration tests across services | Contract testing (Pact), test containers |
| **Security** | JWT propagation, mTLS | JWT forwarding, service mesh |
| **Configuration** | Many services, many configs | Kubernetes ConfigMaps, Azure App Config |
| **Versioning** | API changes break consumers | Versioned APIs, backward compat |
| **Operational overhead** | Many deployments to manage | Kubernetes, Helm, GitOps |

```cs
// Polly — resilience library for network failures
// dotnet add package Microsoft.Extensions.Http.Resilience (.NET 8+)

builder.Services.AddHttpClient<ICatalogClient, CatalogClient>()
    .AddStandardResilienceHandler(opts =>
    {
        opts.Retry.MaxRetryAttempts = 3;
        opts.Retry.Delay = TimeSpan.FromMilliseconds(200);
        opts.Retry.BackoffType = DelayBackoffType.Exponential;
        opts.CircuitBreaker.FailureRatio = 0.5;
        opts.CircuitBreaker.SamplingDuration = TimeSpan.FromSeconds(10);
        opts.TotalRequestTimeout.Timeout = TimeSpan.FromSeconds(30);
    });

// OpenTelemetry — distributed tracing across services
builder.Services.AddOpenTelemetry()
    .WithTracing(tracing => tracing
        .AddAspNetCoreInstrumentation()
        .AddHttpClientInstrumentation()
        .AddEntityFrameworkCoreInstrumentation()
        .AddOtlpExporter(opts => opts.Endpoint = new Uri("http://jaeger:4317")));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you deploy microservices in a containerized environment?

```dockerfile
# Dockerfile — multi-stage build for OrderService
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src
COPY ["OrderService/OrderService.csproj", "OrderService/"]
RUN dotnet restore "OrderService/OrderService.csproj"
COPY . .
RUN dotnet publish "OrderService/OrderService.csproj" -c Release -o /app/publish \
    --no-restore /p:UseAppHost=false

FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS runtime
WORKDIR /app
COPY --from=build /app/publish .
EXPOSE 8080
ENTRYPOINT ["dotnet", "OrderService.dll"]
```

```yaml
# Kubernetes deployment — order-service.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: order-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: order-service
  template:
    metadata:
      labels:
        app: order-service
    spec:
      containers:
      - name: order-service
        image: myregistry.azurecr.io/order-service:1.2.0
        ports:
        - containerPort: 8080
        env:
        - name: ConnectionStrings__Orders
          valueFrom:
            secretKeyRef:
              name: db-secrets
              key: orders-conn-string
        resources:
          requests: { cpu: "100m", memory: "128Mi" }
          limits:   { cpu: "500m", memory: "512Mi" }
        livenessProbe:
          httpGet: { path: /health, port: 8080 }
          initialDelaySeconds: 10
        readinessProbe:
          httpGet: { path: /health/ready, port: 8080 }
---
apiVersion: v1
kind: Service
metadata:
  name: order-service
spec:
  selector:
    app: order-service
  ports:
  - port: 80
    targetPort: 8080
```

```bash
# Deploy
kubectl apply -f order-service.yaml

# Rolling update to new version
kubectl set image deployment/order-service order-service=myregistry.azurecr.io/order-service:1.3.0

# Scale
kubectl scale deployment order-service --replicas=5
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of service discovery in microservices?

**Service discovery** allows microservices to find each other\'s network locations dynamically — without hardcoded IP addresses — as services scale, restart, or move.

```
Without service discovery:
  OrderService → "http://192.168.1.42:8080" (hardcoded — breaks on redeploy)

With service discovery:
  OrderService → "http://catalog-service" → Discovery resolves → "http://10.0.0.15:8080"
```

| Approach | Tools | .NET Integration |
|----------|-------|-----------------|
| **Kubernetes DNS** | K8s built-in | `http://catalog-service` resolves via kube-dns |
| **Consul** | HashiCorp Consul | `Steeltoe.Discovery.Consul` |
| **Eureka** | Netflix Eureka | `Steeltoe.Discovery.Eureka` |
| **Azure Service Fabric** | Service Fabric DNS | Built-in naming service |

```cs
// Kubernetes — simplest; use service name as hostname
builder.Services.AddHttpClient<ICatalogClient, CatalogClient>(client =>
{
    // K8s DNS resolves "catalog-service" to the ClusterIP
    client.BaseAddress = new Uri(
        builder.Configuration["Services:Catalog"] ?? "http://catalog-service");
});

// Consul service discovery (Steeltoe)
// dotnet add package Steeltoe.Discovery.Consul

builder.Services.AddServiceDiscovery(b => b.UseConsul());
builder.Services.AddHttpClient<ICatalogClient, CatalogClient>()
    .AddServiceDiscovery(); // resolves "catalog-service" via Consul

// appsettings.json
{
  "Consul": { "Host": "consul-server", "Port": 8500 },
  "Spring": {
    "Application": { "Name": "order-service" },
    "Cloud": { "Discovery": { "Enabled": true } }
  }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement logging and monitoring in microservices?

```cs
// 1. Structured logging with Serilog — correlate across services
// dotnet add package Serilog.AspNetCore Serilog.Sinks.OpenTelemetry

builder.Host.UseSerilog((ctx, cfg) => cfg
    .ReadFrom.Configuration(ctx.Configuration)
    .Enrich.FromLogContext()
    .Enrich.WithProperty("Service", "OrderService")
    .Enrich.WithProperty("Environment", ctx.HostingEnvironment.EnvironmentName)
    .WriteTo.Console(new JsonFormatter())
    .WriteTo.OpenTelemetry(opts =>
        opts.Endpoint = "http://otel-collector:4317"));

// Use correlation ID middleware
app.Use(async (ctx, next) =>
{
    var correlationId = ctx.Request.Headers["X-Correlation-ID"].FirstOrDefault()
                        ?? Guid.NewGuid().ToString();
    ctx.Response.Headers["X-Correlation-ID"] = correlationId;
    using (Serilog.Context.LogContext.PushProperty("CorrelationId", correlationId))
        await next(ctx);
});

// 2. OpenTelemetry — metrics + tracing + logs
builder.Services.AddOpenTelemetry()
    .WithTracing(t => t
        .AddAspNetCoreInstrumentation()
        .AddHttpClientInstrumentation()
        .AddSource("OrderService")
        .AddOtlpExporter(o => o.Endpoint = new Uri("http://otel-collector:4317")))
    .WithMetrics(m => m
        .AddAspNetCoreInstrumentation()
        .AddRuntimeInstrumentation()
        .AddOtlpExporter());

// 3. Health checks with detailed status
builder.Services.AddHealthChecks()
    .AddNpgSql(connStr)
    .AddRabbitMQ(rabbitUri)
    .AddCheck("self", () => HealthCheckResult.Healthy());

app.MapHealthChecks("/health/live",  new() { Predicate = _ => false });  // liveness
app.MapHealthChecks("/health/ready", new() { Predicate = _ => true  });  // readiness

// 4. Custom metrics
var meter = new System.Diagnostics.Metrics.Meter("OrderService");
var ordersCreated = meter.CreateCounter<long>("orders.created");
ordersCreated.Add(1, new("status", "success"));

// 5. Propagate trace context across HTTP calls (automatic with HttpClient instrumentation)
// X-B3-TraceId / traceparent headers forwarded automatically
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between monolithic and microservices architecture?

| Aspect | Monolith | Microservices |
|--------|----------|---------------|
| **Codebase** | Single deployable unit | Multiple independent services |
| **Deployment** | Deploy entire app for any change | Deploy only changed service |
| **Scaling** | Scale the whole app | Scale individual services |
| **Technology** | Single stack | Polyglot — each service chooses |
| **Data** | Single shared database | Each service owns its DB |
| **Failure** | One bug can crash everything | Failures isolated per service |
| **Complexity** | Simple locally, hard to scale | Complex ops, easy to scale |
| **Team size** | Small-medium teams | Large orgs, multiple teams |
| **Testing** | Simpler end-to-end | Complex distributed testing |
| **Latency** | In-process calls (fast) | Network calls (slower) |

```
When to choose what:

Monolith ✅                    Microservices ✅
─────────────────              ─────────────────────────────────
Early-stage startup            Large org with multiple teams
Small team (<10 devs)          High scale requirements
Unclear domain boundaries      Well-understood bounded contexts
Simple operational needs       Independent release cadence needed
Proof of concept               Different scaling needs per component

Migration path:
Monolith → Strangler Fig Pattern → Microservices
  1. Identify bounded context (e.g., Payment)
  2. Wrap it behind an interface
  3. Extract to separate service behind API Gateway
  4. Route requests to new service
  5. Remove from monolith
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle security in microservices?

```cs
// 1. JWT authentication — validate in each service
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(opts =>
    {
        opts.Authority = "https://identity-server"; // OIDC discovery
        opts.Audience  = "order-service";
        opts.TokenValidationParameters = new()
        {
            ValidateIssuer   = true,
            ValidateAudience = true,
            ValidateLifetime = true,
        };
    });

builder.Services.AddAuthorization(opts =>
    opts.AddPolicy("orders:write", p => p.RequireClaim("scope", "orders:write")));

app.UseAuthentication();
app.UseAuthorization();

app.MapPost("/orders", CreateOrder).RequireAuthorization("orders:write");

// 2. Forward JWT between services (propagate identity)
builder.Services.AddHttpClient<ICatalogClient, CatalogClient>()
    .AddHttpMessageHandler<JwtForwardingHandler>();

public class JwtForwardingHandler(IHttpContextAccessor accessor) : DelegatingHandler
{
    protected override Task<HttpResponseMessage> SendAsync(
        HttpRequestMessage request, CancellationToken ct)
    {
        var token = accessor.HttpContext?.Request.Headers.Authorization.ToString();
        if (!string.IsNullOrEmpty(token))
            request.Headers.Authorization =
                System.Net.Http.Headers.AuthenticationHeaderValue.Parse(token);
        return base.SendAsync(request, ct);
    }
}

// 3. mTLS — mutual TLS for service-to-service (via service mesh: Istio / Linkerd)
// Zero-code change; sidecar proxy handles certificate verification

// 4. Secrets management — never store secrets in code
// Kubernetes secrets
var connStr = builder.Configuration["ConnectionStrings__Orders"]; // from K8s Secret
// Azure Key Vault
builder.Configuration.AddAzureKeyVault(new Uri("https://myvault.vault.azure.net/"),
    new DefaultAzureCredential());
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the role of Docker in microservices?

Docker packages each microservice and its dependencies into an **image** — a portable, reproducible unit that runs consistently everywhere.

```dockerfile
# Each microservice has its own Dockerfile
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src
COPY . .
RUN dotnet publish -c Release -o /app

FROM mcr.microsoft.com/dotnet/aspnet:10.0
WORKDIR /app
COPY --from=build /app .
EXPOSE 8080
ENTRYPOINT ["dotnet", "CatalogService.dll"]
```

```yaml
# docker-compose.yml — run all services locally
version: "3.9"
services:
  api-gateway:
    build: ./ApiGateway
    ports: ["5000:8080"]
    depends_on: [order-service, catalog-service]

  order-service:
    build: ./OrderService
    environment:
      - ConnectionStrings__Orders=Host=postgres;Database=orders;Username=app;Password=secret
    depends_on: [postgres, rabbitmq]

  catalog-service:
    build: ./CatalogService
    depends_on: [postgres]

  postgres:
    image: postgres:16-alpine
    volumes: ["pgdata:/var/lib/postgresql/data"]
    environment:
      POSTGRES_PASSWORD: secret

  rabbitmq:
    image: rabbitmq:3-management
    ports: ["15672:15672"]

volumes:
  pgdata:
```

```bash
# Build and run all services
docker compose up --build

# Build a single image
docker build -t myregistry.azurecr.io/order-service:1.0.0 ./OrderService

# Push to registry
docker push myregistry.azurecr.io/order-service:1.0.0

# Run a single service
docker run -p 8080:8080 myregistry.azurecr.io/order-service:1.0.0
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement resilience and fault tolerance in microservices?

```cs
// Microsoft.Extensions.Http.Resilience (.NET 8+ / Polly v8)
// dotnet add package Microsoft.Extensions.Http.Resilience

builder.Services.AddHttpClient<ICatalogClient, CatalogClient>()
    .AddStandardResilienceHandler(opts =>
    {
        // Retry — 3 times with exponential backoff + jitter
        opts.Retry.MaxRetryAttempts = 3;
        opts.Retry.Delay            = TimeSpan.FromMilliseconds(200);
        opts.Retry.BackoffType      = DelayBackoffType.Exponential;
        opts.Retry.UseJitter        = true;
        opts.Retry.ShouldHandle     = args =>
            ValueTask.FromResult(args.Outcome.Exception is HttpRequestException);

        // Circuit Breaker — open after 50% failure in 10-second window
        opts.CircuitBreaker.FailureRatio    = 0.5;
        opts.CircuitBreaker.SamplingDuration = TimeSpan.FromSeconds(10);
        opts.CircuitBreaker.MinimumThroughput = 10;
        opts.CircuitBreaker.BreakDuration    = TimeSpan.FromSeconds(30);

        // Timeout per attempt
        opts.AttemptTimeout.Timeout = TimeSpan.FromSeconds(5);

        // Total timeout across all retries
        opts.TotalRequestTimeout.Timeout = TimeSpan.FromSeconds(30);
    });

// Fallback / graceful degradation
builder.Services.AddResiliencePipeline("catalog-fallback", builder =>
{
    builder.AddFallback(new FallbackStrategyOptions<Product?>
    {
        FallbackAction = _ => ValueTask.FromResult<Product?>(Product.Default),
        ShouldHandle   = args => ValueTask.FromResult(
            args.Outcome.Exception is BrokenCircuitException),
    });
});

// Bulkhead — limit concurrent requests to a service
builder.Services.AddResiliencePipeline("bulkhead", b =>
    b.AddConcurrencyLimiter(permitLimit: 10, queueLimit: 20));

// Health checks for circuit breaker status
builder.Services.AddHealthChecks()
    .AddCheck("catalog-circuit-breaker", () =>
        circuitBreakerState == CircuitState.Closed
            ? HealthCheckResult.Healthy()
            : HealthCheckResult.Degraded("Circuit breaker is open"));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are some best practices for designing microservices?

| Practice | Description |
|----------|-------------|
| **Design around business domains** | Use Domain-Driven Design bounded contexts |
| **Single responsibility** | Each service does one thing well |
| **Own your data** | No shared databases between services |
| **API-first** | Define contracts (OpenAPI/gRPC) before implementation |
| **Async by default** | Prefer events over synchronous calls |
| **Design for failure** | Retry, circuit breaker, fallback everywhere |
| **Health checks** | Liveness + readiness probes |
| **Structured logging** | Correlation IDs, JSON logs |
| **Distributed tracing** | OpenTelemetry propagation |
| **Versioned APIs** | Never break consumers |
| **Small, frequent releases** | CI/CD per service |
| **Automate everything** | Docker + Kubernetes + GitOps |

```cs
// Checklist for a new microservice:

// ✅ 1. Health endpoints
app.MapHealthChecks("/health/live");
app.MapHealthChecks("/health/ready");

// ✅ 2. Structured logging with correlation
builder.Host.UseSerilog((ctx, cfg) => cfg
    .Enrich.FromLogContext()
    .WriteTo.Console(new JsonFormatter()));

// ✅ 3. OpenTelemetry tracing
builder.Services.AddOpenTelemetry()
    .WithTracing(t => t.AddAspNetCoreInstrumentation().AddOtlpExporter());

// ✅ 4. Resilient HTTP clients
builder.Services.AddHttpClient<IDownstreamClient, DownstreamClient>()
    .AddStandardResilienceHandler();

// ✅ 5. Versioned API
app.MapGroup("/api/v1").MapOrderEndpoints();

// ✅ 6. Graceful shutdown
app.Lifetime.ApplicationStopping.Register(() =>
    logger.LogInformation("Shutting down gracefully..."));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Name the key components of Microservices?

```
Key Components of a Microservices System:

┌─────────────────────────────────────────────────────────────────────┐
│  CLIENT (Browser / Mobile / 3rd-party)                              │
└──────────────────────┬──────────────────────────────────────────────┘
                       │
              ┌────────▼─────────┐
              │   API Gateway    │  Routing, Auth, Rate Limit, SSL
              │  (YARP / Ocelot) │
              └────────┬─────────┘
          ┌────────────┼────────────┐
    ┌─────▼─────┐ ┌────▼────┐ ┌────▼──────┐
    │  Order    │ │Catalog  │ │ Payment   │   ← Individual Services
    │  Service  │ │ Service │ │ Service   │
    └─────┬─────┘ └────┬────┘ └────┬──────┘
          │            │           │
    ┌─────▼──┐   ┌─────▼──┐  ┌────▼───┐
    │ Orders │   │Products│  │Payments│   ← Per-service Databases
    │  DB    │   │   DB   │  │   DB   │
    └────────┘   └────────┘  └────────┘
          │            │           │
          └────────────┼───────────┘
                  ┌────▼─────┐
                  │ Message  │   ← Async Communication (RabbitMQ / Kafka)
                  │   Bus    │
                  └──────────┘
                       │
         ┌─────────────┴───────────────┐
   ┌─────▼──────┐              ┌───────▼──────┐
   │ Notification│             │  Audit / Log │   ← Event Consumers
   │  Service   │             │   Service    │
   └────────────┘              └─────────────┘
```

| Component | Role |
|-----------|------|
| **API Gateway** | Single entry point — routing, auth, rate limiting |
| **Services** | Independent business capabilities |
| **Service Registry** | Service discovery (Consul, K8s DNS) |
| **Message Bus** | Async communication (RabbitMQ, Kafka, Azure Service Bus) |
| **Configuration Server** | Centralised config (Azure App Config, Consul KV) |
| **Identity Provider** | Authentication/authorisation (IdentityServer, Azure AD B2C) |
| **Container Runtime** | Docker for packaging, Kubernetes for orchestration |
| **Observability** | Logs (Serilog), Metrics (Prometheus), Traces (Jaeger) |
| **CI/CD Pipeline** | Per-service build and deploy (GitHub Actions, Azure DevOps) |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the tools commonly used tools for Microservices?

| Category | Tool | Purpose |
|----------|------|---------|
| **Service framework** | ASP.NET Core Minimal API | Build HTTP microservices |
| **API Gateway** | YARP, Ocelot, Azure APIM | Routing, auth, rate limiting |
| **gRPC** | Grpc.AspNetCore | High-performance service-to-service |
| **Messaging** | MassTransit + RabbitMQ | Async pub/sub, saga orchestration |
| **Messaging (cloud)** | Azure Service Bus, Amazon SQS | Managed message queues |
| **Streaming** | Apache Kafka, Azure Event Hubs | High-throughput event streaming |
| **Containers** | Docker, containerd | Package and run services |
| **Orchestration** | Kubernetes, Azure AKS | Scale, deploy, manage containers |
| **Service mesh** | Istio, Linkerd | mTLS, traffic management, observability |
| **Service discovery** | K8s DNS, Consul | Locate services dynamically |
| **Configuration** | Azure App Config, Consul KV | Centralised config + feature flags |
| **Identity** | IdentityServer, Azure AD B2C | OAuth2/OIDC for authentication |
| **Resilience** | Polly / M.E.Http.Resilience | Retry, circuit breaker, timeout |
| **Tracing** | OpenTelemetry + Jaeger/Zipkin | Distributed request tracing |
| **Metrics** | Prometheus + Grafana | Dashboards and alerting |
| **Logging** | Serilog + ELK / Azure Monitor | Structured log aggregation |
| **CI/CD** | GitHub Actions, Azure DevOps | Automated build and deploy |
| **Secrets** | Azure Key Vault, HashiCorp Vault | Secrets management |
| **Health** | ASP.NET Core Health Checks | Liveness/readiness probes |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the key principles to follow when designing microservices?

```cs
// 1. Single Responsibility — one service, one bounded context
// OrderService handles order lifecycle only; Catalog handles product info

// 2. Database per service — no shared DB
// ❌ Shared DB creates coupling
// ✅ Each service owns its schema; communicate via events/API

// 3. Design for failure
builder.Services.AddHttpClient<ICatalogClient, CatalogClient>()
    .AddStandardResilienceHandler(); // retry + circuit breaker + timeout

// 4. Async-first communication
// Prefer events over synchronous calls for non-critical paths
await publishEndpoint.Publish(new OrderShipped(orderId, trackingNumber));

// 5. API versioning — never break consumers
var v1 = app.MapGroup("/api/v1");
var v2 = app.MapGroup("/api/v2");
v1.MapGet("/orders/{id}", GetOrderV1);
v2.MapGet("/orders/{id}", GetOrderV2); // new shape, v1 still works

// 6. Observability from day one
builder.Services.AddOpenTelemetry()
    .WithTracing(t => t.AddAspNetCoreInstrumentation().AddOtlpExporter())
    .WithMetrics(m => m.AddAspNetCoreInstrumentation().AddOtlpExporter());

builder.Host.UseSerilog((ctx, cfg) => cfg
    .Enrich.FromLogContext()
    .WriteTo.Console(new JsonFormatter()));

// 7. Idempotent consumers — safe to process same message twice
public class OrderCreatedConsumer : IConsumer<OrderCreated>
{
    public async Task Consume(ConsumeContext<OrderCreated> ctx)
    {
        // Check if already processed (idempotency key)
        if (await db.ProcessedEvents.AnyAsync(e => e.Id == ctx.MessageId))
            return; // duplicate — skip

        // ... process ...

        db.ProcessedEvents.Add(new ProcessedEvent(ctx.MessageId!.Value));
        await db.SaveChangesAsync();
    }
}

// 8. Health checks for Kubernetes
app.MapHealthChecks("/health/live",  new() { Predicate = _ => false }); // always alive
app.MapHealthChecks("/health/ready", new() { Predicate = _ => true  }); // checks deps

// 9. Use semantic versioning for Docker images + chart versions
// v1.2.3 — never use :latest in production

// 10. 12-Factor App principles
// Config from environment; logs to stdout; stateless processes
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 15. PERFORMANCE AND OPTIMIZATION

<br/>

## Q. How can you improve string concatenation performance in C#?

String concatenation with `+` inside loops creates a new heap allocation on every iteration. Use `StringBuilder`, interpolated string handlers, or `string.Create` depending on context.

```cs
using System.Text;

// ❌ O(n²) — each += allocates a new string
string result = "";
for (int i = 0; i < 100_000; i++)
    result += i.ToString(); // 100,000 heap allocations

// ✅ StringBuilder — single buffer, amortised O(1) append
var sb = new StringBuilder(capacity: 1_024_000); // pre-allocate if size is known
for (int i = 0; i < 100_000; i++)
    sb.Append(i);
string r1 = sb.ToString(); // single final allocation

// ✅ string.Concat / Join — best for fixed number of strings
string r2 = string.Concat("Hello", " ", "World");
string r3 = string.Join(", ", new[] { "Alice", "Bob", "Carol" });

// ✅ Interpolated strings — compiler-optimised in .NET 6+
// Uses DefaultInterpolatedStringHandler internally — no intermediate string
string name = "Alice"; int age = 30;
string r4 = $"{name} is {age}";

// ✅ string.Create — zero-copy, write directly into final buffer (.NET 6+)
int[] numbers = [1, 2, 3, 4, 5];
string r5 = string.Create(numbers.Length * 2 - 1, numbers, (span, nums) =>
{
    for (int i = 0; i < nums.Length; i++)
    {
        span[i * 2] = (char)('0' + nums[i]);
        if (i < nums.Length - 1) span[i * 2 + 1] = ',';
    }
});
Console.WriteLine(r5); // 1,2,3,4,5

// ✅ ValueStringBuilder / stackalloc for hot paths (advanced)
Span<char> buf = stackalloc char[256];
var vsb = new System.Text.StringBuilder(); // or use MemoryExtensions for Span-based ops

// Benchmark result (relative):
// string +=          : 10,000 ms (100k iterations)
// StringBuilder      :      3 ms
// string.Create      :      1 ms (zero intermediate allocations)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you work with parallelism and concurrency in C#?

**Concurrency** — multiple tasks making progress (may share a single thread via async).
**Parallelism** — multiple tasks executing simultaneously on multiple CPU cores.

```cs
// 1. async/await — concurrency for I/O-bound work (no extra threads)
async Task<string[]> FetchAllAsync(string[] urls)
{
    using var client = new HttpClient();
    var tasks = urls.Select(url => client.GetStringAsync(url));
    return await Task.WhenAll(tasks); // all in parallel, no thread blocking
}

// 2. Parallel.For / Parallel.ForEach — parallelism for CPU-bound work
int[] data = Enumerable.Range(1, 1_000_000).ToArray();
long sum = 0;
Parallel.For(0, data.Length,
    () => 0L,                                          // local state
    (i, _, local) => local + data[i],                 // body
    local => Interlocked.Add(ref sum, local));         // merge
Console.WriteLine(sum);

// 3. Parallel.ForEachAsync (.NET 6+) — async work with bounded parallelism
var urls = Enumerable.Range(1, 20).Select(i => $"https://api.example.com/item/{i}");
await Parallel.ForEachAsync(urls,
    new ParallelOptions { MaxDegreeOfParallelism = 4 },
    async (url, ct) =>
    {
        using var client = new HttpClient();
        var data = await client.GetStringAsync(url, ct);
        Console.WriteLine($"Got {data.Length} chars");
    });

// 4. PLINQ — parallel LINQ for data processing
var results = ParallelEnumerable.Range(1, 1_000_000)
    .AsParallel()
    .WithDegreeOfParallelism(Environment.ProcessorCount)
    .Where(n => n % 2 == 0)
    .Select(n => n * n)
    .Sum();

// 5. Channel<T> — producer/consumer pipelines (.NET Core 2.1+)
using System.Threading.Channels;

var channel = Channel.CreateBounded<int>(capacity: 100);

// Producer
var producer = Task.Run(async () =>
{
    for (int i = 0; i < 1000; i++)
    {
        await channel.Writer.WriteAsync(i);
    }
    channel.Writer.Complete();
});

// Consumer
var consumer = Task.Run(async () =>
{
    await foreach (int item in channel.Reader.ReadAllAsync())
        Console.WriteLine(item);
});

await Task.WhenAll(producer, consumer);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are some common performance issues in .NET Core applications?

| Issue | Symptom | Fix |
|-------|---------|-----|
| Excessive allocations | High GC pressure, frequent Gen 0 collections | `Span<T>`, `ArrayPool<T>`, object pooling |
| Sync-over-async | Thread starvation, deadlocks | Use `async`/`await` throughout |
| N+1 queries | Slow DB responses | EF Core `.Include()`, batch queries |
| Large object heap (LOH) | Heap fragmentation, long GC pauses | Avoid large arrays; use `ArrayPool` |
| String concatenation in loops | High memory, slow performance | `StringBuilder`, `string.Create` |
| Missing caching | Repeated expensive computations | `IMemoryCache`, `IDistributedCache` |
| Over-fetching data | Large payloads, slow queries | Projection (`Select`), pagination |
| Boxing value types | Hidden allocations | Use generics, avoid `object` parameters |
| Blocking async code | Deadlocks (`.Result`, `.Wait()`) | `await` everywhere, `ConfigureAwait(false)` |
| Missing `AsNoTracking` | Unnecessary EF change tracking | Add `.AsNoTracking()` for read-only queries |

```cs
// ❌ Common anti-pattern: sync-over-async
public string GetData() => FetchAsync().Result; // blocks thread pool thread

// ✅ Fix: async all the way
public async Task<string> GetDataAsync() => await FetchAsync();

// ❌ Anti-pattern: unnecessary ToList() materialisation
var count = dbContext.Orders.ToList().Count; // loads ALL rows into memory

// ✅ Fix: query at DB level
var count = await dbContext.Orders.CountAsync();

// ❌ Anti-pattern: missing ConfigureAwait in libraries
await SomeLibraryMethodAsync(); // may deadlock in ASP.NET Framework context

// ✅ Fix in library code
await SomeLibraryMethodAsync().ConfigureAwait(false);

// Detect issues via dotnet-counters
// dotnet-counters monitor --process-id <PID> System.Runtime
// Watch: gc-heap-size, gen-0-gc-count, threadpool-queue-length, alloc-rate
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you measure the performance of a .NET Core application?

```cs
// 1. Stopwatch — quick timing
using System.Diagnostics;

var sw = Stopwatch.StartNew();
PerformWork();
sw.Stop();
Console.WriteLine($"Elapsed: {sw.Elapsed.TotalMilliseconds:F3} ms");

// 2. BenchmarkDotNet — production-grade micro-benchmarks (gold standard)
// dotnet add package BenchmarkDotNet

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

[MemoryDiagnoser]
[SimpleJob(iterationCount: 10)]
public class StringBenchmarks
{
    private const int N = 10_000;

    [Benchmark(Baseline = true)]
    public string Concatenation()
    {
        string s = "";
        for (int i = 0; i < N; i++) s += i;
        return s;
    }

    [Benchmark]
    public string StringBuilderBenchmark()
    {
        var sb = new System.Text.StringBuilder(N * 5);
        for (int i = 0; i < N; i++) sb.Append(i);
        return sb.ToString();
    }
}

BenchmarkRunner.Run<StringBenchmarks>();
// Reports: Mean, Allocated, Gen0/1/2 GC counts

// 3. Activity / OpenTelemetry — distributed tracing
using System.Diagnostics;

var source = new ActivitySource("MyApp.Performance");
using var activity = source.StartActivity("ProcessOrder");
activity?.SetTag("order.id", 42);
// ... work ...
activity?.Stop();

// 4. EventCounters — production runtime metrics
// dotnet-counters monitor -p <PID> System.Runtime Microsoft.AspNetCore.Hosting
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What tools are available for profiling .NET Core applications?

| Tool | Type | Use case |
|------|------|---------|
| **dotnet-counters** | CLI / live | Real-time CPU, GC, allocation rates |
| **dotnet-trace** | CLI / file | CPU sampling, GC events, custom events |
| **dotnet-dump** | CLI / post-mortem | Memory dumps, OOM analysis |
| **dotnet-gcdump** | CLI / heap | GC heap snapshot, object retention |
| **Visual Studio Profiler** | GUI | CPU sampling, memory snapshots, timeline |
| **PerfView** | GUI / ETW | Deep GC, JIT, thread pool analysis |
| **JetBrains dotMemory** | GUI | Memory profiling, heap diff |
| **JetBrains dotTrace** | GUI | CPU profiling, async call trees |
| **BenchmarkDotNet** | Code | Micro-benchmarks with statistics |
| **OpenTelemetry** | Code / infra | Distributed tracing, metrics, logs |

```bash
# Install global tools
dotnet tool install -g dotnet-counters
dotnet tool install -g dotnet-trace
dotnet tool install -g dotnet-dump
dotnet tool install -g dotnet-gcdump

# Live metrics
dotnet-counters monitor -p <PID> System.Runtime

# CPU trace (30 seconds)
dotnet-trace collect -p <PID> --duration 00:00:30 --output trace.nettrace

# Open in Visual Studio or PerfView:
# perfview /GCCollectOnly trace.nettrace

# Heap dump
dotnet-gcdump collect -p <PID> --output heap.gcdump
# Open in Visual Studio: File → Open → heap.gcdump

# Full memory dump
dotnet-dump collect -p <PID>
dotnet-dump analyze core_<PID>
# Then: dumpheap -stat, gcroot <address>, etc.
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use the `dotnet-counters` tool to monitor performance?

`dotnet-counters` streams real-time .NET runtime counters (GC, thread pool, exception rates, allocation rates) to the console without restarting the process.

```bash
# Install
dotnet tool install -g dotnet-counters

# List available counter providers
dotnet-counters list

# Monitor default System.Runtime counters for a process
dotnet-counters monitor --process-id 1234

# Monitor multiple providers
dotnet-counters monitor -p 1234 \
  System.Runtime \
  Microsoft.AspNetCore.Hosting \
  Microsoft.AspNetCore.Http.Connections

# Monitor by process name
dotnet-counters monitor --name MyApi

# Export to CSV for analysis
dotnet-counters collect -p 1234 \
  --output counters.csv \
  --duration 00:02:00 \
  --format csv

# Key counters to watch:
# cpu-usage                  — CPU %
# gc-heap-size               — managed heap size (MB)
# gen-0-gc-count             — Gen 0 GC frequency
# alloc-rate                 — allocation rate (bytes/sec)
# threadpool-queue-length    — pending thread pool work items
# active-timer-count         — active timers
# exception-count            — unhandled exceptions/sec
# requests-per-second        — ASP.NET Core RPS
# current-requests           — in-flight HTTP requests
```

```cs
// Emit custom EventCounters from your app
using System.Diagnostics.Tracing;

[EventSource(Name = "MyApp")]
public sealed class MyEventSource : EventSource
{
    public static readonly MyEventSource Log = new();
    private EventCounter? _requestDuration;

    public MyEventSource()
    {
        _requestDuration = new EventCounter("request-duration-ms", this)
        {
            DisplayName = "Request Duration (ms)"
        };
    }

    public void RecordRequest(double ms) => _requestDuration?.WriteMetric(ms);
}

// Usage
MyEventSource.Log.RecordRequest(42.5);

// Monitor custom counters
// dotnet-counters monitor -p <PID> MyApp
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `dotnet-trace` tool and how is it used?

`dotnet-trace` captures ETW/EventPipe events from a running .NET process into a `.nettrace` file for offline analysis of CPU, GC, JIT, and custom events.

```bash
# Install
dotnet tool install -g dotnet-trace

# List available event profiles
dotnet-trace list-profiles
# cpu-sampling   — CPU call stacks (default)
# gc-verbose     — detailed GC events
# gc-collect     — GC collection only
# none           — no built-in; specify providers manually

# Capture 30-second CPU profile
dotnet-trace collect -p <PID> \
  --profile cpu-sampling \
  --duration 00:00:30 \
  --output myapp.nettrace

# GC-focused trace
dotnet-trace collect -p <PID> \
  --profile gc-verbose \
  --output gc.nettrace

# Custom providers (e.g., ASP.NET Core + GC)
dotnet-trace collect -p <PID> \
  --providers "Microsoft.AspNetCore:4:5,System.Runtime:4:4" \
  --output custom.nettrace

# Trace from startup (catches cold-start JIT)
dotnet-trace collect -- dotnet MyApp.dll

# Analyse the trace
# Open in: Visual Studio → File → Open → myapp.nettrace
# Or: PerfView myapp.nettrace
# Or: speedscope.app (convert first)
dotnet-trace convert myapp.nettrace --format Speedscope
# Open myapp.speedscope.json at https://speedscope.app
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use the `dotnet-dump` tool to collect and analyze memory dumps?

`dotnet-dump` captures a full managed memory dump and provides a REPL for analysis — useful for diagnosing OOM crashes, deadlocks, and high memory usage.

```bash
# Install
dotnet tool install -g dotnet-dump

# Capture dump from running process
dotnet-dump collect -p <PID> --output myapp.dmp

# Capture on crash (set environment variable before starting)
export DOTNET_DbgEnableMiniDump=1
export DOTNET_DbgMiniDumpType=4       # 1=Mini, 2=Heap, 3=Triage, 4=Full
export DOTNET_DbgMiniDumpName=/tmp/crash_%p.dmp

# Analyze interactively
dotnet-dump analyze myapp.dmp
```

```
# Inside the analyze REPL — useful commands:

# Show all managed threads
clrthreads

# Show call stacks for all threads
clrstack -all

# Heap statistics — top object types by count/size
dumpheap -stat

# Find all instances of a type
dumpheap -type System.String -stat

# Show object at address
dumpobj 0x00007f8b1c002a18

# Find what holds a reference to an object (root analysis)
gcroot 0x00007f8b1c002a18

# Show finalizer queue
finalizequeue

# Show thread pool state
threadpool

# Show sync blocks (locks)
syncblk

# Exit
exit
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `dotnet-gcdump` tool and how is it used?

`dotnet-gcdump` captures a GC heap snapshot in `.gcdump` format — a lighter alternative to a full memory dump. It triggers a GC collection, then records all live objects and their references.

```bash
# Install
dotnet tool install -g dotnet-gcdump

# Capture heap dump from running process
dotnet-gcdump collect -p <PID> --output myapp.gcdump

# Capture from process by name
dotnet-gcdump collect --name MyApi

# Analyse in VS Code / Visual Studio
# Visual Studio: File → Open → myapp.gcdump
# Shows: object counts, sizes, retention trees

# Report from command line (top types by size)
dotnet-gcdump report myapp.gcdump

# Compare two snapshots (detect leaks)
dotnet-gcdump report baseline.gcdump after.gcdump --diff
```

```cs
// Programmatic heap size info (no external tool needed)
GCMemoryInfo info = GC.GetGCMemoryInfo();
Console.WriteLine($"Heap size:       {info.HeapSizeBytes / 1_048_576:F1} MB");
Console.WriteLine($"Committed:       {info.TotalCommittedBytes / 1_048_576:F1} MB");
Console.WriteLine($"Fragmented:      {info.FragmentedBytes / 1_024:F0} KB");
Console.WriteLine($"Gen0 size after: {info.GenerationInfo[0].SizeAfterBytes:N0} bytes");
Console.WriteLine($"Gen2 size after: {info.GenerationInfo[2].SizeAfterBytes:N0} bytes");

// Force GC and collect snapshot programmatically (for testing only)
GC.Collect(2, GCCollectionMode.Forced, blocking: true, compacting: true);
long heapBytes = GC.GetTotalMemory(forceFullCollection: false);
Console.WriteLine($"Heap after GC: {heapBytes / 1_048_576:F2} MB");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you optimize memory usage in .NET Core applications?

```cs
// 1. Use Span<T> / Memory<T> to avoid allocations
byte[] buffer = new byte[4096];
ReadOnlySpan<byte> slice = buffer.AsSpan(0, 100); // zero allocation

// 2. ArrayPool<T> — reuse buffers instead of allocating
using System.Buffers;

byte[] rented = ArrayPool<byte>.Shared.Rent(4096);
try   { /* use rented */ }
finally { ArrayPool<byte>.Shared.Return(rented, clearArray: false); }

// 3. ObjectPool<T> — reuse expensive objects
using Microsoft.Extensions.ObjectPool;

var policy = new DefaultPooledObjectPolicy<StringBuilder>();
var pool   = new DefaultObjectPool<StringBuilder>(policy, maximumRetained: 10);
var sb     = pool.Get();
try   { sb.Append("work"); var result = sb.ToString(); }
finally { pool.Return(sb); } // sb.Clear() called automatically

// 4. Use struct instead of class for small, short-lived value containers
// (avoids heap allocation when stored as local / struct field)
public readonly record struct Point(double X, double Y);

// 5. Avoid unnecessary LINQ materialisation
// ❌ Loads everything into memory
var names = db.Products.ToList().Select(p => p.Name).ToList();
// ✅ Projection at DB level
var names2 = await db.Products.Select(p => p.Name).ToListAsync();

// 6. String interning for frequently repeated strings
string s = string.Intern(someString);

// 7. Weak references — allow GC to collect when under pressure
var weak = new WeakReference<ExpensiveObject>(new ExpensiveObject());
if (weak.TryGetTarget(out var obj))
    obj.Use();
// else obj was collected — recreate if needed

// 8. Dispose IDisposable resources promptly
await using var stream = new FileStream("file.txt", FileMode.Open);
// stream disposed at end of using block
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are some best practices for managing memory in .NET Core?

```cs
// 1. Implement IDisposable / IAsyncDisposable for unmanaged resources
public sealed class ResourceHolder : IAsyncDisposable
{
    private readonly FileStream _stream = File.Open("data.bin", FileMode.OpenOrCreate);
    private bool _disposed;

    public async ValueTask DisposeAsync()
    {
        if (_disposed) return;
        await _stream.DisposeAsync();
        _disposed = true;
    }
}

// 2. Avoid static collections that grow without bound (memory leaks)
// ❌ Unbounded static cache — never cleaned up
private static readonly Dictionary<int, byte[]> _cache = new();

// ✅ Use IMemoryCache with expiry
builder.Services.AddMemoryCache();
// cache.Set(key, value, TimeSpan.FromMinutes(5));

// 3. Prefer value types for hot-path data
record struct Coordinate(double Lat, double Lon); // stack-allocated when local

// 4. GC tuning for server workloads
// In runtimeconfig.json or environment variables:
// DOTNET_GCConserveMemory=5      (0-9, higher = more aggressive GC)
// DOTNET_GCHeapHardLimit=1073741824  (1 GB hard cap)
// DOTNET_gcServer=1              (server GC — better throughput)

// 5. Use cancellation tokens to abort long operations
async Task<string> FetchWithTimeoutAsync(string url, CancellationToken ct)
{
    using var client = new HttpClient();
    using var cts = CancellationTokenSource.CreateLinkedTokenSource(ct);
    cts.CancelAfter(TimeSpan.FromSeconds(10));
    return await client.GetStringAsync(url, cts.Token);
}

// 6. Avoid finalizers — use IDisposable pattern instead
// Finalizers delay GC (object survives to Gen 1 before collection)

// 7. Pre-size collections when count is known
var list = new List<int>(capacity: 10_000);   // avoids re-allocations
var dict = new Dictionary<string, int>(capacity: 500);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you reduce the memory footprint of a .NET Core application?

```bash
# 1. Publish as trimmed self-contained (remove unused framework code)
dotnet publish -c Release -r linux-x64 --self-contained \
  -p:PublishTrimmed=true \
  -p:TrimMode=full \
  -o ./publish
# Reduces from ~80 MB to ~15-20 MB for a minimal API

# 2. Native AOT — no JIT compiler or reflection metadata in memory
dotnet publish -c Release -r linux-x64 -p:PublishAot=true

# 3. Use minimal framework components
```

```xml
<!-- .csproj — exclude unused features -->
<PropertyGroup>
  <InvariantGlobalization>true</InvariantGlobalization>  <!-- save ~10 MB -->
  <MetadataUpdaterSupport>false</MetadataUpdaterSupport>
  <UseSystemResourceKeys>true</UseSystemResourceKeys>
</PropertyGroup>
```

```cs
// 4. Server GC with memory limit
// DOTNET_GCHeapHardLimitPercent=75   — use max 75% of container memory
// DOTNET_gcServer=1                  — server GC (one heap per CPU core)
// DOTNET_GCConserveMemory=5          — favour smaller heap over throughput

// 5. Reduce per-request allocations
// Use IObjectPool, Span<T>, ArrayPool<T> (covered above)

// 6. Disable features not needed
var builder = WebApplication.CreateSlimBuilder(args); // .NET 8+ minimal builder
// CreateSlimBuilder omits: Kestrel auto-config, startup filters, hosting startup
// Result: faster startup + lower baseline memory

// 7. Monitor allocation rate with dotnet-counters
// dotnet-counters monitor -p <PID> System.Runtime
// Target: alloc-rate < 1 MB/s for steady-state server
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you optimize CPU usage in .NET Core applications?

```cs
// 1. Offload CPU-bound work to thread pool with Task.Run
app.MapGet("/compute", async () =>
{
    var result = await Task.Run(() => ExpensiveCpuWork());
    return result;
});

// 2. PLINQ for data-parallel CPU work
var results = data.AsParallel()
    .WithDegreeOfParallelism(Environment.ProcessorCount)
    .Select(item => Transform(item))
    .ToList();

// 3. Aggressive JIT inlining for hot paths
[System.Runtime.CompilerServices.MethodImpl(
    System.Runtime.CompilerServices.MethodImplOptions.AggressiveInlining)]
static int FastMath(int x) => x * x + 2 * x + 1;

// 4. SIMD / hardware intrinsics (.NET 10)
using System.Runtime.Intrinsics;
using System.Runtime.Intrinsics.X86;

static float SumSimd(float[] values)
{
    if (Avx.IsSupported && values.Length >= 8)
    {
        var sum = Vector256<float>.Zero;
        int i = 0;
        for (; i <= values.Length - 8; i += 8)
            sum = Avx.Add(sum, Vector256.LoadUnsafe(ref values[i]));
        // handle remainder
        float total = 0;
        for (int j = 0; j < Vector256<float>.Count; j++) total += sum[j];
        for (; i < values.Length; i++) total += values[i];
        return total;
    }
    return values.Sum();
}

// 5. Use Vector<T> / generic SIMD (works on all platforms)
using System.Numerics;

static long SumVector(int[] data)
{
    var vSum = Vector<long>.Zero;
    int vLen = Vector<int>.Count;
    int i = 0;
    for (; i <= data.Length - vLen; i += vLen)
    {
        var v = new Vector<int>(data, i);
        Vector.Widen(v, out var lo, out var hi);
        vSum += Vector.ConvertToInt64(lo) + Vector.ConvertToInt64(hi);
    }
    long sum = 0;
    for (int j = 0; j < Vector<long>.Count; j++) sum += vSum[j];
    for (; i < data.Length; i++) sum += data[i];
    return sum;
}

// 6. Avoid unnecessary thread context switches
// Use SemaphoreSlim instead of Monitor for async-compatible locking
var semaphore = new SemaphoreSlim(1, 1);
await semaphore.WaitAsync();
try { /* protected work */ }
finally { semaphore.Release(); }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are some best practices for optimizing CPU-bound operations?

```cs
// 1. Profile before optimising — measure first with BenchmarkDotNet or dotnet-trace
// 2. Use correct data structures — O(1) HashSet lookup vs O(n) List.Contains
var set = new HashSet<int>(data);
bool found = set.Contains(target); // O(1)

// 3. Avoid LINQ in the tightest loops — use plain for loops
// ❌ LINQ in hot loop — delegate invocation overhead
for (int i = 0; i < 1_000_000; i++)
    total += data.Where(x => x > 0).Sum(); // re-evaluates every iteration

// ✅ Pre-filter, use for loop in hot path
var positive = data.Where(x => x > 0).ToArray();
for (int i = 0; i < positive.Length; i++) total += positive[i];

// 4. Prefer stackalloc for small temporary buffers
Span<int> buf = stackalloc int[32]; // no heap allocation

// 5. Use ReadOnlySpan<char> for string parsing (no substrings)
ReadOnlySpan<char> input = "2026-04-19".AsSpan();
int year  = int.Parse(input[..4]);
int month = int.Parse(input[5..7]);
int day   = int.Parse(input[8..]);

// 6. Frozen collections for read-only lookup tables (.NET 8+)
using System.Collections.Frozen;
FrozenDictionary<string, int> lookup =
    new Dictionary<string, int> { ["a"] = 1, ["b"] = 2 }.ToFrozenDictionary();
// Lookup is ~30% faster than Dictionary<K,V> for read-only scenarios

// 7. Use object pooling for expensive-to-create objects
// (see ObjectPool<T> example above)

// 8. Prefer ValueTask over Task for frequently-completing async paths
public ValueTask<int> GetCachedValueAsync(int key)
{
    if (_cache.TryGetValue(key, out int v))
        return ValueTask.FromResult(v); // no Task allocation
    return new ValueTask<int>(FetchFromDbAsync(key));
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use asynchronous programming to improve performance in .NET Core?

```cs
// 1. async/await — releases thread during I/O wait
app.MapGet("/data", async (HttpClient client) =>
{
    // Thread returned to pool while waiting — handles more concurrent requests
    string data = await client.GetStringAsync("https://api.example.com/data");
    return data;
});

// 2. Parallel async I/O with Task.WhenAll
async Task<(string, string)> FetchTwoAsync()
{
    var t1 = httpClient.GetStringAsync("https://api1.example.com");
    var t2 = httpClient.GetStringAsync("https://api2.example.com");
    var results = await Task.WhenAll(t1, t2);   // both run concurrently
    return (results[0], results[1]);
}

// 3. Streaming with IAsyncEnumerable (avoids loading all data at once)
async IAsyncEnumerable<Product> StreamProductsAsync(
    [EnumeratorCancellation] CancellationToken ct = default)
{
    await foreach (var product in dbContext.Products.AsAsyncEnumerable().WithCancellation(ct))
        yield return product;
}

app.MapGet("/products/stream", (AppDbContext db) =>
    db.Products.AsAsyncEnumerable()); // ASP.NET Core streams the JSON array

// 4. ValueTask for cached/synchronous fast paths (avoid Task allocation)
private int _cached;
public ValueTask<int> GetValueAsync()
{
    if (_cached != 0) return ValueTask.FromResult(_cached); // no allocation
    return new ValueTask<int>(LoadFromDbAsync());
}

// 5. ConfigureAwait(false) in library code (avoid context capture overhead)
public async Task<string> LibraryMethodAsync()
{
    var data = await FetchAsync().ConfigureAwait(false);
    return Process(data);
}

// 6. CancellationToken — abort abandoned requests
app.MapGet("/slow", async (CancellationToken ct) =>
{
    await Task.Delay(5000, ct); // if client disconnects, ct is cancelled
    return "done";
});
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `async` and `await` keywords and how are they used?

`async` marks a method as asynchronous. `await` suspends execution of the method until the awaited task completes, **without blocking the thread**.

```cs
// Basic pattern
public async Task<string> FetchDataAsync(string url)
{
    using var client = new HttpClient();
    string data = await client.GetStringAsync(url); // thread released here
    return data.ToUpper(); // resumes here when response arrives
}

// Return types
async Task DoWorkAsync() { /* no return value */ }
async Task<int> GetCountAsync() { return 42; }
async ValueTask<int> GetCachedAsync() { return _cached; } // avoids Task alloc
async IAsyncEnumerable<int> GenerateAsync()
{
    for (int i = 0; i < 10; i++) { await Task.Delay(10); yield return i; }
}

// Awaiting multiple tasks
var t1 = FetchDataAsync("https://api1.example.com");
var t2 = FetchDataAsync("https://api2.example.com");
string[] results = await Task.WhenAll(t1, t2);   // parallel

// WhenAny — first to complete wins
var fastest = await Task.WhenAny(t1, t2);
Console.WriteLine(await fastest);

// Exception handling
try
{
    await RiskyOperationAsync();
}
catch (HttpRequestException ex)
{
    Console.WriteLine($"HTTP error: {ex.StatusCode}");
}

// Async in a console app (.NET 10 supports top-level await)
await foreach (int n in GenerateAsync())
    Console.Write($"{n} ");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use the `Task` class to perform asynchronous operations?

```cs
// 1. Task.Run — offload CPU-bound work to thread pool
var result = await Task.Run(() =>
{
    return Enumerable.Range(1, 1_000_000).Sum(x => (long)x);
});
Console.WriteLine(result); // 500000500000

// 2. Task.Delay — non-blocking pause (replaces Thread.Sleep)
await Task.Delay(TimeSpan.FromSeconds(1));

// 3. Task.WhenAll — wait for all tasks (parallel)
var tasks = urls.Select(url => httpClient.GetStringAsync(url));
string[] responses = await Task.WhenAll(tasks);

// 4. Task.WhenAny — first task to complete
var timeout = Task.Delay(5000);
var work    = DoWorkAsync();
if (await Task.WhenAny(work, timeout) == timeout)
    throw new TimeoutException();

// 5. Task<T> — task with return value
Task<int> countTask = CountItemsAsync();
int count = await countTask;

// 6. TaskCompletionSource — wrap callback-based APIs
Task<string> WrapCallback()
{
    var tcs = new TaskCompletionSource<string>();
    LegacyLibrary.DoWork(
        onSuccess: result => tcs.SetResult(result),
        onError:   ex     => tcs.SetException(ex));
    return tcs.Task;
}

// 7. Task.FromResult / Task.CompletedTask — completed tasks (no allocation for common values)
Task<int> zero = Task.FromResult(0);
Task done      = Task.CompletedTask;

// 8. Chaining with ContinueWith (prefer await over this)
Task<string> chain = Task.Run(() => 42)
    .ContinueWith(t => $"Result: {t.Result}");
Console.WriteLine(await chain);

// 9. Task cancellation
using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(5));
try
{
    await LongRunningAsync(cts.Token);
}
catch (OperationCanceledException)
{
    Console.WriteLine("Cancelled after 5 seconds");
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you optimize I/O operations in .NET Core applications?

```cs
// 1. Always use async I/O — never block on file/network/DB
// ❌ Blocks a thread pool thread
string text = File.ReadAllText("file.txt");

// ✅ Async — thread returns to pool during I/O wait
string text2 = await File.ReadAllTextAsync("file.txt");

// 2. Buffer reads/writes — reduce syscall count
await using var reader = new StreamReader(
    new FileStream("large.csv", FileMode.Open, FileAccess.Read,
        FileShare.Read, bufferSize: 65536, useAsync: true));
string? line;
while ((line = await reader.ReadLineAsync()) is not null)
    Process(line);

// 3. Pipelines API — zero-copy parsing (System.IO.Pipelines)
using System.IO.Pipelines;

async Task ProcessPipeAsync(PipeReader reader, CancellationToken ct)
{
    while (true)
    {
        ReadResult result = await reader.ReadAsync(ct);
        ReadOnlySequence<byte> buffer = result.Buffer;
        // Process buffer without copying
        reader.AdvanceTo(buffer.End);
        if (result.IsCompleted) break;
    }
}

// 4. HttpClient — reuse, configure timeouts, use IHttpClientFactory
builder.Services.AddHttpClient("api", client =>
{
    client.BaseAddress = new Uri("https://api.example.com");
    client.Timeout = TimeSpan.FromSeconds(10);
    client.DefaultRequestHeaders.Add("Accept", "application/json");
});

// 5. Parallel downloads with bounded parallelism
var semaphore = new SemaphoreSlim(10); // max 10 concurrent downloads
var tasks = urls.Select(async url =>
{
    await semaphore.WaitAsync();
    try   { return await client.GetStringAsync(url); }
    finally { semaphore.Release(); }
});
string[] results = await Task.WhenAll(tasks);

// 6. SocketsHttpHandler — fine-grained connection pool tuning
var handler = new SocketsHttpHandler
{
    PooledConnectionLifetime     = TimeSpan.FromMinutes(2),
    PooledConnectionIdleTimeout  = TimeSpan.FromMinutes(1),
    MaxConnectionsPerServer      = 20,
    EnableMultipleHttp2Connections = true,
};
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are some best practices for optimizing disk and network I/O?

```cs
// DISK I/O

// 1. Async file operations
await File.WriteAllTextAsync("output.txt", content);
await File.AppendAllTextAsync("log.txt", $"{DateTime.UtcNow}: event\n");

// 2. File.OpenHandle + RandomAccess (.NET 6+) — best performance, no Stream overhead
using var handle = File.OpenHandle("data.bin",
    FileMode.Open, FileAccess.Read, FileShare.Read,
    FileOptions.Asynchronous | FileOptions.SequentialScan);
var buffer = new byte[4096];
int read = await RandomAccess.ReadAsync(handle, buffer, fileOffset: 0);

// 3. MemoryMappedFile — for very large files or shared memory
using var mmf = MemoryMappedFile.CreateFromFile("huge.dat");
using var accessor = mmf.CreateViewAccessor(offset: 0, size: 4096);
byte value = accessor.ReadByte(0);

// NETWORK I/O

// 4. Reuse HttpClient via IHttpClientFactory (never new HttpClient() per request)
public class MyService(IHttpClientFactory factory)
{
    public async Task<string> GetAsync(string url)
    {
        using var client = factory.CreateClient("api");
        return await client.GetStringAsync(url);
    }
}

// 5. HTTP/2 and HTTP/3 reduce connection overhead
builder.WebHost.ConfigureKestrel(opts =>
    opts.ListenLocalhost(8080, o =>
        o.Protocols = Microsoft.AspNetCore.Server.Kestrel.Core
            .HttpProtocols.Http1AndHttp2AndHttp3));

// 6. Response compression — reduce bytes over the wire
builder.Services.AddResponseCompression(opts =>
{
    opts.EnableForHttps = true;
    opts.Providers.Add<BrotliCompressionProvider>();
    opts.Providers.Add<GzipCompressionProvider>();
});
app.UseResponseCompression();

// 7. Output caching — serve cached responses without hitting the app
builder.Services.AddOutputCache();
app.UseOutputCache();
app.MapGet("/products", GetProducts).CacheOutput(p => p.Expire(TimeSpan.FromMinutes(5)));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use caching to improve performance in .NET Core applications?

```cs
// 1. IMemoryCache — in-process, fast
builder.Services.AddMemoryCache();

public class ProductService(IMemoryCache cache, IProductRepository repo)
{
    public async Task<Product?> GetByIdAsync(int id)
    {
        return await cache.GetOrCreateAsync($"product:{id}", async entry =>
        {
            entry.AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(5);
            entry.SlidingExpiration               = TimeSpan.FromMinutes(1);
            entry.Priority                        = CacheItemPriority.Normal;
            return await repo.GetByIdAsync(id);
        });
    }
}

// 2. Output caching (.NET 7+) — cache full HTTP responses
builder.Services.AddOutputCache(options =>
{
    options.AddPolicy("ProductsCache", b => b
        .Expire(TimeSpan.FromMinutes(10))
        .SetVaryByQuery("category", "page")
        .Tag("products"));
});
app.UseOutputCache();

app.MapGet("/products", async (AppDbContext db) =>
    await db.Products.ToListAsync())
  .CacheOutput("ProductsCache");

// Evict by tag when data changes
app.MapPost("/products", async (Product p, IOutputCacheStore store, CancellationToken ct) =>
{
    // ... save product ...
    await store.EvictByTagAsync("products", ct); // invalidate cached /products responses
    return Results.Created($"/products/{p.Id}", p);
});
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `IMemoryCache` interface and how is it used?

`IMemoryCache` is the ASP.NET Core in-process cache abstraction. It stores key-value pairs in the process\'s memory with expiry policies.

```cs
// Register
builder.Services.AddMemoryCache(options =>
{
    options.SizeLimit = 1024;               // max 1024 size units
    options.CompactionPercentage = 0.25;    // remove 25% when full
});

// Inject and use
public class WeatherService(IMemoryCache cache)
{
    private static readonly string CacheKey = "weather:london";

    // Pattern 1: GetOrCreate (most common)
    public async Task<WeatherData> GetWeatherAsync()
    {
        return await cache.GetOrCreateAsync(CacheKey, async entry =>
        {
            entry.AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(10);
            entry.SlidingExpiration               = TimeSpan.FromMinutes(3);
            entry.Size                            = 1;  // counts toward SizeLimit
            entry.RegisterPostEvictionCallback((key, value, reason, state) =>
                Console.WriteLine($"Cache evicted: {key}, reason: {reason}"));

            return await FetchFromApiAsync();
        }) ?? throw new InvalidOperationException();
    }

    // Pattern 2: TryGetValue + Set
    public WeatherData? GetCachedWeather()
    {
        if (cache.TryGetValue(CacheKey, out WeatherData? data))
            return data;
        return null;
    }

    // Pattern 3: explicit Set
    public void SetWeather(WeatherData data)
    {
        var options = new MemoryCacheEntryOptions
        {
            AbsoluteExpiration = DateTimeOffset.UtcNow.AddHours(1),
            Priority = CacheItemPriority.High,
        };
        cache.Set(CacheKey, data, options);
    }

    // Invalidate
    public void InvalidateWeather() => cache.Remove(CacheKey);
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use distributed caching in .NET Core?

Distributed caching stores data outside the process — shared across multiple app instances. ASP.NET Core provides `IDistributedCache` backed by Redis, SQL Server, or NCache.

```cs
// 1. Redis distributed cache (recommended for production)
// dotnet add package Microsoft.Extensions.Caching.StackExchangeRedis
builder.Services.AddStackExchangeRedisCache(options =>
{
    options.Configuration = builder.Configuration["Redis:ConnectionString"];
    options.InstanceName  = "MyApp:";
});

// 2. SQL Server distributed cache
// dotnet add package Microsoft.Extensions.Caching.SqlServer
builder.Services.AddDistributedSqlServerCache(options =>
{
    options.ConnectionString = builder.Configuration.GetConnectionString("Default");
    options.SchemaName = "dbo";
    options.TableName  = "Cache";
});

// 3. In-memory (development / single-instance only)
builder.Services.AddDistributedMemoryCache();

// Usage via IDistributedCache
public class SessionService(IDistributedCache cache)
{
    public async Task SetUserDataAsync(string userId, UserData data, CancellationToken ct)
    {
        var json  = JsonSerializer.SerializeToUtf8Bytes(data);
        var opts  = new DistributedCacheEntryOptions
        {
            AbsoluteExpirationRelativeToNow = TimeSpan.FromHours(1),
            SlidingExpiration               = TimeSpan.FromMinutes(20),
        };
        await cache.SetAsync($"user:{userId}", json, opts, ct);
    }

    public async Task<UserData?> GetUserDataAsync(string userId, CancellationToken ct)
    {
        byte[]? bytes = await cache.GetAsync($"user:{userId}", ct);
        return bytes is null ? null : JsonSerializer.Deserialize<UserData>(bytes);
    }

    public async Task RemoveAsync(string userId, CancellationToken ct) =>
        await cache.RemoveAsync($"user:{userId}", ct);
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `IDistributedCache` interface and how is it used?

`IDistributedCache` is the abstraction for distributed caching in ASP.NET Core. It stores `byte[]` values by string key with expiry support.

```cs
// Core methods:
// Get / GetAsync           — retrieve bytes (null if not found)
// Set / SetAsync           — store bytes with options
// Refresh / RefreshAsync   — reset sliding expiration
// Remove / RemoveAsync     — delete entry

public class ProductCacheService(IDistributedCache cache)
{
    private static string Key(int id) => $"product:{id}";

    public async Task<Product?> GetAsync(int id, CancellationToken ct = default)
    {
        byte[]? bytes = await cache.GetAsync(Key(id), ct);
        return bytes is null
            ? null
            : JsonSerializer.Deserialize<Product>(bytes);
    }

    public async Task SetAsync(Product product, CancellationToken ct = default)
    {
        byte[] bytes = JsonSerializer.SerializeToUtf8Bytes(product);
        await cache.SetAsync(Key(product.Id), bytes,
            new DistributedCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(30),
                SlidingExpiration               = TimeSpan.FromMinutes(5),
            }, ct);
    }

    // Get-or-create pattern
    public async Task<Product> GetOrCreateAsync(int id,
        Func<CancellationToken, Task<Product>> factory, CancellationToken ct = default)
    {
        var cached = await GetAsync(id, ct);
        if (cached is not null) return cached;

        var product = await factory(ct);
        await SetAsync(product, ct);
        return product;
    }
}

// Extension method (HybridCache — .NET 9+, recommended)
// dotnet add package Microsoft.Extensions.Caching.Hybrid
builder.Services.AddHybridCache();

// HybridCache combines IMemoryCache (L1) + IDistributedCache (L2) automatically
public class CachedService(HybridCache hybridCache)
{
    public async Task<Product?> GetProductAsync(int id, CancellationToken ct)
        => await hybridCache.GetOrCreateAsync(
            $"product:{id}",
            async token => await FetchFromDbAsync(id, token),
            cancellationToken: ct);
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use the `ResponseCaching` middleware to cache responses?

**Response caching** stores complete HTTP responses and serves them for subsequent identical requests — reduces server-side processing.

```cs
// ResponseCaching — HTTP cache headers based (RFC 7234)
builder.Services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 64 * 1024; // max cached body = 64 KB
    options.UseCaseSensitivePaths = false;
});

app.UseResponseCaching();

// Controller — add cache headers
[ResponseCache(Duration = 60, VaryByQueryKeys = ["category"])]
[HttpGet]
public IActionResult GetProducts(string? category) => Ok();

// Minimal API
app.MapGet("/products", async (AppDbContext db) =>
    await db.Products.ToListAsync())
  .WithMetadata(new ResponseCacheAttribute
  {
      Duration = 300,         // cache for 5 minutes
      Location = ResponseCacheLocation.Any,
      VaryByHeader = "Accept-Language",
  });

// ⚠️ ResponseCaching has limitations:
// - Only caches GET/HEAD responses with 200 status
// - Does NOT work with authenticated requests by default
// - Cannot be invalidated programmatically

// ✅ Output Caching (.NET 7+) — recommended replacement
builder.Services.AddOutputCache(opts =>
{
    opts.AddBasePolicy(b => b.Expire(TimeSpan.FromMinutes(5)));
    opts.AddPolicy("Short", b => b.Expire(TimeSpan.FromSeconds(30)));
});
app.UseOutputCache();

app.MapGet("/products", GetProducts).CacheOutput();
app.MapGet("/prices",   GetPrices).CacheOutput("Short");

// Invalidate output cache programmatically
app.MapPost("/products", async (Product p, IOutputCacheStore store, CancellationToken ct) =>
{
    // ... save ...
    await store.EvictByTagAsync("products", ct);
    return Results.Created($"/products/{p.Id}", p);
});
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you optimize database access in .NET Core applications?

```cs
// 1. AsNoTracking — skip change tracking for read-only queries
var products = await db.Products
    .AsNoTracking()
    .Where(p => p.Category == "Electronics")
    .ToListAsync();

// 2. Projection — fetch only needed columns
var dtos = await db.Products
    .AsNoTracking()
    .Select(p => new ProductDto(p.Id, p.Name, p.Price))
    .ToListAsync();

// 3. Compiled queries — avoid repeated query compilation (.NET 7+)
private static readonly Func<AppDbContext, int, Task<Product?>> _getById =
    EF.CompileAsyncQuery((AppDbContext db, int id) =>
        db.Products.AsNoTracking().FirstOrDefault(p => p.Id == id));

var product = await _getById(db, 42);

// 4. Batch operations — EF Core 7+ ExecuteUpdate / ExecuteDelete
// ❌ Load-modify-save (N round trips)
var prods = await db.Products.Where(p => p.Price < 10).ToListAsync();
foreach (var p in prods) p.Price *= 1.1m;
await db.SaveChangesAsync();

// ✅ Single SQL UPDATE
await db.Products
    .Where(p => p.Price < 10)
    .ExecuteUpdateAsync(s => s.SetProperty(p => p.Price, p => p.Price * 1.1m));

// ✅ Single SQL DELETE
await db.Products.Where(p => p.Stock == 0).ExecuteDeleteAsync();

// 5. Eager loading to avoid N+1
var orders = await db.Orders
    .Include(o => o.Customer)
    .Include(o => o.Lines).ThenInclude(l => l.Product)
    .AsNoTracking()
    .ToListAsync();

// 6. Pagination — never load all rows
var page = await db.Products
    .OrderBy(p => p.Name)
    .Skip((pageNumber - 1) * pageSize)
    .Take(pageSize)
    .AsNoTracking()
    .ToListAsync();

// 7. Raw SQL for complex queries
var results = await db.Database
    .SqlQuery<ProductSummary>($"SELECT Id, Name, SUM(Qty) AS TotalSold FROM ...")
    .ToListAsync();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use connection pooling to improve database performance?

Connection pooling reuses existing database connections rather than creating a new one per request. **ADO.NET and EF Core use pooling by default** — the key is configuring it correctly.

```cs
// 1. ADO.NET — pooling is automatic when using connection strings
// The connection string controls pool size:
var connStr = "Server=myserver;Database=mydb;User=sa;Password=pass;" +
              "Min Pool Size=5;Max Pool Size=100;Connection Timeout=30;";

// Open/Close does NOT destroy the connection — it returns it to the pool
await using var conn = new SqlConnection(connStr);
await conn.OpenAsync();
// ... query ...
// conn.Close() / dispose → returned to pool

// 2. EF Core — uses ADO.NET pooling automatically
builder.Services.AddDbContext<AppDbContext>(opt =>
    opt.UseSqlServer(connStr,
        sql => sql.CommandTimeout(30)));

// 3. DbContext pooling — EF Core level (reuse DbContext instances, .NET 6+)
// Use AddDbContextPool instead of AddDbContext
builder.Services.AddDbContextPool<AppDbContext>(opt =>
    opt.UseSqlServer(connStr), poolSize: 128);

// With factory (for Blazor, manual scope control)
builder.Services.AddPooledDbContextFactory<AppDbContext>(opt =>
    opt.UseSqlServer(connStr));

// Usage with factory
public class ProductService(IDbContextFactory<AppDbContext> factory)
{
    public async Task<List<Product>> GetAllAsync()
    {
        await using var db = await factory.CreateDbContextAsync();
        return await db.Products.AsNoTracking().ToListAsync();
    }
}

// 4. Monitor pool health
// dotnet-counters monitor -p <PID> Microsoft.Data.SqlClient
// Watch: active-hard-connects, active-soft-connects, number-of-pooled-connections

// 5. Connection resiliency (transient fault handling)
builder.Services.AddDbContext<AppDbContext>(opt =>
    opt.UseSqlServer(connStr, sql =>
        sql.EnableRetryOnFailure(
            maxRetryCount: 3,
            maxRetryDelay: TimeSpan.FromSeconds(5),
            errorNumbersToAdd: null)));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use the `AsNoTracking` method to optimize query performance?

`AsNoTracking()` tells EF Core to **skip change tracking** for returned entities — the context does not monitor them for modifications. This saves memory and CPU for read-only queries.

```cs
// ❌ Default — EF Core tracks all returned entities (needed for updates)
var tracked = await db.Products.ToListAsync();
// EF holds a snapshot of each entity for change detection

// ✅ AsNoTracking — no snapshot stored, faster and less memory
var readOnly = await db.Products
    .AsNoTracking()
    .ToListAsync();

// ✅ AsNoTrackingWithIdentityResolution — avoids duplicates in navigation props
// Useful when Include() returns the same entity multiple times
var orders = await db.Orders
    .AsNoTrackingWithIdentityResolution()
    .Include(o => o.Lines)
    .ThenInclude(l => l.Product)
    .ToListAsync();

// ✅ Global setting — all queries in this context are non-tracked
db.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

// Benchmark context (queries per second):
// With tracking    : ~15,000 QPS
// AsNoTracking     : ~25,000 QPS  (+67%)
// Projection only  : ~35,000 QPS  (+133%)

// When NOT to use AsNoTracking:
// ❌ When you need to modify and save the entity
var product = await db.Products.FindAsync(id); // tracked — needed for update
product!.Price = newPrice;
await db.SaveChangesAsync(); // EF detects change via tracking

// AsNoTracking + manual update (EF Core 7+)
await db.Products
    .Where(p => p.Id == id)
    .ExecuteUpdateAsync(s => s.SetProperty(p => p.Price, newPrice));
// No loading needed — single UPDATE statement
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the secure coding practices (XSS, CSRF, SQL Injection) available in .NET?

```cs
// ── 1. XSS (Cross-Site Scripting) ────────────────────────────────────
// ASP.NET Core Razor auto-encodes output by default
@Model.UserInput  // HTML-encoded automatically — safe

// HtmlEncoder for manual encoding
using System.Text.Encodings.Web;
string safe = HtmlEncoder.Default.Encode("<script>alert(1)</script>");
// → &lt;script&gt;alert(1)&lt;/script&gt;

// Content Security Policy header
app.Use(async (ctx, next) =>
{
    ctx.Response.Headers.Append("Content-Security-Policy",
        "default-src 'self'; script-src 'self'; style-src 'self'");
    await next(ctx);
});

// ── 2. CSRF (Cross-Site Request Forgery) ─────────────────────────────
// MVC — Antiforgery token (automatic with [ValidateAntiForgeryToken])
builder.Services.AddAntiforgery(opt =>
{
    opt.HeaderName = "X-XSRF-TOKEN";  // for SPA clients
    opt.Cookie.SecurePolicy = CookieSecurePolicy.Always;
    opt.Cookie.SameSite = SameSiteMode.Strict;
});

// Controller
[HttpPost, ValidateAntiForgeryToken]
public IActionResult Create(ProductForm form) { /* safe */ return Ok(); }

// Minimal API — SameSite cookie + antiforgery
app.MapPost("/products", (IAntiforgery af, HttpContext ctx) =>
{
    af.ValidateRequestAsync(ctx);
    // ...
}).RequireAuthorization();

// ── 3. SQL Injection ──────────────────────────────────────────────────
// ❌ Vulnerable — string interpolation into SQL
var name = userInput;
var sql = $"SELECT * FROM Products WHERE Name = '{name}'";  // NEVER DO THIS

// ✅ EF Core — parameterised automatically
var products = await db.Products
    .Where(p => p.Name == name)
    .ToListAsync();

// ✅ Raw SQL with parameters (EF Core 7+)
var results = await db.Products
    .FromSql($"SELECT * FROM Products WHERE Name = {name}")  // interpolated → parameterised
    .ToListAsync();

// ✅ ADO.NET — explicit parameters
await using var cmd = new SqlCommand("SELECT * FROM Products WHERE Name = @name", conn);
cmd.Parameters.AddWithValue("@name", name);

// ── 4. Additional practices ───────────────────────────────────────────
// Enforce HTTPS
app.UseHttpsRedirection();
app.UseHsts(); // HTTP Strict Transport Security

// Secure cookies
builder.Services.ConfigureApplicationCookie(opts =>
{
    opts.Cookie.HttpOnly   = true;
    opts.Cookie.SecurePolicy = CookieSecurePolicy.Always;
    opts.Cookie.SameSite  = SameSiteMode.Strict;
});

// Security headers
app.Use(async (ctx, next) =>
{
    ctx.Response.Headers.Append("X-Content-Type-Options", "nosniff");
    ctx.Response.Headers.Append("X-Frame-Options", "DENY");
    ctx.Response.Headers.Append("Referrer-Policy", "no-referrer");
    await next(ctx);
});

// Input validation at boundary
app.MapPost("/products", ([FromBody] CreateProductRequest req) =>
{
    // Data annotations validated by [ApiController] before action runs
    return Results.Ok();
});
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 16. DEPLOYMENT

<br/>

## Q. What is the `dotnet publish` command and how is it used?

`dotnet publish` compiles the application and copies the output (binaries, dependencies, assets) to a folder ready for deployment.

```bash
# Framework-dependent (requires runtime on target machine — smaller output)
dotnet publish -c Release -o ./publish

# Self-contained (includes the runtime — no .NET needed on target)
dotnet publish -c Release -r linux-x64 --self-contained -o ./publish

# Single-file executable (everything packed into one .exe/.bin)
dotnet publish -c Release -r win-x64 \
  --self-contained \
  -p:PublishSingleFile=true \
  -p:IncludeNativeLibrariesForSelfExtract=true \
  -o ./publish

# Native AOT (.NET 7+ — no JIT, fast startup, small binary)
dotnet publish -c Release -r linux-x64 -p:PublishAot=true -o ./publish

# Trim unused code (reduces binary size)
dotnet publish -c Release -r linux-x64 --self-contained \
  -p:PublishTrimmed=true -o ./publish

# ReadyToRun — pre-JIT code for faster startup (not as small as AOT)
dotnet publish -c Release -r win-x64 --self-contained \
  -p:PublishReadyToRun=true -o ./publish
```

**Or configure in `.csproj`:**

```xml
<PropertyGroup>
  <RuntimeIdentifier>linux-x64</RuntimeIdentifier>
  <SelfContained>true</SelfContained>
  <PublishSingleFile>true</PublishSingleFile>
  <PublishTrimmed>true</PublishTrimmed>
</PropertyGroup>
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `WebHostBuilder` class and how is it used?

`WebHostBuilder` is the legacy (.NET Core 1.x–2.x) host builder. In .NET 6+, it was superseded by `WebApplication.CreateBuilder()` (minimal hosting model). The older `Host.CreateDefaultBuilder()` + `ConfigureWebHostDefaults()` pattern from .NET 3.1–5 is also still supported.

```cs
// ❌ Legacy — .NET Core 2.x WebHostBuilder (avoid in new code)
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .Build();

// ⚠️  .NET 3.1 / 5 — Generic Host + ConfigureWebHostDefaults
Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
        webBuilder.UseKestrel(opts => opts.Limits.MaxRequestBodySize = 10 * 1024 * 1024);
    })
    .Build().Run();

// ✅ .NET 10 — WebApplication.CreateBuilder (current best practice)
var builder = WebApplication.CreateBuilder(args);

builder.WebHost.ConfigureKestrel(opts =>
    opts.Limits.MaxConcurrentConnections = 1000);

builder.Services.AddControllers();

var app = builder.Build();
app.UseHttpsRedirection();
app.MapControllers();
app.Run();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you deploy a .NET Core application to Azure?

```bash
# 1. Azure App Service — simplest option
# Publish directly from CLI
dotnet publish -c Release -o ./publish
az login
az webapp deployment source config-zip \
  --resource-group myRG \
  --name myAppService \
  --src ./publish.zip

# Or use the Azure Web Apps deploy action (see GitHub Actions below)
```

```yaml
# 2. GitHub Actions — deploy to Azure App Service
name: Deploy to Azure
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4

    - name: Setup .NET 10
      uses: actions/setup-dotnet@v4
      with:
        dotnet-version: '10.0.x'

    - name: Publish
      run: dotnet publish -c Release -o ./publish

    - name: Deploy to Azure App Service
      uses: azure/webapps-deploy@v3
      with:
        app-name: 'myAppService'
        publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
        package: ./publish
```

```bash
# 3. Azure Container Apps — deploy as Docker container
docker build -t myapp:latest .
az acr login --name myRegistry
docker tag myapp:latest myregistry.azurecr.io/myapp:latest
docker push myregistry.azurecr.io/myapp:latest

az containerapp update \
  --name myapp \
  --resource-group myRG \
  --image myregistry.azurecr.io/myapp:latest

# 4. Azure Kubernetes Service (AKS)
kubectl apply -f deployment.yaml
kubectl set image deployment/myapp myapp=myregistry.azurecr.io/myapp:v2
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `Azure App Service` and how is it used?

**Azure App Service** is a fully managed PaaS (Platform-as-a-Service) for hosting web apps, REST APIs, and mobile backends. It handles OS patching, scaling, SSL, and load balancing automatically.

```bash
# Create and deploy via Azure CLI
az group create --name myRG --location uksouth

az appservice plan create \
  --name myPlan \
  --resource-group myRG \
  --sku B1 \          # Free(F1), Basic(B1), Standard(S1), Premium(P1v3)
  --is-linux

az webapp create \
  --resource-group myRG \
  --plan myPlan \
  --name my-dotnet-app \
  --runtime "DOTNET|10.0"

# Deploy a ZIP package
dotnet publish -c Release -o ./publish
Compress-Archive ./publish/* publish.zip
az webapp deploy \
  --resource-group myRG \
  --name my-dotnet-app \
  --src-path publish.zip

# Set environment variables / app settings
az webapp config appsettings set \
  --resource-group myRG \
  --name my-dotnet-app \
  --settings \
    ASPNETCORE_ENVIRONMENT=Production \
    ConnectionStrings__Default="Server=mydb.database.windows.net;..."

# Enable auto-scaling (Standard plan or higher)
az monitor autoscale create \
  --resource-group myRG \
  --resource my-dotnet-app \
  --resource-type Microsoft.Web/serverfarms \
  --name myAutoscale \
  --min-count 1 --max-count 5 --count 1
```

```cs
// Read App Service environment variables in code (same as any config)
var builder = WebApplication.CreateBuilder(args);
// App settings from Azure portal automatically appear as environment variables
string? dbConn = builder.Configuration.GetConnectionString("Default");
string? env    = builder.Configuration["ASPNETCORE_ENVIRONMENT"];
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use Docker to containerize a .NET Core application?

```dockerfile
# Dockerfile — multi-stage build for minimal final image (.NET 10)
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src

# Restore dependencies (cached layer)
COPY *.csproj ./
RUN dotnet restore

# Build and publish
COPY . ./
RUN dotnet publish -c Release -o /app/publish --no-restore

# Runtime image — smaller than sdk image
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS runtime
WORKDIR /app
COPY --from=build /app/publish .

# Non-root user (security best practice)
RUN adduser --disabled-password appuser
USER appuser

EXPOSE 8080
ENV ASPNETCORE_URLS=http://+:8080
ENTRYPOINT ["dotnet", "MyApp.dll"]
```

```bash
# Build image
docker build -t myapp:1.0 .

# Run locally
docker run -d -p 8080:8080 \
  -e ASPNETCORE_ENVIRONMENT=Development \
  -e ConnectionStrings__Default="Server=host.docker.internal;..." \
  --name myapp myapp:1.0

# Test
curl http://localhost:8080/health

# Push to registry
docker tag myapp:1.0 myregistry.azurecr.io/myapp:1.0
docker push myregistry.azurecr.io/myapp:1.0
```

```yaml
# docker-compose.yml — local dev with database
version: '3.8'
services:
  api:
    build: .
    ports: ["8080:8080"]
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ConnectionStrings__Default=Server=db;Database=MyDb;User=sa;Password=Pass@123
    depends_on: [db]

  db:
    image: mcr.microsoft.com/mssql/server:2022-latest
    environment:
      SA_PASSWORD: "Pass@123"
      ACCEPT_EULA: "Y"
    ports: ["1433:1433"]
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a Dockerfile for a .NET Core application?

```dockerfile
# Dockerfile — production-grade .NET 10 Web API

# ── Stage 1: Restore ──────────────────────────────────────────
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS restore
WORKDIR /src
COPY ["MyApi/MyApi.csproj", "MyApi/"]
COPY ["MyApi.Core/MyApi.Core.csproj", "MyApi.Core/"]
RUN dotnet restore "MyApi/MyApi.csproj"

# ── Stage 2: Build ────────────────────────────────────────────
FROM restore AS build
COPY . .
RUN dotnet build "MyApi/MyApi.csproj" -c Release --no-restore

# ── Stage 3: Publish ──────────────────────────────────────────
FROM build AS publish
RUN dotnet publish "MyApi/MyApi.csproj" \
    -c Release \
    --no-build \
    -o /app/publish

# ── Stage 4: Runtime (final, smallest image) ──────────────────
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS final

# Security: non-root user
RUN groupadd -r appgroup && useradd -r -g appgroup appuser
WORKDIR /app
COPY --from=publish /app/publish .

# Health check
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD curl -f http://localhost:8080/health || exit 1

USER appuser
EXPOSE 8080
ENV ASPNETCORE_URLS=http://+:8080 \
    DOTNET_RUNNING_IN_CONTAINER=true

ENTRYPOINT ["dotnet", "MyApi.dll"]
```

```bash
# .dockerignore — exclude unnecessary files
```

```text
# .dockerignore
**/.git
**/.vs
**/bin
**/obj
**/*.user
**/node_modules
**/Dockerfile*
**/.gitignore
README.md
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you deploy a .NET Core application using Docker?

```bash
# 1. Build and run locally
docker build -t myapi:latest .
docker run -d -p 8080:8080 --name myapi myapi:latest
curl http://localhost:8080/health

# 2. Push to Docker Hub
docker login
docker tag myapi:latest username/myapi:1.0
docker push username/myapi:1.0

# 3. Push to Azure Container Registry
az acr login --name myRegistry
docker tag myapi:latest myregistry.azurecr.io/myapi:1.0
docker push myregistry.azurecr.io/myapi:1.0

# 4. Deploy to Azure Container Apps
az containerapp create \
  --name myapi \
  --resource-group myRG \
  --environment myEnv \
  --image myregistry.azurecr.io/myapi:1.0 \
  --target-port 8080 \
  --ingress external \
  --min-replicas 1 \
  --max-replicas 10 \
  --cpu 0.5 --memory 1Gi

# 5. Deploy to Kubernetes (AKS)
kubectl apply -f - <<EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapi
spec:
  replicas: 3
  selector:
    matchLabels: { app: myapi }
  template:
    metadata:
      labels: { app: myapi }
    spec:
      containers:
      - name: myapi
        image: myregistry.azurecr.io/myapi:1.0
        ports:
        - containerPort: 8080
        resources:
          requests: { cpu: "100m", memory: "128Mi" }
          limits:   { cpu: "500m", memory: "512Mi" }
        livenessProbe:
          httpGet: { path: /health, port: 8080 }
          initialDelaySeconds: 10
EOF
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Kubernetes and how is it used with .NET Core applications?

**Kubernetes (K8s)** is an open-source container orchestration platform that automates deployment, scaling, and management of containerised applications. For .NET apps it handles rolling updates, health monitoring, auto-scaling, service discovery, and secrets management.

```yaml
# deployment.yaml — full .NET 10 Web API Kubernetes manifest
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapi
  labels: { app: myapi }
spec:
  replicas: 3
  selector:
    matchLabels: { app: myapi }
  template:
    metadata:
      labels: { app: myapi }
    spec:
      containers:
      - name: myapi
        image: myregistry.azurecr.io/myapi:1.0
        ports:
        - containerPort: 8080
        env:
        - name: ASPNETCORE_ENVIRONMENT
          value: Production
        - name: ConnectionStrings__Default
          valueFrom:
            secretKeyRef:
              name: myapi-secrets
              key: db-connection
        resources:
          requests: { cpu: "100m", memory: "128Mi" }
          limits:   { cpu: "500m", memory: "512Mi" }
        readinessProbe:
          httpGet: { path: /health/ready, port: 8080 }
          initialDelaySeconds: 5
        livenessProbe:
          httpGet: { path: /health/live, port: 8080 }
          initialDelaySeconds: 15
---
apiVersion: v1
kind: Service
metadata:
  name: myapi-svc
spec:
  selector: { app: myapi }
  ports:
  - port: 80
    targetPort: 8080
  type: LoadBalancer
```

```bash
# Apply manifests
kubectl apply -f deployment.yaml

# Rolling update (zero-downtime)
kubectl set image deployment/myapi myapi=myregistry.azurecr.io/myapi:1.1

# Scale
kubectl scale deployment myapi --replicas=5

# View logs
kubectl logs -l app=myapi --tail=100 -f
```

```cs
// Health check endpoints for Kubernetes probes
builder.Services.AddHealthChecks()
    .AddDbContextCheck<AppDbContext>();

app.MapHealthChecks("/health/live",  new() { Predicate = _ => false });   // liveness
app.MapHealthChecks("/health/ready", new() { Predicate = r => r.Tags.Contains("ready") }); // readiness
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use CI/CD pipelines to deploy .NET Core applications?

```yaml
# GitHub Actions — CI/CD pipeline for .NET 10 API → Azure App Service
name: CI/CD Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

env:
  DOTNET_VERSION: '10.0.x'
  AZURE_WEBAPP_NAME: 'my-dotnet-api'

jobs:
  # ── CI: Build & Test ──────────────────────────────────────
  build-and-test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4

    - name: Setup .NET ${{ env.DOTNET_VERSION }}
      uses: actions/setup-dotnet@v4
      with:
        dotnet-version: ${{ env.DOTNET_VERSION }}

    - name: Restore
      run: dotnet restore

    - name: Build
      run: dotnet build -c Release --no-restore

    - name: Test
      run: dotnet test -c Release --no-build \
             --logger "trx;LogFileName=results.trx" \
             --collect:"XPlat Code Coverage"

    - name: Upload test results
      uses: actions/upload-artifact@v4
      with:
        name: test-results
        path: "**/*.trx"

    - name: Publish
      run: dotnet publish -c Release -o ./publish --no-build

    - name: Upload artifact
      uses: actions/upload-artifact@v4
      with:
        name: app
        path: ./publish

  # ── CD: Deploy (main branch only) ────────────────────────
  deploy:
    needs: build-and-test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    environment: production

    steps:
    - name: Download artifact
      uses: actions/download-artifact@v4
      with:
        name: app
        path: ./publish

    - name: Deploy to Azure App Service
      uses: azure/webapps-deploy@v3
      with:
        app-name: ${{ env.AZURE_WEBAPP_NAME }}
        publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
        package: ./publish
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `Azure DevOps` service and how is it used for deployment?

**Azure DevOps** is a Microsoft platform providing: **Boards** (work tracking), **Repos** (Git), **Pipelines** (CI/CD), **Test Plans**, and **Artifacts** (NuGet/npm feeds).

```yaml
# azure-pipelines.yml — CI/CD for .NET 10 API
trigger:
  branches:
    include: [main]

pool:
  vmImage: 'ubuntu-latest'

variables:
  buildConfiguration: 'Release'
  dotnetVersion: '10.0.x'

stages:
# ── Stage 1: Build & Test ──────────────────────────────────
- stage: Build
  jobs:
  - job: BuildAndTest
    steps:
    - task: UseDotNet@2
      inputs:
        version: $(dotnetVersion)

    - script: dotnet restore
      displayName: Restore

    - script: dotnet build -c $(buildConfiguration) --no-restore
      displayName: Build

    - script: |
        dotnet test -c $(buildConfiguration) --no-build \
          --logger trx \
          --collect "XPlat Code Coverage"
      displayName: Test

    - task: PublishTestResults@2
      inputs:
        testResultsFormat: VSTest
        testResultsFiles: '**/*.trx'

    - script: dotnet publish -c $(buildConfiguration) -o $(Build.ArtifactStagingDirectory)/publish
      displayName: Publish

    - task: PublishBuildArtifacts@1
      inputs:
        PathtoPublish: $(Build.ArtifactStagingDirectory)/publish
        ArtifactName: drop

# ── Stage 2: Deploy to Staging ────────────────────────────
- stage: DeployStaging
  dependsOn: Build
  jobs:
  - deployment: DeployToStaging
    environment: staging
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            inputs:
              azureSubscription: 'MyServiceConnection'
              appName: 'my-api-staging'
              package: $(Pipeline.Workspace)/drop

# ── Stage 3: Deploy to Production (with approval) ─────────
- stage: DeployProd
  dependsOn: DeployStaging
  jobs:
  - deployment: DeployToProduction
    environment: production   # configure approvals in Azure DevOps portal
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            inputs:
              azureSubscription: 'MyServiceConnection'
              appName: 'my-api-production'
              package: $(Pipeline.Workspace)/drop
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you configure a build pipeline in Azure DevOps?

A **build pipeline** compiles code, runs tests, and produces deployable artifacts. It is defined in `azure-pipelines.yml` at the repo root.

```yaml
# azure-pipelines.yml — Build pipeline for .NET 10
trigger:
  branches:
    include: [main, develop]
  paths:
    exclude: ['*.md', 'docs/**']

pr:
  branches:
    include: [main]

pool:
  vmImage: 'ubuntu-latest'

variables:
  buildConfiguration: 'Release'

steps:
# 1. Use specified .NET SDK
- task: UseDotNet@2
  displayName: 'Install .NET 10 SDK'
  inputs:
    version: '10.0.x'
    includePreviewVersions: false

# 2. Restore
- script: dotnet restore --locked-mode
  displayName: 'dotnet restore'

# 3. Build
- script: dotnet build -c $(buildConfiguration) --no-restore
  displayName: 'dotnet build'

# 4. Test with coverage
- script: |
    dotnet test -c $(buildConfiguration) --no-build \
      --logger "trx;LogFileName=TestResults.trx" \
      --collect "XPlat Code Coverage" \
      -- DataCollectionRunSettings.DataCollectors.DataCollector.Configuration.Format=cobertura
  displayName: 'dotnet test'

- task: PublishTestResults@2
  inputs:
    testResultsFormat: VSTest
    testResultsFiles: '**/TestResults.trx'
    failTaskOnFailedTests: true

- task: PublishCodeCoverageResults@2
  inputs:
    summaryFileLocation: '**/coverage.cobertura.xml'

# 5. Publish
- script: dotnet publish src/MyApi -c $(buildConfiguration) --no-build -o $(Build.ArtifactStagingDirectory)
  displayName: 'dotnet publish'

# 6. Archive artifact
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: $(Build.ArtifactStagingDirectory)
    ArtifactName: 'myapi-$(Build.BuildNumber)'
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you configure a release pipeline in Azure DevOps?

A **release pipeline** (or deployment stage in YAML) takes a build artifact and deploys it to one or more environments, optionally with approval gates.

```yaml
# Deployment stages added to azure-pipelines.yml (single YAML pipeline)
stages:
- stage: Build
  jobs:
  - job: Build
    steps:
    - script: dotnet publish -c Release -o $(Build.ArtifactStagingDirectory)
    - task: PublishBuildArtifacts@1
      inputs: { ArtifactName: drop }

- stage: DeployDev
  displayName: 'Deploy to Dev'
  dependsOn: Build
  condition: succeeded()
  variables:
    webAppName: 'myapi-dev'
  jobs:
  - deployment: Deploy
    environment: dev        # no approval needed
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            inputs:
              azureSubscription: 'MyServiceConnection'
              appName: $(webAppName)
              package: $(Pipeline.Workspace)/drop

          - task: AzureAppServiceSettings@1
            inputs:
              azureSubscription: 'MyServiceConnection'
              appName: $(webAppName)
              appSettings: |
                [
                  {"name": "ASPNETCORE_ENVIRONMENT", "value": "Development"}
                ]

- stage: DeployProd
  displayName: 'Deploy to Production'
  dependsOn: DeployDev
  condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
  variables:
    webAppName: 'myapi-prod'
  jobs:
  - deployment: Deploy
    environment: production   # configure approval in Environments → Approvals & Checks
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            displayName: 'Blue-Green slot swap'
            inputs:
              azureSubscription: 'MyServiceConnection'
              appName: $(webAppName)
              deployToSlotOrASE: true
              resourceGroupName: myRG
              slotName: staging        # deploy to staging slot first
              package: $(Pipeline.Workspace)/drop

          - task: AzureAppServiceManage@0
            displayName: 'Swap staging → production'
            inputs:
              azureSubscription: 'MyServiceConnection'
              Action: 'Swap Slots'
              WebAppName: $(webAppName)
              ResourceGroupName: myRG
              SourceSlot: staging
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use GitHub Actions for deploying .NET Core applications?

```yaml
# .github/workflows/deploy.yml — Full CI/CD with GitHub Actions
name: Build, Test & Deploy

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    outputs:
      artifact-path: ${{ steps.publish.outputs.artifact-path }}

    steps:
    - uses: actions/checkout@v4

    - uses: actions/setup-dotnet@v4
      with:
        dotnet-version: '10.0.x'

    - run: dotnet restore
    - run: dotnet build -c Release --no-restore
    - run: dotnet test -c Release --no-build --logger "trx" --collect "XPlat Code Coverage"

    - uses: actions/upload-artifact@v4        # upload test results
      with:
        name: test-results
        path: "**/*.trx"

    - name: Publish
      id: publish
      run: dotnet publish -c Release -o ./publish --no-build

    - uses: actions/upload-artifact@v4
      with:
        name: app
        path: ./publish

  deploy-azure:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    environment:
      name: production
      url: https://my-dotnet-api.azurewebsites.net

    steps:
    - uses: actions/download-artifact@v4
      with: { name: app, path: ./publish }

    # Option A — Azure App Service
    - uses: azure/webapps-deploy@v3
      with:
        app-name: 'my-dotnet-api'
        publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
        package: ./publish

  deploy-docker:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
    - uses: actions/checkout@v4

    # Option B — Build & push Docker image to GHCR
    - uses: docker/login-action@v3
      with:
        registry: ghcr.io
        username: ${{ github.actor }}
        password: ${{ secrets.GITHUB_TOKEN }}

    - uses: docker/build-push-action@v6
      with:
        context: .
        push: true
        tags: ghcr.io/${{ github.repository }}:${{ github.sha }}
        cache-from: type=gha
        cache-to: type=gha,mode=max
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `Octopus Deploy` tool and how is it used?

**Octopus Deploy** is a release management and deployment automation tool that complements CI systems (Azure DevOps, GitHub Actions, Jenkins). It models environments, targets, deployment processes, and variable sets separately from build pipelines.

```yaml
# GitHub Actions → Octopus Deploy integration
- name: Push package to Octopus
  uses: OctopusDeploy/push-package-action@v3
  with:
    api_key: ${{ secrets.OCTOPUS_API_KEY }}
    server: https://mycompany.octopus.app
    packages: myapi.1.0.${{ github.run_number }}.zip

- name: Create release in Octopus
  uses: OctopusDeploy/create-release-action@v3
  with:
    api_key: ${{ secrets.OCTOPUS_API_KEY }}
    server: https://mycompany.octopus.app
    project: MyApi
    release_number: 1.0.${{ github.run_number }}

- name: Deploy to Staging
  uses: OctopusDeploy/deploy-release-action@v3
  with:
    api_key: ${{ secrets.OCTOPUS_API_KEY }}
    server: https://mycompany.octopus.app
    project: MyApi
    release_number: 1.0.${{ github.run_number }}
    environments: Staging
```

**Key Octopus concepts:**
| Concept | Description |
|---------|-------------|
| **Project** | Deployment process definition (steps, variables) |
| **Environment** | Dev / Staging / Production logical grouping |
| **Release** | Snapshot of project + package versions |
| **Deployment** | Running a release against an environment |
| **Runbook** | Operational scripts (db backup, restart service) |
| **Variable Sets** | Shared variables across projects |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the best practices for deploying .NET Core applications?

```bash
# 1. Always publish in Release configuration
dotnet publish -c Release -o ./publish

# 2. Use health checks for readiness/liveness probes
```

```cs
// Health checks
builder.Services.AddHealthChecks()
    .AddDbContextCheck<AppDbContext>(tags: ["ready"])
    .AddUrlGroup(new Uri("https://ext-service/health"), tags: ["ready"]);

app.MapHealthChecks("/health/live",  new() { Predicate = _ => false });
app.MapHealthChecks("/health/ready", new() { Predicate = r => r.Tags.Contains("ready") });
```

```bash
# 3. Secrets management — never commit secrets; use environment vars / Key Vault
az keyvault secret set --vault-name myVault --name db-password --value "s3cret"
# In app: builder.Configuration.AddAzureKeyVault(...)

# 4. Use structured logging (Serilog/OpenTelemetry)
# 5. Enable HTTPS everywhere
# 6. Multi-stage Docker builds (keep images small)
# 7. Pin runtime versions in Dockerfile FROM mcr.microsoft.com/dotnet/aspnet:10.0
# 8. Blue-green or canary deployments via deployment slots (App Service) or K8s
# 9. Database migrations — apply via startup or migration job, never in production manually
# 10. Enable AOT/ReadyToRun for faster cold starts
dotnet publish -c Release -r linux-x64 -p:PublishAot=true
```

```cs
// Graceful shutdown
builder.Services.Configure<HostOptions>(o =>
    o.ShutdownTimeout = TimeSpan.FromSeconds(30)); // allow in-flight requests to complete

// Environment-specific config
if (app.Environment.IsProduction())
{
    app.UseExceptionHandler("/error");
    app.UseHsts();
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the different types of hosting models available in .NET Core?

| Hosting model | Description | Use case |
|--------------|-------------|---------|
| **In-process (IIS)** | App runs inside IIS worker process (`w3wp.exe`) | Windows IIS hosting, better performance |
| **Out-of-process (IIS)** | IIS proxies to Kestrel running as separate process | Isolation, Linux-compatible workflow |
| **Kestrel (edge)** | Kestrel directly exposed to internet | Linux, Docker, Cloud |
| **Kestrel + reverse proxy** | Nginx/IIS/YARP in front of Kestrel | Production recommended |
| **Self-contained** | App includes .NET runtime — no SDK needed on host | Offline, locked environments |
| **Framework-dependent** | Uses installed .NET runtime | Shared hosting, smaller package |
| **Docker container** | App + runtime in container image | K8s, Azure Container Apps |
| **Native AOT** | Compiled to native binary, no JIT/runtime | Ultra-fast startup, serverless, CLI tools |
| **Background Service / Worker** | `IHostedService` without HTTP | Message queues, scheduled jobs |

```cs
// In-process IIS hosting — .csproj
// <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>

// Out-of-process IIS hosting
// <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>

// Worker Service (no HTTP)
var builder = Host.CreateApplicationBuilder(args);
builder.Services.AddHostedService<MyWorker>();
await builder.Build().RunAsync();

// Self-contained publish
// dotnet publish -r win-x64 --self-contained -o ./publish

// Native AOT
// dotnet publish -r linux-x64 -p:PublishAot=true -o ./publish

// Kestrel with Unix socket (Nginx upstream)
builder.WebHost.ConfigureKestrel(opts =>
    opts.ListenUnixSocket("/tmp/myapp.sock"));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 17. .NET Core

<br/>

## Q. What is .NET Core?

**.NET Core** (now simply called **.NET**) is Microsoft\'s open-source, cross-platform successor to .NET Framework. Starting with .NET 5, the "Core" name was dropped and it became the single unified runtime. The current version is **.NET 10** (2026).

**Key characteristics:**
- **Cross-platform** — Windows, Linux, macOS, ARM64
- **Open source** — hosted on GitHub (dotnet/runtime, dotnet/aspnetcore)
- **High performance** — consistently top-ranked in TechEmpower benchmarks
- **Modular** — NuGet-based; only include what you need
- **Cloud-native** — Docker, Kubernetes, Azure-first design
- **Unified** — one SDK for web, desktop, mobile, cloud, IoT, AI

```bash
# Check installed runtimes
dotnet --list-runtimes

# Check SDK versions
dotnet --list-sdks

# Current version
dotnet --version  # e.g. 10.0.100
```

```cs
// Minimal .NET 10 console app (top-level statements, no boilerplate)
using Microsoft.Extensions.Hosting;

var host = Host.CreateApplicationBuilder(args);
host.Services.AddHostedService<MyWorker>();
await host.Build().RunAsync();

class MyWorker : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken ct)
    {
        while (!ct.IsCancellationRequested)
        {
            Console.WriteLine($"[{DateTime.UtcNow:u}] Worker running");
            await Task.Delay(1000, ct);
        }
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the main differences between .NET Framework and .NET Core?

| Feature | .NET Framework | .NET (Core / 5+) |
|---------|---------------|-----------------|
| Platform | Windows only | Cross-platform |
| Open source | Partially | Fully (GitHub) |
| Deployment | GAC / machine-wide | Self-contained / side-by-side |
| Performance | Good | Significantly faster |
| Current status | Maintenance mode (4.8.x) | Active development (.NET 10) |
| ASP.NET | System.Web (heavy) | ASP.NET Core (lightweight, Kestrel) |
| WPF / WinForms | ✅ | ✅ (Windows only) |
| Xamarin / MAUI | ❌ | ✅ |
| AOT compilation | ❌ | ✅ (.NET 7+) |
| Containers | Limited | First-class Docker support |

```bash
# .NET Framework — Windows only, targeting net48
<TargetFramework>net48</TargetFramework>

# .NET 10 — cross-platform
<TargetFramework>net10.0</TargetFramework>

# Multi-targeting both
<TargetFrameworks>net48;net10.0</TargetFrameworks>
```

```cs
// .NET 10 — self-contained publish (no runtime needed on target machine)
// dotnet publish -r linux-x64 --self-contained -p:PublishSingleFile=true

// .NET 10 — Native AOT (no JIT, instant startup)
// dotnet publish -r linux-x64 -p:PublishAot=true
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the .NET Standard?

**.NET Standard** is a formal specification of .NET APIs that all .NET implementations must support. It was the bridge that allowed a single library to run on .NET Framework, .NET Core, Xamarin, and Unity simultaneously.

**Status:** With .NET 5+ unifying all platforms, .NET Standard is **no longer evolving**. New libraries should target `net10.0` (or a specific TFM). .NET Standard 2.0 is still useful for libraries that must support legacy .NET Framework 4.6.1+.

| .NET Standard | .NET Framework | .NET Core / .NET |
|--------------|---------------|-----------------|
| 2.0 | 4.6.1+ | 2.0+ |
| 2.1 | ❌ (never) | 3.0+ |
| *(no 3.0)* | — | Use net10.0 TFM |

```xml
<!-- Library targeting .NET Standard 2.0 — works on .NET Framework 4.6.1+ and .NET 10 -->
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
  </PropertyGroup>
</Project>

<!-- Modern library — no legacy support needed -->
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net10.0</TargetFramework>
  </PropertyGroup>
</Project>

<!-- Multi-target for maximum compatibility -->
<TargetFrameworks>netstandard2.0;net10.0</TargetFrameworks>
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle configuration in a .NET Core application?

.NET uses a layered configuration system via `Microsoft.Extensions.Configuration`. Sources are stacked — later sources override earlier ones.

```cs
// appsettings.json
{
  "App": {
    "Name": "MyApi",
    "Timeout": 30
  },
  "ConnectionStrings": {
    "Default": "Server=localhost;Database=MyDb"
  }
}
```

```cs
// Program.cs (.NET 10 — WebApplication.CreateBuilder sets up config automatically)
var builder = WebApplication.CreateBuilder(args);

// Config sources (in priority order, lowest → highest):
// 1. appsettings.json
// 2. appsettings.{Environment}.json
// 3. Environment variables
// 4. Command-line args

// Bind to a strongly-typed options class
builder.Services.Configure<AppOptions>(
    builder.Configuration.GetSection("App"));

var app = builder.Build();

// Read raw value
string name = builder.Configuration["App:Name"]!;
string conn = builder.Configuration.GetConnectionString("Default")!;

// Inject IOptions<T> in a service
public class MyService(IOptions<AppOptions> opts)
{
    private readonly AppOptions _opts = opts.Value;
    public string AppName => _opts.Name;
}

public class AppOptions
{
    public string Name { get; set; } = "";
    public int Timeout { get; set; }
}

// Add custom JSON config source
builder.Configuration.AddJsonFile("custom.json", optional: true, reloadOnChange: true);

// Add environment variable with prefix
builder.Configuration.AddEnvironmentVariables(prefix: "MYAPP_");
// MYAPP_App__Name=Override  →  Config["App:Name"] = "Override"
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the .NET Core CLI and how is it used?

The **.NET CLI** (`dotnet`) is the cross-platform command-line interface for creating, building, running, testing, and publishing .NET applications.

```bash
# --- Project Management ---
dotnet new console -n MyApp          # create console app
dotnet new webapi -n MyApi           # create Web API
dotnet new classlib -n MyLib         # create class library
dotnet new sln -n MySolution         # create solution file
dotnet sln add MyApp/MyApp.csproj    # add project to solution

# --- Build & Run ---
dotnet build                         # compile
dotnet run                           # build + run
dotnet run --project MyApp           # specify project
dotnet watch run                     # hot reload on file changes

# --- Testing ---
dotnet test                          # run all tests
dotnet test --filter "Category=Unit" # filter tests
dotnet test --collect "Code Coverage"

# --- Package Management ---
dotnet add package Serilog            # install NuGet package
dotnet remove package Serilog         # remove package
dotnet list package --outdated        # show outdated packages
dotnet restore                        # restore dependencies

# --- Publish ---
dotnet publish -c Release -r linux-x64 --self-contained
dotnet publish -p:PublishSingleFile=true
dotnet publish -p:PublishAot=true     # Native AOT (.NET 7+)

# --- Tools ---
dotnet tool install -g dotnet-ef      # install global tool
dotnet ef migrations add InitialCreate
dotnet ef database update

# --- Info ---
dotnet --version
dotnet --list-sdks
dotnet --list-runtimes
dotnet nuget list source
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a new .NET Core project?

```bash
# Create console app
dotnet new console -n HelloWorld
cd HelloWorld
dotnet run
# Output: Hello, World!

# Create ASP.NET Core Web API
dotnet new webapi -n MyApi --use-minimal-apis
cd MyApi
dotnet run
# Swagger at https://localhost:5001/swagger

# Create solution with multiple projects
mkdir MySolution && cd MySolution
dotnet new sln -n MySolution
dotnet new webapi -n MyApi
dotnet new classlib -n MyApi.Core
dotnet new xunit -n MyApi.Tests
dotnet sln add MyApi MyApi.Core MyApi.Tests
dotnet add MyApi/MyApi.csproj reference MyApi.Core/MyApi.Core.csproj
dotnet add MyApi.Tests/MyApi.Tests.csproj reference MyApi/MyApi.csproj

# Build and test everything
dotnet build
dotnet test
```

**Minimal Web API (generated by `dotnet new webapi --use-minimal-apis`):**

```cs
// Program.cs — .NET 10 minimal API
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddOpenApi();

var app = builder.Build();
app.MapOpenApi();

app.MapGet("/hello", () => new { Message = "Hello, .NET 10!" });

app.MapGet("/weather", () =>
{
    var forecasts = Enumerable.Range(1, 5).Select(i => new WeatherForecast(
        DateOnly.FromDateTime(DateTime.Now.AddDays(i)),
        Random.Shared.Next(-20, 55),
        "Sunny"));
    return forecasts;
});

app.Run();

record WeatherForecast(DateOnly Date, int TemperatureC, string Summary);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the Program.cs and Startup.cs files in a .NET Core application?

In modern .NET (6+), **`Startup.cs` was eliminated** and its responsibilities merged into `Program.cs` using the minimal hosting model.

**Before .NET 6 (two files):**

```cs
// Program.cs — entry point, created the host
public class Program
{
    public static void Main(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(web => web.UseStartup<Startup>())
            .Build().Run();
}

// Startup.cs — service registration + middleware pipeline
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllers();
    }
    public void Configure(IApplicationBuilder app)
    {
        app.UseRouting();
        app.UseEndpoints(e => e.MapControllers());
    }
}
```

**.NET 10 (single Program.cs):**

```cs
// Program.cs — everything in one place
var builder = WebApplication.CreateBuilder(args);

// === ConfigureServices equivalent ===
builder.Services.AddControllers();
builder.Services.AddOpenApi();
builder.Services.AddScoped<IOrderService, OrderService>();

var app = builder.Build();

// === Configure equivalent (middleware pipeline) ===
if (app.Environment.IsDevelopment())
    app.MapOpenApi();

app.UseHttpsRedirection();
app.UseAuthentication();
app.UseAuthorization();
app.MapControllers();

app.Run();
```

**`Program.cs` roles in .NET 10:**
- Sets up the `WebApplicationBuilder` (config, DI, logging)
- Registers services into the DI container
- Builds and configures the middleware pipeline
- Starts the server

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you identify duplicated code in C#, and what techniques can be used to remove it?

**Identifying duplicates:**
- Visual Studio / Rider — built-in "Find Code Clones" / duplicate detection
- **SonarQube/SonarCloud** — detects duplicated blocks with metrics
- **ReSharper** — highlights duplicate code fragments
- **Roslyn Analyzers** — custom rules targeting repetitive patterns

**Techniques to remove duplication:**

```cs
// ❌ Duplicated logic
public decimal CalculateUkTax(decimal amount) => amount * 0.20m;
public decimal CalculateUsTax(decimal amount) => amount * 0.10m;
// Same structure repeated for each region

// ✅ 1. Extract Method / Helper
public decimal CalculateTax(decimal amount, decimal rate) => amount * rate;
// Usage:
decimal uk = CalculateTax(100m, 0.20m);
decimal us = CalculateTax(100m, 0.10m);

// ✅ 2. Generic method
public static T Clamp<T>(T value, T min, T max) where T : IComparable<T>
    => value.CompareTo(min) < 0 ? min : value.CompareTo(max) > 0 ? max : value;

// ✅ 3. Strategy pattern for varying behavior
public interface ITaxStrategy { decimal Calculate(decimal amount); }
public class UkTax : ITaxStrategy { public decimal Calculate(decimal a) => a * 0.20m; }
public class UsTax : ITaxStrategy { public decimal Calculate(decimal a) => a * 0.10m; }

// ✅ 4. Extension methods for repeated operations on types
public static class StringExtensions
{
    public static bool IsNullOrEmpty(this string? s) => string.IsNullOrEmpty(s);
    public static string ToTitleCase(this string s) =>
        System.Globalization.CultureInfo.CurrentCulture.TextInfo.ToTitleCase(s.ToLower());
}

// ✅ 5. Base class / template method for duplicated class structures
public abstract class ReportBase
{
    public string Generate()   // template method
    {
        var data    = FetchData();
        var body    = FormatBody(data);
        return $"<report>{body}</report>";
    }
    protected abstract IEnumerable<object> FetchData();
    protected abstract string FormatBody(IEnumerable<object> data);
}

// ✅ 6. Generic repository to remove per-entity CRUD duplication
public class Repository<T>(AppDbContext db) where T : class
{
    public Task<T?> GetByIdAsync(int id) => db.Set<T>().FindAsync(id).AsTask();
    public void Add(T entity) => db.Set<T>().Add(entity);
    public Task SaveAsync() => db.SaveChangesAsync();
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Kestrel?

**Kestrel** is the cross-platform, high-performance HTTP server built into ASP.NET Core. It is the default web server and handles incoming HTTP connections directly without requiring IIS or Apache.

**Key features:**
- HTTP/1.1, HTTP/2, HTTP/3 (QUIC) support
- TLS termination
- Unix domain sockets, named pipes
- Used as edge server or behind a reverse proxy (Nginx, IIS, Azure Front Door)

```cs
// Program.cs — Kestrel is used by default
var builder = WebApplication.CreateBuilder(args);

// Configure Kestrel explicitly
builder.WebHost.ConfigureKestrel(options =>
{
    // HTTP on port 5000
    options.ListenLocalhost(5000);

    // HTTPS on port 5001 with certificate
    options.ListenLocalhost(5001, listenOptions =>
    {
        listenOptions.UseHttps("cert.pfx", "password");
        listenOptions.Protocols = Microsoft.AspNetCore.Server.Kestrel
            .Core.HttpProtocols.Http1AndHttp2AndHttp3;
    });

    // Limits
    options.Limits.MaxConcurrentConnections = 1000;
    options.Limits.MaxRequestBodySize = 10 * 1024 * 1024; // 10 MB
    options.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
});

// Or configure via appsettings.json
/*
"Kestrel": {
  "Endpoints": {
    "Http":  { "Url": "http://0.0.0.0:80" },
    "Https": { "Url": "https://0.0.0.0:443",
               "Certificate": { "Path": "cert.pfx", "Password": "pw" } }
  }
}
*/

var app = builder.Build();
app.MapGet("/", () => "Running on Kestrel!");
app.Run();
```

**Kestrel vs IIS:**

| Feature | Kestrel | IIS |
|---------|---------|-----|
| Platform | Cross-platform | Windows only |
| Performance | Very high | Good |
| Edge server | ✅ | ✅ |
| Process management | Manual / systemd | Built-in |
| Reverse proxy | Recommended pairing | Built-in |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Dependency Injection in .NET Core?

**Dependency Injection (DI)** is a design pattern where a class receives its dependencies from an external source (the DI container) rather than creating them itself. ASP.NET Core has a built-in DI container via `IServiceCollection`.

```cs
// 1. Define abstraction and implementation
public interface IEmailService
{
    Task SendAsync(string to, string subject, string body);
}

public class SmtpEmailService(IConfiguration config) : IEmailService
{
    public async Task SendAsync(string to, string subject, string body)
    {
        // Use config["Smtp:Host"] etc.
        Console.WriteLine($"Sending email to {to}: {subject}");
        await Task.CompletedTask;
    }
}

// 2. Register in DI container (Program.cs)
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddScoped<IEmailService, SmtpEmailService>();

// 3. Inject via constructor (C# 12 primary constructor)
public class OrderService(IEmailService emailService, ILogger<OrderService> logger)
{
    public async Task PlaceOrderAsync(Order order)
    {
        // ... business logic ...
        await emailService.SendAsync(order.CustomerEmail,
            "Order Confirmed", $"Order #{order.Id} confirmed.");
        logger.LogInformation("Order {OrderId} placed", order.Id);
    }
}

// 4. Inject into minimal API endpoints
app.MapPost("/orders", async (Order order, IOrderService svc) =>
{
    await svc.PlaceOrderAsync(order);
    return Results.Created($"/orders/{order.Id}", order);
});

// 5. Inject into controllers
[ApiController]
[Route("api/[controller]")]
public class OrdersController(IOrderService orderService) : ControllerBase
{
    [HttpPost]
    public async Task<IActionResult> Create(Order order)
    {
        await orderService.PlaceOrderAsync(order);
        return CreatedAtAction(nameof(GetById), new { id = order.Id }, order);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you add services to the dependency injection container?

```cs
var builder = WebApplication.CreateBuilder(args);
var services = builder.Services;

// 1. Lifetime registrations
services.AddTransient<IOrderService, OrderService>();   // new instance every request
services.AddScoped<ICartService, CartService>();         // one per HTTP request
services.AddSingleton<ICacheService, MemoryCacheService>(); // one per app lifetime

// 2. Register with factory (for complex construction)
services.AddScoped<IDbConnection>(_ =>
    new SqlConnection(builder.Configuration.GetConnectionString("Default")));

// 3. Register multiple implementations
services.AddScoped<INotifier, EmailNotifier>();
services.AddScoped<INotifier, SmsNotifier>();
// Inject IEnumerable<INotifier> to get all

// 4. Named/keyed services (.NET 8+)
services.AddKeyedScoped<IPaymentGateway, StripeGateway>("stripe");
services.AddKeyedScoped<IPaymentGateway, PayPalGateway>("paypal");
// Inject: ([FromKeyedServices("stripe")] IPaymentGateway gateway)

// 5. Options pattern
services.Configure<SmtpOptions>(builder.Configuration.GetSection("Smtp"));
// Inject: IOptions<SmtpOptions>, IOptionsSnapshot<T>, IOptionsMonitor<T>

// 6. Extension methods for clean grouping
services.AddApplicationServices(builder.Configuration);

// Extension method
public static class ServiceCollectionExtensions
{
    public static IServiceCollection AddApplicationServices(
        this IServiceCollection services, IConfiguration config)
    {
        services.AddScoped<IOrderService, OrderService>();
        services.AddScoped<IProductRepository, ProductRepository>();
        services.Configure<AppSettings>(config.GetSection("App"));
        return services;
    }
}

// 7. Auto-registration with scrutor
// dotnet add package Scrutor
services.Scan(scan => scan
    .FromAssemblyOf<IOrderService>()
    .AddClasses(c => c.AssignableTo(typeof(IRepository<>)))
    .AsImplementedInterfaces()
    .WithScopedLifetime());
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `IServiceCollection` and `IServiceProvider`?

| | `IServiceCollection` | `IServiceProvider` |
|-|---------------------|-------------------|
| **Role** | Registration — add/configure services | Resolution — retrieve service instances |
| **When used** | Startup / `Program.cs` (before `Build()`) | Runtime — after `Build()` |
| **Key methods** | `AddScoped`, `AddSingleton`, `AddTransient`, `Configure` | `GetService<T>`, `GetRequiredService<T>`, `CreateScope` |
| **Mutability** | Mutable — add services | Read-only — resolve services |

```cs
var builder = WebApplication.CreateBuilder(args);

// IServiceCollection — registration phase
IServiceCollection services = builder.Services;
services.AddScoped<IOrderService, OrderService>();
services.AddSingleton<IClock, SystemClock>();

var app = builder.Build();

// IServiceProvider — resolution phase (after Build)
IServiceProvider provider = app.Services;

// Resolve a singleton directly (rare — prefer injection)
var clock = provider.GetRequiredService<IClock>();
Console.WriteLine(clock.UtcNow);

// Resolve scoped service correctly — create a scope
using var scope = provider.CreateScope();
var orderService = scope.ServiceProvider.GetRequiredService<IOrderService>();
await orderService.ProcessAsync();

// GetService<T> vs GetRequiredService<T>
var optional = provider.GetService<IOptionalService>(); // null if not registered
var required = provider.GetRequiredService<IOrderService>(); // throws if not registered

// Anti-pattern: Service Locator — avoid in application code
// ✅ Prefer constructor injection over IServiceProvider in services
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you configure logging in .NET Core?

.NET\'s built-in logging uses `ILogger<T>` / `ILoggerFactory` from `Microsoft.Extensions.Logging`. The default builder configures console, debug, and event source providers.

```cs
// Program.cs
var builder = WebApplication.CreateBuilder(args);

// Configure logging
builder.Logging
    .ClearProviders()                          // remove defaults
    .AddConsole()                              // console output
    .AddDebug()                                // VS Output window
    .AddEventSourceLogger()                    // ETW / dotnet-trace
    .SetMinimumLevel(LogLevel.Information);    // global minimum

// Per-category level from appsettings.json:
// "Logging": {
//   "LogLevel": { "Default": "Information", "Microsoft.AspNetCore": "Warning" }
// }

// Structured logging with Serilog (recommended for production)
// dotnet add package Serilog.AspNetCore
builder.Host.UseSerilog((ctx, cfg) =>
    cfg.ReadFrom.Configuration(ctx.Configuration)
       .WriteTo.Console(outputTemplate:
           "[{Timestamp:HH:mm:ss} {Level:u3}] {SourceContext}: {Message}{NewLine}{Exception}")
       .WriteTo.File("logs/app-.log", rollingInterval: RollingInterval.Day));

// Use ILogger<T> in services
public class OrderService(ILogger<OrderService> logger)
{
    public async Task PlaceOrderAsync(int orderId)
    {
        logger.LogInformation("Placing order {OrderId}", orderId);   // structured

        try
        {
            // ... work ...
            logger.LogInformation("Order {OrderId} placed successfully", orderId);
        }
        catch (Exception ex)
        {
            logger.LogError(ex, "Failed to place order {OrderId}", orderId);
            throw;
        }
    }
}

// Log levels (lowest → highest severity):
// Trace, Debug, Information, Warning, Error, Critical, None
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you perform database migrations in EF Core?

EF Core migrations track schema changes as versioned C# files and apply them to the database.

```bash
# Install EF Core tools
dotnet tool install -g dotnet-ef

# Add EF Core packages to project
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Design
```

```cs
// DbContext
public class AppDbContext(DbContextOptions<AppDbContext> options) : DbContext(options)
{
    public DbSet<Product> Products => Set<Product>();
    public DbSet<Order>   Orders   => Set<Order>();
}

public class Product
{
    public int Id { get; set; }
    public required string Name { get; set; }
    public decimal Price { get; set; }
}
```

```bash
# Create the initial migration
dotnet ef migrations add InitialCreate --output-dir Data/Migrations

# Apply migrations to the database
dotnet ef database update

# Add a new migration after model changes
dotnet ef migrations add AddOrderDate

# Roll back to a specific migration
dotnet ef database update InitialCreate

# Generate SQL script (for DBA review / production)
dotnet ef migrations script --output migration.sql --idempotent

# Remove last unapplied migration
dotnet ef migrations remove

# List all migrations and their status
dotnet ef migrations list
```

```cs
// Apply migrations programmatically at startup (dev/staging only)
var app = builder.Build();

using (var scope = app.Services.CreateScope())
{
    var db = scope.ServiceProvider.GetRequiredService<AppDbContext>();
    await db.Database.MigrateAsync(); // apply pending migrations
}

app.Run();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `AddTransient`, `AddScoped`, and `AddSingleton`?

| Lifetime | Instance created | Shared within | Use for |
|----------|-----------------|---------------|---------|
| `AddTransient` | Every injection | Never shared | Lightweight, stateless services |
| `AddScoped` | Once per HTTP request (scope) | Same request | DB contexts, unit-of-work |
| `AddSingleton` | Once per application lifetime | Everyone | Caches, config, expensive shared state |

```cs
public interface ICounter { int Next(); }
public class Counter : ICounter
{
    private int _count;
    public int Next() => ++_count;
}

// Register all three lifetimes (for demo)
builder.Services.AddTransient<ICounter, Counter>();   // change to test others

// Controller demonstrating behavior
[ApiController, Route("api/[controller]")]
public class DemoController(
    ICounter counter1,
    ICounter counter2) : ControllerBase
{
    [HttpGet]
    public IActionResult Get()
    {
        return Ok(new
        {
            Counter1 = counter1.Next(),
            Counter2 = counter2.Next(),
            // Transient: counter1 ≠ counter2 (different instances)
            // Scoped: counter1 == counter2 (same instance within request)
            // Singleton: counter1 == counter2, and increments across requests
        });
    }
}

// Scoped services must NOT be injected into Singletons (captive dependency)
// ❌ Singleton capturing Scoped → Scoped outlives its intended scope
builder.Services.AddSingleton<IBadSingleton, BadSingleton>(); // has IScoped inside — BAD
// ✅ Use IServiceScopeFactory inside a singleton to create scopes manually
public class SafeSingleton(IServiceScopeFactory scopeFactory)
{
    public async Task DoWorkAsync()
    {
        using var scope = scopeFactory.CreateScope();
        var scoped = scope.ServiceProvider.GetRequiredService<IScopedService>();
        await scoped.WorkAsync();
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a RESTful API in ASP.NET Core?

```cs
// Program.cs — .NET 10 minimal API (recommended for new APIs)
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddDbContext<AppDbContext>(opt =>
    opt.UseInMemoryDatabase("Products"));
builder.Services.AddOpenApi();

var app = builder.Build();
app.MapOpenApi();

// Route group — /api/products
var products = app.MapGroup("/api/products").WithTags("Products");

products.MapGet("/", async (AppDbContext db) =>
    await db.Products.ToListAsync());

products.MapGet("/{id:int}", async (int id, AppDbContext db) =>
    await db.Products.FindAsync(id) is Product p
        ? Results.Ok(p)
        : Results.NotFound());

products.MapPost("/", async (Product product, AppDbContext db) =>
{
    db.Products.Add(product);
    await db.SaveChangesAsync();
    return Results.Created($"/api/products/{product.Id}", product);
});

products.MapPut("/{id:int}", async (int id, Product updated, AppDbContext db) =>
{
    var product = await db.Products.FindAsync(id);
    if (product is null) return Results.NotFound();
    product.Name  = updated.Name;
    product.Price = updated.Price;
    await db.SaveChangesAsync();
    return Results.NoContent();
});

products.MapDelete("/{id:int}", async (int id, AppDbContext db) =>
{
    var product = await db.Products.FindAsync(id);
    if (product is null) return Results.NotFound();
    db.Products.Remove(product);
    await db.SaveChangesAsync();
    return Results.NoContent();
});

app.Run();

public class Product
{
    public int Id { get; set; }
    public required string Name { get; set; }
    public decimal Price { get; set; }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you read configuration values from `appsettings.json`?

```json
// appsettings.json
{
  "App": {
    "Name": "MyApi",
    "MaxPageSize": 100,
    "FeatureFlags": {
      "EnableNewUI": true
    }
  },
  "ConnectionStrings": {
    "Default": "Server=localhost;Database=MyDb;Trusted_Connection=true"
  }
}
```

```cs
// 1. Raw IConfiguration (simple, no type safety)
var name = builder.Configuration["App:Name"];
var conn = builder.Configuration.GetConnectionString("Default");
var maxPage = builder.Configuration.GetValue<int>("App:MaxPageSize", defaultValue: 50);

// 2. Strongly-typed options (recommended)
public class AppOptions
{
    public string Name { get; set; } = "";
    public int MaxPageSize { get; set; }
    public FeatureFlagsOptions FeatureFlags { get; set; } = new();
}
public class FeatureFlagsOptions { public bool EnableNewUI { get; set; } }

builder.Services.Configure<AppOptions>(builder.Configuration.GetSection("App"));

// Inject and use
public class MyService(IOptionsSnapshot<AppOptions> opts)
{
    // IOptions<T> — singleton, does not update on file change
    // IOptionsSnapshot<T> — scoped, updates per request if reloadOnChange: true
    // IOptionsMonitor<T> — singleton, updates in real-time via OnChange callback
    public string AppName => opts.Value.Name;
    public bool NewUI     => opts.Value.FeatureFlags.EnableNewUI;
}

// 3. Bind directly
var appOptions = builder.Configuration
    .GetSection("App")
    .Get<AppOptions>()!;

// 4. Validate options on startup (.NET 8+ AddOptionsWithValidateOnStart)
builder.Services
    .AddOptions<AppOptions>()
    .Bind(builder.Configuration.GetSection("App"))
    .ValidateDataAnnotations()   // use [Required], [Range] on the class
    .ValidateOnStart();          // fail fast if config is invalid

// 5. Environment variable overrides (: → __ in env vars)
// App__Name=Override  →  Config["App:Name"] = "Override"
// ASPNETCORE_ENVIRONMENT=Production
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use middleware in ASP.NET Core?

**Middleware** is software assembled into the request pipeline that handles requests and responses. Each component can short-circuit or pass to the next middleware via `next()`.

```cs
var app = builder.Build();

// Built-in middleware (order matters!)
app.UseExceptionHandler("/error");
app.UseHttpsRedirection();
app.UseStaticFiles();
app.UseRouting();
app.UseAuthentication();
app.UseAuthorization();
app.UseOutputCache();

// Inline middleware with app.Use
app.Use(async (context, next) =>
{
    var watch = System.Diagnostics.Stopwatch.StartNew();
    await next(context);  // call next middleware
    watch.Stop();
    var path = context.Request.Path;
    Console.WriteLine($"{context.Response.StatusCode} {path} — {watch.ElapsedMilliseconds}ms");
});

// Short-circuit middleware with app.Run (terminal — no next)
app.MapGet("/health", () => Results.Ok(new { Status = "Healthy" }));

// app.UseWhen — conditional branch
app.UseWhen(ctx => ctx.Request.Path.StartsWithSegments("/api"),
    branch => branch.Use(async (ctx, next) =>
    {
        ctx.Response.Headers["X-Api-Version"] = "v1";
        await next(ctx);
    }));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a custom middleware?

```cs
// 1. Class-based middleware (recommended — testable, DI-friendly)
public class RequestTimingMiddleware(RequestDelegate next,
    ILogger<RequestTimingMiddleware> logger)
{
    public async Task InvokeAsync(HttpContext context)
    {
        var sw = System.Diagnostics.Stopwatch.StartNew();

        await next(context);  // call rest of pipeline

        sw.Stop();
        logger.LogInformation("{Method} {Path} → {Status} in {Ms}ms",
            context.Request.Method,
            context.Request.Path,
            context.Response.StatusCode,
            sw.ElapsedMilliseconds);
    }
}

// Extension method for clean registration
public static class MiddlewareExtensions
{
    public static IApplicationBuilder UseRequestTiming(
        this IApplicationBuilder app) =>
        app.UseMiddleware<RequestTimingMiddleware>();
}

// Register in Program.cs
app.UseRequestTiming();

// 2. Middleware with scoped dependencies — use IMiddleware + factory pattern
public class AuditMiddleware(ILogger<AuditMiddleware> logger) : IMiddleware
{
    public async Task InvokeAsync(HttpContext context, RequestDelegate next)
    {
        logger.LogInformation("Audit: {Path}", context.Request.Path);
        await next(context);
    }
}

// Register as scoped (IMiddleware uses DI per request)
builder.Services.AddScoped<AuditMiddleware>();
app.UseMiddleware<AuditMiddleware>();

// 3. Exception-handling middleware example
public class GlobalExceptionMiddleware(RequestDelegate next, ILogger<GlobalExceptionMiddleware> logger)
{
    public async Task InvokeAsync(HttpContext context)
    {
        try
        {
            await next(context);
        }
        catch (Exception ex)
        {
            logger.LogError(ex, "Unhandled exception");
            context.Response.StatusCode = 500;
            context.Response.ContentType = "application/json";
            await context.Response.WriteAsJsonAsync(new
            {
                Error = "An unexpected error occurred.",
                TraceId = context.TraceIdentifier
            });
        }
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use dependency injection in controllers?

```cs
// Constructor injection (primary constructor syntax, C# 12 / .NET 8+)
[ApiController]
[Route("api/[controller]")]
public class OrdersController(
    IOrderService orderService,
    ILogger<OrdersController> logger) : ControllerBase
{
    [HttpGet]
    public async Task<IActionResult> GetAll()
    {
        var orders = await orderService.GetAllAsync();
        return Ok(orders);
    }

    [HttpGet("{id:int}")]
    public async Task<ActionResult<OrderDto>> GetById(int id)
    {
        var order = await orderService.GetByIdAsync(id);
        return order is null ? NotFound() : Ok(order);
    }

    [HttpPost]
    public async Task<IActionResult> Create(CreateOrderRequest request)
    {
        var order = await orderService.CreateAsync(request);
        logger.LogInformation("Order {OrderId} created", order.Id);
        return CreatedAtAction(nameof(GetById), new { id = order.Id }, order);
    }
}

// Inject from DI in action parameter — [FromServices]
[HttpGet("summary")]
public IActionResult GetSummary([FromServices] IReportService reportService)
    => Ok(reportService.GetSummary());

// Keyed services (.NET 8+)
[HttpPost("pay/stripe")]
public IActionResult PayWithStripe(
    [FromKeyedServices("stripe")] IPaymentGateway gateway,
    PaymentRequest request)
{
    gateway.Charge(request);
    return Ok();
}

// Register controller services
builder.Services.AddControllers();
builder.Services.AddScoped<IOrderService, OrderService>();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you return JSON from a controller action?

```cs
// 1. Minimal API — returns JSON automatically for objects
app.MapGet("/products", async (AppDbContext db) =>
    await db.Products.ToListAsync()); // serialized to JSON automatically

// 2. Controller — Ok() wraps object in 200 JSON response
[ApiController, Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    [HttpGet]
    public IActionResult GetAll() =>
        Ok(new[] { new { Id = 1, Name = "Laptop", Price = 999m } });

    // 3. Typed ActionResult<T>
    [HttpGet("{id:int}")]
    public ActionResult<ProductDto> GetById(int id)
    {
        var product = new ProductDto(id, "Laptop", 999m);
        return Ok(product); // 200 with JSON body
    }

    // 4. Custom JSON options for an action
    [HttpGet("formatted")]
    public IActionResult GetFormatted()
    {
        var data = new { Message = "Hello", Date = DateTime.UtcNow };
        return new JsonResult(data, new System.Text.Json.JsonSerializerOptions
        {
            WriteIndented = true,
            PropertyNamingPolicy = System.Text.Json.JsonNamingPolicy.CamelCase,
        });
    }
}

// 5. Configure global JSON options (.NET 10)
builder.Services.ConfigureHttpJsonOptions(opt =>
{
    opt.SerializerOptions.PropertyNamingPolicy = JsonNamingPolicy.CamelCase;
    opt.SerializerOptions.WriteIndented = false;
    opt.SerializerOptions.DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull;
});

// 6. Results.Json for minimal APIs with custom options
app.MapGet("/custom", () => Results.Json(
    new { Status = "ok" },
    new JsonSerializerOptions { WriteIndented = true }));

record ProductDto(int Id, string Name, decimal Price);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is attribute routing?

**Attribute routing** places route templates directly on controllers and actions using `[Route]`, `[HttpGet]`, `[HttpPost]`, etc. It gives fine-grained control over URL patterns and is the standard approach in ASP.NET Core Web APIs.

```cs
// Conventional routing (MVC) — defined globally in Program.cs
app.MapControllerRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Attribute routing — defined on the controller/action (preferred for APIs)
[ApiController]
[Route("api/v{version:apiVersion}/[controller]")] // token replacement
public class ProductsController : ControllerBase
{
    // GET api/v1/products
    [HttpGet]
    public IActionResult GetAll() => Ok();

    // GET api/v1/products/42
    [HttpGet("{id:int}")]
    public IActionResult GetById(int id) => Ok(id);

    // GET api/v1/products/sku/ABC-123
    [HttpGet("sku/{sku:regex(^[A-Z]{{3}}-\\d{{3}}$)}")]
    public IActionResult GetBySku(string sku) => Ok(sku);

    // POST api/v1/products
    [HttpPost]
    [ProducesResponseType<ProductDto>(StatusCodes.Status201Created)]
    [ProducesResponseType(StatusCodes.Status400BadRequest)]
    public IActionResult Create(CreateProductRequest request) => Created();

    // PUT api/v1/products/42
    [HttpPut("{id:int}")]
    public IActionResult Update(int id, UpdateProductRequest request) => NoContent();

    // DELETE api/v1/products/42
    [HttpDelete("{id:int}")]
    public IActionResult Delete(int id) => NoContent();

    // Multiple routes on one action
    [HttpGet("search")]
    [HttpGet("find")]           // both routes map here
    public IActionResult Search([FromQuery] string q) => Ok(q);
}

// Route constraints
// {id:int}           — integer only
// {name:alpha}       — letters only
// {code:length(5)}   — exactly 5 chars
// {date:datetime}    — valid datetime
// {price:decimal}    — decimal number
// {id:min(1)}        — minimum value
// {id:guid}          — GUID format
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a custom route?

```cs
// 1. Custom route constraint — restrict route parameter values
public class EvenNumberConstraint : IRouteConstraint
{
    public bool Match(HttpContext? context, IRouter? route, string routeKey,
        RouteValueDictionary values, RouteDirection routeDirection)
    {
        if (values.TryGetValue(routeKey, out var value)
            && int.TryParse(value?.ToString(), out int num))
            return num % 2 == 0;
        return false;
    }
}

// Register constraint
builder.Services.Configure<RouteOptions>(opt =>
    opt.ConstraintMap["even"] = typeof(EvenNumberConstraint));

// Use in route template
app.MapGet("/items/{id:even}", (int id) => $"Even item {id}");
// /items/2  → matches
// /items/3  → 404

// 2. Custom route in controller
[HttpGet("reports/{year:int:min(2000)}/{month:int:range(1,12)}")]
public IActionResult GetMonthlyReport(int year, int month) =>
    Ok(new { Year = year, Month = month });

// 3. Minimal API with complex route pattern
app.MapGet("/files/{**path}", (string path) => $"Requested: {path}");
// Catch-all: /files/docs/2026/report.pdf

// 4. Route groups with shared prefix and metadata
var v2 = app.MapGroup("/api/v2")
    .RequireAuthorization()
    .WithOpenApi()
    .AddEndpointFilter<ValidationFilter>();

v2.MapGet("/products", () => Results.Ok());
v2.MapPost("/products", (Product p) => Results.Created($"/api/v2/products/{p.Id}", p));

// 5. Custom route transformer (slug-case URLs)
public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string? TransformOutbound(object? value) =>
        value?.ToString() is string s
            ? System.Text.RegularExpressions.Regex.Replace(s, "([a-z])([A-Z])", "$1-$2").ToLower()
            : null;
}

builder.Services.Configure<RouteOptions>(opt =>
    opt.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle form submissions in ASP.NET Core?

```cs
// 1. Minimal API — [FromForm] with IFormCollection
app.MapPost("/contact", async (IFormCollection form) =>
{
    var name    = form["name"].ToString();
    var email   = form["email"].ToString();
    var message = form["message"].ToString();
    return Results.Ok(new { name, email });
}).DisableAntiforgery(); // disable for API endpoints; enable for MVC forms

// 2. Strongly-typed form model
app.MapPost("/signup", async ([FromForm] SignupRequest req) =>
{
    Console.WriteLine($"Signup: {req.Name}, {req.Email}");
    return Results.Redirect("/welcome");
}).DisableAntiforgery();

public record SignupRequest([FromForm] string Name, [FromForm] string Email);

// 3. File upload via IFormFile
app.MapPost("/upload", async (IFormFile file) =>
{
    if (file.Length > 10 * 1024 * 1024)
        return Results.BadRequest("File too large (max 10 MB)");

    var ext = Path.GetExtension(file.FileName).ToLower();
    if (ext is not ".jpg" and not ".png" and not ".pdf")
        return Results.BadRequest("Invalid file type");

    var path = Path.Combine("uploads", Guid.NewGuid() + ext);
    await using var stream = File.Create(path);
    await file.CopyToAsync(stream);
    return Results.Ok(new { FilePath = path });
}).DisableAntiforgery();

// 4. MVC controller form handling with anti-forgery
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Create([FromForm] ProductFormModel model)
{
    if (!ModelState.IsValid)
        return View(model);

    await _service.CreateAsync(model);
    return RedirectToAction(nameof(Index));
}

// 5. Razor form with anti-forgery token
// @using Microsoft.AspNetCore.Mvc.Rendering
// <form method="post" action="/contact">
//   @Html.AntiForgeryToken()
//   <input name="name" />
//   <button type="submit">Submit</button>
// </form>
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is model binding?

**Model binding** is the process by which ASP.NET Core automatically maps HTTP request data (route values, query strings, form fields, JSON body, headers) to action method parameters.

```cs
// Sources (in binding order by default):
// 1. [FromRoute]   — /products/42  → id = 42
// 2. [FromQuery]   — ?page=2       → page = 2
// 3. [FromBody]    — JSON body     → complex object
// 4. [FromForm]    — form data
// 5. [FromHeader]  — request header
// 6. [FromServices]— DI container

[ApiController, Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    // Route + query binding — automatic
    [HttpGet("{id:int}")]
    public IActionResult Get(
        int id,                           // [FromRoute] implied
        [FromQuery] string? currency,     // ?currency=GBP
        [FromHeader(Name = "X-Tenant")] string? tenant) // from header
    {
        return Ok(new { id, currency, tenant });
    }

    // Body binding — JSON deserialized automatically ([ApiController] implies [FromBody])
    [HttpPost]
    public IActionResult Create(CreateProductRequest request) => Ok(request);

    // Mixed binding
    [HttpPut("{id:int}")]
    public IActionResult Update(
        [FromRoute] int id,
        [FromBody]  UpdateProductRequest body,
        [FromQuery] bool notify = false) => NoContent();
}

// Custom model binder — e.g., CSV to List<int>
public class CsvIntListBinder : IModelBinder
{
    public Task BindModelAsync(ModelBindingContext context)
    {
        var raw = context.ValueProvider.GetValue(context.ModelName).FirstValue;
        if (string.IsNullOrEmpty(raw))
        {
            context.Result = ModelBindingResult.Success(new List<int>());
            return Task.CompletedTask;
        }
        var list = raw.Split(',')
            .Select(s => int.TryParse(s.Trim(), out int n) ? (int?)n : null)
            .Where(n => n.HasValue).Select(n => n!.Value).ToList();
        context.Result = ModelBindingResult.Success(list);
        return Task.CompletedTask;
    }
}

// Use custom binder
[HttpGet("batch")]
public IActionResult GetBatch(
    [ModelBinder(typeof(CsvIntListBinder))] List<int> ids)
    => Ok(ids); // GET /batch?ids=1,2,3,4

record CreateProductRequest(string Name, decimal Price);
record UpdateProductRequest(string Name, decimal Price);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you validate models in ASP.NET Core?

```cs
// 1. Data annotations on model (automatic with [ApiController])
public class CreateProductRequest
{
    [Required(ErrorMessage = "Name is required")]
    [StringLength(100, MinimumLength = 2)]
    public string Name { get; set; } = "";

    [Range(0.01, 999999.99, ErrorMessage = "Price must be between 0.01 and 999,999.99")]
    public decimal Price { get; set; }

    [RegularExpression(@"^[A-Z]{3}$", ErrorMessage = "Currency must be 3 uppercase letters")]
    public string Currency { get; set; } = "GBP";
}

// [ApiController] automatically returns 400 if ModelState is invalid
[HttpPost]
public IActionResult Create(CreateProductRequest request)
{
    // ModelState.IsValid is guaranteed true here (ApiController handles it)
    return Ok(request);
}

// 2. Manual ModelState check (MVC controllers without [ApiController])
[HttpPost]
public IActionResult CreateMvc(CreateProductRequest request)
{
    if (!ModelState.IsValid)
        return BadRequest(ModelState);
    return Ok();
}

// 3. Custom validation attribute
public class FutureDateAttribute : ValidationAttribute
{
    protected override ValidationResult? IsValid(object? value, ValidationContext ctx)
    {
        if (value is DateTime date && date > DateTime.UtcNow)
            return ValidationResult.Success;
        return new ValidationResult("Date must be in the future");
    }
}

// 4. IValidatableObject — cross-property validation
public class OrderRequest : IValidatableObject
{
    public DateOnly StartDate { get; set; }
    public DateOnly EndDate { get; set; }

    public IEnumerable<ValidationResult> Validate(ValidationContext context)
    {
        if (EndDate <= StartDate)
            yield return new ValidationResult(
                "End date must be after start date",
                [nameof(EndDate)]);
    }
}

// 5. FluentValidation (popular alternative, .NET 10)
// dotnet add package FluentValidation.AspNetCore
public class ProductValidator : AbstractValidator<CreateProductRequest>
{
    public ProductValidator()
    {
        RuleFor(x => x.Name).NotEmpty().Length(2, 100);
        RuleFor(x => x.Price).GreaterThan(0).LessThan(1_000_000);
        RuleFor(x => x.Currency).Matches(@"^[A-Z]{3}$");
    }
}

builder.Services.AddValidatorsFromAssemblyContaining<ProductValidator>();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `ModelState` property?

`ModelState` is a `ModelStateDictionary` available on `ControllerBase` that contains the state of model binding and validation for the current request. It holds errors for each property and a boolean `IsValid` flag.

```cs
[HttpPost]
public IActionResult Create(CreateProductRequest request)
{
    // Check validity
    if (!ModelState.IsValid)
    {
        // Collect all errors
        var errors = ModelState
            .Where(e => e.Value?.Errors.Count > 0)
            .ToDictionary(
                e => e.Key,
                e => e.Value!.Errors.Select(err => err.ErrorMessage).ToArray()
            );
        return BadRequest(new { Errors = errors });
    }

    return Ok(request);
}

// Add errors manually
[HttpPost("custom")]
public IActionResult CustomValidation(CreateProductRequest request)
{
    if (request.Name == "forbidden")
        ModelState.AddModelError(nameof(request.Name), "This name is not allowed");

    if (!ModelState.IsValid)
        return ValidationProblem(ModelState); // RFC 7807 Problem Details format

    return Ok();
}

// ValidationProblem — returns standardised ProblemDetails (RFC 7807)
// {
//   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",
//   "title": "One or more validation errors occurred.",
//   "status": 400,
//   "errors": { "Name": ["Name is required"] }
// }

// [ApiController] automatically calls ValidationProblem when ModelState is invalid
// — you don\'t need to check it manually for [ApiController] controllers
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use data annotations for validation?

```cs
using System.ComponentModel.DataAnnotations;

public class UserRegistration
{
    [Required(ErrorMessage = "Username is required")]
    [StringLength(50, MinimumLength = 3, ErrorMessage = "Username must be 3–50 characters")]
    public string Username { get; set; } = "";

    [Required]
    [EmailAddress(ErrorMessage = "Invalid email address")]
    public string Email { get; set; } = "";

    [Required]
    [MinLength(8, ErrorMessage = "Password must be at least 8 characters")]
    [RegularExpression(@"^(?=.*[A-Z])(?=.*\d).+$",
        ErrorMessage = "Password must contain an uppercase letter and a digit")]
    public string Password { get; set; } = "";

    [Compare(nameof(Password), ErrorMessage = "Passwords do not match")]
    public string ConfirmPassword { get; set; } = "";

    [Range(18, 120, ErrorMessage = "Age must be between 18 and 120")]
    public int Age { get; set; }

    [Url(ErrorMessage = "Invalid URL")]
    public string? Website { get; set; }

    [Phone(ErrorMessage = "Invalid phone number")]
    public string? Phone { get; set; }

    [DataType(DataType.Date)]
    public DateTime BirthDate { get; set; }

    [CreditCard(ErrorMessage = "Invalid credit card number")]
    public string? CardNumber { get; set; }
}

// Manual validation (outside ASP.NET pipeline)
var user = new UserRegistration { Username = "a", Email = "bad", Age = 15 };
var context = new ValidationContext(user);
var results = new List<ValidationResult>();

bool isValid = Validator.TryValidateObject(user, context, results, validateAllProperties: true);

if (!isValid)
    foreach (var r in results)
        Console.WriteLine($"{string.Join(", ", r.MemberNames)}: {r.ErrorMessage}");

// All common annotations:
// [Required]           — not null/empty
// [StringLength(n)]    — max (and optional min) length
// [MinLength(n)]       — min collection/string length
// [MaxLength(n)]       — max collection/string length
// [Range(min, max)]    — numeric/date range
// [RegularExpression]  — regex pattern
// [EmailAddress]       — email format
// [Url]                — URL format
// [Phone]              — phone format
// [Compare("Prop")]    — must equal another property
// [CreditCard]         — credit card format
// [EnumDataType]       — valid enum value
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `ViewResult` class?

`ViewResult` is an `ActionResult` that renders a **Razor view** (`.cshtml` file) as the HTTP response. It is returned by MVC controller actions that produce HTML.

```cs
// Controller returning a view
public class ProductsController : Controller  // Controller, not ControllerBase
{
    // View() returns ViewResult — renders Views/Products/Index.cshtml
    public IActionResult Index()
    {
        var products = new List<ProductViewModel>
        {
            new(1, "Laptop",  999m),
            new(2, "Monitor", 450m),
        };
        return View(products);              // passes model to the view
    }

    // Specify view name explicitly
    public IActionResult Details(int id)
    {
        var product = new ProductViewModel(id, "Laptop", 999m);
        return View("ProductDetails", product); // renders ProductDetails.cshtml
    }

    // ViewResult properties
    public IActionResult WithViewData()
    {
        ViewData["PageTitle"] = "Products";    // dynamic view data
        ViewBag.Count = 5;                     // dynamic property syntax

        var result = new ViewResult
        {
            ViewName = "Index",
            ViewData = ViewData,              // includes Model + ViewData
            StatusCode = 200,
        };
        return result;
    }
}

// Views/Products/Index.cshtml
@model List<ProductViewModel>
@{
    ViewData["Title"] = "Products";
}
<h1>Products (@Model.Count)</h1>
<ul>
    @foreach (var p in Model)
    {
        <li>@p.Name — @p.Price.ToString("C")</li>
    }
</ul>

record ProductViewModel(int Id, string Name, decimal Price);
```

**`ViewResult` vs other results:**

| Return | Use |
|--------|-----|
| `View()` | Render Razor `.cshtml` |
| `PartialView()` | Render partial view |
| `Json()` | Return JSON |
| `Redirect()` | HTTP 302 redirect |
| `File()` | Return file download |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you return a view from a controller action?

```cs
public class HomeController : Controller
{
    // 1. Default — renders Views/Home/Index.cshtml
    public IActionResult Index() => View();

    // 2. With model
    public IActionResult Products()
    {
        var model = new List<string> { "Laptop", "Mouse", "Monitor" };
        return View(model);
    }

    // 3. Explicit view name
    public IActionResult About() => View("AboutUs"); // Views/Home/AboutUs.cshtml

    // 4. View in different folder
    public IActionResult Shared() => View("~/Views/Shared/Info.cshtml");

    // 5. With ViewData / ViewBag
    public IActionResult Dashboard()
    {
        ViewData["Title"] = "Dashboard";
        ViewBag.UserId = 42;
        return View(new DashboardModel());
    }

    // 6. Conditional view
    public IActionResult Profile(int id)
    {
        var user = GetUser(id);
        if (user is null) return NotFound();
        return user.IsAdmin
            ? View("AdminProfile", user)
            : View("UserProfile", user);
    }
}
```

```cshtml
@* Views/Home/Products.cshtml *@
@model List<string>
@{
    ViewData["Title"] = "Products";
    Layout = "_Layout";      // use shared layout
}

<h1>@ViewData["Title"]</h1>
<ul>
    @foreach (var item in Model)
    {
        <li>@item</li>
    }
</ul>
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Razor syntax?

**Razor** is ASP.NET Core\'s server-side templating syntax that mixes C# and HTML using the `@` symbol as the transition character. Razor files have the `.cshtml` extension.

```cshtml
@* This is a Razor comment *@

@* Declare model type *@
@model List<Product>

@* Code block *@
@{
    ViewData["Title"] = "Product List";
    var count = Model.Count;
    string cssClass = count > 10 ? "many" : "few";
}

@* Inline expression — renders value *@
<h1>Products (@count)</h1>
<p class="@cssClass">Showing @Model.Count items</p>

@* Control flow *@
@if (Model.Any())
{
    <ul>
        @foreach (var product in Model)
        {
            <li>
                <strong>@product.Name</strong> — @product.Price.ToString("C")
                @if (product.Price > 500)
                {
                    <span class="badge">Premium</span>
                }
            </li>
        }
    </ul>
}
else
{
    <p>No products found.</p>
}

@* Explicit expression (multi-token) — use parentheses *@
<p>Tax: @(Model.Sum(p => p.Price) * 0.20m)</p>

@* Render raw HTML (avoid unless trusted content) *@
@Html.Raw("<strong>bold</strong>")

@* Tag helpers (preferred over HTML helpers) *@
<a asp-controller="Products" asp-action="Details" asp-route-id="@product.Id">
    View Details
</a>

@* Partial view *@
@await Html.PartialAsync("_ProductCard", product)

@* Section — inject content into layout *@
@section Scripts {
    <script src="~/js/products.js"></script>
}
```

```cshtml
@* Views/Shared/_Layout.cshtml — master layout *@
<!DOCTYPE html>
<html>
<head><title>@ViewData["Title"]</title></head>
<body>
    <nav>@await Html.PartialAsync("_Nav")</nav>
    @RenderBody()
    @await RenderSectionAsync("Scripts", required: false)
</body>
</html>
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a Razor view?

```bash
# 1. Create via dotnet CLI
dotnet new page -n Index -na MyApp.Pages      # Razor Page
# For MVC views — create .cshtml manually (no template)
```

```cshtml
@* Views/Products/Create.cshtml — MVC Razor view *@
@model CreateProductRequest
@{
    ViewData["Title"] = "Create Product";
    Layout = "~/Views/Shared/_Layout.cshtml";
}

<h2>Create Product</h2>

<form asp-action="Create" asp-controller="Products" method="post">
    @Html.AntiForgeryToken()

    <div asp-validation-summary="ModelOnly" class="text-danger"></div>

    <div class="form-group">
        <label asp-for="Name"></label>
        <input asp-for="Name" class="form-control" />
        <span asp-validation-for="Name" class="text-danger"></span>
    </div>

    <div class="form-group">
        <label asp-for="Price"></label>
        <input asp-for="Price" class="form-control" type="number" step="0.01" />
        <span asp-validation-for="Price" class="text-danger"></span>
    </div>

    <button type="submit" class="btn btn-primary">Create</button>
    <a asp-action="Index">Cancel</a>
</form>

@section Scripts {
    @{ await Html.RenderPartialAsync("_ValidationScriptsPartial"); }
}
```

```cs
// Controller action that returns this view
[HttpGet]
public IActionResult Create() => View();

[HttpPost, ValidateAntiForgeryToken]
public async Task<IActionResult> Create(CreateProductRequest model)
{
    if (!ModelState.IsValid) return View(model);
    await _service.CreateAsync(model);
    return RedirectToAction(nameof(Index));
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use partial views in ASP.NET Core?

**Partial views** are reusable `.cshtml` fragments rendered inside other views. They do not have a layout and are ideal for repeating UI components.

```cshtml
@* Views/Shared/_ProductCard.cshtml — partial view *@
@model Product

<div class="card">
    <div class="card-body">
        <h5 class="card-title">@Model.Name</h5>
        <p class="card-text">@Model.Price.ToString("C")</p>
        <a asp-action="Details" asp-route-id="@Model.Id"
           class="btn btn-primary">View</a>
    </div>
</div>
```

```cshtml
@* Parent view — render partial *@
@model List<Product>

@* 1. Tag helper (preferred, .NET Core) *@
@foreach (var product in Model)
{
    <partial name="_ProductCard" model="product" />
}

@* 2. await Html.PartialAsync (async, preferred in code) *@
@foreach (var product in Model)
{
    @await Html.PartialAsync("_ProductCard", product)
}

@* 3. Pass ViewData to partial *@
@await Html.PartialAsync("_ProductCard", product,
    new ViewDataDictionary(ViewData) { { "ShowBadge", true } })
```

```cs
// Return partial from controller (for AJAX requests)
[HttpGet("card/{id:int}")]
public async Task<IActionResult> GetCard(int id)
{
    var product = await _repo.GetByIdAsync(id);
    return PartialView("_ProductCard", product);
}

// Fetch partial via JavaScript (HTMX or fetch)
// fetch('/products/card/42').then(r => r.text()).then(html => div.innerHTML = html)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a view component?

A **View Component** is like a mini-controller + partial view, suitable for complex reusable UI sections (e.g., shopping cart, navigation menu, notification badge) that require business logic or dependency injection.

```cs
// 1. View Component class
using Microsoft.AspNetCore.Mvc;

public class CartSummaryViewComponent(ICartService cartService) : ViewComponent
{
    public async Task<IViewComponentResult> InvokeAsync()
    {
        var userId = HttpContext.User.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier)?.Value;
        var cart   = await cartService.GetCartAsync(userId ?? "guest");
        return View(cart); // renders Views/Shared/Components/CartSummary/Default.cshtml
    }
}

// ICartService
public interface ICartService
{
    Task<CartViewModel> GetCartAsync(string userId);
}
public record CartViewModel(int ItemCount, decimal Total);
```

```cshtml
@* Views/Shared/Components/CartSummary/Default.cshtml *@
@model CartViewModel

<div class="cart-badge">
    <span class="icon">🛒</span>
    <span class="count">@Model.ItemCount</span>
    <span class="total">@Model.Total.ToString("C")</span>
</div>
```

```cshtml
@* Use in any view *@
@* Tag helper syntax (recommended) *@
<vc:cart-summary></vc:cart-summary>

@* Method syntax *@
@await Component.InvokeAsync("CartSummary")

@* With parameters *@
<vc:recent-products count="5" category="Electronics"></vc:recent-products>
```

```cs
// View component with parameters
public class RecentProductsViewComponent(IProductRepository repo) : ViewComponent
{
    public async Task<IViewComponentResult> InvokeAsync(int count = 4, string? category = null)
    {
        var products = await repo.GetRecentAsync(count, category);
        return View(products);
    }
}

// Register services
builder.Services.AddScoped<ICartService, CartService>();
builder.Services.AddScoped<IProductRepository, ProductRepository>();
// View components are discovered automatically — no explicit registration needed
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a custom tag helper?

**Tag helpers** are C# classes that target HTML elements and transform them server-side. They are the Razor replacement for HTML helpers.

```cs
// 1. Simple custom tag helper — <alert type="success">message</alert>
using Microsoft.AspNetCore.Razor.TagHelpers;

[HtmlTargetElement("alert")]
public class AlertTagHelper : TagHelper
{
    public string Type { get; set; } = "info"; // maps to type attribute

    public override async Task ProcessAsync(TagHelperContext context, TagHelperOutput output)
    {
        var content = await output.GetChildContentAsync();

        output.TagName = "div";
        output.Attributes.SetAttribute("class", $"alert alert-{Type} alert-dismissible");
        output.Attributes.SetAttribute("role", "alert");
        output.Content.SetHtmlContent($"""
            {content.GetContent()}
            <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
            """);
    }
}

// Usage in Razor: <alert type="warning">Watch out!</alert>
// Renders: <div class="alert alert-warning alert-dismissible" role="alert">
//            Watch out!
//            <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
//          </div>

// 2. Tag helper targeting existing element — format bytes
[HtmlTargetElement("span", Attributes = "file-size")]
public class FileSizeTagHelper : TagHelper
{
    [HtmlAttributeName("file-size")]
    public long FileSizeBytes { get; set; }

    public override void Process(TagHelperContext context, TagHelperOutput output)
    {
        output.Attributes.RemoveAll("file-size");
        string formatted = FileSizeBytes switch
        {
            < 1024               => $"{FileSizeBytes} B",
            < 1024 * 1024        => $"{FileSizeBytes / 1024.0:F1} KB",
            < 1024 * 1024 * 1024 => $"{FileSizeBytes / (1024.0 * 1024):F1} MB",
            _                    => $"{FileSizeBytes / (1024.0 * 1024 * 1024):F1} GB",
        };
        output.Content.SetContent(formatted);
    }
}

// Usage: <span file-size="1536000"></span>  →  <span>1.5 MB</span>

// 3. Register tag helpers in _ViewImports.cshtml
// @addTagHelper *, MyApp        — all tag helpers in MyApp assembly
// @addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers  — built-in
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 18. MISCELLANEOUS

<br/>

## Q. What is NuGet?

**NuGet** is the official package manager for .NET. It enables developers to create, share, and consume reusable libraries and tools. NuGet packages are ZIP files with the `.nupkg` extension containing compiled code (DLLs), related files, and a manifest that describes the package.

**Key features:**
- Centrally hosted on [nuget.org](https://www.nuget.org) (public) or private feeds (Azure Artifacts, GitHub Packages, etc.)
- Integrated into `dotnet` CLI, Visual Studio, and MSBuild
- Handles transitive dependencies automatically

```bash
# Install a package
dotnet add package Newtonsoft.Json

# Install a specific version
dotnet add package Microsoft.EntityFrameworkCore --version 9.0.0

# Remove a package
dotnet remove package Newtonsoft.Json

# Restore all packages
dotnet restore

# List outdated packages
dotnet list package --outdated

# Search NuGet
dotnet package search Serilog
```

**In .csproj (central package management, .NET 10):**

```xml
<!-- Directory.Packages.props — define versions once for the entire repo -->
<Project>
  <PropertyGroup>
    <ManagePackageVersionsCentrally>true</ManagePackageVersionsCentrally>
  </PropertyGroup>
  <ItemGroup>
    <PackageVersion Include="Serilog" Version="4.2.0" />
    <PackageVersion Include="System.Text.Json" Version="10.0.0" />
  </ItemGroup>
</Project>
```

```xml
<!-- Individual .csproj — just reference, no version needed -->
<ItemGroup>
  <PackageReference Include="Serilog" />
</ItemGroup>
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between ToString() and Convert.ToString()?

| Feature | `obj.ToString()` | `Convert.ToString(obj)` |
|---------|-----------------|------------------------|
| Null handling | Throws `NullReferenceException` | Returns `""` (empty string) |
| Defined on | `object` | `System.Convert` static class |
| Works on | Any object | Any base type + nullable |
| Overridable | ✅ Yes | ❌ No (calls ToString internally) |

```cs
string? s = null;

// ToString() — throws NullReferenceException on null
try
{
    string result = s!.ToString(); // ❌ NullReferenceException
}
catch (NullReferenceException ex)
{
    Console.WriteLine($"Exception: {ex.Message}");
}

// Convert.ToString() — safe on null
string safe = Convert.ToString(s)!; // returns ""
Console.WriteLine($"Result: '{safe}'"); // Result: ''

// With value types — same result
int number = 42;
Console.WriteLine(number.ToString());          // "42"
Console.WriteLine(Convert.ToString(number));   // "42"

// Custom formatting — prefer ToString(format)
double pi = Math.PI;
Console.WriteLine(pi.ToString("F2"));          // "3.14"
Console.WriteLine(Convert.ToString(pi));       // "3.141592653589793"
```

**Best practice:** Use `ToString()` for non-nullable types. Use `Convert.ToString()` or null-conditional `?.ToString() ?? ""` when the object may be null.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between int.Parse() and Convert.ToInt32()?

| Feature | `int.Parse(s)` | `Convert.ToInt32(s)` |
|---------|---------------|---------------------|
| Input type | `string` only | Any base type (string, double, bool, etc.) |
| Null input | Throws `ArgumentNullException` | Returns `0` |
| Empty string | Throws `FormatException` | Throws `FormatException` |
| Invalid format | Throws `FormatException` | Throws `FormatException` |
| Performance | Slightly faster (no boxing) | Slightly slower |

```cs
// int.Parse — string only
Console.WriteLine(int.Parse("42"));     // 42
Console.WriteLine(int.Parse("-10"));    // -10

try { int.Parse(null!); }               // ❌ ArgumentNullException
catch (ArgumentNullException) { Console.WriteLine("null throws!"); }

// Convert.ToInt32 — handles null
Console.WriteLine(Convert.ToInt32(null)); // 0 (no exception)
Console.WriteLine(Convert.ToInt32(3.9));  // 4 (rounds!)
Console.WriteLine(Convert.ToInt32(true)); // 1
Console.WriteLine(Convert.ToInt32(false));// 0

// Best practice — TryParse for user input (no exceptions)
string input = "abc";
if (int.TryParse(input, out int value))
    Console.WriteLine($"Parsed: {value}");
else
    Console.WriteLine("Invalid input");

// .NET 7+ — TryParse with generic NumberStyles
if (int.TryParse("FF", System.Globalization.NumberStyles.HexNumber,
    null, out int hex))
    Console.WriteLine(hex); // 255
```

**Rule:** Use `int.TryParse()` for user input. Use `int.Parse()` only when you are certain the string is a valid integer. Use `Convert.ToInt32()` when the source may be non-string types.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the use of Code Snippets?

**Code snippets** are predefined reusable templates for commonly typed code patterns. In Visual Studio, typing a shortcut and pressing **Tab twice** expands the snippet.

**Built-in C# snippets:**

| Shortcut | Expands to |
|---------|-----------|
| `cw` | `Console.WriteLine()` |
| `for` | `for` loop |
| `foreach` | `foreach` loop |
| `if` | `if` statement |
| `ctor` | Constructor |
| `prop` | Auto-property |
| `propg` | Get-only property |
| `try` | `try/catch` block |
| `switch` | `switch` statement |
| `class` | Class definition |
| `interface` | Interface definition |

```cs
// Typing 'prop' + Tab + Tab generates:
public int MyProperty { get; set; }

// Typing 'ctor' + Tab + Tab inside a class generates:
public ClassName()
{
}

// Typing 'foreach' + Tab + Tab generates:
foreach (var item in collection)
{
}
```

**Custom snippet file (.snippet XML):**

```xml
<?xml version="1.0" encoding="utf-8"?>
<CodeSnippets xmlns="http://schemas.microsoft.com/VisualStudio/2005/CodeSnippet">
  <CodeSnippet Format="1.0.0">
    <Header>
      <Title>Guard Clause</Title>
      <Shortcut>guard</Shortcut>
    </Header>
    <Snippet>
      <Code Language="CSharp">
        <![CDATA[ArgumentNullException.ThrowIfNull($param$);]]>
      </Code>
      <Declarations>
        <Literal><ID>param</ID><Default>value</Default></Literal>
      </Declarations>
    </Snippet>
  </CodeSnippet>
</CodeSnippets>
```

Import via **Tools → Code Snippets Manager → Import**.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Write a program to get the range of Byte Datatype?

```cs
// Byte (byte) — unsigned 8-bit integer
Console.WriteLine($"byte Min: {byte.MinValue}");   // 0
Console.WriteLine($"byte Max: {byte.MaxValue}");   // 255
Console.WriteLine($"byte Size: {sizeof(byte)} byte(s)");

// Signed byte (sbyte) — signed 8-bit integer
Console.WriteLine($"sbyte Min: {sbyte.MinValue}"); // -128
Console.WriteLine($"sbyte Max: {sbyte.MaxValue}"); // 127

// All numeric type ranges (.NET 10)
var types = new (string Name, long Min, ulong Max)[]
{
    ("byte",   byte.MinValue,   byte.MaxValue),
    ("sbyte",  sbyte.MinValue,  (ulong)sbyte.MaxValue),
    ("short",  short.MinValue,  (ulong)short.MaxValue),
    ("ushort", ushort.MinValue, ushort.MaxValue),
    ("int",    int.MinValue,    (ulong)int.MaxValue),
    ("uint",   uint.MinValue,   uint.MaxValue),
    ("long",   long.MinValue,   (ulong)long.MaxValue),
};

Console.WriteLine($"{"Type",-8} {"Min",22} {"Max",22}");
Console.WriteLine(new string('-', 55));
foreach (var (name, min, max) in types)
    Console.WriteLine($"{name,-8} {min,22} {max,22}");

// Overflow behavior
byte b = byte.MaxValue; // 255
b++;                    // overflow — wraps to 0
Console.WriteLine(b);   // 0

// Checked context — throws OverflowException
try
{
    byte c = checked((byte)(byte.MaxValue + 1));
}
catch (OverflowException)
{
    Console.WriteLine("Overflow caught!");
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are attributes in C# and how can they be used?

**Attributes** are metadata decorators applied to types, methods, properties, and assemblies using `[AttributeName]` syntax. They are inspected at runtime via reflection or at compile time by source generators/analyzers.

```cs
// 1. Built-in attributes
[Obsolete("Use NewMethod() instead", error: false)]
public void OldMethod() { }

[Serializable]
public class LegacyData { public int Value; }

// 2. Validation with [Required], [Range] (System.ComponentModel.DataAnnotations)
using System.ComponentModel.DataAnnotations;

public class Product
{
    [Required]
    [StringLength(100, MinimumLength = 2)]
    public string Name { get; set; } = "";

    [Range(0.01, 99999.99)]
    public decimal Price { get; set; }
}

// Validate manually
var p = new Product { Name = "", Price = -5 };
var context = new ValidationContext(p);
var results = new List<ValidationResult>();
bool valid = Validator.TryValidateObject(p, context, results, true);
foreach (var r in results)
    Console.WriteLine(r.ErrorMessage);

// 3. Custom attribute
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method,
    AllowMultiple = false, Inherited = true)]
public sealed class AuditAttribute(string author) : Attribute
{
    public string Author { get; } = author;
    public DateTime CreatedAt { get; } = DateTime.UtcNow;
}

[Audit("Alice")]
public class OrderService
{
    [Audit("Bob")]
    public void ProcessOrder() { }
}

// 4. Read attributes via reflection
var attr = typeof(OrderService)
    .GetCustomAttribute<AuditAttribute>();
Console.WriteLine(attr?.Author); // Alice

// 5. Conditional compilation attribute
[System.Diagnostics.Conditional("DEBUG")]
public static void DebugLog(string msg) =>
    Console.WriteLine($"[DEBUG] {msg}");

// Called only in DEBUG builds; no-op in Release
DebugLog("Processing started");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Why can\'t you specify the accessibility modifier for methods inside the interface?

In C#, **all interface members are implicitly `public`** by default, because an interface defines a contract — a promise that any implementing type exposes those members publicly. Restricting access would make the contract meaningless.

```cs
public interface IAnimal
{
    void Speak();     // implicitly public
    // private void Speak(); // ❌ Compile error — cannot be private
    // protected void Speak(); // ❌ Compile error
}

public class Dog : IAnimal
{
    public void Speak() => Console.WriteLine("Woof!"); // must be public
}
```

**Exception — C# 8+ Default Interface Methods (DIM):**

Since C# 8, interfaces *can* have `private` members, but only as helpers for default implementations:

```cs
public interface ILogger
{
    void Log(string message);

    // private helper — only usable inside the interface body
    private static string Format(string msg) => $"[{DateTime.UtcNow:u}] {msg}";

    // default implementation uses the private helper
    void LogInfo(string message) => Log(Format(message));
}

public class ConsoleLogger : ILogger
{
    public void Log(string message) => Console.WriteLine(message);
    // LogInfo is inherited from the interface default implementation
}

var logger = new ConsoleLogger();
logger.LogInfo("Application started");
// [2026-04-19 12:00:00Z] Application started
```

**Summary:** Interface members are public by design (the contract must be accessible). Only `private` and `static` members added in C# 8+ for default implementation support are allowed to restrict visibility — and they are internal to the interface body only.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement a custom IComparer<T> for sorting in C#?

`IComparer<T>` provides a `Compare(T x, T y)` method that returns negative (x < y), zero (x == y), or positive (x > y). Implement it when the default ordering doesn\'t fit your requirements.

```cs
record Product(string Name, decimal Price, int Stock);

// 1. Implement IComparer<T> as a class
public class ProductByPriceDescending : IComparer<Product>
{
    public int Compare(Product? x, Product? y)
    {
        if (x is null && y is null) return 0;
        if (x is null) return 1;
        if (y is null) return -1;
        return y.Price.CompareTo(x.Price); // descending
    }
}

var products = new List<Product>
{
    new("Laptop",  999m, 10),
    new("Mouse",    25m, 200),
    new("Monitor", 450m, 50),
};

products.Sort(new ProductByPriceDescending());
foreach (var p in products)
    Console.WriteLine($"{p.Name}: {p.Price:C}");
// Laptop: £999.00, Monitor: £450.00, Mouse: £25.00

// 2. Comparer<T>.Create — inline lambda (preferred for simple cases)
var byStockAsc = Comparer<Product>.Create((a, b) => a.Stock.CompareTo(b.Stock));
products.Sort(byStockAsc);
foreach (var p in products)
    Console.WriteLine($"{p.Name}: {p.Stock} units");

// 3. Multi-key sort — name length then alphabetical
var multiKey = Comparer<Product>.Create((a, b) =>
{
    int byLen = a.Name.Length.CompareTo(b.Name.Length);
    return byLen != 0 ? byLen : string.Compare(a.Name, b.Name, StringComparison.Ordinal);
});
products.Sort(multiKey);

// 4. Use with SortedSet<T>
var sortedSet = new SortedSet<Product>(new ProductByPriceDescending())
{
    new("Keyboard", 75m, 150),
    new("Webcam",  120m, 80),
};
foreach (var p in sortedSet)
    Console.WriteLine(p.Name);

// 5. LINQ OrderBy with IComparer<T>
var orderedByName = products
    .OrderBy(p => p.Name, StringComparer.OrdinalIgnoreCase)
    .ToList();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the use of conditional preprocessor directive in C#?

Conditional preprocessor directives instruct the compiler to include or exclude code blocks based on defined symbols. They are evaluated at **compile time**, so excluded code is never compiled.

```cs
// Define symbols in .csproj
// <DefineConstants>DEBUG;LOGGING</DefineConstants>
// Or from command line: dotnet build -p:DefineConstants=STAGING

// #if / #elif / #else / #endif
#if DEBUG
    Console.WriteLine("Debug build");
#elif STAGING
    Console.WriteLine("Staging build");
#else
    Console.WriteLine("Production build");
#endif

// #define and #undef (file-scope)
#define FEATURE_NEW_UI
#undef DEBUG

#if FEATURE_NEW_UI
    Console.WriteLine("New UI enabled");
#endif

// Practical: environment-specific configuration
public class AppConfig
{
    public static string ApiBaseUrl =>
#if DEBUG
        "https://localhost:5001";
#elif STAGING
        "https://staging.api.example.com";
#else
        "https://api.example.com";
#endif
}

// #warning and #error — compiler diagnostics
#if !NET10_0_OR_GREATER
#warning This code targets .NET 10+. Older runtimes may not work correctly.
#endif

// Preferred modern alternative — [Conditional] attribute
[System.Diagnostics.Conditional("DEBUG")]
static void Trace(string msg) => Console.WriteLine($"[TRACE] {msg}");

Trace("Starting..."); // compiled out in Release builds

// Target framework checks (built-in .NET symbols)
#if NET10_0_OR_GREATER
    Console.WriteLine("Running on .NET 10+");
#elif NET8_0
    Console.WriteLine("Running on .NET 8");
#endif
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are pointer types in C#?

**Pointer types** are variables that hold the memory address of another variable. They require the `unsafe` context and are primarily used for interoperability with unmanaged code or low-level performance optimizations.

```cs
// Enable unsafe code in .csproj
// <AllowUnsafeBlocks>true</AllowUnsafeBlocks>

unsafe
{
    // Pointer declaration and usage
    int x = 42;
    int* p = &x;           // p holds address of x

    Console.WriteLine(*p); // dereference — prints 42
    *p = 100;              // modify via pointer
    Console.WriteLine(x);  // 100

    // Pointer arithmetic
    int[] arr = [10, 20, 30, 40, 50];
    fixed (int* start = arr) // pin array in memory (prevent GC moving it)
    {
        int* current = start;
        for (int i = 0; i < arr.Length; i++, current++)
            Console.Write($"{*current} "); // 10 20 30 40 50
    }

    // Struct pointer
    var point = new System.Drawing.Point(3, 4);
    System.Drawing.Point* pp = &point;
    Console.WriteLine($"X={pp->X}, Y={pp->Y}"); // X=3, Y=4 (-> dereference)
}

// Span<T> and Memory<T> — safe zero-copy alternatives (preferred in .NET 10)
int[] data = [1, 2, 3, 4, 5];
Span<int> slice = data.AsSpan(1, 3); // [2, 3, 4] — no pointer needed
slice[0] = 99;
Console.WriteLine(data[1]); // 99 — modified original

// P/Invoke with pointers for unmanaged interop
using System.Runtime.InteropServices;

[DllImport("msvcrt.dll", CallingConvention = CallingConvention.Cdecl)]
static extern unsafe int memcmp(void* b1, void* b2, nint count);

unsafe
{
    byte[] a = [1, 2, 3];
    byte[] b = [1, 2, 3];
    fixed (byte* pa = a, pb = b)
        Console.WriteLine(memcmp(pa, pb, a.Length) == 0 ? "Equal" : "Different");
}
```

**When to use:** P/Invoke interop, embedded/systems programming, performance-critical buffer manipulation. For most scenarios, prefer `Span<T>`, `Memory<T>`, or `Marshal` class.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is marshalling and why do we need it?

**Marshalling** is the process of transforming data types between managed (.NET) code and unmanaged (native/COM) code. The .NET runtime automatically marshals simple types, but complex types need explicit configuration.

**Why we need it:** Managed memory is controlled by the GC (objects can be moved/collected). Unmanaged code operates with raw memory pointers. Marshalling bridges this gap safely.

```cs
using System.Runtime.InteropServices;

// 1. Simple P/Invoke — primitives marshalled automatically
[DllImport("kernel32.dll", SetLastError = true)]
static extern uint GetCurrentThreadId();

Console.WriteLine($"Thread ID: {GetCurrentThreadId()}");

// 2. String marshalling
[DllImport("kernel32.dll", CharSet = CharSet.Unicode, SetLastError = true)]
static extern bool GetComputerName(
    System.Text.StringBuilder lpBuffer,
    ref uint nSize);

var sb = new System.Text.StringBuilder(256);
uint size = 256u;
GetComputerName(sb, ref size);
Console.WriteLine($"Computer: {sb}");

// 3. Struct marshalling — layout must match native struct
[StructLayout(LayoutKind.Sequential)]
public struct SystemTime
{
    public ushort Year, Month, DayOfWeek, Day;
    public ushort Hour, Minute, Second, Milliseconds;
}

[DllImport("kernel32.dll")]
static extern void GetSystemTime(out SystemTime st);

GetSystemTime(out SystemTime time);
Console.WriteLine($"{time.Year}-{time.Month:D2}-{time.Day:D2} {time.Hour:D2}:{time.Minute:D2}");

// 4. Marshal class — manual marshalling
IntPtr ptr = Marshal.AllocHGlobal(100);     // allocate unmanaged memory
try
{
    Marshal.WriteInt32(ptr, 42);
    int value = Marshal.ReadInt32(ptr);
    Console.WriteLine(value);               // 42
}
finally
{
    Marshal.FreeHGlobal(ptr);               // always free unmanaged memory
}

// 5. Modern alternative — LibraryImport (.NET 7+, source-generated, AOT-compatible)
public partial class NativeMethods
{
    [LibraryImport("kernel32.dll")]
    public static partial uint GetCurrentProcessId();
}
Console.WriteLine($"Process ID: {NativeMethods.GetCurrentProcessId()}");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How to calculate the code execution time in C#?

```cs
using System.Diagnostics;

// 1. Stopwatch — most precise, recommended
var sw = Stopwatch.StartNew();

// Code to measure
long sum = 0;
for (int i = 0; i < 10_000_000; i++) sum += i;

sw.Stop();
Console.WriteLine($"Elapsed: {sw.ElapsedMilliseconds} ms");
Console.WriteLine($"Elapsed: {sw.Elapsed.TotalMicroseconds:F0} µs");
Console.WriteLine($"Sum: {sum}");

// 2. Measure a specific block with a helper
static T Measure<T>(string label, Func<T> action)
{
    var sw = Stopwatch.StartNew();
    T result = action();
    sw.Stop();
    Console.WriteLine($"{label}: {sw.Elapsed.TotalMilliseconds:F3} ms");
    return result;
}

var result = Measure("Enumerable.Sum", () =>
    Enumerable.Range(0, 10_000_000).Sum(x => (long)x));

// 3. High-resolution timestamp (nanoseconds, .NET 7+)
long start = Stopwatch.GetTimestamp();
// ... work ...
long end = Stopwatch.GetTimestamp();
double nanoseconds = (end - start) * 1_000_000_000.0 / Stopwatch.Frequency;
Console.WriteLine($"Elapsed: {nanoseconds:F0} ns");

// 4. BenchmarkDotNet — production-grade micro-benchmarking
// Install: dotnet add package BenchmarkDotNet
/*
[MemoryDiagnoser]
public class MyBenchmarks
{
    [Benchmark]
    public long LinqSum() => Enumerable.Range(0, 10_000_000).Sum(x => (long)x);

    [Benchmark]
    public long LoopSum()
    {
        long s = 0;
        for (int i = 0; i < 10_000_000; i++) s += i;
        return s;
    }
}
BenchmarkRunner.Run<MyBenchmarks>();
*/

// 5. ActivitySource — distributed tracing (.NET 5+)
using var activity = new System.Diagnostics.ActivitySource("MyApp")
    .StartActivity("ComputeSum");
activity?.SetTag("iterations", 1_000_000);
// ... work ...
activity?.Stop();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the distinction between DirectCast and CType?

`DirectCast` and `CType` are **VB.NET** operators. In C# the equivalents are **direct cast** `(T)obj` and **`Convert` / `as` / `is`** expressions.

| VB.NET | C# Equivalent | Behavior |
|--------|--------------|---------|
| `DirectCast(obj, T)` | `(T)obj` | Requires exact or inheritance relationship; throws `InvalidCastException` on failure |
| `CType(obj, T)` | `Convert.ToT(obj)` or operator | Performs data conversion (e.g., `double` → `int`); wider compatibility |
| `TryCast(obj, T)` | `obj as T` | Returns `null` on failure; reference types only |

```cs
// C# explicit cast (≈ DirectCast) — requires compatible types
object obj = "Hello";
string s = (string)obj;  // ✅ succeeds
// int n = (int)obj;     // ❌ InvalidCastException at runtime

// C# Convert (≈ CType) — performs data conversion
double d = 3.9;
int i = (int)d;               // truncates → 3
int j = Convert.ToInt32(d);   // rounds    → 4
string str = Convert.ToString(123); // int → string

// C# 'as' (≈ TryCast) — null on failure, reference types
object value = 42;
string? result = value as string; // null (not a string)
Console.WriteLine(result is null); // True

// C# 'is' pattern matching — recommended modern approach
object item = "World";
if (item is string text && text.Length > 3)
    Console.WriteLine(text.ToUpper()); // WORLD

// Switch expression with type patterns
object data = 3.14;
string description = data switch
{
    int n    => $"Integer: {n}",
    double d => $"Double: {d:F2}",
    string s => $"String: {s}",
    _        => "Unknown"
};
Console.WriteLine(description); // Double: 3.14
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain how to implement a custom serializer and deserializer for a complex object in C#?

`System.Text.Json` (.NET 10) is the recommended serialization library. For complex or non-standard types, implement a custom `JsonConverter<T>`.

```cs
using System.Text.Json;
using System.Text.Json.Serialization;

// Complex type to serialize
public record Money(decimal Amount, string Currency)
{
    public override string ToString() => $"{Amount:F2} {Currency}";
}

public class Order
{
    public int Id { get; init; }
    public Money Total { get; init; } = new(0, "GBP");
    public DateOnly Date { get; init; }
}

// Custom converter for Money — serialize as "99.99 GBP"
public class MoneyConverter : JsonConverter<Money>
{
    public override Money Read(ref Utf8JsonReader reader, Type typeToConvert,
        JsonSerializerOptions options)
    {
        var raw = reader.GetString()
            ?? throw new JsonException("Expected a string for Money");

        var parts = raw.Split(' ', 2);
        if (parts.Length != 2 || !decimal.TryParse(parts[0], out decimal amount))
            throw new JsonException($"Invalid Money format: '{raw}'");

        return new Money(amount, parts[1]);
    }

    public override void Write(Utf8JsonWriter writer, Money value,
        JsonSerializerOptions options)
    {
        writer.WriteStringValue($"{value.Amount:F2} {value.Currency}");
    }
}

// Configure options
var options = new JsonSerializerOptions
{
    WriteIndented = true,
    Converters = { new MoneyConverter() },
    PropertyNamingPolicy = JsonNamingPolicy.CamelCase,
};

var order = new Order
{
    Id   = 1001,
    Total = new Money(249.99m, "GBP"),
    Date  = new DateOnly(2026, 4, 19),
};

// Serialize
string json = JsonSerializer.Serialize(order, options);
Console.WriteLine(json);
// {
//   "id": 1001,
//   "total": "249.99 GBP",
//   "date": "2026-04-19"
// }

// Deserialize
var restored = JsonSerializer.Deserialize<Order>(json, options);
Console.WriteLine(restored?.Total); // 249.99 GBP

// Source-generated serializer (.NET 6+ — AOT & trimming friendly)
[JsonSourceGenerationOptions(WriteIndented = true,
    PropertyNamingPolicy = JsonKnownNamingPolicy.CamelCase)]
[JsonSerializable(typeof(Order))]
public partial class AppJsonContext : JsonSerializerContext { }

string json2 = JsonSerializer.Serialize(order, AppJsonContext.Default.Order);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain the differences between Memory<T> and Span<T> in C#. When would you use one over the other?

Both provide **zero-copy, allocation-free views** over contiguous memory (arrays, strings, native memory), but they differ in where they can live.

| Feature | `Span<T>` | `Memory<T>` |
|---------|-----------|------------|
| Allocation | Stack only (ref struct) | Stack or heap |
| Use in `async` methods | ❌ Cannot cross `await` | ✅ Safe across `await` |
| Use as class field | ❌ | ✅ |
| Use in lambdas/closures | ❌ | ✅ |
| Convert to Span | N/A | `.Span` property |
| Performance | Slightly faster | Slight overhead |
| Best for | Synchronous, local processing | Async pipelines, fields |

```cs
// Span<T> — synchronous, stack-only
void ProcessSync(Span<byte> buffer)
{
    for (int i = 0; i < buffer.Length; i++)
        buffer[i] ^= 0xFF; // in-place XOR — no allocation

    Console.WriteLine($"Processed {buffer.Length} bytes");
}

byte[] data = [0x01, 0x02, 0x03];
ProcessSync(data.AsSpan());         // array → Span
ProcessSync(stackalloc byte[4]);    // stack memory → Span
Console.WriteLine(string.Join(",", data)); // 254,253,252

// ReadOnlySpan<T> — zero-copy string slicing
ReadOnlySpan<char> greeting = "Hello, World!".AsSpan();
ReadOnlySpan<char> hello    = greeting[..5];  // "Hello" — no allocation
Console.WriteLine(hello.ToString());

// Memory<T> — safe across await
async Task ProcessAsync(Memory<byte> buffer)
{
    // Can hold Memory<T> across await (Span<T> cannot)
    await Task.Delay(10);
    buffer.Span[0] = 42; // access via .Span when needed synchronously
}

byte[] asyncData = new byte[10];
await ProcessAsync(asyncData.AsMemory());
Console.WriteLine(asyncData[0]); // 42

// Memory<T> as a class field — Span<T> cannot be a field
public class DataProcessor
{
    private readonly Memory<byte> _buffer; // ✅ Memory<T> as field

    public DataProcessor(byte[] data) => _buffer = data.AsMemory();

    public async Task RunAsync()
    {
        await Task.Yield(); // simulate async work
        _buffer.Span.Fill(0xFF); // zero all bytes
    }
}

// MemoryPool<T> — reusable memory without GC pressure
using System.Buffers;
using IMemoryOwner<byte> owner = MemoryPool<byte>.Shared.Rent(1024);
Memory<byte> rented = owner.Memory;
rented.Span.Fill(0);
Console.WriteLine($"Rented: {rented.Length} bytes");
// owner disposed → memory returned to pool
```

**Decision:** Use `Span<T>` for synchronous, in-method processing. Use `Memory<T>` when you need to store the view across async boundaries or as a field.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a DTO?

A **DTO (Data Transfer Object)** is a simple object used to transfer data between layers or across process/network boundaries. DTOs contain only data (properties) — no business logic, no methods, no validation.

**Purpose:**
- Decouple API contracts from domain models
- Reduce over-posting / under-posting in APIs
- Shape data for specific consumers (e.g., a mobile app response)

```cs
// Domain model — rich, with business logic
public class Order
{
    public int Id { get; private set; }
    public string CustomerId { get; private set; } = "";
    public List<OrderLine> Lines { get; } = [];
    private decimal _discount;

    public decimal Total => Lines.Sum(l => l.Subtotal) * (1 - _discount);
    public void ApplyDiscount(decimal pct) { /* validation logic */ }
}

// DTO — thin, serialization-friendly
public record OrderSummaryDto(
    int Id,
    string CustomerId,
    decimal Total,
    int ItemCount,
    DateTimeOffset CreatedAt);

// Mapping — manually or via AutoMapper / Mapperly
public static class OrderMapper
{
    public static OrderSummaryDto ToDto(Order order) => new(
        order.Id,
        order.CustomerId,
        order.Total,
        order.Lines.Count,
        DateTimeOffset.UtcNow);
}

// ASP.NET Core minimal API — return DTO, not domain model
app.MapGet("/orders/{id}", (int id, IOrderRepository repo) =>
{
    var order = repo.GetById(id);
    return order is null
        ? Results.NotFound()
        : Results.Ok(OrderMapper.ToDto(order));
});

// Request DTO — controls what clients can send (anti-over-posting)
public record CreateOrderRequest(
    string CustomerId,
    IReadOnlyList<OrderLineRequest> Lines);

public record OrderLineRequest(string ProductId, int Quantity);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What does POCO mean?

**POCO** stands for **Plain Old CLR Object** (or **Plain Old C# Object**). A POCO is a simple class that:
- Has no dependency on any framework base class or interface
- Contains only properties and possibly simple methods
- Is not tied to a specific persistence or serialization framework

```cs
// ✅ POCO — no framework dependency
public class Customer
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
    public string Email { get; set; } = "";
    public DateTime CreatedAt { get; set; }
}

// ✅ POCO record (C# 9+) — immutable POCO
public record ProductPoco(int Id, string Name, decimal Price);

// ❌ Not a POCO — inherits from framework class
public class LegacyController : System.Web.Mvc.Controller { }

// POCOs work with EF Core without inheriting DbContext entities
// EF Core maps POCOs via convention or configuration
public class AppDbContext : DbContext
{
    public DbSet<Customer> Customers => Set<Customer>();
}

// POCOs work with System.Text.Json without attributes
var customer = new Customer
{
    Id = 1,
    Name = "Alice",
    Email = "alice@example.com",
    CreatedAt = DateTime.UtcNow,
};

string json = JsonSerializer.Serialize(customer);
Console.WriteLine(json);
// {"Id":1,"Name":"Alice","Email":"alice@example.com","CreatedAt":"..."}

var restored = JsonSerializer.Deserialize<Customer>(json);
Console.WriteLine(restored?.Name); // Alice
```

**POCO vs DTO:** A POCO is a design principle (framework-free class). A DTO is a pattern (data carrier between layers). A DTO is usually a POCO, but not all POCOs are DTOs.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Define Parsing? Explain how to Parse a DateTime String?

**Parsing** is the process of converting a string representation into a typed value. For `DateTime`/`DateTimeOffset`, .NET provides several methods with different trade-offs.

```cs
using System.Globalization;

// 1. DateTime.Parse — lenient, throws on failure
DateTime dt1 = DateTime.Parse("2026-04-19");
DateTime dt2 = DateTime.Parse("April 19, 2026");
Console.WriteLine(dt1); // 4/19/2026 12:00:00 AM

// 2. DateTime.TryParse — safe, no exception
if (DateTime.TryParse("2026-04-19 14:30:00", out DateTime result))
    Console.WriteLine(result); // 4/19/2026 2:30:00 PM

// 3. DateTime.ParseExact — strict format, throws on mismatch
DateTime exact = DateTime.ParseExact(
    "19/04/2026 14:30",
    "dd/MM/yyyy HH:mm",
    CultureInfo.InvariantCulture);
Console.WriteLine(exact);

// 4. DateTime.TryParseExact — strict + safe
bool ok = DateTime.TryParseExact(
    "19-04-2026",
    "dd-MM-yyyy",
    CultureInfo.InvariantCulture,
    DateTimeStyles.None,
    out DateTime strict);
Console.WriteLine($"Parsed: {ok}, Value: {strict:yyyy-MM-dd}");

// 5. Multiple format candidates
string[] formats = ["dd/MM/yyyy", "MM-dd-yyyy", "yyyy.MM.dd"];
if (DateTime.TryParseExact("2026.04.19", formats,
    CultureInfo.InvariantCulture, DateTimeStyles.None, out DateTime multi))
    Console.WriteLine(multi);

// 6. DateOnly / TimeOnly (.NET 6+) — preferred for date/time-only values
DateOnly date = DateOnly.Parse("2026-04-19");
TimeOnly time = TimeOnly.Parse("14:30:00");
Console.WriteLine($"{date}, {time}");

DateOnly.TryParseExact("19/04/2026", "dd/MM/yyyy",
    CultureInfo.InvariantCulture, DateTimeStyles.None, out DateOnly d);
Console.WriteLine(d); // 4/19/2026

// 7. DateTimeOffset — timezone-aware (preferred for APIs)
DateTimeOffset dto = DateTimeOffset.Parse("2026-04-19T14:30:00+05:30");
Console.WriteLine(dto.ToUniversalTime()); // converted to UTC

// 8. ISO 8601 round-trip format ("O")
string iso = DateTime.UtcNow.ToString("O");
DateTime restored = DateTime.Parse(iso, null, DateTimeStyles.RoundtripKind);
Console.WriteLine(restored.Kind); // Utc
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is IL (Intermediate Language) Code?

**IL (Intermediate Language)**, also called **CIL (Common Intermediate Language)** or historically **MSIL (Microsoft Intermediate Language)**, is the CPU-independent bytecode that .NET compilers (C#, F#, VB.NET) produce. It is not machine code — the JIT compiler converts IL to native machine code at runtime.

```
Source Code (C#/F#/VB)
       ↓ compile
  IL (.dll / .exe)          ← platform-independent
       ↓ JIT / AOT
Native Machine Code         ← platform-specific (x64, ARM64, etc.)
```

**Example — C# and its IL:**

```cs
// C# source
public static int Add(int a, int b) => a + b;
```

```il
// Corresponding IL (viewed in ILDASM or ILSpy)
.method public hidebysig static int32 Add(int32 a, int32 b) cil managed
{
    .maxstack 2
    ldarg.0      // push a
    ldarg.1      // push b
    add          // pop both, push sum
    ret          // return
}
```

**View IL in practice:**

```bash
# ILSpy CLI
dotnet tool install -g ilspycmd
ilspycmd MyApp.dll --il

# Built-in ILDASM (Windows .NET SDK)
ildasm MyApp.dll /output:MyApp.il

# dotnet-ildasm
dotnet tool install -g dotnet-ildasm
dotnet-ildasm MyApp.dll
```

**Characteristics of IL:**
- Stack-based virtual machine instructions
- Strongly typed (includes type information)
- Verifiable — the runtime checks type safety before execution
- Enables cross-language interoperability (all .NET languages target IL)
- Inspectable — you can decompile `.dll` back to C# using ILSpy or dotPeek

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the use of JIT (Just-in-Time compiler)?

The **JIT (Just-in-Time) compiler** is part of the .NET CLR. It converts IL (Intermediate Language) bytecode into native machine code **on demand** — the first time each method is called — and caches the result for subsequent calls.

**JIT compilation pipeline:**

```
IL bytecode → [JIT Compiler] → Native x64/ARM64 code → CPU execution
                  ↑
         (first call only — cached after)
```

```cs
// Demonstrate JIT behavior with RuntimeHelpers
using System.Runtime.CompilerServices;

// Force JIT compilation of a method before timing it
RuntimeHelpers.PrepareMethod(typeof(Program)
    .GetMethod(nameof(HotPath))!.MethodHandle);

static long HotPath()
{
    long sum = 0;
    for (int i = 0; i < 10_000_000; i++) sum += i;
    return sum;
}

// [MethodImpl] hints to the JIT
[MethodImpl(MethodImplOptions.AggressiveInlining)]
static int FastAdd(int a, int b) => a + b; // JIT will inline this call

[MethodImpl(MethodImplOptions.AggressiveOptimization)]
static void HeavyLoop()
{
    // JIT uses Tier-2 optimizations immediately (skip warm-up)
    for (int i = 0; i < 100_000; i++) { /* work */ }
}

[MethodImpl(MethodImplOptions.NoInlining)]
static void AlwaysCallStack() { } // prevents inlining (useful for profiling)
```

**JIT tiers (.NET 6+):**

| Tier | Trigger | Optimization |
|------|---------|-------------|
| Tier 0 | First call | Minimal (fast compilation) |
| Tier 1 | After ~30 calls | Full optimizations (inlining, unrolling, vectorization) |

**AOT (Ahead-of-Time) — .NET 7+ alternative to JIT:**

```bash
# Publish as native AOT — compiles IL to machine code at build time
dotnet publish -r win-x64 -p:PublishAot=true
# Result: single native .exe, no JIT at runtime, faster startup
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Is it possible to view IL code?

**Yes.** Multiple tools can inspect IL from any .NET assembly (`.dll`/`.exe`).

```bash
# 1. ILSpy CLI — cross-platform, decompiles back to C# or raw IL
dotnet tool install -g ilspycmd
ilspycmd MyApp.dll --il               # raw IL output
ilspycmd MyApp.dll -l CSharp          # decompile to C#

# 2. dotnet-ildasm — .NET global tool
dotnet tool install -g dotnet-ildasm
dotnet-ildasm MyApp.dll

# 3. Built-in ILDASM (ships with .NET SDK on Windows)
# Run from Developer Command Prompt:
ildasm MyApp.dll

# 4. dotnet-dump / SOS — inspect IL at runtime
dotnet tool install -g dotnet-dump
dotnet-dump collect -p <PID>
dotnet-dump analyze <dump-file>
# Then: clrthreads, dumpil, etc.
```

**In Visual Studio:**
- **ILSpy extension** — right-click method → "Open in ILSpy"
- **Disassembly window** (Debug → Windows → Disassembly) — shows JIT-compiled native code

**SharpLab.io** — paste C# and instantly view IL, JIT ASM, or decompiled output online.

```cs
// C# source
public static int Square(int x) => x * x;

// IL (as seen in ILSpy):
// .method public hidebysig static int32 Square(int32 x) cil managed
// {
//     ldarg.0
//     ldarg.0
//     mul
//     ret
// }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the benefit of compiling into IL code?

Compiling to IL provides several advantages over compiling directly to native machine code:

| Benefit | Explanation |
|---------|------------|
| **Platform independence** | Same IL runs on Windows, Linux, macOS, ARM — JIT produces native code per platform |
| **Language interoperability** | C#, F#, VB.NET all compile to the same IL — can call each other\'s types seamlessly |
| **Runtime optimizations** | JIT knows the exact CPU (AVX-512, etc.) and optimizes better than a cross-compiled binary |
| **Security / verification** | CLR verifies IL for type safety before execution |
| **Reflection** | IL preserves metadata — types, methods, attributes queryable at runtime |
| **Dynamic code** | `System.Reflection.Emit` generates IL at runtime for dynamic proxies, expression trees |
| **AOT-ready** | Same IL can be JIT-compiled, interpreted (Mono), or AOT-compiled (NativeAOT) |

```cs
// Benefit: Language interoperability
// F# library (compiled to IL):
// module MathLib
// let square x = x * x

// C# consuming F# IL seamlessly:
// int result = MathLib.square(5); // 25

// Benefit: Runtime IL generation with Reflection.Emit
using System.Reflection.Emit;

var method = new DynamicMethod("Add", typeof(int),
    [typeof(int), typeof(int)]);
var il = method.GetILGenerator();
il.Emit(OpCodes.Ldarg_0);
il.Emit(OpCodes.Ldarg_1);
il.Emit(OpCodes.Add);
il.Emit(OpCodes.Ret);

var add = (Func<int, int, int>)method.CreateDelegate(typeof(Func<int, int, int>));
Console.WriteLine(add(3, 4)); // 7

// Benefit: JIT knows the target CPU
// On a machine with AVX-512, JIT auto-vectorizes loops:
int[] arr = Enumerable.Range(0, 1024).ToArray();
int sum = 0;
foreach (var n in arr) sum += n; // JIT may emit VPADDD (SIMD) instruction
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the importance of CTS (Common Type System)?

The **CTS (Common Type System)** is the specification that defines how types are declared, used, and managed in the .NET runtime. It ensures that types from different .NET languages (C#, F#, VB.NET) are compatible with each other.

**Key responsibilities:**
- Defines all type categories (value types, reference types, interfaces, delegates, enums)
- Establishes rules for type inheritance and method overriding
- Enables cross-language type sharing — a C# `int` and a VB.NET `Integer` are both `System.Int32`

```cs
// CTS type mapping — all languages share the same underlying CLR types
// C#      VB.NET     F#        CTS (CLR) Type
// int   = Integer  = int    = System.Int32
// long  = Long     = int64  = System.Int64
// string= String   = string = System.String
// bool  = Boolean  = bool   = System.Boolean

// Proof: C# int IS System.Int32
int x = 42;
System.Int32 y = 42;
Console.WriteLine(x.GetType() == y.GetType()); // True
Console.WriteLine(x.GetType().FullName);        // System.Int32

// CTS value types vs reference types
int    vt = 10;          // value type — CTS ValueType
string rt = "hello";     // reference type — CTS object
Console.WriteLine(vt.GetType().IsValueType); // True
Console.WriteLine(rt.GetType().IsValueType); // False

// CTS type hierarchy — everything derives from System.Object
Console.WriteLine(typeof(int).BaseType?.Name);    // ValueType
Console.WriteLine(typeof(string).BaseType?.Name); // Object
Console.WriteLine(typeof(bool).IsSubclassOf(typeof(object))); // True (via ValueType)

// Cross-language interop enabled by CTS
// A C# class can be used by F# or VB.NET code without any adapter
public class Calculator
{
    public int Add(int a, int b) => a + b;    // System.Int32 — universal in .NET
}
```

**CTS vs CLS:** CTS defines all possible types. The **CLS (Common Language Specification)** is a subset that all languages must support for cross-language compatibility (e.g., avoid unsigned types in public APIs since VB.NET doesn\'t have `uint`).

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How are initializers executed?

In C#, initializers run in a specific order before the constructor body executes. Understanding this order prevents subtle bugs.

**Execution order:**
1. **Static field initializers** (once, before first use of the type)
2. **Static constructor** (`static MyClass()`)
3. **Instance field initializers** (top to bottom, before constructor body)
4. **Base class constructor** (`base(...)`)
5. **Derived class constructor body**

```cs
public class Base
{
    public int BaseField = Log("Base field init", 10);
    public int BaseProp { get; } = Log("Base prop init", 20);

    static int Log(string msg, int val)
    {
        Console.WriteLine(msg);
        return val;
    }

    public Base()  => Console.WriteLine("Base constructor");
}

public class Derived : Base
{
    public int DerivedField = Log("Derived field init", 30);

    public Derived() : base()
    {
        Console.WriteLine("Derived constructor");
    }

    static int Log(string msg, int val)
    {
        Console.WriteLine(msg);
        return val;
    }
}

var d = new Derived();
// Output:
// Derived field init    ← instance field initializer runs FIRST (before base ctor)
// Base field init       ← base field initializers run when base() is called
// Base prop init
// Base constructor      ← base constructor body
// Derived constructor   ← derived constructor body

// Object / collection initializers — syntactic sugar, run after constructor
var list = new List<int> { 1, 2, 3 }; // equivalent to: Add(1); Add(2); Add(3);

record Point(int X, int Y);
var p = new Point(1, 2) { X = 10 }; // with expression — creates new Point(10, 2)

// Required init (C# 11+)
public class Config
{
    public required string ConnectionString { get; init; }
    public int Timeout { get; init; } = 30; // field initializer with default
}

var cfg = new Config { ConnectionString = "Server=localhost" };
Console.WriteLine(cfg.Timeout); // 30
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Shadowing?

**Shadowing** (method hiding) is the practice of declaring a member in a derived class with the **same name** as a member in a base class using the `new` keyword. Unlike `override`, shadowing does not participate in polymorphism — the choice of method depends on the **compile-time type** of the variable.

```cs
public class Animal
{
    public void Speak() => Console.WriteLine("Animal speaks");
    public virtual void Move() => Console.WriteLine("Animal moves");
}

public class Dog : Animal
{
    // Shadowing — hides Animal.Speak (compiler warning without 'new')
    public new void Speak() => Console.WriteLine("Dog barks");

    // Overriding — participates in polymorphism
    public override void Move() => Console.WriteLine("Dog runs");
}

var dog = new Dog();
dog.Speak();  // Dog barks       ← Dog.Speak (compile-time type = Dog)
dog.Move();   // Dog runs        ← Dog.Move (override — polymorphic)

Animal animal = new Dog();
animal.Speak(); // Animal speaks ← Animal.Speak (compile-time type = Animal — shadowing!)
animal.Move();  // Dog runs      ← Dog.Move (override — polymorphic)

// Shadowing in practice — useful when extending sealed/framework types
public class MyList<T> : List<T>
{
    // Shadow List<T>.Add to add logging without override
    public new void Add(T item)
    {
        Console.WriteLine($"Adding: {item}");
        base.Add(item);
    }
}

var myList = new MyList<int>();
myList.Add(42);      // Adding: 42 (shadowed version called)

List<int> asList = myList;
asList.Add(99);      // original List<T>.Add called (no log — shadowing, not override)

// Shadowing vs Overriding
Console.WriteLine("--- Key Difference ---");
// Shadow: method selected by compile-time type
// Override: method selected by runtime type (true polymorphism)
```

**Best practice:** Prefer `override` over shadowing. Use `new` (shadowing) only when you cannot or should not override (e.g., method is not `virtual`, or you intentionally want different behavior per reference type).

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>
