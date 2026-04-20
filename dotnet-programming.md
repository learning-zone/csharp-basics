# .NET Core Coding Practice

> *Click &#9733; if you like the project. Your contributions are heartily ♡ welcome.*

<br/>

## Q. What is the output of the following code? Explain why.

```csharp
int x = 5;
object obj = x;       // boxing
int y = (int)obj;     // unboxing

Console.WriteLine(x == y);        // ?
Console.WriteLine(obj == x);      // ?
Console.WriteLine(obj.Equals(x)); // ?
```

<details><summary><b>Answer</b></summary>

```
True
True
True
```

**Explanation:**

- `x == y` → `True`. Both are `int` with value `5`. Simple value comparison.
- `obj == x` → `True`. The compiler promotes `x` to `object` for comparison; since both hold boxed `5`, reference equality on `==` for `object` compares the unboxed value when the right operand is implicitly boxed — but note: for reference types `==` tests reference, so two separately boxed integers would return `False`. Here the compiler uses the integer `==` operator via implicit conversion.
- `obj.Equals(x)` → `True`. `Int32.Equals` performs value comparison.

**Key pitfall — two separate box operations:**

```csharp
object a = 5;
object b = 5;
Console.WriteLine(a == b);      // False — different object references!
Console.WriteLine(a.Equals(b)); // True  — value comparison
```

**Use cases:** Understanding boxing/unboxing is critical when working with non-generic collections (`ArrayList`, `Hashtable`) or passing value types as `object`. Prefer `List<T>` and `Dictionary<TKey,TValue>` to avoid boxing overhead.

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is the output of the following code?

```csharp
using System;

class Program
{
    static void Main()
    {
        var numbers = new List<int> { 1, 2, 3, 4, 5 };
        var actions = new List<Action>();

        foreach (var n in numbers)
        {
            actions.Add(() => Console.WriteLine(n));
        }

        foreach (var action in actions)
        {
            action();
        }
    }
}
```

<details><summary><b>Answer</b></summary>

```
1
2
3
4
5
```

**Explanation:**

In C# 5+, `foreach` loop variables are scoped **per iteration** (a closure over a fresh copy of `n` each time). Each lambda captures its own separate copy of `n`.

This is **different from the classic `for` loop capture bug**:

```csharp
// Classic capture bug — for loop
var actions2 = new List<Action>();
for (int i = 0; i < 5; i++)
{
    actions2.Add(() => Console.WriteLine(i)); // all capture same variable i
}
foreach (var a in actions2)
    a(); // prints: 5 5 5 5 5
```

**Fix for `for` loop — capture a local copy:**

```csharp
for (int i = 0; i < 5; i++)
{
    int copy = i;                              // local copy per iteration
    actions2.Add(() => Console.WriteLine(copy));
}
// prints: 0 1 2 3 4
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is the output of the following async/await code?

```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        Console.WriteLine("1 - Start");

        var t = Task.Run(() =>
        {
            Console.WriteLine("2 - Task.Run");
            return 42;
        });

        Console.WriteLine("3 - After Task.Run");

        int result = await t;

        Console.WriteLine($"4 - Result: {result}");
        Console.WriteLine("5 - End");
    }
}
```

<details><summary><b>Answer</b></summary>

```
1 - Start
3 - After Task.Run      (usually — see note)
2 - Task.Run            (may interleave with line 3)
4 - Result: 42
5 - End
```

**Explanation:**

- `"1 - Start"` — synchronous, prints first.
- `Task.Run(...)` — schedules work on the thread pool and **returns immediately** with a `Task<int>`.
- `"3 - After Task.Run"` — prints before the task body executes in most cases, because the thread pool thread may not be scheduled yet.
- `"2 - Task.Run"` — executes on a pool thread; the order between `2` and `3` is **non-deterministic**.
- `await t` — suspends the `Main` continuation until the task completes.
- `"4 - Result: 42"` and `"5 - End"` — always print after `await`.

**Key rule:** `Task.Run` is *fire and forget* until `await`-ed. The continuation after `await` is guaranteed to run after the task finishes, but the task body itself may run concurrently with code before the `await`.

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Rewrite the following callback-style code using `async`/`await`.

```csharp
public void GetUserData(int userId, Action<User> onSuccess, Action<Exception> onError)
{
    _repository.FindById(userId, user =>
    {
        if (user == null)
        {
            onError(new NotFoundException($"User {userId} not found"));
            return;
        }
        _emailService.Send(user.Email, "Welcome!", body =>
        {
            onSuccess(user);
        }, onError);
    }, onError);
}
```

<details><summary><b>Answer</b></summary>

```csharp
// Async/await version — clean, linear, and exception-safe
public async Task<User> GetUserDataAsync(int userId, CancellationToken cancellationToken = default)
{
    var user = await _repository.FindByIdAsync(userId, cancellationToken);

    if (user is null)
        throw new NotFoundException($"User {userId} not found");

    await _emailService.SendAsync(user.Email, "Welcome!", cancellationToken);

    return user;
}

// Caller
try
{
    var user = await GetUserDataAsync(userId, cancellationToken);
    Console.WriteLine($"User fetched: {user.Name}");
}
catch (NotFoundException ex)
{
    Console.WriteLine($"Not found: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"Error: {ex.Message}");
}
```

**Benefits of async/await over callbacks:**
- Eliminates "callback hell" / pyramid of doom
- Exceptions propagate naturally via `try/catch`
- `CancellationToken` supports cooperative cancellation
- Stack traces are meaningful and complete
- Code reads sequentially, matches mental model

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is the output of the following LINQ code?

```csharp
var numbers = new[] { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

var result = numbers
    .Where(n => { Console.Write($"W{n} "); return n % 2 == 0; })
    .Select(n => { Console.Write($"S{n} "); return n * n; })
    .Take(3)
    .ToList();

Console.WriteLine();
Console.WriteLine(string.Join(", ", result));
```

<details><summary><b>Answer</b></summary>

```
W1 W2 S2 W3 W4 S4 W5 W6 S6
4, 16, 36
```

**Explanation — LINQ deferred execution and short-circuiting:**

LINQ uses **lazy evaluation**. The pipeline processes elements **one at a time**, not the whole sequence for each operator:

1. `W1` — `Where` tests 1 → odd, skip
2. `W2 S2` — `Where` tests 2 → even, pass to `Select`, compute 4
3. `W3` — odd, skip
4. `W4 S4` — even, compute 16
5. `W5` — odd, skip
6. `W6 S6` — even, compute 36 → `Take(3)` satisfied, **stops**

Numbers 7–10 are **never evaluated** because `Take(3)` short-circuits the pipeline once 3 elements are produced.

**Deferred vs Immediate execution:**

```csharp
// Deferred — query not executed until enumerated
var query = numbers.Where(n => n % 2 == 0);

// Immediate — forces execution now
var list  = numbers.Where(n => n % 2 == 0).ToList();
var array = numbers.Where(n => n % 2 == 0).ToArray();
var count = numbers.Where(n => n % 2 == 0).Count();
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is the output of the following code? Explain `ref` vs `out` vs `in`.

```csharp
static void Modify(ref int a, out int b, in int c)
{
    a = a + 10;
    b = 100;
    // c = 5; // compile error — in is readonly
    Console.WriteLine($"Inside: a={a}, b={b}, c={c}");
}

static void Main()
{
    int x = 1, y, z = 50;
    Modify(ref x, out y, in z);
    Console.WriteLine($"After: x={x}, y={y}, z={z}");
}
```

<details><summary><b>Answer</b></summary>

```
Inside: a=11, b=100, c=50
After: x=11, y=100, z=50
```

**Explanation:**

| Keyword | Must be initialized before call? | Can read inside method? | Can write inside method? | Changes visible to caller? |
|---------|----------------------------------|------------------------|--------------------------|---------------------------|
| `ref`   | Yes                              | Yes                    | Yes                      | Yes                        |
| `out`   | No (undefined before call)       | No (before assignment) | Yes (must assign)        | Yes                        |
| `in`    | Yes                              | Yes                    | No (readonly reference)  | No                         |

- `ref x` — passes reference to `x`; method adds 10, caller sees `x=11`
- `out y` — `y` need not be initialized; method must assign `b=100`; caller sees `y=100`
- `in z` — passes readonly reference; attempting `c = 5` is a compile-time error; `z` stays `50`

**Use cases:**
- `ref` — swap two values, modify value type in method
- `out` — multiple return values (`TryParse`, `TryGetValue`)
- `in` — large struct performance (avoids copy, prevents mutation)

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Fix the following code that causes a deadlock in ASP.NET Core.

```csharp
// Controller action — causes deadlock
public IActionResult GetData()
{
    var result = GetDataAsync().Result; // DEADLOCK!
    return Ok(result);
}

private async Task<string> GetDataAsync()
{
    await Task.Delay(100);
    return "data";
}
```

<details><summary><b>Answer</b></summary>

```csharp
// Fix 1 — make the action async (RECOMMENDED)
public async Task<IActionResult> GetData()
{
    var result = await GetDataAsync();
    return Ok(result);
}

private async Task<string> GetDataAsync()
{
    await Task.Delay(100);
    return "data";
}
```

```csharp
// Fix 2 — ConfigureAwait(false) to avoid capturing the SynchronizationContext
private async Task<string> GetDataAsync()
{
    await Task.Delay(100).ConfigureAwait(false);
    return "data";
}
```

**Why the deadlock occurs:**

1. `GetDataAsync().Result` blocks the **ASP.NET request thread** while waiting for the Task.
2. `await Task.Delay(100)` completes and tries to resume on the **original SynchronizationContext** (the captured ASP.NET request context).
3. That context is already **blocked** by `.Result`. Both sides wait → **deadlock**.

**Rules to avoid deadlocks:**
- Never use `.Result` or `.Wait()` on async methods in synchronous contexts.
- Make the entire call chain `async` ("async all the way").
- Use `ConfigureAwait(false)` in library code that doesn\'t need to resume on the original context.

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is the output of the following code involving `struct` vs `class`?

```csharp
class MyClass  { public int Value; }
struct MyStruct { public int Value; }

static void ModifyClass(MyClass obj)  { obj.Value  = 99; }
static void ModifyStruct(MyStruct obj) { obj.Value = 99; }

static void Main()
{
    var c = new MyClass  { Value = 1 };
    var s = new MyStruct { Value = 1 };

    ModifyClass(c);
    ModifyStruct(s);

    Console.WriteLine($"Class:  {c.Value}");
    Console.WriteLine($"Struct: {s.Value}");
}
```

<details><summary><b>Answer</b></summary>

```
Class:  99
Struct: 1
```

**Explanation:**

- **`class`** is a **reference type**. `c` holds a reference to the heap object. Passing `c` to `ModifyClass` passes the **reference** — both caller and method point to the same object. Mutation is visible to the caller.

- **`struct`** is a **value type**. `s` holds the value directly on the stack. Passing `s` to `ModifyStruct` creates a **copy**. The method modifies the copy; the original `s` is unchanged.

**To modify a struct from a method, use `ref`:**

```csharp
static void ModifyStruct(ref MyStruct obj) { obj.Value = 99; }

ModifyStruct(ref s);
Console.WriteLine(s.Value); // 99
```

**When to use struct vs class:**

| Use `struct` when                        | Use `class` when                        |
|------------------------------------------|-----------------------------------------|
| Small, immutable data (e.g., Point, RGB) | Large or complex objects                |
| No inheritance needed                    | Inheritance / polymorphism needed       |
| Frequently allocated (avoids GC)         | Shared, mutable state needed            |
| Implements `IEquatable<T>` by value      | Reference identity matters              |

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Implement a thread-safe Singleton using `Lazy<T>` and explain the alternatives.

<details><summary><b>Answer</b></summary>

```csharp
// Approach 1: Lazy<T> — simplest, thread-safe, lazy initialization (RECOMMENDED)
public sealed class AppConfiguration
{
    private static readonly Lazy<AppConfiguration> _instance =
        new Lazy<AppConfiguration>(() => new AppConfiguration());

    public static AppConfiguration Instance => _instance.Value;

    private AppConfiguration()
    {
        // Load config, connect to resources, etc.
        ConnectionString = "Server=localhost;Database=App;";
    }

    public string ConnectionString { get; }
}

// Usage
var config = AppConfiguration.Instance;
Console.WriteLine(config.ConnectionString);
```

```csharp
// Approach 2: Static constructor — thread-safe due to CLR guarantee, but eager
public sealed class AppConfiguration
{
    public static readonly AppConfiguration Instance = new AppConfiguration();
    static AppConfiguration() { }            // ensures beforefieldinit is suppressed
    private AppConfiguration() { }
}
```

```csharp
// Approach 3: Double-checked locking — manual, verbose, only needed pre-.NET 4
public sealed class AppConfiguration
{
    private static volatile AppConfiguration? _instance;
    private static readonly object _lock = new object();

    public static AppConfiguration Instance
    {
        get
        {
            if (_instance is null)
            {
                lock (_lock)
                {
                    _instance ??= new AppConfiguration();
                }
            }
            return _instance;
        }
    }
    private AppConfiguration() { }
}
```

```csharp
// Approach 4: .NET Core DI (preferred in ASP.NET Core — NOT a class-level singleton)
builder.Services.AddSingleton<AppConfiguration>();
// DI container manages single instance — testable, avoids global state
```

**Comparison:**

| Approach            | Thread-Safe | Lazy | Testable | Recommended |
|---------------------|-------------|------|----------|-------------|
| `Lazy<T>`           | Yes         | Yes  | Moderate | Yes (library code) |
| Static constructor  | Yes         | No   | No       | Simple scenarios |
| Double-checked lock | Yes         | Yes  | No       | Legacy code |
| DI container        | Yes         | Yes  | Yes      | **ASP.NET Core** |

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is the output of the following pattern matching code?

```csharp
object[] items = { 42, "hello", 3.14, null, true, new List<int> { 1, 2, 3 } };

foreach (var item in items)
{
    var result = item switch
    {
        int n when n > 10      => $"Large int: {n}",
        int n                  => $"Small int: {n}",
        string { Length: > 3 } s => $"Long string: {s}",
        string s               => $"Short string: {s}",
        double d               => $"Double: {d}",
        null                   => "Null value",
        IList<int> list        => $"List with {list.Count} items",
        _                      => $"Other: {item}"
    };
    Console.WriteLine(result);
}
```

<details><summary><b>Answer</b></summary>

```
Large int: 42
Long string: hello
Double: 3.14
Null value
Other: True
List with 3 items
```

**Explanation:**

- `42` → matches `int n when n > 10` (guard condition passes)
- `"hello"` → matches `string { Length: > 3 }` (property pattern, length is 5)
- `3.14` → matches `double d`
- `null` → matches `null` arm
- `true` → is `bool`, not matched by any typed arm, falls through to `_` (discard)
- `new List<int>{1,2,3}` → matches `IList<int>` (interface pattern matching)

**Pattern types in C#:**

```csharp
// Type pattern
if (obj is string s) { }

// Declaration pattern + guard
if (obj is int n && n > 0) { }

// Property pattern
if (person is { Age: >= 18, Name: not null }) { }

// Positional pattern (deconstruction)
if (point is (0, 0)) { }

// List pattern (C# 11)
if (arr is [1, 2, ..]) { }

// Logical patterns
if (n is > 0 and < 100) { }
if (obj is null or "") { }
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Write a generic method to find the second largest element in an array.

<details><summary><b>Answer</b></summary>

```csharp
// Approach 1: Single pass O(n) — most efficient
public static T? SecondLargest<T>(T[] arr) where T : IComparable<T>
{
    if (arr.Length < 2)
        throw new ArgumentException("Array must have at least 2 elements");

    T first  = arr[0].CompareTo(arr[1]) >= 0 ? arr[0] : arr[1];
    T second = arr[0].CompareTo(arr[1]) >= 0 ? arr[1] : arr[0];

    for (int i = 2; i < arr.Length; i++)
    {
        if (arr[i].CompareTo(first) > 0)
        {
            second = first;
            first  = arr[i];
        }
        else if (arr[i].CompareTo(second) > 0 && arr[i].CompareTo(first) != 0)
        {
            second = arr[i];
        }
    }
    return second;
}

// Approach 2: LINQ — readable, O(n log n)
public static T SecondLargestLinq<T>(T[] arr) where T : IComparable<T>
    => arr.Distinct().OrderDescending().Skip(1).First();

// Approach 3: Using SortedSet for uniqueness
public static T SecondLargestSet<T>(T[] arr) where T : IComparable<T>
{
    var set = new SortedSet<T>(arr);
    if (set.Count < 2) throw new InvalidOperationException("Need at least 2 distinct values");
    return set.Reverse().Skip(1).First();
}

// Tests
int[] nums = { 3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5 };
Console.WriteLine(SecondLargest(nums));      // 6
Console.WriteLine(SecondLargestLinq(nums));  // 6

string[] words = { "banana", "apple", "cherry", "date" };
Console.WriteLine(SecondLargest(words));     // cherry
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is the output of the following `IEnumerable` deferred execution code?

```csharp
static IEnumerable<int> Generate()
{
    Console.WriteLine("Start");
    yield return 1;
    Console.WriteLine("After 1");
    yield return 2;
    Console.WriteLine("After 2");
    yield return 3;
    Console.WriteLine("After 3");
}

static void Main()
{
    var seq = Generate();
    Console.WriteLine("Before foreach");

    foreach (var n in seq)
    {
        Console.WriteLine($"Got: {n}");
        if (n == 2) break;
    }

    Console.WriteLine("Done");
}
```

<details><summary><b>Answer</b></summary>

```
Before foreach
Start
Got: 1
After 1
Got: 2
Done
```

**Explanation:**

- `Generate()` returns immediately — the body does **not** execute yet (deferred/lazy).
- `"Before foreach"` prints before any generator code runs.
- Each `foreach` iteration calls `MoveNext()` on the iterator, which runs the generator body up to the next `yield return`.
- When `n == 2`, `break` exits the loop — the generator is **disposed** and `"After 2"` / `"After 3"` never execute.

**Iterator state machine** — the compiler transforms `yield return` into a state machine:

```csharp
// Effectively equivalent to:
var enumerator = Generate().GetEnumerator();
try
{
    while (enumerator.MoveNext())
    {
        var n = enumerator.Current;
        Console.WriteLine($"Got: {n}");
        if (n == 2) break;
    }
}
finally
{
    enumerator.Dispose(); // Calls finally blocks inside generator
}
```

**Use cases:** `yield return` is ideal for generating infinite sequences, reading large files line-by-line, building lazy pipelines, and implementing `IEnumerable<T>` without materializing the entire collection.

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Implement a `Retry` method that retries an async operation with exponential backoff.

<details><summary><b>Answer</b></summary>

```csharp
public static async Task<T> RetryAsync<T>(
    Func<Task<T>> operation,
    int maxRetries        = 3,
    int initialDelayMs    = 200,
    double backoffFactor  = 2.0,
    CancellationToken cancellationToken = default)
{
    int attempt    = 0;
    int delayMs    = initialDelayMs;
    Exception? lastException = null;

    while (attempt <= maxRetries)
    {
        try
        {
            return await operation();
        }
        catch (OperationCanceledException)
        {
            throw; // Never retry on cancellation
        }
        catch (Exception ex)
        {
            lastException = ex;
            if (attempt == maxRetries) break;

            Console.WriteLine($"Attempt {attempt + 1} failed: {ex.Message}. Retrying in {delayMs}ms...");
            await Task.Delay(delayMs, cancellationToken);
            delayMs = (int)(delayMs * backoffFactor); // exponential backoff
            attempt++;
        }
    }

    throw new InvalidOperationException(
        $"Operation failed after {maxRetries} retries.", lastException);
}

// Usage — retry an HTTP call
var client = new HttpClient();

string result = await RetryAsync(
    async () =>
    {
        var response = await client.GetAsync("https://api.example.com/data");
        response.EnsureSuccessStatusCode();
        return await response.Content.ReadAsStringAsync();
    },
    maxRetries: 3,
    initialDelayMs: 500);

Console.WriteLine(result);
```

```csharp
// With jitter to avoid thundering herd
public static async Task<T> RetryWithJitterAsync<T>(
    Func<Task<T>> operation, int maxRetries = 3, CancellationToken ct = default)
{
    var rng = Random.Shared;

    for (int attempt = 0; attempt < maxRetries; attempt++)
    {
        try   { return await operation(); }
        catch (OperationCanceledException) { throw; }
        catch when (attempt < maxRetries - 1)
        {
            int baseDelay  = (int)Math.Pow(2, attempt) * 100;
            int jitter     = rng.Next(0, 100);
            await Task.Delay(baseDelay + jitter, ct);
        }
    }
    return await operation(); // last attempt — let exception propagate
}
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is the output of the following `record` and `with` expression code?

```csharp
public record Person(string Name, int Age);

var p1 = new Person("Alice", 30);
var p2 = p1 with { Age = 31 };
var p3 = new Person("Alice", 30);

Console.WriteLine(p1);
Console.WriteLine(p2);
Console.WriteLine(p1 == p3);
Console.WriteLine(p1 == p2);
Console.WriteLine(ReferenceEquals(p1, p3));
Console.WriteLine(p2.Name);
```

<details><summary><b>Answer</b></summary>

```
Person { Name = Alice, Age = 30 }
Person { Name = Alice, Age = 31 }
True
False
False
Alice
```

**Explanation:**

- Records generate a `ToString()` that shows all property values.
- `p1 with { Age = 31 }` creates a **new** `Person` instance with `Name` copied from `p1` and `Age` set to `31` — non-destructive mutation.
- Records use **value-based equality** by default: `p1 == p3` is `True` because both have `Name="Alice"` and `Age=30`.
- `p1 == p2` is `False` because `Age` differs.
- `ReferenceEquals(p1, p3)` is `False` — they are different heap objects even though they are equal by value.
- `p2.Name` is `"Alice"` — the `with` expression copies all other properties unchanged.

**Record features:**

```csharp
// Positional record (auto-generated constructor, Deconstruct, init-only props)
public record Point(double X, double Y);

// Record struct (value semantics + stack allocation)
public record struct Range(int Start, int End);

// Inheritance
public record Employee(string Name, int Age, string Department) : Person(Name, Age);

// Custom equality
public record Product(string Sku, string Name)
{
    // Override to compare only by SKU
    public virtual bool Equals(Product? other) => other is not null && Sku == other.Sku;
    public override int GetHashCode() => Sku.GetHashCode();
}
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Implement a `Producer-Consumer` pattern using `Channel<T>` in .NET Core.

<details><summary><b>Answer</b></summary>

```csharp
using System.Threading.Channels;

// Bounded channel — backpressure when full
var channel = Channel.CreateBounded<WorkItem>(new BoundedChannelOptions(100)
{
    FullMode      = BoundedChannelFullMode.Wait,
    SingleWriter  = false,
    SingleReader  = false
});

// Producer
async Task ProduceAsync(ChannelWriter<WorkItem> writer, CancellationToken ct)
{
    try
    {
        for (int i = 0; i < 1000; i++)
        {
            var item = new WorkItem(Id: i, Payload: $"task-{i}");
            await writer.WriteAsync(item, ct);
            Console.WriteLine($"Produced: {item.Id}");
        }
    }
    finally
    {
        writer.Complete(); // Signal no more items
    }
}

// Consumer
async Task ConsumeAsync(ChannelReader<WorkItem> reader, int consumerId, CancellationToken ct)
{
    await foreach (var item in reader.ReadAllAsync(ct))
    {
        await Task.Delay(10, ct); // Simulate work
        Console.WriteLine($"Consumer {consumerId} processed: {item.Id}");
    }
}

// Run with multiple consumers
using var cts = new CancellationTokenSource();

var producer  = ProduceAsync(channel.Writer, cts.Token);
var consumers = Enumerable.Range(1, 3)
    .Select(id => ConsumeAsync(channel.Reader, id, cts.Token))
    .ToList();

await Task.WhenAll(new[] { producer }.Concat(consumers));

public record WorkItem(int Id, string Payload);
```

**Why `Channel<T>` over `BlockingCollection<T>`:**

| Feature              | `Channel<T>`         | `BlockingCollection<T>` |
|----------------------|----------------------|-------------------------|
| Async-native         | Yes (`await`)        | No (blocking)           |
| `IAsyncEnumerable`   | Yes                  | No                      |
| Backpressure         | Yes (bounded)        | Yes (bounded)           |
| Thread model         | Async/await          | Thread-per-consumer     |
| .NET version         | .NET Core 2.1+       | .NET Framework 4.0+     |

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is the output of the following code? Explain `string` interning and equality.

```csharp
string a = "hello";
string b = "hello";
string c = new string("hello".ToCharArray());
string d = string.Intern(c);

Console.WriteLine(a == b);               // ?
Console.WriteLine(ReferenceEquals(a, b)); // ?
Console.WriteLine(a == c);               // ?
Console.WriteLine(ReferenceEquals(a, c)); // ?
Console.WriteLine(ReferenceEquals(a, d)); // ?
```

<details><summary><b>Answer</b></summary>

```
True
True
True
False
True
```

**Explanation:**

- `a == b` → `True`. String `==` compares by **value** (content).
- `ReferenceEquals(a, b)` → `True`. String literals are **interned** by the CLR — `"hello"` appears once in the intern pool; both `a` and `b` point to the same object.
- `a == c` → `True`. Value comparison — same characters.
- `ReferenceEquals(a, c)` → `False`. `new string(...)` allocates a **new** string object on the heap, bypassing the intern pool.
- `ReferenceEquals(a, d)` → `True`. `string.Intern(c)` looks up the intern pool; since `"hello"` is already interned, it returns the **same reference** as `a`.

**Key rules:**

```csharp
// Always use == for string value comparison (not ReferenceEquals)
string.Equals(a, b, StringComparison.Ordinal);        // case-sensitive
string.Equals(a, b, StringComparison.OrdinalIgnoreCase); // case-insensitive

// String is immutable — every "modification" creates a new string
string s = "hello";
s += " world"; // allocates a new string

// StringBuilder for many concatenations
var sb = new StringBuilder();
for (int i = 0; i < 10000; i++) sb.Append(i);
string result = sb.ToString();
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Implement `Flatten` — flatten a nested list structure using recursion and LINQ.

<details><summary><b>Answer</b></summary>

```csharp
// Input: [[1, [2, 3]], [4, [5, [6]]]] → [1, 2, 3, 4, 5, 6]

// Approach 1: Recursive with yield
public static IEnumerable<T> Flatten<T>(object item)
{
    if (item is T leaf)
    {
        yield return leaf;
    }
    else if (item is IEnumerable<object> collection)
    {
        foreach (var child in collection)
            foreach (var val in Flatten<T>(child))
                yield return val;
    }
}

// Approach 2: Flatten IEnumerable<IEnumerable<T>> — one level
public static IEnumerable<T> FlattenOne<T>(IEnumerable<IEnumerable<T>> nested)
    => nested.SelectMany(x => x);

// Approach 3: Recursive tree flatten (domain object)
public class TreeNode<T>
{
    public T Value    { get; init; }
    public List<TreeNode<T>> Children { get; init; } = new();
}

public static IEnumerable<T> FlattenTree<T>(TreeNode<T> node)
{
    yield return node.Value;
    foreach (var child in node.Children)
        foreach (var val in FlattenTree(child))
            yield return val;
}

// Elegant LINQ version for tree
public static IEnumerable<T> FlattenTreeLinq<T>(TreeNode<T> node)
    => node.Children
        .SelectMany(FlattenTreeLinq)
        .Prepend(node.Value);

// Usage
var tree = new TreeNode<int>
{
    Value = 1,
    Children =
    [
        new() { Value = 2, Children = [ new() { Value = 4 }, new() { Value = 5 } ] },
        new() { Value = 3, Children = [ new() { Value = 6 } ] }
    ]
};

var flat = FlattenTreeLinq(tree).ToList(); // [1, 2, 4, 5, 3, 6]
Console.WriteLine(string.Join(", ", flat));
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What does the following code print? Explain `IDisposable` and `using`.

```csharp
class Resource : IDisposable
{
    public Resource()  => Console.WriteLine("Acquired");
    public void Use()  => Console.WriteLine("Using");
    public void Dispose() => Console.WriteLine("Released");
}

static void Main()
{
    using (var r = new Resource())
    {
        r.Use();
        throw new Exception("Oops!");
    }
    Console.WriteLine("After using"); // reached?
}
```

<details><summary><b>Answer</b></summary>

```
Acquired
Using
Released
Unhandled exception: System.Exception: Oops!
```

**Explanation:**

- `using` compiles to a `try/finally` block. `Dispose()` is **always** called even when an exception is thrown.
- `"After using"` is **never reached** because the exception propagates after `Dispose()`.
- The exception is unhandled so the runtime prints it and terminates.

**Compiled equivalent:**

```csharp
var r = new Resource();
try
{
    r.Use();
    throw new Exception("Oops!");
}
finally
{
    r?.Dispose(); // guaranteed — even on exception
}
```

**Modern syntax (C# 8+):**

```csharp
// Declaration using — disposes at end of enclosing scope
static void Main()
{
    using var r = new Resource(); // no braces needed
    r.Use();
} // Dispose() called here automatically
```

**`IAsyncDisposable` for async resources:**

```csharp
public class AsyncResource : IAsyncDisposable
{
    public async ValueTask DisposeAsync()
    {
        await FlushAsync();
        Console.WriteLine("Async released");
    }
}

await using var ar = new AsyncResource();
// DisposeAsync() called on scope exit
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Write a LINQ query to group, filter, and project a collection of orders.

<details><summary><b>Answer</b></summary>

```csharp
public record Order(int Id, string Customer, string Category, decimal Amount, DateTime Date);

var orders = new List<Order>
{
    new(1, "Alice", "Electronics", 1200m, new DateTime(2024, 1, 15)),
    new(2, "Bob",   "Clothing",     300m, new DateTime(2024, 1, 20)),
    new(3, "Alice", "Electronics",  800m, new DateTime(2024, 2, 5)),
    new(4, "Charlie","Clothing",    150m, new DateTime(2024, 2, 10)),
    new(5, "Bob",   "Electronics", 2500m, new DateTime(2024, 3, 1)),
    new(6, "Alice", "Food",          50m, new DateTime(2024, 3, 15)),
};

// Query: customers with total Electronics spend > $1000, sorted by total desc
var result = orders
    .Where(o => o.Category == "Electronics")
    .GroupBy(o => o.Customer)
    .Where(g => g.Sum(o => o.Amount) > 1000)
    .Select(g => new
    {
        Customer   = g.Key,
        OrderCount = g.Count(),
        TotalSpend = g.Sum(o => o.Amount),
        AverageOrder = g.Average(o => o.Amount),
        LastOrder  = g.Max(o => o.Date)
    })
    .OrderByDescending(x => x.TotalSpend);

foreach (var r in result)
    Console.WriteLine($"{r.Customer}: {r.OrderCount} orders, ${r.TotalSpend}, avg ${r.AverageOrder:F0}");

// Output:
// Bob:   1 orders, $2500, avg $2500
// Alice: 2 orders, $2000, avg $1000

// Query syntax equivalent
var resultQuery =
    from o in orders
    where o.Category == "Electronics"
    group o by o.Customer into g
    where g.Sum(o => o.Amount) > 1000
    orderby g.Sum(o => o.Amount) descending
    select new
    {
        Customer   = g.Key,
        TotalSpend = g.Sum(o => o.Amount)
    };

// Pivot — monthly totals per category
var pivot = orders
    .GroupBy(o => new { o.Date.Year, o.Date.Month, o.Category })
    .Select(g => new
    {
        Period   = $"{g.Key.Year}-{g.Key.Month:D2}",
        Category = g.Key.Category,
        Total    = g.Sum(o => o.Amount)
    })
    .OrderBy(x => x.Period);
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Implement a generic `Result<T>` type for functional error handling without exceptions.

<details><summary><b>Answer</b></summary>

```csharp
public readonly struct Result<T>
{
    private readonly T? _value;
    private readonly string? _error;

    private Result(T value)          { _value = value; _error = null; IsSuccess = true; }
    private Result(string error)     { _value = default; _error = error; IsSuccess = false; }

    public bool    IsSuccess { get; }
    public bool    IsFailure => !IsSuccess;
    public T       Value     => IsSuccess ? _value! : throw new InvalidOperationException($"Result failed: {_error}");
    public string  Error     => IsFailure ? _error! : throw new InvalidOperationException("Result succeeded");

    public static Result<T> Success(T value)   => new(value);
    public static Result<T> Failure(string error) => new(error);

    // Functor map
    public Result<TOut> Map<TOut>(Func<T, TOut> mapper)
        => IsSuccess ? Result<TOut>.Success(mapper(_value!)) : Result<TOut>.Failure(_error!);

    // Monad bind
    public Result<TOut> Bind<TOut>(Func<T, Result<TOut>> binder)
        => IsSuccess ? binder(_value!) : Result<TOut>.Failure(_error!);

    // Unwrap with default
    public T GetValueOrDefault(T defaultValue = default!)
        => IsSuccess ? _value! : defaultValue;

    public override string ToString()
        => IsSuccess ? $"Success({_value})" : $"Failure({_error})";
}

// Factory helpers
public static class Result
{
    public static Result<T> Try<T>(Func<T> operation)
    {
        try   { return Result<T>.Success(operation()); }
        catch (Exception ex) { return Result<T>.Failure(ex.Message); }
    }
}

// Usage
public static Result<int> Divide(int a, int b)
{
    if (b == 0) return Result<int>.Failure("Division by zero");
    return Result<int>.Success(a / b);
}

var result = Divide(10, 2)
    .Map(x => x * 3)           // 15
    .Bind(x => Divide(x, 3))   // 5
    .Map(x => $"Answer: {x}"); // "Answer: 5"

if (result.IsSuccess)
    Console.WriteLine(result.Value);  // Answer: 5

// Chained operations
var pipeline = Result<string>.Success("42")
    .Bind(s => int.TryParse(s, out int n)
        ? Result<int>.Success(n)
        : Result<int>.Failure("Not a number"))
    .Map(n => n * 2)
    .GetValueOrDefault(0);

Console.WriteLine(pipeline); // 84
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is the output of the following code involving `Span<T>` and `Memory<T>`?

```csharp
using System;

Span<int> span = stackalloc int[] { 1, 2, 3, 4, 5 };

var slice = span[1..4];      // [2, 3, 4]
slice[0] = 99;               // modifies the original?

Console.WriteLine(string.Join(", ", span.ToArray()));
Console.WriteLine(string.Join(", ", slice.ToArray()));
```

<details><summary><b>Answer</b></summary>

```
1, 99, 3, 4, 5
99, 3, 4
```

**Explanation:**

- `Span<T>` is a **view** over memory — not a copy. `slice` points into the same underlying memory as `span`.
- Modifying `slice[0] = 99` changes the element at index 1 of the original `span`.
- `span[1..4]` uses a C# 8 **range operator** creating a slice from index 1 (inclusive) to 4 (exclusive).

**`Span<T>` vs `Memory<T>` vs `ArraySegment<T>`:**

```csharp
// Span<T> — stack-only ref struct, zero-copy slice of any contiguous memory
Span<byte> stackSpan = stackalloc byte[256];
Span<byte> arraySpan = new byte[1024];
Span<byte> slice2    = arraySpan[..512];

// Memory<T> — heap-safe, can be stored in fields, awaited across async boundaries
Memory<byte> memory = new byte[1024];
Memory<byte> memSlice = memory[..512];
await stream.WriteAsync(memSlice); // Span cannot be used here (async)

// High-performance string parsing without allocation
string date = "2024-03-15";
ReadOnlySpan<char> yearSpan = date.AsSpan(0, 4);
int year = int.Parse(yearSpan); // no substring allocation
Console.WriteLine(year); // 2024
```

**Use cases:** High-performance parsing, binary protocols, avoiding allocations in hot paths, interop with unmanaged memory.

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Implement a simple in-memory cache with expiration using `ConcurrentDictionary`.

<details><summary><b>Answer</b></summary>

```csharp
public class ExpiringCache<TKey, TValue> where TKey : notnull
{
    private readonly ConcurrentDictionary<TKey, CacheEntry<TValue>> _store = new();
    private readonly TimeSpan _defaultExpiry;

    public ExpiringCache(TimeSpan defaultExpiry)
    {
        _defaultExpiry = defaultExpiry;
    }

    public void Set(TKey key, TValue value, TimeSpan? expiry = null)
    {
        _store[key] = new CacheEntry<TValue>(value, DateTime.UtcNow + (expiry ?? _defaultExpiry));
    }

    public bool TryGet(TKey key, out TValue? value)
    {
        if (_store.TryGetValue(key, out var entry) && !entry.IsExpired)
        {
            value = entry.Value;
            return true;
        }

        _store.TryRemove(key, out _); // lazy eviction
        value = default;
        return false;
    }

    public TValue GetOrAdd(TKey key, Func<TKey, TValue> factory, TimeSpan? expiry = null)
    {
        if (TryGet(key, out var cached)) return cached!;
        var value = factory(key);
        Set(key, value, expiry);
        return value;
    }

    public void Remove(TKey key) => _store.TryRemove(key, out _);

    // Evict all expired entries
    public void Evict()
    {
        foreach (var (key, entry) in _store)
            if (entry.IsExpired)
                _store.TryRemove(key, out _);
    }

    public int Count => _store.Count;
}

public record CacheEntry<T>(T Value, DateTime ExpiresAt)
{
    public bool IsExpired => DateTime.UtcNow > ExpiresAt;
}

// Usage
var cache = new ExpiringCache<string, string>(defaultExpiry: TimeSpan.FromMinutes(5));

cache.Set("user:1", "Alice");
cache.Set("config", "dark-mode", expiry: TimeSpan.FromHours(1));

if (cache.TryGet("user:1", out var name))
    Console.WriteLine($"Found: {name}"); // Found: Alice

// Get or fetch from DB if not cached
var userData = cache.GetOrAdd("user:2", key =>
{
    Console.WriteLine($"Cache miss — fetching {key} from DB");
    return "Bob"; // simulate DB call
});
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Implement `FizzBuzz` using various C# approaches.

<details><summary><b>Answer</b></summary>

```csharp
// Approach 1: Classic if/else
public static IEnumerable<string> FizzBuzzClassic(int n)
{
    for (int i = 1; i <= n; i++)
    {
        if      (i % 15 == 0) yield return "FizzBuzz";
        else if (i % 3  == 0) yield return "Fizz";
        else if (i % 5  == 0) yield return "Buzz";
        else                  yield return i.ToString();
    }
}

// Approach 2: String concatenation (avoids duplicate divisibility checks)
public static IEnumerable<string> FizzBuzzConcat(int n)
{
    for (int i = 1; i <= n; i++)
    {
        var result = "";
        if (i % 3 == 0) result += "Fizz";
        if (i % 5 == 0) result += "Buzz";
        yield return result.Length > 0 ? result : i.ToString();
    }
}

// Approach 3: LINQ / functional
public static IEnumerable<string> FizzBuzzLinq(int n)
    => Enumerable.Range(1, n).Select(i =>
        (i % 15 == 0) ? "FizzBuzz" :
        (i % 3  == 0) ? "Fizz"     :
        (i % 5  == 0) ? "Buzz"     : i.ToString());

// Approach 4: Switch expression (C# 8+)
public static string FizzBuzzSwitch(int i) => (i % 3, i % 5) switch
{
    (0, 0) => "FizzBuzz",
    (0, _) => "Fizz",
    (_, 0) => "Buzz",
    _      => i.ToString()
};

// Approach 5: Extensible / data-driven
public static IEnumerable<string> FizzBuzzExtensible(int n,
    IEnumerable<(int Divisor, string Word)>? rules = null)
{
    rules ??= new[] { (3, "Fizz"), (5, "Buzz") };

    for (int i = 1; i <= n; i++)
    {
        var words = string.Concat(rules
            .Where(r => i % r.Divisor == 0)
            .Select(r => r.Word));
        yield return words.Length > 0 ? words : i.ToString();
    }
}

// Usage
foreach (var s in FizzBuzzLinq(20))
    Console.Write($"{s} ");
// 1 2 Fizz 4 Buzz Fizz 7 8 Fizz Buzz 11 Fizz 13 14 FizzBuzz 16 17 Fizz 19 Buzz
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is the output of the following code? Explain covariance and contravariance.

```csharp
IEnumerable<string> strings = new List<string> { "a", "b", "c" };
IEnumerable<object> objects = strings; // valid?

Action<object>  actObj = x => Console.WriteLine(x);
Action<string>  actStr = actObj; // valid?

Func<string>    funcStr = () => "hello";
Func<object>    funcObj = funcStr; // valid?
```

<details><summary><b>Answer</b></summary>

All three assignments compile and run without error.

**Output:** No runtime output — just demonstrates type-safe variance.

**Explanation:**

**Covariance (`out T`) — "produce" direction, safe to widen:**

```csharp
// IEnumerable<out T> is covariant
IEnumerable<string> strings = new List<string> { "a", "b", "c" };
IEnumerable<object> objects = strings; // OK — string IS-A object
// Safe: you only READ from IEnumerable, so returning a string as object is safe.
```

**Contravariance (`in T`) — "consume" direction, safe to narrow:**

```csharp
// Action<in T> is contravariant
Action<object> actObj = x => Console.WriteLine(x);
Action<string> actStr = actObj; // OK — Action<object> can handle strings
// Safe: actStr will be called with strings, and actObj can accept any object.
```

**Covariance on return type:**

```csharp
// Func<out TResult> is covariant on return type
Func<string> funcStr = () => "hello";
Func<object> funcObj = funcStr; // OK — returns string which IS-A object
```

**Invariance — `List<T>` is NOT covariant:**

```csharp
List<string> listStr = new List<string>();
// List<object> listObj = listStr; // COMPILE ERROR — List<T> is invariant
// Dangerous: listObj.Add(42) would corrupt listStr!
```

**Custom variance declarations:**

```csharp
interface IProducer<out T>  { T Produce(); }       // covariant (return only)
interface IConsumer<in T>   { void Consume(T item);} // contravariant (param only)
interface ITransformer<T>   { T Transform(T item); } // invariant (both)
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Implement a rate limiter that allows N requests per time window.

<details><summary><b>Answer</b></summary>

```csharp
// Sliding window rate limiter using a Queue
public class SlidingWindowRateLimiter
{
    private readonly int _maxRequests;
    private readonly TimeSpan _window;
    private readonly Queue<DateTime> _timestamps = new();
    private readonly object _lock = new();

    public SlidingWindowRateLimiter(int maxRequests, TimeSpan window)
    {
        _maxRequests = maxRequests;
        _window      = window;
    }

    public bool TryAcquire()
    {
        lock (_lock)
        {
            var now    = DateTime.UtcNow;
            var cutoff = now - _window;

            // Evict timestamps outside window
            while (_timestamps.Count > 0 && _timestamps.Peek() < cutoff)
                _timestamps.Dequeue();

            if (_timestamps.Count >= _maxRequests)
                return false;

            _timestamps.Enqueue(now);
            return true;
        }
    }

    public int AvailableRequests
    {
        get
        {
            lock (_lock)
            {
                var cutoff = DateTime.UtcNow - _window;
                return _maxRequests - _timestamps.Count(t => t >= cutoff);
            }
        }
    }
}

// Token bucket rate limiter
public class TokenBucketRateLimiter
{
    private readonly int _capacity;
    private readonly double _refillPerSecond;
    private double _tokens;
    private DateTime _lastRefill = DateTime.UtcNow;
    private readonly object _lock = new();

    public TokenBucketRateLimiter(int capacity, double refillPerSecond)
    {
        _capacity        = capacity;
        _refillPerSecond = refillPerSecond;
        _tokens          = capacity;
    }

    public bool TryAcquire(int tokens = 1)
    {
        lock (_lock)
        {
            Refill();
            if (_tokens < tokens) return false;
            _tokens -= tokens;
            return true;
        }
    }

    private void Refill()
    {
        var now     = DateTime.UtcNow;
        var elapsed = (now - _lastRefill).TotalSeconds;
        _tokens     = Math.Min(_capacity, _tokens + elapsed * _refillPerSecond);
        _lastRefill = now;
    }
}

// Usage with ASP.NET Core middleware
public class RateLimitingMiddleware
{
    private readonly RequestDelegate _next;
    private readonly SlidingWindowRateLimiter _limiter;

    public RateLimitingMiddleware(RequestDelegate next)
    {
        _next    = next;
        _limiter = new SlidingWindowRateLimiter(maxRequests: 100, window: TimeSpan.FromMinutes(1));
    }

    public async Task InvokeAsync(HttpContext context)
    {
        if (!_limiter.TryAcquire())
        {
            context.Response.StatusCode = StatusCodes.Status429TooManyRequests;
            context.Response.Headers["Retry-After"] = "60";
            await context.Response.WriteAsync("Rate limit exceeded");
            return;
        }
        await _next(context);
    }
}

// .NET 7+ built-in: System.Threading.RateLimiting
// dotnet add package System.Threading.RateLimiting
using System.Threading.RateLimiting;

var limiter = new SlidingWindowRateLimiter(new SlidingWindowRateLimiterOptions
{
    PermitLimit          = 100,
    Window               = TimeSpan.FromMinutes(1),
    SegmentsPerWindow    = 6,
    QueueProcessingOrder = QueueProcessingOrder.OldestFirst,
    QueueLimit           = 0
});
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Implement the Observer Pattern using C# events and `IObservable<T>`.

<details><summary><b>Answer</b></summary>

```csharp
// Approach 1: C# Events (delegate-based)
public class StockMarket
{
    public event EventHandler<StockChangedEventArgs>? PriceChanged;

    private decimal _price;
    public decimal Price
    {
        get => _price;
        set
        {
            if (_price == value) return;
            var old = _price;
            _price  = value;
            PriceChanged?.Invoke(this, new StockChangedEventArgs("MSFT", old, value));
        }
    }
}

public class StockChangedEventArgs : EventArgs
{
    public string Symbol    { get; }
    public decimal OldPrice { get; }
    public decimal NewPrice { get; }
    public decimal Change   => NewPrice - OldPrice;

    public StockChangedEventArgs(string symbol, decimal old, decimal @new)
    {
        Symbol = symbol; OldPrice = old; NewPrice = @new;
    }
}

// Observers
var market  = new StockMarket { Price = 100m };
market.PriceChanged += (_, e) => Console.WriteLine($"Alert: {e.Symbol} moved {e.Change:+0.00;-0.00}");
market.PriceChanged += (_, e) => Console.WriteLine($"Log: {e.Symbol} {e.OldPrice} → {e.NewPrice}");

market.Price = 105m; // triggers both handlers
market.Price = 98m;

// Approach 2: IObservable<T> / IObserver<T>
public class TemperatureSensor : IObservable<double>
{
    private readonly List<IObserver<double>> _observers = new();

    public IDisposable Subscribe(IObserver<double> observer)
    {
        _observers.Add(observer);
        return new Subscription(_observers, observer);
    }

    public void ReadTemperature(double celsius)
    {
        foreach (var o in _observers.ToList())
            o.OnNext(celsius);
    }

    public void Fail(Exception ex)
    {
        foreach (var o in _observers) o.OnError(ex);
    }

    public void Complete()
    {
        foreach (var o in _observers) o.OnCompleted();
        _observers.Clear();
    }

    private class Subscription : IDisposable
    {
        private readonly List<IObserver<double>> _list;
        private readonly IObserver<double> _observer;
        public Subscription(List<IObserver<double>> list, IObserver<double> obs)
        { _list = list; _observer = obs; }
        public void Dispose() => _list.Remove(_observer);
    }
}

public class TemperatureDisplay : IObserver<double>
{
    public void OnNext(double value)       => Console.WriteLine($"Temp: {value}°C");
    public void OnError(Exception error)   => Console.WriteLine($"Error: {error.Message}");
    public void OnCompleted()              => Console.WriteLine("Sensor disconnected");
}

var sensor  = new TemperatureSensor();
var display = new TemperatureDisplay();
using var sub = sensor.Subscribe(display);

sensor.ReadTemperature(22.5);
sensor.ReadTemperature(23.1);
sensor.Complete();
// Output:
// Temp: 22.5°C
// Temp: 23.1°C
// Sensor disconnected
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. What is the output of the following exception handling code?

```csharp
static async Task<int> RiskyOperationAsync()
{
    try
    {
        await Task.Delay(10);
        throw new InvalidOperationException("Inner error");
    }
    catch (InvalidOperationException ex)
    {
        throw new ApplicationException("Wrapped", ex);
    }
    finally
    {
        Console.WriteLine("Finally in RiskyOperationAsync");
    }
}

static async Task Main()
{
    try
    {
        var result = await RiskyOperationAsync();
    }
    catch (ApplicationException ex)
    {
        Console.WriteLine($"Caught: {ex.Message}");
        Console.WriteLine($"Inner:  {ex.InnerException?.Message}");
    }
    finally
    {
        Console.WriteLine("Finally in Main");
    }
}
```

<details><summary><b>Answer</b></summary>

```
Finally in RiskyOperationAsync
Caught: Wrapped
Inner:  Inner error
Finally in Main
```

**Explanation:**

1. `RiskyOperationAsync` throws `InvalidOperationException` after the delay.
2. The inner `catch` catches it and throws a new `ApplicationException`, wrapping the original as `InnerException`.
3. The inner `finally` runs **before** the exception propagates — always.
4. `Main`\'s `catch` catches `ApplicationException`, prints message and `InnerException.Message`.
5. `Main`\'s `finally` runs last — always, even when an exception was caught.

**Exception best practices in .NET Core:**

```csharp
// Preserve stack trace when rethrowing
catch (Exception ex)
{
    Log(ex);
    throw;               // preserves original stack trace
    // throw ex;         // WRONG — resets stack trace to this line
}

// Exception filters — inspect without catching
catch (HttpRequestException ex) when (ex.StatusCode == HttpStatusCode.NotFound)
{
    return NotFound();
}

// AggregateException from Task.WhenAll
try
{
    await Task.WhenAll(task1, task2, task3);
}
catch (Exception ex) // await unwraps first exception from AggregateException
{
    Console.WriteLine(ex.Message);
}
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Implement a strongly-typed pipeline builder using generics and delegates.

<details><summary><b>Answer</b></summary>

```csharp
// Pipeline builder — each step transforms the value
public class Pipeline<T>
{
    private readonly List<Func<T, T>> _steps = new();

    public Pipeline<T> AddStep(Func<T, T> step)
    {
        _steps.Add(step);
        return this;
    }

    public T Execute(T input)
        => _steps.Aggregate(input, (current, step) => step(current));
}

// Type-changing pipeline
public class Pipeline<TIn, TOut>
{
    private Func<TIn, TOut>? _pipeline;

    private Pipeline(Func<TIn, TOut> pipeline) { _pipeline = pipeline; }

    public static Pipeline<TIn, TOut> Start(Func<TIn, TOut> first)
        => new(first);

    public Pipeline<TIn, TNext> Then<TNext>(Func<TOut, TNext> next)
        => new Pipeline<TIn, TNext>(input => next(_pipeline!(input)));

    public TOut Execute(TIn input) => _pipeline!(input);
}

// Usage — string processing pipeline
var pipeline = new Pipeline<string>()
    .AddStep(s => s.Trim())
    .AddStep(s => s.ToUpper())
    .AddStep(s => s.Replace(" ", "_"))
    .AddStep(s => $"[{s}]");

Console.WriteLine(pipeline.Execute("  hello world  ")); // [HELLO_WORLD]

// Type-transforming pipeline
var result = Pipeline<string, int>
    .Start(s => s.Length)
    .Then(len => len * 2)
    .Then(n => $"Length doubled: {n}")
    .Execute("hello");                   // "Length doubled: 10"

Console.WriteLine(result);

// Async pipeline
public class AsyncPipeline<T>
{
    private readonly List<Func<T, Task<T>>> _steps = new();

    public AsyncPipeline<T> AddStep(Func<T, Task<T>> step)
    {
        _steps.Add(step);
        return this;
    }

    public async Task<T> ExecuteAsync(T input)
    {
        var current = input;
        foreach (var step in _steps)
            current = await step(current);
        return current;
    }
}

var asyncPipeline = new AsyncPipeline<string>()
    .AddStep(async s => { await Task.Delay(1); return s.Trim(); })
    .AddStep(async s => { await Task.Delay(1); return s.ToUpper(); });

var output = await asyncPipeline.ExecuteAsync("  hello  ");
Console.WriteLine(output); // HELLO
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Write a program to find all prime numbers up to N using the Sieve of Eratosthenes.

<details><summary><b>Answer</b></summary>

```csharp
// Sieve of Eratosthenes — O(n log log n) time, O(n) space
public static IEnumerable<int> SieveOfEratosthenes(int limit)
{
    if (limit < 2) yield break;

    var isComposite = new bool[limit + 1]; // false = prime candidate

    for (int i = 2; (long)i * i <= limit; i++)
    {
        if (!isComposite[i])
        {
            for (int j = i * i; j <= limit; j += i)
                isComposite[j] = true;
        }
    }

    for (int i = 2; i <= limit; i++)
        if (!isComposite[i])
            yield return i;
}

// Segmented sieve for large ranges — memory efficient
public static IEnumerable<int> SegmentedSieve(int limit)
{
    int segmentSize = (int)Math.Sqrt(limit) + 1;
    var primes      = SieveOfEratosthenes(segmentSize).ToList();

    foreach (var p in primes) yield return p;

    for (int low = segmentSize; low <= limit; low += segmentSize)
    {
        int high       = Math.Min(low + segmentSize - 1, limit);
        var sieve      = new bool[segmentSize];

        foreach (int prime in primes)
        {
            int start = (int)Math.Ceiling((double)low / prime) * prime;
            if (start == prime) start += prime;
            for (int j = start; j <= high; j += prime)
                sieve[j - low] = true;
        }

        for (int i = low; i <= high; i++)
            if (!sieve[i - low])
                yield return i;
    }
}

// Check primality — Miller-Rabin for large numbers
public static bool IsPrime(long n)
{
    if (n < 2)  return false;
    if (n == 2) return true;
    if (n % 2 == 0) return false;

    for (long i = 3; i * i <= n; i += 2)
        if (n % i == 0) return false;

    return true;
}

// Usage
var primes = SieveOfEratosthenes(50).ToList();
Console.WriteLine(string.Join(", ", primes));
// 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47

Console.WriteLine($"Primes up to 1,000,000: {SieveOfEratosthenes(1_000_000).Count()}"); // 78498
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Implement a simple dependency injection container from scratch.

<details><summary><b>Answer</b></summary>

```csharp
public enum Lifetime { Transient, Singleton }

public class SimpleContainer
{
    private readonly Dictionary<Type, ServiceDescriptor> _registrations = new();
    private readonly Dictionary<Type, object> _singletons = new();

    public SimpleContainer Register<TService, TImplementation>(Lifetime lifetime = Lifetime.Transient)
        where TImplementation : TService
    {
        _registrations[typeof(TService)] = new ServiceDescriptor(
            typeof(TImplementation), lifetime);
        return this;
    }

    public SimpleContainer RegisterInstance<TService>(TService instance)
    {
        _singletons[typeof(TService)] = instance!;
        _registrations[typeof(TService)] = new ServiceDescriptor(typeof(TService), Lifetime.Singleton);
        return this;
    }

    public TService Resolve<TService>() => (TService)Resolve(typeof(TService));

    private object Resolve(Type serviceType)
    {
        if (_singletons.TryGetValue(serviceType, out var existing))
            return existing;

        if (!_registrations.TryGetValue(serviceType, out var descriptor))
            throw new InvalidOperationException($"No registration for {serviceType.Name}");

        var instance = CreateInstance(descriptor.ImplementationType);

        if (descriptor.Lifetime == Lifetime.Singleton)
            _singletons[serviceType] = instance;

        return instance;
    }

    private object CreateInstance(Type type)
    {
        var constructor = type.GetConstructors()
            .OrderByDescending(c => c.GetParameters().Length)
            .First();

        var parameters = constructor.GetParameters()
            .Select(p => Resolve(p.ParameterType))
            .ToArray();

        return constructor.Invoke(parameters);
    }

    private record ServiceDescriptor(Type ImplementationType, Lifetime Lifetime);
}

// Usage
public interface ILogger  { void Log(string msg); }
public interface IMailer  { void Send(string to, string body); }
public interface IUserService { void Register(string email); }

public class ConsoleLogger : ILogger
{
    public void Log(string msg) => Console.WriteLine($"[LOG] {msg}");
}

public class Mailer : IMailer
{
    private readonly ILogger _logger;
    public Mailer(ILogger logger) => _logger = logger;
    public void Send(string to, string body) => _logger.Log($"Mail to {to}: {body}");
}

public class UserService : IUserService
{
    private readonly ILogger _logger;
    private readonly IMailer _mailer;

    public UserService(ILogger logger, IMailer mailer)
    {
        _logger = logger;
        _mailer = mailer;
    }

    public void Register(string email)
    {
        _logger.Log($"Registering {email}");
        _mailer.Send(email, "Welcome!");
    }
}

var container = new SimpleContainer()
    .Register<ILogger, ConsoleLogger>(Lifetime.Singleton)
    .Register<IMailer, Mailer>(Lifetime.Transient)
    .Register<IUserService, UserService>(Lifetime.Transient);

var userService = container.Resolve<IUserService>();
userService.Register("alice@example.com");
// [LOG] Registering alice@example.com
// [LOG] Mail to alice@example.com: Welcome!
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Write a program to serialize and deserialize a complex object to/from JSON using `System.Text.Json`.

<details><summary><b>Answer</b></summary>

```csharp
using System.Text.Json;
using System.Text.Json.Serialization;

public record Address(string Street, string City, string Country);

public class User
{
    public int Id { get; set; }
    public string Name { get; set; } = string.Empty;

    [JsonPropertyName("email_address")]
    public string Email { get; set; } = string.Empty;

    [JsonIgnore]
    public string PasswordHash { get; set; } = string.Empty;

    public Address? Address { get; set; }
    public List<string> Roles { get; set; } = new();

    [JsonConverter(typeof(JsonStringEnumConverter))]
    public UserStatus Status { get; set; }

    public DateTime CreatedAt { get; set; }
}

public enum UserStatus { Active, Inactive, Suspended }

// Serialize
var user = new User
{
    Id           = 1,
    Name         = "Alice",
    Email        = "alice@example.com",
    PasswordHash = "secret-hash",
    Address      = new Address("123 Main St", "Springfield", "USA"),
    Roles        = new List<string> { "admin", "user" },
    Status       = UserStatus.Active,
    CreatedAt    = DateTime.UtcNow
};

var options = new JsonSerializerOptions
{
    WriteIndented          = true,
    PropertyNamingPolicy   = JsonNamingPolicy.CamelCase,
    DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull,
    Converters             = { new JsonStringEnumConverter() }
};

string json = JsonSerializer.Serialize(user, options);
Console.WriteLine(json);
// {
//   "id": 1,
//   "name": "Alice",
//   "email_address": "alice@example.com",  ← JsonPropertyName applied
//   "address": { "street": "123 Main St", "city": "Springfield", "country": "USA" },
//   "roles": ["admin", "user"],
//   "status": "Active",                    ← JsonStringEnumConverter
//   "createdAt": "2024-01-15T10:30:00Z"
//   // passwordHash omitted — JsonIgnore
// }

// Deserialize
var restored = JsonSerializer.Deserialize<User>(json, options);
Console.WriteLine($"{restored!.Name} is {restored.Status}");

// Source generation — AOT-friendly, faster serialization
[JsonSerializable(typeof(User))]
[JsonSerializable(typeof(List<User>))]
public partial class AppJsonContext : JsonSerializerContext { }

// Registration in ASP.NET Core
builder.Services.ConfigureHttpJsonOptions(options =>
{
    options.SerializerOptions.TypeInfoResolverChain.Insert(0, AppJsonContext.Default);
    options.SerializerOptions.WriteIndented = false;
});
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Implement an anagram checker and grouper.

<details><summary><b>Answer</b></summary>

```csharp
// Check if two strings are anagrams
public static bool AreAnagrams(string a, string b)
{
    if (a.Length != b.Length) return false;

    // Approach 1: Sort characters O(n log n)
    static string Normalize(string s) => new string(s.ToLower().OrderBy(c => c).ToArray());
    // return Normalize(a) == Normalize(b);

    // Approach 2: Frequency count O(n), O(1) space (bounded by character set)
    var freq = new int[128]; // ASCII
    foreach (char c in a.ToLower()) freq[c]++;
    foreach (char c in b.ToLower()) freq[c]--;
    return freq.All(f => f == 0);
}

// Group anagrams — O(n * k log k) where k = max word length
public static IReadOnlyDictionary<string, List<string>> GroupAnagrams(IEnumerable<string> words)
    => words
        .GroupBy(w => new string(w.ToLower().OrderBy(c => c).ToArray()))
        .ToDictionary(g => g.Key, g => g.ToList());

// Find anagrams of a word in a list
public static List<string> FindAnagrams(string word, IEnumerable<string> dictionary)
{
    var key = new string(word.ToLower().OrderBy(c => c).ToArray());
    return dictionary
        .Where(w => !w.Equals(word, StringComparison.OrdinalIgnoreCase))
        .Where(w => new string(w.ToLower().OrderBy(c => c).ToArray()) == key)
        .ToList();
}

// Usage
Console.WriteLine(AreAnagrams("listen", "silent")); // True
Console.WriteLine(AreAnagrams("hello", "world"));   // False

var words = new[] { "eat", "tea", "tan", "ate", "nat", "bat" };
var groups = GroupAnagrams(words);

foreach (var (key, group) in groups)
    Console.WriteLine($"[{key}] → {string.Join(", ", group)}");
// [aet] → eat, tea, ate
// [ant] → tan, nat
// [abt] → bat

var anagrams = FindAnagrams("race", new[] { "care", "acre", "face", "pace", "race" });
Console.WriteLine(string.Join(", ", anagrams)); // care, acre
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Implement a simple Expression Evaluator for arithmetic expressions.

<details><summary><b>Answer</b></summary>

```csharp
// Recursive descent parser for: +, -, *, /, parentheses
public class ExpressionEvaluator
{
    private string _input = "";
    private int _pos;

    public double Evaluate(string expression)
    {
        _input = expression.Replace(" ", "");
        _pos   = 0;
        return ParseExpression();
    }

    // expression := term (('+' | '-') term)*
    private double ParseExpression()
    {
        double result = ParseTerm();

        while (_pos < _input.Length && (_input[_pos] == '+' || _input[_pos] == '-'))
        {
            char op = _input[_pos++];
            double right = ParseTerm();
            result = op == '+' ? result + right : result - right;
        }
        return result;
    }

    // term := factor (('*' | '/') factor)*
    private double ParseTerm()
    {
        double result = ParseFactor();

        while (_pos < _input.Length && (_input[_pos] == '*' || _input[_pos] == '/'))
        {
            char op = _input[_pos++];
            double right = ParseFactor();
            if (op == '/' && right == 0) throw new DivideByZeroException();
            result = op == '*' ? result * right : result / right;
        }
        return result;
    }

    // factor := number | '(' expression ')' | '-' factor
    private double ParseFactor()
    {
        if (_pos < _input.Length && _input[_pos] == '-')
        {
            _pos++;
            return -ParseFactor();
        }

        if (_pos < _input.Length && _input[_pos] == '(')
        {
            _pos++; // consume '('
            double result = ParseExpression();
            if (_pos >= _input.Length || _input[_pos] != ')')
                throw new FormatException("Missing closing parenthesis");
            _pos++; // consume ')'
            return result;
        }

        return ParseNumber();
    }

    private double ParseNumber()
    {
        int start = _pos;
        while (_pos < _input.Length && (char.IsDigit(_input[_pos]) || _input[_pos] == '.'))
            _pos++;

        if (_pos == start) throw new FormatException($"Expected number at position {_pos}");
        return double.Parse(_input[start.._pos]);
    }
}

// Usage
var eval = new ExpressionEvaluator();

Console.WriteLine(eval.Evaluate("2 + 3 * 4"));        // 14
Console.WriteLine(eval.Evaluate("(2 + 3) * 4"));      // 20
Console.WriteLine(eval.Evaluate("10 / 2 - 3"));       // 2
Console.WriteLine(eval.Evaluate("3.14 * 2"));         // 6.28
Console.WriteLine(eval.Evaluate("-5 + 10"));          // 5
Console.WriteLine(eval.Evaluate("((2+3)*4)/2"));      // 10
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. Implement a concurrent task processor with a bounded degree of parallelism.

<details><summary><b>Answer</b></summary>

```csharp
// Approach 1: SemaphoreSlim — limit concurrent HTTP calls
public static async Task<IEnumerable<TResult>> ProcessConcurrentlyAsync<TSource, TResult>(
    IEnumerable<TSource> items,
    Func<TSource, Task<TResult>> processor,
    int maxConcurrency = 4,
    CancellationToken ct = default)
{
    using var semaphore = new SemaphoreSlim(maxConcurrency);
    var tasks = items.Select(async item =>
    {
        await semaphore.WaitAsync(ct);
        try   { return await processor(item); }
        finally { semaphore.Release(); }
    });
    return await Task.WhenAll(tasks);
}

// Approach 2: Parallel.ForEachAsync (.NET 6+) — cleaner API
public static async Task ProcessWithParallelAsync<T>(
    IEnumerable<T> items,
    Func<T, CancellationToken, ValueTask> processor,
    int maxDegreeOfParallelism = 4,
    CancellationToken ct = default)
{
    await Parallel.ForEachAsync(
        items,
        new ParallelOptions
        {
            MaxDegreeOfParallelism = maxDegreeOfParallelism,
            CancellationToken      = ct
        },
        processor);
}

// Approach 3: ActionBlock<T> from TPL Dataflow
using System.Threading.Tasks.Dataflow;

public static async Task ProcessWithDataflowAsync<T>(
    IEnumerable<T> items,
    Func<T, Task> processor,
    int maxDegreeOfParallelism = 4)
{
    var block = new ActionBlock<T>(
        processor,
        new ExecutionDataflowBlockOptions
        {
            MaxDegreeOfParallelism = maxDegreeOfParallelism
        });

    foreach (var item in items)
        await block.SendAsync(item);

    block.Complete();
    await block.Completion;
}

// Usage — download URLs with max 3 concurrent requests
var urls = Enumerable.Range(1, 20).Select(i => $"https://api.example.com/item/{i}").ToList();
var client = new HttpClient();

// SemaphoreSlim approach
var results = await ProcessConcurrentlyAsync(
    urls,
    async url =>
    {
        Console.WriteLine($"Fetching {url} on thread {Thread.CurrentThread.ManagedThreadId}");
        var response = await client.GetStringAsync(url);
        return response.Length;
    },
    maxConcurrency: 3);

// Parallel.ForEachAsync approach (.NET 6+)
await Parallel.ForEachAsync(urls, new ParallelOptions { MaxDegreeOfParallelism = 3 },
    async (url, ct) =>
    {
        var response = await client.GetAsync(url, ct);
        Console.WriteLine($"{url} → {response.StatusCode}");
    });
```

</details>

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>
