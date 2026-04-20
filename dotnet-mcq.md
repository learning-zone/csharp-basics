# C# & .NET Scenario-Based MCQ

> Scenario-based multiple choice questions covering core C# and .NET topics.

<br>

## Table of Contents

1. [Fundamentals & Type System](#1-fundamentals--type-system)
2. [Classes, OOP & Inheritance](#2-classes-oop--inheritance)
3. [Generics & Collections](#3-generics--collections)
4. [Delegates, Events & Lambda](#4-delegates-events--lambda)
5. [LINQ](#5-linq)
6. [Async / Await & Multithreading](#6-async--await--multithreading)
7. [Exception Handling](#7-exception-handling)
8. [Memory Management & GC](#8-memory-management--gc)
9. [Interfaces & Patterns](#9-interfaces--patterns)
10. [File I/O & Serialization](#10-file-io--serialization)
11. [ASP.NET Core & Web API](#11-aspnet-core--web-api)
12. [Entity Framework Core](#12-entity-framework-core)
13. [C# Language Features](#13-c-language-features)
14. [Records, Structs & Value Types](#14-records-structs--value-types)
15. [Dependency Injection & Testing](#15-dependency-injection--testing)

<br>

## 1. Fundamentals & Type System

**Q.** A developer writes the following code and is surprised by the output. What does the program print?

```cs
int a = 5;
object b = a;
int c = (int)b;
b = 10;
Console.WriteLine(a + " " + c);
```

- A) `10 10`
- B) `5 5`
- C) `5 10`
- D) Compile error

> **Answer: B**
> Boxing copies the value of `a` into a heap-allocated object. Unboxing copies that value back to `c`. Assigning `b = 10` creates a new box; it does not change `a` or `c`. Both `a` and `c` remain `5`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** What is the output of the following code?

```cs
string s1 = "hello";
string s2 = "hello";
Console.WriteLine(object.ReferenceEquals(s1, s2));
```

- A) `False` — two separate string objects are always created.
- B) `True` — the CLR interns string literals, so both variables point to the same object.
- C) Compile error — `ReferenceEquals` cannot be used with `string`.
- D) `True` — strings are value types.

> **Answer: B**
> The CLR string intern pool ensures that identical string literals share the same object at runtime. `object.ReferenceEquals` therefore returns `true` for two variables holding the same literal.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A team member writes this code expecting it to swap two integers:

```cs
void Swap(int x, int y) { int t = x; x = y; y = t; }
int a = 1, b = 2;
Swap(a, b);
Console.WriteLine(a + " " + b); // expected: 2 1
```

What is actually printed and why?

- A) `2 1` — the swap works correctly.
- B) `1 2` — integers are value types and are passed by value, so the originals are unchanged.
- C) `0 0` — int defaults are applied inside the method.
- D) Compile error.

> **Answer: B**
> `int` is a value type. Parameters `x` and `y` are copies. Modifying them inside the method has no effect on `a` and `b`. To fix, use `ref` parameters or a tuple swap: `(a, b) = (b, a)`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer needs a variable that can hold either an `int` or `null`. Which declaration is correct in C#?

- A) `int x = null;`
- B) `Nullable x = new Nullable(0);`
- C) `int? x = null;`
- D) `object x = (int)null;`

> **Answer: C**
> `int?` is shorthand for `Nullable<int>`. It allows the value type `int` to hold `null`. Assigning `null` directly to a non-nullable `int` causes a compile error.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** What does the following expression print?

```cs
Console.WriteLine(10 / 3);
Console.WriteLine(10.0 / 3);
```

- A) `3` and `3`
- B) `3` and `3.3333333333333335`
- C) `3.33` and `3.33`
- D) `3` and `3.33`

> **Answer: B**
> Integer division truncates toward zero — `10 / 3` is `3`. When either operand is `double`, floating-point division is used — `10.0 / 3` produces the full double-precision result `3.3333333333333335`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer declares the following and attempts to use it. What is the result?

```cs
const double Pi = 3.14159;
Pi = 3.14; // line B
```

- A) `Pi` is updated to `3.14` at runtime.
- B) Compile error at line B — `const` fields cannot be reassigned.
- C) `Pi` is updated only in the current scope.
- D) Runtime exception.

> **Answer: B**
> `const` values are compile-time constants and are embedded directly into the IL. Any attempt to assign to them causes a compile-time error.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br>

## 2. Classes, OOP & Inheritance

**Q.** A developer writes the following class hierarchy and runs the code. What is printed?

```cs
class Animal
{
    public virtual string Speak() => "...";
}
class Dog : Animal
{
    public override string Speak() => "Woof";
}
Animal a = new Dog();
Console.WriteLine(a.Speak());
```

- A) `...`
- B) `Woof`
- C) Compile error — `a` is typed as `Animal`.
- D) `null`

> **Answer: B**
> `virtual`/`override` enables runtime polymorphism. The CLR dispatches to the most-derived override at runtime. Although `a` is declared as `Animal`, it holds a `Dog` instance, so `Dog.Speak()` is called.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer uses `new` instead of `override`. What does the code print?

```cs
class Base   { public virtual  string Name() => "Base"; }
class Derived : Base { public new string Name() => "Derived"; }

Base obj = new Derived();
Console.WriteLine(obj.Name());
```

- A) `Derived`
- B) `Base`
- C) Compile error.
- D) `null`

> **Answer: B**
> `new` hides the base method rather than overriding it. Because the reference is typed as `Base`, the virtual dispatch table still points to `Base.Name()`. If the variable were typed as `Derived`, it would print `Derived`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A class needs to prevent instantiation but still provide shared helper methods to derived classes. Which modifier achieves this?

- A) `static`
- B) `sealed`
- C) `abstract`
- D) `protected`

> **Answer: C**
> An `abstract` class cannot be instantiated directly but can be inherited. It may contain a mix of abstract members (no implementation, must be overridden) and concrete members (with implementation).

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** What is the output of the following code?

```cs
class Counter
{
    public static int Count = 0;
    public Counter() { Count++; }
}
var c1 = new Counter();
var c2 = new Counter();
Console.WriteLine(Counter.Count);
```

- A) `0`
- B) `1`
- C) `2`
- D) Compile error — `Count` is `static`.

> **Answer: C**
> `Count` is a static field shared by all instances. Each constructor call increments it. After two instantiations, `Count` is `2`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer wants a class that can only be used as a base class inside its own assembly, but can be used and instantiated normally outside. Which combination is correct?

- A) `public abstract class`
- B) `internal abstract class`
- C) `protected class`
- D) `public class` with `protected` constructor

> **Answer: D**
> A `public` class with a `protected` constructor cannot be instantiated directly from outside (no accessible constructor) but derived classes inside or outside the assembly can call it. `internal abstract` is only visible inside the assembly.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** What is printed by the following code?

```cs
class A
{
    public A()        => Console.Write("A ");
    public A(int x)   => Console.Write($"A{x} ");
}
class B : A
{
    public B() : base(1) => Console.Write("B ");
}
new B();
```

- A) `B A1`
- B) `A B`
- C) `A1 B`
- D) `B A`

> **Answer: C**
> Base constructors always run before derived constructors. `B()` chains to `A(int x)`, printing `A1 `, then `B()` body prints `B `.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br>

## 3. Generics & Collections

**Q.** A developer writes the following generic method and calls it:

```cs
T Max<T>(T a, T b) where T : IComparable<T>
    => a.CompareTo(b) >= 0 ? a : b;

Console.WriteLine(Max(3, 7));
Console.WriteLine(Max("apple", "mango"));
```

What is printed?

- A) `7` and `mango`
- B) `3` and `apple`
- C) Compile error — `int` does not implement `IComparable<T>`.
- D) `7` and `apple`

> **Answer: A**
> The `where T : IComparable<T>` constraint allows calling `CompareTo`. Both `int` and `string` implement `IComparable<T>`. `7 > 3` and `"mango" > "apple"` lexicographically, so `7` and `mango` are returned.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer needs a collection that:
- Maintains insertion order
- Allows O(1) key-based lookup
- Stores unique keys

Which .NET collection best fits?

- A) `SortedDictionary<K,V>`
- B) `Dictionary<K,V>`
- C) `OrderedDictionary`
- D) `LinkedList<T>`

> **Answer: B**
> In .NET 5+, `Dictionary<K,V>` preserves insertion order as an implementation detail (though not guaranteed by specification). For a guaranteed ordered dict use `SortedList`/`SortedDictionary` (sorted by key, not insertion) or `OrderedDictionary` (non-generic, legacy). The most idiomatic modern answer is `Dictionary<K,V>` for key lookup; if strict insertion-order guarantee is needed `List<(K,V)>` or a purpose-built `OrderedDictionary<K,V>` (.NET 9) should be used.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** What is the output of the following code?

```cs
var list = new List<int> { 1, 2, 3 };
var copy = list;
copy.Add(4);
Console.WriteLine(list.Count);
```

- A) `3` — `copy` is a separate list.
- B) `4` — `copy` references the same object as `list`.
- C) Compile error.
- D) `0`

> **Answer: B**
> `List<T>` is a reference type. Assigning `copy = list` copies the reference, not the data. Both variables point to the same `List<int>` instance. Adding to `copy` is visible through `list`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer needs fast membership testing for a large set of string tags with no duplicates. Which collection offers O(1) average-case lookup?

- A) `List<string>`
- B) `SortedSet<string>`
- C) `HashSet<string>`
- D) `LinkedList<string>`

> **Answer: C**
> `HashSet<T>` uses a hash table internally, giving O(1) average `Contains` and `Add`. `List` is O(n), `SortedSet` is O(log n), `LinkedList` is O(n).

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** What does this code print?

```cs
var stack = new Stack<int>();
stack.Push(1); stack.Push(2); stack.Push(3);
Console.WriteLine(stack.Pop());
Console.WriteLine(stack.Peek());
```

- A) `1` and `2`
- B) `3` and `2`
- C) `3` and `3`
- D) `1` and `1`

> **Answer: B**
> `Stack<T>` is LIFO. `Push(1)`, `Push(2)`, `Push(3)` — top is `3`. `Pop()` removes and returns `3`. `Peek()` returns the new top without removing it — `2`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer uses covariance on a generic interface:

```cs
IEnumerable<Dog> dogs = new List<Dog>();
IEnumerable<Animal> animals = dogs;
```

Why does this compile without a cast?

- A) `List<Dog>` inherits from `List<Animal>`.
- B) `IEnumerable<T>` is covariant (`out T`), so `IEnumerable<Dog>` is assignable to `IEnumerable<Animal>` when `Dog : Animal`.
- C) The cast is implicit because `Animal` is a `class`.
- D) This does not compile.

> **Answer: B**
> `IEnumerable<out T>` is covariant. The `out` keyword means `T` only appears in output (return) position. When `Dog` derives from `Animal`, `IEnumerable<Dog>` is safely assignable to `IEnumerable<Animal>`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br>

## 4. Delegates, Events & Lambda

**Q.** What is the output of the following code?

```cs
Func<int, int> doubler = x => x * 2;
Func<int, int> addTen  = x => x + 10;
Func<int, int> combined = x => addTen(doubler(x));
Console.WriteLine(combined(5));
```

- A) `20`
- B) `30`
- C) `25`
- D) `15`

> **Answer: A**
> `doubler(5)` = `10`, then `addTen(10)` = `20`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer builds a multicast delegate and notices only the last return value is received:

```cs
Func<int> d = () => 1;
d += () => 2;
d += () => 3;
Console.WriteLine(d());
```

What is printed and why?

- A) `1` — first delegate result is returned.
- B) `6` — results are summed.
- C) `3` — only the last delegate\'s return value is returned by multicast invocation.
- D) Compile error — `Func<int>` does not support `+=`.

> **Answer: C**
> Multicast delegates invoke all targets in order but only propagate the return value of the **last** invoked delegate. If you need all results, iterate `GetInvocationList()`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** What is the classic closure variable-capture bug in the following code?

```cs
var actions = new List<Action>();
for (int i = 0; i < 3; i++)
    actions.Add(() => Console.Write(i + " "));
actions.ForEach(a => a());
```

- A) Prints `0 1 2` — correct capture.
- B) Prints `3 3 3` — all lambdas capture the same variable `i`, which is `3` after the loop ends.
- C) Prints `1 2 3`.
- D) Compile error.

> **Answer: B**
> All three lambdas close over the **same** `i` variable. After the loop finishes, `i` is `3`. Fix: `int copy = i; actions.Add(() => Console.Write(copy + " "));`

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A junior developer accidentally does this:

```cs
public class Publisher
{
    public Action<string>? MessageSent;  // public field delegate
}
var pub = new Publisher();
pub.MessageSent += s => Console.WriteLine("A: " + s);
pub.MessageSent  = s => Console.WriteLine("B: " + s); // line X
pub.MessageSent?.Invoke("hello");
```

What is a problem with this design, and what does line X demonstrate?

- A) No problem. Line X adds another subscriber.
- B) The field delegate allows external code to replace all subscribers with `=` (as on line X), losing the first subscriber. An `event` keyword would prevent this.
- C) Line X causes a compile error.
- D) Both subscribers fire.

> **Answer: B**
> A public delegate field exposes the assignment operator to external code. `=` on line X replaces the entire invocation list. Using `event` restricts external code to `+=` and `-=` only, protecting subscribers.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** What is the difference between `Action<string>` and `Func<string, bool>`?

- A) Both are identical — `Action` and `Func` are aliases.
- B) `Action<string>` takes a `string` and returns `void`; `Func<string, bool>` takes a `string` and returns `bool`.
- C) `Action<string>` returns `bool`; `Func<string, bool>` returns `void`.
- D) `Func` cannot be used with lambda expressions.

> **Answer: B**
> `Action<T>` delegates always return `void`. `Func<T, TResult>` delegates return the last type parameter (`bool` here). Choose `Action` for side-effect operations and `Func` for transformations/predicates.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br>

## 5. LINQ

**Q.** What does this query return?

```cs
int[] numbers = { 1, 2, 3, 4, 5, 6 };
var result = numbers.Where(n => n % 2 == 0).Select(n => n * n);
foreach (var x in result) Console.Write(x + " ");
```

- A) `1 4 9 16 25 36`
- B) `4 16 36`
- C) `2 4 6`
- D) `1 9 25`

> **Answer: B**
> `Where` filters to even numbers (`2, 4, 6`). `Select` squares each: `4, 16, 36`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer writes the following LINQ and is surprised that the database is queried five times. Why?

```cs
IQueryable<Product> query = db.Products.Where(p => p.Price > 100);
for (int i = 0; i < 5; i++)
    Console.WriteLine(query.Count());
```

- A) `IQueryable` caches results after the first query.
- B) LINQ queries are lazy — each call to `Count()` executes a new SQL query.
- C) `Where` forces immediate execution.
- D) This is a compile error.

> **Answer: B**
> `IQueryable<T>` uses deferred execution. The query is not executed when defined; it executes each time a terminal operator (`Count`, `ToList`, `First`, etc.) is called. Fix: `int count = query.Count();` before the loop.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** What does the following code print?

```cs
var words = new[] { "banana", "apple", "cherry", "avocado" };
var result = words
    .Where(w => w.StartsWith('a'))
    .OrderBy(w => w.Length)
    .FirstOrDefault();
Console.WriteLine(result);
```

- A) `apple`
- B) `avocado`
- C) `banana`
- D) `null`

> **Answer: A**
> `Where` filters to words starting with `a`: `["apple", "avocado"]`. `OrderBy(Length)` sorts by length: `["apple" (5), "avocado" (7)]`. `FirstOrDefault` returns `apple`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer needs to flatten a list of orders where each order contains multiple items, and get a distinct list of all product names. Which LINQ operators should be used?

- A) `Select` then `Distinct`
- B) `SelectMany` then `Distinct`
- C) `Join` then `GroupBy`
- D) `Aggregate` then `Where`

> **Answer: B**
> `SelectMany` flattens a sequence of sequences (each order\'s items into a single sequence). `Distinct` then removes duplicate product names.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** What is the difference between `First()` and `FirstOrDefault()` in LINQ?

- A) `First()` returns all matching elements; `FirstOrDefault()` returns only the first.
- B) `First()` throws `InvalidOperationException` if the sequence is empty; `FirstOrDefault()` returns the default value for the type (`null` for reference types, `0` for `int`, etc.).
- C) `FirstOrDefault()` is faster than `First()`.
- D) There is no difference.

> **Answer: B**
> `First()` throws when no element matches or the sequence is empty. `FirstOrDefault()` returns `default(T)` in those cases. Prefer `FirstOrDefault()` when an empty result is a valid scenario.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** What is printed by this GroupBy query?

```cs
var data = new[] { "cat", "car", "bat", "bar", "bee" };
var groups = data.GroupBy(w => w[0]);
foreach (var g in groups)
    Console.WriteLine($"{g.Key}:{g.Count()}");
```

- A) `c:2  b:3`
- B) `cat:1  car:1  bat:1  bar:1  bee:1`
- C) `c:2  b:3` on separate lines
- D) All words on one line.

> **Answer: C**
> Words starting with `c`: `cat`, `car` (count 2). Words starting with `b`: `bat`, `bar`, `bee` (count 3). Output is `c:2` and `b:3` on separate lines.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br>

## 6. Async / Await & Multithreading

**Q.** A developer writes this async method. What is wrong with it?

```cs
public async Task<string> GetDataAsync()
{
    Thread.Sleep(2000); // simulate work
    return "done";
}
```

- A) `async` methods cannot return `string`.
- B) `Thread.Sleep` blocks the calling thread even though the method is `async`. It should be `await Task.Delay(2000)`.
- C) The method is missing a `return` statement.
- D) `async void` should be used instead of `async Task<string>`.

> **Answer: B**
> `Thread.Sleep` is a synchronous blocking call. It blocks the thread for 2 seconds even inside an `async` method, defeating the purpose of async. `await Task.Delay(2000)` yields the thread to the pool while waiting.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** What is printed by the following code?

```cs
async Task<int> ComputeAsync()
{
    await Task.Delay(0);
    return 42;
}
var t = ComputeAsync();
Console.WriteLine(t.IsCompleted ? "Done" : "Running");
await t;
Console.WriteLine("Finished: " + t.Result);
```

- A) `Running` then `Finished: 42`
- B) `Done` then `Finished: 42`
- C) `Finished: 42` immediately
- D) Deadlock

> **Answer: A**
> `await Task.Delay(0)` still schedules a continuation, so the task may not be complete by the time the `IsCompleted` check runs. After `await t` the task is complete and `Result` is `42`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer writes a fire-and-forget async method as `async void`. A tester reports that exceptions from the method are swallowed silently. Why?

- A) `async void` exceptions are propagated to the calling thread\'s `SynchronizationContext` but are unobservable if no handler is registered, causing unhandled exceptions.
- B) `async void` silently suppresses all exceptions by design.
- C) Exceptions in `async` methods never propagate.
- D) The developer should use `try/catch` inside the method to solve it.

> **Answer: A**
> `async void` raises exceptions on the `SynchronizationContext` rather than returning a faultable `Task`. If there is no handler, the exception can crash the process or be silently lost. Use `async Task` and always `await` the result.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** What is the risk of the following code in a high-throughput ASP.NET Core application?

```cs
public string GetData() => GetDataAsync().Result;
```

- A) No risk — `.Result` is the standard way to call async from sync.
- B) Potential deadlock: `.Result` blocks the current thread while the async continuation may be waiting to resume on that same thread (SynchronizationContext deadlock).
- C) `.Result` cannot be called on `Task<T>`.
- D) This only risks a `NullReferenceException`.

> **Answer: B**
> In contexts with a `SynchronizationContext` (classic ASP.NET, WPF, WinForms), `.Result` blocks the thread, while the awaited continuation needs that same thread to resume — causing a deadlock. Use `await` throughout or `ConfigureAwait(false)`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** Two threads share a counter and increment it 10,000 times each. The final value is often less than 20,000. What is the cause and fix?

```cs
int counter = 0;
void Increment() { for (int i = 0; i < 10_000; i++) counter++; }
var t1 = Task.Run(Increment);
var t2 = Task.Run(Increment);
Task.WaitAll(t1, t2);
Console.WriteLine(counter);
```

- A) Integer overflow.
- B) `counter++` is not atomic (read-modify-write race condition). Fix: `Interlocked.Increment(ref counter)`.
- C) `Task.WaitAll` does not wait for both tasks.
- D) `for` loops cannot run concurrently.

> **Answer: B**
> `counter++` is three operations: read, add 1, write back. Two threads can both read the same value and write the same incremented value, losing updates. `Interlocked.Increment` is CPU-atomic and fixes the race.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** When should `ValueTask<T>` be preferred over `Task<T>`?

- A) Always — `ValueTask<T>` is faster in all scenarios.
- B) When the method frequently completes synchronously (no I/O needed), avoiding a heap allocation for the `Task` object.
- C) When the method runs on a background thread.
- D) `ValueTask<T>` and `Task<T>` are identical.

> **Answer: B**
> `Task<T>` always allocates a heap object. `ValueTask<T>` is a `struct` that avoids the allocation when the result is already available synchronously. Prefer it in hot-path methods like caching layers. Do not `await` a `ValueTask<T>` more than once without converting to `Task` first.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br>

## 7. Exception Handling

**Q.** What is the difference between `throw` and `throw ex` in the following code?

```cs
try { DoWork(); }
catch (Exception ex)
{
    // Option A:
    throw;
    // Option B:
    throw ex;
}
```

- A) Both are identical.
- B) `throw` preserves the original stack trace; `throw ex` resets the stack trace to the current `catch` block, losing the original call site.
- C) `throw ex` is preferred because it includes the variable.
- D) `throw` rethrows as a new exception type.

> **Answer: B**
> `throw` (bare) re-throws the current exception with its original stack trace intact. `throw ex` creates a new throw point — the stack trace appears to originate from the `catch` block, hiding the real source of the error.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** Given this code, does the `finally` block execute?

```cs
try
{
    Console.WriteLine("try");
    return;
}
finally
{
    Console.WriteLine("finally");
}
```

- A) No — `return` exits the method before `finally`.
- B) Yes — `finally` always executes before the method returns.
- C) Only if an exception is thrown.
- D) Compile error — `return` inside `try` is not allowed.

> **Answer: B**
> `finally` blocks execute regardless of how the `try` block exits — normal flow, `return`, `break`, `continue`, or a handled exception. Exceptions: `Environment.FailFast()` and `StackOverflowException` can bypass `finally`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer defines a custom exception. Which constructor pattern is most important to include?

```cs
public class OrderException : Exception
{
    // Which constructors should be included?
}
```

- A) Only `OrderException(string message)`.
- B) Only the parameterless constructor.
- C) `OrderException()`, `OrderException(string message)`, and `OrderException(string message, Exception inner)`.
- D) Constructors are not needed — `Exception` handles them.

> **Answer: C**
> Best practice is to provide the three standard constructors. The `(string, Exception)` constructor is especially important: it allows wrapping a lower-level exception as an `InnerException`, preserving the full exception chain for debugging.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer uses an exception filter. What is an advantage of the `when` clause?

```cs
catch (HttpRequestException ex) when (ex.StatusCode == HttpStatusCode.NotFound)
{
    // handle 404 only
}
```

- A) It is purely cosmetic — functionally identical to an `if` inside `catch`.
- B) The `when` clause is evaluated before the stack is unwound, allowing debuggers to break at the throw site rather than at the `catch`. Also, if it returns `false`, the exception propagates to outer handlers without entering the `catch` block.
- C) `when` prevents exceptions from being logged.
- D) Exception filters only work in async methods.

> **Answer: B**
> The `when` predicate is evaluated in the context of the original throw site. If it returns `false`, the stack is not unwound, allowing debuggers to inspect the original state. It also cleanly routes exception handling without re-throwing.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** What happens when `Task.WhenAll` is awaited and two of three tasks fail?

```cs
Task[] tasks = [
    Task.Run(() => throw new Exception("E1")),
    Task.Run(() => throw new Exception("E2")),
    Task.Run(() => Console.WriteLine("ok")),
];
try { await Task.WhenAll(tasks); }
catch (Exception ex) { Console.WriteLine(ex.Message); }
```

- A) Both `E1` and `E2` are printed.
- B) Only `E1` is printed — `await` re-throws only the first exception from the `AggregateException`.
- C) `AggregateException` is printed.
- D) No exception is thrown.

> **Answer: B**
> `await Task.WhenAll(...)` waits for all tasks and then re-throws the **first** exception from the underlying `AggregateException`. To see all failures, inspect `tasks[i].Exception` for each faulted task after the catch.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br>

## 8. Memory Management & GC

**Q.** A developer notices that an object with a finalizer takes two garbage collection cycles to be reclaimed. Why?

- A) The GC waits for 2 cycles as a safety buffer for all objects.
- B) Objects with finalizers are placed on the finalization queue when unreachable (cycle 1). The finalizer thread processes them asynchronously, after which a second collection reclaims the memory (cycle 2).
- C) Finalizers are only called every other GC cycle.
- D) This is a bug in the GC.

> **Answer: B**
> When an unreachable finalizable object is discovered in cycle 1, the GC queues it for finalization rather than reclaiming it immediately. The finalizer thread runs the `~Destructor`, and the next GC cycle reclaims the now-finalized object. This is why `GC.SuppressFinalize(this)` in `Dispose()` improves performance.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** Which code pattern correctly implements the Dispose pattern to handle both managed and unmanaged resources?

```cs
public class Resource : IDisposable
{
    private bool _disposed;

    // Option A:
    public void Dispose() { CloseHandle(); }

    // Option B:
    protected virtual void Dispose(bool disposing)
    {
        if (_disposed) return;
        if (disposing) { /* free managed */ }
        CloseHandle(); // free unmanaged
        _disposed = true;
    }
    public void Dispose() { Dispose(true); GC.SuppressFinalize(this); }
    ~Resource() { Dispose(false); }
}
```

- A) Option A — simpler is better.
- B) Option B — correctly handles both managed/unmanaged, prevents double-dispose, removes object from finalization queue when explicitly disposed.
- C) Neither — you should never implement `IDisposable`.
- D) Option A, but add `GC.Collect()` at the end.

> **Answer: B**
> Option B is the canonical Dispose pattern: `Dispose(bool)` separates managed from unmanaged cleanup, `GC.SuppressFinalize` prevents a redundant finalizer call when `Dispose()` is called explicitly, and the `_disposed` guard prevents double-dispose.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer writes a cache using strong references and observes memory keeps growing. What alternative avoids holding objects alive unnecessarily?

- A) Use `List<T>` with a timer to clear entries.
- B) Use `WeakReference<T>` — the GC can reclaim the cached object if memory pressure is high, and the cache falls back to recomputing.
- C) Use `static` fields instead.
- D) Increase the LOH threshold.

> **Answer: B**
> `WeakReference<T>` allows the GC to collect the referenced object. When `TryGetTarget` returns `false`, the cache misses and recomputes. This avoids holding large objects alive longer than necessary under memory pressure.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer finds a memory leak in a long-running service. The most likely root cause from the following list is:

- A) Using `int` instead of `long`.
- B) An event handler subscribed to a long-lived publisher object that is never unsubscribed, keeping the subscriber alive.
- C) Calling `GC.Collect()` too frequently.
- D) Using `async/await`.

> **Answer: B**
> Unsubscribed event handlers are one of the most common managed memory leaks. The publisher holds a delegate reference to the subscriber. As long as the publisher lives, the subscriber cannot be collected. Always unsubscribe in `Dispose()` or use `WeakEventManager`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** When is it acceptable to call `GC.Collect()` explicitly?

- A) In every method to keep memory low.
- B) Almost never — but valid rare cases include: after releasing a known large temporary allocation (e.g., loading a large dataset that is now discarded), in unit tests that verify finalizer behaviour, or before a performance-critical benchmark to establish a clean baseline.
- C) Whenever memory usage exceeds 100 MB.
- D) It should replace `Dispose()`.

> **Answer: B**
> Forcing a GC disrupts the GC\'s self-tuning heuristics, promotes objects to higher generations unnecessarily, and introduces latency pauses. It is almost always the wrong choice in production application code.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br>

## 9. Interfaces & Patterns

**Q.** A developer defines two interfaces with the same method name and implements both:

```cs
interface IA { string Hello(); }
interface IB { string Hello(); }
class C : IA, IB
{
    public string Hello() => "shared";
    string IB.Hello() => "IB explicit";
}
C c = new C();
Console.WriteLine(c.Hello());
Console.WriteLine(((IB)c).Hello());
```

What is the output?

- A) `shared` and `shared`
- B) `shared` and `IB explicit`
- C) `IB explicit` and `IB explicit`
- D) Compile error — two interfaces cannot share a method name.

> **Answer: B**
> The public `Hello()` satisfies `IA` and is accessible via the class reference. `IB.Hello()` is an explicit interface implementation, only accessible via an `IB` reference, and returns `IB explicit`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer wants to add a default implementation to an interface method so existing implementors do not need to be updated. Which C# feature supports this?

- A) `abstract` interface methods.
- B) Default interface methods (C# 8+) — add a method body directly in the interface.
- C) Extension methods on the interface.
- D) This is not possible in C#.

> **Answer: B**
> C# 8 introduced default interface method implementations. Existing classes that implement the interface inherit the default without change. Classes can still override the default.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** Which design pattern is implemented in this code?

```cs
public abstract class DataProcessor
{
    public void Process()
    {
        ReadData();
        TransformData();
        WriteData();
    }
    protected abstract void ReadData();
    protected abstract void TransformData();
    protected virtual void WriteData() => Console.WriteLine("Writing...");
}
```

- A) Factory Method
- B) Strategy
- C) Template Method
- D) Decorator

> **Answer: C**
> The Template Method pattern defines the skeleton of an algorithm in a base class (`Process()`), deferring specific steps to subclasses via `abstract` or `virtual` methods. The overall structure is fixed; the details vary.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer wants to ensure only one instance of a `ConfigurationManager` class exists throughout the application. Which pattern should be used?

- A) Factory Method
- B) Singleton
- C) Prototype
- D) Abstract Factory

> **Answer: B**
> The Singleton pattern restricts instantiation to one object. In .NET, the thread-safe lazy singleton is commonly implemented with `Lazy<T>`: `private static readonly Lazy<ConfigurationManager> _instance = new(() => new());`

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer wants to add logging and timing behaviour to an existing `IOrderService` without modifying its implementation. Which pattern applies?

- A) Template Method
- B) Observer
- C) Decorator
- D) Bridge

> **Answer: C**
> The Decorator pattern wraps an existing object with a new class implementing the same interface, adding behaviour before/after delegating to the inner implementation — no modification of the original class needed.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br>

## 10. File I/O & Serialization

**Q.** A developer reads a large file line by line using `File.ReadAllLines`. A colleague suggests replacing it with `File.ReadLinesAsync` in .NET 9. What is the key difference?

- A) `ReadAllLines` returns a `string[]`; it loads the entire file into memory at once. `ReadLinesAsync` returns `IAsyncEnumerable<string>` and streams one line at a time, using far less memory for large files.
- B) They are identical — `Async` just adds a keyword.
- C) `ReadAllLines` is faster because it reads the whole file in one I/O call.
- D) `ReadLinesAsync` requires a `CancellationToken`.

> **Answer: A**
> `File.ReadAllLines` allocates an array containing every line. For a 10 GB log file this can exhaust memory. `File.ReadLinesAsync` (returning `IAsyncEnumerable<string>`) yields one line at a time, allowing processing with `await foreach` and minimal memory usage.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer serializes an object to JSON:

```cs
var order = new Order { Id = 1, CustomerName = "Alice" };
string json = JsonSerializer.Serialize(order);
```

The output is `{"Id":1,"CustomerName":"Alice"}` but the API contract requires `{"id":1,"customer_name":"Alice"}`. What is the simplest fix?

- A) Rename the C# properties.
- B) Use `[JsonPropertyName("id")]` on each property, or configure `JsonSerializerOptions` with `PropertyNamingPolicy = JsonNamingPolicy.SnakeCaseLower`.
- C) Implement a custom converter.
- D) JSON output cannot be customised.

> **Answer: B**
> `[JsonPropertyName("id")]` on a property overrides the serialized name. For a global policy, `JsonNamingPolicy.SnakeCaseLower` (added in .NET 8) converts all property names to snake_case automatically.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer needs to process a 2 GB JSON file without loading it all into memory. Which .NET API is most appropriate?

- A) `JsonSerializer.Deserialize<T>(string)`
- B) `Utf8JsonReader` or `JsonDocument.Parse` with a `Stream` overload
- C) `XmlDocument.Load`
- D) `BinaryReader`

> **Answer: B**
> `Utf8JsonReader` is a forward-only, low-allocation JSON reader that processes tokens from a `ReadOnlySpan<byte>` or `ReadOnlySequence<byte>`. `JsonSerializer.DeserializeAsync` with a `Stream` also streams the document. Both avoid loading the entire file into memory.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer is writing a file and wants to ensure the data is flushed to disk even if the application crashes immediately after. Which approach is correct?

- A) `File.WriteAllText` — the OS guarantees persistence.
- B) Open the file with `FileOptions.WriteThrough` or call `FileStream.Flush(flushToDisk: true)` to bypass OS write-back caching and force data to the physical disk.
- C) Calling `GC.Collect()` after writing ensures the buffer is flushed.
- D) Use `Thread.Sleep(1000)` to give the OS time to flush.

> **Answer: B**
> `FileOptions.WriteThrough` (or `flushToDisk: true`) bypasses the OS page cache and writes directly to the storage device. This is important for write-ahead logs and databases where durability is critical. Normal `Flush()` only moves data from .NET buffers to the OS, not necessarily to disk.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br>

## 11. ASP.NET Core & Web API

**Q.** An ASP.NET Core controller action is registered with `[HttpPost]` but the client receives `405 Method Not Allowed` when sending a `POST` request. What is the most likely cause?

- A) `POST` is not supported by ASP.NET Core.
- B) The route is only matched by `GET`. Either the `[Route]` template conflicts, or the client is hitting the wrong URL that maps to a different action decorated with `[HttpGet]` only.
- C) The `[ApiController]` attribute disables `POST`.
- D) Missing `[FromBody]` attribute.

> **Answer: B**
> `405 Method Not Allowed` means the URL matched a route but the HTTP method was not allowed. The most common causes are: the route matches a different action that only allows `GET`, or attribute routing has a mismatch between the declared HTTP method and the client\'s request method.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer registers a service as `Singleton` but injects it into a `Scoped` service, which is then injected into a `Transient` service. What is the problem?

- A) No problem — all lifetimes are compatible.
- B) Injecting a `Scoped` service into a `Singleton` creates a "captive dependency" — the singleton holds the scoped service for its entire lifetime (app lifetime), preventing it from being disposed at the end of the request scope.
- C) `Transient` cannot depend on `Scoped`.
- D) This causes a compile error.

> **Answer: B**
> A `Singleton` outlives a `Scope`. If a singleton captures a `Scoped` service, that scoped service is never released at the end of the request, causing memory leaks and stale state. ASP.NET Core\'s DI container throws a `InvalidOperationException` in Development mode to catch this.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A minimal API endpoint is defined as:

```cs
app.MapGet("/users/{id}", (int id) => Results.Ok($"User {id}"));
```

A client calls `GET /users/abc`. What response does the client receive?

- A) `200 OK` with `"User abc"`.
- B) `400 Bad Request` — route constraint binding fails because `"abc"` cannot be parsed as `int`.
- C) `404 Not Found`.
- D) `500 Internal Server Error`.

> **Answer: B**
> When a route parameter cannot be converted to the declared parameter type (`int`), the ASP.NET Core model binding system returns `400 Bad Request` automatically. With `[ApiController]` or minimal APIs, this is handled before the handler runs.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer registers two middleware components in this order:

```cs
app.Use(async (ctx, next) => { Console.Write("A1 "); await next(); Console.Write("A2 "); });
app.Use(async (ctx, next) => { Console.Write("B1 "); await next(); Console.Write("B2 "); });
app.Run(async ctx => Console.Write("C "));
```

What is printed for a single request?

- A) `A1 B1 C B2 A2`
- B) `A1 A2 B1 B2 C`
- C) `C B2 A2 B1 A1`
- D) `A1 B1 C A2 B2`

> **Answer: A**
> The middleware pipeline is a chain of nested calls. `A1` executes, calls `next` → `B1` executes, calls `next` → `C` runs (terminal). Unwinding: `B2` runs, then `A2`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A Web API returns `200 OK` for a newly created resource. A reviewer says this violates REST conventions. What should be returned instead?

- A) `204 No Content`
- B) `201 Created` with a `Location` header pointing to the new resource URL.
- C) `202 Accepted`
- D) `200 OK` is perfectly correct for POST.

> **Answer: B**
> REST convention for a successful resource creation is `201 Created` with a `Location` header containing the URI of the new resource. Use `CreatedAtAction` or `CreatedAtRoute` in ASP.NET Core controllers, or `Results.Created(uri, value)` in minimal APIs.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer forgets to add `app.UseAuthentication()` but has `app.UseAuthorization()`. What happens when a protected endpoint is accessed with a valid JWT?

- A) It works — authorization does not need authentication middleware.
- B) The user identity is never set (authentication middleware was skipped), so the user is treated as anonymous, and the `[Authorize]` attribute returns `401 Unauthorized`.
- C) The application crashes at startup.
- D) `403 Forbidden` is returned.

> **Answer: B**
> `UseAuthentication()` must come before `UseAuthorization()`. Authentication middleware reads and validates the JWT, setting `HttpContext.User`. Without it, `User` is an unauthenticated principal and every `[Authorize]` check fails with `401`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br>

## 12. Entity Framework Core

**Q.** A developer writes the following LINQ query against an EF Core `DbContext`. What is the issue?

```cs
var names = db.Products
    .ToList()
    .Where(p => p.Price > 100)
    .Select(p => p.Name)
    .ToList();
```

- A) `Where` cannot be applied after `ToList`.
- B) `ToList()` before `Where` forces the entire `Products` table to be loaded into memory, then filtering happens in-memory instead of in SQL. The correct approach is to call `Where` and `Select` before `ToList`.
- C) `Select` must come before `Where`.
- D) No issue — EF Core translates the full chain to SQL.

> **Answer: B**
> `ToList()` materialises the query immediately, pulling all rows into memory. Any LINQ operators applied after `ToList()` run in-process (LINQ to Objects), not as SQL. Move `Where` and `Select` before the final `ToList()` to generate an efficient SQL query.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer loads a list of orders with their items:

```cs
var orders = db.Orders.ToList();
foreach (var order in orders)
    Console.WriteLine(order.Items.Count);
```

The app is extremely slow when there are 1,000 orders. What is the problem?

- A) `foreach` is slow.
- B) The N+1 query problem — each iteration issues a separate SQL query to load `Items` for each order, resulting in 1,001 queries total.
- C) `order.Items.Count` is an expensive operation.
- D) No problem — EF Core batches the queries.

> **Answer: B**
> Without `.Include(o => o.Items)`, EF Core uses lazy loading (if enabled) or throws a `NullReferenceException`. With lazy loading enabled, each `order.Items` access fires a new SQL query — 1,000 extra queries. Fix: `db.Orders.Include(o => o.Items).ToList()`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer calls `db.SaveChangesAsync()` without calling `AsNoTracking()`. A colleague adds `AsNoTracking()` to a read-only query. What is the benefit?

- A) `AsNoTracking()` is required for read operations.
- B) `AsNoTracking()` disables change tracking for the returned entities, reducing memory usage and improving performance for read-only queries where the entities are never updated.
- C) `AsNoTracking()` prevents `SaveChangesAsync` from persisting changes accidentally.
- D) No benefit — `AsNoTracking()` only works with `IQueryable`.

> **Answer: B**
> EF Core\'s change tracker stores a snapshot of each loaded entity for change detection. For read-only scenarios (reports, APIs that only return data), this snapshot wastes memory and CPU. `AsNoTracking()` skips this overhead.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** Two concurrent requests both read an order with `Status = "Pending"` and try to set it to `"Processing"`. Only one should succeed. Which EF Core mechanism handles this?

- A) `AsNoTracking`
- B) Optimistic concurrency with a `[ConcurrencyCheck]` or `rowversion`/`timestamp` column — EF Core throws `DbUpdateConcurrencyException` when the row has been modified between read and write.
- C) Wrapping in a `lock` statement.
- D) Using `SingleOrDefault` instead of `FirstOrDefault`.

> **Answer: B**
> Optimistic concurrency adds a concurrency token (e.g., `[Timestamp] public byte[] RowVersion { get; set; }`). EF Core includes the original token in the `WHERE` clause of `UPDATE`. If another request has changed the row, the `WHERE` matches zero rows and EF throws `DbUpdateConcurrencyException`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br>

## 13. C# Language Features

**Q.** What does the following pattern matching code print?

```cs
object obj = 42;
if (obj is int n and > 10 and < 100)
    Console.WriteLine($"In range: {n}");
else
    Console.WriteLine("Out of range");
```

- A) `Out of range`
- B) `In range: 42`
- C) Compile error — `and` is not a valid pattern keyword.
- D) `42`

> **Answer: B**
> C# 9 conjunctive patterns use `and` to combine conditions. `obj is int n and > 10 and < 100` checks that `obj` is an `int`, assigns it to `n`, and verifies `10 < n < 100`. `42` satisfies all conditions.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer uses a `switch` expression. What does the following return for `shape = new Circle(5)`?

```cs
double Area(Shape shape) => shape switch
{
    Circle  c => Math.PI * c.Radius * c.Radius,
    Rectangle r => r.Width * r.Height,
    _ => throw new ArgumentOutOfRangeException()
};
```

- A) `0`
- B) `78.53981633974483`
- C) `ArgumentOutOfRangeException`
- D) Compile error

> **Answer: B**
> The `switch` expression matches `Circle` with radius 5 and computes `π × 5 × 5 ≈ 78.54`. Switch expressions in C# 8+ allow concise pattern-based dispatch without `break` statements.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** What does the null-coalescing assignment operator do in the following code?

```cs
List<string>? tags = null;
tags ??= new List<string>();
tags.Add("dotnet");
Console.WriteLine(tags.Count);
```

- A) Throws `NullReferenceException`.
- B) Prints `1` — `??=` assigns `new List<string>()` to `tags` only if it is `null`.
- C) Compile error — `??=` is not a valid operator.
- D) Prints `0`.

> **Answer: B**
> The `??=` operator (C# 8+) assigns the right-hand value to the left-hand variable only when the variable is `null`. After `??=`, `tags` is a new list. Adding `"dotnet"` makes `Count` = `1`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** What is the output of the following using C# string interpolation with format specifiers?

```cs
decimal price = 1234.5m;
Console.WriteLine($"Price: {price:C2}");
Console.WriteLine($"Pi: {Math.PI:F4}");
```

- A) `Price: 1234.5` and `Pi: 3.14`
- B) `Price: $1,234.50` (locale-dependent) and `Pi: 3.1416`
- C) Compile error.
- D) `Price: C21234.5` and `Pi: F43.14159`

> **Answer: B**
> `{value:C2}` formats as currency with 2 decimal places (locale-dependent — `$1,234.50` in `en-US`). `{value:F4}` formats as fixed-point with 4 decimal places — `3.1416` (rounded).

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer uses `Span<T>` instead of array slicing. What is the advantage?

```cs
int[] data = { 1, 2, 3, 4, 5 };
Span<int> slice = data.AsSpan(1, 3); // { 2, 3, 4 }
```

- A) `Span<T>` copies the slice into a new array.
- B) `Span<T>` is a view over the original array — no allocation, no copy. Modifications through `slice` affect `data`.
- C) `Span<T>` is only for `string` manipulation.
- D) `Span<T>` cannot be modified.

> **Answer: B**
> `Span<T>` is a stack-allocated struct that represents a contiguous region of memory. `.AsSpan(1, 3)` creates a zero-copy window into `data`. Mutations via `slice` are reflected in the original array. It is allocation-free, unlike creating a `new int[]` slice.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer uses `required` on a property:

```cs
public class Config
{
    public required string ConnectionString { get; init; }
}
var cfg = new Config(); // line A
```

What happens at line A in C# 11+?

- A) `ConnectionString` defaults to `null`.
- B) Compile error — `required` properties must be set in the object initializer.
- C) Runtime exception.
- D) `required` is only for records.

> **Answer: B**
> The `required` modifier (C# 11) enforces that the property is set in an object initializer. Omitting it is a compile-time error: `CS9035: Required member 'Config.ConnectionString' must be set in the object initializer or attribute constructor`.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br>

## 14. Records, Structs & Value Types

**Q.** What does the following code print?

```cs
record Point(int X, int Y);
var p1 = new Point(1, 2);
var p2 = new Point(1, 2);
Console.WriteLine(p1 == p2);
Console.WriteLine(ReferenceEquals(p1, p2));
```

- A) `False` and `False`
- B) `True` and `True`
- C) `True` and `False`
- D) Compile error — records cannot be compared with `==`.

> **Answer: C**
> Records use **value-based equality**: `p1 == p2` compares properties and returns `true`. But records are still reference types (unless `record struct`), so `ReferenceEquals` returns `false` — they are distinct objects on the heap.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer uses a `record` with the `with` expression. What does this print?

```cs
record Person(string Name, int Age);
var alice = new Person("Alice", 30);
var bob   = alice with { Name = "Bob" };
Console.WriteLine(alice.Name + " " + bob.Name);
```

- A) `Bob Bob`
- B) `Alice Alice`
- C) `Alice Bob`
- D) Compile error — records are immutable.

> **Answer: C**
> The `with` expression creates a **new** record copying all properties from the original and overriding specified ones. `alice` is unchanged (`Alice, 30`); `bob` is a new record (`Bob, 30`). Records are immutable — `with` produces a copy, not a mutation.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** What is the key difference between a `class` and a `struct` in C#?

- A) Structs cannot have methods.
- B) A `class` is a reference type allocated on the heap; a `struct` is a value type typically allocated on the stack (or inline in its containing type). Assignment copies the value for structs, copies the reference for classes.
- C) Structs support inheritance from other structs.
- D) Classes cannot have fields.

> **Answer: B**
> The fundamental distinction: `class` → reference type → assignment copies reference → heap allocation. `struct` → value type → assignment copies the entire value → typically stack/inline. Structs cannot inherit from other structs/classes (except interfaces) and are sealed implicitly.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer defines a large mutable struct and passes it by value repeatedly. A performance profiler shows excessive copying. What is the most appropriate fix?

- A) Convert to a `class`.
- B) Pass by `ref` or `in` (read-only ref) to avoid copying, or convert to a `readonly struct` for immutable data.
- C) Use `unsafe` code.
- D) Add a finalizer to the struct.

> **Answer: B**
> Passing a large struct by value copies all its bytes. `ref` parameters pass a reference to the original, avoiding copying. `in` passes a read-only reference (no mutation, no copy). `readonly struct` also allows the JIT to pass by reference automatically in some cases.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br>

## 15. Dependency Injection & Testing

**Q.** A developer wants to write a unit test for `OrderService` which calls `IEmailSender`. The test must verify that `SendAsync` was called once without actually sending email. Which technique achieves this?

- A) Inherit from `EmailSender` and override the method.
- B) Use a mocking framework (e.g., Moq, NSubstitute) to create a mock `IEmailSender`, then verify the call using `mock.Verify(x => x.SendAsync(...), Times.Once)`.
- C) Set `SMTP_HOST=test` in environment variables.
- D) Unit tests cannot verify method calls.

> **Answer: B**
> Mocking frameworks create test doubles that implement the interface. After execution, `Verify` asserts that specific methods were called with specific arguments. This is the standard approach to testing method interactions without side effects.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer registers a service in `Program.cs` and needs to resolve it in a test without starting a full web host. Which approach is correct?

- A) Copy all service registrations into the test manually.
- B) Use `WebApplicationFactory<Program>` from `Microsoft.AspNetCore.Mvc.Testing` — it creates an in-memory test server with the real DI container, allowing services to be resolved via `factory.Services.GetRequiredService<T>()`.
- C) Use `new ServiceCollection().BuildServiceProvider()` in the test.
- D) Services cannot be tested without a running server.

> **Answer: B**
> `WebApplicationFactory<T>` spins up the application in memory using the same `Program.cs` configuration. Tests can make HTTP requests via `CreateClient()` or resolve services from the container, enabling integration tests without a real network connection.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer writes this test and it always passes even when the code is broken. What is wrong?

```cs
[Fact]
public async Task GetOrder_ReturnsOrder()
{
    var service = new OrderService();
    var order = service.GetOrderAsync(1); // missing await
    Assert.NotNull(order);
}
```

- A) `Assert.NotNull` is the wrong assertion.
- B) The `await` is missing. `order` holds a `Task<Order?>`, not the resolved `Order?`. A `Task` object is never `null`, so `Assert.NotNull` always passes regardless of the actual result.
- C) `[Fact]` should be `[Test]`.
- D) `async` methods cannot be tested.

> **Answer: B**
> Without `await`, `order` is a non-null `Task<Order?>` — not the actual order value. The assertion checks the task object, not the result. Fix: `var order = await service.GetOrderAsync(1);`

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer uses the `Arrange-Act-Assert` pattern in a test. A reviewer suggests extracting repeated `Arrange` steps. Which xUnit attribute supports shared setup that runs once before all tests in a class?

- A) `[SetUp]`
- B) `IClassFixture<T>` — the fixture class is constructed once per test class and injected via the constructor.
- C) `[TestInitialize]`
- D) `[OneTimeSetUp]`

> **Answer: B**
> xUnit uses `IClassFixture<T>` for class-scoped setup (created once, shared by all tests in the class). `[SetUp]` and `[TestInitialize]` are NUnit/MSTest attributes. xUnit\'s per-test setup is done in the test class constructor.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

**Q.** A developer wants to run the same test with multiple input values:

```cs
[Theory]
[InlineData(2, 4)]
[InlineData(3, 9)]
[InlineData(-1, 1)]
public void Square_ReturnsCorrectValue(int input, int expected)
{
    Assert.Equal(expected, input * input);
}
```

What xUnit feature is being used and how many test cases will be run?

- A) `[Fact]` — 1 test.
- B) `[Theory]` with `[InlineData]` — 3 separate test cases will be run, one per `[InlineData]` attribute.
- C) `[Theory]` runs all data in a single test.
- D) Compile error — `[Theory]` is not a valid xUnit attribute.

> **Answer: B**
> `[Theory]` marks a data-driven test. Each `[InlineData(...)]` attribute provides one set of arguments, resulting in 3 individual test cases. This is xUnit\'s equivalent of parameterised tests in NUnit (`[TestCase]`) and MSTest (`[DataRow]`).

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>
