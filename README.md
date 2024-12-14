# C# Basics

> *Click &#9733; if you like the project. Your contributions are heartily ♡ welcome.*

<br/>

## Table of Contents

* [Fundamentals](#-1-fundamentals)
* [Operators](#-2-operators)
* [Classes](#-3-classes)
* [Encapsulation](#-4-encapsulation)
* [Inheritance](#-5-inheritance)
* [Collections](#-6-collections)
* [Multithreading](#-7-multithreading)
* [File Handling](#-8-file-handling)
* [Regular Expression](#-9-regular-expression)
* [Exception Handling](#-10-exception-handling)
* [Events and Delegates](#-11-events-and-delegates)
* [Garbage Collection](#-12-garbage-collection)
* [Lambda Expressions](#-13-lambda-expressions)
* [LINQ](#-14-linq)
* [Microservices](#-15-microservices)
* [Unit Tests](#-16-unit-tests)
* [Design Patterns](#-17-design-patterns)
* [Azure](#-18-azure)
* [Miscellaneous](#-19-miscellaneous)

<br/>

## # 1. FUNDAMENTALS

<br/>

## Q. What is C#?


C# is an object-oriented programming language created by Microsoft that runs on the .NET Framework. C# has roots from the C family, and the language is close to other popular languages like C++ and Java. C# provides the rapid application development found in Visual Basic with the power of C++.

Some common areas where C# is used include:

* **Desktop Applications:** C# is commonly used for developing Windows desktop applications using frameworks like Windows Presentation Foundation (WPF) and Windows Forms.

* **Web Development:** C# can be used for server-side web development with frameworks like ASP.NET and ASP.NET Core. These frameworks allow developers to build dynamic websites and web applications.

* **Mobile App Development:** C# can be used for developing mobile applications for iOS and Android using frameworks like Xamarin, which allows developers to write code once and deploy it on multiple platforms.

* **Game Development:** C# is a popular choice for game development using game engines like Unity3D and Unreal Engine. Unity3D, in particular, uses C# as its primary scripting language.

* **Enterprise Software:** C# is often used in the development of enterprise-level applications and systems due to its scalability, performance, and integration capabilities.

* **Cloud Services:** C# can be used to develop applications that run on cloud platforms like Microsoft Azure, making it a good choice for cloud-based services and solutions.

* **Machine Learning and AI:** C# can be used for developing machine learning and artificial intelligence applications using libraries like ML.NET.

* **IoT (Internet of Things):** C# can be used in IoT development for creating applications that interact with IoT devices and sensors.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is object-oriented programming?

Object-Oriented Programming (OOP) is a programming paradigm that focuses on organizing code into objects, which are instances of classes. It promotes the idea of encapsulating data (attributes) and behavior (methods) related to a specific entity into a single unit. C# is a language that fully supports OOP concepts.

In C#, classes serve as blueprints for creating objects. Let\'s go through some OOP concepts in C# with examples:

**Class and Object Creation:**

A class is a template that defines the structure and behavior of objects. An object is an instance of a class.

```cs
class Person
{
    public string Name;
    public int Age;

    public void Introduce()
    {
        Console.WriteLine($"Hi, I'm {Name} and I'm {Age} years old.");
    }
}

class Program
{
    static void Main()
    {
        Person person1 = new Person();
        person1.Name = "Alice";
        person1.Age = 30;
        person1.Introduce(); // Output: Hi, I'm Alice and I'm 30 years old.
    }
}
```

**Encapsulation:**

Encapsulation ensures that the internal details of an object are hidden from the outside world. It helps maintain data integrity and allows controlled access to data through methods.

```cs
class BankAccount
{
    private double balance;

    public void Deposit(double amount)
    {
        if (amount > 0)
            balance += amount;
    }

    public void Withdraw(double amount)
    {
        if (amount > 0 && amount <= balance)
            balance -= amount;
    }

    public double GetBalance()
    {
        return balance;
    }
}
```

**Inheritance:**

Inheritance allows you to create a new class based on an existing class, inheriting its attributes and methods. It promotes code reusability and hierarchy.

```cs
class Animal
{
    public string Species;

    public void MakeSound()
    {
        Console.WriteLine("Animal makes a sound.");
    }
}

class Dog : Animal
{
    public void Bark()
    {
        Console.WriteLine("Woof woof!");
    }
}
```

**Polymorphism:**

Polymorphism allows objects of different classes to be treated as objects of a common base class through inheritance. It enables dynamic method binding and flexibility in handling different object types.

```cs
class Shape
{
    public virtual void Draw()
    {
        Console.WriteLine("Drawing a shape.");
    }
}

class Circle : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a circle.");
    }
}

class Square : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a square.");
    }
}
```

These are just a few fundamental concepts of Object-Oriented Programming in C#. They provide a way to structure your code, improve reusability, and create more maintainable software applications.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is an extension method in C# and how is it implemented?

An extension method in C# is a static method that allows you to extend the functionality of an existing type without modifying the type\'s original source code.

The syntax for defining an extension method is to define a static class with a static method that takes the extended type as its first parameter. This first parameter is marked with the `this` keyword to indicate that it is the type being extended.

Here\'s an example of an extension method that adds a "Reverse" method to the string type:

```cs
public static class StringExtensions
{
    public static string Reverse(this string str)
    {
        char[] charArray = str.ToCharArray();
        Array.Reverse(charArray);
        return new string(charArray);
    }
}
```

In this example, the "Reverse" method takes a string as its first parameter (marked with the `this` keyword) and returns a reversed version of the string.

To use this extension method, you simply need to include the namespace that contains the extension method and call it on an instance of the extended type, as if it were a member method of that type:

```cs
using ExtensionMethods;

string original = "Hello, world!";
string reversed = original.Reverse();

Console.WriteLine(reversed); // Outputs: "!dlrow ,olleH"

```

In this example, the "Reverse" method is called on a string instance ("original") and returns a new string instance with the reversed content.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a generic type in C# and why is it used?

In C#, a generic type is a type that is defined with one or more placeholders for its type parameters, which can be replaced with specific types at the time the type is instantiated.

For example, consider a simple generic class called `List<T>`. The `T` in this class definition is a type parameter, which can be replaced with any specific type when an instance of the `List<T>` class is created. For example, you can create a list of integers by instantiating `List<int>` or a list of strings by instantiating `List<string>`.

Generics are used in C# to create more flexible and reusable code. By defining a generic class or method, you can write code that works with any type that meets certain requirements, rather than writing separate code for each type. This can help reduce code duplication and improve maintainability.

Generics are also used to provide type safety at compile time. By specifying the type parameter when instantiating a generic class, the C# compiler can ensure that the code only uses that specific type, which helps prevent runtime errors that could occur with type mismatches.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is an anonymous method in C# and how is it used?

In C#, an anonymous method is a method that does not have a name and is defined inline within the code of another method or expression. It can be used in place of a named method anywhere a delegate is expected. A delegate is an object that can refer to a method and can be passed as a parameter or returned as a value.

Anonymous methods can be defined using the delegate keyword, followed by the argument list and body of the method.

**Example:**

```cs
delegate (int x, int y) {
    return x + y;
};
```

This anonymous method takes two integer arguments and returns their sum. It can be used as follows:

```cs
int result = myDelegate(3, 4); // result = 7
```

Where myDelegate is a delegate variable that has been assigned to this anonymous method.

Alternatively, C# also introduced lambda expressions, which are a shorter and more readable syntax for creating anonymous methods. For example, the anonymous method above can be expressed as a lambda expression like this:

```cs
(x, y) => x + y
```

Anonymous methods are commonly used in event handling and LINQ queries, among other places, where they allow for concise and flexible code.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you explain the use of the "this" keyword in C#?

In C#, the `this` keyword is a reference to the current instance of a class or struct. It is commonly used to:

Distinguish between instance variables and local variables with the same name: When a local variable has the same name as an instance variable, using `this` allows you to refer to the instance variable.

**Example:**

```cs
class MyClass {
   int num;

   public void SetNum(int num) {
      this.num = num; // use "this" to refer to the instance variable
   }
}
```

Pass the current object as an argument to another method or constructor: You can use `this` to pass the current object as an argument to another method or constructor.

**Example:**

```cs
class MyClass {
   int num;

   public MyClass(int num) {
      this.num = num; // pass "this" as an argument to set the instance variable
   }

   public void DoSomething() {
      OtherClass.Method(this); // pass "this" as an argument to another method
   }
}
```

Return the current object from a method: You can use `this` to return the current object from a method. This is useful for chaining method calls.

**Example:**

```cs
class MyClass {
   public MyClass DoSomething() {
      // do something
      return this; // return the current object
   }

   public MyClass DoSomethingElse() {
      // do something else
      return this; // return the current object
   }
}
```

Overall, the `this` keyword is used to refer to the current object and is useful for disambiguating variables, passing the current object as an argument, and chaining method calls.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between a value type and a reference type in C#?

In C#, data types are categorized into two main categories: value types and reference types. The distinction between these two types is crucial because it affects how data is stored and manipulated in memory.

**Value Types:**

Value types directly store the value itself, and they are usually stored in the stack memory. When you assign a value type to a new variable or pass it as a parameter to a method, a copy of the value is made. As a result, changes made to the copied value do not affect the original value.

Examples of value types include:

**Numeric Types:**

```cs
int x = 5;
int y = x; // y gets a copy of the value in x
y = 10;    // Changing y does not affect x
```

**Enumerations:**

```cs
enum Color { Red, Green, Blue }
Color color1 = Color.Red;
Color color2 = color1; // color2 gets a copy of color1
color2 = Color.Green;  // Changing color2 does not affect color1
```

**Structures:**

```cs
struct Point { public int X; public int Y; }
Point p1 = new Point { X = 2, Y = 3 };
Point p2 = p1;         // p2 gets a copy of p1
p2.X = 5;              // Changing p2 does not affect p1
```

**Reference Types:**

Reference types store a reference to the memory location where the data is actually stored, usually on the heap. When you assign a reference type to a new variable or pass it as a parameter to a method, you\'re passing a reference to the same memory location. This means changes made to the data are reflected wherever that data is referenced.

Examples of reference types include:

**Classes:**

```cs
class Person { public string Name; }
Person person1 = new Person { Name = "Alice" };
Person person2 = person1;  // person2 references the same object as person1
person2.Name = "Bob";      // Changing person2 also changes person1
```

**Arrays:**

```cs
int[] arr1 = new int[] { 1, 2, 3 };
int[] arr2 = arr1;      // arr2 references the same array as arr1
arr2[0] = 5;            // Changing arr2 also changes arr1
```

**Strings:**

```cs
string str1 = "Hello";
string str2 = str1;     // str2 references the same string as str1
str2 = "Hi";            // Changing str2 does not affect str1
```

In summary, the primary difference between value types and reference types in C# lies in how they store and share data. Value types store the actual value, while reference types store a reference to the data\'s memory location. This difference has implications for memory management, copying behavior, and how changes to data are propagated.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a constructor in C# and what are its different types?

In C#, a constructor is a special method that is called when an object of a class is created. The purpose of a constructor is to initialize the state of an object with some initial values.

C# supports two types of constructors:

**Instance Constructors:**

An instance constructor is used to initialize the instance variables of a class. It is called when an object of the class is created using the new keyword. An instance constructor has the same name as the class and does not have a return type.
Here is an example of an instance constructor:

```cs
public class MyClass {
    public int x;
    public int y;

    public MyClass(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

```

**Static Constructors:**

A static constructor is used to initialize the static variables of a class. It is called only once, when the class is first used. A static constructor has the same name as the class and does not have any parameters.
Here is an example of a static constructor:

```cs
public class MyClass {
    public static int x;
    public static int y;

    static MyClass() {
        x = 10;
        y = 20;
    }
}
```

Note that a class can have multiple constructors, but they must have different signatures (i.e., different parameter lists). This is known as constructor overloading.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is an interface in C# and what is its purpose?

In C#, an interface is a programming construct that defines a contract between two parts of a program: the interface and the implementing class. An interface defines a set of methods, properties, and events that the implementing class must implement, but it does not provide any implementation itself.

The purpose of an interface is to provide a way for classes to communicate with each other without needing to know about the specific implementation of the other class. By using interfaces, you can define a set of methods and properties that a class must implement in order to satisfy the contract of the interface. This makes it easier to write code that can work with a variety of different classes that share a common set of behaviors.

Interfaces are often used in object-oriented programming to achieve polymorphism, which is the ability of objects of different types to be treated as if they are of the same type. By defining a common interface, you can write code that can work with any class that implements that interface, without needing to know the specific implementation details of the class. This can make your code more modular, easier to maintain, and more flexible.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between a method and a function in C#?

In C#, both methods and functions are terms used to describe executable code blocks that can be called to perform specific tasks. However, there\'s a subtle distinction between the two:

**Method:**

A method is a code block associated with a class or struct. It defines a set of instructions that can be executed on an instance of the class or struct, or it can be a static method that is associated with the class itself rather than instances of the class. Methods are used to perform actions, manipulate data, or provide functionality related to the class.

Here\'s an example of a method within a class:

```cs
public class MathOperations
{
    public int Add(int a, int b)
    {
        return a + b;
    }
}

class Program
{
    static void Main(string[] args)
    {
        MathOperations math = new MathOperations();
        int result = math.Add(5, 3); // Calling the method
        Console.WriteLine(result);   // Output: 8
    }
}
```

In this example, Add is a method defined within the MathOperations class. It takes two integers as parameters and returns their sum.

**Function:**

In C#, the term "function" is commonly used interchangeably with "method." However, in a broader context, a function is a self-contained block of code that performs a specific task and can return a value. This term is often used in languages that support both procedural programming and object-oriented programming. Since C# is primarily an object-oriented language, the concept of "function" is usually referred to as "method."

So, while the distinction between methods and functions can be blurred, especially in C#, it\'s more accurate to use "method" when discussing code blocks associated with classes and structs in C#.

#### Q. Why can you have only one static constructor?
#### Q. How are static constructors executed in Parent child?
#### Q. When does static constructor fires?
#### Q. When the static constructor will be called?
#### Q. Can we declare Public access modifier for static constructor?
#### Q. If we declare Main() and static constructor in the same class which one will be called first?
#### Q. What is the use of static constructors?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 2. OPERATORS

<br/>

## Q. How you would use a bitwise operator in C#?

Bitwise operators manipulate individual bits within integral data types like integers (int), longs (long), bytes (byte), etc. They are primarily used in scenarios where you need to perform low-level bit manipulation or optimize storage.

**Example:**

```cs
using System;

class Program
{
    static void Main()
    {
        int num1 = 10;  // Binary: 1010
        int num2 = 6;   // Binary: 0110

        // Bitwise AND
        int resultAnd = num1 & num2;  // Binary: 0010 (2 in decimal)
        Console.WriteLine("Bitwise AND result: " + resultAnd);

        // Bitwise OR
        int resultOr = num1 | num2;   // Binary: 1110 (14 in decimal)
        Console.WriteLine("Bitwise OR result: " + resultOr);

        // Bitwise XOR
        int resultXor = num1 ^ num2;  // Binary: 1100 (12 in decimal)
        Console.WriteLine("Bitwise XOR result: " + resultXor);

        // Bitwise NOT (Unary)
        int resultNot = ~num1;        // Binary: 0101 (in two\'s complement)
        Console.WriteLine("Bitwise NOT result: " + resultNot);

        // Bitwise Left Shift
        int resultLeftShift = num1 << 1;  // Binary: 10100 (20 in decimal)
        Console.WriteLine("Bitwise Left Shift result: " + resultLeftShift);

        // Bitwise Right Shift
        int resultRightShift = num1 >> 1; // Binary: 0101 (5 in decimal)
        Console.WriteLine("Bitwise Right Shift result: " + resultRightShift);
    }
}
```

In this example:

- Bitwise AND (`&`): Sets each bit to 1 if both corresponding bits are 1.
- Bitwise OR (`|`): Sets each bit to 1 if at least one corresponding bit is 1.
- Bitwise XOR (`^`): Sets each bit to 1 if only one corresponding bit is 1.
- Bitwise NOT (`~`): Inverts all the bits. Note that in C#, the result is affected by two\'s complement representation.
- Bitwise Left Shift (`<<`): Shifts all bits to the left by a specified number of positions, filling the shifted bits with zeros.
- Bitwise Right Shift (`>>`): Shifts all bits to the right by a specified number of positions, filling the shifted bits differently depending on the sign of the number being shifted.

Instances where bitwise operators make the most sense include:

- Manipulating individual bits within a bit field.
- Implementing efficient algorithms for data compression or encryption.
- Working with hardware I/O ports.
- Optimizing memory usage and performance in low-level programming.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain the use of the `as` operator in C# and the best way to use it?

In C#, the `as` operator is used for casting or converting a variable to a different type, but with a safer approach than some other casting operators. The `as` operator is particularly useful when you want to perform a cast, but you\'re not sure if the cast is valid. If the cast is not valid, the `as` operator returns `null` instead of throwing an exception.

Here\'s the basic syntax of the `as` operator:

```cs
result = expression as type;
```

Here, `expression` is the variable or expression you want to cast, and `type` is the type to which you want to cast it. If the cast is successful, `result` will hold the casted value; otherwise, it will be `null`.

Here\'s an example to illustrate its usage:

```cs
object myObject = "Hello, World!";

// Attempt to cast using the as operator
string myString = myObject as string;

if (myString != null)
{
    Console.WriteLine("Casting successful: " + myString);
}
else
{
    Console.WriteLine("Casting failed");
}
```

In this example, `myObject` is an `object` that contains a string. We use the `as` operator to attempt to cast it to a `string`. If the cast is successful, `myString` will hold the string value, and the program will print "Casting successful: Hello, World!". If the cast fails (for example, if `myObject` doesn't actually contain a string), `myString` will be `null`, and the program will print "Casting failed".

The `as` operator is especially useful when dealing with types that might not be compatible, such as when working with objects of different types from a collection or when deserializing data of unknown structure. It helps prevent runtime exceptions by providing a way to check whether the cast was successful before using the result.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the use of Null Coalescing Operator (??) in C#?

In C#, the `??` operator is called the null-coalescing operator. It is used for handling null values in expressions and provides a concise way to return a default value when a nullable type is null.

**syntax:**

```cs
result = expression1 ?? expression2;
```

Here is how it works:

- If `expression1` is not null, the result is the value of `expression1`.
- If `expression1` is null, the result is the value of `expression2`.

It is a shorthand way of expressing a common pattern of checking for null and providing a default value if the nullable expression is null.

**Example:**

```cs
// Example 1: Using ?? with nullable types
int? nullableValue = GetNullableValue(); // some method returning a nullable int

// If nullableValue is not null, result will be nullableValue; otherwise, it will be 10.
int result = nullableValue ?? 10;

// Example 2: Using ?? with reference types
string nullableString = GetString(); // some method returning a string or null

// If nullableString is not null, result will be nullableString; otherwise, it will be "Default".
string resultString = nullableString ?? "Default";
```

In the first example, if `nullableValue` is not null, then `result` will take the value of `nullableValue`. If `nullableValue` is null, then `result` will be set to 10.

In the second example, if `nullableString` is not null, `resultString` will take the value of `nullableString`. If `nullableString` is null, then `resultString` will be set to the string "Default".

This operator is useful for simplifying code and making it more readable, especially when dealing with nullable types or situations where you want to provide default values in case of null.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you give an example of how to use the is operator and the as operator with inheritance in C#?

Let\'s say we have a class hierarchy as follows:

```cs
class Animal
{
    public virtual void MakeSound()
    {
        Console.WriteLine("Animal sound");
    }
}

class Cat : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Meow");
    }
}

class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Woof");
    }
}

```

Now, we can use the is operator to check if an object is an instance of a particular class or one of its derived classes.

**Example:**

```cs
Animal animal = new Cat();

if (animal is Cat)
{
    Cat cat = (Cat)animal;
    cat.Purr();
}

```

In the above code, we create an instance of Cat and assign it to an Animal variable. We then use the is operator to check if the object referred to by animal is a Cat. If it is, we cast it to a Cat and call the Purr method (which we assume is defined in the Cat class).

We can also use the as operator to perform a safe cast to a derived class. If the object is not an instance of the derived class, the result will be null.

**Example:**

```cs
Animal animal = new Dog();

Cat cat = animal as Cat;

if (cat != null)
{
    cat.Purr();
}
else
{
    Console.WriteLine("Not a cat");
}

```

In the above code, we create an instance of Dog and assign it to an Animal variable. We then use the as operator to cast it to a Cat. Since the object is not a Cat, the result of the cast will be null, so we check for that and print a message if it is null. If the cast had succeeded, we could have called the Purr method on the cat variable.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>


## Q. What is the difference between Equality Operator (==) and Equals() Method in C#?

Both the == Operator and the Equals() method are used to compare two value type data items or reference type data items. The Equality Operator (==) is the comparison operator and the Equals() method compares the contents of a string. The == Operator compares the reference identity while the Equals() method compares only contents. Let\'s see with some examples.
In this example we assigned a string variable to another variable. A string is a reference type and in the following example, a string variable is assigned to another string variable so they are referring to the same identity in the heap and both have the same content so you get True output for both the == Operator and the Equals() method.

```cs
using System;

namespace ComparisionExample {

class Program {

  static void Main(string[] args) {
 
    string name = "Kansiris";
    string myName = name;
   
    Console.WriteLine("== operator result is {0}", name == myName);
    Console.WriteLine("Equals method result is {0}", name.Equals(myName));
    Console.ReadKey();
  }
 }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Difference between "is" and "as" operator in C#.

"is" operator-

In the C# language, we use the "is" operator to check the object type. If the two objects are of the same type, it returns true and false if not.

**Example:**

```cs

class Speaker {
public string Name { get; set; } }
class Author {
public string Name { get; set; } }
```

Now, let\'s try to check the preceding types as:

• var speaker = new Speaker { Name="Gaurav Kumar Arora"};
• We declared an object of Speaker as in the following:
• var isTrue = speaker is Speaker;
• In the preceding, we are just checking the matching type. Yes, our speaker is an object of Speaker type.
• Console.WriteLine("speaker is of Speaker type:{0}", isTrue);
   
So, the results as true.

But, here we get false:

* var author = new Author { Name = "Gaurav Kumar Arora" };
* var isTrue = speaker is Author;
* Console.WriteLine("speaker is of Author type:{0}", isTrue);
   
Because our speaker is not an object of Author type.

"as" operator-

The "as" operator behaves similar to the "is" operator. The only difference is it returns the object if both are compatible to that
type else it returns null.

**Example:**

```cs
public static string GetAuthorName(dynamic obj)
{
    Author authorObj = obj as Author;
return (authorObj != null) ? authorObj.Name : string.Empty;  
}
```

We have a method that accepts dynamic objects and returns the object name property if the object is of the Author type.

Here, we declared two objects:

```cs
var speaker = new Speaker { Name="Gaurav Kumar Arora"};
var author = new Author { Name = "Gaurav Kumar Arora" };
```

The following returns the "Name" property:

```cs
var authorName = GetAuthorName(author);
Console.WriteLine("Author name is:{0}", authorName);
```

It returns an empty string:

```cs
authorName = GetAuthorName(speaker);
Console.WriteLine("Author name is:{0}", authorName);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 5. What are _nullable types_ in _C#_?

**Nullable types** are a special data type in C# that can represent both a regular value and `null`. It is a beneficial feature for representing data that might be absent or unknown.

Key Considerations for Using Nullable Types

- **Memory Consumption**: Data types with explicit sizes, such as `int` or `bool`, need more space in memory when converted to nullable types.

- **Performance**: Nullable types might result in fewer optimizations, particularly with certain operations.

- **Clarity**: They offer transparency about the possible absence of a value.

The "null-conditional" operators

In C#, there are dedicated **"null-conditional" operators** caters to appropriately performing actions for nullable types, ensuring that there is no NullReferenceException. Among these are `?.`, `??`, `??=`, and `?.[]`.

Syntax Requirements for Nullable Types

- Assigning a value: `int? nullableInt = 10;`
 
- Assigning `null`: `nullableInt = null;`

- Performing Operations: Before utilizing the value, ensure it\'s not null.

```cs
if (nullableInt.HasValue) {
    int result = nullableInt.Value;
}
```

Common Application Scenarios

- **Database Interactions**: Adaptability to designate if a field is not set.

- **API Requests**: Effective communication to specify if the endpoint did not return a value as anticipated.

- **User Inputs**: Allowing for the perception of a lack of explicit user input.

- **Workflow Dependencies**: Identifying components that necessitate additional data to proceed.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 8. What is _Type Casting_ and what are its types in _C#_?

**Type casting**, in the context of C#, refers to the explicit and implicit conversion of data types. While explicit casting might lead to data loss, implicit casting is carried out by the C# compiler, ensuring safe conversion.

Implicit Casting

This form of casting is **automatic** and occurs when there is **no risk of data loss**. For instance, an `int` can be automatically cast to a `long` as there is no possibility of truncation.

Explicit Casting

This form of casting must be done manually and is necessary when there is a risk of data loss. For example, when converting a `double` to an `int`, data loss due to truncation could occur. C# requires you to explicitly indicate such conversion, and if the conversion is not possible, it throws a `System.InvalidCastException`.

Using `as` Operand

If you are sure that an object can be cast to a specific type, use the `as` operand for optimization. If the conversion is possible, the operand returns an object of the specified type; otherwise, it returns `null`.

The `is` Operator for Type Checking

With the `is` operator, you can test whether an object is compatible with a specific type. This tool is very useful to prevent `InvalidCastException` errors.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 9. What are _operators_ in _C#_ and can you provide examples?

**Operators** in C# are symbols or keywords responsible for specific program actions, such as basic arithmetic, logical evaluations, or assignment.

Basic Operators

**Arithmetic Operators:**

- **Addition**: `+`
- **Subtraction**: `-`
- **Multiplication**: `*`
- **Division**: `/`
- **Modulus**: `%` (remainder of division)

```cs
int result = 10 % 3;  // Output: 1 (the remainder of 10 divided by 3)
```

**Comparison Operators:**

- **Equal to**: `==`
- **Not equal to**: `!=`
- **Greater than**: `>`
- **Less than**: `<`
- **Greater than or equal to**: `>=`
- **Less than or equal to**: `<=`

```cs
  bool isGreater = 5 > 3;  // Output: true
```

**Conditional Operators:**

- **AND (both conditions are true)**: `&&`
- **OR (at least one condition is true)**: `||`
 
```cs
  bool conditionMet = (5 > 3) && (7 < 10);  // Output: true, both conditions are true
```

- **Ternary**: `?` and `:`
```cs
    int max = (5 > 3) ? 5 : 3;  // Output: 5 (if true, takes the first value, if false, takes the second value)
```

- **Null Coalescing**: `??`
```cs
    string name = incomingName ?? "Default Name";  // Output: The value in incomingName, or "Default Name" if incomingName is null
```

**Bitwise Operators:**

These operators perform actions at the bit level.

- **AND**: `&`
- **OR**: `|`
- **XOR**: `^` (Exclusive OR)
- **NOT**: `~` (Unary complement)
- **Shift left**: `<<`
- **Shift right**: `>>`

```cs
  int bitwiseAndResult = 5 & 3;  // Output: 1 (bitwise AND of 5 and 3 is 1)
```

**Assignment Operators

- **Simple assignment**: `=`
- **Add then assign**: `+=`
- **Subtract then assign**: `-=`
- **Multiply then assign**: `*=`
- **Divide then assign**: `/=`
- **Modulus then assign**: `%=`
- **AND then assign**: `&=`
- **OR then assign**: `|=`

```cs
  int num = 5;
  num += 3;  // num is now 8
```
<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## 10. What is the difference between `==` _operator_ and `.Equals()` _method_?

The `==` operator in C# is used to compare two objects for reference equality, meaning it checks whether the two objects are the same instance in memory. On the other hand, the `.Equals()` method is used to compare the actual values of the two objects, based on how the method is implemented for a particular class.

For value types like `int`, `bool`, `double`, and `structs`, the `==` operator compares the values, whereas for reference types like `string` and custom classes, it compares the references.

It\'s important to note that the behavior of the `==` operator can be overridden for reference types by overloading the operator, allowing it to compare values instead of references.

The `.Equals()` method can also be overridden in classes to provide custom equality comparisons. By default, it behaves like the `==` operator for reference types but can be customized to compare values instead.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the `var` _keyword_ in _C#_?

Introduced in C# 3.0 along with the Language-Integrated Query (LINQ) features, the `var` keyword primarily serves to **reduce redundancy**.

Key Benefits

- **Code Conciseness**: It streamlines code by inferring types, especially with complex types or generic data structures.

- **Dynamic and Anonymous Types Support**: It\'s useful when using dynamic types or initializing objects with an anonymous type; a feature useful in LINQ queries.

Avoid Misusing `var`

It\'s important to use `var` with caution to prevent these issues:

- **Type Clarity**: Incorporating explicit type declarations can enhance code clarity, especially for beginners or when working with inherited code.

- **Code Readability for Method Chaining**: Avoid using `var` excessively when chaining methods to maintain readability.

Best Practices and Common Use-Cases

- **Initialization**: Use `var` while initializing; it helps adapt to object type changes, reduces redundancy, and aligns with the DRY (Don't Repeat Yourself) principle.

- **For-Each Loops with Collections**: It\'s standard practice to use `var` in for-each loops when iterating through collections**.

- **Complex or Generic Types**: `var` brings clarity and brevity when working with such types.

**Example:**

```cs
var user = GetUser();
var customer = LookUpCustomer(user); // What\'s the type of customer?

// This would be better for clarity:
Customer customer = LookUpCustomer(user);
```

This code shows the potential confusion that can stem from not explicitly declaring types. In this example, using `var` might not communicate the "Customer" type as clearly or immediately as an explicit type declaration.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the differences between `const` and `readonly` _keywords_?

**C#** supports two mechanisms for creating **immutable** fields: **`const`** and **`readonly`**.

Key Distinctions

- **Initialization**: `const` fields are initialized when declared, while `readonly` fields can be initialized at the point of declaration or in the class constructor.

- **Visibility**: `readonly` fields allow for differing values within different class instances, whereas `const` ensures the same value across all instances.

- **Type Compatibility**: `const` fields are limited to primitive data types, `String`, and `null`. `readonly` can be utilized with any data type.

- **Compile-Time/Run-Time**: `const` fields are evaluated at compile time. On the contrary, `readonly` fields are evaluated at run-time, fittingly appealing to scenarios that necessitate a run-time reference or state.

**Example:**

```cs
public class Example {
    private readonly int readOnlyField;
    public const int constField = 5;

    public Example(int value) {
        readOnlyField = value;
    }

    public void AssignValueToReadOnlyField(int value) {
        readOnlyField = value;  // Will cause a compilation error
    }
}
```

Note that the `readOnlyField` is assigned its value either in the constructor or at declaration. Once it has a value, that value is unalterable. This invariance ensures its immutability. Likewise, `constField` is initialized with a value at the time of declaration and is unalterable for the duration of the program.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does `checked` and `unchecked` _context_ affect arithmetic operations?

In C#, the enablers `checked` and `unchecked` ensure more predictable behavior when using specific arithmetic operations. By default, C# employs overflow checking, but you can toggle to overflow wrapping using appropriate blocks.

Context Control Keywords

- **checked**: Forces operations to produce exceptions for overflow.
- **unchecked**: Causes operations to wrap on overflow.

When to Use Each Context

- **checked**: Ideal when you require accurate numeric results to guarantee that potential overflows are detected and handled.
 
- **unchecked**: Primarily used to enhance performance in scenarios where the logic or the expected input range ensures that overflows are less probable or inconsequential.

C# Code Example

Here is the C# code:

```cs
using System;

public class Program
{
    public static void Main()
    {
        int a = int.MaxValue;
        int b = 1;
       
        Console.WriteLine("Using checked context:");
        try {
            checked {
                int c = a + b;
                Console.WriteLine($"Sum: {c}");
            }
        }
        catch (OverflowException) {
            Console.WriteLine("Overflow detected!");
        }
       
        Console.WriteLine("\nUsing unchecked context:");
        unchecked {
            int d = a + b;
            Console.WriteLine($"Sum (unlike C#): {d}");
        }
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

#### Q. What is short-circuit evaluation in C#?
#### Q. List some different ways for equality check in .Net?
#### Q. How to get the sizeof a datatype in C#?
#### Q. What is difference between == and Equals() Method in C#
#### Q. Asynchronous programming with async, await, Task in C#
#### Q. What is difference between static, readonly, and constant in C#
#### Q. How to loop through an enum in C#?
#### Q. What is NullReferenceException in C#?
#### Q. How to set default value to Property in C#
#### Q. How to convert int to enum in C#
#### Q. What is BigInteger Data Type in C#
#### Q. How to convert String to Enum in C#
#### Q. How to convert an Object to JSON in C#
#### Q. How to convert JSON String to Object in C#
#### Q. How to Pass or Access Command-line Arguments in C#?
#### Q. How to convert date object to string in C#?
#### Q. How to combine two arrays without duplicate values in C#?
#### Q. How to convert string to int in C#?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 3. CLASSES

<br/>

## Q. Can you explain the difference between a static and an instance method in C#?

In C#, a method is a block of code that performs a specific task. There are two types of methods in C#: static and instance methods.

**Static Methods:**

A static method is a method that is associated with the class rather than an instance of the class. It can be called without creating an instance of the class. In other words, it\'s a method that belongs to the type itself and not to an instance of the type.

Here is an example of a static method:

```cs
public static int Add(int num1, int num2)
{
    return num1 + num2;
}
```

In this example, the Add method is a static method. It can be called using the class name, like this:

```cs
int sum = MyClass.Add(5, 10);
```

**Instance Methods:**

An instance method is a method that is associated with an instance of a class. It can only be called on an instance of the class, after an object of the class has been created.

Here is an example of an instance method:

```cs
public void DisplayMessage(string message)
{
    Console.WriteLine(message);
}
```

In this example, the DisplayMessage method is an instance method. It can only be called on an instance of the class, like this:

```cs
MyClass obj = new MyClass();
obj.DisplayMessage("Hello, world!");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is an abstract class in C# and when is it used?

In C#, an abstract class is a class that cannot be instantiated directly and must be inherited by a subclass. It is a way to provide a common interface and shared implementation for a group of related classes, while also allowing for individual implementation in each subclass.

An abstract class is defined using the abstract keyword before the class keyword. It can have abstract methods, which are declared without implementation, as well as non-abstract methods with implementation. Subclasses that inherit from the abstract class must implement all abstract methods or else they will also be abstract.

Here is an example of an abstract class in C#:

```cs
public abstract class Shape
{
    public abstract double GetArea();
    public abstract double GetPerimeter();
}

public class Rectangle : Shape
{
    private double length;
    private double width;
   
    public Rectangle(double length, double width)
    {
        this.length = length;
        this.width = width;
    }
   
    public override double GetArea()
    {
        return length * width;
    }
   
    public override double GetPerimeter()
    {
        return 2 * (length + width);
    }
}
```

In this example, Shape is an abstract class that defines the common interface for shapes, while Rectangle is a concrete subclass that provides its own implementation for the abstract methods. By defining Shape as an abstract class, we can ensure that any subclass of Shape must implement the GetArea and GetPerimeter methods.

Abstract classes are useful when you want to define a common interface and shared implementation for a group of related classes, but you also want to allow for individual implementation in each subclass. They are commonly used in frameworks and APIs to define a base class that provides common functionality for a set of related classes.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between an abstract class and an interface?

In C#, both abstract classes and interfaces are types that enable polymorphism, allowing objects of different classes to be treated as objects of a common super class. However, they serve different purposes and have different rules:

**Abstract Class:**

* Can contain implementation of methods, properties, fields, or events.
* Can have access modifiers (public, protected, etc.).
* A class can inherit from only one abstract class (single inheritance).
* Can contain constructors.
* Used when different implementations of objects have common methods or properties that can share a common implementation.

**Interface:**

* Cannot contain implementations, only declarations of methods, properties, events, or indexers.
* Members of an interface are implicitly public.
* A class or struct can implement multiple interfaces (multiple inheritance).
* Cannot contain fields or constructors.
* Used to define a contract for classes without imposing inheritance hierarchies.

**Example:**

```cs
public abstract class Animal
{
    public abstract void Eat();
    public void Sleep()
    {
        Console.WriteLine("Sleeping");
    }
}

public interface IMovable
{
    void Move();
}

public class Dog : Animal, IMovable
{
    public override void Eat()
    {
        Console.WriteLine("Dog is eating");
    }

    public void Move()
    {
        Console.WriteLine("Dog is running");
    }
}

class Program
{
    static void Main(string[] args)
    {
        Dog myDog = new Dog();
        myDog.Eat();
        myDog.Sleep();
        myDog.Move();
    }
}
```

In this example, Animal is an abstract class that provides a default implementation of the Sleep method and an abstract Eat method that must be overridden. IMovable is an interface that defines a contract with a Move method that must be implemented. Dog inherits from Animal and implements IMovable, thereby fulfilling both the contract defined by the interface and extending the functionality provided by the abstract class.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain the use of reflection in C#?

Reflection in C# is a powerful feature that allows you to inspect and interact with the metadata of types (classes, interfaces, etc.) at runtime. It provides a way to examine and manipulate the structure, behavior, and attributes of types, including accessing fields, properties, methods, and events dynamically.

Common Use Cases of Reflection

**1. Dynamic Loading and Instantiation:**

Reflection is often used when you need to dynamically load and instantiate types at runtime. For example, you might have a configuration file specifying a class name, and you want to create an instance of that class without knowing it at compile time.

```cs
string typeName = "Namespace.ClassName";
Type type = Type.GetType(typeName);
object instance = Activator.CreateInstance(type);
```

**2. Accessing Members Dynamically:**

Reflection allows you to access and invoke members (fields, properties, methods) of a type dynamically. This is useful when dealing with objects of unknown types.

```cs
Type type = typeof(MyClass);
PropertyInfo property = type.GetProperty("MyProperty");
object value = property.GetValue(myObject);
```

**3. Attribute Inspection:**

Reflection is commonly used for inspecting custom attributes applied to types or members. This can be useful in scenarios like custom serialization or validation.

```cs
Type type = typeof(MyClass);
var attributes = type.GetCustomAttributes(typeof(MyCustomAttribute), true);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. When to use reflection and when not in C#?

**1. Appropriate Times to Use Reflection:**

**When Dealing with Unknown Types:**

Use reflection when you need to work with types that are not known at compile time, such as loading plugins or implementing generic frameworks.

**Metadata Inspection:**

Use reflection when you need to inspect metadata, like attributes, or when building tools that analyze code.

**Serialization and Deserialization:**

Reflection is often used in serialization frameworks to dynamically inspect and manipulate object structures.

**2. Inappropriate Times to Use Reflection**:

**Performance-Critical Code:**

Reflection can be slower compared to direct, compile-time code. Avoid using reflection in performance-critical sections of your application.

**Security-Sensitive Scenarios:**

Reflection can be a security risk, especially if it\'s used to access or modify private members of a type. It\'s important to validate and secure the use of reflection, especially in scenarios where security is crucial.

**Code Maintainability:**

Excessive use of reflection can make code less maintainable, as it may lead to hard-to-read and error-prone code. Favor strongly-typed code whenever possible.

**Example:**

```cs
// Inappropriate use: Using reflection to access private members
Type type = typeof(MyClass);
FieldInfo privateField = type.GetField("myPrivateField", BindingFlags.NonPublic | BindingFlags.Instance);
object value = privateField.GetValue(myObject);
```

In this example, accessing a private field through reflection can compromise encapsulation and is generally not recommended unless absolutely necessary.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is an abstract class in C#, and how does it differ from an interface?

In C#, both abstract classes and interfaces are used to define contracts that classes must adhere to, but they have some differences in terms of their implementation and usage.

**Abstract Class:**

An abstract class is a class that cannot be instantiated on its own; it is meant to be subclassed by other classes. Abstract classes can contain a mix of regular (non-abstract) methods and abstract methods. Abstract methods are declared without providing an implementation in the abstract class itself, but any derived class must provide concrete implementations for these abstract methods. Abstract classes can also contain fields, properties, and regular methods with implementations.

**Example:**

```cs
abstract class Shape
{
    public abstract double CalculateArea();
    public abstract double CalculatePerimeter();
}

class Circle : Shape
{
    private double radius;

    public Circle(double radius)
    {
        this.radius = radius;
    }

    public override double CalculateArea()
    {
        return Math.PI * radius * radius;
    }

    public override double CalculatePerimeter()
    {
        return 2 * Math.PI * radius;
    }
}
```

In this example, Shape is an abstract class that defines the contract for calculating the area and perimeter of shapes. The Circle class inherits from Shape and provides concrete implementations for the abstract methods.

**Interface:**

An interface is a contract that specifies a set of methods and properties that implementing classes must provide. Unlike abstract classes, interfaces cannot contain any implementation code; they only define method signatures, properties, events, and indexers. A class can implement multiple interfaces.

**Example:**

```cs
interface IResizable
{
    void Resize(int width, int height);
}

class Rectangle : IResizable
{
    private int width;
    private int height;

    public Rectangle(int width, int height)
    {
        this.width = width;
        this.height = height;
    }

    public void Resize(int newWidth, int newHeight)
    {
        width = newWidth;
        height = newHeight;
    }
}
```

In this example, IResizable is an interface that defines the contract for resizing objects. The Rectangle class implements the interface and provides an implementation for the Resize method.

Key Differences:

* Instantiation: Abstract classes cannot be instantiated directly, while interfaces cannot be instantiated at all. Classes that inherit from an abstract class or implement an interface provide the instantiation.

* Multiple Inheritance: A class can inherit from only one abstract class (abstract class doesn\'t support multiple inheritance), but it can implement multiple interfaces.

* Method Implementation: Abstract classes can have methods with actual code (both abstract and non-abstract), while interfaces only define method signatures without any implementation.

* Fields and Properties: Abstract classes can have fields and properties with implementations, while interfaces cannot have fields and properties with initial values.

Both abstract classes and interfaces serve as ways to define contracts and promote code reusability, but the choice between them depends on the specific design and requirements of your application.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a sealed class in C# and why is it used?

In C#, a sealed class is a class that cannot be inherited by other classes. When a class is marked as sealed, it means that it is complete and cannot be extended further.

The main reason to use a sealed class is to prevent the class from being modified or extended by other developers. This is useful when you want to ensure that the class works as intended and cannot be altered in unexpected ways. Sealed classes are also useful for performance reasons, as they can be optimized more effectively by the compiler.

Here are a few other things to note about sealed classes in C#:

* Sealed classes can still inherit from other classes, but they cannot be inherited by other classes.
* Sealed methods can still be overridden by derived classes, but sealed classes cannot be inherited by derived classes.
* It\'s best practice to mark a class as sealed if it is intended to be used as a standalone class and not be inherited.

**Example:**

```cs
sealed class MySealedClass
{
    // class members
}
```

In this example, MySealedClass is marked as sealed, which means that no other class can inherit from it.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the benefits of using a sealed class in C#?

Sealed classes in C# are classes that cannot be inherited by other classes. There are several benefits to using sealed classes in your C# code:

**Security:** Sealing a class prevents other developers from extending or modifying its behavior, which can be useful if the class contains sensitive data or code.

**Performance:** Sealing a class can improve performance because the C# compiler can optimize the code knowing that the class cannot be extended.

**Maintenance:** Sealing a class can make it easier to maintain because you don\'t have to worry about unexpected behavior from subclasses. This can reduce the amount of testing and debugging required.

**Design:** Sealing a class can be useful in your object-oriented design because it can signal that a class is meant to be a final implementation, and not a base class for other classes.

Overall, using sealed classes in C# can help you create more secure, maintainable, and efficient code, and can be an important tool in your object-oriented programming toolkit.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Is it possible for a sealed class to be used as a base class in C#?

In C#, a sealed class is a class that cannot be inherited, meaning you cannot create a derived class from it. Therefore, a sealed class cannot be used as a base class for other classes. The primary purpose of a sealed class is to prevent inheritance, usually when the designer of the class believes that it shouldn't be extended or overridden.

**Example:**

```cs
public sealed class SealedClass
{
    public void SomeMethod()
    {
        Console.WriteLine("This is a method in the sealed class.");
    }
}
```

Now, if you try to create a derived class from the sealed class, you will get a compilation error:

```cs
public class DerivedClass : SealedClass // This will cause a compilation error
{
    // Some additional code
}
```

The error message will be something like: "cannot derive from sealed type 'SealedClass'."

However, you can use a sealed class as a normal class, creating instances of it and using its methods and properties, as shown below:

```cs
class Program
{
    static void Main()
    {
        SealedClass sealedObj = new SealedClass();
        sealedObj.SomeMethod();
    }
}
```

Output:

```cs
This is a method in the sealed class.
```

Remember that using sealed classes is optional, and it's mostly a design decision. If you create a class with the intent of preventing inheritance, you can mark it as sealed. Otherwise, most classes in C# are not sealed, allowing them to be extended and used as base classes for other classes.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Is it possible for a sealed class in C# to define virtual methods?

It is not possible to define a sealed class with virtual methods in C#. The reason for this is that a sealed class is designed to prevent inheritance, and virtual methods are meant to be overridden in derived classes.

When you declare a class as sealed in C#, it means it cannot be inherited, and thus, there would be no derived class to override any virtual methods. This restriction exists to ensure that the behavior of the sealed class remains consistent and cannot be altered or extended by other classes.

**Example:**

```cs
public sealed class SealedClass
{
    public void NonVirtualMethod()
    {
        Console.WriteLine("This is a non-virtual method.");
    }
}
```

If you try to add the virtual keyword to any method in a sealed class, you will encounter a compile-time error.

However, if you want to have a virtual method in a class that can be overridden by its derived classes, you should remove the sealed modifier.

**Example:**

```cs
public class BaseClass
{
    public virtual void VirtualMethod()
    {
        Console.WriteLine("This is a virtual method.");
    }
}
```

Derived classes can then override this virtual method as needed:

```cs
public class DerivedClass : BaseClass
{
    public override void VirtualMethod()
    {
        Console.WriteLine("This is an overridden virtual method in the derived class.");
    }
}

```

In summary, a sealed class in C# cannot define virtual methods, but you can define virtual methods in non-sealed classes to allow for method overriding in derived classes.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Is it possible for a non-child class to define sealed methods in C#?

In C#, the sealed keyword is used to prevent further derivation of a class and to indicate that a method cannot be overridden by any derived class. However, the sealed keyword is typically used with classes to prevent inheritance, not with methods.

Methods in C# are virtual by default, which means they can be overridden by derived classes unless they are marked with the sealed keyword. However, the sealed keyword is used at the class level to prevent inheritance, not at the method level to prevent method overriding.

**Example:**

```cs
public class BaseClass
{
    public virtual void SomeMethod()
    {
        Console.WriteLine("BaseClass: SomeMethod");
    }
}

public sealed class SealedClass : BaseClass
{
    public override void SomeMethod()
    {
        Console.WriteLine("SealedClass: SomeMethod");
    }
}

// This class cannot be defined since SealedClass is sealed
//public class DerivedClass : SealedClass
//{
//}

```

In the example above, the SealedClass is marked as sealed, so it cannot be further derived from. The SomeMethod method in SealedClass is marked with the override keyword, indicating that it overrides the virtual method defined in the BaseClass.

Remember that the sealed keyword is applied at the class level, not at the method level. If you want to prevent a method from being overridden, you can achieve that by not marking the method as virtual or abstract in the first place. There's no need to use the sealed keyword with methods.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can abstract classes be used to implement the Template Method design pattern?

The Template Method design pattern is a behavioral pattern that defines the skeleton of an algorithm in a superclass, but lets subclasses override specific steps of the algorithm without changing its structure. This pattern can be implemented using abstract classes in C#.

**Example:**

```cs
public abstract class AbstractClass
{
    // The template method defines the skeleton of the algorithm.
    public void TemplateMethod()
    {
        Operation1();
        Operation2();
    }

    // These abstract methods will be implemented by the subclasses.
    protected abstract void Operation1();
    protected abstract void Operation2();
}

public class ConcreteClassA : AbstractClass
{
    protected override void Operation1()
    {
        Console.WriteLine("ConcreteClassA.Operation1");
    }

    protected override void Operation2()
    {
        Console.WriteLine("ConcreteClassA.Operation2");
    }
}

public class ConcreteClassB : AbstractClass
{
    protected override void Operation1()
    {
        Console.WriteLine("ConcreteClassB.Operation1");
    }

    protected override void Operation2()
    {
        Console.WriteLine("ConcreteClassB.Operation2");
    }
}
```

In this example, AbstractClass is the abstract class that defines the template method TemplateMethod(). This method calls two abstract methods Operation1() and Operation2(). These methods will be implemented by the subclasses ConcreteClassA and ConcreteClassB.

When a client wants to use the algorithm, it creates an instance of one of the concrete classes and calls the template method:

```cs
AbstractClass abstractClass = new ConcreteClassA();
abstractClass.TemplateMethod();
```

This will output:

```cs
ConcreteClassA.Operation1
ConcreteClassA.Operation2
```

Similarly, the client can create an instance of ConcreteClassB and call the template method:

```cs
AbstractClass abstractClass = new ConcreteClassB();
abstractClass.TemplateMethod();
```

This will output:

```cs
ConcreteClassB.Operation1
ConcreteClassB.Operation2
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you give an example of how abstract classes can lead to implementation of anti-patterns such as the God Object pattern?

Suppose we have an abstract class Animal with various subclasses such as Dog, Cat, Bird, etc. The Animal class has a method Move() that all subclasses are required to implement. Now suppose we want to add a new feature that requires all animals to be able to make a sound. We might be tempted to add a new method MakeSound() to the Animal class, but not all animals can make the same kind of sound. A dog barks, a cat meows, and a bird chirps. One way to implement this is to add a new abstract method MakeSound() to the Animal class and require all subclasses to implement it.

However, this can lead to the God Object pattern if we end up with many methods like MakeSound() that are specific to only a subset of animals. For example, suppose we add a new feature that requires all animals to be able to swim. We might be tempted to add a new method Swim() to the Animal class, but not all animals can swim. A dog can swim, but a cat cannot. Instead of adding a new abstract method Swim() to the Animal class and requiring all subclasses to implement it, we might end up adding a new subclass AquaticAnimal and move all animals that can swim into this subclass. We might then add more subclasses for animals that can fly or crawl, and so on.

Over time, this can lead to the God Object pattern where the Animal class becomes a massive class that contains many methods specific to only a subset of animals. This can make the code difficult to understand and maintain, and violate the single responsibility principle. Instead, it would be better to avoid using abstract classes in this way and use interfaces or smaller, more focused classes that have specific responsibilities.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can abstract classes be used to implement the Factory Method pattern?

The Factory Method pattern is a design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created. Abstract classes in C# can be used to implement the Factory Method pattern by defining the interface for creating objects in the superclass, but delegating the actual creation of objects to its subclasses.

**Example:**

```cs
// Define the abstract Creator class that provides the interface for creating objects
public abstract class Creator
{
    // The factory method that subclasses must implement to create objects
    public abstract Product FactoryMethod();

    // A method that uses the factory method to create a product
    public void Operation()
    {
        Product product = FactoryMethod();
        Console.WriteLine(product.Operation());
    }
}

// Define the abstract Product class that represents the objects to be created
public abstract class Product
{
    public abstract string Operation();
}

// Define concrete Product classes that implement the Product interface
public class ConcreteProductA : Product
{
    public override string Operation()
    {
        return "ConcreteProductA operation.";
    }
}

public class ConcreteProductB : Product
{
    public override string Operation()
    {
        return "ConcreteProductB operation.";
    }
}

// Define concrete Creator classes that implement the Factory Method to create different types of Products
public class ConcreteCreatorA : Creator
{
    public override Product FactoryMethod()
    {
        return new ConcreteProductA();
    }
}

public class ConcreteCreatorB : Creator
{
    public override Product FactoryMethod()
    {
        return new ConcreteProductB();
    }
}

```

In this implementation, the abstract Creator class defines the factory method FactoryMethod() that subclasses must implement to create Product objects. The Creator class also provides a Operation() method that uses the FactoryMethod() to create a Product object and perform an operation on it.

The abstract Product class defines the interface for the objects to be created by the factory method.

The concrete Product classes ConcreteProductA and ConcreteProductB implement the Product interface and define the actual behavior of the objects to be created.

The concrete Creator classes ConcreteCreatorA and ConcreteCreatorB implement the FactoryMethod() to create different types of Product objects. By implementing the FactoryMethod() in each subclass, the type of Product created can be changed without modifying the Creator class.

**Example:**

```cs
// Use a ConcreteCreatorA to create a ConcreteProductA
Creator creator = new ConcreteCreatorA();
creator.Operation();  // Output: ConcreteProductA operation.

// Use a ConcreteCreatorB to create a ConcreteProductB
creator = new ConcreteCreatorB();
creator.Operation();  // Output: ConcreteProductB operation.
```

In this usage example, the Creator object is created as either a ConcreteCreatorA or a ConcreteCreatorB, and the Operation() method is called on it to create a Product object and perform an operation on it. The type of Product created depends on the FactoryMethod() implemented in the concrete Creator subclass.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you explain the SOLID principle of Single Responsibility and how it relates to abstract classes in C#?

The SOLID principles are a set of five design principles that aim to create well-structured, maintainable, and scalable software systems. The first principle, known as the Single Responsibility Principle (SRP), states that a class should have only one reason to change. In other words, a class should have only one primary responsibility.

This principle encourages you to design your classes in such a way that each class is focused on doing one thing and doing it well. This improves the maintainability of your code because changes related to a specific responsibility won't affect other parts of the code that are unrelated.

Now, let\'s relate the Single Responsibility Principle to abstract classes in C# using an example.

Suppose we\'re building a simple drawing application. We might have various shapes like circles, rectangles, and triangles. We want to apply the Single Responsibility Principle to our design.

**Example:**

```cs
// This class violates SRP by having multiple responsibilities
class Shape
{
    public virtual void Draw() { /* Draw the shape */ }
    public virtual void Move() { /* Move the shape */ }
    public virtual void Resize() { /* Resize the shape */ }
}

```

In this example, the Shape class has multiple responsibilities: drawing, moving, and resizing. If any of these responsibilities changes, it could potentially impact the other responsibilities. This violates the SRP.

To adhere to the Single Responsibility Principle, we should separate these responsibilities into different classes. Abstract classes can be used to define common behavior that multiple classes share, while still adhering to SRP. Here's an improved design:

```cs
// Abstract class for drawing shapes
abstract class Shape
{
    public abstract void Draw();
}

// Separate class for moving shapes
class MovableShape : Shape
{
    public override void Draw() { /* Draw the shape */ }
    public void Move() { /* Move the shape */ }
}

// Separate class for resizing shapes
class ResizableShape : Shape
{
    public override void Draw() { /* Draw the shape */ }
    public void Resize() { /* Resize the shape */ }
}

```

In this design, we have separated the responsibilities of drawing, moving, and resizing into different classes. Each class adheres to the SRP, as they have only one primary responsibility.

By using abstract classes, we define a common interface (in this case, the Draw method) for the various shape-related classes. This allows us to ensure consistent behavior across different shapes while still adhering to the principle of single responsibility.

Remember, the Single Responsibility Principle helps create more maintainable code by reducing the impact of changes and improving the clarity of class responsibilities.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can abstract classes be used to implement the Dependency Inversion principle?

The Dependency Inversion principle (DIP) states that high-level modules should not depend on low-level modules, but rather both should depend on abstractions. Abstractions should not depend on details, but details should depend on abstractions.

In C#, one way to implement DIP using abstract classes is by defining an abstract class that represents the abstraction, and then defining concrete implementations of that abstraction in lower-level modules.

Here is an example of how this can be done:

```cs
public abstract class ILogger
{
    public abstract void Log(string message);
}

public class FileLogger : ILogger
{
    public void Log(string message)
    {
        // Log message to a file
    }
}

public class DatabaseLogger : ILogger
{
    public void Log(string message)
    {
        // Log message to a database
    }
}

public class Application
{
    private readonly ILogger _logger;

    public Application(ILogger logger)
    {
        _logger = logger;
    }

    public void DoSomething()
    {
        // Do something

        _logger.Log("Did something");
    }
}

```

In this example, ILogger is an abstract class that represents the abstraction of a logger. FileLogger and DatabaseLogger are concrete implementations of the ILogger abstraction in lower-level modules.

The Application class is a higher-level module that depends on the ILogger abstraction. Instead of depending on a specific implementation of ILogger, Application depends on the abstraction itself. This allows different implementations of ILogger to be used without changing the Application class.

By using abstract classes to define abstractions and depending on those abstractions instead of concrete implementations, we can achieve the Dependency Inversion principle in our code.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you explain the SOLID principle of Liskov Substitution and how it relates to abstract classes in C#?

The Liskov Substitution Principle (LSP) is a fundamental principle of object-oriented programming that states that objects of a superclass should be able to be replaced with objects of its subclasses without affecting the correctness of the program. In other words, if a program is designed to work with a superclass object, it should also work with any of its subclasses.

In C#, abstract classes are often used to implement Liskov Substitution. Abstract classes are classes that cannot be instantiated, but instead serve as a template for other classes to inherit from. When a class inherits from an abstract class, it must provide implementations for all of the abstract methods declared in the abstract class. This ensures that any object created from the subclass can be substituted for an object of the superclass, without affecting the correctness of the program.

For example, consider a program that uses a superclass called "Animal" to represent different types of animals, with a method called "MakeSound" that all animals should implement. We can use an abstract class to define the Animal class and make the MakeSound method abstract, like so:

```cs
public abstract class Animal
{
    public abstract void MakeSound();
}

```

Then, we can create subclasses for different types of animals, like "Cat", "Dog", and "Bird", that inherit from the Animal class and implement the MakeSound method:

```cs
public class Cat : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Meow");
    }
}

public class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Woof");
    }
}

public class Bird : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Chirp");
    }
}

```

Now, we can create objects of the different animal types and use them interchangeably, because they all inherit from the same Animal superclass and implement the same MakeSound method:

```cs
Animal cat = new Cat();
Animal dog = new Dog();
Animal bird = new Bird();

cat.MakeSound(); // prints "Meow"
dog.MakeSound(); // prints "Woof"
bird.MakeSound(); // prints "Chirp"
```

This demonstrates the Liskov Substitution Principle, because we can use objects of the subclass types (Cat, Dog, Bird) in place of the superclass type (Animal), without affecting the correctness of the program.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between an abstract class and a concrete class in C#, and how does this relate to the Open-Closed principle?

In C#, an abstract class is a class that cannot be instantiated directly, but instead serves as a template for other classes to inherit from. It is typically used to define a set of common characteristics or behaviors that all derived classes should have. Abstract classes may contain one or more abstract methods, which are declared without an implementation and must be implemented by any derived class.

On the other hand, a concrete class is a class that can be instantiated directly and provides an implementation for all its methods. It does not contain any abstract methods and can be used as-is without any need for further implementation.

The Open-Closed principle is a principle in object-oriented programming that states that classes should be open for extension but closed for modification. This means that a class should be designed in such a way that it can be easily extended to add new functionality without modifying its existing code.

Abstract classes and interfaces are typically used to achieve this principle. By defining an abstract class that contains a set of abstract methods, the class can be easily extended by creating new derived classes that implement these abstract methods. This allows for new functionality to be added to the system without modifying the existing code. In contrast, concrete classes that do not use abstract methods may be more difficult to extend without modifying the existing code.

In summary, the difference between an abstract class and a concrete class in C# is that an abstract class provides a template for other classes to inherit from and contains one or more abstract methods that must be implemented by any derived class, while a concrete class can be instantiated directly and provides an implementation for all its methods. The use of abstract classes and interfaces can help to achieve the Open-Closed principle by allowing for easy extension of a system without modifying its existing code.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you give an example of how abstract classes can help to enforce the Interface Segregation principle?

Let\'s say we have an interface called IAnimal that has several methods related to animal behavior, such as Eat(), Sleep(), and Move(). However, not all animals have the same behavior, so it might be difficult to create a single interface that covers all animal types.

One way to enforce the Interface Segregation principle in this scenario is to create abstract classes that implement subsets of the methods in the IAnimal interface.

**Example:**

```cs
public interface IAnimal {
    void Eat();
    void Sleep();
    void Move();
}

public abstract class Carnivore : IAnimal {
    public void Eat() {
        // Carnivores have a specific way of eating
    }

    public void Sleep() {
        // All animals sleep similarly
    }

    public abstract void Move();
}

public abstract class Herbivore : IAnimal {
    public void Eat() {
        // Herbivores have a specific way of eating
    }

    public void Sleep() {
        // All animals sleep similarly
    }

    public abstract void Move();
}

```

In this example, we\'ve created two abstract classes Carnivore and Herbivore that implement the Eat() and Sleep() methods from the IAnimal interface. These abstract classes also declare an abstract Move() method, which allows subclasses to implement their own version of Move() depending on the specific animal type.

By creating these abstract classes, we've separated the methods in the IAnimal interface into smaller, more specific groups of behavior. This helps ensure that classes that implement IAnimal only need to implement the methods that are relevant to their specific behavior, rather than having to implement all the methods in the interface, which might not be necessary or applicable for all animal types.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can abstract classes be used to implement the Strategy design pattern?

The Strategy design pattern is a behavioral design pattern that allows you to define a family of algorithms, encapsulate each one as an object, and make them interchangeable at runtime. Abstract classes in C# can be used to implement the Strategy design pattern by defining a common interface for all the algorithms in the family and providing a set of concrete implementations that can be swapped out at runtime.

Here is an example of how to use abstract classes to implement the Strategy design pattern in C#:

Define an abstract base class that provides the common interface for all the algorithms in the family. This abstract base class should define an abstract method that represents the algorithm.

```cs
public abstract class Strategy
{
    public abstract void Algorithm();
}
```

Define concrete subclasses that implement the abstract method defined in the abstract base class. Each concrete subclass represents a specific algorithm in the family.

```cs
public class ConcreteStrategyA : Strategy
{
    public override void Algorithm()
    {
        // implement algorithm A
    }
}

public class ConcreteStrategyB : Strategy
{
    public override void Algorithm()
    {
        // implement algorithm B
    }
}
```

Define a context class that uses the Strategy objects. The context class should have a property that holds a reference to a Strategy object and a method that invokes the algorithm defined in the Strategy object.

```cs
public class Context
{
    private Strategy _strategy;

    public void SetStrategy(Strategy strategy)
    {
        _strategy = strategy;
    }

    public void ExecuteAlgorithm()
    {
        _strategy.Algorithm();
    }
}
```

Use the context class to execute different algorithms at runtime by setting the appropriate Strategy object using the SetStrategy method and then invoking the ExecuteAlgorithm method.

```cs
Context context = new Context();

context.SetStrategy(new ConcreteStrategyA());
context.ExecuteAlgorithm(); // executes algorithm A

context.SetStrategy(new ConcreteStrategyB());
context.ExecuteAlgorithm(); // executes algorithm B
```

In this example, the abstract base class Strategy defines the common interface for all the algorithms in the family, while the concrete subclasses ConcreteStrategyA and ConcreteStrategyB provide specific implementations of the algorithms. The context class Context uses the Strategy objects to execute the algorithms at runtime, and the client code can switch between different algorithms by setting the appropriate Strategy object using the SetStrategy method.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Is it possible to declare abstract methods as private in C#?

In C#, it is not possible to declare abstract methods as private directly. The abstract keyword is used to define a method signature in an abstract class or interface that must be implemented in derived classes. However, the private access modifier restricts the visibility of a method to within the defining class only and cannot be combined with abstract.

Here\'s an example to illustrate why it's not possible:

```cs
public abstract class MyBaseClass
{
    private abstract void MyAbstractMethod(); // Error: 'private' and 'abstract' cannot be combined
}

```

The above code will result in a compilation error.

However, there\'s an indirect way to achieve similar behavior using a combination of protected and abstract:

```cs
public abstract class MyBaseClass
{
    protected abstract void MyAbstractMethod();
}

public class MyDerivedClass : MyBaseClass
{
    // We override the abstract method, but the base class made it 'protected,' not 'public.'
    protected override void MyAbstractMethod()
    {
        // Implementation goes here
    }
}
```

By using protected, we ensure that the abstract method can only be accessed within the derived classes, effectively restricting its visibility outside the inheritance chain. Although it's not precisely private, this pattern provides a way to enforce implementation in derived classes while maintaining restricted access from outside the class hierarchy.

Keep in mind that if you need the abstract method to be strictly private, you may need to reconsider your design and refactor your code to achieve your desired encapsulation.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Is it possible for an abstract class to contain a main method in C#?

In C#, it is possible for an abstract class to contain a static main method. However, you won't be able to create an instance of an abstract class directly because abstract classes are meant to be subclassed and used as base classes. The main method is the entry point of a C# application and is usually found in a static class. When it comes to abstract classes, you can use them in combination with concrete classes that derive from them.

**Example:**

```cs
using System;

// Abstract class with a static main method
abstract class Shape
{
    public abstract void Draw();

    public static void Main(string[] args)
    {
        // Since the class is abstract, we can't create an instance of it.
        // Let's create instances of its subclasses to demonstrate.
        Shape shape1 = new Circle();
        Shape shape2 = new Square();

        shape1.Draw();
        shape2.Draw();
    }
}

// Concrete subclass 1
class Circle : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a circle.");
    }
}

// Concrete subclass 2
class Square : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a square.");
    }
}

```

In this example, we have an abstract class Shape with an abstract method Draw(). The class also contains a static Main method. Notice that you can't directly create an instance of the Shape class, but you can create instances of its subclasses, Circle and Square.

When you run this program, the Main method will be the entry point and will execute the Draw method for both the Circle and Square instances, as they are assigned to variables of the abstract class Shape.

Remember, the Main method can only be declared in a static class, but an abstract class can contain static members like the Main method.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Is it possible in C# that a class inherit from multiple abstract classes?

No, in C#, a class cannot directly inherit from multiple abstract classes. C# supports single inheritance for classes, which means a class can only inherit from one base class. This is a fundamental design choice in the language.

However, C# allows a class to implement multiple interfaces. Interfaces are similar to abstract classes in that they define a contract for the derived classes, but they cannot provide any implementation details. A class can implement multiple interfaces, effectively providing a form of multiple inheritance through interfaces.

**Example:**

```cs
// Abstract class 1
public abstract class Shape
{
    public abstract double Area();
}

// Abstract class 2
public abstract class Colorful
{
    public abstract string GetColor();
}

// Concrete class implementing multiple interfaces
public class Circle : Shape, Colorful
{
    private double radius;
    private string color;

    public Circle(double radius, string color)
    {
        this.radius = radius;
        this.color = color;
    }

    public override double Area()
    {
        return Math.PI * radius * radius;
    }

    public string GetColor()
    {
        return color;
    }
}

```

In this example, the class Circle inherits from the Shape abstract class and implements the Colorful interface. This allows Circle to provide its own implementation for the Area() method and the GetColor() method.

If you need a class to have behavior from multiple sources resembling abstract classes, you can use a combination of abstract classes and interfaces to achieve that. Create one abstract class that holds the common behavior and then have the class implement interfaces to handle other aspects.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between a sealed class and an unsealed class in C#?

In C#, a sealed class and an unsealed (or open) class have distinct characteristics related to inheritance and extension. Let's explore the differences between them using examples.

Sealed Class:

A sealed class is a class that cannot be inherited. It's marked with the sealed keyword. This means you cannot create a derived class that inherits from a sealed class.

```cs
sealed class SealedClass
{
    public void SomeMethod()
    {
        Console.WriteLine("Method in sealed class");
    }
}

class DerivedClass : SealedClass // This will result in a compilation error
{
    // ...
}
```

In this example, attempting to derive a class from SealedClass (DerivedClass in this case) would result in a compilation error.

Unsealed (Open) Class:
An unsealed class is a regular class that can be inherited. By default, all classes in C# are unsealed, meaning they can be used as base classes for other classes.

```cs
class BaseClass
{
    public virtual void SomeMethod()
    {
        Console.WriteLine("Method in base class");
    }
}

class DerivedClass : BaseClass
{
    public override void SomeMethod()
    {
        Console.WriteLine("Method in derived class");
    }
}

```

In this example, BaseClass is an unsealed class, and you can derive a new class from it (DerivedClass). The virtual keyword in the SomeMethod of the base class allows the method to be overridden in the derived class using the override keyword.

To summarize:

Sealed Class: Cannot be inherited. Marked with the sealed keyword.
Unsealed (Open) Class: Can be inherited. By default, all classes are unsealed unless marked sealed.

Use sealed classes when you want to prevent further derivation from a particular class to maintain control over its behavior and design. Use unsealed classes for normal class hierarchies where you expect other classes to derive from them and extend their functionality.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can you use a sealed class to prevent inheritance in C#?

In C#, a sealed class is a class that cannot be inherited by other classes. To prevent inheritance, you can declare a class as sealed using the sealed keyword.

**Example:**

```cs
public sealed class MyClass
{
    // class members
}

```

In the example above, the MyClass class is declared as sealed, which means it cannot be inherited by other classes.

By marking a class as sealed, you prevent other developers from creating derived classes that inherit from it. This can be useful in situations where you want to maintain control over how your class is used, and ensure that it is not extended in a way that could break its behavior.

It\'s worth noting that while a sealed class cannot be inherited, it can still be used as a base class for other classes. Additionally, it\'s important to keep in mind that the sealed keyword only prevents inheritance; it doesn't prevent a developer from creating an instance of the class and using its public members.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

#### Q. Can we do Multiple inheritance with Abstract classes?
#### Q. Whats the difference between Abstract class and interface?
#### Q. Why simple base class replace Abstract class?
#### Q. What are nested classes and when to use them?
#### Q. Can Nested class access outer class variables?
#### Q. Can we have public, protected access modifiers in nested class?
#### Q. Are private class members inherited to the derived class?
#### Q. Where is a protected class-level variable available?
#### Q. Are private class-level variables inherited?
#### Q. Which class is at the top of .NET class hierarchy?
#### Q. What is the .NET collection class that allows an element to be accessed using a unique key?
#### Q. Explain the three services model commonly know as a three-tier application?
#### Q. Can you prevent your class from being inherited by another class?
#### Q. Can you allow a class to be inherited, but prevent the method from being over-ridden?
#### Q. When do you absolutely have to declare a class as abstract?
#### Q. What is the implicit name of the parameter that gets passed into the set method/property of a class?
#### Q. If a base class has a number of overloaded constructors, and an inheriting class has a number of overloaded constructors; can you enforce a call from an inherited constructor to a specific base constructor?
#### Q. When you inherit a protected class-level variable, who is it available to?
#### Q. Are private class-level variables inherited?
#### Q. What is the top .NET class that everything is derived from?
#### Q. When do you absolutely have to declare a class as abstract (as opposed to free-willed educated choice or decision based on UML diagram)?
#### Q. If a base class has a bunch of overloaded constructors, and an inherited class has another bunch of overloaded constructors, can you enforce a call from an inherited constructor to an arbitrary base constructor?
#### Q. Is it namespace class or class namespace?
#### Q. What is the default Access Modifier for the members of the class?
#### Q. How to Call the Default constructor of one class with the parameterised constructor of same class?
#### Q. How to access the constructors of one class to another class?
#### Q. What is scope of a Protected Internal member variable of a C# class?
#### Q. What is the difference between Interface and Abstract Class?    
#### Q. What is the difference between Virtual method and Abstract method?
#### Q. What is scope of a Internal member variable of a C# class?
#### Q. How would you implement multiple interfaces with the same method name in the same class?
#### Q. How can you create a derived class object from a base class?
#### Q. Which is the parent class of all classes which we create in C#?
#### Q. What is the base class in .NET framework from which all the classes have been developed?
#### Q. How do you implement a custom attribute in C#? Provide an example of how to use it to decorate a class.
#### Q. What is the  System. String and System.Text.StringBuilder classes?
#### Q. What are I/O classes in C#?
#### Q. What is the difference between an abstract and interface class?
#### Q. Can you add extension methods to an existing static class?
#### Q. Can you create sealed abstract class in C#?  
#### Q. Can you inherit multiple interfaces?
#### Q. What interface should your data structure implement to make the "Where" method work?
#### Q. Can we define methods as private in interface?
#### Q. If i want to change interface whats the best practice?
#### Q. Can we create instance of interface?
#### Q. Explain the differences between covariance and contravariance in C# for delegates and interfaces.
#### Q. What is an abstraction?
#### Q. Is it compulsory to implement Abstract methods?
#### Q. What is the method MemberwiseClone() doing?
#### Q. Explain the difference between destructor, dispose and finalize method?
#### Q. Could you explain the difference between Func vs. Action vs. Predicate?
#### Q. What is a namespace and is it compulsory?
#### Q. What do you think about empty destructor?
#### Q. What are the different types of "USING/HAS A" relationship?
#### Q. Differentiate between Composition vs Aggregation vs Association?
#### Q. What are circular references?
#### Q. What is weak reference in C#?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 4. ENCAPSULATION

<br/>

#### Q. How encapsulation is implemented in C#?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 5. INHERITANCE

<br/>

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is inheritance in C# and how does it work?

Inheritance in C# is a mechanism by which a new class can be derived from an existing class, known as the base class or superclass. The derived class, known as the subclass or child class, inherits all the properties, methods, and fields of the base class and can also add new properties, methods, and fields of its own.

To create a derived class in C#, you use the colon (:) symbol after the name of the derived class and specify the name of the base class.

**Example:**

```cs
class ChildClass : BaseClass
{
    // Add new properties, methods, and fields here
}
```

Once you have defined the child class, you can create objects of that class that will inherit all the properties, methods, and fields of the base class, as well as any additional ones defined in the child class.

Inheritance allows you to create a hierarchy of related classes, where each class inherits from the one above it. This can help you to write more modular, reusable code, as you can define common behavior in the base class and then add specific behavior in the child classes.

C# also supports multiple inheritance, where a class can inherit from more than one base class, but this is achieved through the use of interfaces rather than direct inheritance.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between single inheritance and multiple inheritance in C#?

In object-oriented programming, inheritance is a mechanism that allows a new class (the derived or subclass) to inherit properties and behaviors (fields and methods) from an existing class (the base or superclass). C# supports both single inheritance and multiple inheritance, and each has its own characteristics.

Single Inheritance:

In single inheritance, a class can inherit from only one base class. This promotes a simpler and more straightforward class hierarchy.

```cs
// Base class
class Animal
{
    public void Eat()
    {
        Console.WriteLine("Animal is eating.");
    }
}

// Derived class inheriting from a single base class
class Dog : Animal
{
    public void Bark()
    {
        Console.WriteLine("Dog is barking.");
    }
}
```

In this example, the class Dog inherits from the single base class Animal. It can access the Eat method from the base class and also has its own method Bark.

Multiple Inheritance:

Multiple inheritance allows a class to inherit from more than one base class. However, C# does not support multiple inheritance of classes (i.e., inheriting from multiple classes). Instead, it supports multiple inheritance through interfaces.

```cs
// First base interface
interface IDriveable
{
    void Drive();
}

// Second base interface
interface IFlyable
{
    void Fly();
}

// Derived class implementing multiple interfaces
class FlyingCar : IDriveable, IFlyable
{
    public void Drive()
    {
        Console.WriteLine("Flying car is being driven on the road.");
    }

    public void Fly()
    {
        Console.WriteLine("Flying car is taking off and flying.");
    }
}
```

In this example, the class FlyingCar implements both the IDriveable and IFlyable interfaces, effectively achieving a form of multiple inheritance. It can access the methods defined in both interfaces.

It\'s important to note that C# interfaces provide a way to achieve the benefits of multiple inheritance while avoiding some of the complexities and ambiguities associated with traditional multiple inheritance in other languages like C++.

In summary, single inheritance involves a class inheriting from a single base class, while multiple inheritance can be achieved in C# through interfaces, allowing a class to inherit from multiple interfaces.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can you use inheritance to extend an existing class in C#?

In C#, you can use inheritance to extend an existing class by creating a new class that inherits from the existing class. Here are the steps to do so:

Define the parent class: Start by defining the existing class that you want to extend. This class is often referred to as the "base class" or the "parent class".

```cs
public class ParentClass
{
    // fields, properties, and methods of the parent class
}
```

Define the child class: Create a new class that inherits from the parent class using the : symbol, followed by the name of the parent class.

```cs
public class ChildClass : ParentClass
{
    // fields, properties, and methods of the child class
}
```

Add new functionality: Now you can add new fields, properties, and methods to the child class that were not present in the parent class. The child class can also override or extend existing methods and properties of the parent class.

```cs
public class ChildClass : ParentClass
{
    public int NewField { get; set; }

    public void NewMethod()
    {
        // new functionality
    }

    public override void ExistingMethod()
    {
        base.ExistingMethod(); // calling the method of the parent class
        // additional functionality
    }
}
```

By using inheritance, you can reuse the code from the parent class and add new functionality to the child class, without having to rewrite the existing code in the parent class.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can you use the "base" keyword in C# to call the base class constructor?

In C#, you can use the base keyword to call the constructor of a base class from a derived class. Here is the syntax for using the base keyword to call a base class constructor:

```cs
public class DerivedClass : BaseClass
{
    public DerivedClass(args) : base(args)
    {
        // constructor body
    }
}
```

In this example, DerivedClass is derived from BaseClass, and the constructor of DerivedClass is calling the constructor of BaseClass using the base keyword.

Note that the base(args) call must be the first statement in the derived class constructor. This is because the base class constructor needs to be called before any other initialization can be done in the derived class constructor.

You can also use the base keyword to call methods and access properties of the base class from the derived class.

**Example:**

```cs
public class DerivedClass : BaseClass
{
    public DerivedClass(args)
    {
        base.MethodInBaseClass();
        int value = base.PropertyInBaseClass;
    }
}
```

In this example, MethodInBaseClass() is a method defined in BaseClass, and PropertyInBaseClass is a property defined in BaseClass. The base keyword is used to access these members from the derived class.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you explain polymorphism and how it is achieved in C#?

Polymorphism is a concept in object-oriented programming (OOP) that allows objects of different types to be treated as if they were the same type, providing a way to write code that is more flexible and reusable.

In C#, polymorphism can be achieved in several ways, including:

Method Overloading: This allows you to define multiple methods with the same name, but with different parameters. C# will determine which method to call based on the number and types of the arguments passed to it.

Method Overriding: This allows a subclass to provide a different implementation of a method that is already defined in its superclass. When the method is called on an instance of the subclass, the overridden version is executed instead of the superclass version.

Interfaces: An interface is a contract that specifies a set of methods and properties that a class must implement. This allows objects of different types to be treated as if they were of the same type as long as they implement the same interface.

**Example:**

```cs
public class Animal {
    public virtual void MakeSound() {
        Console.WriteLine("Animal makes a sound.");
    }
}

public class Cat : Animal {
    public override void MakeSound() {
        Console.WriteLine("Meow.");
    }
}

public class Dog : Animal {
    public override void MakeSound() {
        Console.WriteLine("Woof.");
    }
}

// Usage
Animal animal1 = new Cat();
animal1.MakeSound(); // outputs "Meow."

Animal animal2 = new Dog();
animal2.MakeSound(); // outputs "Woof."

```

In this example, we have a base class Animal with a virtual MakeSound() method. The Cat and Dog classes inherit from Animal and override the MakeSound() method with their own implementation. We can then create instances of Cat and Dog, but store them in variables of type Animal. When we call the MakeSound() method on these variables, the overridden version of the method in the corresponding subclass is executed.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How can you use inheritance and polymorphism together to achieve dynamic dispatch in C#?

In C#, inheritance and polymorphism can be used together to achieve dynamic dispatch through method overriding. Dynamic dispatch allows for the selection of the most appropriate method to be called based on the runtime type of the object.

To achieve dynamic dispatch in C# using inheritance and polymorphism, you can follow these steps:

Define a base class with a virtual method. This base class will serve as the superclass for all derived classes. The virtual method can be overridden by the derived classes.

```cs
public class Shape
{
    public virtual void Draw()
    {
        Console.WriteLine("Drawing a shape.");
    }
}
```

Define a derived class that inherits from the base class and overrides the virtual method. The derived class can implement its own version of the method.

```cs
public class Circle : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a circle.");
    }
}

```

Instantiate an object of the derived class and call the virtual method. The runtime type of the object will determine which version of the method will be called.

```cs
Shape shape1 = new Shape();
Shape shape2 = new Circle();

shape1.Draw(); // Output: "Drawing a shape."
shape2.Draw(); // Output: "Drawing a circle."

```

In the above code, shape1 is an instance of the Shape class, so calling Draw() will invoke the method in the Shape class. However, shape2 is an instance of the Circle class, so calling Draw() will invoke the overridden method in the Circle class.

This is an example of dynamic dispatch using inheritance and polymorphism in C#. The appropriate method is selected at runtime based on the actual type of the object, allowing for flexible and extensible code.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the base keyword in C#, and how is it used in inheritance?

In C#, the base keyword is used to refer to the base class of the current class. It is used to access members of the base class, such as methods, properties, and fields.

In inheritance, the base keyword is used to call a constructor of the base class from a derived class. When a derived class is instantiated, its constructor must call a constructor of the base class to initialize the base class members. This can be done using the base keyword followed by the constructor parameters.

**Example:**

```cs
public class Vehicle
{
    public int NumWheels { get; set; }

    public Vehicle(int numWheels)
    {
        NumWheels = numWheels;
    }
}

public class Car : Vehicle
{
    public string Make { get; set; }

    public Car(int numWheels, string make) : base(numWheels)
    {
        Make = make;
    }
}

```

In this example, Vehicle is the base class and Car is the derived class. The Car constructor takes two parameters, numWheels and make, and calls the base class constructor using the base keyword with the numWheels parameter. This initializes the NumWheels property of the Vehicle base class. The Make property of the Car derived class is initialized separately.

By using the base keyword, we can ensure that the base class is properly initialized before any derived class-specific initialization takes place.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Can you discuss the differences between virtual methods and abstract methods in C#, and when each should be used?

In C#, both virtual and abstract methods are used for defining methods that can be overridden by derived classes. However, there are some differences between the two.

+ Virtual Methods:
A virtual method is a method that can be overridden in a derived class. It provides a default implementation of a method that can be replaced with a derived class-specific implementation. The virtual keyword is used to define a virtual method.

**Example:**

```cs
public class MyBaseClass
{
   public virtual void MyVirtualMethod()
   {
      Console.WriteLine("This is the base class virtual method.");
   }
}

public class MyDerivedClass : MyBaseClass
{
   public override void MyVirtualMethod()
   {
      Console.WriteLine("This is the derived class override of the virtual method.");
   }
}

```

+ Abstract Methods:
An abstract method is a method that does not have a default implementation in the base class and must be implemented by any derived class. Abstract methods are defined using the abstract keyword and must be implemented by the derived classes. Abstract methods are used to define a common interface or contract that derived classes must implement.

**Example:**

```cs
public abstract class MyBaseClass
{
   public abstract void MyAbstractMethod();
}

public class MyDerivedClass : MyBaseClass
{
   public override void MyAbstractMethod()
   {
      Console.WriteLine("This is the derived class implementation of the abstract method.");
   }
}

```

+ When to use virtual methods:
Virtual methods are useful when you want to provide a default implementation of a method that can be overridden by a derived class. Virtual methods are often used in base classes to provide a default behavior that can be customized in the derived classes.

+ When to use abstract methods:
Abstract methods are useful when you want to define a common interface or contract that derived classes must implement. Abstract methods are often used in abstract classes and interfaces to define the behavior that must be implemented by the derived classes. Abstract methods are also useful when you want to ensure that the derived classes implement a specific behavior or functionality.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle constructor inheritance in C#, and what is the role of the base keyword in constructor inheritance?

In C#, constructor inheritance is handled through the use of the base keyword. The base keyword allows a derived class to call a constructor from its base class. This is useful when the base class has state or behavior that needs to be initialized before the derived class can be constructed.

To inherit a constructor from a base class, the derived class must include a constructor that calls the base constructor using the base keyword.

**Example:**

```cs
public class MyBaseClass
{
    private int myInt;

    public MyBaseClass(int myInt)
    {
        this.myInt = myInt;
    }
}

public class MyDerivedClass : MyBaseClass
{
    public MyDerivedClass(int myInt) : base(myInt)
    {
        // Additional initialization code for the derived class can be added here
    }
}

```

In this example, MyBaseClass has a constructor that takes an integer argument and initializes the private field myInt. MyDerivedClass inherits from MyBaseClass and includes a constructor that takes an integer argument. The MyDerivedClass constructor calls the MyBaseClass constructor using the base keyword, passing in the integer argument. This initializes the myInt field in the base class, which can then be used by the derived class.

It\'s important to note that if the base class has multiple constructors, the derived class must call one of them using the base keyword in each of its constructors.

Constructor inheritance allows derived classes to reuse code from their base classes and can help to reduce code duplication. The base keyword is used to call the appropriate constructor in the base class and ensure that the base class\'s state is properly initialized before the derived class is constructed.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between inheritance and composition in C# and when should you use each one?

Inheritance and composition are two fundamental concepts in object-oriented programming that allow developers to build more complex and robust software systems.

Inheritance is a mechanism that allows a class to inherit the characteristics of another class, referred to as the base or parent class. In C#, this is achieved using the "extends" keyword. The derived or child class inherits all the non-private members (methods, properties, and fields) of the parent class and can also add new members or override existing ones. Inheritance is often used to model an "is-a" relationship between classes, where a derived class represents a more specialized version of the parent class.

Composition, on the other hand, is a mechanism that allows a class to contain objects of other classes, referred to as its components. In C#, this is achieved by creating a field in the class that holds a reference to an instance of another class. Composition is often used to model a "has-a" relationship between classes, where a class has one or more components that are essential to its functionality.

So, when should you use inheritance vs composition? Here are some guidelines:

Use inheritance when you want to model an "is-a" relationship between classes, where a derived class is a more specialized version of the parent class. For example, a Dog class may inherit from an Animal class because a dog is a type of animal.

Use composition when you want to model a "has-a" relationship between classes, where a class has one or more components that are essential to its functionality. For example, a Car class may have a Engine component because a car needs an engine to function.

In general, prefer composition over inheritance, because it leads to more flexible and maintainable code. With composition, you can easily change the behavior of a class by replacing one of its components, whereas with inheritance, you are more tightly coupled to the parent class\'s implementation.

Avoid deep inheritance hierarchies, as they can make the code difficult to understand and maintain. Instead, use interfaces to define common behaviors that classes can implement.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

#### Q. Can you override private virtual methods?
#### Q. What does the keyword virtual mean in the method definition?
#### Q. What is Virtual Method in C#?
#### Q. What is a Virtual Method in C#?
#### Q. Why a private virtual method cannot be overridden in C#?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 6. COLLECTIONS

<br/>

## Q. What are Concurrent Collection Classes?

.NET Framework class library comes with Concurrent collection classes so that multiple threads can share collection data between them without using synchronization primitives.

There are four types of Concurrent classes.

* ConcurrentQueue
* ConcurrentStack
* ConcurrentDictionary
* ConcurrentBag

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain Hashtable in C#?

A Hashtable is a collection that stores (Keys, Values) pairs. Here, the Keys are used to find the storage location and are immutable and cannot have duplicate entries in the Hashtable. The .Net Framework has provided a Hash Table class that contains all the functionality required to implement a hash table without any additional development. The hash table is a general-purpose dictionary collection. Each item within the collection is a DictionaryEntry object with two properties: a key object and a value object. These are known as Key/Value. When items are added to a hash table, a hashcode is generated automatically. This code is hidden from the developer. All access to the table\'s values is achieved using the key object for identification.
As the items in the collection are sorted according to the hidden hash code, the items should be considered to be randomly ordered.

The Hashtable Collection: The Base Class libraries offers a Hashtable Class that is defined in the System.Collections namespace, so you don't have to code your own hash tables. It processes each key of the hash that you add every time and then uses the hash code to look up the element very quickly. The capacity of a hash table is the number of elements the hash table can hold. As elements are added to a hash table, the capacity is automatically increased as required through reallocation. It is an older .Net Framework type.

Declaring  a  Hashtable:  The  Hashtable  class  is  generally  found  in  the  namespace  called System.Collections. So to execute any of the examples, we have to add using System.Collections; to the source code. The declaration for the Hashtable is:
Hashtable HT = new Hashtable ();

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is IEnumerable<> in C#?

IEnumerable is the parent interface for all non-generic collections in System.Collections namespace like ArrayList, HastTable etc. that can be enumerated. For the generic version of this interface as IEnumerable<T> which a parent interface of all generic collections class in System.Collections.Generic namespace like List<> and more.
In System.Collections.Generic.IEnumerable<T> have only a single method which is GetEnumerator() that returns an IEnumerator. IEnumerator provides the power to iterate through the collection by exposing a Current property and Move Next and Reset methods, if we doesn\'t have this interface as a parent so we can\'t use iteration by foreach loop or can\'t use that class object in our LINQ query.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>