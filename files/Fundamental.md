# C# Basics

> *Click &#9733; if you like the project. Your contributions are heartily ♡ welcome.*

<br>

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

<br>

## Table of Contents

## [L1: Fundamental (Entry-Level / Junior)](Fundamental.md)
Focus: Syntax, basic language constructs, and core type system.

* [Fundamentals](#-1-fundamentals): Data types, variables, type system, and basic `C#` syntax.
* [Operators](#-2-operators): Arithmetic, comparison, logical, bitwise, and null-coalescing operators.
* [Control Flow](#-3-control-flow): Conditional statements (if/else, switch expressions) and loops (for, foreach, while).

## [L2: Intermediate (Junior-Mid / Developer)](Intermediate.md)
Focus: Object-oriented programming, collections, and common language features.

* **Classes and Structs**: Fields, properties, constructors, methods, access modifiers, and records.
* **Inheritance and OOP**: Base/derived classes, abstract classes, interfaces, and polymorphism.
* **Collections and Generics**: List, Dictionary, HashSet, Stack, Queue, and IEnumerable.
* **File Handling**: StreamReader/Writer, File, Path, and Directory APIs.
* **Regular Expression**: Regex patterns, matching, groups, and replacements.
* **Exception Handling**: try/catch/finally, custom exceptions, and best practices.

## [L3: Advanced (Mid-Senior / Lead)](Advanced.md)
Focus: Concurrency, memory management, and advanced language features.

* **Delegates and Events**: Delegates, multicast delegates, events, and EventHandler patterns.
* **Lambda Expressions**: Func, Action, Predicate, expression trees, and closures.
* **Language Integrated Query (LINQ)**: LINQ operators, deferred execution, query syntax, and method chaining.
* **Asynchronous Programming and Multithreading**: Thread, Task, async/await, Parallel, and synchronization primitives.
* **Memory Management and Garbage Collection**: GC generations, IDisposable, finalizers, and memory pressure.

## [L4: Expert (Senior / Architect)](Expert.md)
Focus: Architecture, scalability, performance, and deployment strategies.

* **Advanced C# Features**: Reflection, source generators, unsafe code, and dynamic programming.
* **Performance and Optimization**: `Span<T>`, `Memory<T>`, object pooling, benchmarking, and profiling.
* **Microservices and Distributed Systems**: Service decomposition, gRPC, message brokers, and distributed patterns.
* **Architecture and Design Patterns**: Clean Architecture, CQRS, DDD, and enterprise integration patterns.
* **Deployment**: CI/CD pipelines, containerization, publishing profiles, and environment config.
* **.NET Core**: Middleware, DI container, configuration, hosted services, and ASP.NET Core internals.
* **Miscellaneous**: Reflection, attributes, source generators, and advanced `C#` patterns.

<br>

## # 1. FUNDAMENTALS

<br>

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

**Example — inspecting runtime info (.NET 10)**

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

1. **Source ’ IL:** The C# compiler (`csc` / `dotnet build` using **Roslyn**) compiles source code into **Intermediate Language (IL)** and stores it in assemblies (`.dll` / `.exe`).
2. **Assembly Loading:** The CoreCLR loads the required assemblies at startup.
3. **JIT Compilation:** When a method is called for the first time, the JIT compiler translates its IL to **native machine code** optimized for the current CPU (x64, Arm64, etc.).
4. **Caching:** The native code is cached in memory so subsequent calls execute directly without re-compilation.
5. **Execution:** The CPU runs the native code.

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
| Long-running services      | Preferred       | Supported             |
| Cold-start sensitive apps  | Warm-up delay  | Instant start         |
| Reflection-heavy code      | Full support    | Limited              |
| Smallest binary size       | Runtime needed | Single file           |

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

- Right-click on a reference in Solution Explorer ’ "Go to Definition" to view metadata.
- Use "Object Browser" (View ’ Object Browser) to explore assemblies.

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

## Q. What are variables in C# and how are they declared?

A **variable** in C# is a named storage location that holds a value of a specific type. Variables must be declared before use and can be declared as local, instance, static, or constant.

**1. Local variables — declared inside a method:**

```cs
int age = 25;          // explicitly typed
var name = "Pradeep"; // implicitly typed (compiler infers string)
```

**2. Instance variables — fields of a class:**

```cs
public class Person
{
    public string Name;   // instance field
    public int Age = 0;   // with default value
}
```

**3. Static variables — shared across all instances:**

```cs
public class Counter
{
    public static int Count = 0; // shared by all instances
}
```

**4. Constants — immutable compile-time values:**

```cs
const double Pi = 3.14159;
// Pi = 3.14; // Compile error — cannot reassign
```

**5. Variable scope:**

```cs
void Example()
{
    int x = 10; // x is scoped to this method

    if (x > 5)
    {
        int y = 20; // y is scoped to this block
        Console.WriteLine(x + y); // Output: 30
    }
    // Console.WriteLine(y); // Error: y is out of scope
}
```

**6. Multiple declaration:**

```cs
int a = 1, b = 2, c = 3; // declare and initialize multiple variables
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the basic structure of a C# program?

A C# program is built around **classes** and **methods**. With C# 9+ **Top-Level Statements**, you can also write programs without explicit class or `Main` boilerplate.

**1. Traditional program structure (all versions):**

```cs
using System; // Import namespace

namespace MyApp
{
    class Program
    {
        static void Main(string[] args) // Entry point
        {
            Console.WriteLine("Hello, World!");
        }
    }
}
```

**2. Top-level statements (C# 9+) — preferred for small programs:**

No class or `Main` required. The compiler generates them automatically.

```cs
using System;

Console.WriteLine("Hello, World!"); // Valid C# 9+ program
```

**3. Key structural elements:**

| Element           | Description                                                   |
|-------------------|---------------------------------------------------------------|
| `using`           | Imports a namespace to use its types without full qualification|
| `namespace`       | Logical grouping of related classes and types                 |
| `class`           | Blueprint for objects; contains fields, properties, methods   |
| `static void Main`| Entry point — where execution begins                         |
| `args`            | Command-line arguments passed to the program                  |

**4. File-scoped namespace (C# 10+) — reduces nesting:**

```cs
using System;

namespace MyApp; // No braces needed

class Program
{
    static void Main() => Console.WriteLine("Hello!");
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are access modifiers in C# and how do they control visibility?

Access modifiers in C# control the visibility and accessibility of types and their members. C# has six access modifiers:

| Modifier                    | Accessibility                                                      |
|-----------------------------|--------------------------------------------------------------------|
| `public`                    | Accessible from anywhere                                           |
| `private`                   | Accessible only within the same class (default for members)        |
| `protected`                 | Accessible within the class and derived classes                    |
| `internal`                  | Accessible within the same assembly (default for top-level types)  |
| `protected internal`        | Accessible within the same assembly or from derived classes        |
| `private protected` (C# 7.2)| Accessible within the class and derived classes in the same assembly|

**Example:**

```cs
public class BankAccount
{
    public string Owner { get; set; }        // accessible everywhere
    private decimal _balance;               // accessible only inside this class
    protected string AccountType = "Savings"; // accessible in derived classes
    internal int BranchCode = 101;           // accessible within the same assembly

    public void Deposit(decimal amount)
    {
        if (amount > 0)
            _balance += amount; // private field accessed within the class
    }

    public decimal GetBalance() => _balance;
}

var account = new BankAccount();
account.Owner = "Pradeep";   // OK — public
// account._balance = 100;   // Error — private
account.Deposit(500);
Console.WriteLine(account.GetBalance()); // Output: 500
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is string interpolation in C# and how is it used?

String interpolation (introduced in C# 6) provides a concise syntax to embed expressions directly inside string literals using the `$` prefix. It is the preferred way to format strings in modern C#.

**1. Basic interpolation:**

```cs
string name = "Pradeep";
int age = 28;

string message = $"Name: {name}, Age: {age}";
Console.WriteLine(message); // Output: Name: Pradeep, Age: 28
```

**2. Expressions inside `{}`:**

```cs
int a = 10, b = 5;
Console.WriteLine($"Sum: {a + b}, Product: {a * b}"); // Output: Sum: 15, Product: 50
```

**3. Format specifiers:**

```cs
double price = 1234.567;
Console.WriteLine($"Price: {price:C2}");  // Output: Price: $1,234.57 (currency)
Console.WriteLine($"Price: {price:F1}");  // Output: Price: 1234.6 (1 decimal)
Console.WriteLine($"Hex: {255:X}");       // Output: Hex: FF
```

**4. Multi-line with `$@` or `@$` (verbatim interpolated string):**

```cs
string path = "C:\\Users";
string msg = $@"Hello {name},
Your path is: {path}";
Console.WriteLine(msg);
```

**5. Raw interpolated string (C# 11+):**

```cs
string json = $$"""{ "name": {{name}},  "age": {{age}} }""";

Console.WriteLine(json);
```

**Comparison with alternatives:**

| Method                         | Example                                 |
|--------------------------------|-----------------------------------------|
| Concatenation                  | `"Hello " + name`                       |
| `string.Format`                | `string.Format("Hello {0}", name)`      |
| Interpolation (preferred)      | `$"Hello {name}"`                       |
| `StringBuilder`                | For repeated modifications in loops     |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 2. OPERATORS

<br>

## Q. What are operators available in C#?

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
- `"yyyy-MM-dd"` ’ 2025-05-26
- `"MM/dd/yyyy"` ’ 05/26/2025
- `"dddd, MMMM dd, yyyy"` ’ Monday, May 26, 2025

**Summary:**  
Use `dateTime.ToString()` for default, or `dateTime.ToString("format")` for custom string output.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How to combine two arrays without duplicate values in C#?

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

## Q. What is operator precedence in C# and how does it affect expressions?

**Operator precedence** determines the order in which operators are evaluated in an expression when multiple operators appear together. Operators with higher precedence are evaluated first.

**Precedence table (high -> low):**

| Priority | Operators                                | Description                   |
|----------|------------------------------------------|-------------------------------|
| 1        | `()`, `[]`, `.`, `?.`, `!` (null-forgiving)| Primary                     |
| 2        | `++`, `--`, `+`, `-`, `~`, `!` (unary), `(T)` | Unary               |
| 3        | `*`, `/`, `%`                            | Multiplicative                |
| 4        | `+`, `-`                                 | Additive                      |
| 5        | `<<`, `>>`                               | Shift                         |
| 6        | `<`, `>`, `<=`, `>=`, `is`, `as`         | Relational / type             |
| 7        | `==`, `!=`                               | Equality                      |
| 8        | `&`                                      | Bitwise AND                   |
| 9        | `^`                                      | Bitwise XOR                   |
| 10       | `\|`                                     | Bitwise OR                    |
| 11       | `&&`                                     | Logical AND                   |
| 12       | `\|\|`                                   | Logical OR                    |
| 13       | `??`                                     | Null-coalescing               |
| 14       | `?:`                                     | Conditional (ternary)         |
| 15       | `=`, `+=`, `-=`, `*=`, `??=`, etc.       | Assignment                    |

**Example — precedence affects the result:**

```cs
int result1 = 2 + 3 * 4;       // 14 (* before +)
int result2 = (2 + 3) * 4;     // 20 (parentheses override)
bool check  = 5 > 3 && 2 < 4;  // true (&& after comparisons)

Console.WriteLine(result1); // Output: 14
Console.WriteLine(result2); // Output: 20
Console.WriteLine(check);   // Output: True
```

**Tip:** Use parentheses `()` to make intent explicit and avoid subtle bugs.

```cs
int x = 10;
bool y = x > 5 || x < 3 && x != 7; // && evaluated before ||
bool z = (x > 5 || x < 3) && x != 7; // different result with parentheses

Console.WriteLine(y); // Output: True
Console.WriteLine(z); // Output: True (same here, but intent is clear)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between pre-increment (`++i`) and post-increment (`i++`) in C#?

Both `++i` (pre-increment) and `i++` (post-increment) add 1 to a variable, but they differ in **when** the incremented value is returned.

- **Pre-increment (`++i`):** Increments the value **first**, then returns the new value.
- **Post-increment (`i++`):** Returns the **current** value first, then increments.

**Example:**

```cs
int a = 5;
Console.WriteLine(++a); // Output: 6 (incremented before use)
Console.WriteLine(a);   // Output: 6

int b = 5;
Console.WriteLine(b++); // Output: 5 (used before increment)
Console.WriteLine(b);   // Output: 6
```

**Practical difference in expressions:**

```cs
int x = 3;
int y = ++x * 2; // x becomes 4 first, then y = 4 * 2 = 8
Console.WriteLine($"x={x}, y={y}"); // Output: x=4, y=8

int p = 3;
int q = p++ * 2; // q = 3 * 2 = 6 first, then p becomes 4
Console.WriteLine($"p={p}, q={q}"); // Output: p=4, q=6
```

**In loops — both produce the same result:**

```cs
for (int i = 0; i < 3; i++)  // i++ and ++i behave identically here
    Console.Write(i + " ");
// Output: 0 1 2
```

**Same applies to decrement:** `--i` (pre-decrement) and `i--` (post-decrement).

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 3. CONTROL FLOW

<br>

## Q. What are the conditional statements available in C# and how are they used?

C# provides several conditional statements to control program execution based on conditions: `if`, `else if`, `else`, and `switch`.

**1. `if` / `else if` / `else`:**

```cs
int score = 75;

if (score >= 90)
{
    Console.WriteLine("Grade: A");
}
else if (score >= 75)
{
    Console.WriteLine("Grade: B");
}
else if (score >= 60)
{
    Console.WriteLine("Grade: C");
}
else
{
    Console.WriteLine("Grade: F");
}
// Output: Grade: B
```

**2. `switch` statement:**

```cs
int day = 3;

switch (day)
{
    case 1:
        Console.WriteLine("Monday");
        break;
    case 2:
        Console.WriteLine("Tuesday");
        break;
    case 3:
        Console.WriteLine("Wednesday");
        break;
    default:
        Console.WriteLine("Other day");
        break;
}
// Output: Wednesday
```

**3. `switch` expression (C# 8+):**

A concise, expression-based alternative to the `switch` statement.

```cs
int day = 3;
string dayName = day switch
{
    1 => "Monday",
    2 => "Tuesday",
    3 => "Wednesday",
    4 => "Thursday",
    5 => "Friday",
    _ => "Weekend"
};
Console.WriteLine(dayName); // Output: Wednesday
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the loop constructs available in C# and when should each be used?

C# provides four main loop constructs: `for`, `foreach`, `while`, and `do-while`.

**1. `for` loop — when the number of iterations is known:**

```cs
for (int i = 0; i < 5; i++)
{
    Console.Write(i + " ");
}
// Output: 0 1 2 3 4
```

**2. `foreach` loop — iterating over a collection:**

```cs
string[] fruits = { "Apple", "Banana", "Cherry" };

foreach (string fruit in fruits)
{
    Console.WriteLine(fruit);
}
// Output: Apple  Banana  Cherry
```

**3. `while` loop — when the number of iterations is unknown:**

```cs
int count = 0;

while (count < 5)
{
    Console.Write(count + " ");
    count++;
}
// Output: 0 1 2 3 4
```

**4. `do-while` loop — executes at least once:**

```cs
int number = 0;

do
{
    Console.Write(number + " ");
    number++;
} while (number < 5);
// Output: 0 1 2 3 4
```

**Comparison:**

| Loop        | Use When                                      |
|-------------|-----------------------------------------------|
| `for`       | Known iteration count                         |
| `foreach`   | Iterating over a collection/array             |
| `while`     | Condition checked before each iteration       |
| `do-while`  | Body must execute at least once               |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `break`, `continue`, and `return` in loops?

These three keywords alter the normal flow of a loop or method:

**`break` — exits the loop immediately:**

```cs
for (int i = 0; i < 10; i++)
{
    if (i == 5)
        break;
    Console.Write(i + " ");
}
// Output: 0 1 2 3 4
```

**`continue` — skips the current iteration and moves to the next:**

```cs
for (int i = 0; i < 10; i++)
{
    if (i % 2 == 0)
        continue;
    Console.Write(i + " ");
}
// Output: 1 3 5 7 9
```

**`return` — exits the entire method:**

```cs
int FindFirst(int[] numbers, int target)
{
    for (int i = 0; i < numbers.Length; i++)
    {
        if (numbers[i] == target)
            return i; // exits the method immediately
    }
    return -1;
}

int[] arr = { 10, 20, 30, 40 };
Console.WriteLine(FindFirst(arr, 30)); // Output: 2
```

**Summary:**

| Keyword    | Effect                                      |
|------------|---------------------------------------------|
| `break`    | Exits the current loop or switch            |
| `continue` | Skips to the next loop iteration            |
| `return`   | Exits the current method, optionally with a value |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the ternary operator and how is it used in C#?

The ternary operator `? :` is a concise shorthand for a simple `if-else` statement. It evaluates a condition and returns one of two values.

**Syntax:** `condition ? valueIfTrue : valueIfFalse`

**Example:**

```cs
int age = 20;
string result = age >= 18 ? "Adult" : "Minor";
Console.WriteLine(result); // Output: Adult
```

**Nested ternary (use sparingly for readability):**

```cs
int score = 75;
string grade = score >= 90 ? "A"
             : score >= 75 ? "B"
             : score >= 60 ? "C"
             : "F";
Console.WriteLine(grade); // Output: B
```

**Null-coalescing operator `??` (related):**

Returns the left-hand operand if it is not null; otherwise returns the right-hand operand.

```cs
string? name = null;
string displayName = name ?? "Guest";
Console.WriteLine(displayName); // Output: Guest
```

**Null-coalescing assignment `??=` (C# 8+):**

```cs
string? value = null;
value ??= "Default";
Console.WriteLine(value); // Output: Default
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is pattern matching in C# and how does it enhance control flow?

Pattern matching allows you to test an expression against a pattern and execute code based on the result. C# 8–14 significantly expanded pattern matching capabilities.

**1. Type pattern:**

```cs
object obj = 42;

if (obj is int number)
{
    Console.WriteLine($"Integer: {number}"); // Output: Integer: 42
}
```

**2. Relational and logical patterns (C# 9+):**

```cs
int temperature = 35;

string description = temperature switch
{
    < 0  => "Freezing",
    < 15 => "Cold",
    < 25 => "Mild",
    < 35 => "Warm",
    _    => "Hot"
};
Console.WriteLine(description); // Output: Hot
```

**3. Property pattern:**

```cs
public record Person(string Name, int Age);

var person = new Person("Alice", 17);

string category = person switch
{
    { Age: < 13 }        => "Child",
    { Age: < 18 }        => "Teenager",
    { Age: < 65 }        => "Adult",
    _                    => "Senior"
};
Console.WriteLine(category); // Output: Teenager
```

**4. List pattern (C# 11+):**

```cs
int[] numbers = { 1, 2, 3 };

string result = numbers switch
{
    [1, 2, 3]    => "Exact match",
    [1, ..]      => "Starts with 1",
    _            => "No match"
};
Console.WriteLine(result); // Output: Exact match
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `goto` statement in C# and when should it be used?

The `goto` statement transfers control to a labeled statement elsewhere in the same method. Its most common and accepted use in C# is within `switch` statements to fall through to another case.

**Syntax:**

```cs
goto labelName;
// ...
labelName:
    // code
```

**Example — `goto` in a `switch` statement:**

```cs
int option = 1;

switch (option)
{
    case 1:
        Console.WriteLine("Option 1 selected");
        goto case 3; // falls through to case 3
    case 2:
        Console.WriteLine("Option 2 selected");
        break;
    case 3:
        Console.WriteLine("Common handler");
        break;
}
// Output:
// Option 1 selected
// Common handler
```

**Example — `goto` to exit nested loops:**

```cs
for (int i = 0; i < 3; i++)
{
    for (int j = 0; j < 3; j++)
    {
        if (i == 1 && j == 1)
            goto done;
        Console.WriteLine($"i={i}, j={j}");
    }
}
done:
Console.WriteLine("Exited loops");
```

**Note:** Avoid `goto` for general control flow as it reduces readability. Prefer `break`, `continue`, or refactoring into methods.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `while` and `do-while` loops?

Both loops repeat a block of code while a condition is true, but they differ in **when the condition is checked**.

- **`while` loop:** Checks the condition **before** each iteration. The body may never execute if the condition is false from the start.
- **`do-while` loop:** Checks the condition **after** each iteration. The body **always executes at least once**.

**Example — condition is false from the start:**

```cs
int x = 10;

// while: body never executes
while (x < 5)
{
    Console.WriteLine("while: " + x);
}
// (no output)

// do-while: body executes once regardless
do
{
    Console.WriteLine("do-while: " + x);
} while (x < 5);
// Output: do-while: 10
```

**Practical use case — input validation:**

```cs
string input;
do
{
    Console.Write("Enter a non-empty value: ");
    input = Console.ReadLine();
} while (string.IsNullOrWhiteSpace(input));

Console.WriteLine($"You entered: {input}");
```

**Summary:**

| Feature             | `while`                        | `do-while`                         |
|---------------------|--------------------------------|------------------------------------||
| Condition check     | Before each iteration          | After each iteration               |
| Minimum executions  | 0 (may never run)              | 1 (always runs at least once)      |
| Best for            | Condition may fail from start  | Must run at least once (e.g., menus, validation) |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does the `foreach` loop work with `IEnumerable<T>`?

The `foreach` loop in C# works with any type that implements `IEnumerable` or `IEnumerable<T>`. Internally, the compiler calls `GetEnumerator()` and repeatedly calls `MoveNext()` / `Current` to iterate.

**Compiler translation of `foreach`:**

```cs
foreach (var item in collection)
    Console.WriteLine(item);

// Equivalent to:
var enumerator = collection.GetEnumerator();
try
{
    while (enumerator.MoveNext())
    {
        var item = enumerator.Current;
        Console.WriteLine(item);
    }
}
finally
{
    (enumerator as IDisposable)?.Dispose();
}
```

**Example — iterating common collections:**

```cs
// Array
string[] fruits = { "Apple", "Banana", "Cherry" };
foreach (string fruit in fruits)
    Console.WriteLine(fruit);

// List<T>
var numbers = new List<int> { 1, 2, 3 };
foreach (int n in numbers)
    Console.WriteLine(n);

// Dictionary<K,V>
var scores = new Dictionary<string, int> { ["Alice"] = 90, ["Bob"] = 85 };
foreach (var (name, score) in scores)  // deconstruction (C# 7+)
    Console.WriteLine($"{name}: {score}");
```

**Custom `IEnumerable<T>` with `yield return`:**

```cs
IEnumerable<int> GetEvenNumbers(int max)
{
    for (int i = 0; i <= max; i += 2)
        yield return i; // lazily produced
}

foreach (int n in GetEvenNumbers(10))
    Console.Write(n + " "); // Output: 0 2 4 6 8 10
```

**Note:** You cannot modify the collection being iterated inside a `foreach` — this throws an `InvalidOperationException` at runtime.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use multiple case labels and fall-through in a `switch` statement?

C# `switch` does **not** fall through by default (unlike C/C++). You must use `break`, `return`, or `goto case` explicitly. However, you can stack multiple case labels on a single block.

**1. Multiple case labels for the same block:**

```cs
int day = 6;

switch (day)
{
    case 1:
    case 2:
    case 3:
    case 4:
    case 5:
        Console.WriteLine("Weekday");
        break;
    case 6:
    case 7:
        Console.WriteLine("Weekend");
        break;
    default:
        Console.WriteLine("Invalid day");
        break;
}
// Output: Weekend
```

**2. `goto case` for explicit fall-through:**

```cs
int code = 1;

switch (code)
{
    case 1:
        Console.WriteLine("Code 1 — running extra logic");
        goto case 3; // explicitly jump to case 3
    case 2:
        Console.WriteLine("Code 2");
        break;
    case 3:
        Console.WriteLine("Shared handler for codes 1 and 3");
        break;
}
// Output:
// Code 1 — running extra logic
// Shared handler for codes 1 and 3
```

**3. `switch` expression with multiple patterns (C# 8+):**

```cs
int day = 6;
string type = day switch
{
    1 or 2 or 3 or 4 or 5 => "Weekday",  // or pattern (C# 9+)
    6 or 7                => "Weekend",
    _                     => "Invalid"
};
Console.WriteLine(type); // Output: Weekend
```

**4. Pattern matching with `when` guards:**

```cs
int score = 85;
string grade = score switch
{
    >= 90          => "A",
    >= 75 and < 90 => "B",  // and pattern (C# 9+)
    >= 60          => "C",
    _              => "F"
};
Console.WriteLine(grade); // Output: B
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>
