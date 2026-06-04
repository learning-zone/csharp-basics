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

* **Fundamentals**: Data types, variables, type system, and basic `C#` syntax.
* **Operators**: Arithmetic, comparison, logical, bitwise, and null-coalescing operators.
* **Control Flow**: Conditional statements (if/else, switch expressions) and loops (for, foreach, while).

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

* [Delegates and Events](#-10-delegates-and-events): Delegates, multicast delegates, events, and EventHandler patterns.
* [Lambda Expressions](#-11-lambda-expressions): Func, Action, Predicate, expression trees, and closures.
* [Language Integrated Query (LINQ)](#-12-language-integrated-query-linq): LINQ operators, deferred execution, query syntax, and method chaining.
* [Asynchronous Programming and Multithreading](#-13-asynchronous-programming-and-multithreading): Thread, Task, async/await, Parallel, and synchronization primitives.
* [Memory Management and Garbage Collection](#-14-memory-management-and-garbage-collection): GC generations, IDisposable, finalizers, and memory pressure.

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

## # 10. DELEGATES AND EVENTS

<br>

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

// btn.Clicked?.Invoke(...)  //  compile error — external code cannot invoke event
// btn.Clicked = null;       //  compile error — cannot assign externally
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
int result = chain(5); // (5+1)=6, (5*2)=10, (5-3)=2 ’ only last: 2
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
Func<int, bool> isPositiveLambda = x => x > 0; //  preferred

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
| `Action` | `Action<T1, T2, >` | `void` | Side-effect operations (print, log, update) |
| `Func` | `Func<T1, T2, , TResult>` | `TResult` | Transformations, computations |
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
public event EventHandler<OrderEventArgs>? OrderPlaced; // …
// public delegate void OrderHandler(Order o);          //  unnecessary

// 2. Null-conditional invoke — thread-safe raise
OrderPlaced?.Invoke(this, new OrderEventArgs(1, "Placed")); // …
// if (OrderPlaced != null) OrderPlaced(this, ...);          //  race condition

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
        => _processor.OrderPlaced -= HandleOrderPlaced; // … unsubscribe
}

// 5. Prefer Func/Action over custom delegates for simple cases
Func<int, bool>  isValid = x => x > 0;   // … concise
Action<string>   log     = Console.WriteLine; // …

// 6. Use weak event patterns for long-lived publishers / short-lived subscribers
// (WeakEventManager in WPF; or manual WeakReference<T> in other scenarios)

// 7. EventArgs should be immutable (read-only properties)
public sealed class OrderEventArgs(int orderId, string message) : EventArgs
{
    public int OrderId    { get; } = orderId;   // … read-only
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

## Q. What is the `EventHandler<TEventArgs>` pattern and how do you implement it correctly?

The **standard .NET event pattern** uses `EventHandler<TEventArgs>` where `TEventArgs` derives from `EventArgs`. This convention ensures compatibility with the .NET event system, tooling, and frameworks.

```cs
//  1. Custom EventArgs —————————————————————————————————————————
public class StockPriceChangedEventArgs : EventArgs
{
    public string Symbol    { get; }
    public decimal OldPrice { get; }
    public decimal NewPrice { get; }
    public decimal Change   => NewPrice - OldPrice;

    public StockPriceChangedEventArgs(string symbol, decimal oldPrice, decimal newPrice)
    {
        Symbol   = symbol;
        OldPrice = oldPrice;
        NewPrice = newPrice;
    }
}

//  2. Publisher — declares and raises the event ————————————————
public class StockTicker
{
    private readonly Dictionary<string, decimal> _prices = [];

    // Standard declaration: EventHandler<TEventArgs>?, nullable for no subscribers
    public event EventHandler<StockPriceChangedEventArgs>? PriceChanged;

    // Protected virtual method — allows derived classes to override raising logic
    protected virtual void OnPriceChanged(StockPriceChangedEventArgs e)
        => PriceChanged?.Invoke(this, e);  // ?.Invoke is thread-safer than null check + invoke

    public void UpdatePrice(string symbol, decimal newPrice)
    {
        decimal oldPrice = _prices.GetValueOrDefault(symbol);
        _prices[symbol] = newPrice;

        if (oldPrice != newPrice)
            OnPriceChanged(new StockPriceChangedEventArgs(symbol, oldPrice, newPrice));
    }
}

//  3. Subscriber — attaches and detaches handlers ——————————————
public class AlertSystem
{
    private readonly StockTicker _ticker;

    public AlertSystem(StockTicker ticker)
    {
        _ticker = ticker;
        _ticker.PriceChanged += OnPriceChanged;   // subscribe
    }

    private void OnPriceChanged(object? sender, StockPriceChangedEventArgs e)
    {
        if (Math.Abs(e.Change) > 5m)
            Console.WriteLine($"ALERT: {e.Symbol} moved {e.Change:+0.00;-0.00} ’ {e.NewPrice}");
    }

    public void Detach() => _ticker.PriceChanged -= OnPriceChanged;  // unsubscribe
}

//  4. Usage ———————————————————————————————————————————————————
var ticker = new StockTicker();
var alerts = new AlertSystem(ticker);

ticker.UpdatePrice("AAPL", 182.50m);   // no alert — first price
ticker.UpdatePrice("AAPL", 191.00m);   // ALERT: AAPL moved +8.50 ’ 191.00

alerts.Detach();                        // unsubscribe — prevent memory leaks

ticker.UpdatePrice("AAPL", 200.00m);   // no alert — subscriber detached

//  5. Thread-safe event invocation (local copy pattern) ————————
public class SafePublisher
{
    public event EventHandler<EventArgs>? DataReady;

    protected virtual void OnDataReady()
    {
        // Copy reference before null check — prevents race condition
        // where another thread unsubscribes between the null check and invoke
        var handler = DataReady;
        handler?.Invoke(this, EventArgs.Empty);  // ?.Invoke already does this internally
    }
}

//  6. EventHandler without custom args (simple notification) ———
public class Timer
{
    public event EventHandler? Tick;            // uses plain EventHandler
    protected virtual void OnTick()
        => Tick?.Invoke(this, EventArgs.Empty);
}
```

**Standard event pattern rules:**

| Rule | Reason |
|------|--------|
| Derive `EventArgs` subclass for custom data | Type-safe event data |
| Use `EventHandler<TEventArgs>?` (nullable) | No NullReferenceException when no subscribers |
| Raise via `protected virtual void OnXxx()` | Allows override in derived classes |
| Use `?.Invoke(this, e)` not `if (E != null) E()` | Thread-safer single evaluation |
| Always unsubscribe when done | Prevents memory leaks (publisher holds reference to subscriber) |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you prevent memory leaks caused by event subscriptions in C#?

When a subscriber registers with an event, the **publisher holds a strong reference** to the subscriber. If the publisher outlives the subscriber and the handler is never unsubscribed, the subscriber cannot be garbage collected — a classic **event-caused memory leak**.

```cs
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// PROBLEM — publisher outlives subscriber, subscriber leaks
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
static event EventHandler? StaticEvent;    // static ’ lives forever

class Subscriber
{
    public Subscriber() => StaticEvent += OnEvent;        // subscribe
    private void OnEvent(object? s, EventArgs e) => Console.WriteLine("Event fired");
    // No Dispose / unsubscribe ’ instance can never be GC'd while StaticEvent exists
}

var sub = new Subscriber();
sub = null!;                //  we think it\'s gone, but StaticEvent still holds a reference
GC.Collect();               // sub is NOT collected — memory leak

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// FIX 1 — Implement IDisposable and unsubscribe in Dispose
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
class ProperSubscriber : IDisposable
{
    private readonly StockTicker _ticker;
    private bool _disposed;

    public ProperSubscriber(StockTicker ticker)
    {
        _ticker = ticker;
        _ticker.PriceChanged += HandlePriceChanged;
    }

    private void HandlePriceChanged(object? sender, StockPriceChangedEventArgs e)
        => Console.WriteLine($"{e.Symbol}: {e.NewPrice}");

    public void Dispose()
    {
        if (_disposed) return;
        _ticker.PriceChanged -= HandlePriceChanged;   //  critical
        _disposed = true;
    }
}

// Usage with using ensures unsubscription
var ticker2 = new StockTicker();
using (var sub2 = new ProperSubscriber(ticker2))
{
    ticker2.UpdatePrice("GOOG", 175m);
}   // Dispose called ’ handler unregistered

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// FIX 2 — WeakEventManager / weak references (WPF helper)
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// In WPF: System.Windows.WeakEventManager<TEventSource, TEventArgs>
// Stores handler via WeakReference — subscriber can be GC'd even if subscribed

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// FIX 3 — Weak delegate wrapper (general-purpose)
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
class WeakEventHandler<TArgs> where TArgs : EventArgs
{
    private readonly WeakReference<EventHandler<TArgs>> _weakRef;

    public WeakEventHandler(EventHandler<TArgs> handler)
        => _weakRef = new WeakReference<EventHandler<TArgs>>(handler);

    public void Invoke(object? sender, TArgs args)
    {
        if (_weakRef.TryGetTarget(out var handler))
            handler(sender, args);
    }
}

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// FIX 4 — Lambda unsubscription (store reference)
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
EventHandler<StockPriceChangedEventArgs>? handler = null;
handler = (s, e) => Console.WriteLine(e.Symbol);
ticker2.PriceChanged += handler;
// ...
ticker2.PriceChanged -= handler;   // … works because handler variable is stored
// ticker2.PriceChanged -= (s, e) => Console.WriteLine(e.Symbol); //  new lambda — never matches

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// DIAGNOSTIC — check subscriber count via reflection (debug only)
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
static int GetSubscriberCount<T>(object publisher, string eventName) where T : Delegate
{
    var fi = publisher.GetType()
        .GetField(eventName, System.Reflection.BindingFlags.Instance | System.Reflection.BindingFlags.NonPublic);
    var del = fi?.GetValue(publisher) as T;
    return del?.GetInvocationList().Length ?? 0;
}
```

**Memory leak prevention checklist:**

| Technique | When to use |
|-----------|-------------|
| Unsubscribe in `Dispose` | Long-lived subscribers with IDisposable lifecycle |
| `using` statement | Short-scoped subscribers |
| Store lambda reference | When subscribing with a lambda expression |
| `WeakReference` wrapper | When you cannot control subscriber lifetime |
| Avoid `static` events | Static events hold references forever — use with care |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 11. LAMBDA EXPRESSIONS

<br>

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
Func<int, int> staticLambda = static x => x * 2; // … cannot capture 'multiplier'
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
Expression<Func<int, bool>> expr  = x => x > 5;   // … expression lambda
// Expression<Func<int, bool>> fail = x => { return x > 5; }; //  compile error (statement)

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
| **Expression trees** | Lambdas can be inspected/translated (EF Core ’ SQL) |
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

//  Classic closure trap in loops
var funcs = new List<Func<int>>();
for (int i = 0; i < 5; i++)
    funcs.Add(() => i); // captures the VARIABLE i, not its current value

funcs.ForEach(f => Console.Write(f() + " ")); // 5 5 5 5 5 — all see final i

// … Fix: capture a copy with a local variable
funcs.Clear();
for (int i = 0; i < 5; i++)
{
    int copy = i;
    funcs.Add(() => copy); // captures 'copy' — each iteration\'s own variable
}
funcs.ForEach(f => Console.Write(f() + " ")); // 0 1 2 3 4 — … correct

// … C# foreach — loop variable is NOT shared (safe by design)
int[] items = [10, 20, 30];
var itemFuncs = items.Select(item => (Func<int>)(() => item)).ToList();
itemFuncs.ForEach(f => Console.Write(f() + " ")); // 10 20 30 …

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
Func<int, int> refLambda = (int x) =>       // …
{
    // ref int y = ref x; //  lambdas cannot yield ref returns via Func<>
    return x + 1;
};

// 2. Cannot use 'yield return' inside lambdas
// Func<IEnumerable<int>> gen = () => { yield return 1; }; //  compile error

// 3. Cannot use 'goto', 'break', 'continue' to jump outside the lambda
// 4. Cannot use unsafe code (pointers) inside lambdas

// 5. Statement lambdas cannot be expression trees
using System.Linq.Expressions;
// Expression<Func<int,int>> e = x => { return x; }; //  compile error
Expression<Func<int, int>> e = x => x; // … expression only

// 6. Debugging is harder — stack traces show generated names like <MethodName>b__0_0

// 7. Performance — each lambda declaration creates a delegate allocation
//    Use 'static' lambda or cache delegate to avoid repeated allocations
static Func<int, int>? _cached;
_cached ??= static x => x * 2; // allocated once

// 8. Cannot be used as default argument values
// void Method(Func<int, int> f = x => x) { } //  (but null default is fine)
// void Method(Func<int, int>? f = null) { }  // …
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

//  Avoid async void lambda (no way to await/observe exceptions)
// Action asyncVoid = async () => { await Task.Delay(100); }; //  fire-and-forget, exception lost
// Instead use Func<Task>:
Func<Task> safeAsyncAction = async () => { await Task.Delay(100); }; // …

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

## Q. How do `Func<T>`, `Action<T>`, and `Predicate<T>` work as built-in delegate types in C#?

C# provides three families of built-in generic delegate types that eliminate the need to declare custom delegates for common patterns.

```cs
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// Func<TResult> and Func<T1..T16, TResult>
// — delegates that RETURN a value
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
Func<int>                      getZero    = () => 0;
Func<int, int>                 square     = x => x * x;
Func<int, int, int>            add        = (a, b) => a + b;
Func<string, int, string>      repeat     = (s, n) => string.Concat(Enumerable.Repeat(s, n));
Func<int, int, int, int>       clamp      = (v, lo, hi) => Math.Clamp(v, lo, hi);

Console.WriteLine(square(5));              // 25
Console.WriteLine(add(3, 4));             // 7
Console.WriteLine(repeat("ab", 3));       // ababab
Console.WriteLine(clamp(15, 0, 10));      // 10

// Use Func as parameter
static TResult Apply<T, TResult>(T value, Func<T, TResult> transform)
    => transform(value);

Console.WriteLine(Apply("hello", s => s.ToUpper())); // HELLO

// Compose two Funcs
Func<int, int>   doubleIt  = x => x * 2;
Func<int, string> toStr    = x => $"Value: {x}";
Func<int, string> combined = x => toStr(doubleIt(x));
Console.WriteLine(combined(7));   // Value: 14

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// Action<T1..T16>
// — delegates that RETURN void
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
Action                 printHello  = () => Console.WriteLine("Hello");
Action<string>         printName   = name => Console.WriteLine($"Hello, {name}!");
Action<string, int>    printRepeat = (s, n) => { for (int i = 0; i < n; i++) Console.Write(s); };
Action<int, int, int>  printRange  = (start, end, step) =>
    { for (int i = start; i < end; i += step) Console.Write($"{i} "); };

printHello();                  // Hello
printName("Alice");            // Hello, Alice!
printRepeat("* ", 3);         // * * *
printRange(0, 10, 2);         // 0 2 4 6 8

// Pipeline with Action
static void Process<T>(IEnumerable<T> items, Action<T> processor)
{
    foreach (var item in items)
        processor(item);
}
Process(new[] { "a", "b", "c" }, s => Console.WriteLine(s.ToUpper()));

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// Predicate<T>
// — shorthand for Func<T, bool> — used in List<T> methods
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
Predicate<int>    isEven      = n => n % 2 == 0;
Predicate<string> isLong      = s => s.Length > 5;
Predicate<string> startsWithA = s => s.StartsWith('A');

Console.WriteLine(isEven(4));           // True
Console.WriteLine(isLong("Hi"));        // False

// Used directly with List<T> methods
var words = new List<string> { "Apple", "Ant", "Banana", "Cherry", "Avocado" };
List<string> aWords = words.FindAll(startsWithA);    // ["Apple", "Ant", "Avocado"]
int idx = words.FindIndex(isLong);                   // 2 (Banana = 6 chars)
words.RemoveAll(w => w.Length < 4);                  // removes "Ant"

Console.WriteLine(string.Join(", ", aWords));        // Apple, Ant, Avocado

// Predicate<T> == Func<T, bool> — interchangeable via conversion
Func<int, bool> funcVersion = isEven.Invoke;
Predicate<int>  predVersion = new Predicate<int>(funcVersion);

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// Comparison<T>  — bonus built-in delegate for sorting
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
var people = new List<(string Name, int Age)>
{
    ("Charlie", 30), ("Alice", 25), ("Bob", 35)
};
people.Sort((a, b) => string.Compare(a.Name, b.Name, StringComparison.Ordinal));
Console.WriteLine(string.Join(", ", people.Select(p => p.Name)));  // Alice, Bob, Charlie
```

**Built-in delegate families at a glance:**

| Type | Signature | Returns | Example |
|------|-----------|---------|---------|
| `Func<TResult>` | `() => T` | Value | `() => 42` |
| `Func<T, TResult>` | `T => TResult` | Value | `x => x * 2` |
| `Func<T1,T2,TResult>` | `(T1,T2) => TResult` | Value | `(a,b) => a+b` |
| `Action` | `() => void` | void | `() => Console.WriteLine()` |
| `Action<T>` | `T => void` | void | `x => Console.WriteLine(x)` |
| `Predicate<T>` | `T => bool` | bool | `x => x > 0` |
| `Comparison<T>` | `(T,T) => int` | int | `(a,b) => a.CompareTo(b)` |
| `Converter<TIn,TOut>` | `TIn => TOut` | TOut | `s => int.Parse(s)` |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `Expression<Func<T,TResult>>` and `Func<T,TResult>` in C#?

`Func<T, TResult>` is a **compiled delegate** — executable code stored as IL. `Expression<Func<T, TResult>>` is a **data structure** representing the lambda as an abstract syntax tree that can be inspected, translated, or compiled at runtime. This distinction is critical for ORMs like Entity Framework Core.

```cs
using System.Linq.Expressions;

//  1. Func — compiled IL, not inspectable ——————————————————————
Func<int, bool> funcDelegate = x => x > 10;

// Executes directly
Console.WriteLine(funcDelegate(15));   // True
Console.WriteLine(funcDelegate(5));    // False

// Cannot inspect the body — it\'s already compiled binary code

//  2. Expression<Func<T,TResult>> — a data structure (AST) —————
Expression<Func<int, bool>> expr = x => x > 10;

// Inspect the expression tree
Console.WriteLine(expr.Body);                              // (x > 10)
Console.WriteLine(expr.Body.NodeType);                    // GreaterThan
Console.WriteLine(((BinaryExpression)expr.Body).Left);   // x
Console.WriteLine(((BinaryExpression)expr.Body).Right);  // 10

// Compile to a delegate when you need to execute it
Func<int, bool> compiled = expr.Compile();
Console.WriteLine(compiled(15));   // True

//  3. Why ORMs use Expression<Func<>> ———————————————————————————
// IQueryable<T>.Where() accepts Expression<Func<T, bool>>
// The ORM inspects the tree and translates it to SQL

// IEnumerable (LINQ to Objects) — uses Func, executes in memory
IEnumerable<int> numbers = Enumerable.Range(1, 100);
var inMemory = numbers.Where(funcDelegate);   // Func — runs as .NET code

// IQueryable (EF Core, LINQ to SQL) — uses Expression, translates to SQL
// IQueryable<Product> products = dbContext.Products;
// var fromDb = products.Where(expr);  // Expression ’ "SELECT  WHERE Price > 10"

//  4. Build an Expression tree manually ————————————————————————
// Equivalent to: x => x * x + 2 * x + 1
ParameterExpression param = Expression.Parameter(typeof(int), "x");
Expression xSquared   = Expression.Multiply(param, param);        // x * x
Expression twoX       = Expression.Multiply(Expression.Constant(2), param); // 2 * x
Expression sum        = Expression.Add(xSquared, twoX);           // x*x + 2*x
Expression full       = Expression.Add(sum, Expression.Constant(1)); // + 1

var quadratic = Expression.Lambda<Func<int, int>>(full, param).Compile();
Console.WriteLine(quadratic(3));   // 3*3 + 2*3 + 1 = 16
Console.WriteLine(quadratic(5));   // 5*5 + 2*5 + 1 = 36

//  5. Modify / rewrite an expression ——————————————————————————
// Common use case: expression visitor to rewrite predicates
class ReplaceParameterVisitor : ExpressionVisitor
{
    private readonly ParameterExpression _old, _new;
    public ReplaceParameterVisitor(ParameterExpression o, ParameterExpression n)
        => (_old, _new) = (o, n);
    protected override Expression VisitParameter(ParameterExpression node)
        => node == _old ? _new : base.VisitParameter(node);
}

// Combine two predicates: x > 5 AND x < 20
Expression<Func<int, bool>> gt5  = x => x > 5;
Expression<Func<int, bool>> lt20 = x => x < 20;

var newParam = Expression.Parameter(typeof(int), "x");
var visitor  = new ReplaceParameterVisitor(lt20.Parameters[0], gt5.Parameters[0]);
var combined2 = Expression.Lambda<Func<int, bool>>(
    Expression.AndAlso(gt5.Body, visitor.Visit(lt20.Body)),
    gt5.Parameters[0]);

var between = combined2.Compile();
Console.WriteLine(between(10));  // True
Console.WriteLine(between(25));  // False
```

**`Func<T>` vs `Expression<Func<T>>`:**

| Aspect | `Func<T, TResult>` | `Expression<Func<T, TResult>>` |
|--------|---------------------|-------------------------------|
| Nature | Compiled delegate | AST data structure |
| Executable | Directly | Requires `.Compile()` |
| Inspectable |  No | … Yes |
| Used with `IEnumerable` | … Yes (LINQ to Objects) | Converted to `Func` |
| Used with `IQueryable` |  Pulls all data to memory | … Translates to SQL/query |
| Performance | Fast execution | Slower (compilation overhead) |
| Modifiable |  No | … Via `ExpressionVisitor` |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 12. LANGUAGE INTEGRATED QUERY (LINQ)

<br>

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

//  Query syntax ————————————————————————————————————
var queryResult =
    from n in numbers
    where n > 3
    orderby n descending
    select n * 2;

//  Equivalent method syntax ————————————————————————
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
| **Translation** | None — pure C# | C# ’ SQL | C# ’ XPath-style |

```cs
// 1. LINQ to Objects — in-memory
int[] nums = [1, 2, 3, 4, 5];
var evens = nums.Where(n => n % 2 == 0).ToArray(); // [2, 4]

// 2. LINQ to SQL / EF Core — translated to SQL
// SELECT p.* FROM Products p WHERE p.Price > 100 ORDER BY p.Name
var products = await db.Products
    .Where(p => p.Price > 100)
    .OrderBy(p => p.Name)
    .ToListAsync(); // IQueryable<T> ’ SQL query at DB

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
| **Operators** | `Where`, `Select`, `OrderBy`, `GroupBy`, `Skip`, `Take` | `ToList`, `ToArray`, `Count`, `First`, `Sum`, `Any`, `ToDictionary` |

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
//  Query re-executes on each iteration
IEnumerable<int> query = data.Where(n => n > 0);
int count = query.Count(); // first iteration
int sum   = query.Sum();   // second iteration

// … Materialise once
var list  = data.Where(n => n > 0).ToList();
int count2 = list.Count;
int sum2   = list.Sum();

// 2. AsNoTracking with EF Core (covered in DB section)
// 3. Use Any() not Count() > 0
//  Counts all elements
if (data.Count() > 0) { }
// … Stops at first match
if (data.Any()) { }

// 4. Filter early — Where before Select/OrderBy
//  Sort all 1M items, then filter
data.OrderBy(n => n).Where(n => n > 1000);
// … Filter first, sort fewer items
data.Where(n => n > 1000).OrderBy(n => n);

// 5. Use concrete collection operators over LINQ when possible
//  LINQ on List when you know it\'s a List
int last = list.Last();
// … Direct property
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
// I/O-bound parallel work  ’ Parallel.ForEachAsync
// CPU-bound transformation ’ PLINQ
// CPU-bound side effects   ’ Parallel.ForEach

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

//  Caveat: re-enumeration re-executes the query
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

//  Query syntax (SQL-like) ——————————————————————————————————————————
var queryExpr =
    from p in products
    where p.Price > 50
    orderby p.Category, p.Price descending
    select new { p.Name, p.Price };

//  Equivalent method chain ——————————————————————————————————————————
var methodChain = products
    .Where(p => p.Price > 50)
    .OrderBy(p => p.Category)
    .ThenByDescending(p => p.Price)
    .Select(p => new { p.Name, p.Price });

//  Join — query syntax is more readable ————————————————————————————
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

//  Operators ONLY available in method syntax ———————————————————————
// (no query syntax equivalent)
var count = products.Count(p => p.Price > 100);
var first = products.FirstOrDefault(p => p.Price > 100);
var dist  = products.DistinctBy(p => p.Category);
var chunk = products.Chunk(2);

//  let in query syntax = intermediate Select in method syntax ——————
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
//  Violates SRP — one method filters, transforms, logs, AND saves
public async Task ProcessOrdersAsync(List<Order> orders, AppDbContext db)
{
    var result = orders
        .Where(o => o.Status == "Pending" && o.Amount > 100)
        .Select(o => { Console.WriteLine($"Processing {o.Id}"); return o; }) // side effect!
        .Select(o => new InvoiceDto(o.Id, o.Amount * 1.1m));

    db.Invoices.AddRange(result.Select(dto => new Invoice(dto)));
    await db.SaveChangesAsync();
}

// … SRP — each method has one responsibility
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
// … Methods accept IEnumerable<T> — substitutable with any collection type
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

//  LSP violation — casting to concrete type breaks substitutability
public static List<Product> FilterBad(IEnumerable<Product> products, decimal t)
{
    var list = (List<Product>)products; // throws if array or IQueryable
    return list.Where(p => p.Price > t).ToList();
}

// … Custom IEnumerable<T> that behaves like a sequence
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
//  Fat interface — query code must depend on write operations it doesn\'t use
public interface IProductRepository
{
    IQueryable<Product> Query();
    Task AddAsync(Product p);
    Task UpdateAsync(Product p);
    Task DeleteAsync(int id);
    Task SaveAsync();
}

// … Segregated interfaces
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
//  Violates DIP — high-level class depends on concrete EF Core DbSet
public class ReportService(AppDbContext db)
{
    public List<string> GetTopProductNames(int count) =>
        db.Products                    // concrete EF Core dependency
            .OrderByDescending(p => p.Price)
            .Take(count)
            .Select(p => p.Name)
            .ToList();
}

// … DIP — depend on abstraction (IQueryable<T> or IProductReader)
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
| **Output count** |  input count | = input count |
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
// IEnumerable: loads 1M rows ’ filters ’ 10 objects
// IQueryable:  DB filters ’ loads only 10 rows

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
    .ToListAsync();                 //  ToListAsync only on IQueryable; use ToList() here

var data2 = db.Products
    .Where(p => p.Price > 100)      // SQL WHERE
    .AsEnumerable()
    .Select(p => new { p.Name, Tag = FormatTag(p) })
    .ToList(); // …

static string FormatTag(Product p) => $"[{p.Name}]";
record Product(string Name, decimal Price);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use `SelectMany` in LINQ to flatten nested collections?

`SelectMany` projects each element to an `IEnumerable<T>` and then **flattens** all those sequences into a single flat sequence. It is the LINQ equivalent of a `flatMap` in other languages.

```cs
//  1. Basic flatten ————————————————————————————————————————————
var departments = new[]
{
    new { Name = "Engineering", Employees = new[] { "Alice", "Bob", "Carol" } },
    new { Name = "Design",      Employees = new[] { "Dave", "Eve" } },
    new { Name = "Marketing",   Employees = new[] { "Frank" } },
};

// Without SelectMany — nested loops required
IEnumerable<string> allEmployeesNested =
    departments.SelectMany(d => d.Employees);

Console.WriteLine(string.Join(", ", allEmployeesNested));
// Alice, Bob, Carol, Dave, Eve, Frank

//  2. With result selector — access both parent and child ———————
var withDept = departments.SelectMany(
    d => d.Employees,
    (dept, emp) => $"{emp} ({dept.Name})");

Console.WriteLine(string.Join(", ", withDept));
// Alice (Engineering), Bob (Engineering), Carol (Engineering), Dave (Design), ...

//  3. Flatten a list of lists ——————————————————————————————————
var matrix = new List<List<int>>
{
    [1, 2, 3],
    [4, 5],
    [6, 7, 8, 9],
};

List<int> flat = matrix.SelectMany(row => row).ToList();
Console.WriteLine(string.Join(", ", flat));  // 1, 2, 3, 4, 5, 6, 7, 8, 9

//  4. Flatten with filtering ———————————————————————————————————
var orders = new[]
{
    new { Id = 1, Items = new[] { "Widget", "Gadget", "Widget" } },
    new { Id = 2, Items = new[] { "Gizmo" } },
    new { Id = 3, Items = new[] { "Widget", "Doohickey" } },
};

var widgetOrderIds = orders
    .Where(o => o.Items.Contains("Widget"))
    .Select(o => o.Id);
Console.WriteLine(string.Join(", ", widgetOrderIds));  // 1, 3

// All distinct items ever ordered
var distinctItems = orders
    .SelectMany(o => o.Items)
    .Distinct()
    .OrderBy(i => i);
Console.WriteLine(string.Join(", ", distinctItems));  // Doohickey, Gadget, Gizmo, Widget

//  5. Query syntax equivalent ——————————————————————————————————
var queryResult =
    from d in departments
    from emp in d.Employees         // second `from` = SelectMany
    where emp.StartsWith('A') || emp.StartsWith('E')
    select $"{emp} — {d.Name}";

foreach (var r in queryResult)
    Console.WriteLine(r);
// Alice — Engineering
// Eve — Design

//  6. String as char sequence (practical) ——————————————————————
string[] words = ["hello", "world"];
char[] allChars = words.SelectMany(w => w).Distinct().OrderBy(c => c).ToArray();
Console.WriteLine(new string(allChars));  // dehlorw

//  7. Cross join (Cartesian product) ——————————————————————————
var colors  = new[] { "Red", "Blue" };
var sizes   = new[] { "S", "M", "L" };

var variants = colors.SelectMany(
    _ => sizes,
    (color, size) => $"{color}-{size}");

Console.WriteLine(string.Join(", ", variants));
// Red-S, Red-M, Red-L, Blue-S, Blue-M, Blue-L
```

**`Select` vs `SelectMany`:**

| Aspect | `Select` | `SelectMany` |
|--------|----------|--------------|
| Input | `T` | `T` |
| Output per element | Single `TResult` | `IEnumerable<TResult>` |
| Result shape | Same count, possibly nested | Flattened single sequence |
| Use case | Transform 1-to-1 | Flatten 1-to-many |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are LINQ set operators (`Union`, `Intersect`, `Except`, `Distinct`) and how are they used?

LINQ set operators work on sequences the same way mathematical sets work — they compare elements for equality and produce results without duplicates (unless using the `By` or `WithComparer` overloads).

```cs
int[] a = [1, 2, 3, 4, 5];
int[] b = [3, 4, 5, 6, 7];

//  Distinct — remove duplicates ————————————————————————————————
int[] withDups = [1, 2, 2, 3, 3, 3, 4];
int[] unique = withDups.Distinct().ToArray();
Console.WriteLine(string.Join(", ", unique));   // 1, 2, 3, 4

//  Union — all elements from both, no duplicates ———————————————
int[] union = a.Union(b).ToArray();
Console.WriteLine(string.Join(", ", union));    // 1, 2, 3, 4, 5, 6, 7

//  Intersect — only elements in BOTH ———————————————————————————
int[] intersect = a.Intersect(b).ToArray();
Console.WriteLine(string.Join(", ", intersect)); // 3, 4, 5

//  Except — elements in a but NOT in b (set difference) ————————
int[] except = a.Except(b).ToArray();
Console.WriteLine(string.Join(", ", except));    // 1, 2

// Reverse — elements in b but not a
int[] exceptReverse = b.Except(a).ToArray();
Console.WriteLine(string.Join(", ", exceptReverse)); // 6, 7

//  DistinctBy / UnionBy / IntersectBy / ExceptBy (.NET 6+) ————
record Person(string Name, int DeptId);

var team1 = new[]
{
    new Person("Alice", 1), new Person("Bob", 2), new Person("Carol", 1),
};
var team2 = new[]
{
    new Person("Dave", 1), new Person("Alice", 3), new Person("Eve", 2),
};

// UnionBy — merge teams, deduplicate by Name
var merged = team1.UnionBy(team2, p => p.Name);
Console.WriteLine(string.Join(", ", merged.Select(p => p.Name)));
// Alice, Bob, Carol, Dave, Eve

// IntersectBy — people in BOTH teams (by name)
var inBoth = team1.IntersectBy(team2.Select(p => p.Name), p => p.Name);
Console.WriteLine(string.Join(", ", inBoth.Select(p => p.Name)));  // Alice

// ExceptBy — people only in team1 (not in team2, by name)
var onlyTeam1 = team1.ExceptBy(team2.Select(p => p.Name), p => p.Name);
Console.WriteLine(string.Join(", ", onlyTeam1.Select(p => p.Name))); // Bob, Carol

// DistinctBy — one person per department (first occurrence wins)
var onePerDept = team1.DistinctBy(p => p.DeptId);
Console.WriteLine(string.Join(", ", onePerDept.Select(p => p.Name))); // Alice, Bob

//  Custom equality comparer ————————————————————————————————————
class CaseInsensitiveComparer : IEqualityComparer<string>
{
    public bool Equals(string? x, string? y)
        => string.Equals(x, y, StringComparison.OrdinalIgnoreCase);
    public int GetHashCode(string obj) => obj.ToLowerInvariant().GetHashCode();
}

string[] words1 = ["apple", "Banana", "cherry"];
string[] words2 = ["APPLE", "Date", "Cherry"];

var caseInsensitiveUnion = words1.Union(words2, new CaseInsensitiveComparer());
Console.WriteLine(string.Join(", ", caseInsensitiveUnion));
// apple, Banana, cherry, Date
```

**Set operator summary:**

| Operator | Returns | Description |
|----------|---------|-------------|
| `Distinct()` | `IEnumerable<T>` | Unique elements from one sequence |
| `DistinctBy(key)` | `IEnumerable<T>` | Unique by key selector (.NET 6+) |
| `Union(b)` | `IEnumerable<T>` | All unique elements from a and b |
| `Intersect(b)` | `IEnumerable<T>` | Elements in both a and b |
| `Except(b)` | `IEnumerable<T>` | Elements in a but not b |
| `UnionBy/IntersectBy/ExceptBy` | `IEnumerable<T>` | Keyed versions (.NET 6+) |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are LINQ partitioning and element operators in C#?

**Partitioning operators** (`Take`, `Skip`, `TakeWhile`, `SkipWhile`, `Chunk`) split a sequence into parts. **Element operators** (`First`, `Last`, `Single`, `ElementAt`, `Any`, `All`, `Count`) retrieve or test individual elements.

```cs
int[] numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// PARTITIONING
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••

// Take — first N elements
Console.WriteLine(string.Join(", ", numbers.Take(3)));          // 1, 2, 3

// Skip — skip first N elements
Console.WriteLine(string.Join(", ", numbers.Skip(7)));          // 8, 9, 10

// Take + Skip — paging
int pageSize = 3, page = 2;
var paged = numbers.Skip((page - 1) * pageSize).Take(pageSize);
Console.WriteLine(string.Join(", ", paged));                    // 4, 5, 6

// TakeWhile — take while condition is true (stops at first false)
Console.WriteLine(string.Join(", ", numbers.TakeWhile(n => n < 5)));  // 1, 2, 3, 4

// SkipWhile — skip while condition is true, then take the rest
Console.WriteLine(string.Join(", ", numbers.SkipWhile(n => n < 5)));  // 5, 6, 7, 8, 9, 10

// TakeLast / SkipLast (.NET Core 2.0+)
Console.WriteLine(string.Join(", ", numbers.TakeLast(3)));      // 8, 9, 10
Console.WriteLine(string.Join(", ", numbers.SkipLast(3)));      // 1, 2, 3, 4, 5, 6, 7

// Chunk — split into fixed-size batches (.NET 6+)
foreach (int[] chunk in numbers.Chunk(3))
    Console.WriteLine(string.Join(", ", chunk));
// 1, 2, 3
// 4, 5, 6
// 7, 8, 9
// 10

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// ELEMENT OPERATORS
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
int[] evens = numbers.Where(n => n % 2 == 0).ToArray();   // [2,4,6,8,10]
int[] empty = [];

// First / Last — throw if sequence is empty
Console.WriteLine(evens.First());                    // 2
Console.WriteLine(evens.Last());                     // 10
Console.WriteLine(evens.First(n => n > 6));          // 8

// FirstOrDefault / LastOrDefault — return default(T) if empty
Console.WriteLine(empty.FirstOrDefault());           // 0 (int default)
Console.WriteLine(empty.FirstOrDefault(-1));         // -1 (custom default, .NET 6+)
Console.WriteLine(evens.FirstOrDefault(n => n > 100, -99)); // -99

// Single — exactly one element; throws if 0 or >1
Console.WriteLine(evens.Single(n => n == 6));        // 6
// evens.Single()   throws — more than one element

// SingleOrDefault — 0 or 1 elements; throws if >1
Console.WriteLine(evens.SingleOrDefault(n => n == 5));  // 0 (not found)
Console.WriteLine(evens.SingleOrDefault(n => n == 5, -1)); // -1

// ElementAt / ElementAtOrDefault
Console.WriteLine(evens.ElementAt(2));               // 6
Console.WriteLine(evens.ElementAtOrDefault(99));     // 0 (out of range)

//  Boolean aggregates ——————————————————————————————————————————
Console.WriteLine(numbers.Any());                    // True (not empty)
Console.WriteLine(empty.Any());                      // False
Console.WriteLine(numbers.Any(n => n > 9));          // True

Console.WriteLine(numbers.All(n => n > 0));          // True
Console.WriteLine(numbers.All(n => n > 5));          // False

Console.WriteLine(numbers.Contains(7));              // True

//  Count / LongCount ———————————————————————————————————————————
Console.WriteLine(numbers.Count());                  // 10
Console.WriteLine(numbers.Count(n => n % 3 == 0));  // 3  (3, 6, 9)
Console.WriteLine(numbers.LongCount());              // 10L

//  Min, Max, Sum, Average ——————————————————————————————————————
Console.WriteLine(numbers.Min());    // 1
Console.WriteLine(numbers.Max());    // 10
Console.WriteLine(numbers.Sum());    // 55
Console.WriteLine(numbers.Average()); // 5.5

//  MinBy / MaxBy (.NET 6+) —————————————————————————————————————
record Product2(string Name, decimal Price);
var products = new[] { new Product2("A", 5m), new Product2("B", 2m), new Product2("C", 8m) };
Console.WriteLine(products.MinBy(p => p.Price)?.Name);  // B
Console.WriteLine(products.MaxBy(p => p.Price)?.Name);  // C
```

**Operator behaviour on empty sequences:**

| Operator | Empty sequence behaviour |
|----------|--------------------------|
| `First()` | Throws `InvalidOperationException` |
| `FirstOrDefault()` | Returns `default(T)` or custom default |
| `Single()` | Throws `InvalidOperationException` |
| `SingleOrDefault()` | Returns `default(T)` or custom default |
| `Last()` | Throws `InvalidOperationException` |
| `Any()` | Returns `false` |
| `Count()` | Returns `0` |
| `Min()` / `Max()` | Throws on empty non-nullable |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 13. ASYNCHRONOUS PROGRAMMING AND MULTITHREADING

<br>

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

// … Type-safe approach — closure over strongly-typed variables
int id    = 7;
string name = "Alice";
var t3 = new Thread(() =>
{
    // id and name captured by reference — fully type-safe, no casting
    Console.WriteLine($"Worker {id}: {name}");
});
t3.Start();

// … Pass a typed object via closure
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

// Preferred: Task<T> — return values built-in, no shared variable needed
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
| **Unwraps nested tasks** | … Automatically |  Must call `.Unwrap()` manually |
| **Default scheduler** | `ThreadPool` | Current `TaskScheduler` |
| **LongRunning option** |  Not supported | … `TaskCreationOptions.LongRunning` |
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

//  Rules:
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
// Short critical section on same machine ’ lock
// Need timeout / TryEnter             ’ Monitor.TryEnter
// Limit concurrency (e.g., DB pool)   ’ SemaphoreSlim
// Signal all waiting threads           ’ ManualResetEventSlim
// Signal one thread, auto-reset        ’ AutoResetEvent
// Count-down to zero                   ’ CountdownEvent
// Phase-by-phase parallel work         ’ Barrier
// Concurrent reads, rare writes        ’ ReaderWriterLockSlim
// Nanosecond-critical inner loops      ’ SpinLock
// Cross-process lock                   ’ Mutex

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
| **Async** |  |  | … `WaitAsync` |
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

// … Modern approach: Channel<T> (preferred in .NET 5+)
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

//  Parallel.For with async — use Parallel.ForEachAsync (.NET 6+)
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
// … CPU-bound: image processing, data crunching, compression
// … Parallel independent tasks: batch file processing
// … Background work: keep UI responsive
// … I/O-bound: async/await without dedicated threads

// When to AVOID:
//  Simple sequential logic — adds complexity with no benefit
//  Shared state that\'s complex to synchronize
//  Very short tasks — thread creation overhead exceeds benefit

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

// Multi-threaded: tasks run in parallel — total time  max of each
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
// … USE multithreading when:

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

//  AVOID multithreading when:

// 1. Simple sequential logic — no gain, only complexity
// BAD:
int badResult = await Task.Run(() => 2 + 2);

// GOOD:
int goodResult = 2 + 2;

// 2. Tasks are too short — thread overhead > benefit
// BAD: threading a 1 s operation
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
    Thread.SpinWait(1); // spin until we set flag 0’1
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
| **Reusable** | … Automatically resets for each phase |  One-shot (or manually reset) |
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
//  Thread.Abort — NOT available in .NET 5+
// var t = new Thread(...);
// t.Abort(); // throws PlatformNotSupportedException on .NET 5+

// … Graceful cancellation via CancellationToken (recommended)
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

// … Volatile flag — simple polling (no Task)
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
| **Upgradeable read lock** |  | … `EnterUpgradeableReadLock` |
| **Recommendation** | Legacy (avoid) | … Use this |

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
| **Prevents caching** | … | … (implicit) | … (explicit fence) |
| **Prevents reordering** | Partial (acquire/release) | … | … (full fence) |
| **Atomic compound ops** |  | … |  |
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
// One thread writes, one thread reads a simple flag  ’ volatile
// Atomic increment / compare-and-swap               ’ Interlocked
// Custom lock-free algorithm with ordering needs    ’ MemoryBarrier / Volatile.Read/Write
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
// Mixed:     experiment; start with 2 — ProcessorCount for I/O

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

// … Mitigation 1: Interlocked — no lock needed for simple atomic ops
int atomicCounter = 0;
await Task.WhenAll(Enumerable.Range(0, 1000).Select(_ =>
    Task.Run(() => Interlocked.Increment(ref atomicCounter))));

// … Mitigation 2: Lock striping — partition data across multiple locks
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

// … Mitigation 3: ReaderWriterLockSlim — allow concurrent reads
var rwl = new ReaderWriterLockSlim();
var dict = new Dictionary<string, int> { ["key"] = 0 };

// Many readers can proceed simultaneously
await Task.WhenAll(Enumerable.Range(0, 100).Select(_ => Task.Run(() =>
{
    rwl.EnterReadLock();
    try { _ = dict["key"]; }
    finally { rwl.ExitReadLock(); }
})));

// … Mitigation 4: Reduce lock scope — keep critical section minimal
int result;
lock (sharedLock) result = counter; // read fast under lock
Console.WriteLine(ExpensiveProcess(result)); // heavy work OUTSIDE lock

// … Mitigation 5: ConcurrentDictionary — built-in lock striping
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
| **Best for** | Sections taking > ~1 s | Sections taking < ~1 s |
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

//  Rules:
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

## Q. What is `ConfigureAwait(false)` and when should it be used?

When a `Task` is awaited, by default .NET tries to **resume on the original synchronisation context** (e.g., the UI thread, or an ASP.NET Classic request context). `ConfigureAwait(false)` instructs the runtime to resume on **any available thread-pool thread** instead, which avoids unnecessary context switches and potential deadlocks.

```cs
//  1. Default behaviour (ConfigureAwait(true) / omitted) ———————
// Resumes on the captured synchronisation context (e.g. UI thread)
async Task LoadAndDisplayAsync()
{
    var data = await FetchDataAsync();   // resumes on UI thread  important for WPF/WinForms
    label.Text = data;                   // … safe — UI update on UI thread
}

//  2. Library code — always use ConfigureAwait(false) ——————————
// Library methods should NOT capture the caller\'s context
public static async Task<string> FetchDataAsync(string url)
{
    using var client = new HttpClient();
    // ConfigureAwait(false) — resume on any thread pool thread
    string json = await client.GetStringAsync(url).ConfigureAwait(false);
    return json;   // no context-sensitive work here
}

//  3. Deadlock scenario (ASP.NET Classic / WPF without ConfigureAwait) 
// BAD: .Result on async method in single-threaded context causes deadlock
// string result = FetchDataAsync("https://example.com").Result; //  DEADLOCK

// GOOD: await end-to-end, or use ConfigureAwait(false) in the library
public static async Task<string> SafeFetchAsync(string url)
{
    using var client = new HttpClient();
    return await client.GetStringAsync(url).ConfigureAwait(false);
}

//  4. ASP.NET Core — no SynchronisationContext, so ConfigureAwait(false)
//    is not required for correctness, but still a good habit in library code
public async Task<IActionResult> GetAsync()
{
    // In ASP.NET Core, SynchronisationContext is null — both are equivalent
    var data = await FetchDataAsync("https://api.example.com/data");
    return Ok(data);
}

//  5. ConfigureAwait in a loop —————————————————————————————————
public static async Task ProcessItemsAsync(IEnumerable<int> ids)
{
    foreach (int id in ids)
    {
        var result = await LoadItemAsync(id).ConfigureAwait(false);
        Console.WriteLine(result);
    }
}

static Task<string> LoadItemAsync(int id) => Task.FromResult($"Item-{id}");
```

**When to use / not use `ConfigureAwait(false)`:**

| Scenario | Use `ConfigureAwait(false)`? | Reason |
|----------|------------------------------|--------|
| Library / NuGet package code | … Always | Don\'t impose context on callers |
| ASP.NET Core controller / middleware | Optional | No SynchronisationContext |
| WPF / WinForms UI method |  No | Need to return to UI thread |
| ASP.NET Classic (System.Web) | … Yes | Avoid deadlocks on captured context |
| Unit test with `async` | … Yes | Test runners may have a context |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is `IAsyncEnumerable<T>` and how do you use `await foreach` in C#?

`IAsyncEnumerable<T>` (C# 8 / .NET Standard 2.1+) enables **asynchronous streaming** — producing and consuming items one at a time without buffering the entire result set in memory. It combines the pull-based iteration of `IEnumerable<T>` with asynchrony.

```cs
using System.Runtime.CompilerServices;

//  1. Producing an async stream ————————————————————————————————
// Use `yield return` inside an `async` method returning IAsyncEnumerable<T>
static async IAsyncEnumerable<int> GenerateNumbersAsync(
    int count,
    [EnumeratorCancellation] CancellationToken ct = default)
{
    for (int i = 1; i <= count; i++)
    {
        ct.ThrowIfCancellationRequested();
        await Task.Delay(50, ct);   // simulate async work (DB query, HTTP, etc.)
        yield return i;
    }
}

//  2. Consuming with `await foreach` ———————————————————————————
await foreach (int number in GenerateNumbersAsync(5))
    Console.WriteLine(number);   // prints 1..5 as they arrive

//  3. CancellationToken support ————————————————————————————————
using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(2));
try
{
    await foreach (int n in GenerateNumbersAsync(100, cts.Token))
        Console.WriteLine(n);
}
catch (OperationCanceledException)
{
    Console.WriteLine("Stream cancelled.");
}

//  4. ConfigureAwait on IAsyncEnumerable ———————————————————————
await foreach (int n in GenerateNumbersAsync(5).ConfigureAwait(false))
    Console.WriteLine(n);

//  5. Real-world: streaming database rows ——————————————————————
// (EF Core 3+ supports IAsyncEnumerable via AsAsyncEnumerable())
// async IAsyncEnumerable<Order> StreamOrdersAsync(AppDbContext db)
// {
//     await foreach (var order in db.Orders.AsAsyncEnumerable())
//         yield return order;
// }

//  6. Stream large file lines without loading all into memory ——
static async IAsyncEnumerable<string> ReadLinesAsync(
    string path,
    [EnumeratorCancellation] CancellationToken ct = default)
{
    await using var fs = File.OpenRead(path);
    using var reader = new StreamReader(fs);
    string? line;
    while ((line = await reader.ReadLineAsync(ct)) is not null)
        yield return line;
}

//  7. LINQ-style on async streams (System.Linq.Async NuGet) ————
// var evens = GenerateNumbersAsync(10).Where(n => n % 2 == 0);
// await foreach (var n in evens) Console.WriteLine(n);

//  8. Collect to list when needed ——————————————————————————————
var items = new List<int>();
await foreach (int n in GenerateNumbersAsync(5))
    items.Add(n);
Console.WriteLine(string.Join(", ", items));  // 1, 2, 3, 4, 5
```

**`IAsyncEnumerable<T>` vs alternatives:**

| Approach | Buffering | Back-pressure | Best for |
|----------|-----------|---------------|----------|
| `Task<List<T>>` | All items at once | No | Small result sets |
| `IAsyncEnumerable<T>` | One item at a time | Yes (pull) | Large / infinite streams |
| `Channel<T>` | Configurable | Yes | Producer-consumer pipelines |
| `IObservable<T>` (Rx) | Push-based | Complex | Event-driven streams |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the pitfalls of `async void` methods in C#?

`async void` is allowed only for event handlers. Using it anywhere else creates silent, hard-to-debug failures because **exceptions escape the caller\'s context** and cannot be awaited.

```cs
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// PROBLEM 1 — Unhandled exceptions crash the process
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
async void FireAndForget()                 //  async void — avoid
{
    await Task.Delay(100);
    throw new InvalidOperationException("Oops!");  // crashes the process — cannot be caught by caller
}

try
{
    FireAndForget();    // returns immediately — exception is NOT catchable here
}
catch (Exception)
{
    //  Never reached — the exception happens after the await
    Console.WriteLine("This will never print");
}

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// PROBLEM 2 — Cannot be awaited or composed
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
async void LoadAsync() { await Task.Delay(500); Console.WriteLine("Done"); }

// await LoadAsync();     //  compile error — void is not awaitable
// Task t = LoadAsync();  //  compile error — returns void, not Task

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// CORRECT ALTERNATIVES
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••

//  1. Return Task — preferred for all non-event-handler async methods
async Task LoadDataAsync()
{
    await Task.Delay(100);
    Console.WriteLine("Data loaded");
}
await LoadDataAsync();   // … awaitable, exception propagates normally

//  2. async void is ONLY acceptable for event handlers
// (because event delegates have a void return signature)
public class MyForm
{
    private Button _btn = new Button();

    public MyForm()
    {
        _btn.Click += OnButtonClickAsync;   // … event handler — async void OK
    }

    private async void OnButtonClickAsync(object? sender, EventArgs e)
    {
        try
        {
            await LoadDataAsync();
        }
        catch (Exception ex)
        {
            // … Always wrap async void event handlers in try/catch
            Console.Error.WriteLine($"Event handler error: {ex.Message}");
        }
    }
}

//  3. Fire-and-forget with proper error handling ———————————————
static Task StartBackgroundWork()
{
    return Task.Run(async () =>
    {
        try
        {
            await Task.Delay(100);
            Console.WriteLine("Background work done");
        }
        catch (Exception ex)
        {
            Console.Error.WriteLine($"Background error: {ex.Message}");
        }
    });
}

_ = StartBackgroundWork();   // discard Task intentionally — fire-and-forget pattern

//  4. Top-level async in older frameworks ——————————————————————
// BEFORE C# 7.1: Main couldn\'t be async ’ temptation to use async void
// async void Main() { }  // 

// C# 7.1+: async Main is fully supported
// static async Task Main(string[] args) { await DoWorkAsync(); }  // …
```

**`async void` rules:**

| Rule | Reason |
|------|--------|
| Never use `async void` except for event handlers | Exceptions crash the process |
| Always `try/catch` inside `async void` event handlers | Last line of defence |
| Replace `async void` with `async Task` everywhere else | Awaitable, composable, testable |
| For fire-and-forget, use `_ = task` with internal error handling | Explicitly marks the intent |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 14. MEMORY MANAGEMENT AND GARBAGE COLLECTION

<br>

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
list = null;                           // now unreachable ’ eligible for GC

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
Console.WriteLine(GC.GetGeneration(obj)); // 1 (survived ’ promoted)

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

// GC phases: Mark ’ Sweep ’ Compact
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
var holder = new DataHolder(); // holder reference on stack, object on heap ’ GC manages

// Struct on the stack
struct Point { public int X, Y; }
Point p = new Point { X = 1, Y = 2 }; // entirely on stack — NO GC

// Struct on the heap (boxed or inside a class/array)
object boxed = p;          // boxing — copied to heap ’ GC manages
Point[] points = new Point[10]; // array on heap, but Point values inline in array

// Summary:
// Primitive/value types on stack ’ freed by stack unwind (no GC)
// Reference types on heap ’ GC manages
// Boxed value types on heap ’ GC manages
// Value types as fields of heap objects ’ GC manages (as part of parent object)

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

// Cycle 1: object found unreachable ’ moved to finalization queue (NOT reclaimed yet)
// Cycle 2: finalizer thread runs, GC reclaims memory

// … Dispose pattern — call GC.SuppressFinalize to skip the second cycle
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
        GC.SuppressFinalize(this); // remove from finalization queue ’ single cycle
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
//  GC cannot free unmanaged resources — you must do it
var handle = System.Runtime.InteropServices.Marshal.AllocHGlobal(1024);
// ... use handle
System.Runtime.InteropServices.Marshal.FreeHGlobal(handle); // manual cleanup required

// … Wrap in SafeHandle or IDisposable for automatic cleanup
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

//  Is it good practice to force GC?
//  Almost never — reasons:
// - Promotes objects to higher generations unnecessarily (Gen 0 ’ Gen 1 ’ Gen 2)
// - Disrupts GC\'s self-tuning heuristics
// - Causes latency spikes (stop-the-world pause)
// - Rarely improves performance; often makes it worse

// … Acceptable rare cases:
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
// - Visual Studio Diagnostic Tools ’ Memory Usage ’ Snapshots
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
        GC.SuppressFinalize(this); // … skip finalizer — memory reclaimed in ONE cycle
    }

    private static IntPtr OpenFile(string path) => new(1);
    private static void CloseFile(IntPtr h) { }
}

// Always call Dispose with 'using'
using var fw = new FileWrapper("data.bin");
// Dispose called ’ GC.SuppressFinalize ’ no finalizer overhead

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
//  Lazy<T>  — built-in BCL class
//  Custom lazy patterns (lazy fields, lazy properties)

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
| **Cross-process** | … Named semaphores |  In-process only |
| **Async support** |  | … `WaitAsync()` |
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

// t1.Start(); t2.Start();  DEADLOCK! Both threads wait forever

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
// counter++ ’ IL: ldloc, ldc.i4.1, add, stloc — three non-atomic operations

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
// … Await it exactly once
// … Don\'t store and await later (use AsTask() first)
// … Don\'t await from multiple consumers
// … Use when method frequently returns synchronously

// Converting ValueTask to Task when you need to share/store
ValueTask<int> vt = GetCachedCountAsync();
Task<int> task = vt.AsTask(); // convert — now safely multi-awaitable
int r1 = await task;
int r2 = await task; // … safe after AsTask()

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

## Q. How do you implement the `IDisposable` pattern correctly in C#?

`IDisposable` is used to release **unmanaged resources** (file handles, database connections, sockets, native memory) deterministically — without waiting for the garbage collector. The complete "dispose pattern" combines a public `Dispose()` method with a `~finalizer` as a safety net.

```cs
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// 1. Simple IDisposable — no finalizer needed (wraps another IDisposable)
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
public class FileProcessor : IDisposable
{
    private StreamReader? _reader;
    private bool _disposed;

    public FileProcessor(string path)
        => _reader = new StreamReader(path);

    public string? ReadLine() => _reader?.ReadLine();

    public void Dispose()
    {
        if (_disposed) return;
        _reader?.Dispose();   // dispose managed resource
        _reader = null;
        _disposed = true;
    }
}

// Usage — always use `using` for IDisposable
using var processor = new FileProcessor("data.txt");
string? line = processor.ReadLine();

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// 2. Full Dispose Pattern — when you hold UNMANAGED resources directly
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
public class NativeResourceHolder : IDisposable
{
    // Managed resource (another IDisposable)
    private Stream? _stream;

    // Unmanaged resource (IntPtr, SafeHandle, etc.)
    private IntPtr _nativeHandle;

    private bool _disposed;

    public NativeResourceHolder(string path)
    {
        _stream       = File.OpenRead(path);
        _nativeHandle = AllocateNativeResource();   // hypothetical P/Invoke
    }

    //  Public entry point ———————————————————————————————————
    public void Dispose()
    {
        Dispose(disposing: true);
        GC.SuppressFinalize(this);   // no need for finalizer — Dispose already ran
    }

    //  Core logic — called by both Dispose() and finalizer ——
    protected virtual void Dispose(bool disposing)
    {
        if (_disposed) return;

        if (disposing)
        {
            // Safe to access managed objects here (Dispose called)
            _stream?.Dispose();
            _stream = null;
        }

        // Always release unmanaged resources
        if (_nativeHandle != IntPtr.Zero)
        {
            FreeNativeResource(_nativeHandle);   // hypothetical P/Invoke
            _nativeHandle = IntPtr.Zero;
        }

        _disposed = true;
    }

    //  Finalizer — safety net if caller forgot Dispose() ————
    ~NativeResourceHolder() => Dispose(disposing: false);

    private static IntPtr AllocateNativeResource() => new IntPtr(1);   // placeholder
    private static void FreeNativeResource(IntPtr handle) { }           // placeholder
}

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// 3. Preferred modern approach — wrap unmanaged handle in SafeHandle
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
using Microsoft.Win32.SafeHandles;

public class SafeResourceHolder : IDisposable
{
    private SafeFileHandle? _handle;
    private Stream?         _stream;
    private bool            _disposed;

    public SafeResourceHolder(string path)
    {
        _handle = File.OpenHandle(path);
        _stream = new FileStream(_handle, FileAccess.Read);
    }

    public void Dispose()
    {
        if (_disposed) return;
        _stream?.Dispose();   // disposes both stream and handle
        _disposed = true;
        // No finalizer needed — SafeHandle has its own
    }
}

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// 4. IAsyncDisposable — for async cleanup (C# 8+)
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
public class AsyncDbConnection : IAsyncDisposable
{
    private bool _disposed;

    public async ValueTask DisposeAsync()
    {
        if (_disposed) return;
        await CloseConnectionAsync();   // async teardown
        _disposed = true;
    }

    private static Task CloseConnectionAsync() => Task.Delay(10);
}

await using var conn = new AsyncDbConnection();
// ... use conn ...
// DisposeAsync called automatically at end of scope
```

**Dispose pattern summary:**

| Scenario | Use |
|----------|-----|
| Wraps only other `IDisposable` | Simple `Dispose()` — no finalizer |
| Holds unmanaged resource directly | Full pattern with `Dispose(bool)` + finalizer |
| Unmanaged handle | `SafeHandle` subclass — preferred over raw `IntPtr` |
| Async teardown required | `IAsyncDisposable` + `await using` |
| Always call GC.SuppressFinalize | After successful `Dispose()` to skip finalizer queue |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the Large Object Heap (LOH) and how does it affect memory and GC performance?

The .NET GC splits the managed heap into the **Small Object Heap (SOH)** for objects < 85,000 bytes and the **Large Object Heap (LOH)** for objects ≥ 85,000 bytes. The LOH is treated differently and can cause **memory pressure** and **fragmentation**.

```cs
//  1. What goes to the LOH —————————————————————————————————————
// Any single managed object >= 85,000 bytes (default threshold)
// Most common: large arrays (byte[], int[], string with >40K chars)

byte[] small = new byte[84_999];  // SOH — Gen 0
byte[] large = new byte[85_000];  // LOH — collected only during Gen 2 GC

//  2. LOH is collected only with Gen 2 (Full GC) ———————————————
// SOH: Gen 0 ’ Gen 1 ’ Gen 2 (short-lived objects collected quickly)
// LOH: always collected together with Gen 2 ’ more expensive, less frequent

//  3. LOH fragmentation ————————————————————————————————————————
// LOH is NOT compacted by default (unlike SOH)
// Allocate and free many large arrays ’ holes appear ’ OutOfMemoryException
// even when total free memory is enough (fragmentation)
void DemonstrateFragmentation()
{
    var arrays = new List<byte[]>();
    for (int i = 0; i < 100; i++)
        arrays.Add(new byte[100_000]);   // 100 — 100KB = 10 MB on LOH

    // Release every other one
    for (int i = 0; i < arrays.Count; i += 2)
        arrays[i] = null!;

    GC.Collect();   // compacts SOH but NOT LOH by default — fragmented holes remain
}

//  4. Force LOH compaction (one-time, expensive) ———————————————
GCSettings.LargeObjectHeapCompactionMode = GCLargeObjectHeapCompactionMode.CompactOnce;
GC.Collect();   // compacts LOH this one time, then resets to NoCompaction

//  5. Best practice: ArrayPool<T> to avoid LOH allocations —————
using System.Buffers;

void ProcessData(int size)
{
    //  Allocates a new large array — goes to LOH, increases GC pressure
    // byte[] buffer = new byte[size];

    // … Rent from pool — reuses existing arrays, no LOH pressure
    byte[] buffer = ArrayPool<byte>.Shared.Rent(size);
    try
    {
        // Use buffer (may be slightly larger than requested)
        Array.Clear(buffer, 0, size);
        Console.WriteLine($"Processing {buffer.Length} bytes");
    }
    finally
    {
        ArrayPool<byte>.Shared.Return(buffer);   // return to pool — NOT freed
    }
}

ProcessData(200_000);   // large but no LOH allocation

//  6. Span<T> and Memory<T> — zero-copy, stack-friendly slices —
byte[] fullBuffer = new byte[1_000_000];

// Span<T> — stack-allocated slice reference (cannot be stored in heap fields)
Span<byte> slice = fullBuffer.AsSpan(0, 100);
slice.Fill(0xFF);

// Memory<T> — heap-compatible async-friendly slice
Memory<byte> memSlice = fullBuffer.AsMemory(100, 200);
await ProcessMemoryAsync(memSlice);

static async Task ProcessMemoryAsync(Memory<byte> mem)
{
    await Task.Yield();
    Console.WriteLine($"Processing {mem.Length} bytes asynchronously");
}

//  7. Monitor LOH size —————————————————————————————————————————
long lohSize = GC.GetGCMemoryInfo().GenerationInfo[3].SizeAfterBytes;
Console.WriteLine($"LOH size after GC: {lohSize / 1024:N0} KB");
```

**LOH rules of thumb:**

| Rule | Reason |
|------|--------|
| Objects ≥ 85 KB go to LOH | Default GC threshold |
| LOH collected only with Gen 2 GC | More expensive, less frequent |
| LOH is NOT compacted by default | Fragmentation risk |
| Use `ArrayPool<T>.Shared.Rent()` | Reuse large arrays, avoid LOH pressure |
| Use `Span<T>` / `Memory<T>` | Zero-copy slices, no allocation |
| Compact LOH only when needed | `GCLargeObjectHeapCompactionMode.CompactOnce` |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are `Span<T>` and `Memory<T>` in C# and how do they reduce allocations?

`Span<T>` and `Memory<T>` are **allocation-free slice types** that let you work with contiguous regions of memory — whether from arrays, stack, or native memory — without copying.

```cs
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// Span<T> — stack-only, synchronous, ultra-fast
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••

//  1. Slice an array without copying ———————————————————————————
int[] numbers = [10, 20, 30, 40, 50, 60, 70];
Span<int> middle = numbers.AsSpan(2, 3);   // [30, 40, 50] — no copy
middle[1] = 99;                             // mutates the original array
Console.WriteLine(numbers[3]);              // 99

//  2. Parse substrings without allocating a new string —————————
ReadOnlySpan<char> date = "2026-06-01".AsSpan();
int year  = int.Parse(date[..4]);     // "2026"
int month = int.Parse(date[5..7]);    // "06"
int day   = int.Parse(date[8..]);     // "01"
Console.WriteLine(new DateTime(year, month, day)); // 01/06/2026

//  3. Stack-allocated Span (stackalloc) ————————————————————————
// No heap allocation at all
Span<byte> stackBuffer = stackalloc byte[256];
stackBuffer.Fill(0);
Console.WriteLine(stackBuffer.Length);   // 256

//  4. String split without allocating substrings ———————————————
static int CountCommas(ReadOnlySpan<char> text)
{
    int count = 0;
    foreach (var c in text)
        if (c == ',') count++;
    return count;
}
Console.WriteLine(CountCommas("a,b,c,d".AsSpan()));   // 3

//  5. Span across native memory (unsafe) ———————————————————————
// unsafe {
//     byte* ptr = stackalloc byte[100];
//     Span<byte> native = new Span<byte>(ptr, 100);
// }

// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••
// Memory<T> — heap-compatible, works with async
// ••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••••

//  6. Memory<T> in async methods ———————————————————————————————
byte[] buffer = new byte[4096];
Memory<byte> mem = buffer.AsMemory(0, 1024);

async Task ReadToMemoryAsync(Stream stream, Memory<byte> destination)
{
    int bytesRead = await stream.ReadAsync(destination);  // no copy — writes directly
    Console.WriteLine($"Read {bytesRead} bytes");
}

//  7. ReadOnlyMemory<T> for strings and read-only data —————————
ReadOnlyMemory<char> roMem = "Hello, World!".AsMemory(7, 5);  // "World"
Console.WriteLine(new string(roMem.Span));  // World

//  8. MemoryPool<T> for reusable large buffers —————————————————
using IMemoryOwner<byte> owner = MemoryPool<byte>.Shared.Rent(minBufferSize: 4096);
Memory<byte> pooledMem = owner.Memory;
// use pooledMem...
// IMemoryOwner.Dispose() returns memory to pool automatically

//  9. Performance comparison ———————————————————————————————————
// Traditional (allocates):       string sub = str.Substring(start, length);
// Span-based (zero alloc):       ReadOnlySpan<char> sub = str.AsSpan(start, length);

static bool StartsWithHttp(string url)
{
    ReadOnlySpan<char> span = url;
    return span.StartsWith("https://", StringComparison.OrdinalIgnoreCase)
        || span.StartsWith("http://", StringComparison.OrdinalIgnoreCase);
}
```

**`Span<T>` vs `Memory<T>` vs `string`:**

| Feature | `Span<T>` | `Memory<T>` | `string` / `T[]` |
|---------|-----------|-------------|-----------------|
| Stack-only | … Yes |  No |  No |
| Works in `async` |  No | … Yes | … Yes |
| Slicing | Zero-copy | Zero-copy | Allocates new object |
| Mutation | … Yes | … Yes | `string` immutable |
| Works with `stackalloc` | … Yes |  No |  No |
| GC pressure | None (stack) | Low (slice only) | High (new object) |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>
