# C# Basics

> *Click &#9733; if you like the project. Your contributions are heartily ♡ welcome.*

<br>

## Related Topics

* *[ADO.NET](https://github.com/learning-zone/csharp-basics/blob/main/ado.net.md)*
* *[ASP.NET Core](https://github.com/learning-zone/csharp-basics/blob/main/asp-net-core.md)*
* *[C# Multiple Choice Questions](https://github.com/learning-zone/csharp-basics/blob/main/dotnet-mcq.md)*
* *[C# Unit Testing](https://github.com/learning-zone/csharp-basics/blob/main/csharp-unit-test.md)*
* *[C# Design Patterns](https://github.com/learning-zone/csharp-basics/blob/main/csharp-dp.md)*
* *[C# Data Structures and Algorithms](https://github.com/learning-zone/csharp-basics/blob/main/csharp-ds.md)*
* *[React Basics](https://github.com/learning-zone/react-basics)*
* *[SQL Basics](https://github.com/learning-zone/sql-basics)*

<br>

## Table of Contents

## L1: Fundamental (Entry-Level / Junior)
Focus: Syntax, basic language constructs, and core type system.

* [Fundamentals](#-1-fundamentals): Data types, variables, type system, and basic `C#` syntax.
* [Operators](#-2-operators): Arithmetic, comparison, logical, bitwise, and null-coalescing operators.
* [Control Flow](#-3-control-flow): Conditional statements (if/else, switch expressions) and loops (for, foreach, while).

## [L2: Intermediate (Junior-Mid / Developer)](files/Intermediate.md)
Focus: Object-oriented programming, collections, and common language features.

* **Classes and Structs**: Fields, properties, constructors, methods, access modifiers, and records.
* **Inheritance and OOP**: Base/derived classes, abstract classes, interfaces, and polymorphism.
* **Collections and Generics**: List, Dictionary, HashSet, Stack, Queue, and IEnumerable.
* **File Handling**: StreamReader/Writer, File, Path, and Directory APIs.
* **Regular Expression**: Regex patterns, matching, groups, and replacements.
* **Exception Handling**: try/catch/finally, custom exceptions, and best practices.

## [L3: Advanced (Mid-Senior / Lead)](files/Advanced.md)
Focus: Concurrency, memory management, and advanced language features.

* **Delegates and Events**: Delegates, multicast delegates, events, and EventHandler patterns.
* **Lambda Expressions**: Func, Action, Predicate, expression trees, and closures.
* **Language Integrated Query (LINQ)**: LINQ operators, deferred execution, query syntax, and method chaining.
* **Asynchronous Programming and Multithreading**: Thread, Task, async/await, Parallel, and synchronization primitives.
* **Memory Management and Garbage Collection**: GC generations, IDisposable, finalizers, and memory pressure.

## [L4: Expert (Senior / Architect)](files/Expert.md)
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

**C# (C-Sharp)** is a modern, object-oriented, and type-safe programming language developed by Microsoft. It runs on the **.NET** platform and is designed to build a wide variety of secure and robust applications, including web apps, desktop software, mobile apps, cloud services, and video games.

**Main features of C#:**

**1. Object-Oriented Programming (OOP)** 

C# fully supports core OOP pillars:

* **Encapsulation**: Bundling data and methods into classes using access modifiers.
* **Abstraction**: Hiding complex implementation details and exposing only the essential features through abstract classes and interfaces.
* **Inheritance**: Creating new classes based on existing ones to reuse code.
* **Polymorphism**: Allowing methods to behave differently based on the object calling them.

**2. Type Safety and Static Typing**

C# enforces strict type checking at compile-time:

* The compiler detects data type mismatches before the program runs.
* It prevents unauthorized memory operations and eliminates many common coding bugs.

**3. Automatic Memory Management**

Developers do not need to manually allocate and free up memory:

* A built-in Garbage Collector (GC) automatically reclaims memory occupied by unused objects.
* This drastically reduces memory leaks and pointer corruption issues.

**4. Type Inference (`var`)**

While strongly typed, C# offers a `var` keyword:

* The compiler automatically deduces the data type based on the assigned value.
* It shortens code length without sacrificing compile-time safety.

**5. Language Integrated Query (LINQ)**

LINQ is a powerful, built-in feature that lets you query data inside C# code:

* You can query SQL databases, XML documents, and standard collections using a uniform syntax.
* Example: `var adults = users.Where(u => u.Age >= 18)`;

**6. Asynchronous Programming (async / await)**

C# makes writing non-blocking code remarkably simple:

* It uses the async and await keywords to handle time-consuming tasks (like database queries or file I/O).* This keeps applications fast, responsive, and scalable under heavy traffic.

**7. Modern Language Enhancements**

Microsoft updates C# annually with modern functional features:

* **Pattern Matching**: Easily inspect structures and extract data safely.
* **Records**: Lightweight, immutable data objects ideal for data transfer.
* **Top-Level Statements**: Eliminates boilerplate code, making entry-level files clean and readable.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the data types available in C#?

C# is a strongly-typed language, meaning every variable and constant must have a declared data type. C# offers a variety of data types categorized as **value types**, **reference types**, and **pointer types**. Value types store data directly, while reference types store memory addresses to the actual data. 

**1. Value Types (Stored on the Stack)**

Value types hold the actual data directly within their own memory space.

**Integer Types**

* `int`: 32-bit signed integer (most common for whole numbers).
* `long`: 64-bit signed integer (for very large whole numbers).
* `short`: 16-bit signed integer.
* `byte`: 8-bit unsigned integer.

**Note**: Unsigned variants exist, prefixed with 'u' (e.g., `uint`, `ulong`).

**Floating-Point Types**

* `float`: 32-bit single-precision (requires `f` suffix, e.g., `3.14f`).
* `double`: 64-bit double-precision (default for decimals).
* `decimal`: 128-bit high-precision (requires `m` suffix, e.g., `19.99m`). Highly recommended for financial calculations.

**Other Simple Types**

* `bool`: Stores Boolean values (`true` or `false`).
* `char`: Stores a single 16-bit Unicode character (uses single quotes, e.g., `'A'`).

Complex Value Types

* `struct`: User-defined structures used to encapsulate small groups of related variables.
* `enum`: A distinct type that consists of a set of named constants.

**2. Reference Types:**

Reference types do not store the actual data. Instead, they store a reference (a pointer) to the memory address where the data is kept.

* `string`: Represents a sequence of Unicode characters (uses double quotes, e.g., `"Hello"`).
* `object`: The ultimate base type from which all other types inherit.
* `class`: User-defined blueprints used to create objects.
* `interface`: Contracts that define a set of operations that a class or struct must implement.
* `delegate`: References to methods (used for events and callbacks).
* `array`: Collections of data elements of the same type (e.g., `int[]`).

**3. Pointer Types:**

Used in unsafe code for direct memory manipulation (e.g., int*, char*).

**4. Nullable Types:**

Allow value types to represent null (e.g., int?, bool?).

**Example:**

```cs
using System;

class Program
{
    static void Main()
    {
        // Value types
        int age = 25;
        double salary = 45000.75;
        bool isActive = true;
        char grade = 'A';

        // Reference types
        string name = "Pradeep";
        int[] marks = { 80, 90, 85 };
        object data = "Hello";

        // Nullable type
        int? optionalNumber = null;

        Console.WriteLine($"Age: {age}");
        Console.WriteLine($"Salary: {salary}");
        Console.WriteLine($"Is Active: {isActive}");
        Console.WriteLine($"Grade: {grade}");
        Console.WriteLine($"Name: {name}");
        Console.WriteLine($"First Mark: {marks[0]}");
        Console.WriteLine($"Object Data: {data}");
        Console.WriteLine($"Optional Number: {optionalNumber}");
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can primitive data types be stored in heap?

Yes, primitive data types in C# (such as int, float, bool, etc.) are value types and are typically stored on the stack when used as local variables. However, they can be stored on the heap in certain scenarios:

**1. Fields inside a Class**

Classes are reference types and always live on the heap. Any primitive fields inside that class live on the heap with it.

```cs
public class Player
{
    public int health = 100; // Stored on the heap inside the Player object
}
```

**2. Boxing**

Boxing converts a value type to a reference type. This copies the primitive value into a wrapper object on the heap

```cs
int age = 25;          // Stored on the stack
object boxedAge = age; // The value 25 is copied to the heap
```

**3. Elements in an Array**

Arrays are reference types and live on the heap. Even if the array holds primitives, the data resides on the heap

```cs
int[] scores = new int[3]; // The three integers are stored on the heap
```

**4. Variables in Closures (Lambda Expressions)**

If a local primitive is used inside a lambda expression or LINQ query, the compiler moves it to a hidden class on the heap.

```cs
int multiplier = 2; 
Func<int, int> myFunc = x => x * multiplier; // multiplier is moved to the heap
```

**5. Async Method Variables**

Local variables inside an async method that persist across an await point are stored in a state machine structure on the heap.

```cs
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        await CalculateAsync();
    }

    static async Task CalculateAsync()
    {
        int number = 10; // Local variable
        Console.WriteLine($"Before await: {number}");

        await Task.Delay(500); // Method is suspended here

        // 'number' persists across the await point
        number += 5;
        Console.WriteLine($"After await: {number}");
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. It is possible to store mixed datatypes such as int, string, float, char all in one array?

Yes, it is possible to store mixed types, but not single strongly-type variable. However, there are several ways to achieve this depending on use case.

**1. The object[] Array (Most Common)**

In C#, every data type inherits from the base class object. Creating an array of objects allows you to store any data type inside it

```cs
// Implicit conversion happens automatically via "boxing"
object[] mixedArray = new object[] { 42, "Hello", 3.14f, 'A' };

// Accessing elements requires manual casting (unboxing)
int firstElement = (int)mixedArray[0];
string secondElement = (string)mixedArray[1];
```

* **Pros**: Quick, simple, and accepts absolutely any data type.
* **Cons**: Poor performance due to boxing/unboxing (converting value types to objects on the heap), and it can throw runtime errors if you cast to the wrong type.

**2. The ArrayList Collection (Legacy)**

Before C# introduced generics, `ArrayList` was the standard way to make a dynamically sizing mixed array. It functions identically to an `object[]` array under the hood.

```cs
using System.Collections;

ArrayList mixedList = new ArrayList();

mixedList.Add(42);
mixedList.Add("Hello");
mixedList.Add(3.14f);
mixedList.Add('A');
```

* **Pros**: Automatically resizes like a list.
* **Cons**: Outdated legacy type. It suffers from the same performance and safety issues as `object[]`

**3. Tuples (Best for Fixed-Size Records)**

If you know exactly how many items you have and their position ahead of time, a Tuple provides a strongly-typed collection of mixed data without losing performance.

```cs
// Declaring a strongly-typed mixed group
var mixedTuple = (Number: 42, Text: "Hello", Decimal: 3.14f, Character: 'A');

// Accessing items is lightning fast and type-safe
int myInt = mixedTuple.Number; 
string myString = mixedTuple.Text;
```

* **Pros**: Type-safe, fast, no boxing penalties, and supports clean element naming.
* **Cons**: Fixed size. You cannot loop through it like a traditional array using an index (`[i]`).

**4. Custom Classes or Structs (Best for Data Models)**

If your mixed array represents a specific entity (like a user profile containing an ID, Name, and Rating), wrapping the types inside a class or struct is the best practice.

```cs
public struct MixedData
{
    public int MyInt;
    public string MyString;
    public float MyFloat;
    public char MyChar;
}

// You can now safely make an array of this unified custom type
MixedData[] array = new MixedData[5];
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is an extension method in C# and how is it implemented?

An extension method in C# is a special kind of static method that allows you to add new functionality to an existing type (such as a class, interface, struct, or enum) without modifying its original source code, recompiling it, or using inheritance.

It is defined as a `static` method in a `static` class, with the first parameter prefixed with `this`. C# 14 also introduces **extension members** (a superset of extension methods).

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

In C#, a generic type is a blueprint that allows you to define classes, interfaces, structures, methods, or delegates with a placeholder (type parameter) instead of a specific data type. You specify the actual data type later, when you instantiate the class or call the method in your code. With **C# 11 Generic Math**, generic code can now also perform arithmetic operations across numeric types.

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
using System;

class Program
{
    // Define a delegate type
    public delegate void PrintMessage(string message);

    static void Main()
    {
        // Declaring an Anonymous Method
        PrintMessage print = delegate(string val) 
        {
            Console.WriteLine($"Anonymous Method Output: {val}");
        };

        // Invoking the delegate
        print("Hello World!"); 

        // Example with an Event Handler
        // button.Click += delegate (object sender, EventArgs e) { LogClick(); };
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

**Core Architecture & Execution Flow**

```cs
[ C# Source Code ] 
       │
       ▼ (Language Compiler: csc)
[ Common Intermediate Language (CIL) / Assembly ]
       │
       ▼ (Loaded into the CLR Engine)
┌────────────────────────────────────────┐
│      Common Language Runtime (CLR)     │
│  ┌──────────────────────────────────┐  │
│  │       Just-In-Time (JIT)         │  │
│  └──────────────────────────────────┘  │
│  ┌──────────────────────────────────┐  │
│  │        Garbage Collector         │  │
│  └──────────────────────────────────┘  │
│  ┌──────────────────┬───────────────┐  │
│  │ Type Checker     │ Security      │  │
│  └──────────────────┴───────────────┘  │
└────────────────────────────────────────┘
       │
       ▼ (Compiled at Runtime)
[ Native Machine Code ] 
       │
       ▼
[ Operating System & Hardware ]
```

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

Logger logger = new Logger(); // No prefix needed now
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `using` statement in C#?

The primary purpose of the `using` statement in C# is to **ensure automatic resource cleanup** for objects that implement the `IDisposable` interface.

It guarantees that unmanaged resources (like file handles, database connections, or network sockets) are properly released as soon as the execution block exits, even if an unhandled exception occurs.

**1. Traditional `using` block:**

```cs
using (StreamReader file = new StreamReader("file.txt"))
{
    string content = file.ReadToEnd();
    // file is automatically disposed when the block exits
}
```

**2. `using` declaration:**

No braces needed — the object is disposed at the end of the enclosing scope. This is the preferred modern style.

```cs
using var file = new StreamReader("file.txt");
string content = file.ReadToEnd();
// file is automatically disposed here (end of method/block)
```

**3. `await using` for async disposal:**

For objects implementing `IAsyncDisposable` (e.g., async streams, `HttpClient`, `DbContext`):

```cs
await using var connection = new SqlConnection(connectionString);
await connection.OpenAsync();
// connection is asynchronously disposed at end of scope
```

**Key points:**

* Applies to any type implementing `IDisposable` or `IAsyncDisposable`.
* Prevents resource leaks for files, streams, database connections, HTTP clients, etc.
* `using` declarations reduce nesting and are generally preferred in modern .NET code.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are properties in C# and how are they used?

Properties in C# are class members that provide a flexible mechanism to read, write, or compute the value of a private field. They act like a public variable to anyone using the object, but they are actually special methods called **accessors** (`get` and `set`). This ensures clean encapsulation and data safety.

**Why Use Properties?**

* **Data Validation**: Block invalid entries before saving them.
* **Encapsulation**: Hide your internal class data structure.
* **Read/Write Control**: Make a value read-only or write-only easily.
* **Computed Values**: Calculate data dynamically on demand

**Types of Properties**

**1. Auto-Implemented Properties**

Use these when you do not need special validation logic. The C# compiler automatically creates a hidden, private variable (backing field) behind the scenes

```cs
public class User
{
    // A standard read-write property
    public string Name { get; set; }

    // A property with a default initial value
    public int Level { get; set; } = 1;
}
```

**2. Fully-Implemented Properties (With Backing Fields)**

Use these when you need to execute extra logic, such as validation or formatting, when data is read or written.

```cs
public class Account
{
    private decimal _balance; // Private backing field

    public decimal Balance
    {
        get 
        { 
            return _balance; 
        }
        set 
        {
            // Enforce validation logic
            if (value < 0)
            {
                throw new ArgumentException("Balance cannot be negative!");
            }
            _balance = value; // 'value' is the incoming data
        }
    }
}
```

**3. Read-Only & Expression-Bodied Properties**

You can restrict external code from modifying your values. Expression-bodied syntax (`=>`) makes calculated properties incredibly short

```cs
public class Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }

    // Read-only calculated property using expression body
    public string FullName => $"{FirstName} {LastName}";
}
```

**4. Fine-Grained Access Control (`private set` and `init`)**

You can mix access modifiers to allow anyone to read a variable, but only allow your class or initializer to set it

```cs
public class Product
{
    // Can be read anywhere, but only modified inside this class
    public string SKU { get; private set; }

    // Can only be set during object creation (Immutable after)
    public decimal Cost { get; init; }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the Arrays in C#.Net?

An array in C# is a collection of variables of the same data type stored in contiguous memory locations. Arrays have a fixed length determined at creation, which cannot be changed later. They allow you to store multiple items (like a list of numbers or names) under a single variable name and access them using an index

**Key Points:**

* **Type-Safe**: Can only hold one specific data type.
* **Fixed Size**: Must define the size upon initialization.
* **Zero-Indexed**: The first element starts at index `0`.
* **Performance**: Extremely fast for reading and writing data

**Types of Arrays:**

**1. Single-Dimensional Array:**  
   
It stores a single row of data elements

```cs
// Declaration and initialization
int[] numbers  = new int[3] { 10, 20, 30 };

foreach(int number in numbers) {
    Console.WriteLine(number); // Output: 10, 20, 30
}

// Shortened syntax (Compiler infers the size)
string[] fruits = { "Apple", "Banana", "Cherry" };

// Accessing and modifying elements
int firstItem = numbers[0]; // Returns 10
fruits[1] = "Blueberry";    // Changes "Banana" to "Blueberry"
```

**2. Multi-Dimensional Array:**  

Stores data in a grid format with rows and columns. Every row must have the exact same number of columns.

```cs
// 2D Array: 2 rows and 3 columns
int[,] matrix = new int[2, 3] 
{
    { 1, 2, 3 },
    { 4, 5, 6 }
};

// Accessing an element (Row 1, Column 2)
int value = matrix[1, 2]; // Returns 6
```

**3. Jagged Array (Array of Arrays):**  

An array of arrays, where each inner array can have a different length.

```cs
// Declaration
int[][] jaggedArray = new int[3][];

// Initializing individual rows with different sizes
jaggedArray[0] = new int[] { 1, 2 };
jaggedArray[1] = new int[] { 3, 4, 5, 6 };
jaggedArray[2] = new int[] { 7 };

// Accessing an element (Row 1, Element index 2)
int jaggedValue = jaggedArray[1][2]; // Returns 5
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between the System.Array.CopyTo() and System.Array.Clone()?

The main difference is that `Clone()` creates a brand-new array object, while `CopyTo()` copies elements into an already existing array. 

**1. Using `CopyTo()`:**

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

**2. Using `Clone()`:**

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

A jagged array in C# is an "array of arrays." Unlike a standard multi-dimensional array, a jagged array consists of rows where each row can have a completely different length.

**Key Characteristics**

* **Irregular Shape**: Each row is an independent array object with its own size.
* **Separate Allocations**: You must initialize the top-level array first, and then initialize each individual row array separately.
* **Performance**: Jagged arrays can sometimes perform faster than multidimensional arrays (`int[,]`) because the .NET runtime optimizes single-dimensional array access.
* **Memory Efficient**: It saves memory when rows naturally require different amounts of data (e.g., storing the number of days per month).

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

To sort the elements of an array in descending order in C#, you can use the `Array.Sort` method with a custom comparer, or use `LINQ` for a more concise approach.

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

The primary difference between `var` and `dynamic` is when the data type is determined: `var` resolves the variable type at **compile** time, whereas `dynamic` resolves it at **runtime**.

**`var` (Implicit Typing):**

* **Type inference**: The type of a `var` variable is determined by the compiler at compile time, based on the assigned value.
* **Type safety**: Once assigned, the type cannot change, and all type checks are performed at compile time.
* **Usage**: Useful for anonymous types or when the type is obvious from the right-hand side.

**`dynamic` (Dynamic Typing):**

* **Runtime type resolution**: The type of a dynamic variable is determined at runtime, not at compile time.
* **No compile-time type checking**: Errors related to type usage are only detected at runtime.
* **Flexibility**: Allows operations that may not be valid at compile time, but can cause runtime exceptions if used incorrectly.
* **Usage**: Useful when working with COM objects, dynamic languages, or reflection.

**Example:**

```cs
// Using var
var score = 95;        // Compiler explicitly locks 'score' as an Integer
score = "Excellent";   // COMPILE ERROR: Cannot implicitly convert string to int

// Using dynamic
dynamic response = 95;      // Resolved as Integer at runtime
response = "Excellent";     // Perfectly fine; changed to String at runtime
```

**Key Differences:**

| Feature | var (Implicitly Typed) | dynamic (Dynamically Typed) |
|---|---|---|
| **Type Resolution** | Done by the compiler at **compile time**. | Done by the execution engine at **runtime**. |
| **Type Safety** | Completely **type-safe**. | **Not type-safe** at compile time. |
| **Initialization** | Mandatory at the moment of declaration. | Optional; can be declared empty. |
| **Type Alteration** | Cannot change after it is first assigned. | Can change freely to any type at any point. |
| **Error Discovery** | Errors are caught immediately during compilation. | Errors only trigger as exceptions during execution. |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a `struct` in C#?

A `struct` (structure) in C# is a user-defined value type designed to encapsulate small groups of related variables. Unlike a `class`, which is a reference type, a struct stores its data directly where it is declared. 

**At-a-Glance Comparison: Struct vs. Class**

| Feature | struct | class |
|---|---|---|
| **Type Category** | Value type | Reference type |
| **Memory Location** | Stack (usually) | Heap |
| **Assignment** | Copies the actual data | Copies the reference (pointer) |
| **Inheritance** | Cannot inherit from other classes or structs | Fully supports inheritance |
| **Nullability** | Cannot be `null` by default (requires `?`) | Can be `null` by default |
| **Default Constructor** | Automatically provided (cannot be removed) | Custom ones override default |


**Core Characteristics of a Struct**

**1. Value Type Copy Behavior** 
 
When you assign one struct variable to another, C# creates an independent copy of all the data. Modifying the copy does not alter the original structure. 

```cs
public struct Point 
{
    public int X;
    public int Y;
}
// Behavioral Example
Point p1 = new Point { X = 10, Y = 20 };
Point p2 = p1; // p2 is a brand new copy of the data

p2.X = 99;     // Changing p2 does NOT change p1
Console.WriteLine(p1.X); // Outputs: 10
```

**2. Memory and Performance**

Structs are generally allocated on the stack, making creation and destruction incredibly fast. They bypass the garbage collector (GC) overhead entirely, which makes them highly efficient for critical performance loops. 

**3. Limitations**

* No Inheritance: A struct cannot inherit from any class or other struct, and it cannot be base-classed. However, it can implement interfaces.  
* Size Restrictions: Microsoft recommends keeping structs small—typically under 16 bytes of data. Large structs degrade performance because copying large chunks of data across methods is slow. 

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `abstract` and `virtual` methods?

The primary difference is that an `abstract` method has no implementation and must be overridden, while a `virtual` method has a default implementation and can optionally be overridden.

**Abstract Methods:**

* An abstract method is declared in an abstract class, meaning it doesn\'t have a concrete implementation. 
* Abstract classes, by definition, cannot be instantiated directly. 
* Derived classes that inherit from an abstract class must provide a concrete implementation for all abstract methods to be instantiated. 
* This forces derived classes to implement specific behavior related to the abstract method.

**Example:**

```cs
public abstract class Animal
{
    // No body allowed here
    public abstract void MakeSound(); 
}

public class Dog : Animal
{
    // MUST override, or code won\'t compile
    public override void MakeSound() 
    {
        Console.WriteLine("Bark!");
    }
}
```

**Virtual Methods:**

* A virtual method has a default implementation in the base class. 
* Derived classes can choose to override the virtual method, providing their own implementation. 
* If a derived class does not override a virtual method, the base class\'s implementation will be used. 
* Virtual methods enable polymorphism, allowing different object types to respond to the same method call in different ways. 

**Example:**

```cs
public class User
{
    // Has a default implementation
    public virtual void AccessDashboard() 
    {
        Console.WriteLine("Loading standard user dashboard...");
    }
}

public class Admin : User
{
    // OPTIONAL override to add unique behavior
    public override void AccessDashboard() 
    {
        Console.WriteLine("Loading admin controls and dashboard...");
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `out` and `ref` parameters?

The primary difference between `ref` and `out` parameters is who is responsible for initializing the variable. A `ref` parameter requires the variable to be initialized before it is passed into the method, while an `out` parameter requires the method to initialize it before the method returns. 

Both keywords allow you to pass arguments by reference rather than by value. 

**At-a-Glance Comparison**

| Feature | ref Parameter | out Parameter |
|---|---|---|
| Prior Initialization | Mandatory before calling the method. | Optional; can be passed completely uninitialized. |
| Method Responsibility | Can read or modify the value freely. | Must assign a value before the method ends. |
| Primary Purpose | Modifying an existing variable\'s data. | Returning multiple values from a single method. |
| Direction of Data | Two-way (Data goes In and comes Out). | One-way (Data only comes Out). |

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

A `Tuple` in C# is a lightweight data structure that allows you to group multiple data elements into a single, cohesive unit. It is primarily used to return multiple values from a method without the overhead of creating a formal `class` or `struct`. 

C# supports two types of tuples: modern **ValueTuples** (strongly recommended) and legacy **System.Tuple** classes. 

**Modern ValueTuples vs. Legacy Tuples**

| Feature | Modern Tuple (ValueTuple) | Legacy Tuple (System.Tuple) |
|---|---|---|
| Type Category | Value type (struct) | Reference type (class) |
| Syntax | Simple parentheses: (int, string) | Verbose generic: Tuple<int, string> |
| Element Naming | Supported (e.g., tuple.Age) | Not supported (restricted to Item1, Item2) |
| Mutability | Mutable (values can be changed) | Immutable (read-only) |
| Performance | High (Stack allocated, zero GC allocation) | Lower (Heap allocated, creates GC pressure) |


**Core Usage and Examples (Using Modern ValueTuple)**

**1. Creating and Accessing Named Tuples**

You can assign explicit, readable names to your tuple fields instead of relying on generic properties. 

```cs
// Creating a tuple with named fields
var employee = (Id: 101, Name: "Alice", IsActive: true);

// Accessing fields by their custom names
Console.WriteLine(employee.Name);    // Outputs: Alice
Console.WriteLine(employee.IsActive); // Outputs: True
```

**2. Returning Multiple Values from a Method**

This is the most common use case for tuples, replacing the older out parameter pattern completely.  

```cs
// Method definition returning a tuple
public (int Min, int Max) FindRange(int[] numbers)
{
    // Implementation logic here...
    return (12, 89); 
}

// Usage
var range = FindRange(new int[] { 23, 12, 45, 89 });
Console.WriteLine($"Min is {range.Min}, Max is {range.Max}");
```

**3. Deconstructing a Tuple**

You can split a tuple\'s elements directly into separate, distinct variables in a single line. 

```cs 
var product = (Id: 50, Price: 19.99);
// Deconstruction syntax
(int prodId, double cost) = product;

Console.WriteLine(prodId); // Outputs: 50
Console.WriteLine(cost);   // Outputs: 19.99
```

**Best Practices: When to Use a Tuple**

* Use them for internal, transient data structures within a local method, a private loop, or as private method return types.
* Do not use them for public APIs or long-lived data structures. If the data structure needs to be exposed publicly across multiple application layers, it is much better to define a formal record or class.  

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

- The method receives a reference to the original variable.
- Changes made to the parameter affect the original variable.
- In C#, use the ref or out keyword to pass by reference.

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

The **JIT Compiler** (Just-In-Time) is a core component of the `.NET` CLR (Common Language Runtime) that converts Intermediate Language (IL) code into native machine code at runtime, just before the code execution.

**JIT Compilation Flow in .NET**

```
C# Source Code
        |
C# Complier (cs.exe)
        |
MSIL (Intermediate Language)
        |
Assembly (.exe/dll)
        |
CLR Loads Assembly
        |
JIT Compiler
        |
Native Machine Code
        |
CPU Executes Code
```

**JIT Compilation Process:**

1. **Source Compiler IL:** The C# compiler (`csc` / `dotnet build` using **Roslyn**) compiles source code into **Intermediate Language (IL)** and stores it in assemblies (`.dll` / `.exe`).
2. **Assembly Loading:** The CoreCLR loads the required assemblies at startup.
3. **JIT Compilation:** When a method is called for the first time, the JIT compiler translates its IL to **native machine code** optimized for the current CPU (x64, Arm64, etc.).
4. **Caching:** The native code is cached in memory so subsequent calls execute directly without re-compilation.
5. **Execution:** The CPU runs the native code.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are types of JIT compiler available in .NET?

In .NET, JIT compilation is commonly categorized into these types:

**1. Normal JIT**

Compiles a method the first time it is called, then caches the native code for later calls. This is the standard behavior.

**2. Pre-JIT**

Compiles IL to native code ahead of execution (historically via NGen; in modern .NET, ReadyToRun and Native AOT are the relevant ahead-of-time approaches).

**3. Econo JIT (historical)**

Used limited memory by compiling methods on demand and discarding compiled code when possible. This is obsolete in modern .NET.

**Note:**
In modern .NET (Core/.NET 5+), the main runtime JIT engine is **RyuJIT**, and it uses tiered compilation:

- Tier 0: quick initial code generation
- Tier 1: optimized recompilation for hot methods (often with Dynamic PGO)

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

## Q. What is a parameter? Explain the new types of parameters introduced in C#?

A **parameter** in C# is a variable defined in a method, constructor, or indexer declaration that receives a value (called an argument) when the method is called. Parameters allow you to pass data into methods so they can operate on different values.

C# introduced specialized parameter modifiers aimed at optimizing memory and performance, particularly when passing value types

* **`in` Parameters**: Passes an argument by reference but makes it strictly read-only. It prevents copying large struct variables while guaranteeing the method cannot modify the original data. 

* **`out` Variables**: Allows you to declare an output parameter directly inside the method call argument list (e.g., `int.TryParse("123", out int result)`), eliminating the need for separate variable declarations. 

* **`ref` readonly Parameters**: Combines reference passing with read-only constraints, allowing methods to accept both references and values (like literals or constants) without creating unnecessary copies. 

* **Optional Parameters**: Allows you to assign a default value to a parameter inside the method signature (e.g., `void Log(string msg, int level = 1)`). Callers can omit this argument entirely.  

* **Named Parameters**: Allows you to pass arguments by matching their explicit name followed by a colon (e.g., `Log(msg: "Error", level: 3)`). This lets you pass arguments in any order or skip certain optional parameters.  

* **Dynamic Parameters**: Uses the dynamic keyword to bypass compile-time type checking. The exact type and operations are resolved at runtime, which is highly useful for interoperability. 

* **Primary Constructor Parameters**: Introduced for standard classes and structs (extending C# 9 records), these let you define parameters directly in the class declaration line. They automatically map to fields and are visible throughout the entire scope of the class. 

**Example:**

```cs
using System;

namespace ParameterDemo
{
    // C# 12 Primary Constructor Parameter
    // 'id' and 'name' are available throughout the entire class scope
    public class User(int id, string name)
    {
        public void DisplayUser() => Console.WriteLine($"User {id}: {name}");
    }

    // A large struct to demonstrate performance optimization
    public struct LargeDataPoint
    {
        public double X;
        public double Y;
        public double Z;
    }

    class Program
    {
        static void Main(string[] args)
        {
            // 1. Primary Constructor Example
            User newUser = new User(101, "Alice");
            newUser.DisplayUser();

            // 2. Named and Optional Parameters Example
            // We omit 'prefix', so it uses the default value "LOG:"
            // We use named parameters to pass 'message' and 'level' out of order
            WriteLog(level: 3, message: "System initialized.");

            // 3. Inline 'out' Parameter Example
            // The variable 'parsedValue' is declared right inside the method call
            if (int.TryParse("456", out int parsedValue))
            {
                Console.WriteLine($"Successfully parsed out value: {parsedValue}");
            }

            // 4. 'in' Parameter Example
            LargeDataPoint point = new LargeDataPoint { X = 1.0, Y = 2.0, Z = 3.0 };
            // Passed by reference (no copy made), but read-only
            ProcessCoordinates(in point);

            // 5. Dynamic Parameter Example
            // Bypasses compile-time checking; resolved at runtime
            dynamic dynamicString = "Hello Dynamic World!";
            PrintLength(dynamicString);
        }

        // Method with an optional parameter (prefix)
        static void WriteLog(string message, int level, string prefix = "LOG:")
        {
            Console.WriteLine($"{prefix} [{level}] {message}");
        }

        // 'in' modifier guarantees 'data' cannot be modified inside this method
        static void ProcessCoordinates(in LargeDataPoint data)
        {
            // data.X = 10.0; // ERROR: This line would fail to compile!
            Console.WriteLine($"Processing coordinate X: {data.X}");
        }

        // Dynamic parameter resolves operations at runtime
        static void PrintLength(dynamic item)
        {
            // The compiler does not check if '.Length' exists until this runs
            Console.WriteLine($"The length of the item is: {item.Length}");
        }
    }
}
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

In C#, the equivalent of a **sub-procedure** (from languages like VB/VBA) is a **method with void return type**, while a **function** is a method that **returns a value.**

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

In C#, `string` and `StringBuilder` both handle text, but string is **immutable**, and StringBuilder is **mutable**. This means that when you modify a string, a new string object is created, while with StringBuilder, you can modify the object in place without creating new objects. 

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

* Use `string` when you have a fixed set of text data, perform very few modifications, or are simply concatenating a small, known number of strings.
* Use `StringBuilder` when you are modifying text an unknown number of times, such as inside a for or foreach loop, or when building massive blocks of text dynamically.

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

**Example 1: Late binding using `dynamic`:**

```cs
using System;

public class Greeter
{
    public void SayHello(string name)
    {
        Console.WriteLine($"Hello, {name}!");
    }
}

class Program
{
    static void Main()
    {
        dynamic obj = new Greeter();
        obj.SayHello("Pradeep"); // Method resolved at runtime
        // Output: Hello, Pradeep!

        // The type can change at runtime
        obj = 42;
        Console.WriteLine(obj + 8); // Output: 50
    }
}
```

**Example 2: Late binding using Reflection:**

```cs
using System;
using System.Reflection;

public class Calculator
{
    public int Add(int a, int b) => a + b;
}

class Program
{
    static void Main()
    {
        // Load type and invoke method at runtime — no compile-time knowledge needed
        Type type = typeof(Calculator);
        object instance = Activator.CreateInstance(type);

        MethodInfo method = type.GetMethod("Add");
        object result = method.Invoke(instance, new object[] { 10, 20 });

        Console.WriteLine($"Result: {result}"); // Output: Result: 30
    }
}
```

**Real-World Usage**

* **Early Binding**: Business applications, APIs, enterprise applications(.NET Core, ASP.NET Core)
* **Late Binding**: Plugin architectures, loading external assemblies, COM Interop, dependency discovery at runtime.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Indexer in C#?

An indexer in C# is a special type of property that allows objects of a class or struct to be indexed just like arrays, using the square bracket `[]` syntax. Indexers enable you to access elements in an object using an index, making custom classes behave like collections.

**Key Benefits:**

* **Intuitive Syntax**: Users can access internal collections using familiar `obj[index]` brackets instead of calling explicit methods.
* **Simplified Data Access**: They abstract away the underlying data structure, making your custom collection feel like a native array.
* **Property-Like Flexibility**: Indexers can use get and set accessors, allowing you to validate data or run logic during access.
* **Overloading Support**: You can define multiple indexers on the same class if they use different data types for the index (e.g., indexing by int vs. indexing by string).
* **Multi-Dimensional Indexing**: They support multiple parameters, enabling clean access to grid-like or matrix data structures. 

**Example:**

```cs
public class MyCollection
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
var collection = new MyCollection();
collection[0] = "Hello";
Console.WriteLine(collection[0]); // Output: Hello
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the differences between Object, Var and Dynamic type?

In C#, `object` is the ultimate base class checked at compile-time, `var` is shorthand for a specific compile-time type, and `dynamic` bypasses compile-time checking entirely until runtime..

**Key Differences**

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

**Detailed Comparison:** 

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

**Comparison:**

| Feature           | Eager Evaluation           | Lazy Evaluation             |
|------------------|---------------------------|-------------------------------|
| Execution Time   | Immediately               | On demand                     |
| Use cases        | Always-needed values      | Expensive/optional values     |
| LINQ Examples    | `.ToList()`, `ToArray()`, `Count()`| `.Where()`, `Select()`, `Skip()`|
| Memory Usage     | Higher                    | Lower                         |
| Performance      | Better when results are needed repeatedly| Better for large datasets|


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
```

```cs
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

The primary difference is that `typeof` returns a `System.Type` object representing metadata about a type, whereas `sizeof` returns an integer indicating the memory footprint (in bytes) of an unmanaged type

**1. `typeof()` Operator:**

- Returns the `System.Type` object for a given type.
- Used to get metadata information about a type at compile time.
- Commonly used with reflection.

**Example:** `typeof()` is commonly used with Reflection

```cs
if(obj.GetType() == typeof(Employee))
{
    Console.WriteLine("Employee Object");
}
```

**2. `sizeof()` Operator:**

- Returns the size (in bytes) of a value type.
- Used to determine how much memory a type occupies.
- Only works with primitive types (like int, char, float, etc.) unless used in an unsafe context.

**Example:**

```cs
Console.WriteLine(sizeof(byte));  // Output: 1
Console.WriteLine(sizeof(short)); // Output: 2
Console.WriteLine(sizeof(int));   // Output: 4
Console.WriteLine(sizeof(long));  // Output: 8
Console.WriteLine(sizeof(char));  // Output: 2
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

Widening conversion (Implicit) converts a smaller data type into a larger data type without losing data. Narrowing conversion (Explicit) converts a larger data type into a smaller data type, which can cause data loss.

**1. Widening Conversion (Implicit Conversion):**

- Converts a value to a larger or more general type.
- No data loss; safe and automatic.

**Example:** `int` to `long`, `float` to `double`.

```cs
int num = 100;
long result = num;      // Widening: int to long (implicit)
float flt = num;       // Widening: int to float (implicit)

Console.WriteLine(result); // Output: 100
Console.WriteLine(flt);    // Output: 100
```

Common Widening Conversions

| From          | To               |
|---------------|------------------|
| byte          | short, int, long, float, double, decimal|
| short         | int, long, float, double, decimal|
| int           | long, double, decimal |
| float         | double |

**2. Narrowing Conversion (Explicit Conversion):**

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

To view a compiled .NET Assembly (a `.dll` or `.exe` file), you must decompile it using an assembly inspector tool to reconstruct the original C# code or read its Intermediate Language (IL) metadata. Because C# compiles into a standardized byte-code, these tools can instantly deconstruct an assembly back into readable source code.

**1. Using IL Disassembler:**

IL Disassembler is a tool provided with the .NET SDK to view the contents of an assembly (DLL or EXE).

**Steps:**

1. Open the Developer Command Prompt for Visual Studio.
2. Run:
```cs
ildasm YourAssembly.dll
```
3. The IL Disassembler window will open, allowing you to browse namespaces, classes, methods, and view IL code.

**2. Using ILSpy (Third-Party Tools):**

- [ILSpy](https://github.com/icsharpcode/ILSpy) are free .NET decompilers.
- Open your `.dll` or `.exe` file in these tools to view C# code, metadata, and resources.

**Example:**

```
MyLibrary.dll
  |-------Services
            |-------EmployeeService
                      |------GetEmployee()
                      |------SaveEmployee()   
```

**3. Using Visual Studio:**

- Right-click on a reference in Solution Explorer -> "Go to Definition" to view metadata.
- Use "Object Browser" (View -> Object Browser) to explore assemblies.

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

In C#, multilingual applications are software programs that leverage the built-in `System.Globalization` namespace to dynamically adapt their user interface, text, and formatting based on a user\'s language and region. This is typically achieved using resource files (`.resx`) and the .NET localization framework.

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

**Common .NET Classes Used**

|Class | Purpose  |
|-----------------|-------------------------|
|CultureInfo      |Language and region information|
|Resource Manager |Load localized resources |
|IStringLocalizer |ASP.NET Core localization |
|CurrentCulture   |Date/number formatting    |
|CurrentUICulture |UI Language selection     |



<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Describe the process of code compilation in .NET?

The .NET compilation process uses a two-stage compilation model to transform source code into machine-executable instructions. It converts high-level code (like C# or F#) into **Common Intermediate Language (CIL)**, which the **Just-In-Time (JIT)** compiler then translates into native machine code at runtime. 
Here is the step-by-step breakdown of how the .NET compilation process works:

**1. Compile Source Code to Intermediate Language (Compile Time)**

* **Language Compiler**: You write code in a .NET-compliant language like C#, VB.NET, or F#.  
* **Source Translation**: When you build the project, a language-specific compiler (like csc for C#) compiles the source code.  
* **CIL Generation**: The compiler outputs Common Intermediate Language (CIL), also known as Microsoft Intermediate Language (MSIL) or simply IL.  
* **Metadata Creation**: The compiler simultaneously generates metadata, which contains descriptions of your code\'s types, members, and dependencies.  
* **Assembly Packaging**: The CIL and metadata are packaged into a portable executable (PE) file, typically an `.exe` or `.dll` file, known as a **.NET Assembly**. 

**2. Load the Assembly (Runtime)**

* **Execution Trigger**: The user or a system process executes the .NET Assembly.
* **CLR Initialization**: The operating system starts the Common Language Runtime (CLR), which is the virtual machine engine of .NET.
* **Assembly Loading**: The CLR reads the assembly\'s metadata to understand its dependencies and structure, preparing the environment for execution. 

**3. Just-In-Time (JIT) Compilation (Runtime execution)**

* **On-Demand Compilation**: The CLR does not compile the entire assembly at once. Instead, the JIT Compiler (**RyuJIT**) compiles individual methods only when they are called for the first time.  
* **Native Code Translation**: The JIT compiler takes the platform-independent CIL from the assembly and translates it into highly optimized native machine code (CPU instructions) specific to the host operating system and hardware architecture (e.g., `x64`, `ARM64`). 
* **Caching**: The compiled native code is saved in memory. Subsequent calls to the same method bypass the JIT compiler and execute the native code directly, maximizing performance. 


**Alternative: Ahead-Of-Time (AOT) Compilation**

Modern .NET also supports **Native AOT compilation**. If enabled, this alternative process completely bypasses the runtime JIT stage. It compiles the source code directly into a single, platform-specific native binary during development. This results in faster startup times and a smaller memory footprint, though it removes certain dynamic runtime capabilities. 

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you return multiple values from a function in C#?

Yes, you can return multiple values from a function in C#. There are several common ways to achieve this:

**1. Using Tuples:**

Tuples allow you to return multiple values of different types in a single return statement.

```cs
// Definition using named tuple elements
(string Name, int Age) GetPerson()
{
    return ("Pradeep", 20);
}

// Usage
var person = GetPerson();
Console.WriteLine(person.Name); // Pradeep
Console.WriteLine(person.Age);  // 20

// Usage with Deconstruction (splitting directly into individual variables)
var (name, age) = GetPerson();
Console.WriteLine($"{name} is {age}"); // Pradeep is 20
```

**2. Using Out Parameters:**

The Out parameters allow a function to modify the values of variables passed as arguments. This is a way to "return" additional values indirectly.

```cs
// Definition
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

The basic structure of a C# program consists of five core organizational building blocks: **Namespaces**, **Classes**, the **Main Method**, **Statements**, and **Directives**. With C# 9+ **Top-Level Statements**, you can also write programs without explicit class or `Main` boilerplate.

**1. Traditional program structure (all versions):**

```cs
using System; 

namespace MyFirstApplication 
{
    class Program 
    {
        static void Main(string[] args) 
        {
            Console.WriteLine("Hello, World!"); 
        }
    }
}
```

**The 5 Structural Building Blocks**

* **`using System;` (Directive)**: Imports the System namespace so you can use built-in classes (like Console) without typing their full paths.

* **`namespace MyFirstApplication` (Container)**: Organizes your code and prevents naming conflicts by grouping related classes together.

* **`class Program` (Blueprint)**: All executable C# code must live inside a class. Classes act as containers for data (fields) and actions (methods).

* **`static void Main(string[] args)` (Entry Point)**: The critical starting point where the operating system begins executing your application.

* **`Console.WriteLine(...)` (Statement)**: An individual instruction that performs an action. Every standalone statement in C# must end with a semicolon (;).  

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

Access modifiers are keywords used to set the **accessibility (visibility) level** of classes, methods, fields, and properties in C#. They enforce encapsulation by restricting which parts of your program can see or interact with specific code blocks. C# has six access modifiers:

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
public class Account
{
    // Accessible anywhere
    public string AccountNumber;

    // Accessible only within this Account class
    private decimal balance;

    // Accessible in Account and any child class (e.g., SavingsAccount)
    protected string OwnerName;

    // Accessible anywhere inside this specific project file/assembly
    internal string BankBranch;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is string interpolation in C# and how is it used?

String interpolation provides a concise syntax to embed expressions directly inside string literals using the `$` prefix. It is the preferred way to format strings in modern C#.

**1. Basic Interpolation:**

```cs
string name = "Pradeep";
int age = 28;

// Using string interpolation
string message = $"Name: {name}, Age: {age}";
Console.WriteLine(message); // Output: Name: Pradeep, Age: 28
```

**2. Expressions Inside `{}`:**

```cs
int a = 10, b = 5;

Console.WriteLine($"Sum: {a + b}, Product: {a * b}"); // Output: Sum: 15, Product: 50
```

**3. Format Specifiers:**

```cs
double price = 1234.567;

Console.WriteLine($"Price: {price:C2}");  // Output: Price: $1,234.57 (currency)
Console.WriteLine($"Price: {price:F1}");  // Output: Price: 1234.6 (1 decimal)
Console.WriteLine($"Hex: {255:X}");       // Output: Hex: FF

// Formatting Numbers and Dates
decimal price = 19.99m;
DateTime today = DateTime.Now;

string formattedPrice = $"Price: {price:C}";            // Outputs: Price: $19.99 (based on local currency)
string formattedDate  = $"Today is {today:yyyy-MM-dd}"; // Outputs: Today is 2026-07-17
```

**4. Multi-line with `$@` or `@$` (verbatim interpolated string):**

```cs
string path = "C:\\Users";
string msg = $@"Hello {name},
Your path is: {path}";

Console.WriteLine(msg);
```

**5. Raw Interpolated String:**

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

* `+`  : Addition (also used for string concatenation).
* `-`  : Subtraction (or unary negation).
* `*`  : Multiplication.
* `/`  : Division.
* `%`  : Modulus (returns the remainder of division).
* `++` : Increment (increases a value by 1).
* `--` : Decrement (decreases a value by 1). 

**Example:**
```cs
int a = 10, b = 3;

Console.WriteLine(a + b); // Output: 13
Console.WriteLine(a - b); // Output: 7
Console.WriteLine(a * b); // Output: 30
Console.WriteLine(a / b); // Output: 3
Console.WriteLine(a % b); // Output: 1
```

**2. Relational and Comparison Operators**

These operators compare two values and return a boolean result (`true` or `false`).

- `==` : Equal to
- `!=` : Not equal to
- `>`  : Greater than
- `<`  : Less than
- `>=` : Greater than or equal to
- `<=` : Less than or equal to

**Example:**
```cs
int a = 5, b = 10;

Console.WriteLine(a == b) // Output: False
Console.WriteLine(a < b) // Output: True
```

**3. Boolean Logical and Conditional Operators**

These operators perform logical operations on boolean expressions.

* `&&` : Conditional logical AND (short-circuiting evaluation).
* `||` : Conditional logical OR (short-circuiting evaluation).
* `!`  : Logical NOT (inverts a Boolean state).
* `&`  : Logical AND (evaluates both sides regardless).
* `|`  : Logical OR (evaluates both sides regardless).
* `^`  : Logical XOR (exclusive OR). 

**Example:**
```cs
bool isAdult = true;
bool hasID = false;

Console.WriteLine(isAdult && hasID); // Output: False
Console.WriteLine(isAdult || hasID); // Output: True
```

**4. Assignment Operators**

These operators assign values to variables.

* `=` : Simple assignment.
* `+=` , `-=` : Add/Subtract and assign.
* `*=` , `/=` , `%=` : Multiply/Divide/Modulus and assign.
* `&=` , `|=` , `^=` : Bitwise/Logical operations and assign.
* `<<=` , `>>=` , `>>>=` : Shift and assign.
* `??=` : Null-coalescing assignment (assigns only if the left-hand variable is null).  

**Example:**
```cs
int x = 5;
x += 3; // x = x + 3

Console.WriteLine(x); // Output: 8
```

**5. Bitwise and Shift Operators**

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

Bitwise operators in C# are used to perform bit level operations on integer types like `int`, `uint`, `long`, `ulong`, `bytes`, etc. These operators treat their operands as a sequence of bits rather than as decimal, hexadecimal, or octal numbers. They are highly efficient and commonly used for performance-critical tasks, cryptography, hardware communication, and managing configuration flags.

**Overview:**

| Operator   | Symbol | Description  |
|------------|--------|--------------|
|AND         |  &     | Sets each bit to 1 if both bits are 1|
|OR          |   \|   | Sets each bit to 1 if at least one of the corresponding bits is 1, otherwise 0.|
|XOR         |  ^     | Sets each bit to 1 if only one of two bits is 1|
|NOT         |  ~     | Inverts all the bits (0 becomes 1, 1 becomes 0).|
|Left Shift  |  <<    | Shifts bits to the left, filling empty spaces with 0.|
|Right Shift |  >>    | Shifts bits to the right, preserving or dropping sign bits.|

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

**Example: 01**

```cs
object message = "Hello World";

// 1. Traditional Casting (Throws exception if it fails)
string text1 = (string)message; 

// 2. The 'as' Operator (Returns null if it fails)
string text2 = message as string; 
```

**The Best Way to Use It**

The absolute best practice when using the as operator is to immediately follow it with a null check. If you do not check for null, your code will eventually crash with a `NullReferenceException` when you try to use the variable.  

**1. The Dangerous Way**

```cs
public void ProcessData(object input)
{
    // If input is an integer, 'text' becomes null
    string text = input as string; 
    
    // CRASH! Throws NullReferenceException if input wasn\'t a string
    Console.WriteLine(text.ToUpper()); 
}
```

**2. The Best Practice** 

```cs
public void ProcessData(object input)
{
    string text = input as string;
    
    if (text != null)
    {
        // Safe to use here
        Console.WriteLine(text.ToUpper());
    }
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

The `is` **operator** checks if an object is compatible with a specific type and returns a boolean (`true`/`false`), while the `as` **operator** attempts to convert an object to a specific type, returning `null` if the conversion fails.

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

**Nullable types** in C# allow value types (like `int`, `bool`, `double`, or `structs`) to represent a `null` value, indicating the absence of data. By default, regular value types cannot be `null` and must always hold a value (e.g., `int` defaults to `0`).

In C#, **nullable types** cover two distinct concepts:

**1. Nullable Value Types (`T?` / `Nullable<T>`):**

Allow value types (e.g., `int`, `bool`, `DateTime`) to represent `null`, useful for optional database fields or missing data.

**Key members:**

* `HasValue` — `true` if the variable holds a non-null value.
* `Value` — gets the value (throws `InvalidOperationException` if `null`).
* `GetValueOrDefault()` — returns the value or the type\'s default.

**Example:**

```cs
int? score = null;

if (score.HasValue)
    Console.WriteLine($"Score: {score.Value}");
else
    Console.WriteLine("Score is not set."); // Output: Score is not set.

int fallback = score.GetValueOrDefault(-1); // -1
```

**2. Nullable Reference Types:**

Enable compile-time null safety for **reference types**. Enabled project-wide with `<Nullable>enable</Nullable>` in the `.csproj` (default in .NET 6+ projects).

**Example:**

```cs
// Without nullable context: string can be null silently (old behavior)
// With nullable context:
string nonNullable = "Pradeep"; // Cannot be null — compiler warns if you try
string? nullable = null;        // Explicitly nullable — must check before use

int length = nullable?.Length ?? 0; // Safe with null-conditional + null-coalescing
Console.WriteLine(length); // Output: 0
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Type Casting and what are its types in C#?

Type casting in C# is the process of converting a variable from one data type to another. This is often necessary when working with different types of data, such as converting an `int` to a `double`, or casting a base class reference to a derived class.

There are two main types of type casting in C#:

**1. Implicit Casting (Automatically)**

C# performs implicit casting automatically when converting a smaller type to a larger type size, or when converting a derived class to a base class.

* **Safety**: Completely safe.
* **Data Loss**: No data loss occurs.

**Examples:**  

```cs
int integerNumber = 9;

// Automatic conversion: int to double
double doubleNumber = integerNumber; 

Console.WriteLine(doubleNumber); // Output: 9
```

**2. Explicit Casting (Manually)**

Explicit casting is required when converting a larger type to a smaller type size, or when converting a base class back to a derived class. You must place the target type in parentheses () in front of the value.

* **Safety**: Unsafe; requires developer intent.
* **Data Loss**: Potential for data loss or truncation.

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

In C#, the primary difference is that `==` is an operator evaluated at **compile-time**, whereas `.Equals()` is a virtual method evaluated at **runtime**. While both are used to test for equality, they can produce entirely different results depending on how the data types are defined and cast.

**1. Value Types (e.g., `int`, `double`, `bool`)** 

For built-in value types, both `==` and `.Equals()` behave exactly the same way. They compare the actual content or underlying values.  

```cs
int a = 5;
int b = 5;

Console.WriteLine(a == b);       // True
Console.WriteLine(a.Equals(b));  // True
```

**2. Reference Types (Classes)**

By default, both check for **reference equality**—whether both variables point to the exact same memory location on the heap. However, the behavior changes significantly if a class overrides `.Equals()` but does not overload `==`.  

```cs
// Example using a class that overrides Equals() but doesn\'t overload ==
public class Person {
    public string Name { get; set; }
    public override bool Equals(object obj) => obj is Person p && p.Name == this.Name;
}

Person p1 = new Person { Name = "Alice" };
Person p2 = new Person { Name = "Alice" };

Console.WriteLine(p1 == p2);       // False (Different spots in memory)
Console.WriteLine(p1.Equals(p2));  // True (Custom logic checks the Name)
```

**3. The String Exception**

The `string` class is a reference type, but Microsoft intentionally overloaded both `==` and `.Equals()` to compare the **characters/content** inside the string rather than the memory pointer.  

```cs
string s1 = new string(new char[] {'h', 'e', 'l', 'l', 'o'});
string s2 = new string(new char[] {'h', 'e', 'l', 'l', 'o'});

Console.WriteLine(s1 == s2);       // True (Overloaded to check content)
Console.WriteLine(s1.Equals(s2));  // True (Overridden to check content)
```

**Best Practices**

* Use `==` when comparing primitive value types or strings.
* Use `.Equals()` when working with polymorphic objects, generics, or classes with custom value-matching logic.
* Use `object.ReferenceEquals(a, b)` if you explicitly need to verify if two references point to the exact same memory address, bypassing any custom equality logic.  

**Key Differences:**

| Aspect                | `==` Operator                        | `.Equals()` Method                |
|-----------------------|--------------------------------------|-----------------------------------|
| Reference Types       | Reference equality (unless overloaded)| Reference equality (unless overridden) |
| Value Types           | Value equality                       | Value equality (overridden)       |
| Overridable           | Yes (operator overloading)           | Yes (method override)             |
| Null Handling         | Safe (returns false if either is null)| Throws if called on null instance |

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

The `==` operator checks for **value equality** on primitive types (like `integers`) and **reference equality** on classes by default. However, its behavior can be modified if a type overloads the operator (such as the `string` class, which checks for value equality).

**Example:**

```cs
int a = 5, b = 5;
bool result = a == b; // true

string s1 = "hello", s2 = "hello";
bool isEqual = s1 == s2; // true (string overloads ==)
```

**2. `.Equals()` Method**

Inherited from `System.Object`, this virtual method can be overridden by any class or struct to provide custom **value-based comparison** logic. If not overridden, it defaults to reference equality for classes.

**Example:**

```cs
object obj1 = "Hello";
object obj2 = "Hello";

bool isEqual = obj1.Equals(obj2); // true
```

**3. `Object.ReferenceEquals()`**

This method guarantees a **strict memory-address comparison**, completely bypassing any overridden equality methods or overloaded operators. It determines whether two references point to the exact same object instance.

**Example:**

```cs
object obj1 = new object();
object obj2 = obj1;

bool isEqual = ReferenceEquals(obj1, obj2); // true
```

**4. `Object.Equals(a, b)`**

This static utility helper handles **null safety** natively. It returns `true` if both elements are `null`, `false` if only one is `null`, and calls the virtual `Equals()` method if both references exist.

**Example:**

```cs
bool isEqual = Object.Equals(objA, objB);
```

**5. `IEquatable<T>.Equals()`**

Implementing `IEquatable<T>` provides a strongly typed `Equals(T other)` method. It improves execution speed by preventing performance overheads like boxing and unboxing when evaluating structs and value types.

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
public const double Pi = 3.14159; // Evaluated when the app builds
```

**2. `readonly`**

- Value can be assigned at declaration or in the constructor.
- Value can differ per instance (unless also static).
- Value cannot change after construction.
- Can be any type (including reference types).

**Example:**
```cs
public readonly DateTime ConnectionTime = DateTime.Now; // Evaluated when object is created
```

**3. `static`**

- Belongs to the type itself, not to any instance.
- Shared across all instances.
- Can be changed at runtime (unless also readonly/const).
- Can be used with fields, methods, constructors, and classes.

**Example:**
```cs
public static int TotalUsers = 0; // Shared across the entire application lifetime
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
public enum Days { Monday, Tuesday, Wednesday }

foreach (Days day in Enum.GetValues<Days>())
{
    Console.WriteLine(day);
}
```

**Output:**
```
Monday
Tuesday
Wednesday
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

**1. Direct Explicit Casting:** 

This is the fastest and most common method

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

**2. Defensive Conversion (Safe Handling):**

C# allows casting any integer to an enum, even if that value is not defined in the enum structure. To prevent unexpected bugs, validate the integer first using `Enum.IsDefined`.

**Example:**

```cs
int input = 99;

if (Enum.IsDefined(typeof(Status), input))
{
    Status currentStatus = (Status)input;
    // Proceed safely
}
else
{
    // Handle invalid enum value case
}
```

**3. Dynamic Conversion**

If the enum type is only known at runtime as a Type object, use **Enum.ToObject**:

**Example:**

```js
Type enumType = typeof(Status);
int numericValue = 2;

object enumObject = Enum.ToObject(enumType, numericValue);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is BigInteger Data Type in C#?

The `BigInteger` data type in C# is a structure provided by the `System.Numerics` namespace that allows you to work with arbitrarily large integers—much larger than the built-in numeric types like `int` or `long`. Unlike these fixed-size types, `BigInteger` can represent numbers of any size and precision, limited only by the available system memory.

**Key Points:**

- `BigInteger` is used when you need to handle numbers larger than `long.MaxValue` (9,223,372,036,854,775,807).
- It supports all standard arithmetic operations (`+`, `-`, `*`, `/`, `%`, etc.).
- It is immutable—operations return a new `BigInteger` instance.
- Since it handles dynamic memory allocation, it is significantly slower than native primitive types like `int` or `long`.

**Example:**

```cs
using System;
using System.Numerics;

class Program
{
    static void Main()
    {
        // Parse a massive number from a string
        BigInteger massiveNumber = BigInteger.Parse("1234567890123456789012345678901234567890");

        // Multiply it
        BigInteger result = massiveNumber * 2;

        Console.WriteLine(result); // Output: 246913578024691357802469135780
    }
}
```

**Common Use Cases**

- **Cryptography**: Generating and processing massive prime numbers or public/private keys.

- **Mathematical Simulations**: Calculating massive factorials, Fibonacci sequences, or astronomical values.

- **Financial Modeling**: High-precision calculations where fractional parts are converted to large whole numbers to avoid floating-point drift

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

To convert an object to JSON in C#, use the `JsonSerializer.Serialize` method from the `System.Text.Json` namespace or `Newtonsoft.Json` (also known as `Json.NET`) library. The most common and modern approach is with `System.Text.Json`.

**1. Using System.Text.Json:**

You can convert an object to a JSON string in C# using the built-in `System.Text.Json` namespace:

**Example:** 

```cs
using System.Text.Json;

var person = new { Name = "Pradeep", Age = 30 };

// Convert object to JSON string
string json = JsonSerializer.Serialize(person);

Console.WriteLine(json); // Output: {"Name":"Pradeep","Age":30}
```

**2. Using Newtonsoft.Json:**

If you are working on an older project or legacy codebase using the third-party `Newtonsoft.Json` package:

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

To convert a JSON string back into a C# object, use the `JsonSerializer.Deserialize<T>` method from the `System.Text.Json` namespace or the popular third-party library `Newtonsoft.Json` (`Json.NET`).

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

If you are working in a legacy project utilizing the third-party `Newtonsoft.Json` package:

```cs
Install-Package Newtonsoft.Json
```

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
- For dynamic or anonymous types, you can use `JsonDocument` (`System.Text.Json`) or `JObject` (`Newtonsoft.Json`).

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How to Pass or Access Command-line Arguments in C#?

You can pass or access command-line arguments in C# either through the `args` parameter in the **Main** entry point or by using the global `Environment.GetCommandLineArgs()` method.

**1. Using the Main Method Parameter**

This is the most common way. If you are using modern C# with **Top-Level Statements**, the `string[] args` array is automatically available globally in your file without declaring it.

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

**2. Using `Environment.GetCommandLineArgs()`**

Use this if you need to access arguments from deep within a helper class or library where args wasn\'t passed down.

**Example:**

```cs
using System;

string[] allArgs = Environment.GetCommandLineArgs();

// Index 0 is the app executable path
Console.WriteLine($"Application Path: {allArgs[0]}"); 

if (allArgs.Length > 1)
{
    // Index 1 is the first actual user argument
    Console.WriteLine($"First User Argument: {allArgs[1]}"); 
}
```

This includes the executable name as the first element(`args[0]`), unlike the Main method\'s args which starts from the first actual argument.

## Q. How to convert date object to string in C#?

You can convert a date object to a string in C# by calling the `ToString()` method on a `DateTime` or `DateTimeOffset` instance.

**1. Default Format:**

C# provides built-in, culture-aware shorthand format specifiers.

```cs
DateTime now = DateTime.Now;

string shortDate = now.ToString("d");    // Output: 09-08-2026 (Short date format)
string longDate  = now.ToString("D");    // Output: Sunday, 09 August 2026 (Long date format)
string universal = now.ToString("u");    // Output: 2026-08-09 14:26:00Z (Universal sortable)
```

**2. Custom Format:**

You can pass a specific pattern using custom tokens like `yyyy` (year), `MM` (month), and `dd` (day) for precise layout control.

```cs
DateTime now = DateTime.Now;

string custom1 = now.ToString("yyyy-MM-dd");          // Output: 2026-08-09
string custom2 = now.ToString("dd/MM/yyyy HH:mm:ss"); // Output: 09/08/2026 14:26:00
string custom3 = now.ToString("MMMM dd, yyyy");       // Output: August 09, 2026
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

You can combine two arrays without duplicate values in C# by using the `Union` extension method from the `System.Linq` namespace.

**1. Using LINQ Union**

The `Union` method automatically merges both arrays and removes duplicates while producing a single, clean sequence.

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

        // Merge and filter duplicates, then convert back to array
        int[] combinedArray = array1.Union(array2).ToArray();

        Console.WriteLine("Combined Array: " + string.Join(", ", combinedArray)); // Output: 10, 20, 30, 50
    }
}
```

**2. Using HashSet (High-Performance Approach):**

For massive datasets, initializing a `HashSet` is much faster because it uses a hash table structure to enforce uniqueness instantly.

**Example:**

```cs
using System;
using System.Linq;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        int[] array1 = { 10, 20, 30 };
        int[] array2 = { 30, 50, 10 };

        // Initialize set with the first array
        var uniqueSet = new HashSet<int>(array1);

        // UnionWith modifies the set to include elements of the second array, skipping duplicates
        uniqueSet.UnionWith(array2);

        // Copy back to an array structure
        int[] combinedArray = uniqueSet.ToArray();

        Console.WriteLine("Combined Array: " + string.Join(", ", combinedArray)); // Output: 10, 20, 30, 50
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How to convert string to int in C#?

You can convert a string to an int in C# using `int.TryParse`, `int.Parse`, or the `Convert.ToInt32` method

**1. int.Parse()**  

Use this only if you are absolutely certain the string is a valid integer. If the string is malformed or null, it throws an exception (`FormatException` or `ArgumentNullException`).

```cs
string str = "123";
int number = int.Parse(str); // number = 123
```

**2. int.TryParse()**  

This method attempts the conversion without crashing your app if the string is invalid or `null`. It returns a boolean indicating success.

```cs
string input = "123";

if (int.TryParse(input, out int result))
{
    // Conversion succeeded, 'result' now holds the value 123
    Console.WriteLine($"Success: {result}");
}
else
{
    // Conversion failed (e.g., input was letters or empty)
    Console.WriteLine("Invalid integer string.");
}
```

**3. Convert.ToInt32()**  

This functions similarly to `int.Parse`, but if the string input is `null`, it gracefully returns `0` instead of throwing a null exception. However, it will still crash if the string contains letters.

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

**Boxing** is the process of converting a value type (like an `int` or `struct`) into a reference type (`object`), which allocates memory on the managed heap. **Unboxing** is the explicit conversion back from the reference type (`object`) on the heap to the value type on the stack.

**1. Boxing:**  

- Boxing is the process of converting a **value type** to a **reference type** (specially, to object or to any interface type implemented by the value type). 
- The value is wrapped inside a `System.Object` and stored on the heap.

**Example:**

```cs
int number = 42;  // Value type on the stack

object boxed = number ;  // BOXING: Copying the value to a new object on the heap
```

- The value 42 (a value type) is wrapped inside an object (a reference type).
- This involves copying the value and storing it on the heap.

**2. Unboxing:**
 
**Unboxing** is the reverse process: converting a **reference type** back to a **value type**.

**Example:**

```cs
object obj = 42;

int unboxed = (int)obj; // UNBOXING: Explicitly casting back to a stack value
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

**Boxing** is the process of converting a value type (like `int`, `double`, or a `struct`) to a reference type (`object`). This involves allocating memory on the heap and copying the value, which is more expensive than working with value types on the stack.

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

The core difference is that `==` can be overridden to compare **values**, while `ReferenceEquals` strictly compares **memory addresses**.

**1. The `==` Operator**:

  - For **value types** (like `int`, `struct`), `==` compares the actual values.
  - For **reference types** (like classes), by default, `==` checks if both references point to the same object (reference equality). However, many classes (like `string`) **override** `==` to compare values instead.
  - Can be **overloaded** by custom types to provide value-based equality.

**Example:**

````cs
object a = new string("hello");
object b = new string("hello");

Console.WriteLine(a == b); // True, because string overrides == for value equality
````

**2. The `ReferenceEquals` Method**:

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

**1. Lambda Expressions:** Used to define inline functions, especially with LINQ, delegates, and events.

**Examples:**

```csharp
using System;
using System.Linq.Expressions;

class Program
{
    static void Main()
    {
        // Define an expression tree for a Lambda: x => x*x
		Func<int, int, int> add = (a, b) => a + b;
		Console.WriteLine(add(2, 3)); // Output: 5
    }
}
```

**2. LINQ Queries:** Commonly used in LINQ to filter, project, or transform data.

**Example:**

```csharp
using System;
using System.Linq;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        var numbers = new List<int> { 1, 2, 3, 4, 5 };
        
        var evenNumbers = numbers.Where(n => n % 2 == 0); 

        // Flatten the collection into a readable string
        Console.WriteLine(string.Join(", ", evenNumbers)); // Output: 2, 4
    }
}
```

**3. Event Handlers:** Can be use to define event handlers inline.

**Example:**

```csharp
using System;

class Program
{
    // Define a class that exposes a Click event
    public class CustomButton
    {
        public event EventHandler Click;

        // Method to trigger the event safely
        public void Push() => Click?.Invoke(this, EventArgs.Empty);
    }

    static void Main()
    {
        CustomButton button = new CustomButton();

        // Event Handlers: Define the event handler inline using lambda syntax
        button.Click += (sender, e) => { Console.WriteLine("Button clicked!"); };

        // Simulating a user clicking the button
        button.Push();
    }
}
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
        // Changed "Fun" to "Func"
        Expression<Func<int, int>> squareExpr = x => x * x;

        // Print the expression
        Console.WriteLine("Expression: " + squareExpr); // Output: Expression: x => (x * x)

        // Compile and invoke the expression 
        Func<int, int> square = squareExpr.Compile();
        Console.WriteLine("Result of square(5): " + square(5)); // Output: Result of square(5): 25
    }
}
```

**Summary:**  

The `=>` operator is used to define inline functions (lambdas) and concise member implementations, making code more readable and expressive, especially in LINQ and functional programming scenarios.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the null-conditional operator (?.) and how does it differ from the null-coalescing operator (??)?

In C#, the **null-conditional operator** (`?.`) safely tests for `null` before accessing members, properties, or methods of an object. If the object is `null`, the expression short-circuits and immediately returns `null` instead of throwing a `NullReferenceException`.

The **null-coalescing operator** (`??`), by contrast, is used to provide a default fallback value when an expression evaluates to `null`. It evaluates the left-hand side, and if it is `null`, returns the right-hand side.

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

The **default literal** (`default`) in C# produces the default value of a type. It initializes a type to its blank state: `null` for reference types, `0` or `0.0` for numeric types, `false` for booleans, and all-zero bit patterns for structs.

**Benefits**

- **Code Closeness**: You omit the type name when the compiler can infer it.

- **Simplifies Generics**: It initializes unknown generic parameters (`T`) cleanly.

- **Reduces Redundancy**: It replaces longer expressions like `default(int)` or `default(CancellationToken)`.

**Example:**

```cs
// 1. Variable initialization (Type is inferred from left side)
int number = default; // Sets to 0
string text = default; // Sets to null

// 2. Optional method parameters
void ProcessData(Guid id, CancellationToken token = default) { }

// 3. Returning from generic methods
T GetElementAt<T>(int index) => default; 
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain the is not pattern introduced in C#?

The `is not` pattern in C# allows you to check if an expression does not match a specific pattern or type. It is a readable, English-like syntax that negates a pattern check without requiring you to wrap the entire expression in parentheses and an exclamation point (`!`).

**Example:**

```csharp
// 1. Cleaner Null Checks (safely ignores overloaded != operators)
if (user is not null)
{
    Console.WriteLine(user.Name);
}

// 2. Type Checking
if (vehicle is not Truck)
{
    Console.WriteLine("This vehicle does not require a commercial driver license.");
}

// 3. Combining with Property Patterns
if (order is not { IsShipped: true })
{
    Console.WriteLine("This order is still processing or cancelled.");
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

The **Operator precedence** in C# determines the strict order in which different operators are evaluated within an expression. Operators with higher precedence are executed before operators with lower precedence.

**How It Affects Expressions**

When an expression contains multiple operators, precedence acts as the mathematical "order of operations." Without it, code evaluation would be unpredictable.

- **Order Evaluation**: In `5 + 3 * 2`, multiplication (`*`) has higher precedence than addition (`+`). The expression evaluates to `11`, not `16`.

- **Associativity**: When operators share the same precedence level, associativity controls the order. Most binary operators evaluate from **left to right** (`a + b + c`), while assignment operators evaluate from **right to left** (`x = y = z`).

- **Parentheses Override**: You can use parentheses `()` to explicitly override precedence rules. Wrapping an expression in parentheses forces that segment to evaluate first (e.g., `(5 + 3) * 2` equals `16`).

**Summary of Precedence (Highest to Lowest):**

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

The core difference between **pre-increment** (`++i`) and **post-increment** (`i++`) is the timing of when the variable\'s value is updated relative to when it is evaluated in an expression.

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
