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
* *[Entity Framework](entity-framework.md)*
* *[Unit Testing](unit-testing.md)*
* *[Design Patterns](design-patterns.md)*

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

### Q. What is C# and what are its main features?

C# is a modern, object-oriented programming language developed by Microsoft as part of the .NET platform. It is designed for building a wide range of applications, including web, desktop, mobile, cloud, and games.

Main features of C#:
 
* **Object-Oriented Programming:** C# is built on the principles of OOP, enabling developers to create modular, reusable, and maintainable code through classes, objects, inheritance, and polymorphism. 

* **Type Safety:** C# enforces type safety, helping to prevent errors by ensuring that variables are used with the correct data types, enhancing code reliability and security. 

* **Garbage Collection:** C# utilizes automatic garbage collection, freeing developers from manual memory management and reducing the risk of memory leaks.

* **Interoperability:** C# allows seamless integration with other languages, including C++ and other languages within the .NET ecosystem, making it easier to reuse existing code and build complex systems. 

* **LINQ (Language Integrated Query):** LINQ provides a powerful and unified way to query data from various sources, such as databases, collections, and XML files, simplifying data access and manipulation.

* **Asynchronous Programming:** C# supports asynchronous programming through the async/await keywords, enabling non-blocking operations and improving application responsiveness, especially for web and mobile applications.

* **Dynamic Programming:** C# also supports dynamic programming, allowing code to adapt to runtime conditions and improve flexibility.

* **.NET Framework Integration:** C# is tightly integrated with the .NET Framework, providing access to a wide range of libraries, tools, and features for building various types of applications. 

* **Cross-Platform Development:** The .NET platform, which C# is a core part of, supports cross-platform development, enabling developers to create applications that run on Windows, Linux, and macOS. 

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

### Q. What are the different types of data types available in C#?

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

* **Integral types**: byte, sbyte, short, ushort, int, uint, long, ulong, char
* **Floating-point types**: float, double
* **Decimal type**: decimal
* **Boolean type**: bool
* **Structs**: Custom value types (e.g., DateTime, Guid)
* **Enumerations**: enum

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
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

### Q. Can primitive data types be stored in heap?

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

### Q. It is possible to store mixed datatypes such as int, string, float, char all in one array?

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
* Using object[] allows mixed types, but it's generally better to use generic collections or tuples for type safety and clarity.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

### Q. What is an extension method in C# and how is it implemented?

In C#, an extension method is a special type of static method that allows you to add functionality to existing types (classes, structs, interfaces) without modifying their source code. 

It\'s implemented by defining a static method within a static class, with the first parameter specifying the type to be extended and prefixed with the `this` keyword. The result is that the extension method can be called as if it were an instance method of the extended type. 

**Example:**

```cs
// Extension method for the string type
public static class StringExtensions {
    public static string Reverse(this string input) {
        char[] chars = input.ToCharArray();
        Array.Reverse(chars);
        return new string(chars);
    }
}

// Use the extension method
public class Example {
    public static void Main(string[] args) {
        string text = "Hello";
        string reversedText = text.Reverse(); // Call the extension method
        Console.WriteLine(reversedText); // Output: olleH
    }
}
```

In this example, the `StringExtensions` class contains the `Reverse` extension method. The `this string input` part indicates that this method extends the `string` type. You can then call `text.Reverse()` just like a regular instance method of the `string` type,

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

### Q. What is a generic type in C# and why is it used?

Generics are fundamental feature that enhances code reusability, type safety, and flexibility. They allow you to create classes, methods, delegates, and interfaces that work with different data types, making code more versatile and efficient.

**Features:**

* **Type Safety**: Generics allow you to write code that works with any data type while maintaining compile-time type checking.

* **Code Reusability**: You can create classes and methods that work with different types without duplicating code.

* **Performance**: Generics avoid the need for boxing/unboxing and type casting, which improves performance compared to non-generic collections.

**Example:**

```cs
using System;

// define a generics class named Employee
public class Employee<T>
{
    // define a variable of type T 
    public T data;

    // define a constructor of the Employee class 
    public Employee(T data)
    {
        this.data = data;
        Console.WriteLine("Data: " + this.data);
    }
}

class Program
{
    static void Main()
    {
        // create an instance with data type string 
        Employee<string> employeeName = new Employee<string>("Pradeep Kumar");

        // create an instance with data type int
        Employee<int> employeeId = new Employee<int>(28);
    }
}
```

Output:

```cs
Data: Pradeep Kumar
Data: 28
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

### Q. What is an anonymous method in C# and how is it used?

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

### Q. What is the difference between a method and a function in C#?

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

### Q. What is the Common Language Runtime (CLR) in C#?

The Common Language Runtime (CLR) is the core execution engine for the .NET Framework, including C# programs. It manages the execution, lifecycle, and resources of .NET applications. CLR provides a managed execution environment that handles tasks like memory allocation, security, exception handling etc.

**Key Functions:**

* **Execution and Management:** CLR manages the execution of .NET applications, ensuring they run smoothly and efficiently. 

* **Memory Management:** CLR provides automatic garbage collection, freeing developers from manual memory management. 

* **Security:** CLR enforces security policies and ensures that applications are protected from malicious code. 

* **Exception Handling:** CLR handles exceptions and errors, preventing application crashes. 

* **Cross-Language Interoperability:** CLR allows code written in different .NET languages to interoperate and share resources. 

* **Just-In-Time (JIT) Compilation:** CLR uses JIT compilers to convert Common Intermediate Language (CIL) code to native machine code at runtime. 

* **Metadata:** CLR uses metadata, which is information about the application\'s structure (types, objects, members, etc.), to manage the execution process. 

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

### Q. What is the purpose of namespaces in C#?

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

### Q. What is the purpose of the `using` statement in C#?

The `using` statement in C# is used to ensure that objects which consume unmanaged resources (like files, database connections, or network streams) are properly disposed of as soon as they are no longer needed. It provides a convenient syntax that automatically calls the `Dispose()` method on the object when the code block is exited, even if an exception occurs.

**Purpose:**

* Automatically releases resources held by an object implementing IDisposable.
* Helps prevent resource leaks and improves application reliability.

**Example:**

```cs
using (var file = new StreamReader("example.txt"))
{
    string content = file.ReadToEnd();
    // file is automatically closed and disposed here
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

### Q. What are properties in C# and how are they used?

Properties in C# are special members that provide a flexible mechanism to read, write, or compute the values of private fields. Properties also support encapsulation and abstraction through "get" and "set" methods for accessing and modifying them.

**Example:**

```cs
public class Person
{
    // Private field
    private string name;

    // Property with get and set
    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    // Auto-implemented property
    public int Age { get; set; }

    // Read-only property
    public string Info
    {
        get { return $"Name: {Name}, Age: {Age}"; }
    }
}

// Use the extension method
public class Example {
    public static void Main(string[] args) {
        var person = new Person();
        person.Name = "Pradeep";
        person.Age = 30;
        Console.WriteLine(person.Info); // Output: Pradeep, Age 30
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

### Q. What are the Arrays in C#.Net?

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

### Q. What is the difference between the System.Array.CopyTo() and System.Array.Clone()?

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

### Q. What is jagged array in C#.Net?

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

### Q. How can you sort the elements of the array in descending order?

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

### Q. What is the difference between `var` and `dynamic` types?

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

Console.WriteLine(value.NonExistentMethod()); // Compiles, but throws runtime exception if method doesn't exist
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

### Q. What is a `struct` in C#?

A struct (short for structure) is a data type that allows grouping related variables together into a single unit. It is similar to a class but is a value type, meaning that when you create an instance of a struct, the data itself is stored, not a reference to the data. 

Structs are typically used for smaller, more lightweight data structures that are often copied rather than referenced. 

**Differences from Classes:**

* **Value Type:** Structs are stored on the stack (unless boxed or part of a reference type), and assignments copy the entire value.
* **No Inheritance:** Structs cannot inherit from other structs or classes (except for interfaces).
* **Default Constructor:** Structs cannot have a parameterless constructor (the compiler provides one automatically).
* **Immutability:** Structs are often used for immutable data, though they can have mutable fields.

**Example:**

```cs
public struct Point
{
    public int X;
    public int Y;

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }
}

// Usage
Point p1 = new Point(10, 20);
Point p2 = p1; // p2 is a copy of p1
p2.X = 30;     // p1.X remains 10

Console.WriteLine(p1.X); // Output: 10
Console.WriteLine(p2.X); // Output: 30
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

### Q. What is the difference between `abstract` and `virtual` methods?

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

### Q. What is the difference between `out` and `ref` parameters?

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

### Q. What is a `Tuple` in C#?

A Tuple is a data structure that allows you to store a fixed number of elements, potentially of different types, in a single object. Tuples are useful for grouping related values without creating a custom class or struct.

**Features:**

* It allows us to represent multiple data into a single data set.
* It returns multiple values from a method without using out parameter.
* It can also store duplicate elements.
* It allows us to pass multiple values to a method with the help of single parameters.

**Example:**

```cs
```csharp
// Creating a tuple with named elements
var person = (Name: "Pradeep", Age: 28, IsEmployed: true);

// Accessing tuple elements
Console.WriteLine(person.Name);       // Output: Pradeep
Console.WriteLine(person.Age);        // Output: 28
Console.WriteLine(person.IsEmployed); // Output: True
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

### Q. Explain byval and byref?

In C#, **byval** (by value) and **byref** (by reference) describe how arguments are passed to methods:

**By Value (byval):**

- The method receives a **copy** of the variable's value.
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

### Q. What is an immutable string?

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

### Q. What is the JIT compiler process?

The JIT (Just-In-Time) compiler is a core component of the .NET runtime (CLR) that converts Intermediate Language (IL) code into native machine code at runtime, just before execution. This process is "just-in-time" because it only translates CIL code into native code when it's needed for execution

**JIT Compiler Process:**

* **Compilation to IL:** When you build a C# application, the source code is compiled into Intermediate Language (IL), not directly to machine code. This IL is platform-independent and stored in assemblies (.exe or .dll files).

* **Loading and Execution:** When you run the application, the CLR loads the required assemblies and starts executing the code.

* **JIT Compilation:** As methods are called for the first time, the JIT compiler translates the IL code of those methods into native machine code specific to the underlying CPU and operating system.

* **Caching:** The native code generated by the JIT is cached in memory, so subsequent calls to the same method use the already-compiled native code, improving performance.

* **Execution:** The CPU executes the native code directly.

**Benefits:**

* **Platform Independence:** IL code can run on any platform with a compatible CLR/JIT.
* **Optimization:** JIT can optimize code at runtime based on the actual environment.
* **Security:** JIT compilation allows for verification and security checks before execution.

**Summary Diagram:**

```js
   Source Code
      ↓
   Compiler
      ↓
   IL Code (Assembly)
      ↓
   [At Runtime]
      ↓
   JIT Compiler
      ↓
   Native Machine Code
      ↓
   Execution by CPU
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

### Q. Explain the characteristics of value-type variables that are supported in the C# programming language.

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

### Q. What is a parameter? Explain the new types of parameters introduced in C# 4.0.

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

### Q. What are the different types of literals?

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
  - **Verbatim string literals**: Start with `@` and preserve escape sequences and line breaks, e.g., `@"C:\Users\Name"`.

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

### Q. What is the main difference between sub-procedure and function?

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

### Q. What is the difference between string and StringBuilder in C#?

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

### Q. What is difference between late binding and early binding in C#?

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

### Q. What is Indexer in C#?

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

### Q. What are the differences between Object, Var and Dynamic type?

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

### Q. What is the difference between managed and unmanaged code?

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

### Q. What is an Object Pool in C#?

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

### Q. Explain the difference between lazy and eager evaluation in C#?

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

### Q. Mention the two major categories that distinctly classify the variables of C# programs.

In C# programs, variables are primarily categorized into **value types** and **reference types**. Value types directly store the variable\'s value in memory, while reference types store a memory address (reference) to the value\'s location. 

**Value Types:**

These store the actual data directly in the memory location of the variable (Stack). Examples include `int`, `bool`, `float`, `enum`, and `struct` types. When a value type variable is copied, a new copy of the data is created, so changes to one variable don\'t affect others.

**Reference Types:**

These store a memory address to the location where the actual data is stored (Heap). Examples include `string`, `object`, `array`, and `class` types. When a reference type variable is copied, the copy contains the same memory address, meaning both variables point to the same data. Therefore, changes to the data through one reference will be reflected in the other. 

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

### Q. What is checked and unchecked block?

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

### Q. What is the difference between typeOf() and sizeOf()?

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

### Q. What is widening and Narrowing conversion in C#?

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

### Q. How to view an Assembly?

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

### Q. What are MultiLingual Applications?

MultiLingual Applications are software applications designed to support multiple languages, allowing users to interact with the application in their preferred language. In C#, this is typically achieved using resource files (.resx) and the .NET localization framework.

**Key Points:**

- **Localization:** Adapting the application for different languages and regions (e.g., translating UI text, formatting dates/numbers).
- **Resource Files:** Store language-specific strings and resources in separate .resx files (e.g., `Resources.en.resx`, `Resources.fr.resx`).
- **Culture Settings:** The application detects or allows the user to select their culture (language/region), and loads the appropriate resources at runtime.
- **.NET Support:** .NET provides classes like `ResourceManager` and `CultureInfo` to manage localization.

**How it works:**

- Text and UI strings are stored in resource files for each supported language.
- The application loads the appropriate resource file based on the user's culture or language preference.
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

### Q. Can you describe the process of code compilation in .NET?

In .NET, code compilation involves several stages, including translating source code into Common Intermediate Language (CIL) and then executing it using the Common Language Runtime (CLR) with the Just-In-Time (JIT) compiler.

The compiler (like Roslyn for C#) initially converts the high-level source code into CIL, a CPU-independent set of instructions. This CIL code, along with metadata, is stored in an assembly (PE format). When the program runs, the CLR uses the JIT compiler to convert the CIL into machine code, which is then executed on the specific CPU. 

**Process of Code Compilation**

1. **Source Code to Intermediate Language (IL):**
   - When you write C# code and build your project, the C# compiler (`csc.exe`) compiles your source code into an Intermediate Language (IL), also known as Microsoft Intermediate Language (MSIL) or Common Intermediate Language (CIL).
   - The compiled IL code, along with metadata, is stored in assemblies (`.dll` or `.exe` files).

2. **Assembly Loading:**
   - When you run your application, the .NET runtime (CLR – Common Language Runtime) loads the required assemblies.

3. **Just-In-Time (JIT) Compilation:**
   - The CLR uses a Just-In-Time (JIT) compiler to convert the IL code into native machine code specific to the operating system and processor.
   - This compilation happens method-by-method, just before each method is executed for the first time.

4. **Execution:**
   - The native code is executed by the CPU.
   - The JIT-compiled code is cached in memory for subsequent calls, improving performance.

**Summary Diagram:**

```
Source Code (C#)
      ↓
C# Compiler
      ↓
IL Code + Metadata (Assembly)
      ↓
CLR Loads Assembly
      ↓
JIT Compiler
      ↓
Native Machine Code
      ↓
Execution by CPU
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

### Q. Can you return multiple values from a function in C#?

Yes, you can return multiple values from a function in C#. There are several common ways to achieve this:

**1. Using Tuples:**

Tuples allow you to return multiple values of different types in a single return statement.

```cs
(string, int) GetPerson()
{
    return ("Pradeep", 30);
}

// Usage
var person = GetPe//duv.Item1); // Pradeep
Console.WriteLine(person.Item2); // 30
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

### Q. In how many ways you can pass parameters to a method?

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

### Q. What are the different types of operators in C#?
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
    Console.WriteLine("It's a string!");
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

### Q. What is the purpose of the `nameof` operator in C#?
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

### Q. What is difference between const and readonly in C#?
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

### Q. How you would use a bitwise operator in C#? 
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

### Q. Explain the use of the `as` operator in C# and the best way to use it?
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

### Q. What is the use of Null Coalescing Operator (??) in C#? 
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

### Q. What is difference between "is" and "as" operator in C#?
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

### Q. What are nullable types in C#?
---
In C#, nullable types are a way to represent data that can optionally have a value or be null. This is useful when you need to represent the absence of a value, such as optional fields.

**Key Features**

* **HasValue**: Indicates whether the variable contains a non-null value.
* **Value**: Gets the value (throws an exception if HasValue is false).
* **GetValueOrDefault()**: Returns the value or the default for the type if null.

**Example:**
```cs
int? score = null;
if (score.HasValue)
{
    Console.WriteLine($"Score: {score.Value}");
}
else
{
    Console.WriteLine("Score is not set.");
}
```
<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

### Q. What is Type Casting and what are its types in C#?
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

### Q. What is the difference between `==` operator and `.Equals()` method?
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

### Q. What is short-circuit evaluation in C#?
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

### Q. List some different ways for equality check in .Net?
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

### Q. What is difference between static, readonly, and constant in C#?
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

### Q. How to loop through an enum in C#?
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

### Q. How to set default value to Property in C#?
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

### Q. How to convert int to enum in C#?
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

### Q. What is BigInteger Data Type in C#?
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

### Q. How to convert String to Enum in C#?
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

### Q. How to convert an Object to JSON in C#?
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

### Q. How to convert JSON String to Object in C#?
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

### Q. How to Pass or Access Command-line Arguments in C#?
---
In C#. you can access command-line arguments using the Main method's parameter, typicallydefined as a string[] args.

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

### Q. How to convert date object to string in C#?
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

### Q. How to combine two arrays without duplicate values in C#?
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

### Q. How to convert string to int in C#?
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

### Q. What is boxing and unboxing?
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


### Q. What effect does boxing and unboxing have on performance?
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

### Q. What is the difference between `==` and `ReferenceEquals` in C#?
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

### Q. How does operator overloading work in C#?
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

### Q. What is the "=>" operator in C#? Where is it used?
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

### Q. What is the null-conditional operator (?.) and how does it differ from the null-coalescing operator (??)?
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

### Q. What is the purpose of the default literal in C#?
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

### Q. Can you explain the is not pattern introduced in C# 9.0?
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

### Q. What is object-oriented programming?

Object-oriented programming (OOP) in C# is a programming paradigm based on the concept of "objects", which are instances of classes. OOP enables developers to structure software in a modular way by organizing code into reusable components.

**Key principles:**

* **Encapsulation**: Bundles data and methods that operate on the data into a single unit called a class, and restricts direct access to some of the object\'s components.

* **Inheritance**: Allows a class to inherit members (fields, methods, properties) from another class, promoting code reuse.

* **Polymorphism**: Enables objects to be treated as instances of their parent class rather than their actual class, allowing for flexible and interchangeable code.

* **Abstraction**: Hides complex implementation details and exposes only the necessary features of an object.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

### Q. What is a constructor in C# and what are its different types?
### Q. Can you explain the use of the "this" keyword in C#?
### Q. How are static constructors executed in Parent child?
### Q. If a base class has a number of overloaded constructors, and an inheriting class has a number of overloaded constructors; can you enforce a call from an inherited constructor to a specific base constructor?
### Q. If a base class has a bunch of overloaded constructors, and an inherited class has another bunch of overloaded constructors, can you enforce a call from an inherited constructor to an arbitrary base constructor?
### Q. How to access the constructors of one class to another class?
### Q. What is the use of static constructors?
### Q. Can you explain the difference between a static and an instance method in C#?
### Q. What is an abstract class in C# and when is it used?
### Q. What is the difference between an abstract class and an interface?
### Q. What is reflection in .NET and how would you use it?
### Q. When to use reflection and when not in C#?
### Q. What is a sealed class in C# and why is it used?
### Q. What are the benefits of using a sealed class in C#?
### Q. Is it possible for a sealed class to be used as a base class in C#?
### Q. Is it possible for a sealed class in C# to define virtual methods?
### Q. Is it possible for a non-child class to define sealed methods in C#? 
### Q. How can abstract classes be used to implement the Template Method design pattern?
### Q. Can you give an example of how abstract classes can lead to implementation of anti-patterns such as the God Object pattern?
### Q. How can abstract classes be used to implement the Factory Method pattern?
### Q. What are the SOLID principles in C#?
### Q. How can abstract classes be used to implement the Dependency Inversion principle?
### Q. What is the difference between an abstract class and a concrete class in C#, and how does this relate to the Open-Closed principle?
### Q. Can you give an example of how abstract classes can help to enforce the Interface Segregation principle?
### Q. How can abstract classes be used to implement the Strategy design pattern?
### Q. Is it possible to declare abstract methods as private in C#?
### Q. Is it possible for an abstract class to contain a main method in C#?
### Q. Is it possible in C# that a class inherit from multiple abstract classes?
### Q. What is the difference between a sealed class and an unsealed class in C#?
### Q. How can you use a sealed class to prevent inheritance in C#?
### Q. Can we do Multiple inheritance with Abstract classes?
### Q. Whats the difference between Abstract class and interface?
### Q. Why simple base class replace Abstract class?
### Q. What are nested classes and when to use them?
### Q. Can Nested class access outer class variables?
### Q. Can we have public, protected access modifiers in nested class?
### Q. Are private class members inherited to the derived class?
### Q. Where is a protected class-level variable available?
### Q. Are private class-level variables inherited?
### Q. Which class is at the top of .NET class hierarchy?
### Q. What is the .NET collection class that allows an element to be accessed using a unique key?
### Q. Explain the three services model commonly know as a three-tier application?
### Q. Can you prevent your class from being inherited by another class?
### Q. Can you allow a class to be inherited, but prevent the method from being over-ridden?
### Q. When do you absolutely have to declare a class as abstract?
### Q. What is the implicit name of the parameter that gets passed into the set method/property of a class?
### Q. When you inherit a protected class-level variable, who is it available to?
### Q. Are private class-level variables inherited?
### Q. What is the top .NET class that everything is derived from?
### Q. When do you absolutely have to declare a class as abstract (as opposed to free-willed educated choice or decision based on UML diagram)?
### Q. Is it namespace class or class namespace?
### Q. What is the default Access Modifier for the members of the class?
### Q. How to Call the Default constructor of one class with the parameterized constructor of same class?
### Q. What is scope of a Protected Internal member variable of a C# class?    
### Q. What is the difference between Virtual method and Abstract method? 
### Q. What is scope of a Internal member variable of a C# class? 
### Q. How would you implement multiple interfaces with the same method name in the same class?
### Q. How can you create a derived class object from a base class?
### Q. Which is the parent class of all classes which we create in C#?
### Q. What is the base class in .NET framework from which all the classes have been developed?
### Q. How do you implement a custom attribute in C#?
### Q. What is the  System. String and System.Text.StringBuilder classes?
### Q. What are I/O classes in C#? 
### Q. Can you add extension methods to an existing static class? 
### Q. Can you create sealed abstract class in C#?  
### Q. Can you inherit multiple interfaces?
### Q. What interface should your data structure implement to make the "Where" method work? 
### Q. Can we define methods as private in interface?
### Q. If i want to change interface whats the best practice?
### Q. Can we create instance of interface?
### Q. Explain the differences between covariance and contravariance in C# for delegates and interfaces.
### Q. What is an abstraction?
### Q. Is it compulsory to implement Abstract methods?
### Q. What is the method MemberwiseClone() doing? 
### Q. Explain the difference between destructor, dispose and finalize method? 
### Q. Could you explain the difference between Func vs. Action vs. Predicate? 
### Q. What is a namespace and is it compulsory?
### Q. What do you think about empty destructor?
### Q. What are the different types of "USING/HAS A" relationship?
### Q. Differentiate between Composition vs Aggregation vs Association?
### Q. What are circular references? 
### Q. What is weak reference in C#? 
### Q. Explain weak and strong references?
### Q. How can you use Interfaces in C# to reduce coupling and improve maintainability?
### Q. What happens if the inherited interfaces have conflicting method names?
### Q. What is the purpose of the `volatile` keyword in C#?
### Q. What is the purpose of the `checked` keyword in C#?
### Q. What is the purpose of the `this` keyword in C#?
### Q. What is the purpose of the `base` keyword in C#?
### Q. What is the purpose of the `default` keyword in C#?
### Q. What is the purpose of the `params` keyword in C#?
### Q. What is the purpose of the `yield` keyword in C#?
### Q. What is the `async` and `await` keyword in C#?
### Q. What is the difference between `override` and `new` keywords?
### Q. Why do we need the out keyword ?
### Q. What is the params keyword in C#?
### Q. What is the purpose of the yield keyword in C#? Provide an example of using it with an iterator?
### Q. What is the "yield" keyword used for in C#? 
### Q. What is the importance of "this" keyword?
### Q. What is the difference between ref and out keywords?
### Q. What is a `partial` class in C#?
### Q. What is a `static` class in C#?
### Q. What is a `sealed` class in C#?
### Q. What are the different types of classes in C#? 
### Q. What is an Abstract Class? 
### Q. Are private class members inherited to the derived class?
### Q. What are partial classes?
### Q. What is the difference between a struct and a class in C#?
### Q. What is the difference between `public`, `private`, `protected`, and `internal` access modifiers?
### Q. C# provides a default constructor for me. I write a constructor that takes a string as a parameter, but want to keep the no parameter one. How many constructors should I write?
### Q. Why do we need constructors?
### Q. In parent child which constructor fires first?
### Q. What is the Constructor Chaining in C#?
### Q. What are the different Ways of method can be overloaded?
### Q. When and why to use method overloading?
### Q. Is operator overloading supported in C#?
### Q. Can "this" be used within a static method? 
### Q. Can you declare an override method to be static if the original method is not static?
### Q. Can you declare the override method static while the original method is non-static?
### Q. Why can you have only one static constructor?
### Q. When does static constructor fires?
### Q. When the static constructor will be called?
### Q. Can we declare Public access modifier for static constructor?
### Q. If we declare Main() and static constructor in the same class which one will be called first?
### Q. How does dependency inversion benefit, show with an example?
### Q. Will only Dependency inversion solve decoupling problem?
### Q. What is the difference between a value type and a reference type in C#?
### Q. What is encapsulation in C# and why is it important?
### Q. How do you implement encapsulation in C#?
### Q. Can you provide an example of encapsulation in C#?
### Q. What are the benefits of using encapsulation in object-oriented programming?
### Q. How does encapsulation help in maintaining code?
### Q. What is the difference between encapsulation and abstraction in C#?
### Q. How can encapsulation be violated in C#?
### Q. How do access modifiers (public, private, protected, internal) relate to encapsulation in C#?
### Q. Explain method hiding?
### Q. What is polymorphism?
### Q. Explain the four pillars of OOP in C#?
### Q. What is the purpose of using statement in C#?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 4. INHERITANCE

<br/>

### Q. What is inheritance in C# and how does it work?
### Q. What is the difference between single inheritance and multiple inheritance in C#?
### Q. How can you use the "base" keyword in C# to call the base class constructor?
### Q. Can you explain polymorphism and how it is achieved in C#?
### Q. How can you use inheritance and polymorphism together to achieve dynamic dispatch in C#?
### Q. What is the purpose of the base keyword in C#, and how is it used in inheritance?
### Q. What are the differences between virtual methods and abstract methods in C#, and when each should be used?
### Q. How do you handle constructor inheritance in C#, and what is the role of the base keyword in constructor inheritance?
### Q. What is the difference between inheritance and composition in C# and when should you use each one?
### Q. Can you override private virtual methods?
### Q. What does the keyword virtual mean in the method definition?
### Q. What is a Virtual Method in C#?
### Q. Why a private virtual method cannot be overridden in C#?
### Q. How do you override a method in C#?
### Q. What is the difference between `override` and `new` keywords in C#?
### Q. How does inheritance promote code reusability?
### Q. What are the potential pitfalls of using inheritance?
### Q. How do you prevent a class from being inherited in C#?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 5. COLLECTIONS

<br/>

### Q. What are collections in C# and why are they used?
### Q. What is the difference between an array and a collection in C#?
### Q. What are the different types of collections available in C#?
### Q. What are Concurrent Collection Classes?
### Q. Explain Hashtable in C#?
### Q. What is IEnumerable<> in C#?
### Q. What is the difference between a BlockingCollection and a ConcurrentQueue or ConcurrentStack? In which scenarios would you choose to use the BlockingCollection, and why?
### Q. Explain the difference between IQueryable, ICollection, IList & IDictionary interfaces? 
### Q. What is difference between Hashtable and Dictionary?
### Q. What is difference between SortedList and SortedDictionary in C#?
### Q. What is the difference between Array and ArrayList?
### Q. Whose performance is better array or arraylist?
### Q. What class is underneath the Sorted List class?
### Q. IEnumerable vs List - What to Use? How do they work? 
### Q. When to use ArrayList over array[] in C#?
### Q. What are the differences between IEnumerable and IQueryable?
### Q. How to sort the generic SortedList in the descending order?
### Q. What is a `HashSet` and when would you use it?
### Q. How is LinkedList used in C#?
### Q. What is difference between Stack and Heap?
### Q. Are string allocated on stack or heap?
### Q. How many stack and heaps are created for an application?
### Q. How are stack and heap memory deallocated?
### Q. Who clears the heap memory?
### Q. Where is structure allocated Stack or Heap?
### Q. Can structures get created on Heap?
### Q. Is there a way we can see this Heap memory?
### Q. Explain stack and Heap?
### Q. Where are stack and heap stored?
### Q. What goes on stack and what goes on heap?
### Q. How is the stack memory address arranged?
### Q. How is stack memory deallocated LIFO or FIFO?
### Q. How do you choose between `List<T>` and `ArrayList` in C#?
### Q. How do you iterate over a collection in C#?
### Q. What is the difference between `IEnumerable` and `ICollection` in C#?
### Q. How do you sort a collection in C#?
### Q. How do you remove duplicates from a collection in C#?
### Q. What is the difference between `Queue` and `Stack` in C#?
### Q. How do you convert an array to a collection in C#?
### Q. What are the thread-safe collection classes available in C#?
### Q. How do you use LINQ with collections in C#?
### Q. What is the difference between `Array` and `List` in C#?
### Q. What is the difference between Array and Collections?
### Q. What are generic collections?
### Q. How can you optimize memory usage and performance when working with large data collections in C#?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 6. MULTITHREADING

<br/>

### Q. What is multithreading in C# and why is it important?
### Q. How do you create a new thread in C#?
### Q. What is the difference between a thread and a process?
### Q. How do you synchronize threads in C#?
### Q. What is the `ThreadPool` class and how is it used?
### Q. What are `Task` and `async/await` in C#?
### Q. How do you handle exceptions in multithreaded applications?
### Q. What is a deadlock and how can it be avoided?
### Q. What is the purpose of the `lock` statement in C#?
### Q. What is the difference between Monitor and lock in C#?
### Q. How do you use the `Monitor` class for thread synchronization?
### Q. What is the difference between `Thread.Join` and `Thread.Sleep`?
### Q. How do you implement a producer-consumer scenario in C#?
### Q. What is the `CancellationToken` and how is it used in multithreading?
### Q. How do you use `Concurrent` collections in C#?
### Q. What is the difference between `Parallel.For` and `Task.Run`?
### Q. What are the advantages and disadvantages of multithreading?
### Q. Why does a delegate need to be passed as a parameter to the Thread class constructor?
### Q. How to pass the parameter in Thread?
### Q. Why do we need a ParameterizedThreadStart delegate?
### Q. When to use ParameterizedThreadStart over ThreadStart delegate?
### Q. How to pass data to the thread function in a type-safe manner?
### Q. How to retrieve the data from a thread function?
### Q. What is the difference between Threads and Tasks?
### Q. What is the Significance of Thread.Join and Thread.IsAlive functions in multithreading?
### Q. What happens if shared resources are not protected from concurrent access in a multithreaded program?
### Q. How do protect shared resources from concurrent access?
### Q. Which option is better?
### Q. What are the Interlocked functions?
### Q. What is AutoResetEvent and how it is different from ManualResetEvent?
### Q. What is the Semaphore?
### Q. What is Mutex and how it is different from other Synchronization mechanisms?
### Q. What is the Race condition?
### Q. What is the volatile keyword?
### Q. How can you share data between multiple threads?
### Q. What is synchronization and why it is important?
### Q. Can you count some names of Synchronization primitives?
### Q. What are the four necessary conditions for Deadlock?
### Q. What is LiveLock?
### Q. In the context of C# multithreading, what are the main differences between the ThreadPool and creating your own dedicated Thread instances?
### Q. How can you ensure mutual exclusion while accessing shared resources in C# multithreading without using lock statement or Monitor methods?
### Q. Explain the difference between the Barrier and CountdownEvent synchronization primitives in multithreading. Provide a real-world scenario in which each of these would be useful.
### Q. What would be the potential issues with using Thread.Abort() to terminate a running thread? Explain the implications and suggest alternative methods for gracefully stopping a thread.
### Q. How do you achieve thread synchronization using ReaderWriterLockSlim in C# multithreading? Explain its advantages over traditional ReaderWriterLock.
### Q. How do Tasks in C# differ from traditional Threads? Explain the benefits and scenarios where Tasks would be preferred over directly spawning Threads.
### Q. Discuss the differences between Volatile, Interlocked, and MemoryBarrier methods for using shared variables in multithreading. When should each of these be used?
### Q. In C# multithreading, explain the concept of thread-local and data partitioning and how it can help improve the overall performance of a multi-threaded application.
### Q. How does the Cancellation model in a Task Parallel Library work? Explain how you can use CancellationToken to handle cancellations in TPL.
### Q. How do you combine asynchronous programming with multithreading using C#\'s async/await pattern? Explain how the TaskScheduler class can be used in this context.
### Q. What is parallelism, and how do you control the degree of parallelism for a parallel loop in C# using the Parallel class?
### Q. Describe the concept of lock contention in multithreading and explain its impact on the performance of your application. How can you address and mitigate lock contention issues?
### Q. How do Tasks in C# differ from traditional Threads? Explain the benefits and scenarios where Tasks would be preferred over directly spawning Threads.
### Q. Describe the concept of a race condition in C# multithreading and explain different strategies for preventing race conditions from occurring in your application.
### Q. What is lazy initialization in C# multithreading and how does it affect application start-up performance? 
### Q. How do you achieve thread synchronization using ReaderWriterLockSlim in C# multithreading? Explain its advantages over traditional ReaderWriterLock.
### Q. What are the differences between ManualResetEvent and AutoResetEvent synchronization primitives in C# multithreading?
### Q. Explain the use of SpinLock in C# multithreading and how it differs from a standard lock or Monitor. Describe the potential advantages and limitations of using SpinLocks.
### Q. What is Multithreading with .NET?
### Q. How can you implement multithreading in C#?
### Q. When should multithreading be used and when should it be avoided in C#?
### Q. What is difference between lock and interlock in Multithreading C#?
### Q. How does multithreading improve the performance over a single thread solution?
### Q. How do you handle deadlocks in multi-threaded C# applications?
### Q. What is the difference between `lock` and `Monitor` in C#?
### Q. What is a `thread` in C#?
### Q. What is the purpose of the `ThreadPool` class in C#?
### Q. What is the difference between `Task.Run` and `Task.Factory.StartNew`?
### Q. What is the difference between `Task` and `Thread` in C#?
### Q. What is the purpose of the `lock` statement in C#?
### Q. Explain the concept of threading in .NET.
### Q. What is Thread Pooling in C#?
### Q. What are the different states of a Thread in C#?
### Q. What is the difference between Task and Thread in C#?
### Q. What is the lock statement in C#?
### Q. How are threads different from TPL ?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 7. FILE HANDLING

<br/>

### Q. What is File Handling in C#.Net?
### Q. How do you read a file in C#?
### Q. How do you write to a file in C#?
### Q. What is the difference between `File.ReadAllText` and `File.ReadAllLines` in C#?
### Q. How do you append text to an existing file in C#?
### Q. How do you check if a file exists in C#?
### Q. What is the purpose of the `StreamReader` and `StreamWriter` classes in C#?
### Q. How do you handle exceptions when working with files in C#?
### Q. How do you delete a file in C#?
### Q. What is the difference between `FileStream` and `MemoryStream` in C#?
### Q. How do you copy a file in C#?
### Q. How do you move a file in C#?
### Q. What is the `FileInfo` class and how is it used?
### Q. How do you read and write binary files in C#?
### Q. How do you work with directories in C#?
### Q. How do you get the size of a file in C#?
### Q. What are DLL files, and what are the advantages of using them?
### Q. What is a `MemoryStream` in C#?
### Q. What is the purpose of the `FileStream` class in C#?
### Q. What is the difference between `StreamReader` and `StreamWriter`?
### Q. What is a `BinaryReader` in C#?
### Q. What is the purpose of the `BinaryWriter` class in C#?
### Q. What is the difference between `TextReader` and `TextWriter`?
### Q. What is a `StringReader` in C#?
### Q. What is the purpose of the `StringWriter` class in C#?
### Q. What is the difference between `XmlReader` and `XmlWriter`?
### Q. What is a `JsonReader` in C#?
### Q. What is the purpose of the `JsonWriter` class in C#?
### Q. What is the difference between `DataContractSerializer` and `XmlSerializer`?
### Q. What is a `BinaryFormatter` in C#?
### Q. What is the purpose of the `SoapFormatter` class in C#?
### Q. What is the difference between `BinaryFormatter` and `SoapFormatter`?
### Q. What is Serialization?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 8. REGULAR EXPRESSION

<br/>

### Q. What is a regular expression in C# and what is it used for?
### Q. How do you create a regular expression in C#?
### Q. What is the purpose of the `Regex` class in C#?
### Q. How do you match a pattern in a string using regular expressions in C#?
### Q. How do you replace text in a string using regular expressions in C#?
### Q. What are some common use cases for regular expressions in C#?
### Q. How do you validate an email address using regular expressions in C#?
### Q. What is the difference between `Regex.Match` and `Regex.IsMatch` in C#?
### Q. How do you extract groups from a match using regular expressions in C#?
### Q. How do you handle case sensitivity in regular expressions in C#?
### Q. How to create precompiled Regex object in .Net?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 9. EXCEPTION HANDLING

<br/>

### Q. What is exception handling in C# and why is it important?
### Q. How do you handle exceptions in C#?
### Q. What is the difference between `try`, `catch`, and `finally` blocks in C#?
### Q. How do you create a custom exception in C#?
### Q. What is the purpose of the `throw` keyword in C#?
### Q. How do you rethrow an exception in C#?
### Q. What is the difference between `throw` and `throw ex` in C#?
### Q. How do you handle multiple exceptions in a single `catch` block in C#?
### Q. What is the `Exception` class in C# and what are its key properties?
### Q. How do you log exceptions in C#?
### Q. What is the `InnerException` property in C#?
### Q. How do you use the `using` statement to handle exceptions in C#?
### Q. What is the difference between checked and unchecked exceptions in C#?
### Q. How do you handle exceptions in asynchronous methods in C#?
### Q. What is the `AggregateException` class in C# and when is it used?
### Q. What is difference between Throw Exception and Throw Clause.
### Q. How do you handle exceptions in a method that returns a Task?
### Q. Which class acts as a base class for all exceptions in C#?
### Q. Will the finally block get executed if an exception has not occurred?
### Q. What is the C# syntax to catch any possible exception?
### Q. What is the difference between System.ApplicationException class and System.SystemException class? 
### Q. In try block if we add return statement whether finally block is executed in C#? 
### Q. What is the difference between StackOverflowError and OutOfMemoryError? 
### Q. Name different types errors which can occur during the execution of a program?
### Q. Explain the difference between "finally" and "finalize block"?
### Q. What is the difference between "throw" and "throw ex"
### Q. Can Multiple Catch Blocks executed in C#?
### Q. What is the role of System.Exception class?
### Q. List down the most commonly used types of exceptions in .NET?
### Q. What is the difference between `throw` and `throw new` in C#?
### Q. What are the different ways to handle errors in C#?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 10. EVENTS AND DELEGATES

<br/>

### Q. What are Events?
### Q. What are delegates in C# and why are they used?
### Q. What is the difference between a delegate and an event in C#?
### Q. How do you declare and raise an event in C#?
### Q. What is the purpose of the `EventHandler` delegate in C#?
### Q. How do you subscribe to and unsubscribe from an event in C#?
### Q. What is a multicast delegate in C#?
### Q. How do you use anonymous methods with delegates in C#?
### Q. What are the advantages of using events and delegates in C#?
### Q. How do you use lambda expressions with delegates in C#?
### Q. What is the difference between `Action`, `Func`, and `Predicate` delegates in C#?
### Q. How do you implement custom event accessors in C#?
### Q. What is the `event` keyword in C# and how is it used?
### Q. How do you handle events in a derived class in C#?
### Q. What are the best practices for using events and delegates in C#?
### Q. What is an event in C# and how does it work?
### Q. What is an event in C# and how is it different from a delegate?
### Q. What is a delegate in C# and how is it related to events?
### Q. What are called combinable delegates?
### Q. How to use delegates with Events? 
### Q. What is the difference between Event and Method?
### Q. What is the difference between lambdas and delegates? 
### Q. What is the difference between Func<string,string> and delegate? 

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 11. GARBAGE COLLECTION

<br/>

### Q. What is garbage collection in .NET?
### Q. Explain the role of the garbage collector in .NET.
### Q. How do you manage memory in .NET applications?
### Q. How does garbage collection work in .NET and how can you optimize it?
### Q. How does the garbage collector know when to clean up objects?
### Q. Does the garbage collector clean up primitive types?
### Q. What are Generation 0, Generation 1, and Generation 2 in garbage collection?
### Q. How does the garbage collector behave when a class has a destructor?
### Q. Explain the importance of the garbage collector.
### Q. Can the garbage collector reclaim unmanaged objects?
### Q. Can the garbage collector clean up unmanaged code?
### Q. Can you force garbage collection in .NET?
### Q. Is it a good practice to force garbage collection?
### Q. How do you detect memory leaks in .NET applications?
### Q. What is GC0, GC1, and GC2?
### Q. How does GC behave when we have a destructor?
### Q. What is a `Mutex` in C#?
### Q. What is the purpose of the `Semaphore` class in C#?
### Q. What is the difference between `Semaphore` and `SemaphoreSlim`?
### Q. What is a `deadlock` in C#?
### Q. What is the purpose of the `Interlocked` class in C#?
### Q. What is the difference between `Task` and `ValueTask`?
### Q. What is a `CancellationToken` in C#?
### Q. What is the purpose of the `CancellationTokenSource` class in C#?
### Q. What is the difference between `Task.WhenAll` and `Task.WhenAny`?
### Q. What is a `ConcurrentDictionary` in C#?
### Q. What is the purpose of the `BlockingCollection` class in C#?
### Q. What is the difference between `ConcurrentBag` and `ConcurrentQueue`?
### Q. What is a `MemoryCache` in C#?
### Q. What is the purpose of the `Lazy` class in C#?
### Q. What is the difference between `Lazy` and `Lazy<T>`?
### Q. What is a `WeakReference` in C#?
### Q. What is the purpose of the `GC.Collect` method in C#?
### Q. What is the difference between `GC.Collect` and `GC.WaitForPendingFinalizers`?
### Q. What is a `finalizer` in C#?
### Q. What is the purpose of the `GC.SuppressFinalize` method in C#?
### Q. What is the difference between `GC.KeepAlive` and `GC.ReRegisterForFinalize`?
### Q. What is garbage collection and how does it work in .NET Core?
### Q. How do you optimize garbage collection in .NET Core?
### Q. What is the difference between server and workstation garbage collection?
### Q. How do you use the `GCSettings` class to configure garbage collection?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 12. LAMBDA EXPRESSIONS

<br/>

### Q. What is a lambda expression in C# and why is it used?
### Q. How do you declare a lambda expression in C#?
### Q. What is the difference between a lambda expression and an anonymous method in C#?
### Q. How do you use lambda expressions with LINQ in C#?
### Q. Can you provide an example of a simple lambda expression in C#?
### Q. What are the advantages of using lambda expressions in C#?
### Q. How do you capture variables in a lambda expression in C#?
### Q. What is the difference between statement lambdas and expression lambdas in C#?
### Q. How do you use lambda expressions with delegates in C#?
### Q. What are the limitations of lambda expressions in C#?
### Q. How do you handle exceptions in lambda expressions in C#?
### Q. Can you use async/await with lambda expressions in C#?
### Q. How do you use lambda expressions in event handling in C#?
### Q. What is the syntax for a lambda expression with multiple parameters in C#?
### Q. How do you use lambda expressions with generic types in C#?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 13. Language Integrated Query

<br/>

### Q. What is Expression Tree In C#?

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

### Q. What is LINQ in C# and why is it used?
### Q. What are the main advantages of using LINQ in C#?
### Q. How do you write a basic LINQ query in C#?
### Q. What is the difference between LINQ to Objects, LINQ to SQL, and LINQ to XML?
### Q. Can you provide an example of a LINQ query that filters and sorts data?
### Q. What are the different types of LINQ operators in C#?
### Q. How do you use the `Select` and `Where` operators in LINQ?
### Q. What is the purpose of the `GroupBy` operator in LINQ?
### Q. How do you perform a join operation using LINQ?
### Q. What is the difference between deferred execution and immediate execution in LINQ?
### Q. How do you use the `Aggregate` operator in LINQ?
### Q. What is the `Let` keyword in LINQ and how is it used?
### Q. How do you handle null values in LINQ queries?
### Q. What are anonymous types in LINQ and how are they used?
### Q. How do you use LINQ with collections in C#?
### Q. How do you optimize LINQ queries for performance?
### Q. Explain the differences between Parallel.ForEach and PLINQ (Parallel LINQ)?
### Q. How does LINQ's "Where" method works?
### Q. What are the benefits of a Deferred Execution in LINQ?  
### Q. Can you explain the difference between a query expression and a method chain in LINQ?
### Q. Can you give an example of using LINQ to filter data in a collection?
### Q. How can LINQ be used to perform grouping and aggregation operations on data?
### Q. How does the Single Responsibility Principle (SRP) apply to LINQ code?
### Q.  Can you demonstrate the use of LINQ to implement the Open-Closed Principle (OCP)?
### Q. How does the Liskov Substitution Principle (LSP) apply to LINQ code?
### Q. Can you give an example of using LINQ to implement the Interface Segregation Principle (ISP)?
### Q. Can you explain the Dependency Inversion Principle (DIP) and how it relates to LINQ code?
### Q. Explain the difference between Select and Where?
### Q. What is the difference between `IEnumerable` and `IQueryable`?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 14. MICROSERVICES

<br/>

### Q. What are microservices and why are they used?
### Q. How do you implement microservices in .NET Core?
### Q. What are the main advantages of using microservices architecture?
### Q. How do you handle communication between microservices in .NET Core?
### Q. What is the role of API Gateway in microservices architecture?
### Q. How do you manage data consistency in microservices?
### Q. What are some common challenges when working with microservices?
### Q. How do you deploy microservices in a containerized environment?
### Q. What is the purpose of service discovery in microservices?
### Q. How do you implement logging and monitoring in microservices?
### Q. What is the difference between monolithic and microservices architecture?
### Q. How do you handle security in microservices?
### Q. What is the role of Docker in microservices?
### Q. How do you implement resilience and fault tolerance in microservices?
### Q. What are some best practices for designing microservices?
### Q. Name the key components of Microservices?
### Q. What are the tools commonly used tools for Microservices?
### Q. What are the key principles to follow when designing microservices?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 15. PERFORMANCE AND OPTIMIZATION

<br/>

### Q. How can you improve string concatenation performance in C#?
### Q. How do you work with parallelism and concurrency in C#?
### Q. What are some common performance issues in .NET Core applications?
### Q. How do you measure the performance of a .NET Core application?
### Q. What tools are available for profiling .NET Core applications?
### Q. How do you use the `dotnet-counters` tool to monitor performance?
### Q. What is the `dotnet-trace` tool and how is it used?
### Q. How do you use the `dotnet-dump` tool to collect and analyze memory dumps?
### Q. What is the `dotnet-gcdump` tool and how is it used?
### Q. How do you optimize memory usage in .NET Core applications?
### Q. What are some best practices for managing memory in .NET Core?
### Q. How do you reduce the memory footprint of a .NET Core application?
### Q. How do you optimize CPU usage in .NET Core applications?
### Q. What are some best practices for optimizing CPU-bound operations?
### Q. How do you use asynchronous programming to improve performance in .NET Core?
### Q. What is the `async` and `await` keywords and how are they used?
### Q. How do you use the `Task` class to perform asynchronous operations?
### Q. How do you optimize I/O operations in .NET Core applications?
### Q. What are some best practices for optimizing disk and network I/O?
### Q. How do you use caching to improve performance in .NET Core applications?
### Q. What is the `IMemoryCache` interface and how is it used?
### Q. How do you use distributed caching in .NET Core?
### Q. What is the `IDistributedCache` interface and how is it used?
### Q. How do you use the `ResponseCaching` middleware to cache responses?
### Q. How do you optimize database access in .NET Core applications?
### Q. How do you use connection pooling to improve database performance?
### Q. How do you use the `AsNoTracking` method to optimize query performance?
### Q. What are the secure coding practices (XSS, CSRF, SQL Injection) available in .NET?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 16. DEPLOYMENT

<br/>

### Q. How do you deploy a .NET Core application to Azure?
### Q. What is the `dotnet publish` command and how is it used?
### Q. How do you deploy a .NET Core application to IIS?
### Q. What is the `WebHostBuilder` class and how is it used?
### Q. What is the `Azure App Service` and how is it used?
### Q. How do you use Docker to containerize a .NET Core application?
### Q. How do you create a Dockerfile for a .NET Core application?
### Q. How do you deploy a .NET Core application using Docker?
### Q. What is Kubernetes and how is it used with .NET Core applications?
### Q. How do you use CI/CD pipelines to deploy .NET Core applications?
### Q. What is the `Azure DevOps` service and how is it used for deployment?
### Q. How do you configure a build pipeline in Azure DevOps?
### Q. How do you configure a release pipeline in Azure DevOps?
### Q. How do you use GitHub Actions for deploying .NET Core applications?
### Q. What is the `Octopus Deploy` tool and how is it used?
### Q. What are the best practices for deploying .NET Core applications?
### Q. What are the different types of hosting models available in .NET Core?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 17. .NET Core

<br/>

### Q. What is .NET Core?
### Q. What are the main differences between .NET Framework and .NET Core?
### Q. What is the .NET Standard?
### Q. How do you handle configuration in a .NET Core application?
### Q. What is the .NET Core CLI and how is it used?
### Q. How do you create a new .NET Core project?
### Q. What is the purpose of the Program.cs and Startup.cs files in a .NET Core application?
### Q. How do you identify duplicated code in C#, and what techniques can be used to remove it?
### Q. What is Kestrel?
### Q. What is Dependency Injection in .NET Core?
### Q. How do you handle configuration in .NET Core?
### Q. How do you add services to the dependency injection container?
### Q. What is the difference between `IServiceCollection` and `IServiceProvider`?
### Q. How do you configure logging in .NET Core?
### Q. How do you perform database migrations in EF Core?
### Q. What is the difference between `AddTransient`, `AddScoped`, and `AddSingleton`?
### Q. How do you create a RESTful API in ASP.NET Core?
### Q. How do you read configuration values from `appsettings.json`?
### Q. How do you use middleware in ASP.NET Core?
### Q. How do you create a custom middleware?
### Q. How do you use dependency injection in controllers?
### Q. How do you return JSON from a controller action?
### Q. What is attribute routing?
### Q. How do you create a custom route?
### Q. How do you handle form submissions in ASP.NET Core?
### Q. What is model binding?
### Q. How do you validate models in ASP.NET Core?
### Q. What is the `ModelState` property?
### Q. How do you use data annotations for validation?
### Q. What is the `ViewResult` class?
### Q. How do you return a view from a controller action?
### Q. What is Razor syntax?
### Q. How do you create a Razor view?
### Q. How do you use partial views in ASP.NET Core?
### Q. How do you create a view component?
### Q. How do you create a custom tag helper?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 18. MISCELLANEOUS

<br/>

### Q. What is NuGet?
### Q. What is the difference between ToString() and Convert.ToString()?
### Q. What is the difference between int.Parse() and Convert.ToInt32()?
### Q. What is the use of Code Snippets?
### Q. Write a program to get the range of Byte Datatype?
### Q. What are attributes in C# and how can they be used?
### Q. Why can't you specify the accessibility modifier for methods inside the interface?
### Q. How do you implement a custom IComparer<T> for sorting in C#?
### Q. How are custom controls added to an application in C#?
### Q. What is the use of conditional preprocessor directive in C#?
### Q. What are pointer types in C#?
### Q. What is marshalling and why do we need it?
### Q. Query Geolocation & proxy data in .NET using IP2Location
### Q. How to calculate the code execution time in C#?
### Q. What is the distinction between directcast and ctype?
### Q. Explain how to implement a custom serializer and deserializer for a complex object in C#?
### Q. Explain the differences between Memory<T> and Span<T> in C#. When would you use one over the other?
### Q. What is a DTO?
### Q. What does POCO mean?
### Q. Define Parsing? Explain how to Parse a DateTime String?
### Q. What is IL (Intermediate Language) Code?
### Q. What is the use of JIT (Just in time compiler)?
### Q. Is it possible to view IL code?
### Q. What is the benefit of compiling into IL code?
### Q. What is thimportance of CTS?
### Q. How are initializers executed?
### Q. What is Shadowing?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>

Thanks & Regards,
Pradeep Kumar
Phone: 7760653743