# C# Basics

> *Click &#9733; if you like the project. Your contributions are heartily ♡ welcome.*

<br/>

## Related Topics

* *[HTML Basics](https://github.com/learning-zone/html-basics)*
* *[CSS Basics](https://github.com/learning-zone/css-basics)*
* *[React Basics](https://github.com/learning-zone/react-basics)*
* *[SQL Basics](https://github.com/learning-zone/sql-basics)*
* *[ASP.NET Core](asp-net-core.md)*
* *[Entity Framework](entity-framework.md)*

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
* [Performance and Optimization](#-18-performance-and-optimization)
* [Deployment](#-19-deployment)
* [.NET Core](#-20-net-core)
* [Miscellaneous](#-21-miscellaneous)

<br/>

## # 1. FUNDAMENTALS

<br/>

#### Q. What is C# and what are its main features?
#### Q. What is object-oriented programming?
#### Q. What are the different types of data types available in C#?
#### Q. Can primitive data types be stored in heap?
#### Q. Can you store multiple data types in System.Array?
#### Q. What is an extension method in C# and how is it implemented?
#### Q. What is a generic type in C# and why is it used?
#### Q. What is an anonymous method in C# and how is it used?
#### Q. Can you explain the use of the "this" keyword in C#?
#### Q. What is the difference between a value type and a reference type in C#?
#### Q. What is a constructor in C# and what are its different types?
#### Q. What is the difference between a method and a function in C#?
#### Q. What is the Common Language Runtime (CLR) in C#?
#### Q. What is the purpose of namespaces in C#?
#### Q. What is the purpose of the `using` statement in C#?
#### Q. What are properties in C# and how are they used?
#### Q. What are the Arrays in C#.Net?
#### Q. What is the difference between the System.Array.CopyTo() and System.Array.Clone()?
#### Q. What is jagged array in C#.Net?
#### Q. It is possible to store mixed datatypes such as int, string, float, char all in one array?
#### Q. How can you sort the elements of the array in descending order?
#### Q. What is a `property` in C#?
#### Q. What is the difference between `var` and `dynamic` types?
#### Q. What is a `struct` in C#?
#### Q. What is the difference between `abstract` and `virtual` methods?
#### Q. What is the difference between `out` and `ref` parameters?
#### Q. What is the purpose of the `nameof` operator in C#?
#### Q. What is the difference between `checked` and `unchecked` contexts?
#### Q. What is a `generic` in C#?
#### Q. What is a `Tuple` in C#?
#### Q. What is a namespace in C#?
#### Q. How are primitive and objects stored in memory?
#### Q. Explain byval and byref?
#### Q. Differentiate between copy byvalue and copy byref?
#### Q. What is NuGet?
#### Q. What is an immutable string?
#### Q. What is the JIT compiler process?
#### Q. Explain the characteristics of value-type variables that are supported in the C# programming language.
#### Q. What is a parameter? Explain the new types of parameters introduced in C# 4.0.
#### Q. Briefly explain the characteristics of reference-type variables that are supported in the C# programming language.
#### Q. What are the different types of literals?
#### Q. What is the main difference between sub-procedure and function?
#### Q. How to use extension methods?
#### Q. What is the difference between string and StringBuilder in C#?
#### Q. What is difference between late binding and early binding in C#?
#### Q. What is Indexer in C# .Net?
#### Q. What are the differences between Object, Var and Dynamic type?
#### Q. What is the difference between `IEnumerable` and `IQueryable`?
#### Q. What is the difference between managed and unmanaged code?
#### Q. What is Expression Trees In C#?
#### Q. What is an Object Pool in .Net?
#### Q. What are generics in C#.net?
#### Q. What is Serialization?
#### Q. Can you explain the difference between lazy and eager evaluation in C#?
#### Q. Mention the two major categories that distinctly classify the variables of C# programs.
#### Q. What is checked block and unchecked block?
#### Q. What is the difference between typeOf() and sizeOf()?
#### Q. What is widening and Narrowing?
#### Q. How to view an Assembly?
#### Q. What are MultiLingual Applications?
#### Q. What is the use of Codesnippets?
#### Q. What are dynamic type variables in C#? 
#### Q. Can you describe the process of code compilation in .NET?
#### Q. What is the difference between ToString() and Convert.ToString()?
#### Q. What is the difference between int.Parse() and Convert.ToInt32()?
#### Q. What is the use of Code Snippets?
#### Q. Write a program to get the range of Byte Datatype?
#### Q. What are attributes in C# and how can they be used?
#### Q. What are property Accessors?
#### Q. Why can't you specify the accessibility modifier for methods inside the interface?
#### Q. Can you return multiple values from a function in C#?
#### Q. In how many ways you can pass parameters to a method?
#### Q. What are Indexers in C#?
#### Q. How do you implement a custom IComparer<T> for sorting in C#?
#### Q. How can you improve string concatenation performance in C#?
#### Q. How do you work with parallelism and concurrency in C#?
#### Q. What is an indexer in C#?
#### Q. How are custom controls added to an application in C#?
#### Q. What is the use of conditional preprocessor directive in C#?
#### Q. What are pointer types in C#?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 2. OPERATORS

<br/>

#### Q. How you would use a bitwise operator in C#? 
#### Q. Explain the use of the `as` operator in C# and the best way to use it?
#### Q. What is the use of Null Coalescing Operator (??) in C#? 
#### Q. Can you give an example of how to use the is operator and the as operator with inheritance in C#?
#### Q. Difference between "is" and "as" operator in C#.
#### Q. What are nullable types in C#?
#### Q. What is Type Casting and what are its types in C#?
#### Q. What are operators in C# and can you provide examples?
#### Q. What is the difference between `==` operator and `.Equals()` method?
#### Q. What is the purpose of the `var` keyword in C#?
#### Q. What are the differences between `const` and `readonly` keywords?
#### Q. How does `checked` and `unchecked` context affect arithmetic operations?
#### Q. What is short-circuit evaluation in C#?
#### Q. List some different ways for equality check in .Net?
#### Q. How to get the sizeof a datatype in C#?
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
#### Q. What is boxing and unboxing?
#### Q. Is boxing unboxing good or bad?
#### Q. Can we avoid boxing and unboxing?
#### Q. What effect does boxing and unboxing have on performance?
#### Q. Differentiate between Boxing and Unboxing.
#### Q. Explain casting, implicit casting and explicit casting ?
#### Q. What can happen during explicit casting ?
#### Q. What is the difference between `explicit` and `implicit` conversions?
#### Q. What is the difference between `==` and `ReferenceEquals` in C#?
#### Q. Explain var and dynamic?
#### Q. What is the difference between constant and readonly in C#?
#### Q. What is the purpose of the `is` and `as` operators in C#?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 3. CLASSES

<br/>

#### Q. How are static constructors executed in Parent child?
#### Q. If a base class has a number of overloaded constructors, and an inheriting class has a number of overloaded constructors; can you enforce a call from an inherited constructor to a specific base constructor?
#### Q. If a base class has a bunch of overloaded constructors, and an inherited class has another bunch of overloaded constructors, can you enforce a call from an inherited constructor to an arbitrary base constructor?
#### Q. How to access the constructors of one class to another class?
#### Q. What is the use of static constructors?
#### Q. Can you explain the difference between a static and an instance method in C#?
#### Q. What is an abstract class in C# and when is it used?
#### Q. What is the difference between an abstract class and an interface?
#### Q. What is reflection in .NET and how would you use it?
#### Q. When to use reflection and when not in C#?
#### Q. What is a sealed class in C# and why is it used?
#### Q. What are the benefits of using a sealed class in C#?
#### Q. Is it possible for a sealed class to be used as a base class in C#?
#### Q. Is it possible for a sealed class in C# to define virtual methods?
#### Q. Is it possible for a non-child class to define sealed methods in C#? 
#### Q. How can abstract classes be used to implement the Template Method design pattern?
#### Q. Can you give an example of how abstract classes can lead to implementation of anti-patterns such as the God Object pattern?
#### Q. How can abstract classes be used to implement the Factory Method pattern?
#### Q. Can you explain the SOLID principle of Single Responsibility and how it relates to abstract classes in C#?
#### Q. How can abstract classes be used to implement the Dependency Inversion principle?
#### Q. Can you explain the SOLID principle of Liskov Substitution and how it relates to abstract classes in C#?
#### Q. What is the difference between an abstract class and a concrete class in C#, and how does this relate to the Open-Closed principle?
#### Q. Can you give an example of how abstract classes can help to enforce the Interface Segregation principle?
#### Q. How can abstract classes be used to implement the Strategy design pattern?
#### Q. Is it possible to declare abstract methods as private in C#?
#### Q. Is it possible for an abstract class to contain a main method in C#?
#### Q. Is it possible in C# that a class inherit from multiple abstract classes?
#### Q. What is the difference between a sealed class and an unsealed class in C#?
#### Q. How can you use a sealed class to prevent inheritance in C#?
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
#### Q. When you inherit a protected class-level variable, who is it available to?
#### Q. Are private class-level variables inherited?
#### Q. What is the top .NET class that everything is derived from?
#### Q. When do you absolutely have to declare a class as abstract (as opposed to free-willed educated choice or decision based on UML diagram)?
#### Q. Is it namespace class or class namespace?
#### Q. What is the default Access Modifier for the members of the class?
#### Q. How to Call the Default constructor of one class with the parameterized constructor of same class?
#### Q. What is scope of a Protected Internal member variable of a C# class?    
#### Q. What is the difference between Virtual method and Abstract method? 
#### Q. What is scope of a Internal member variable of a C# class? 
#### Q. How would you implement multiple interfaces with the same method name in the same class?
#### Q. How can you create a derived class object from a base class?
#### Q. Which is the parent class of all classes which we create in C#?
#### Q. What is the base class in .NET framework from which all the classes have been developed?
#### Q. How do you implement a custom attribute in C#?
#### Q. What is the  System. String and System.Text.StringBuilder classes?
#### Q. What are I/O classes in C#? 
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
#### Q. Explain weak and strong references?
#### Q. How can you use Interfaces in C# to reduce coupling and improve maintainability?
#### Q. What happens if the inherited interfaces have conflicting method names?
#### Q. What is the purpose of the `volatile` keyword in C#?
#### Q. What is the purpose of the `checked` keyword in C#?
#### Q. What is the purpose of the `this` keyword in C#?
#### Q. What is the purpose of the `base` keyword in C#?
#### Q. What is the purpose of the `default` keyword in C#?
#### Q. What is the purpose of the `params` keyword in C#?
#### Q. What is the purpose of the `yield` keyword in C#?
#### Q. What is the `async` and `await` keyword in C#?
#### Q. What is the difference between `override` and `new` keywords?
#### Q. Why do we need the out keyword ?
#### Q. What is the params keyword in C#?
#### Q. What is the purpose of the yield keyword in C#? Provide an example of using it with an iterator?
#### Q. What is the "yield" keyword used for in C#? 
#### Q. What is the importance of "this" keyword?
#### Q. What is the difference between ref and out keywords?
#### Q. What is a `partial` class in C#?
#### Q. What is a `static` class in C#?
#### Q. What is a `sealed` class in C#?
#### Q. What are the different types of classes in C#? 
#### Q. What is an Abstract Class? 
#### Q. Are private class members inherited to the derived class?
#### Q. What are partial classes?
#### Q. What is the difference between a struct and a class in C#?
#### Q. What is the difference between `public`, `private`, `protected`, and `internal` access modifiers?
#### Q. C# provides a default constructor for me. I write a constructor that takes a string as a parameter, but want to keep the no parameter one. How many constructors should I write?
#### Q. Why do we need constructors?
#### Q. In parent child which constructor fires first?
#### Q. What is the Constructor Chaining in C#?
#### Q. What are the different Ways of method can be overloaded?
#### Q. When and why to use method overloading?
#### Q. Is operator overloading supported in C#?
#### Q. Can "this" be used within a static method? 
#### Q. Can you declare an override method to be static if the original method is not static?
#### Q. Can you declare the override method static while the original method is non-static?
#### Q. Why can you have only one static constructor?
#### Q. When does static constructor fires?
#### Q. When the static constructor will be called?
#### Q. Can we declare Public access modifier for static constructor?
#### Q. If we declare Main() and static constructor in the same class which one will be called first?
#### Q. How does dependency inversion benefit, show with an example?
#### Q. Will only Dependency inversion solve decoupling problem?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 4. ENCAPSULATION

<br/>

#### Q. What is encapsulation in C# and why is it important?
#### Q. How do you implement encapsulation in C#?
#### Q. Can you provide an example of encapsulation in C#?
#### Q. What are the benefits of using encapsulation in object-oriented programming?
#### Q. How does encapsulation help in maintaining code?
#### Q. What is the difference between encapsulation and abstraction in C#?
#### Q. How can encapsulation be violated in C#?
#### Q. How do access modifiers (public, private, protected, internal) relate to encapsulation in C#?
#### Q. Explain method hiding?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 5. INHERITANCE

<br/>

#### Q. What is inheritance in C# and how does it work?
#### Q. What is the difference between single inheritance and multiple inheritance in C#?
#### Q. How can you use the "base" keyword in C# to call the base class constructor?
#### Q. Can you explain polymorphism and how it is achieved in C#?
#### Q. How can you use inheritance and polymorphism together to achieve dynamic dispatch in C#?
#### Q. What is the purpose of the base keyword in C#, and how is it used in inheritance?
#### Q. What are the differences between virtual methods and abstract methods in C#, and when each should be used?
#### Q. How do you handle constructor inheritance in C#, and what is the role of the base keyword in constructor inheritance?
#### Q. What is the difference between inheritance and composition in C# and when should you use each one?
#### Q. Can you override private virtual methods?
#### Q. What does the keyword virtual mean in the method definition?
#### Q. What is a Virtual Method in C#?
#### Q. Why a private virtual method cannot be overridden in C#?
#### Q. How do you override a method in C#?
#### Q. What is the difference between `override` and `new` keywords in C#?
#### Q. How does inheritance promote code reusability?
#### Q. What are the potential pitfalls of using inheritance?
#### Q. How do you prevent a class from being inherited in C#?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 6. COLLECTIONS

<br/>

#### Q. What are collections in C# and why are they used?
#### Q. What is the difference between an array and a collection in C#?
#### Q. What are the different types of collections available in C#?
#### Q. What are Concurrent Collection Classes?
#### Q. Explain Hashtable in C#?
#### Q. What is IEnumerable<> in C#?
#### Q. What is the difference between a BlockingCollection and a ConcurrentQueue or ConcurrentStack? In which scenarios would you choose to use the BlockingCollection, and why?
#### Q. Explain the difference between IQueryable, ICollection, IList & IDictionary interfaces? 
#### Q. What is difference between Hashtable and Dictionary?
#### Q. What is difference between SortedList and SortedDictionary in C#?
#### Q. What is the difference between Array and ArrayList?
#### Q. Whose performance is better array or arraylist?
#### Q. What class is underneath the Sorted List class?
#### Q. IEnumerable vs List - What to Use? How do they work? 
#### Q. When to use ArrayList over array[] in C#?
#### Q. What are the differences between IEnumerable and IQueryable?
#### Q. How to sort the generic SortedList in the descending order?
#### Q. What is a `HashSet` and when would you use it?
#### Q. How is LinkedList used in C#?
#### Q. What is difference between Stack and Heap?
#### Q. Are string allocated on stack or heap?
#### Q. How many stack and heaps are created for an application?
#### Q. How are stack and heap memory deallocated?
#### Q. Who clears the heap memory?
#### Q. Where is structure allocated Stack or Heap?
#### Q. Can structures get created on Heap?
#### Q. Is there a way we can see this Heap memory?
#### Q. Explain stack and Heap?
#### Q. Where are stack and heap stored?
#### Q. What goes on stack and what goes on heap?
#### Q. How is the stack memory address arranged?
#### Q. How is stack memory deallocated LIFO or FIFO?
#### Q. How do you choose between `List<T>` and `ArrayList` in C#?
#### Q. How do you iterate over a collection in C#?
#### Q. What is the difference between `IEnumerable` and `ICollection` in C#?
#### Q. How do you sort a collection in C#?
#### Q. How do you remove duplicates from a collection in C#?
#### Q. What is the difference between `Queue` and `Stack` in C#?
#### Q. How do you convert an array to a collection in C#?
#### Q. What are the thread-safe collection classes available in C#?
#### Q. How do you use LINQ with collections in C#?
#### Q. What is the difference between `Array` and `List` in C#?
#### Q. What is the difference between Array and Collections?
#### Q. What are generic collections?
#### Q. How can you optimize memory usage and performance when working with large data collections in C#?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 7. MULTITHREADING

<br/>

#### Q. What is multithreading in C# and why is it important?
#### Q. How do you create a new thread in C#?
#### Q. What is the difference between a thread and a process?
#### Q. How do you synchronize threads in C#?
#### Q. What is the `ThreadPool` class and how is it used?
#### Q. What are `Task` and `async/await` in C#?
#### Q. How do you handle exceptions in multithreaded applications?
#### Q. What is a deadlock and how can it be avoided?
#### Q. What is the purpose of the `lock` statement in C#?
#### Q. What is the difference between Monitor and lock in C#?
#### Q. How do you use the `Monitor` class for thread synchronization?
#### Q. What is the difference between `Thread.Join` and `Thread.Sleep`?
#### Q. How do you implement a producer-consumer scenario in C#?
#### Q. What is the `CancellationToken` and how is it used in multithreading?
#### Q. How do you use `Concurrent` collections in C#?
#### Q. What is the difference between `Parallel.For` and `Task.Run`?
#### Q. What are the advantages and disadvantages of multithreading?
#### Q. Why does a delegate need to be passed as a parameter to the Thread class constructor?
#### Q. How to pass the parameter in Thread?
#### Q. Why do we need a ParameterizedThreadStart delegate?
#### Q. When to use ParameterizedThreadStart over ThreadStart delegate?
#### Q. How to pass data to the thread function in a type-safe manner?
#### Q. How to retrieve the data from a thread function?
#### Q. What is the difference between Threads and Tasks?
#### Q. What is the Significance of Thread.Join and Thread.IsAlive functions in multithreading?
#### Q. What happens if shared resources are not protected from concurrent access in a multithreaded program?
#### Q. How do protect shared resources from concurrent access?
#### Q. Which option is better?
#### Q. What are the Interlocked functions?
#### Q. What is AutoResetEvent and how it is different from ManualResetEvent?
#### Q. What is the Semaphore?
#### Q. What is Mutex and how it is different from other Synchronization mechanisms?
#### Q. What is the Race condition?
#### Q. What is the volatile keyword?
#### Q. How can you share data between multiple threads?
#### Q. What is synchronization and why it is important?
#### Q. Can you count some names of Synchronization primitives?
#### Q. What are the four necessary conditions for Deadlock?
#### Q. What is LiveLock?
#### Q. In the context of C# multithreading, what are the main differences between the ThreadPool and creating your own dedicated Thread instances?
#### Q. How can you ensure mutual exclusion while accessing shared resources in C# multithreading without using lock statement or Monitor methods?
#### Q. Explain the difference between the Barrier and CountdownEvent synchronization primitives in multithreading. Provide a real-world scenario in which each of these would be useful.
#### Q. What would be the potential issues with using Thread.Abort() to terminate a running thread? Explain the implications and suggest alternative methods for gracefully stopping a thread.
#### Q. How do you achieve thread synchronization using ReaderWriterLockSlim in C# multithreading? Explain its advantages over traditional ReaderWriterLock.
#### Q. How do Tasks in C# differ from traditional Threads? Explain the benefits and scenarios where Tasks would be preferred over directly spawning Threads.
#### Q. Discuss the differences between Volatile, Interlocked, and MemoryBarrier methods for using shared variables in multithreading. When should each of these be used?
#### Q. In C# multithreading, explain the concept of thread-local and data partitioning and how it can help improve the overall performance of a multi-threaded application.
#### Q. How does the Cancellation model in a Task Parallel Library work? Explain how you can use CancellationToken to handle cancellations in TPL.
#### Q. How do you combine asynchronous programming with multithreading using C#\'s async/await pattern? Explain how the TaskScheduler class can be used in this context.
#### Q. What is parallelism, and how do you control the degree of parallelism for a parallel loop in C# using the Parallel class?
#### Q. Describe the concept of lock contention in multithreading and explain its impact on the performance of your application. How can you address and mitigate lock contention issues?
#### Q. How do Tasks in C# differ from traditional Threads? Explain the benefits and scenarios where Tasks would be preferred over directly spawning Threads.
#### Q. Describe the concept of a race condition in C# multithreading and explain different strategies for preventing race conditions from occurring in your application.
#### Q. What is lazy initialization in C# multithreading and how does it affect application start-up performance? 
#### Q. How do you achieve thread synchronization using ReaderWriterLockSlim in C# multithreading? Explain its advantages over traditional ReaderWriterLock.
#### Q. What are the differences between ManualResetEvent and AutoResetEvent synchronization primitives in C# multithreading?
#### Q. Explain the use of SpinLock in C# multithreading and how it differs from a standard lock or Monitor. Describe the potential advantages and limitations of using SpinLocks.
#### Q. What is Multithreading with .NET?
#### Q. How can you implement multithreading in C#?
#### Q. When should multithreading be used and when should it be avoided in C#?
#### Q. What is difference between lock and interlock in Multithreading C#?
#### Q. How does multithreading improve the performance over a single thread solution?
#### Q. How do you handle deadlocks in multi-threaded C# applications?
#### Q. What is the difference between `lock` and `Monitor` in C#?
#### Q. What is a `thread` in C#?
#### Q. What is the purpose of the `ThreadPool` class in C#?
#### Q. What is the difference between `Task.Run` and `Task.Factory.StartNew`?
#### Q. What is the difference between `Task` and `Thread` in C#?
#### Q. What is the purpose of the `lock` statement in C#?
#### Q. Explain the concept of threading in .NET.
#### Q. What is Thread Pooling in C#?
#### Q. What are the different states of a Thread in C#?
#### Q. What is the difference between Task and Thread in C#?
#### Q. What is the lock statement in C#?
#### Q. How are threads different from TPL ?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 8. FILE HANDLING

<br/>

#### Q. What is File Handling in C#.Net?
#### Q. How do you read a file in C#?
#### Q. How do you write to a file in C#?
#### Q. What is the difference between `File.ReadAllText` and `File.ReadAllLines` in C#?
#### Q. How do you append text to an existing file in C#?
#### Q. How do you check if a file exists in C#?
#### Q. What is the purpose of the `StreamReader` and `StreamWriter` classes in C#?
#### Q. How do you handle exceptions when working with files in C#?
#### Q. How do you delete a file in C#?
#### Q. What is the difference between `FileStream` and `MemoryStream` in C#?
#### Q. How do you copy a file in C#?
#### Q. How do you move a file in C#?
#### Q. What is the `FileInfo` class and how is it used?
#### Q. How do you read and write binary files in C#?
#### Q. How do you work with directories in C#?
#### Q. How do you get the size of a file in C#?
#### Q. What are DLL files, and what are the advantages of using them?
#### Q. What is a `MemoryStream` in C#?
#### Q. What is the purpose of the `FileStream` class in C#?
#### Q. What is the difference between `StreamReader` and `StreamWriter`?
#### Q. What is a `BinaryReader` in C#?
#### Q. What is the purpose of the `BinaryWriter` class in C#?
#### Q. What is the difference between `TextReader` and `TextWriter`?
#### Q. What is a `StringReader` in C#?
#### Q. What is the purpose of the `StringWriter` class in C#?
#### Q. What is the difference between `XmlReader` and `XmlWriter`?
#### Q. What is a `JsonReader` in C#?
#### Q. What is the purpose of the `JsonWriter` class in C#?
#### Q. What is the difference between `DataContractSerializer` and `XmlSerializer`?
#### Q. What is a `BinaryFormatter` in C#?
#### Q. What is the purpose of the `SoapFormatter` class in C#?
#### Q. What is the difference between `BinaryFormatter` and `SoapFormatter`?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 9. REGULAR EXPRESSION

<br/>

## Regular Expression

<br/>

#### Q. What is a regular expression in C# and what is it used for?
#### Q. How do you create a regular expression in C#?
#### Q. What is the purpose of the `Regex` class in C#?
#### Q. How do you match a pattern in a string using regular expressions in C#?
#### Q. How do you replace text in a string using regular expressions in C#?
#### Q. What are some common use cases for regular expressions in C#?
#### Q. How do you validate an email address using regular expressions in C#?
#### Q. What is the difference between `Regex.Match` and `Regex.IsMatch` in C#?
#### Q. How do you extract groups from a match using regular expressions in C#?
#### Q. How do you handle case sensitivity in regular expressions in C#?
#### Q. How to create precompiled Regex object in .Net?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 10. EXCEPTION HANDLING

<br/>

#### Q. What is exception handling in C# and why is it important?
#### Q. How do you handle exceptions in C#?
#### Q. What is the difference between `try`, `catch`, and `finally` blocks in C#?
#### Q. How do you create a custom exception in C#?
#### Q. What is the purpose of the `throw` keyword in C#?
#### Q. How do you rethrow an exception in C#?
#### Q. What is the difference between `throw` and `throw ex` in C#?
#### Q. How do you handle multiple exceptions in a single `catch` block in C#?
#### Q. What is the `Exception` class in C# and what are its key properties?
#### Q. How do you log exceptions in C#?
#### Q. What is the `InnerException` property in C#?
#### Q. How do you use the `using` statement to handle exceptions in C#?
#### Q. What is the difference between checked and unchecked exceptions in C#?
#### Q. How do you handle exceptions in asynchronous methods in C#?
#### Q. What is the `AggregateException` class in C# and when is it used?
#### Q. What is difference between Throw Exception and Throw Clause.
#### Q. How do you handle exceptions in a method that returns a Task?
#### Q. Which class acts as a base class for all exceptions in C#?
#### Q. Will the finally block get executed if an exception has not occurred?
#### Q. What is the C# syntax to catch any possible exception?
#### Q. What is the difference between System.ApplicationException class and System.SystemException class? 
#### Q. In try block if we add return statement whether finally block is executed in C#? 
#### Q. What is the difference between StackOverflowError and OutOfMemoryError? 
#### Q. Name different types errors which can occur during the execution of a program?
#### Q. Explain the difference between "finally" and "finalize block"?
#### Q. What is the difference between "throw" and "throw ex"
#### Q. Can Multiple Catch Blocks executed in C#?
#### Q. What is the role of System.Exception class?
#### Q. List down the most commonly used types of exceptions in .NET?
#### Q. What is the difference between `throw` and `throw new` in C#?
#### Q. What are the different ways to handle errors in C#?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 11. EVENTS AND DELEGATES

<br/>

#### Q. What are Events?
#### Q. What are delegates in C# and why are they used?
#### Q. What is the difference between a delegate and an event in C#?
#### Q. How do you declare and raise an event in C#?
#### Q. What is the purpose of the `EventHandler` delegate in C#?
#### Q. How do you subscribe to and unsubscribe from an event in C#?
#### Q. What is a multicast delegate in C#?
#### Q. How do you use anonymous methods with delegates in C#?
#### Q. What are the advantages of using events and delegates in C#?
#### Q. How do you use lambda expressions with delegates in C#?
#### Q. What is the difference between `Action`, `Func`, and `Predicate` delegates in C#?
#### Q. How do you implement custom event accessors in C#?
#### Q. What is the `event` keyword in C# and how is it used?
#### Q. How do you handle events in a derived class in C#?
#### Q. What are the best practices for using events and delegates in C#?
#### Q. What is an event in C# and how does it work?
#### Q. What is an event in C# and how is it different from a delegate?
#### Q. What is a delegate in C# and how is it related to events?
#### Q. What are called combinable delegates?
#### Q. How to use delegates with Events? 
#### Q. What is the difference between Event and Method?
#### Q. What is the difference between lambdas and delegates? 
#### Q. What is the difference between Func<string,string> and delegate? 

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 12. GARBAGE COLLECTION

<br/>

#### Q. What is garbage collection in .NET?
#### Q. Explain the role of the garbage collector in .NET.
#### Q. How do you manage memory in .NET applications?
#### Q. How does garbage collection work in .NET and how can you optimize it?
#### Q. How does the garbage collector know when to clean up objects?
#### Q. Does the garbage collector clean up primitive types?
#### Q. What are Generation 0, Generation 1, and Generation 2 in garbage collection?
#### Q. How does the garbage collector behave when a class has a destructor?
#### Q. Explain the importance of the garbage collector.
#### Q. Can the garbage collector reclaim unmanaged objects?
#### Q. Can the garbage collector clean up unmanaged code?
#### Q. Can you force garbage collection in .NET?
#### Q. Is it a good practice to force garbage collection?
#### Q. How do you detect memory leaks in .NET applications?
#### Q. What is GC0, GC1, and GC2?
#### Q. How does GC behave when we have a destructor?
#### Q. What is a `Mutex` in C#?
#### Q. What is the purpose of the `Semaphore` class in C#?
#### Q. What is the difference between `Semaphore` and `SemaphoreSlim`?
#### Q. What is a `deadlock` in C#?
#### Q. What is the purpose of the `Interlocked` class in C#?
#### Q. What is the difference between `Task` and `ValueTask`?
#### Q. What is a `CancellationToken` in C#?
#### Q. What is the purpose of the `CancellationTokenSource` class in C#?
#### Q. What is the difference between `Task.WhenAll` and `Task.WhenAny`?
#### Q. What is a `ConcurrentDictionary` in C#?
#### Q. What is the purpose of the `BlockingCollection` class in C#?
#### Q. What is the difference between `ConcurrentBag` and `ConcurrentQueue`?
#### Q. What is a `MemoryCache` in C#?
#### Q. What is the purpose of the `Lazy` class in C#?
#### Q. What is the difference between `Lazy` and `Lazy<T>`?
#### Q. What is a `WeakReference` in C#?
#### Q. What is the purpose of the `GC.Collect` method in C#?
#### Q. What is the difference between `GC.Collect` and `GC.WaitForPendingFinalizers`?
#### Q. What is a `finalizer` in C#?
#### Q. What is the purpose of the `GC.SuppressFinalize` method in C#?
#### Q. What is the difference between `GC.KeepAlive` and `GC.ReRegisterForFinalize`?
#### Q. What is garbage collection and how does it work in .NET Core?
#### Q. How do you optimize garbage collection in .NET Core?
#### Q. What is the difference between server and workstation garbage collection?
#### Q. How do you use the `GCSettings` class to configure garbage collection?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 13. LAMBDA EXPRESSIONS

<br/>

#### Q. What is a lambda expression in C# and why is it used?
#### Q. How do you declare a lambda expression in C#?
#### Q. What is the difference between a lambda expression and an anonymous method in C#?
#### Q. How do you use lambda expressions with LINQ in C#?
#### Q. Can you provide an example of a simple lambda expression in C#?
#### Q. What are the advantages of using lambda expressions in C#?
#### Q. How do you capture variables in a lambda expression in C#?
#### Q. What is the difference between statement lambdas and expression lambdas in C#?
#### Q. How do you use lambda expressions with delegates in C#?
#### Q. What are the limitations of lambda expressions in C#?
#### Q. How do you handle exceptions in lambda expressions in C#?
#### Q. Can you use async/await with lambda expressions in C#?
#### Q. How do you use lambda expressions in event handling in C#?
#### Q. What is the syntax for a lambda expression with multiple parameters in C#?
#### Q. How do you use lambda expressions with generic types in C#?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 13. LINQ

<br/>

#### Q. What is LINQ in C# and why is it used?
#### Q. What are the main advantages of using LINQ in C#?
#### Q. How do you write a basic LINQ query in C#?
#### Q. What is the difference between LINQ to Objects, LINQ to SQL, and LINQ to XML?
#### Q. Can you provide an example of a LINQ query that filters and sorts data?
#### Q. What are the different types of LINQ operators in C#?
#### Q. How do you use the `Select` and `Where` operators in LINQ?
#### Q. What is the purpose of the `GroupBy` operator in LINQ?
#### Q. How do you perform a join operation using LINQ?
#### Q. What is the difference between deferred execution and immediate execution in LINQ?
#### Q. How do you use the `Aggregate` operator in LINQ?
#### Q. What is the `Let` keyword in LINQ and how is it used?
#### Q. How do you handle null values in LINQ queries?
#### Q. What are anonymous types in LINQ and how are they used?
#### Q. How do you use LINQ with collections in C#?
#### Q. How do you optimize LINQ queries for performance?
#### Q. Explain the differences between Parallel.ForEach and PLINQ (Parallel LINQ)?
#### Q. How does LINQ's "Where" method works?
#### Q. What are the benefits of a Deferred Execution in LINQ?  
#### Q. Can you explain the difference between a query expression and a method chain in LINQ?
#### Q. Can you give an example of using LINQ to filter data in a collection?
#### Q. How can LINQ be used to perform grouping and aggregation operations on data?
#### Q. How does the Single Responsibility Principle (SRP) apply to LINQ code?
#### Q.  Can you demonstrate the use of LINQ to implement the Open-Closed Principle (OCP)?
#### Q. How does the Liskov Substitution Principle (LSP) apply to LINQ code?
#### Q. Can you give an example of using LINQ to implement the Interface Segregation Principle (ISP)?
#### Q. Can you explain the Dependency Inversion Principle (DIP) and how it relates to LINQ code?
#### Q. Explain the difference between Select and Where?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 15. MICROSERVICES

<br/>

#### Q. What are microservices and why are they used?
#### Q. How do you implement microservices in .NET Core?
#### Q. What are the main advantages of using microservices architecture?
#### Q. How do you handle communication between microservices in .NET Core?
#### Q. What is the role of API Gateway in microservices architecture?
#### Q. How do you manage data consistency in microservices?
#### Q. What are some common challenges when working with microservices?
#### Q. How do you deploy microservices in a containerized environment?
#### Q. What is the purpose of service discovery in microservices?
#### Q. How do you implement logging and monitoring in microservices?
#### Q. What is the difference between monolithic and microservices architecture?
#### Q. How do you handle security in microservices?
#### Q. What is the role of Docker in microservices?
#### Q. How do you implement resilience and fault tolerance in microservices?
#### Q. What are some best practices for designing microservices?
#### Q. Name the key components of Microservices?
#### Q. What are the tools commonly used tools for Microservices?
#### Q. What are the key principles to follow when designing microservices?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 16. UNIT TESTS

<br/>

#### Q. What is unit testing and why is it important for writing high-quality code?
#### Q. How do you write a unit test in C#?
#### Q. What is the difference between unit testing and integration testing?
#### Q. What frameworks are commonly used for unit testing in C#?
#### Q. What is the purpose of mocking in unit testing?
#### Q. How do you use Moq framework for mocking in C#?
#### Q. What are the best practices for writing unit tests?
#### Q. How do you run unit tests in Visual Studio?
#### Q. What is code coverage and how do you measure it?
#### Q. How do you handle dependencies in unit tests?
#### Q. What is the Arrange-Act-Assert (AAA) pattern in unit testing?
#### Q. What is the difference between `TestInitialize` and `ClassInitialize` in MSTest?
#### Q. How do you use data-driven tests in C#?
#### Q. Can you explain the different types of unit tests that can be performed in C#?
#### Q. How do you use the xUnit framework to write and run unit tests in C#?
#### Q. Can you explain how to write a test case for checking the exception handling in C#?
#### Q. How do you use mock objects to test code in isolation from its dependencies in C#?
#### Q. Can you explain the difference between unit tests and integration tests, and when to use each type of test?
#### Q. Can you describe the process of test-driven development (TDD) and how it can be applied to C# development?
#### Q. How do you measure the code coverage of your unit tests, and what is considered an acceptable level of code coverage in C#?
#### Q. How to use dependency injection to make code more testable in C#?
#### Q. What is xUnit and why is it used for unit testing in C#?
#### Q. How do you write a basic unit test using xUnit?
#### Q. What is the difference between xUnit and other testing frameworks like MSTest and NUnit?
#### Q. How do you set up xUnit in a .NET Core project?
#### Q. How do you use the `[Fact]` attribute in xUnit?
#### Q. What is the purpose of the `[Theory]` attribute in xUnit?
#### Q. How do you use `InlineData` with `[Theory]` in xUnit?
#### Q. How do you handle test initialization and cleanup in xUnit?
#### Q. What is the role of `Assert` class in xUnit?
#### Q. How do you run xUnit tests from the command line?
#### Q. How do you use dependency injection in xUnit tests?
#### Q. How do you mock dependencies in xUnit tests?
#### Q. How do you test asynchronous methods using xUnit?
#### Q. How do you group and order tests in xUnit?
#### Q. What are some best practices for writing xUnit tests?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 17. DESIGN PATTERNS

<br/>

#### Q. What are design patterns and why are they important in software development?
#### Q. Can you explain the Singleton pattern and provide an example in C#?
#### Q. What is the Factory Method pattern and how is it implemented in C#?
#### Q. How does the Observer pattern work and when would you use it?
#### Q. What is the difference between the Adapter and Decorator patterns?
#### Q. How do you implement the Strategy pattern in C#?
#### Q. What is the purpose of the Dependency Injection pattern?
#### Q. Can you explain the Repository pattern and its benefits?
#### Q. How does the Command pattern work and when should it be used?
#### Q. What is the difference between the Prototype and Builder patterns?
#### Q. Can you provide an example of the Template Method pattern in C#?
#### Q. How does the State pattern help in managing object states?
#### Q. What are the benefits of using the Composite pattern?
#### Q. Can you explain the Open-Closed Principle in C#, and provide an example of how it can be implemented in code?
#### Q. Can you explain the SOLID design principles and how they relate to writing maintainable code?
#### Q. Can you explain the Single Responsibility Principle in your own words, and how it applies to C# code?
#### Q. How would you refactor C# code that is tightly coupled between multiple classes?
#### Q. How do you avoid Primitive Obsession when designing C# classes, and what are the benefits of doing so?
#### Q. What are design patterns in C# and why are they important?
#### Q. Can you describe the difference between Structural and Behavioral design patterns in C#, and provide an example of each?
#### Q. What is the difference between a design pattern and a Architecture Pattern?
#### Q. Can you please tell me what is the Observer Pattern in C# and how does it work?
#### Q. How can you use the Decorator Pattern in C# to extend the behavior of an object dynamically?
#### Q. Can you explain the use of the builder pattern for constructing complex objects?
#### Q. What is the Prototype pattern in C# and how does it differ from the Factory Method pattern?
#### Q. How can you use the Facade pattern in C# to simplify a complex system?
#### Q. What is the adaptor pattern in C# and how is it used to integrate with existing systems?
#### Q. What is the Flyweight pattern in C# and how is it used to optimize memory usage?
#### Q. Can you explain the use of the Bridge pattern in C# to separate the abstraction from its implementation?
#### Q. What is the Chain of Responsibility pattern in C# and how is it used to handle events in a loosely coupled manner?
#### Q. How can you use the Mediator pattern in C# to reduce coupling between objects?
#### Q. What is the Interpreter pattern in C# and how is it used for language interpretation?
#### Q. Explain the Dispose Pattern?
#### Q. What is CQRS pattern in C#?
#### Q. Can you explain LISKOV Principle and it\'s violation?
#### Q. How can we fix LISKOV Problem?
#### Q. Is there a connection between LISKOV and ISP?
#### Q. How can you write code that follows the DRY (Don't Repeat Yourself) principle in C#?
#### Q. What are some common anti-patterns in C# and how can they be avoided to write clean code?
#### Q. Explain SRP with an example?
#### Q. Explain OCP with an example?
#### Q. What is higher level module and lower level module?
#### Q. Why do developers move object creation outside high level module?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 18. PERFORMANCE AND OPTIMIZATION

<br/>

#### Q. What are some common performance issues in .NET Core applications?
#### Q. How do you measure the performance of a .NET Core application?
#### Q. What tools are available for profiling .NET Core applications?
#### Q. How do you use the `dotnet-counters` tool to monitor performance?
#### Q. What is the `dotnet-trace` tool and how is it used?
#### Q. How do you use the `dotnet-dump` tool to collect and analyze memory dumps?
#### Q. What is the `dotnet-gcdump` tool and how is it used?
#### Q. How do you optimize memory usage in .NET Core applications?
#### Q. What are some best practices for managing memory in .NET Core?
#### Q. How do you reduce the memory footprint of a .NET Core application?
#### Q. How do you optimize CPU usage in .NET Core applications?
#### Q. What are some best practices for optimizing CPU-bound operations?
#### Q. How do you use asynchronous programming to improve performance in .NET Core?
#### Q. What is the `async` and `await` keywords and how are they used?
#### Q. How do you use the `Task` class to perform asynchronous operations?
#### Q. How do you optimize I/O operations in .NET Core applications?
#### Q. What are some best practices for optimizing disk and network I/O?
#### Q. How do you use caching to improve performance in .NET Core applications?
#### Q. What is the `IMemoryCache` interface and how is it used?
#### Q. How do you use distributed caching in .NET Core?
#### Q. What is the `IDistributedCache` interface and how is it used?
#### Q. How do you use the `ResponseCaching` middleware to cache responses?
#### Q. How do you optimize database access in .NET Core applications?
#### Q. How do you use connection pooling to improve database performance?
#### Q. How do you use the `AsNoTracking` method to optimize query performance?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 19. DEPLOYMENT

<br/>

#### Q. How do you deploy a .NET Core application to Azure?
#### Q. What is the `dotnet publish` command and how is it used?
#### Q. How do you deploy a .NET Core application to IIS?
#### Q. What is the `WebHostBuilder` class and how is it used?
#### Q. What is the `Azure App Service` and how is it used?
#### Q. How do you use Docker to containerize a .NET Core application?
#### Q. How do you create a Dockerfile for a .NET Core application?
#### Q. How do you deploy a .NET Core application using Docker?
#### Q. What is Kubernetes and how is it used with .NET Core applications?
#### Q. How do you use CI/CD pipelines to deploy .NET Core applications?
#### Q. What is the `Azure DevOps` service and how is it used for deployment?
#### Q. How do you configure a build pipeline in Azure DevOps?
#### Q. How do you configure a release pipeline in Azure DevOps?
#### Q. How do you use GitHub Actions for deploying .NET Core applications?
#### Q. What is the `Octopus Deploy` tool and how is it used?
#### Q. What are the best practices for deploying .NET Core applications?
#### Q. What are the different types of hosting models available in .NET Core?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 20. .NET Core

<br/>

#### Q. What is .NET Core?
#### Q. What are the main differences between .NET Framework and .NET Core?
#### Q. What is the .NET Standard?
#### Q. How do you handle configuration in a .NET Core application?
#### Q. What is the .NET Core CLI and how is it used?
#### Q. How do you create a new .NET Core project?
#### Q. What is the purpose of the Program.cs and Startup.cs files in a .NET Core application?
#### Q. How do you identify duplicated code in C#, and what techniques can be used to remove it?
#### Q. What is Kestrel?
#### Q. What is Dependency Injection in .NET Core?
#### Q. How do you handle configuration in .NET Core?
#### Q. How do you add services to the dependency injection container?
#### Q. What is the difference between `IServiceCollection` and `IServiceProvider`?
#### Q. How do you configure logging in .NET Core?
#### Q. How do you perform database migrations in EF Core?
#### Q. What is the difference between `AddTransient`, `AddScoped`, and `AddSingleton`?
#### Q. How do you create a RESTful API in ASP.NET Core?
#### Q. How do you read configuration values from `appsettings.json`?
#### Q. What is the `IConfiguration` interface?
#### Q. What is the `IApplicationBuilder` interface?
#### Q. How do you use middleware in ASP.NET Core?
#### Q. What is the `UseEndpoints` method?
#### Q. How do you create a custom middleware?
#### Q. What is the `ConfigureServices` method?
#### Q. What is the `Configure` method?
#### Q. How do you use dependency injection in controllers?
#### Q. What is the `ControllerBase` class?
#### Q. How do you return JSON from a controller action?
#### Q. What is the `ActionResult` class?
#### Q. What is attribute routing?
#### Q. How do you create a custom route?
#### Q. What is the `HttpGet` attribute?
#### Q. What is the `HttpPost` attribute?
#### Q. How do you handle form submissions in ASP.NET Core?
#### Q. What is model binding?
#### Q. How do you validate models in ASP.NET Core?
#### Q. What is the `ModelState` property?
#### Q. How do you use data annotations for validation?
#### Q. What is the `ViewResult` class?
#### Q. How do you return a view from a controller action?
#### Q. What is Razor syntax?
#### Q. How do you create a Razor view?
#### Q. What is the `_ViewStart.cshtml` file?
#### Q. What is the `_Layout.cshtml` file?
#### Q. How do you use partial views in ASP.NET Core?
#### Q. What is the `ViewComponent` class?
#### Q. How do you create a view component?
#### Q. What is the `TagHelper` class?
#### Q. How do you create a custom tag helper?
#### Q. How do you use the `HtmlHelper` class in a view?
#### Q. What is the `UrlHelper` class?
#### Q. How do you use the `UrlHelper` class in a view?
#### Q. What is the `IActionResult` interface?
#### Q. How do you use the `IActionResult` interface in a controller action?
#### Q. What is the `JsonResult` class?
#### Q. How do you return a JSON result from a controller action?
#### Q. What is the `ContentResult` class?
#### Q. How do you return a content result from a controller action?
#### Q. What is the `FileResult` class?
#### Q. How do you return a file result from a controller action?
#### Q. What is the `RedirectResult` class?
#### Q. How do you return a redirect result from a controller action?
#### Q. What is the `StatusCodeResult` class?
#### Q. How do you return a status code result from a controller action?
#### Q. What is the `ChallengeResult` class?
#### Q. How do you return a challenge result from a controller action?
#### Q. What is the `SignInResult` class?
#### Q. How do you return a sign-in result from a controller action?
#### Q. What is the `SignOutResult` class?
#### Q. How do you return a sign-out result from a controller action?
#### Q. What is the `ForbidResult` class?
#### Q. How do you return a forbid result from a controller action?
#### Q. What is the `LocalRedirectResult` class?
#### Q. How do you return a local redirect result from a controller action?
#### Q. What is the `PartialViewResult` class?
#### Q. How do you return a partial view result from a controller action?
#### Q. What is the `ViewComponentResult` class?
#### Q. How do you return a view component result from a controller action?
#### Q. What is the `PhysicalFileResult` class?
#### Q. How do you return a physical file result from a controller action?
#### Q. What is the `VirtualFileResult` class?
#### Q. How do you return a virtual file result from a controller action?
#### Q. What is the `FileStreamResult` class?
#### Q. How do you return a file stream result from a controller action?
#### Q. What is the `FileContentResult` class?
#### Q. How do you return a file content result from a controller action?
#### Q. What is the `FilePathResult` class?
#### Q. How do you return a file path result from a controller action?
#### Q. What is the `PhysicalFileProvider` class?
#### Q. How do you use the `PhysicalFileProvider` class?
#### Q. What is the `CompositeFileProvider` class?
#### Q. How do you use the `CompositeFileProvider` class?
#### Q. What is the `ManifestEmbeddedFileProvider` class?
#### Q. How do you use the `ManifestEmbeddedFileProvider` class?
#### Q. What is the `EmbeddedFileProvider` class?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 20. MISCELLANEOUS

<br/>

#### Q. What is marshalling and why do we need it?
#### Q. Query Geolocation & proxy data in .NET using IP2Location
#### Q. How to calculate the code execution time in C#?
#### Q. What is the distinction between directcast and ctype?
#### Q. Explain how to implement a custom serializer and deserializer for a complex object in C#?
#### Q. Explain the differences between Memory<T> and Span<T> in C#. When would you use one over the other?
#### Q. What is a DTO?
#### Q. What does POCO mean?
#### Q. Define Parsing? Explain how to Parse a DateTime String?
#### Q. What is IL (Intermediate Language) Code?
#### Q. What is the use of JIT (Just in time compiler)?
#### Q. Is it possible to view IL code?
#### Q. What is the benefit of compiling into IL code?
#### Q. What is the importance of CTS?
#### Q. How are initializers executed?
#### Q. What is Shadowing?

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>
