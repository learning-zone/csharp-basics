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

## [L1: Fundamental (Entry-Level / Junior)](Fundamental.md)
Focus: Syntax, basic language constructs, and core type system.

* **Fundamentals**: Data types, variables, type system, and basic `C#` syntax.
* **Operators**: Arithmetic, comparison, logical, bitwise, and null-coalescing operators.
* **Control Flow**: Conditional statements (if/else, switch expressions) and loops (for, foreach, while).

## [L2: Intermediate (Junior-Mid / Developer)](Intermediate.md)
Focus: Object-oriented programming, collections, and common language features.

* [Classes and Structs](#-4-classes-and-structs): Fields, properties, constructors, methods, access modifiers, and records.
* [Inheritance and OOP](#-5-inheritance-and-oop): Base/derived classes, abstract classes, interfaces, and polymorphism.
* [Collections and Generics](#-6-collections-and-generics): List, Dictionary, HashSet, Stack, Queue, and IEnumerable.
* [File Handling](#-7-file-handling): StreamReader/Writer, File, Path, and Directory APIs.
* [Regular Expression](#-8-regular-expression): Regex patterns, matching, groups, and replacements.
* [Exception Handling](#-9-exception-handling): try/catch/finally, custom exceptions, and best practices.

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

## # 4. CLASSES AND STRUCTS

<br>

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

** .NET 10 note — prefer Source Generators over Reflection:**

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
| `public`             | … Yes                      |
| `protected`          | … Yes (most common)        |
| `internal`           | … Yes                      |
| `protected internal` | … Yes                      |
| `private`            |  No — compile error       |
| `private protected`  |  No — compile error       |

```cs
public abstract class Shape
{
    public abstract double Area();       // … public
    protected abstract string Describe(); // … protected
    // private abstract void Init();    //  compile error
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

**Rule of thumb:** If the base class can stand alone and all methods have reasonable defaults ’ concrete base class. If the base class is incomplete without subclasses ’ abstract class.

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
            => Console.WriteLine(_staticSecret); // … direct access to outer static

        public void ShowInstance(Outer outer)
            => Console.WriteLine(outer._instanceSecret); // … via outer reference
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
        new PublicNested().Hello();    // …
        new ProtectedNested().Hello(); // …
        new PrivateNested().Hello();   // …
    }
}

public class Derived : OuterClass
{
    public void Test()
    {
        new PublicNested().Hello();    // …
        new ProtectedNested().Hello(); // … — accessible via inheritance
        // new PrivateNested().Hello(); //  not accessible
    }
}

// From outside:
new OuterClass.PublicNested().Hello(); // …
// new OuterClass.ProtectedNested();   // 
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
        Console.WriteLine(Name);      // … public member
        Console.WriteLine(Protected); // … protected member
        // Console.WriteLine(_secret); //  compile error — private
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
        => Console.WriteLine(Species); // … accessible in derived class
}

// Outside the hierarchy:
var a = new Animal();
// Console.WriteLine(a.Species); //  compile error — not accessible here
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
string s    = "hello";     // string ’ object
int boxed   = 42;          // int ’ ValueType ’ object (when boxed)
object arr  = new int[5];  // Array ’ object

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
    // public override void Log(string msg) { } //  compile error — sealed
    public override void Error(string msg) => Console.WriteLine($"ALERT: {msg}"); // …
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
// public class BadShape { public abstract double Area(); } // 
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
    public void Show() => Console.WriteLine(Value); // … accessible
}

public class Unrelated
{
    public void Test(Base b)
    {
        // Console.WriteLine(b.Value); //  not accessible from unrelated class
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
// FORCED: contains abstract member ’ class must be abstract
public abstract class DataExporter //  required by compiler
{
    public abstract void Export(string data); //  forces class to be abstract
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

// Assembly A — unrelated class (same assembly ’ internal part grants access)
public class Unrelated
{
    public void Test(Base b) => Console.WriteLine(b.Data); // …
}

// Assembly B — derived class (protected part grants access)
public class Derived : Base
{
    public void Show() => Console.WriteLine(Data); // …
}

// Assembly B — unrelated class
public class External
{
    // Console.WriteLine(new Base().Data); //  not accessible
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
        Console.WriteLine(config.ConnectionString); // … same assembly
    }
}

// Assembly B — external project referencing MyApp.dll
// var cfg = new Configuration();
// Console.WriteLine(cfg.ConnectionString); //  not accessible externally
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
Animal animal = new Dog(); // … upcasting (implicit)
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

** Note for .NET 10 / Native AOT:** Reflection-based attribute reading works but is trimmed by the AOT compiler. Prefer source generators or `[DynamicallyAccessedMembers]` annotations when targeting Native AOT.

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

//  Cannot write: public static void NewMethod(this MathHelper m) { }
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

ILogger logger = new ConsoleLogger(); // … interface reference to concrete instance
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
IEnumerable<Animal> animals = dogs; // … Dog is more derived than Animal

// Func<T> is covariant in TResult
Func<Dog> getDog = () => new Dog();
Func<Animal> getAnimal = getDog; // … covariant
```

**Contravariance (`in`) — parameter type can be less derived:**

```cs
// Action<T> is contravariant (in T)
Action<Animal> processAnimal = a => Console.WriteLine("Processing animal");
Action<Dog> processDog = processAnimal; // … contravariant — can handle Dog via Animal handler

processDog(new Dog()); // Output: Processing animal
```

**Custom covariant interface:**

```cs
public interface IProducer<out T> { T Produce(); }

public class DogProducer : IProducer<Dog>
{
    public Dog Produce() => new Dog();
}

IProducer<Animal> producer = new DogProducer(); // … covariant
```

**Summary:**

| Keyword | Type parameter | Allowed direction | Example                       |
|---------|---------------|-------------------|-------------------------------|
| `out`   | Return type    | More derived ’ base | `IEnumerable<Dog>` ’ `IEnumerable<Animal>` |
| `in`    | Parameter type | Base ’ more derived | `Action<Animal>` ’ `Action<Dog>` |

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
    ~MyClass() { } //  Remove this
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
| Association   | Independent              | Teacher  Student    |
| Aggregation   | Part survives whole      | Department ’ Employee|
| Composition   | Part dies with whole     | Car ’ Engine         |

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

A **circular reference** occurs when two or more objects reference each other, forming a cycle (A ’ B ’ A, or A ’ B ’ C ’ A). In .NET, the GC handles circular references via tracing (mark-and-sweep), so they don\'t cause memory leaks in managed code by themselves.

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
child.Parent  = parent; // circle: parent ’ child ’ parent

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

// data still has a strong reference, so it\'s alive
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
        Console.WriteLine($"SMTP ’ {to}: {subject}");
        await Task.CompletedTask;
    }
}

public class SendGridEmailService : IEmailService
{
    public async Task SendAsync(string to, string subject, string body)
    {
        Console.WriteLine($"SendGrid ’ {to}: {subject}");
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
    int result = checked(max + 1); //  throws OverflowException
}
catch (OverflowException ex)
{
    Console.WriteLine($"Overflow caught: {ex.Message}");
}

// Checked block — applies to all arithmetic in the block
checked
{
    int a = int.MaxValue;
    int b = a + 1; //  throws OverflowException
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
        // Console.WriteLine(_secret);  //  compile error — not accessible
        // PrivateHelper();             //  compile error — not accessible

        Console.WriteLine(GetSecret()); // … access via public method
        CallHelper();                   // … access via protected method
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
| `public` | … | … |
| `protected` | … | … |
| `internal` | … | … (same assembly) |
| `private` | … |  |

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
Console.WriteLine($"{acc.Owner}: {acc.Balance:C}"); // Pradeep: 1,000.00

// var bad = new BankAccount("", -100); //  throws at construction
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
Console.WriteLine($"{p.Name}: {p.Price:C}"); // Laptop: 999.00
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
// Animal created: Rex       base runs first
// Dog created: Labrador     derived runs second
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

new Car("Tesla");         // Vehicle: Tesla ’ Car: 4 doors
new Car("BMW", 2);        // Vehicle: BMW   ’ Car: 2 doors
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

** NOT valid for overloading:**
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
        // Console.WriteLine(this._count); //  Compile error: CS0026
        // 'this' is not valid in a static member
        return new Counter();              // … create a new instance instead
    }

    public static int Compare(Counter a, Counter b) =>
        a._count.CompareTo(b._count); // … work with explicit instances
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
    //  Compile error CS0106: cannot change 'virtual' to 'static' in override
    // public static override void Show() => Console.WriteLine("Derived");

    // … Correct override — non-static like the base
    public override void Show() => Console.WriteLine("Derived");
}
```

**Summary of override rules:**

| Attempt | Allowed? |
|---------|----------|
| `virtual` ’ `override` (non-static) | … |
| `virtual` non-static ’ `static override` |  |
| `static` method ’ `static override` |  (static methods cannot be virtual) |
| `abstract` ’ `override` | … |

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
    // … Hides (does NOT override) the base virtual method
    public new static void Process() => Console.WriteLine("Derived static Process");

    // … Still override the virtual instance method separately if needed
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
// Static constructor ran    runs first
// Instance constructor ran  then instance ctor
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
    //  Compile error CS0515: access modifiers are not allowed on static constructors
    // public static MyClass() { }

    // … Correct — no access modifier
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
- Removes direct class-to-class dependencies (high-level ’ abstraction  low-level).
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
// DIP applied: depends on interface …
// BUT violates SRP: does ordering, emailing, AND logging in one class 
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
| Inheritance | Inherits from `ValueType` ’ `object` | Inherits from `object` |
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
object boxed = n;     // boxing: value type ’ heap allocation
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
// p.Price = 500m; //  compile error — init-only
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
    t.Celsius = -300; //  below absolute zero
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

// order.Total = 0; //  compile error — read-only, prevents accidental zeroing
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
        Console.WriteLine($"Push ’ {userId}: {message}");
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
//  Violation — direct public field
public class Circle { public double Radius; } // can be set to -1

// … Fix — property with validation
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
//  Violation — caller can mutate internal list
public class Roster
{
    private readonly List<string> _names = ["Alice", "Bob"];
    public List<string> Names => _names; // exposes the internal list
}

var r = new Roster();
r.Names.Clear(); // corrupts internal state!

// … Fix — return read-only view
public IReadOnlyList<string> Names => _names.AsReadOnly();
```

**3. Reflection — bypasses access modifiers (use only in exceptional scenarios):**

```cs
public class Secret { private int _code = 42; }

var s = new Secret();
var field = typeof(Secret).GetField("_code",
    System.Reflection.BindingFlags.NonPublic |
    System.Reflection.BindingFlags.Instance);

field!.SetValue(s, 999); //  bypasses encapsulation via reflection
```

**4. Overly broad access modifiers:**

```cs
//  Making implementation details public/internal unnecessarily
public class PaymentProcessor
{
    public string _internalToken = "abc123"; // should be private
}
```

**5. Mutable default property setters without validation:**

```cs
//  Auto-property with public setter — no opportunity to validate
public class Person
{
    public int Age { get; set; } // can be set to -1 or 999
}

// … Validate in setter or use init + constructor validation
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
Console.WriteLine(a1.Sound()); // Output: Woof   Dog\'s version (override wins)

// ---- Method hiding with new ----
Animal a2 = new Dog();
Console.WriteLine(a2.Name()); // Output: Animal  reference type decides (base wins!)

Dog d = new Dog();
Console.WriteLine(d.Name());  // Output: Dog     derived reference ’ Dog\'s version
```

**Why method hiding exists:**
- Versioning — a base class added a new method AFTER the derived class defined one with the same name; `new` prevents a compile warning while acknowledging the conflict.
- Intentional API divergence — rare, but sometimes a derived class needs a completely separate method with the same name.

**Warning:** Method hiding breaks the **Liskov Substitution Principle** — code that holds a `Dog` via an `Animal` reference will call the base `Name()` unexpectedly. Prefer `override` for polymorphic behavior.

```cs
// Practical demonstration of the problem
void PrintName(Animal a) => Console.WriteLine(a.Name());

PrintName(new Animal()); // Output: Animal
PrintName(new Dog());    // Output: Animal   NOT Dog! Hiding breaks polymorphism
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

## Q. What are fields in C# and how do they differ from properties?

A **field** is a variable declared directly inside a class or struct that holds data. A **property** is a member that wraps a field (or computes a value) with `get`/`set` accessors, enabling validation and encapsulation.

**Field types:**

| Modifier | Behaviour |
|----------|-----------|
| (none) | Instance field — one per object |
| `static` | Shared across all instances |
| `readonly` | Can only be assigned in declaration or constructor |
| `const` | Compile-time constant — implicitly `static` |

```cs
public class BankAccount
{
    //  Fields ——————————————————————————————————————————————————
    private decimal _balance;                      // instance field
    private static int _totalAccounts;             // static field
    private readonly string _accountNumber;        // readonly field
    private const decimal MinimumBalance = 0m;     // const field

    //  Auto-implemented property (compiler generates backing field)
    public string Owner { get; set; }

    //  Property with custom getter/setter using the private field
    public decimal Balance
    {
        get => _balance;
        private set
        {
            if (value < MinimumBalance)
                throw new ArgumentOutOfRangeException(nameof(value), "Balance cannot be negative.");
            _balance = value;
        }
    }

    //  Init-only property (C# 9) — settable only during object initialisation
    public DateTime OpenedOn { get; init; } = DateTime.UtcNow;

    public BankAccount(string owner, string accountNumber, decimal initialDeposit)
    {
        Owner = owner;
        _accountNumber = accountNumber;   // OK — inside constructor
        Balance = initialDeposit;
        _totalAccounts++;
    }

    public void Deposit(decimal amount) => Balance += amount;

    public static int TotalAccounts => _totalAccounts;
}

var account = new BankAccount("Alice", "ACC-001", 500m);
account.Deposit(200m);
Console.WriteLine(account.Balance);          // 700
Console.WriteLine(BankAccount.TotalAccounts); // 1

// account.OpenedOn = DateTime.UtcNow;       //  init-only — compile error outside init
var account2 = new BankAccount("Bob", "ACC-002", 100m) { OpenedOn = new DateTime(2024, 1, 1) };
Console.WriteLine(BankAccount.TotalAccounts); // 2
```

**Key differences — field vs property:**

| Aspect | Field | Property |
|--------|-------|----------|
| Access control | Single modifier | Independent `get`/`set` modifiers |
| Validation | Manual — direct assignment | Encapsulated in `set` accessor |
| Interface support | Cannot be declared in interface | Can be declared in interface |
| Data binding | Typically not bindable | Bindable (WPF, Blazor, etc.) |
| Reflection | `FieldInfo` | `PropertyInfo` |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are records in C# and how do they differ from classes?

A **record** (C# 9+) is a reference type that is designed for **immutable data models** with value-based equality. Records automatically generate `Equals`, `GetHashCode`, `ToString`, and a **positional deconstruct** from their primary constructor.

**Record variants:**

| Kind | Syntax | Type |
|------|--------|------|
| `record` (class) | `record Person(string Name, int Age)` | Reference type |
| `record struct` | `record struct Point(int X, int Y)` | Value type |
| `readonly record struct` | `readonly record struct Point(int X, int Y)` | Immutable value type |

```cs
//  1. Positional record with primary constructor ————————————————
record Person(string FirstName, string LastName, int Age);

var p1 = new Person("Alice", "Smith", 30);
var p2 = new Person("Alice", "Smith", 30);

Console.WriteLine(p1 == p2);          // true  — value equality
Console.WriteLine(p1.Equals(p2));     // true
Console.WriteLine(p1);                // Person { FirstName = Alice, LastName = Smith, Age = 30 }

//  2. Non-destructive mutation with `with` expression ———————————
var p3 = p1 with { Age = 31 };        // creates a new record; p1 is unchanged
Console.WriteLine(p3);               // Person { FirstName = Alice, LastName = Smith, Age = 31 }

//  3. Deconstruction ———————————————————————————————————————————
var (first, last, age) = p1;
Console.WriteLine($"{first} {last}, {age}");  // Alice Smith, 30

//  4. Inheritance ——————————————————————————————————————————————
record Employee(string FirstName, string LastName, int Age, string Department)
    : Person(FirstName, LastName, Age);

var emp = new Employee("Bob", "Jones", 25, "Engineering");
Console.WriteLine(emp);
// Employee { FirstName = Bob, LastName = Jones, Age = 25, Department = Engineering }

//  5. Custom members ———————————————————————————————————————————
record Product(string Name, decimal Price)
{
    // Computed property
    public string Label => $"{Name} (${Price:F2})";

    // Custom validation via init accessor
    public decimal Price { get; init; } =
        Price >= 0 ? Price : throw new ArgumentOutOfRangeException(nameof(Price));
}

var prod = new Product("Widget", 9.99m);
Console.WriteLine(prod.Label);  // Widget ($9.99)

//  6. record struct (C# 10) ————————————————————————————————————
record struct Coordinate(double Lat, double Lon);

var c1 = new Coordinate(51.5, -0.1);
var c2 = c1 with { Lon = -0.2 };
Console.WriteLine(c1 == c2);  // false
```

**Records vs classes:**

| Feature | `class` | `record` |
|---------|---------|---------|
| Equality | Reference (by default) | Value (auto-generated) |
| Immutability | Manual | `init`-only by default |
| `ToString()` | Type name | Property dump |
| `with` expression | No | Yes |
| Inheritance | Yes | Yes (record-to-record) |
| Deconstruction | Manual | Auto (positional) |
| Use case | Mutable entities, services | DTOs, value objects, event data |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 5. INHERITANCE AND OOP

<br>

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
| Class | Supported |  Not supported |
| Interface | … | … (multiple interfaces allowed) |
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

    // Chains all the way up: Vehicle ’ Car ’ ElectricCar
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
        Console.WriteLine($"[SMTP:{smtpServer}] Email ’ {Recipient}: {message}");
        return Task.CompletedTask;
    }
}

public class SmsNotification(string recipient, string phoneNumber)
    : Notification(recipient)
{
    public override Task SendAsync(string message)
    {
        Console.WriteLine($"[SMS:{phoneNumber}] ’ {Recipient}: {message}");
        return Task.CompletedTask;
    }
}

public class PushNotification(string recipient, string deviceToken)
    : Notification(recipient)
{
    public override Task SendAsync(string message)
    {
        Console.WriteLine($"[Push:{deviceToken}] ’ {Recipient}: {message}");
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
[SMTP:smtp.gmail.com] Email ’ alice@example.com: Your order has shipped!
Done.
Preparing notification for Bob...
[SMS:+1-555-1234] ’ Bob: Your order has shipped!
Done.
Preparing notification for Carol...
[Push:tok_abc123] ’ Carol: Your order has shipped!
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
| Has a body | … Yes — provides a default implementation |  No body (in non-default-interface scenarios) |
| Must be overridden |  Optional | … Mandatory in concrete derived classes |
| Class requirement | Can be in any non-sealed class | Class **must** be `abstract` |
| Can be instantiated directly | … (if class is concrete) |  Abstract class cannot be instantiated |
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
        : base(brand, year)             //  base constructor called first
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
        : base(brand, year, doors)      //  calls Car ’ Vehicle
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
// A    grandparent first
// B
// C    derived last
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

public class Dog(string name) : Animal(name)       // Dog IS-A Animal …
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
        Console.WriteLine($"[SMTP] ’ {to}: {body}");
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
// [SMTP] ’ pradeep@example.com: Your order for Laptop is confirmed!
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
    //  Compile error CS0621: 'Base.DoWork()' cannot be declared virtual
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
//     public override double Area() => 999; //  compile error — sealed
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
    //  CS0621 — cannot be both private and virtual
    // private virtual void Compute() { }

    // … To be overridable, minimum access is protected
    protected virtual void Compute() => Console.WriteLine("Base.Compute");

    // … Private method can be called via a protected/public virtual hook
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
| `private` |  No (CS0621) |
| `protected` | … Yes |
| `internal` | … Yes |
| `protected internal` | … Yes |
| `public` | … Yes |

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
| Polymorphism | … Yes — runtime type decides |  No — reference type decides |
| Dispatch | Dynamic (runtime) | Static (compile-time) |
| LSP compliance | … |  (often breaks it) |
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
Console.WriteLine(a.Sound());    // Output: Woof    Dog.Sound

// new (hiding): reference type (Animal) decides
Console.WriteLine(a.Category()); // Output: Animal  Animal.Category (NOT Dog!)

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
// Sound: Woof        override works correctly (Dog\'s Sound)
// Category: Animal   hiding is wrong here (expected "Dog", got "Animal")
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
    // AddAll calls base.AddAll which calls virtual Add ’ _count incremented TWICE per item!
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

public class Square : Rectangle //  Square IS-NOT substitutable for Rectangle
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
Animal ’ Vertebrate ’ Mammal ’ Carnivore ’ Feline ’ Cat
```

**5. Inheritance for code reuse only (not "is-a"):**

```cs
//  Stack should NOT inherit List just to reuse its storage
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

//  CS0509: 'MyConnection' cannot derive from sealed type 'DatabaseConnection'
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
    //  CS0239: cannot override inherited member 'Dog.Sound()' because it is sealed
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

## Q. What is explicit interface implementation in C# and when should it be used?

**Explicit interface implementation** allows a class to implement an interface member without exposing it as a regular public method. The member is only accessible through a reference of the interface type.

**When to use it:**
- Two interfaces declare the same method name with different intended semantics.
- You want to hide interface members from the class\'s public API.
- Implementing an interface purely as a contract without polluting IntelliSense.

```cs
interface IArea
{
    double Calculate();
}

interface IPerimeter
{
    double Calculate();   // same name, different meaning
}

public class Rectangle : IArea, IPerimeter
{
    public double Width { get; init; }
    public double Height { get; init; }

    //  Explicit implementation — only callable via the interface 
    double IArea.Calculate()      => Width * Height;
    double IPerimeter.Calculate() => 2 * (Width + Height);

    //  Optional convenience properties —————————————————————————
    public double Area      => ((IArea)this).Calculate();
    public double Perimeter => ((IPerimeter)this).Calculate();
}

var rect = new Rectangle { Width = 5, Height = 3 };

// Through the class directly — public Area/Perimeter properties
Console.WriteLine(rect.Area);       // 15
Console.WriteLine(rect.Perimeter);  // 16

// Through the interface references
IArea     ia = rect;
IPerimeter ip = rect;
Console.WriteLine(ia.Calculate());   // 15
Console.WriteLine(ip.Calculate());   // 16

// rect.Calculate()   compile error — not accessible directly
```

**Explicit vs implicit implementation:**

| Aspect | Implicit | Explicit |
|--------|----------|----------|
| Access modifier | `public` | None (interface access only) |
| Accessible via class reference | Yes | No — requires cast |
| Visible in IntelliSense | Yes | Only when typed as interface |
| Use case | Normal implementation | Disambiguation, hiding |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are default interface members in C# 8 and how are they used?

**Default interface members** (C# 8+) allow interfaces to provide a **default implementation** for a method. Implementing classes can optionally override them. This enables interface versioning without breaking existing implementations.

```cs
//  1. Basic default implementation —————————————————————————————
interface ILogger
{
    void Log(string message);

    // Default implementation — optional override in implementing class
    void LogError(string message) => Log($"[ERROR] {message}");
    void LogInfo(string message)  => Log($"[INFO]  {message}");
}

class ConsoleLogger : ILogger
{
    // Only required method implemented; defaults inherited
    public void Log(string message) => Console.WriteLine(message);
}

class FileLogger : ILogger
{
    public void Log(string message) => File.AppendAllText("app.log", message + "\n");

    // Override the default for a custom format
    public void LogError(string message) => Log($"CRITICAL >> {message}");
}

ILogger console = new ConsoleLogger();
console.LogInfo("Server started");   // [INFO]  Server started
console.LogError("Disk full");       // [ERROR] Disk full

ILogger file = new FileLogger();
file.LogError("DB timeout");         // CRITICAL >> DB timeout (overridden)

//  2. Interface evolution — adding a method without breaking callers
interface ICache
{
    object? Get(string key);
    void Set(string key, object value);

    // Added in v2 — existing implementors are not broken
    bool TryGet(string key, out object? value)
    {
        value = Get(key);
        return value is not null;
    }
}

//  3. Static abstract members (C# 11) — for generic math
interface IAddable<T> where T : IAddable<T>
{
    static abstract T operator +(T left, T right);
    static abstract T Zero { get; }
}

record struct Vector2D(double X, double Y) : IAddable<Vector2D>
{
    public static Vector2D operator +(Vector2D a, Vector2D b) => new(a.X + b.X, a.Y + b.Y);
    public static Vector2D Zero => new(0, 0);
}

T Sum<T>(IEnumerable<T> items) where T : IAddable<T>
    => items.Aggregate(T.Zero, (acc, x) => acc + x);

Console.WriteLine(Sum(new[] { new Vector2D(1, 2), new Vector2D(3, 4) })); // Vector2D { X = 4, Y = 6 }
```

**Key rules:**
- Default members are only accessible through the **interface reference**, not through the class directly (unless the class explicitly overrides them).
- A class is **not required** to override a default member.
- `static`, `private`, `protected`, and `virtual` modifiers are allowed in interfaces.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 6. COLLECTIONS AND GENERICS

<br>

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
// scores.Add(60); //  arrays have no Add — fixed size

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
| Type safety |  `object` — boxing/unboxing | … Strongly typed |
| Performance | Slower (boxing overhead) | Faster (no boxing for value types) |
| Null keys |  Not allowed |  Not allowed (same) |
| Thread safety | Thread-safe for reads only | Use `ConcurrentDictionary` for writes |
| Recommended | Legacy code only | … Always prefer |

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
| Blocking |  Non-blocking (`TryDequeue` returns false) | … Blocks consumer until item available |
| Bounded capacity |  Unlimited | … Optional upper bound |
| Completion signal |  No | … `CompleteAdding()` signals end-of-stream |
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
| Type safety |  Non-generic (`object`) | … Generic — strongly typed |
| Performance | Slower (boxing for value types) | Faster (no boxing) |
| Null key |  Not allowed |  Not allowed |
| Null value | … Allowed | … Allowed |
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
| Lookup by index | … `Keys[i]`, `Values[i]` |  Not supported |
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
- Many reads, few insertions, need index access ’ `SortedList<K,V>`
- Frequent insertions/deletions ’ `SortedDictionary<K,V>`

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between Array and ArrayList?

| Feature | `Array` (`T[]`) | `ArrayList` |
|---------|----------------|-------------|
| Type safety | … Strongly typed |  Stores `object` — no type safety |
| Size | Fixed | Dynamic |
| Performance | Fast — no boxing for value types | Slower — boxing/unboxing for value types |
| Namespace | Built-in | `System.Collections` |
| Generics | N/A | Non-generic — superseded by `List<T>` |
| LINQ support | … | … (via cast to `IEnumerable`) |

```cs
// Array — fixed size, typed
int[] arr = new int[3] { 1, 2, 3 };
arr[0] = 10;
Console.WriteLine(arr.Length); // 3
// arr[3] = 4; //  IndexOutOfRangeException

// ArrayList — dynamic, but loses type safety
var al = new System.Collections.ArrayList();
al.Add(1);        // boxing int ’ object
al.Add("hello");  // mixes types — no compile error!
al.Add(3.14);

foreach (object item in al)
    Console.WriteLine(item); // 1 / hello / 3.14

//  Runtime error possible:
// int x = (int)al[1]; // InvalidCastException — "hello" is not int

// … Modern replacement — List<T>
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
// Typical: Array  List<int> >> ArrayList
```

**Performance ranking (value types):** `T[]`  `List<T>` >> `ArrayList`

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
// keys:   ["apple", "banana", "cherry"]    sorted array
// values: [1,       2,        3]            parallel array

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
| `Count` / `Length` |  Not available (enumerate to count) | … O(1) `.Count` |
| Index access |  | … `list[i]` |
| Mutation |  | … `Add`, `Remove`, `Sort` |
| Re-enumeration | May re-execute the query | … Safe — always same data |
| Best for | Method parameters (widest compatibility) | In-memory data management |

```cs
// IEnumerable<T> — lazy, deferred
IEnumerable<int> LazySquares(int n)
{
    for (int i = 1; i <= n; i++)
    {
        Console.WriteLine($"Computing {i}");
        yield return i * i;
    }
}

var seq = LazySquares(5); // nothing computed yet
var first = seq.First();   // computes 1 only, stops
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
//  Old approach — ArrayList loses type safety
var al = new System.Collections.ArrayList();
al.Add(42);       // boxing
al.Add("hello");  // mixed types — no compile error
int x = (int)al[0]; // unboxing — runtime error if wrong type

// … Modern replacement
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

// Mixing: IQueryable ’ AsEnumerable() forces in-memory from that point
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
fruits.Add("apple");    //  duplicate ignored — returns false
fruits.Add("date");     // … added

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
—————————————————           ————————————————————————
 x = 42                      List<int> object        
 y = 3.14                     [_items array, Count]  
 list ————————————————————–                         
 point = (1,2)               object {}               
 obj ——————————————————————–                         
—————————————————           ————————————————————————
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

**Summary:** String variable ’ stack (or field in object); String content ’ always heap.

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
int x = 10; // local int (struct) captured by lambda ’ promoted to heap
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
 —————————————————————————         —————————————————————————
  Main() frame                      ————————————————————— 
    args reference —————————————–   string[] args        
    person ref —————————————————–  ————————————————————— 
 —————————————————————————          ————————————————————— 
  CreatePerson() frame               Person object        
    name = "Pradeep"                  Name: "Pradeep"     
    age  = 30                         Age:  30            
    p ref ——————————————————————–  ————————————————————— 
 —————————————————————————         —————————————————————————
```

```cs
class Person { public string Name { get; } = ""; public int Age; }

void Demo()
{
    int age = 30;                  // value type ’ stack
    string name = "Pradeep";      // reference ’ stack, content ’ heap
    Person p = new Person();       // reference ’ stack, object ’ heap

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
   OS kernel code + data
   Thread 1 stack (virtual address range, ~1 MB reserved)
   Thread 2 stack (separate range)
   Managed heap segments (GC-managed, can grow)
        Gen 0 segment
        Gen 1 segment
        Gen 2 segment
        LOH segment
   Native heaps (unmanaged, DLL code, etc.)
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
    //  STACK ————————————————————————————————————————
    int age     = 30;          // int ’ stack
    bool active = true;        // bool ’ stack
    Point pt    = new(3, 4);   // struct ’ stack (inline)
    Circle c    = new() { R = 5 }; // c (reference) ’ stack; object ’ heap

    //  HEAP —————————————————————————————————————————
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
  —————————————————————————  Stack base (thread start)
   Main() frame            
     [return address]      
     [saved registers]     
     args = ...             RSP points here when in Main()
  —————————————————————————
   Method1() frame           RSP decremented when Method1() called
     [return address]      
     local int a = 10      
     local double b = 3.14 
  —————————————————————————
   Method2() frame           RSP decremented again
     [return address]      
     local int x = 5         RSP points here (top of stack)
  —————————————————————————
Low addresses (stack grows downward “)
```

```cs
// You can observe stack behavior via StackTrace
void InnerMethod()
{
    var trace = new System.Diagnostics.StackTrace(fNeedFileInfo: true);
    Console.WriteLine(trace);
    // Output shows call stack: InnerMethod ’ OuterMethod ’ Main
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
// C: entered    C allocated last
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
| Type safety |  Stores `object` — runtime cast errors | … Compile-time type checking |
| Performance (value types) |  Boxing/unboxing on every operation | … No boxing — direct storage |
| IntelliSense / tooling |  All methods return `object` | … Full typed IntelliSense |
| LINQ | Requires cast | … Native LINQ support |
| Recommended | Legacy code only | … Always |

```cs
//  ArrayList — avoid in new code
var al = new System.Collections.ArrayList();
al.Add(42);       // boxing int ’ object
al.Add("mixed");  // no compile error — mixed types allowed
int n = (int)al[0]; // unboxing, runtime error if wrong type

// … List<T> — always prefer
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
| `GetEnumerator()` | … | … (inherited) |
| `Count` |  | … |
| `IsReadOnly` |  | … |
| `Add(T)` |  | … |
| `Remove(T)` |  | … |
| `Contains(T)` |  | … |
| `Clear()` |  | … |
| `CopyTo(T[], int)` |  | … |

```cs
// IEnumerable<T> — iterate only
IEnumerable<int> seq = [1, 2, 3, 4, 5];
foreach (var n in seq) Console.Write($"{n} ");
// seq.Count(); // works via LINQ extension, but O(n)
// seq.Add(6);  //  no Add on IEnumerable

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

Console.WriteLine(queue.Dequeue()); // First   oldest item out
Console.WriteLine(queue.Peek());    // Second  next without removing
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

Console.WriteLine(stack.Pop());  // Third   most recent item out
Console.WriteLine(stack.Peek()); // Second  next without removing
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
| Index access | … O(1) | … O(1) |
| Add / Remove |  Not supported | … `Add`, `Remove`, `Insert` |
| `Length` / `Count` | `Length` | `Count` |
| Memory | Slightly more efficient (no metadata) | Small overhead for capacity tracking |
| Multi-dimensional | … (`int[,]`, `int[][]`) |  (use `List<List<T>>` instead) |
| LINQ | … | … |
| Span/Memory | … `AsSpan()` | … `CollectionsMarshal.AsSpan()` |
| Interop (P/Invoke, etc.) | Preferred |  Usually requires `.ToArray()` |

```cs
// Array — fixed size
int[] arr = new int[5];
arr[0] = 10;
// arr[5] = 60; //  IndexOutOfRangeException

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
| Multi-dim | … (`int[,]`) |  (nest collections) |
| Nullability | Can hold nulls | Depends on type |

```cs
// Array — tight, fixed, minimal API
int[] arr = [10, 20, 30];
Console.WriteLine(arr.Length); // 3
// arr[3] = 40; //  cannot resize

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
// numbers.Add("text"); //  compile error — type-safe

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
//  No capacity — triggers multiple re-allocations as list grows
var list = new List<int>();
for (int i = 0; i < 1_000_000; i++) list.Add(i);

// … Pre-allocate — single backing array allocation
var list2 = new List<int>(capacity: 1_000_000);
for (int i = 0; i < 1_000_000; i++) list2.Add(i);
```

**2. Use `Span<T>` and `Memory<T>` for zero-allocation slicing:**

```cs
int[] arr = Enumerable.Range(0, 1_000_000).ToArray();

//  Creates a new array copy
int[] slice = arr[100..200];

// … Zero-copy view
ReadOnlySpan<int> span = arr.AsSpan(100, 100);
int sum = 0;
foreach (ref readonly int n in span) sum += n;
```

**3. Use `ArrayPool<T>` for temporary buffers:**

```cs
using System.Buffers;

//  Allocates a new array each time (GC pressure)
void ProcessData(byte[] data) { /* ... */ }

// … Rent from pool — no GC allocation
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
//  Loads all 10M records into memory
List<int> LoadAll() => Enumerable.Range(0, 10_000_000).ToList();

// … Streams one at a time — O(1) memory
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
//  Materialises to List unnecessarily
var list = items.Where(x => x > 0).ToList().Count;

// … Count without materialising
var count = items.Count(x => x > 0); // single pass, no intermediate list
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use `List<T>` in C# with common operations?

`List<T>` is the most commonly used generic collection in .NET. It is a resizable array that provides O(1) indexed access, O(1) amortised appends, and O(n) inserts/removes.

```cs
//  1. Create and initialise ————————————————————————————————————
var fruits = new List<string> { "Apple", "Banana", "Cherry" };

//  2. Add / Insert —————————————————————————————————————————————
fruits.Add("Date");                   // append
fruits.Insert(1, "Avocado");          // insert at index 1
fruits.AddRange(["Elderberry", "Fig"]); // add multiple

//  3. Access ———————————————————————————————————————————————————
Console.WriteLine(fruits[0]);        // Apple
Console.WriteLine(fruits.Count);     // 6

//  4. Search ———————————————————————————————————————————————————
Console.WriteLine(fruits.Contains("Banana"));       // True
Console.WriteLine(fruits.IndexOf("Cherry"));        // 3 (after insert)
Console.WriteLine(fruits.Find(f => f.StartsWith("A"))); // Apple

//  5. Remove ———————————————————————————————————————————————————
fruits.Remove("Banana");              // by value
fruits.RemoveAt(0);                   // by index
fruits.RemoveAll(f => f.Length > 5);  // by predicate

//  6. Sort and reverse —————————————————————————————————————————
fruits.Sort();
fruits.Reverse();

//  7. Convert ——————————————————————————————————————————————————
string[] array = fruits.ToArray();
List<string> copy  = fruits.ToList();           // shallow copy

//  8. Iterate ——————————————————————————————————————————————————
foreach (var f in fruits)
    Console.WriteLine(f);

//  9. LINQ integration —————————————————————————————————————————
var longNames = fruits.Where(f => f.Length > 4).OrderBy(f => f).ToList();

//  10. Capacity vs Count ———————————————————————————————————————
var numbers = new List<int>(capacity: 100);  // reserve space upfront
Console.WriteLine(numbers.Capacity);  // 100
Console.WriteLine(numbers.Count);     // 0  — no elements yet
```

**Time complexity summary:**

| Operation | Complexity |
|-----------|-----------|
| `Add` (end) | O(1) amortised |
| `Insert` (middle) | O(n) |
| `Remove` by value | O(n) |
| `RemoveAt` (end) | O(1) |
| Index access `[i]` | O(1) |
| `Contains` | O(n) |
| `BinarySearch` (sorted) | O(log n) |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use `Dictionary<TKey, TValue>` in C# with CRUD operations?

`Dictionary<TKey, TValue>` stores key–value pairs in a hash table, providing O(1) average-case lookups, inserts, and deletes.

```cs
//  1. Create ———————————————————————————————————————————————————
var scores = new Dictionary<string, int>
{
    ["Alice"] = 95,
    ["Bob"]   = 82,
    ["Carol"] = 78,
};

//  2. Add / Update —————————————————————————————————————————————
scores.Add("Dave", 91);              // throws if key exists
scores["Eve"] = 88;                  // add or overwrite
scores.TryAdd("Alice", 0);           // no-op if key exists — returns false

scores["Bob"] = 85;                  // update existing value

//  3. Read —————————————————————————————————————————————————————
Console.WriteLine(scores["Alice"]);  // 95 — throws KeyNotFoundException if missing

if (scores.TryGetValue("Frank", out int frankScore))
    Console.WriteLine(frankScore);
else
    Console.WriteLine("Frank not found");

// Safe default without exception
int val = scores.GetValueOrDefault("Ghost", 0);

//  4. Delete ———————————————————————————————————————————————————
scores.Remove("Carol");              // returns true if removed
scores.Remove("Carol");              // returns false — already gone, no exception

//  5. Check existence ——————————————————————————————————————————
Console.WriteLine(scores.ContainsKey("Alice"));   // True
Console.WriteLine(scores.ContainsValue(91));      // True

//  6. Iterate ——————————————————————————————————————————————————
foreach (var (name, score) in scores)             // KeyValuePair deconstruction
    Console.WriteLine($"{name}: {score}");

foreach (string key in scores.Keys)   Console.WriteLine(key);
foreach (int    v   in scores.Values) Console.WriteLine(v);

//  7. Merge / upsert pattern ———————————————————————————————————
var extras = new Dictionary<string, int> { ["Alice"] = 5, ["Zoe"] = 70 };
foreach (var (k, v) in extras)
    scores[k] = scores.GetValueOrDefault(k) + v;  // adds or accumulates

//  8. Group by with Dictionary —————————————————————————————————
string[] words = ["apple", "ant", "banana", "berry", "cherry"];
var byFirstLetter = words.GroupBy(w => w[0])
                         .ToDictionary(g => g.Key, g => g.ToList());
Console.WriteLine(string.Join(", ", byFirstLetter['a']));  // apple, ant

//  9. Concurrent access ————————————————————————————————————————
// For thread-safe scenarios use ConcurrentDictionary<K,V>
var concurrentScores = new System.Collections.Concurrent.ConcurrentDictionary<string, int>();
concurrentScores.AddOrUpdate("Alice", 95, (_, old) => old + 5);
```

**Dictionary vs Hashtable:**

| Feature | `Dictionary<K,V>` | `Hashtable` |
|---------|-------------------|-------------|
| Type safety | Generic — strongly typed | `object` — requires boxing |
| Null key |  Not allowed | … Allowed |
| Thread safety | Not thread-safe | Partially thread-safe (reads) |
| Performance | Faster (no boxing) | Slower |
| Preferred | … Modern code | Legacy code only |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use `Stack<T>` and `Queue<T>` in C#?

**`Stack<T>`** is a LIFO (Last-In, First-Out) collection. **`Queue<T>`** is a FIFO (First-In, First-Out) collection. Both provide O(1) push/pop and enqueue/dequeue operations.

```cs
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// Stack<T>  — LIFO
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
var stack = new Stack<string>();

// Push — add to top
stack.Push("First");
stack.Push("Second");
stack.Push("Third");
Console.WriteLine(stack.Count);   // 3

// Peek — view top without removing
Console.WriteLine(stack.Peek());  // Third

// Pop — remove from top
Console.WriteLine(stack.Pop());   // Third
Console.WriteLine(stack.Pop());   // Second
Console.WriteLine(stack.Count);   // 1

// TryPop / TryPeek (safe, no exception on empty)
if (stack.TryPop(out string? item))
    Console.WriteLine($"Popped: {item}");   // Popped: First

// Real-world: undo/redo, expression evaluation, DFS traversal
var history = new Stack<string>();
history.Push("Page A");
history.Push("Page B");
history.Push("Page C");
string last = history.Pop();   // Go back to Page B
Console.WriteLine(last);       // Page C

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// Queue<T>  — FIFO
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
var queue = new Queue<string>();

// Enqueue — add to back
queue.Enqueue("Task 1");
queue.Enqueue("Task 2");
queue.Enqueue("Task 3");
Console.WriteLine(queue.Count);     // 3

// Peek — view front without removing
Console.WriteLine(queue.Peek());    // Task 1

// Dequeue — remove from front
Console.WriteLine(queue.Dequeue()); // Task 1
Console.WriteLine(queue.Dequeue()); // Task 2

// TryDequeue / TryPeek (safe variants)
if (queue.TryDequeue(out string? next))
    Console.WriteLine($"Processing: {next}");   // Processing: Task 3

// Real-world: request queuing, BFS, print spooler
var printQueue = new Queue<string>();
printQueue.Enqueue("Document1.pdf");
printQueue.Enqueue("Document2.docx");

while (printQueue.TryDequeue(out string? doc))
    Console.WriteLine($"Printing: {doc}");

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// PriorityQueue<TElement, TPriority>  — C# 10+
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
var pq = new PriorityQueue<string, int>();
pq.Enqueue("Low priority task",    10);
pq.Enqueue("High priority task",   1);
pq.Enqueue("Medium priority task", 5);

while (pq.TryDequeue(out string? task, out int priority))
    Console.WriteLine($"[{priority}] {task}");
// [1] High priority task
// [5] Medium priority task
// [10] Low priority task
```

**Stack vs Queue comparison:**

| Feature | `Stack<T>` (LIFO) | `Queue<T>` (FIFO) |
|---------|------------------|------------------|
| Add | `Push(item)` | `Enqueue(item)` |
| Remove | `Pop()` | `Dequeue()` |
| Peek | `Peek()` | `Peek()` |
| Safe remove | `TryPop(out T)` | `TryDequeue(out T)` |
| Use case | Undo, DFS, parsing | Task queues, BFS, printing |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 7. FILE HANDLING

<br>

## Q. What is File Handling in C#.Net?

**File handling** is the ability to create, read, write, append, copy, move, and delete files and directories from C# code. The primary namespaces are:

| Namespace | Contents |
|-----------|---------|
| `System.IO` | `File`, `FileInfo`, `Directory`, `DirectoryInfo`, `Stream`, `StreamReader`, `StreamWriter`, `FileStream`, `MemoryStream`, `BinaryReader`, `BinaryWriter` |
| `System.Text.Json` | `JsonSerializer`, `Utf8JsonReader`, `Utf8JsonWriter` |
| `System.Xml` | `XmlReader`, `XmlWriter`, `XDocument` |

**Example:**

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

**Example:**

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

**Example:**

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
// ReadAllText  ’ parse JSON/XML/config as a whole string
// ReadAllLines ’ process CSV/log line by line but file fits in memory
// ReadLines    ’ large files that don\'t fit in memory (streaming)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you append text to an existing file in C#?

**Example:**

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

**Example:**

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

// 4.  TOCTOU race condition — check-then-use is not atomic
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

**Example:**

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

**Example:**

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

**Example:**

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
| **Async** | … `useAsync: true` | (but completes synchronously) |
| **Use case** | Read/write actual files | Temporary buffers, unit testing, serialisation |
| **Seek** | … (seekable) | … (seekable) |

**Example:**

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

**Example:**

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

**Example:**

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

**Example:**

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

**Example:**

```cs
//  Writing binary data ————————————————————————————————————————————

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

//  Reading binary data ————————————————————————————————————————————

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

**Example:**

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

**Example:**

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

**Example:**

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
| **Random access** |  — forward only |  — forward only |
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
| **Private members** |  Not serialised | … With `[DataMember]` |
| **Inheritance** | Uses `[XmlInclude]` | Uses `[KnownType]` |
| **XML output** | More customisable (element names, attributes) | Less customisable, more strict |
| **Performance** | Slower (reflection-based) | Faster (generated code) |
| **Null handling** | Omits null elements by default | Serialises null with `xsi:nil="true"` |
| **Interfaces** |  Cannot serialise |  Cannot serialise |
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

// … Modern recommendation: use System.Text.Json for new code
var json = System.Text.Json.JsonSerializer.Serialize(new { Id = 1, Name = "Laptop" });
Console.WriteLine(json); // {"Id":1,"Name":"Laptop"}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a `BinaryFormatter` in C#?

>  **`BinaryFormatter` is obsolete and disabled by default since .NET 5, removed in .NET 9.** It had critical security vulnerabilities (arbitrary code execution via deserialization gadget chains). Do not use it in new or existing code.

```cs
//  BinaryFormatter — OBSOLETE, INSECURE, REMOVED in .NET 9
// DO NOT USE:
// var formatter = new System.Runtime.Serialization.Formatters.Binary.BinaryFormatter();
// formatter.Serialize(stream, obj);   // throws NotSupportedException in .NET 9

// … Modern replacements:

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

>  **`SoapFormatter` is obsolete and removed in .NET Core / .NET 5+.** It was a WCF/SOAP-era serialiser that formatted object graphs as SOAP XML. Like `BinaryFormatter`, it had security vulnerabilities.

```cs
//  SoapFormatter — .NET Framework only, OBSOLETE
// System.Runtime.Serialization.Formatters.Soap.SoapFormatter
// Not available in .NET 5+ / .NET Core at all

// … Modern alternatives for SOAP/XML scenarios:

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
| **Security** |  Critical vulnerabilities |  Critical vulnerabilities |
| **Status in .NET 9+** | Removed | Removed (never in .NET Core) |
| **Replacement** | `System.Text.Json`, `BinaryWriter`, MessagePack | `XmlSerializer`, `DataContractSerializer`, gRPC |

```cs
//  Both are obsolete — DO NOT USE in new code.

// … Choose the right modern serialiser based on needs:

// Scenario ’ Recommended serialiser
// REST API payloads         ’ System.Text.Json
// Configuration files       ’ System.Text.Json / YAML
// Compact binary IPC        ’ MessagePack / MemoryPack
// Custom binary protocol    ’ BinaryWriter + BinaryReader
// XML interop / SOAP legacy ’ XmlSerializer / DataContractSerializer
// Service-to-service RPC    ’ gRPC (Protobuf)

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
//  JSON Serialization (recommended in .NET 10) ——————————————————
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

//  XML Serialization —————————————————————————————————————————————
[Serializable]
public class ProductXml { public int Id; public string Name = ""; }

var xmlSer = new System.Xml.Serialization.XmlSerializer(typeof(ProductXml));
await using var sw = new StringWriter();
xmlSer.Serialize(sw, new ProductXml { Id = 1, Name = "Laptop" });
Console.WriteLine(sw.ToString());

//  Custom binary (no third-party, no security risk) ——————————————
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

## Q. How do you use the `Path` class in C# to work with file paths?

The static `System.IO.Path` class provides platform-independent methods for manipulating file and directory path strings without performing any I/O.

```cs
using System.IO;

//  1. Combine path segments (handles separators automatically) —
string full = Path.Combine("C:\\Users", "Alice", "Documents", "report.pdf");
Console.WriteLine(full);
// Windows: C:\Users\Alice\Documents\report.pdf

//  2. Get parts of a path ——————————————————————————————————————
string path = @"C:\Projects\MyApp\src\Program.cs";

Console.WriteLine(Path.GetFileName(path));           // Program.cs
Console.WriteLine(Path.GetFileNameWithoutExtension(path)); // Program
Console.WriteLine(Path.GetExtension(path));          // .cs
Console.WriteLine(Path.GetDirectoryName(path));      // C:\Projects\MyApp\src
Console.WriteLine(Path.GetPathRoot(path));           // C:\

//  3. Change or check extension ————————————————————————————————
string newPath = Path.ChangeExtension(path, ".bak");
Console.WriteLine(newPath);   // C:\Projects\MyApp\src\Program.bak

Console.WriteLine(Path.HasExtension("readme.txt"));  // True
Console.WriteLine(Path.HasExtension("Makefile"));    // False

//  4. Rooted / absolute paths ——————————————————————————————————
Console.WriteLine(Path.IsPathRooted(@"C:\temp"));    // True
Console.WriteLine(Path.IsPathRooted(@"relative\path")); // False

string absolute = Path.GetFullPath(@".\logs\app.log");
Console.WriteLine(absolute);  // expands relative to current directory

//  5. Temporary files and random names —————————————————————————
string tempFile = Path.GetTempFileName();   // creates empty file in %TEMP%
string tempDir  = Path.GetTempPath();       // e.g. C:\Users\Alice\AppData\Local\Temp\
string random   = Path.GetRandomFileName(); // e.g. 3j4knw32.tmp (no file created)

//  6. Path separator constants —————————————————————————————————
Console.WriteLine(Path.DirectorySeparatorChar); // \ on Windows, / on Linux
Console.WriteLine(Path.PathSeparator);          // ; on Windows, : on Linux
Console.WriteLine(Path.AltDirectorySeparatorChar); // /

//  7. Relative paths (net5.0+) —————————————————————————————————
string relative = Path.GetRelativePath(@"C:\Projects\MyApp", @"C:\Projects\MyApp\src\Program.cs");
Console.WriteLine(relative);  // src\Program.cs

//  8. Safe file naming —————————————————————————————————————————
char[] invalid = Path.GetInvalidFileNameChars();
string safeName = string.Concat("my:file*name".Select(c => invalid.Contains(c) ? '_' : c));
Console.WriteLine(safeName);  // my_file_name

// Always clean up temp files
File.Delete(tempFile);
```

**Key `Path` methods at a glance:**

| Method | Returns |
|--------|---------|
| `Combine()` | Joined path string |
| `GetFileName(path)` | `"Program.cs"` |
| `GetFileNameWithoutExtension(path)` | `"Program"` |
| `GetExtension(path)` | `".cs"` |
| `GetDirectoryName(path)` | Parent folder string |
| `GetFullPath(path)` | Absolute path |
| `GetRelativePath(base, path)` | Relative string |
| `GetTempPath()` | System temp directory |
| `GetTempFileName()` | Creates and returns a temp file path |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between the `File` and `FileInfo` classes in C#?

Both classes operate on files, but `File` provides **static methods** for one-off operations while `FileInfo` is an **instance class** that caches file metadata and is more efficient for multiple operations on the same file.

```cs
using System.IO;

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// File (static) — convenient for single operations
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
string path = @"C:\Temp\example.txt";

// Create / write
File.WriteAllText(path, "Hello, World!");
File.AppendAllText(path, "\nAppended line.");
File.WriteAllLines(path, ["Line 1", "Line 2", "Line 3"]);

// Read
string content  = File.ReadAllText(path);
string[] lines  = File.ReadAllLines(path);
byte[] bytes    = File.ReadAllBytes(path);

// Check and manage
Console.WriteLine(File.Exists(path));            // True
File.Copy(path, @"C:\Temp\backup.txt", overwrite: true);
File.Move(path, @"C:\Temp\moved.txt");
File.Delete(@"C:\Temp\moved.txt");

// Get basic attributes
DateTime created  = File.GetCreationTime(path);
DateTime modified = File.GetLastWriteTime(path);
FileAttributes attrs = File.GetAttributes(path);

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// FileInfo (instance) — better for multiple operations on one file
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
var fi = new FileInfo(@"C:\Temp\backup.txt");

// Cached metadata — no extra syscall for each property
Console.WriteLine(fi.Name);              // backup.txt
Console.WriteLine(fi.Extension);        // .txt
Console.WriteLine(fi.Length);           // file size in bytes
Console.WriteLine(fi.DirectoryName);    // C:\Temp
Console.WriteLine(fi.FullName);         // C:\Temp\backup.txt
Console.WriteLine(fi.CreationTime);
Console.WriteLine(fi.IsReadOnly);

// Operations via instance
fi.CopyTo(@"C:\Temp\copy.txt", overwrite: true);
fi.MoveTo(@"C:\Temp\renamed.txt");

using StreamReader sr = fi.OpenText();
Console.WriteLine(sr.ReadToEnd());

// Refresh after external changes
fi.Refresh();
Console.WriteLine(fi.Length);           // up-to-date size
```

**File vs FileInfo:**

| Aspect | `File` (static) | `FileInfo` (instance) |
|--------|-----------------|----------------------|
| Type | Static utility class | Instance class |
| Security checks | Per call | Once at construction |
| Multiple ops on same file | Repeated security checks | More efficient |
| Metadata caching | No | Yes (call `Refresh()` to update) |
| Best for | One-off operations | Multiple ops on the same file |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you perform asynchronous file I/O in C#?

Async file I/O prevents blocking the calling thread during disk operations, which is critical in web servers and UI applications.

```cs
using System.IO;
using System.Text;

//  1. Read all text asynchronously ————————————————————————————
string path = @"C:\Temp\data.txt";
string content = await File.ReadAllTextAsync(path);
Console.WriteLine(content);

//  2. Write all text asynchronously ———————————————————————————
await File.WriteAllTextAsync(path, "Async content written.");

//  3. Read / write lines asynchronously ————————————————————————
await File.WriteAllLinesAsync(path, ["Line A", "Line B", "Line C"]);
string[] lines = await File.ReadAllLinesAsync(path);

//  4. StreamReader / StreamWriter (streaming large files) ——————
// Read large file line by line without loading all into memory
await using var reader = new StreamReader(path, Encoding.UTF8);
string? line;
while ((line = await reader.ReadLineAsync()) is not null)
    Console.WriteLine(line);

// Write using StreamWriter
await using var writer = new StreamWriter(path, append: false, Encoding.UTF8);
await writer.WriteLineAsync("Header");
for (int i = 1; i <= 5; i++)
    await writer.WriteLineAsync($"Row {i}");
// FlushAsync called automatically on DisposeAsync

//  5. FileStream with explicit async I/O ———————————————————————
await using var fs = new FileStream(
    path,
    FileMode.Create,
    FileAccess.Write,
    FileShare.None,
    bufferSize: 4096,
    useAsync: true);          //  enables true OS-level async

byte[] data = Encoding.UTF8.GetBytes("Binary async write");
await fs.WriteAsync(data);

//  6. CancellationToken support ————————————————————————————————
using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(5));
try
{
    string text = await File.ReadAllTextAsync(path, cts.Token);
    Console.WriteLine(text);
}
catch (OperationCanceledException)
{
    Console.WriteLine("File read cancelled.");
}

//  7. Process multiple files concurrently ——————————————————————
string[] files = Directory.GetFiles(@"C:\Temp\Logs", "*.log");

IEnumerable<Task<string>> readTasks = files.Select(f => File.ReadAllTextAsync(f));
string[] contents = await Task.WhenAll(readTasks);
Console.WriteLine($"Read {contents.Length} log files.");
```

**Sync vs async file I/O:**

| Aspect | Synchronous | Asynchronous |
|--------|-------------|--------------|
| Thread blocking | Blocks caller | Frees caller thread |
| Throughput | Lower (1 thread per op) | Higher (thread pool) |
| Code complexity | Simple | Requires `async`/`await` |
| Use in ASP.NET Core |  Avoid | Always prefer |
| Use in Console/scripts | Acceptable | Optional |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 8. REGULAR EXPRESSION

<br>

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
// IsMatch(input)       ’ bool — does pattern occur?
// Match(input)         ’ Match — first occurrence
// Matches(input)       ’ MatchCollection — all occurrences
// Replace(input, repl) ’ string — replace matches
// Split(input)         ’ string[] — split on pattern

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
// Reorder "Last, First" ’ "First Last"
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
    "user@example.com",        // …
    "user.name+tag@domain.co", // …
    "user@sub.domain.org",     // …
    "bad@.com",                // 
    "@nodomain",               // 
    "noDomainExtension@abc",   // 
    "spaces in@email.com",     // 
];

foreach (string email in emails)
    Console.WriteLine($"{email,-35} ’ {(EmailValidator.IsValid(email) ? "…" : "")}");

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
// Just validating?        ’ IsMatch
// Need value/groups?      ’ Match
// Need all occurrences?   ’ Matches
// Need replace/transform? ’ Replace + MatchEvaluator
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

// 2. Named capture groups — (?<name>...)   recommended
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
| `new Regex(pattern)` | Interpreted (default) | Baseline | … |
| `new Regex(pattern, RegexOptions.Compiled)` | JIT-compiled to IL | ~2–3— faster | … |
| `[GeneratedRegex]` source generator (.NET 7+) | Compiled at build time | Fastest, no startup cost | … |

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
// Interpreted (~1—) ’ Compiled (~3—) ’ GeneratedRegex (~5—+)
// Use GeneratedRegex for new code targeting .NET 7+
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are regex quantifiers and character classes in C#?

**Quantifiers** control how many times a pattern element must match. **Character classes** define sets of characters to match at a position.

```cs
using System.Text.RegularExpressions;

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// Character Classes
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// \d  — digit [0-9]
// \D  — non-digit
// \w  — word char [a-zA-Z0-9_]
// \W  — non-word char
// \s  — whitespace (\t, \n, \r, space)
// \S  — non-whitespace
// .   — any char except \n (use RegexOptions.Singleline to include \n)
// [abc]     — any of a, b, c
// [^abc]    — any EXCEPT a, b, c
// [a-z]     — range a to z
// [A-Za-z0-9] — letters and digits

var digits  = Regex.Matches("abc123def456", @"\d+");
foreach (Match m in digits) Console.WriteLine(m.Value);  // 123   456

var words   = Regex.Matches("Hello, World! 42", @"\w+");
foreach (Match m in words) Console.WriteLine(m.Value);   // Hello  World  42

// Hex colour
bool isHex = Regex.IsMatch("#1aF9b3", @"^#[0-9A-Fa-f]{6}$");  // True

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// Quantifiers
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// *    — 0 or more (greedy)
// +    — 1 or more (greedy)
// ?    — 0 or 1
// {n}  — exactly n
// {n,} — at least n
// {n,m}— between n and m
// *?   — 0 or more (lazy)
// +?   — 1 or more (lazy)

string text = "<b>bold</b> and <i>italic</i>";

// Greedy — matches as much as possible
var greedy = Regex.Match(text, @"<.+>");
Console.WriteLine(greedy.Value);   // <b>bold</b> and <i>italic</i>

// Lazy — matches as little as possible
var lazy = Regex.Match(text, @"<.+?>");
Console.WriteLine(lazy.Value);     // <b>

// All tags (lazy)
var tags = Regex.Matches(text, @"<.+?>");
foreach (Match m in tags) Console.WriteLine(m.Value);
// <b>  </b>  <i>  </i>

// Phone number: exactly 10 digits
bool validPhone = Regex.IsMatch("5551234567", @"^\d{10}$");  // True

// Postal code: 5 or 9 digits (US ZIP)
bool zip = Regex.IsMatch("12345-6789", @"^\d{5}(-\d{4})?$");  // True

// Password: at least 8 chars, one upper, one lower, one digit
bool strongPwd = Regex.IsMatch("MyPass1!", @"^(?=.*[A-Z])(?=.*[a-z])(?=.*\d).{8,}$"); // True

//  Split on multiple delimiters ———————————————————————————————
string csv = "one, two;three|four";
string[] parts = Regex.Split(csv, @"[,;|]\s*");
Console.WriteLine(string.Join(" | ", parts));  // one | two | three | four
```

**Quantifier quick reference:**

| Quantifier | Meaning | Example | Matches |
|-----------|---------|---------|---------|
| `*` | 0 or more | `a*` | `""`, `"a"`, `"aaa"` |
| `+` | 1 or more | `a+` | `"a"`, `"aaa"` |
| `?` | 0 or 1 | `colou?r` | `"color"`, `"colour"` |
| `{3}` | Exactly 3 | `\d{3}` | `"123"` |
| `{2,4}` | 2 to 4 | `\w{2,4}` | `"ab"`, `"abcd"` |
| `*?` | Lazy 0+ | `<.*?>` | Shortest match |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use named capturing groups and `Regex.Split` in C#?

**Named groups** (`(?<name>pattern)`) allow you to refer to captured substrings by name instead of index, making patterns more readable and maintainable. **`Regex.Split`** divides a string at each match of a pattern.

```cs
using System.Text.RegularExpressions;

//  1. Named groups —————————————————————————————————————————————
// Parse a date in the format  YYYY-MM-DD
var datePattern = new Regex(@"(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})");

Match m = datePattern.Match("Event on 2026-04-19 at noon");
if (m.Success)
{
    Console.WriteLine(m.Groups["year"].Value);   // 2026
    Console.WriteLine(m.Groups["month"].Value);  // 04
    Console.WriteLine(m.Groups["day"].Value);    // 19
    Console.WriteLine($"{m.Groups["day"].Value}/{m.Groups["month"].Value}/{m.Groups["year"].Value}");
    // 19/04/2026
}

//  2. Multiple matches with named groups ———————————————————————
string log = "ERROR 2026-01-10 | INFO 2026-01-11 | WARN 2026-01-12";
var logPattern = new Regex(@"(?<level>\w+) (?<date>\d{4}-\d{2}-\d{2})");

foreach (Match entry in logPattern.Matches(log))
    Console.WriteLine($"{entry.Groups["level"].Value} on {entry.Groups["date"].Value}");
// ERROR on 2026-01-10
// INFO  on 2026-01-11
// WARN  on 2026-01-12

//  3. Backreference with named group ———————————————————————————
// Match doubled words: "the the", "is is"
var doubled = new Regex(@"\b(?<word>\w+)\s+\k<word>\b", RegexOptions.IgnoreCase);
Console.WriteLine(doubled.IsMatch("The the quick fox"));  // True
string cleaned = doubled.Replace("This is is a test test.", "${word}");
Console.WriteLine(cleaned);  // This is a test.

//  4. Replace using named group references —————————————————————
string dates = "Born: 1990-06-15, Hired: 2015-03-22";
// Reformat YYYY-MM-DD ’ DD/MM/YYYY
string reformatted = Regex.Replace(dates,
    @"(?<y>\d{4})-(?<m>\d{2})-(?<d>\d{2})",
    "${d}/${m}/${y}");
Console.WriteLine(reformatted);  // Born: 15/06/1990, Hired: 22/03/2015

//  5. Regex.Split ——————————————————————————————————————————————
// Basic split on whitespace
string sentence = "Split   this\tsentence\nnow";
string[] words = Regex.Split(sentence, @"\s+");
Console.WriteLine(string.Join("|", words));  // Split|this|sentence|now

// Split on multiple delimiters
string data = "one,two;three|four::five";
string[] items = Regex.Split(data, @"[,;|:]+");
Console.WriteLine(string.Join(" ", items));  // one two three four five

// Split and keep the delimiter (place pattern in capture group)
string csv = "a,b,c";
string[] withSeps = Regex.Split(csv, @"(,)");
Console.WriteLine(string.Join(" ", withSeps));  // a , b , c

// Split into fixed-width chunks
string hex = "DEADBEEFCAFE";
string[] bytes2 = Regex.Split(hex, @"(?<=\G.{2})(?=.)");  // every 2 chars
Console.WriteLine(string.Join("-", bytes2));  // DE-AD-BE-EF-CA-FE
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are lookahead and lookbehind assertions in regular expressions?

**Lookahead** (`(?=...)`, `(?!...)`) and **lookbehind** (`(?<=...)`, `(?<!...)`) are **zero-width assertions** — they check what\'s ahead/behind the current position without consuming characters.

```cs
using System.Text.RegularExpressions;

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// Positive lookahead  (?=pattern)  — position is followed by pattern
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// Find all numbers followed by "px"
var pixelValues = Regex.Matches("width:100px height:200px margin:5em", @"\d+(?=px)");
foreach (Match m in pixelValues) Console.Write($"{m.Value} "); // 100 200

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// Negative lookahead  (?!pattern)  — NOT followed by pattern
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// Match numbers NOT followed by "px"
var nonPx = Regex.Matches("width:100px margin:5em font:16px zoom:2", @"\d+(?!px)");
foreach (Match m in nonPx) Console.Write($"{m.Value} "); // 5 2

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// Positive lookbehind  (?<=pattern)  — preceded by pattern
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// Extract prices (number preceded by "$")
var prices = Regex.Matches("Items: $9.99, $24.50, £5.00", @"(?<=\$)\d+\.\d{2}");
foreach (Match m in prices) Console.Write($"{m.Value} "); // 9.99 24.50

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// Negative lookbehind  (?<!pattern)  — NOT preceded by pattern
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// Find digits NOT preceded by "$"
var nonDollar = Regex.Matches("tax:$10 qty:5 price:$99", @"(?<!\$)\b\d+\b");
foreach (Match m in nonDollar) Console.Write($"{m.Value} "); // 5

//  Password strength — lookaheads for multiple requirements ———
// At least 8 chars, one uppercase, one lowercase, one digit, one special char
string passwordPattern = @"^(?=.*[A-Z])(?=.*[a-z])(?=.*\d)(?=.*[!@#$%^&*]).{8,}$";
Console.WriteLine(Regex.IsMatch("MyP@ss1!", passwordPattern));    // True
Console.WriteLine(Regex.IsMatch("weakpass", passwordPattern));    // False

//  Insert separator before each uppercase in camelCase ————————
// Lookbehind to find position after lowercase, lookahead for uppercase
string camel = "getFirstNameById";
string snake = Regex.Replace(camel, @"(?<=[a-z])(?=[A-Z])", "_").ToLower();
Console.WriteLine(snake);  // get_first_name_by_id

//  Remove trailing whitespace per line (multiline mode) ———————
string multiline = "Hello   \nWorld  \nDone";
string trimmed = Regex.Replace(multiline, @"[ \t]+(?=\r?\n|$)", "",
    RegexOptions.Multiline);
Console.WriteLine(trimmed);
// Hello
// World
// Done
```

**Lookaround summary:**

| Syntax | Name | Description |
|--------|------|-------------|
| `(?=abc)` | Positive lookahead | Followed by `abc` |
| `(?!abc)` | Negative lookahead | NOT followed by `abc` |
| `(?<=abc)` | Positive lookbehind | Preceded by `abc` |
| `(?<!abc)` | Negative lookbehind | NOT preceded by `abc` |

**Key property:** Lookarounds are **zero-width** — they assert a condition but do not consume any characters, so the matched text does not include the lookaround content.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 9. EXCEPTION HANDLING

<br>

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
 System.SystemException          (CLR/runtime exceptions)
    NullReferenceException
    IndexOutOfRangeException
    InvalidOperationException
    ArgumentException
       ArgumentNullException
       ArgumentOutOfRangeException
    IOException
       FileNotFoundException
    OverflowException
    FormatException
    StackOverflowException
    OutOfMemoryException
 System.ApplicationException     (user/app exceptions — rarely used directly)
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
| **Stack trace** | Preserved … | Reset to current line  | New exception, new trace |
| **Use when** | Re-throwing the same exception |  Avoid — loses origin | Wrapping with context |
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
    throw; // … preserves full stack trace
}

// throw ex — resets stack trace (AVOID)
// catch (FileNotFoundException ex)
// {
//     throw ex; //  stack trace now starts HERE, original location lost
// }

// throw new — wrap with context (preserve original as InnerException)
try
{
    await LoadConfigAsync("appsettings.json");
}
catch (IOException ex)
{
    // Add domain context while preserving original exception
    throw new ApplicationException("Failed to start: config unavailable.", innerException: ex); // …
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
    Console.WriteLine("finally: always runs"); // … runs
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
    Console.WriteLine("finally: runs after catch"); // … runs
}

// Case 3: Exception NOT caught — finally still runs, then exception propagates
try
{
    try { throw new Exception("unhandled"); }
    finally { Console.WriteLine("finally: runs before propagation"); } // … runs
}
catch (Exception ex) { Console.WriteLine($"outer catch: {ex.Message}"); }

// Case 4: return in try — finally still runs before the method returns
int Calculate()
{
    try    { return 42; }
    finally { Console.WriteLine("finally: runs even with return"); } // …
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
| **Modern guidance** | Do not catch directly — catch specific types |  **Obsolete pattern** — avoid |
| **Recommended now** | Catch specific `SystemException` subtypes | Derive directly from `Exception` |

```cs
//  ApplicationException was originally meant as the base for app exceptions
// — this guidance was ABANDONED in .NET 2.0 — the pattern is now discouraged

//  Old pattern (avoid)
// public class MyException : ApplicationException { }

// … Modern pattern — derive directly from Exception
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
// catch (SystemException ex) { }  catches way too much

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
| **Recoverable** |  Not catchable (terminates process in .NET) |  Sometimes catchable but rarely recoverable |
| **Common trigger** | Infinite recursion, very deep call chains | Large allocations, memory leaks, huge arrays |
| **Prevention** | Add base cases, use iteration, `Span<T>` | Pool objects, use `ArrayPool`, reduce allocations |

```cs
// StackOverflowException — infinite recursion
// int Factorial(int n) => n == 0 ? 1 : n * Factorial(n); // BUG: missing base case
// ’ StackOverflowException — process terminates, cannot be caught!

// Correct: proper base case
int Factorial(int n) => n <= 1 ? 1 : n * Factorial(n - 1); // …

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

    // … Implement IDisposable for deterministic cleanup
    public void Dispose()
    {
        FreeResource(_handle);
        GC.SuppressFinalize(this); // tell GC: no need to finalize
        Console.WriteLine("Dispose called");
    }

    private static IntPtr AllocateResource() => new IntPtr(1);
    private static void FreeResource(IntPtr h) { }
}

// … Always prefer using/Dispose over relying on finalizer
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
    Console.WriteLine($"1st: {ex.GetType().Name}"); // … This runs
}
catch (ArgumentException ex)
{
    Console.WriteLine($"2nd: {ex.GetType().Name}"); //  NEVER reached
}
catch (Exception ex)
{
    Console.WriteLine($"3rd: {ex.GetType().Name}"); //  NEVER reached
}

// Correct ordering: most specific ’ most general
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

## Q. What are exception filters (`when` clause) in C# and how are they used?

**Exception filters** (C# 6+) allow you to conditionally catch an exception only when a boolean expression evaluates to `true`. The `when` keyword attaches a filter to a `catch` block. If the filter is `false`, the exception is not caught and continues to propagate — **without unwinding the stack**, which preserves the original call stack for debugging.

```cs
//  1. Basic exception filter ———————————————————————————————————
try
{
    int result = int.Parse(Console.ReadLine() ?? "");
    Console.WriteLine(100 / result);
}
catch (FormatException ex) when (ex.Message.Contains("Input string"))
{
    Console.WriteLine("Please enter a valid integer.");
}
catch (DivideByZeroException) when (DateTime.Now.DayOfWeek != DayOfWeek.Sunday)
{
    Console.WriteLine("Division by zero on a weekday.");
}

//  2. Filter on HttpStatusCode —————————————————————————————————
static async Task FetchDataAsync(string url)
{
    try
    {
        using var client = new System.Net.Http.HttpClient();
        var response = await client.GetAsync(url);
        response.EnsureSuccessStatusCode();
    }
    catch (System.Net.Http.HttpRequestException ex)
        when ((int?)ex.StatusCode == 404)
    {
        Console.WriteLine($"Resource not found: {url}");
    }
    catch (System.Net.Http.HttpRequestException ex)
        when ((int?)ex.StatusCode >= 500)
    {
        Console.WriteLine($"Server error ({ex.StatusCode}): {url}");
    }
}

//  3. Logging filter (side-effect without catching) ————————————
static bool Log(Exception ex)
{
    Console.Error.WriteLine($"[LOG] {ex.GetType().Name}: {ex.Message}");
    return false;   //  never catch — just log and re-throw
}

try
{
    throw new InvalidOperationException("Something went wrong");
}
catch (Exception ex) when (Log(ex))   // Log is called, returns false ’ not caught
{
    // Never reached
}
// The exception propagates with the original stack intact

//  4. Multiple filters on the same exception type ———————————————
static void ProcessOrder(int orderId)
{
    try
    {
        if (orderId <= 0)
            throw new ArgumentOutOfRangeException(nameof(orderId), orderId, "Must be > 0");
        if (orderId > 1_000_000)
            throw new ArgumentOutOfRangeException(nameof(orderId), orderId, "Must be <= 1 000 000");
    }
    catch (ArgumentOutOfRangeException ex) when (ex.ActualValue is int v && v <= 0)
    {
        Console.WriteLine("Order ID must be positive.");
    }
    catch (ArgumentOutOfRangeException ex) when (ex.ActualValue is int v && v > 1_000_000)
    {
        Console.WriteLine("Order ID too large.");
    }
}
```

**`when` vs regular `catch`:**

| Aspect | `catch (T ex)` | `catch (T ex) when (condition)` |
|--------|---------------|--------------------------------|
| Stack unwound | Yes | Only if condition is `true` |
| Condition | None | Boolean expression |
| Re-throw fidelity | Needs `throw;` | Stack preserved if not caught |
| Multiple blocks for same type |  Compile error | … Allowed |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the common built-in exception types in C# and when are they thrown?

.NET defines a rich hierarchy of exception types under `System.Exception`. Understanding when each is thrown helps write targeted `catch` blocks.

```cs
//  Exception hierarchy (simplified) ———————————————————————————
// System.Exception
//  System.SystemException          (runtime errors)
//     ArgumentException
//        ArgumentNullException
//        ArgumentOutOfRangeException
//     InvalidOperationException
//     NullReferenceException
//     IndexOutOfRangeException
//     InvalidCastException
//     OverflowException
//     DivideByZeroException
//     StackOverflowException      (non-catchable)
//     OutOfMemoryException
//     NotImplementedException
//     NotSupportedException
//     TimeoutException
//     OperationCanceledException
//        TaskCanceledException
//     IOException
//         FileNotFoundException
//         DirectoryNotFoundException
//         EndOfStreamException
//  System.ApplicationException    (app-level — rarely used directly)

//  Demonstration of common exceptions ——————————————————————————
// 1. ArgumentNullException
static void Greet(string name)
{
    ArgumentNullException.ThrowIfNull(name);         // .NET 6+ helper
    Console.WriteLine($"Hello, {name}!");
}

// 2. ArgumentOutOfRangeException
static string GetElement(string[] arr, int i)
{
    ArgumentOutOfRangeException.ThrowIfNegative(i);
    ArgumentOutOfRangeException.ThrowIfGreaterThanOrEqual(i, arr.Length);
    return arr[i];
}

// 3. InvalidOperationException — wrong state
class EmailSender
{
    private bool _connected;
    public void Connect() => _connected = true;
    public void Send(string msg)
    {
        if (!_connected) throw new InvalidOperationException("Call Connect() first.");
        Console.WriteLine($"Sent: {msg}");
    }
}

// 4. NullReferenceException — accessing null member
string? s = null;
try { _ = s!.Length; }
catch (NullReferenceException) { Console.WriteLine("Null ref"); }

// 5. IndexOutOfRangeException
int[] nums = [1, 2, 3];
try { _ = nums[5]; }
catch (IndexOutOfRangeException ex) { Console.WriteLine(ex.Message); }

// 6. InvalidCastException
object obj = "hello";
try { int n = (int)obj; }
catch (InvalidCastException) { Console.WriteLine("Bad cast"); }

// 7. OverflowException (inside checked block)
try { int x = checked(int.MaxValue + 1); }
catch (OverflowException) { Console.WriteLine("Arithmetic overflow"); }

// 8. FileNotFoundException
try { string _ = File.ReadAllText(@"C:\missing.txt"); }
catch (FileNotFoundException ex) { Console.WriteLine($"File not found: {ex.FileName}"); }

// 9. OperationCanceledException / TaskCanceledException
using var cts = new CancellationTokenSource(millisecondsDelay: 10);
try
{
    await Task.Delay(1000, cts.Token);
}
catch (OperationCanceledException)
{
    Console.WriteLine("Task cancelled.");
}
```

**Common exception types summary:**

| Exception | Thrown when |
|-----------|-------------|
| `ArgumentNullException` | `null` passed for non-nullable parameter |
| `ArgumentOutOfRangeException` | Value outside allowed range |
| `InvalidOperationException` | Object in wrong state for operation |
| `NullReferenceException` | Dereferencing a `null` object |
| `IndexOutOfRangeException` | Array/string index out of bounds |
| `InvalidCastException` | Invalid type cast |
| `OverflowException` | Arithmetic overflow in `checked` context |
| `DivideByZeroException` | Integer division by zero |
| `FileNotFoundException` | File path does not exist |
| `NotImplementedException` | Method body not yet written |
| `NotSupportedException` | Operation not supported by this type |
| `TaskCanceledException` | `CancellationToken` cancelled a `Task` |
| `AggregateException` | One or more async/parallel task failures |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the best practices for exception handling in C#?

```cs
using System.IO;

//  1. Catch only what you can handle ———————————————————————————
//  Swallowing all exceptions hides bugs
try { /*  */ }
catch (Exception) { }    // silent swallow — avoid

// … Catch the most specific type you can actually handle
try
{
    string data = File.ReadAllText("config.json");
}
catch (FileNotFoundException)
{
    // Handle missing config specifically
    Console.WriteLine("Config not found — using defaults.");
}

//  2. Use `finally` or `using` for resource cleanup ————————————
// … using — preferred for IDisposable resources
await using var conn = new System.Data.SqlClient.SqlConnection("");
await conn.OpenAsync();

//  3. Re-throw with `throw;` not `throw ex;` ———————————————————
static void LoadData()
{
    try { File.ReadAllText("data.csv"); }
    catch (IOException ex)
    {
        //  throw ex;  — resets the stack trace
        // … throw;     — preserves original stack trace
        Console.Error.WriteLine($"Failed to load: {ex.Message}");
        throw;
    }
}

//  4. Wrap low-level exceptions in meaningful ones —————————————
public class UserRepository
{
    public User FindById(int id)
    {
        try
        {
            // DB call 
            throw new System.Data.SqlClient.SqlException(); // simulated
        }
        catch (System.Data.SqlClient.SqlException ex)
        {
            throw new RepositoryException($"Failed to load user {id}.", ex); // preserves inner
        }
    }
}

// Custom exception
public class RepositoryException : Exception
{
    public RepositoryException(string message, Exception inner) : base(message, inner) { }
}
public record User(int Id, string Name);

//  5. Use exception filters for conditional handling ———————————
try { /* network call */ }
catch (TimeoutException ex) when (ex.Message.Contains("read"))
{
    Console.WriteLine("Read timeout — retry.");
}

//  6. Validate early — avoid exceptions as control flow ————————
//  Using exceptions as flow control
static int ParseAgeException(string s)
{
    try { return int.Parse(s); }
    catch (FormatException) { return -1; }
}

// … Use TryParse / guard clauses
static int ParseAgeSafe(string s)
    => int.TryParse(s, out int age) ? age : -1;

//  7. Log exceptions with context ——————————————————————————————
static void ProcessOrder(int orderId)
{
    try { /*  */ }
    catch (Exception ex)
    {
        // Log full exception (type, message, stack trace, inner)
        Console.Error.WriteLine($"[ERROR] ProcessOrder({orderId}): {ex}");
        throw;  // re-throw — don\'t hide the exception from callers
    }
}

//  8. Never catch StackOverflowException / ExecutionEngineException
// These are non-recoverable — the process must terminate.

//  9. Handle async exceptions properly —————————————————————————
static async Task<string> DownloadAsync(string url)
{
    using var client = new System.Net.Http.HttpClient();
    try
    {
        return await client.GetStringAsync(url);
    }
    catch (System.Net.Http.HttpRequestException ex)
    {
        Console.Error.WriteLine($"HTTP error: {ex.StatusCode} — {ex.Message}");
        return string.Empty;
    }
}

//  10. Global unhandled exception handlers —————————————————————
// In Program.cs / top-level setup:
AppDomain.CurrentDomain.UnhandledException += (_, e) =>
    Console.Error.WriteLine($"Unhandled: {e.ExceptionObject}");

TaskScheduler.UnobservedTaskException += (_, e) =>
{
    Console.Error.WriteLine($"Unobserved task: {e.Exception.Message}");
    e.SetObserved();
};
```

**Best practice checklist:**

| Practice | Reason |
|----------|--------|
| Catch specific exception types | Avoids hiding unexpected errors |
| Never swallow exceptions silently | Bugs become invisible |
| Use `throw;` not `throw ex;` | Preserves original stack trace |
| Wrap with meaningful exception | Adds domain context |
| Use `finally`/`using` for cleanup | Resources released even on failure |
| Validate inputs early | Prevents exceptions as flow control |
| Log full exception object | Stack trace + inner exception captured |
| Don\'t catch non-recoverable exceptions | `StackOverflowException`, `OutOfMemoryException` |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>
