

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

|Feature        |var                |dynamic
|---------------|-------------------|----------------
|Type Checking  |Compile time       |Runtime
|Type Determination |Inferred from initializer  |Determined at runtime
|Type Changes   |Fixed after initialization     |Allowed to change at runtime
|Error Detection   |Compile time| Runtime

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

### Q. What is a `struct` in C#?
### Q. What is the difference between `abstract` and `virtual` methods?
### Q. What is the difference between `out` and `ref` parameters?
### Q. What is the purpose of the `nameof` operator in C#?
### Q. What is the difference between `checked` and `unchecked` contexts?
### Q. What is a `generic` in C#?
### Q. What is a `Tuple` in C#?
### Q. What is a namespace in C#?
### Q. How are primitive and objects stored in memory?
### Q. Explain byval and byref?
### Q. Differentiate between copy byvalue and copy byref?
### Q. What is an immutable string?
### Q. What is the JIT compiler process?
### Q. Explain the characteristics of value-type variables that are supported in the C# programming language.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

### Q. What is a parameter? Explain the new types of parameters introduced in C# 4.0.
### Q. Briefly explain the characteristics of reference-type variables that are supported in the C# programming language.
### Q. What are the different types of literals?
### Q. What is the main difference between sub-procedure and function?
### Q. How to use extension methods?
### Q. What is the difference between string and StringBuilder in C#?
### Q. What is difference between late binding and early binding in C#?
### Q. What is Indexer in C# .Net?
### Q. What are the differences between Object, Var and Dynamic type?
### Q. What is the difference between `IEnumerable` and `IQueryable`?
### Q. What is the difference between managed and unmanaged code?
### Q. What is Expression Trees In C#?
### Q. What is an Object Pool in .Net?
### Q. What are generics in C#.net?
### Q. What is Serialization?
### Q. Can you explain the difference between lazy and eager evaluation in C#?
### Q. Mention the two major categories that distinctly classify the variables of C# programs.
### Q. What is checked block and unchecked block?
### Q. What is the difference between typeOf() and sizeOf()?
### Q. What is widening and Narrowing?
### Q. How to view an Assembly?
### Q. What are MultiLingual Applications?
### Q. What is the use of Codesnippets?
### Q. What are dynamic type variables in C#? 
### Q. Can you describe the process of code compilation in .NET?
### Q. What are property Accessors?
### Q. Can you return multiple values from a function in C#?
### Q. In how many ways you can pass parameters to a method?
### Q. What are Indexers in C#?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>



## # 2. OPERATORS

<br/>

### Q. What is difference between const and readonly in C#?
### Q. How you would use a bitwise operator in C#? 
### Q. Explain the use of the `as` operator in C# and the best way to use it?
### Q. What is the use of Null Coalescing Operator (??) in C#? 
### Q. Can you give an example of how to use the is operator and the as operator with inheritance in C#?
### Q. Difference between "is" and "as" operator in C#.
### Q. What are nullable types in C#?
### Q. What is Type Casting and what are its types in C#?
### Q. What are operators in C# and can you provide examples?
### Q. What is the difference between `==` operator and `.Equals()` method?
### Q. What is the purpose of the `var` keyword in C#?
### Q. What are the differences between `const` and `readonly` keywords?
### Q. How does `checked` and `unchecked` context affect arithmetic operations?
### Q. What is short-circuit evaluation in C#?
### Q. List some different ways for equality check in .Net?
### Q. How to get the sizeof a datatype in C#?
### Q. Asynchronous programming with async, await, Task in C#
### Q. What is difference between static, readonly, and constant in C#
### Q. How to loop through an enum in C#?
### Q. What is NullReferenceException in C#?
### Q. How to set default value to Property in C#
### Q. How to convert int to enum in C#
### Q. What is BigInteger Data Type in C#
### Q. How to convert String to Enum in C#
### Q. How to convert an Object to JSON in C#
### Q. How to convert JSON String to Object in C#
### Q. How to Pass or Access Command-line Arguments in C#?
### Q. How to convert date object to string in C#?
### Q. How to combine two arrays without duplicate values in C#?
### Q. How to convert string to int in C#?
### Q. What is boxing and unboxing?
### Q. What effect does boxing and unboxing have on performance?
### Q. Explain casting, implicit casting and explicit casting ?
### Q. What can happen during explicit casting ?
### Q. What is the difference between `explicit` and `implicit` conversions?
### Q. What is the difference between `==` and `ReferenceEquals` in C#?
### Q. Explain var and dynamic?
### Q. What is the difference between constant and readonly in C#?
### Q. What is the purpose of the `is` and `as` operators in C#?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 3. CLASSES

<br/>

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
### Q. Can you explain the SOLID principle of Single Responsibility and how it relates to abstract classes in C#?
### Q. How can abstract classes be used to implement the Dependency Inversion principle?
### Q. Can you explain the SOLID principle of Liskov Substitution and how it relates to abstract classes in C#?
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
### Q. What is the importance of CTS?
### Q. How are initializers executed?
### Q. What is Shadowing?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

