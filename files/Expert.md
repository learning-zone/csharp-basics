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

* [Advanced C# Features](#-15-advanced-c--features): Reflection, source generators, unsafe code, and dynamic programming.
* [Performance and Optimization](#-16-performance-and-optimization): Span<T>, Memory<T>, object pooling, benchmarking, and profiling.
* [Microservices and Distributed Systems](#-17-microservices-and-distributed-systems): Service decomposition, gRPC, message brokers, and distributed patterns.
* [Architecture and Design Patterns](#-18-architecture-and-design-patterns): Clean Architecture, CQRS, DDD, and enterprise integration patterns.
* [Deployment](#-19-deployment): CI/CD pipelines, containerization, publishing profiles, and environment config.
* [.NET Core](#-20-net-core): Middleware, DI container, configuration, hosted services, and ASP.NET Core internals.
* [Miscellaneous](#-21-miscellaneous): Reflection, attributes, source generators, and advanced `C#` patterns.

<br>

## # 15. ADVANCED C# FEATURES

<br>

## Q. How does unsafe code and pointers work in C#?

**Unsafe code** enables direct memory manipulation using pointers — useful for performance-critical interop, image processing, and working with unmanaged APIs.

```cs
//  1. Enable unsafe code in .csproj ———————————————————————————————
// <AllowUnsafeBlocks>true</AllowUnsafeBlocks>

//  2. Pointer basics ———————————————————————————————————————————————
unsafe
{
    int value = 42;
    int* ptr  = &value;           // take address
    Console.WriteLine(*ptr);      // dereference: 42
    *ptr = 100;
    Console.WriteLine(value);     // 100 — modified via pointer

    // Pointer arithmetic
    int[] arr = { 10, 20, 30, 40, 50 };
    fixed (int* p = arr)          // pin array so GC doesn\'t move it
    {
        for (int i = 0; i < arr.Length; i++)
            Console.Write(*(p + i) + " "); // 10 20 30 40 50
    }
}

//  3. stackalloc — allocate on stack (no GC) ———————————————————————
unsafe
{
    // Stack-allocated buffer — no heap allocation, no GC pressure
    int* numbers = stackalloc int[8];
    for (int i = 0; i < 8; i++) numbers[i] = i * i;
    for (int i = 0; i < 8; i++) Console.Write(numbers[i] + " "); // 0 1 4 9 16 25 36 49
}

// Preferred: stackalloc with Span<T> (no unsafe keyword needed)
Span<int> safeStack = stackalloc int[8];
for (int i = 0; i < 8; i++) safeStack[i] = i * i;

//  4. Structs with fixed-size arrays ———————————————————————————————
public unsafe struct NetworkHeader
{
    public fixed byte IpAddress[4];     // inline array — no pointer chasing
    public ushort Port;
    public uint Sequence;
}

unsafe
{
    NetworkHeader header = new();
    header.IpAddress[0] = 192;
    header.IpAddress[1] = 168;
    header.IpAddress[2] = 1;
    header.IpAddress[3] = 1;
    header.Port = 8080;
    Console.WriteLine($"IP: {header.IpAddress[0]}.{header.IpAddress[1]}.{header.IpAddress[2]}.{header.IpAddress[3]}:{header.Port}");
}

//  5. Interop with native libraries ———————————————————————————————
[System.Runtime.InteropServices.DllImport("msvcrt.dll", CallingConvention = System.Runtime.InteropServices.CallingConvention.Cdecl)]
private static unsafe extern void* memcpy(void* dest, void* src, nint count);

// Modern interop: LibraryImport + Span<T> (avoids unsafe, .NET 7+)
[System.Runtime.InteropServices.LibraryImport("msvcrt.dll")]
private static partial void memset_s(nint dest, nint destSize, int value, nint count);

//  6. Performance: unsafe struct copy ——————————————————————————————
public static unsafe void FastCopy(byte[] src, byte[] dst, int length)
{
    fixed (byte* pSrc = src, pDst = dst)
    {
        Buffer.MemoryCopy(pSrc, pDst, dst.Length, length); // hardware-accelerated
    }
}

// Modern alternative: Span<T> (preferred)
public static void SafeCopy(ReadOnlySpan<byte> src, Span<byte> dst)
    => src[..dst.Length].CopyTo(dst); // no unsafe, no pointers
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are advanced C# patterns — pattern matching, records, and primary constructors?

Modern C# (10–14) provides expressive patterns and type features that reduce boilerplate and improve code clarity.

```cs
//  1. Extended pattern matching (C# 8–12) ——————————————————————————
public record Shape;
public record Circle(double Radius) : Shape;
public record Rectangle(double Width, double Height) : Shape;
public record Triangle(double Base, double Height) : Shape;

static string Describe(Shape shape) => shape switch
{
    Circle { Radius: 0 }                => "Degenerate circle",
    Circle { Radius: > 100 }            => "Huge circle",
    Circle c                            => $"Circle r={c.Radius:F1}",
    Rectangle { Width: var w, Height: var h } when w == h
                                        => $"Square {w}x{h}",
    Rectangle(var w, var h)             => $"Rect {w}x{h}",
    Triangle(var b, var h)              => $"Triangle b={b} h={h}",
    null                                => "null",
    _                                   => "unknown"
};

// List patterns (C# 11+)
static string DescribeList(int[] arr) => arr switch
{
    []          => "empty",
    [var x]     => $"one element: {x}",
    [var x, var y] => $"two elements: {x}, {y}",
    [1, 2, ..]  => "starts with 1, 2",
    [.., 99]    => "ends with 99",
    _           => $"{arr.Length} elements"
};

Console.WriteLine(DescribeList([]));         // empty
Console.WriteLine(DescribeList([42]));       // one element: 42
Console.WriteLine(DescribeList([1, 2, 5])); // starts with 1, 2

//  2. Records — immutable data with value semantics ————————————————
public record OrderLine(string ProductId, int Quantity, decimal UnitPrice)
{
    public decimal Total => Quantity * UnitPrice;

    // Custom deconstruct
    public void Deconstruct(out string sku, out decimal total)
        => (sku, total) = (ProductId, Total);
}

var line = new OrderLine("SKU-001", 3, 9.99m);
Console.WriteLine(line);       // OrderLine { ProductId = SKU-001, Quantity = 3, UnitPrice = 9.99 }

var modified = line with { Quantity = 5 }; // non-destructive update
Console.WriteLine(modified.Total); // 49.95

var (sku, total) = line;       // custom deconstruct
Console.WriteLine($"{sku}: £{total:F2}");

// Record struct (C# 10+) — value type record
public record struct Point(double X, double Y)
{
    public double Distance => Math.Sqrt(X * X + Y * Y);
}

//  3. Primary constructors (C# 12) —————————————————————————————————
// For classes (not just records)
public class OrderService(
    IOrderRepository repository,
    IPublishEndpoint  publishEndpoint,
    ILogger<OrderService> logger)
{
    public async Task<Order> PlaceOrderAsync(PlaceOrderRequest req, CancellationToken ct)
    {
        // Parameters are captured as fields automatically
        logger.LogInformation("Placing order for {Customer}", req.CustomerId);
        var order = new Order(req.CustomerId, req.Items);
        await repository.AddAsync(order, ct);
        await publishEndpoint.Publish(new OrderPlaced(order.Id), ct);
        return order;
    }
}

//  4. Required members (C# 11) —————————————————————————————————————
public class ProductDto
{
    public required string Name  { get; init; }
    public required decimal Price { get; init; }
    public string? Description  { get; init; }
}

// Compile error if required members not set:
// var p = new ProductDto(); 
var p = new ProductDto { Name = "Laptop", Price = 999m }; // …

//  5. Generic math (C# 11+) ———————————————————————————————————————
using System.Numerics;

static T Average<T>(IEnumerable<T> values) where T : INumber<T>
{
    T sum   = values.Aggregate(T.Zero, (acc, n) => acc + n);
    T count = T.CreateChecked(values.Count());
    return sum / count;
}

Console.WriteLine(Average([1, 2, 3, 4, 5]));           // 3
Console.WriteLine(Average([1.5, 2.5, 3.5]));            // 2.5
Console.WriteLine(Average(new decimal[] { 10m, 20m })); // 15

//  6. Interceptors (C# 12, preview) ———————————————————————————————
// Allow source generators to intercept specific call sites
// Used by EF Core compiled models, System.Text.Json, ASP.NET Core Minimal APIs
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 16. PERFORMANCE AND OPTIMIZATION

<br>

## Q. How can you improve string concatenation performance in C#?

String concatenation with `+` inside loops creates a new heap allocation on every iteration. Use `StringBuilder`, interpolated string handlers, or `string.Create` depending on context.

```cs
using System.Text;

//  O(n) — each += allocates a new string
string result = "";
for (int i = 0; i < 100_000; i++)
    result += i.ToString(); // 100,000 heap allocations

// … StringBuilder — single buffer, amortised O(1) append
var sb = new StringBuilder(capacity: 1_024_000); // pre-allocate if size is known
for (int i = 0; i < 100_000; i++)
    sb.Append(i);
string r1 = sb.ToString(); // single final allocation

// … string.Concat / Join — best for fixed number of strings
string r2 = string.Concat("Hello", " ", "World");
string r3 = string.Join(", ", new[] { "Alice", "Bob", "Carol" });

// … Interpolated strings — compiler-optimised in .NET 6+
// Uses DefaultInterpolatedStringHandler internally — no intermediate string
string name = "Alice"; int age = 30;
string r4 = $"{name} is {age}";

// … string.Create — zero-copy, write directly into final buffer (.NET 6+)
int[] numbers = [1, 2, 3, 4, 5];
string r5 = string.Create(numbers.Length * 2 - 1, numbers, (span, nums) =>
{
    for (int i = 0; i < nums.Length; i++)
    {
        span[i * 2] = (char)('0' + nums[i]);
        if (i < nums.Length - 1) span[i * 2 + 1] = ',';
    }
});
Console.WriteLine(r5); // 1,2,3,4,5

// … ValueStringBuilder / stackalloc for hot paths (advanced)
Span<char> buf = stackalloc char[256];
var vsb = new System.Text.StringBuilder(); // or use MemoryExtensions for Span-based ops

// Benchmark result (relative):
// string +=          : 10,000 ms (100k iterations)
// StringBuilder      :      3 ms
// string.Create      :      1 ms (zero intermediate allocations)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you work with parallelism and concurrency in C#?

**Concurrency** — multiple tasks making progress (may share a single thread via async).
**Parallelism** — multiple tasks executing simultaneously on multiple CPU cores.

```cs
// 1. async/await — concurrency for I/O-bound work (no extra threads)
async Task<string[]> FetchAllAsync(string[] urls)
{
    using var client = new HttpClient();
    var tasks = urls.Select(url => client.GetStringAsync(url));
    return await Task.WhenAll(tasks); // all in parallel, no thread blocking
}

// 2. Parallel.For / Parallel.ForEach — parallelism for CPU-bound work
int[] data = Enumerable.Range(1, 1_000_000).ToArray();
long sum = 0;
Parallel.For(0, data.Length,
    () => 0L,                                          // local state
    (i, _, local) => local + data[i],                 // body
    local => Interlocked.Add(ref sum, local));         // merge
Console.WriteLine(sum);

// 3. Parallel.ForEachAsync (.NET 6+) — async work with bounded parallelism
var urls = Enumerable.Range(1, 20).Select(i => $"https://api.example.com/item/{i}");
await Parallel.ForEachAsync(urls,
    new ParallelOptions { MaxDegreeOfParallelism = 4 },
    async (url, ct) =>
    {
        using var client = new HttpClient();
        var data = await client.GetStringAsync(url, ct);
        Console.WriteLine($"Got {data.Length} chars");
    });

// 4. PLINQ — parallel LINQ for data processing
var results = ParallelEnumerable.Range(1, 1_000_000)
    .AsParallel()
    .WithDegreeOfParallelism(Environment.ProcessorCount)
    .Where(n => n % 2 == 0)
    .Select(n => n * n)
    .Sum();

// 5. Channel<T> — producer/consumer pipelines (.NET Core 2.1+)
using System.Threading.Channels;

var channel = Channel.CreateBounded<int>(capacity: 100);

// Producer
var producer = Task.Run(async () =>
{
    for (int i = 0; i < 1000; i++)
    {
        await channel.Writer.WriteAsync(i);
    }
    channel.Writer.Complete();
});

// Consumer
var consumer = Task.Run(async () =>
{
    await foreach (int item in channel.Reader.ReadAllAsync())
        Console.WriteLine(item);
});

await Task.WhenAll(producer, consumer);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are some common performance issues in .NET Core applications?

| Issue | Symptom | Fix |
|-------|---------|-----|
| Excessive allocations | High GC pressure, frequent Gen 0 collections | `Span<T>`, `ArrayPool<T>`, object pooling |
| Sync-over-async | Thread starvation, deadlocks | Use `async`/`await` throughout |
| N+1 queries | Slow DB responses | EF Core `.Include()`, batch queries |
| Large object heap (LOH) | Heap fragmentation, long GC pauses | Avoid large arrays; use `ArrayPool` |
| String concatenation in loops | High memory, slow performance | `StringBuilder`, `string.Create` |
| Missing caching | Repeated expensive computations | `IMemoryCache`, `IDistributedCache` |
| Over-fetching data | Large payloads, slow queries | Projection (`Select`), pagination |
| Boxing value types | Hidden allocations | Use generics, avoid `object` parameters |
| Blocking async code | Deadlocks (`.Result`, `.Wait()`) | `await` everywhere, `ConfigureAwait(false)` |
| Missing `AsNoTracking` | Unnecessary EF change tracking | Add `.AsNoTracking()` for read-only queries |

```cs
//  Common anti-pattern: sync-over-async
public string GetData() => FetchAsync().Result; // blocks thread pool thread

// … Fix: async all the way
public async Task<string> GetDataAsync() => await FetchAsync();

//  Anti-pattern: unnecessary ToList() materialisation
var count = dbContext.Orders.ToList().Count; // loads ALL rows into memory

// … Fix: query at DB level
var count = await dbContext.Orders.CountAsync();

//  Anti-pattern: missing ConfigureAwait in libraries
await SomeLibraryMethodAsync(); // may deadlock in ASP.NET Framework context

// … Fix in library code
await SomeLibraryMethodAsync().ConfigureAwait(false);

// Detect issues via dotnet-counters
// dotnet-counters monitor --process-id <PID> System.Runtime
// Watch: gc-heap-size, gen-0-gc-count, threadpool-queue-length, alloc-rate
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you measure the performance of a .NET Core application?

```cs
// 1. Stopwatch — quick timing
using System.Diagnostics;

var sw = Stopwatch.StartNew();
PerformWork();
sw.Stop();
Console.WriteLine($"Elapsed: {sw.Elapsed.TotalMilliseconds:F3} ms");

// 2. BenchmarkDotNet — production-grade micro-benchmarks (gold standard)
// dotnet add package BenchmarkDotNet

using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

[MemoryDiagnoser]
[SimpleJob(iterationCount: 10)]
public class StringBenchmarks
{
    private const int N = 10_000;

    [Benchmark(Baseline = true)]
    public string Concatenation()
    {
        string s = "";
        for (int i = 0; i < N; i++) s += i;
        return s;
    }

    [Benchmark]
    public string StringBuilderBenchmark()
    {
        var sb = new System.Text.StringBuilder(N * 5);
        for (int i = 0; i < N; i++) sb.Append(i);
        return sb.ToString();
    }
}

BenchmarkRunner.Run<StringBenchmarks>();
// Reports: Mean, Allocated, Gen0/1/2 GC counts

// 3. Activity / OpenTelemetry — distributed tracing
using System.Diagnostics;

var source = new ActivitySource("MyApp.Performance");
using var activity = source.StartActivity("ProcessOrder");
activity?.SetTag("order.id", 42);
// ... work ...
activity?.Stop();

// 4. EventCounters — production runtime metrics
// dotnet-counters monitor -p <PID> System.Runtime Microsoft.AspNetCore.Hosting
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What tools are available for profiling .NET Core applications?

| Tool | Type | Use case |
|------|------|---------|
| **dotnet-counters** | CLI / live | Real-time CPU, GC, allocation rates |
| **dotnet-trace** | CLI / file | CPU sampling, GC events, custom events |
| **dotnet-dump** | CLI / post-mortem | Memory dumps, OOM analysis |
| **dotnet-gcdump** | CLI / heap | GC heap snapshot, object retention |
| **Visual Studio Profiler** | GUI | CPU sampling, memory snapshots, timeline |
| **PerfView** | GUI / ETW | Deep GC, JIT, thread pool analysis |
| **JetBrains dotMemory** | GUI | Memory profiling, heap diff |
| **JetBrains dotTrace** | GUI | CPU profiling, async call trees |
| **BenchmarkDotNet** | Code | Micro-benchmarks with statistics |
| **OpenTelemetry** | Code / infra | Distributed tracing, metrics, logs |

```bash
# Install global tools
dotnet tool install -g dotnet-counters
dotnet tool install -g dotnet-trace
dotnet tool install -g dotnet-dump
dotnet tool install -g dotnet-gcdump

# Live metrics
dotnet-counters monitor -p <PID> System.Runtime

# CPU trace (30 seconds)
dotnet-trace collect -p <PID> --duration 00:00:30 --output trace.nettrace

# Open in Visual Studio or PerfView:
# perfview /GCCollectOnly trace.nettrace

# Heap dump
dotnet-gcdump collect -p <PID> --output heap.gcdump
# Open in Visual Studio: File ’ Open ’ heap.gcdump

# Full memory dump
dotnet-dump collect -p <PID>
dotnet-dump analyze core_<PID>
# Then: dumpheap -stat, gcroot <address>, etc.
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use the `dotnet-counters` tool to monitor performance?

`dotnet-counters` streams real-time .NET runtime counters (GC, thread pool, exception rates, allocation rates) to the console without restarting the process.

```bash
# Install
dotnet tool install -g dotnet-counters

# List available counter providers
dotnet-counters list

# Monitor default System.Runtime counters for a process
dotnet-counters monitor --process-id 1234

# Monitor multiple providers
dotnet-counters monitor -p 1234 \
  System.Runtime \
  Microsoft.AspNetCore.Hosting \
  Microsoft.AspNetCore.Http.Connections

# Monitor by process name
dotnet-counters monitor --name MyApi

# Export to CSV for analysis
dotnet-counters collect -p 1234 \
  --output counters.csv \
  --duration 00:02:00 \
  --format csv

# Key counters to watch:
# cpu-usage                  — CPU %
# gc-heap-size               — managed heap size (MB)
# gen-0-gc-count             — Gen 0 GC frequency
# alloc-rate                 — allocation rate (bytes/sec)
# threadpool-queue-length    — pending thread pool work items
# active-timer-count         — active timers
# exception-count            — unhandled exceptions/sec
# requests-per-second        — ASP.NET Core RPS
# current-requests           — in-flight HTTP requests
```

```cs
// Emit custom EventCounters from your app
using System.Diagnostics.Tracing;

[EventSource(Name = "MyApp")]
public sealed class MyEventSource : EventSource
{
    public static readonly MyEventSource Log = new();
    private EventCounter? _requestDuration;

    public MyEventSource()
    {
        _requestDuration = new EventCounter("request-duration-ms", this)
        {
            DisplayName = "Request Duration (ms)"
        };
    }

    public void RecordRequest(double ms) => _requestDuration?.WriteMetric(ms);
}

// Usage
MyEventSource.Log.RecordRequest(42.5);

// Monitor custom counters
// dotnet-counters monitor -p <PID> MyApp
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `dotnet-trace` tool and how is it used?

`dotnet-trace` captures ETW/EventPipe events from a running .NET process into a `.nettrace` file for offline analysis of CPU, GC, JIT, and custom events.

```bash
# Install
dotnet tool install -g dotnet-trace

# List available event profiles
dotnet-trace list-profiles
# cpu-sampling   — CPU call stacks (default)
# gc-verbose     — detailed GC events
# gc-collect     — GC collection only
# none           — no built-in; specify providers manually

# Capture 30-second CPU profile
dotnet-trace collect -p <PID> \
  --profile cpu-sampling \
  --duration 00:00:30 \
  --output myapp.nettrace

# GC-focused trace
dotnet-trace collect -p <PID> \
  --profile gc-verbose \
  --output gc.nettrace

# Custom providers (e.g., ASP.NET Core + GC)
dotnet-trace collect -p <PID> \
  --providers "Microsoft.AspNetCore:4:5,System.Runtime:4:4" \
  --output custom.nettrace

# Trace from startup (catches cold-start JIT)
dotnet-trace collect -- dotnet MyApp.dll

# Analyse the trace
# Open in: Visual Studio ’ File ’ Open ’ myapp.nettrace
# Or: PerfView myapp.nettrace
# Or: speedscope.app (convert first)
dotnet-trace convert myapp.nettrace --format Speedscope
# Open myapp.speedscope.json at https://speedscope.app
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use the `dotnet-dump` tool to collect and analyze memory dumps?

`dotnet-dump` captures a full managed memory dump and provides a REPL for analysis — useful for diagnosing OOM crashes, deadlocks, and high memory usage.

```bash
# Install
dotnet tool install -g dotnet-dump

# Capture dump from running process
dotnet-dump collect -p <PID> --output myapp.dmp

# Capture on crash (set environment variable before starting)
export DOTNET_DbgEnableMiniDump=1
export DOTNET_DbgMiniDumpType=4       # 1=Mini, 2=Heap, 3=Triage, 4=Full
export DOTNET_DbgMiniDumpName=/tmp/crash_%p.dmp

# Analyze interactively
dotnet-dump analyze myapp.dmp
```

```
# Inside the analyze REPL — useful commands:

# Show all managed threads
clrthreads

# Show call stacks for all threads
clrstack -all

# Heap statistics — top object types by count/size
dumpheap -stat

# Find all instances of a type
dumpheap -type System.String -stat

# Show object at address
dumpobj 0x00007f8b1c002a18

# Find what holds a reference to an object (root analysis)
gcroot 0x00007f8b1c002a18

# Show finalizer queue
finalizequeue

# Show thread pool state
threadpool

# Show sync blocks (locks)
syncblk

# Exit
exit
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `dotnet-gcdump` tool and how is it used?

`dotnet-gcdump` captures a GC heap snapshot in `.gcdump` format — a lighter alternative to a full memory dump. It triggers a GC collection, then records all live objects and their references.

```bash
# Install
dotnet tool install -g dotnet-gcdump

# Capture heap dump from running process
dotnet-gcdump collect -p <PID> --output myapp.gcdump

# Capture from process by name
dotnet-gcdump collect --name MyApi

# Analyse in VS Code / Visual Studio
# Visual Studio: File ’ Open ’ myapp.gcdump
# Shows: object counts, sizes, retention trees

# Report from command line (top types by size)
dotnet-gcdump report myapp.gcdump

# Compare two snapshots (detect leaks)
dotnet-gcdump report baseline.gcdump after.gcdump --diff
```

```cs
// Programmatic heap size info (no external tool needed)
GCMemoryInfo info = GC.GetGCMemoryInfo();
Console.WriteLine($"Heap size:       {info.HeapSizeBytes / 1_048_576:F1} MB");
Console.WriteLine($"Committed:       {info.TotalCommittedBytes / 1_048_576:F1} MB");
Console.WriteLine($"Fragmented:      {info.FragmentedBytes / 1_024:F0} KB");
Console.WriteLine($"Gen0 size after: {info.GenerationInfo[0].SizeAfterBytes:N0} bytes");
Console.WriteLine($"Gen2 size after: {info.GenerationInfo[2].SizeAfterBytes:N0} bytes");

// Force GC and collect snapshot programmatically (for testing only)
GC.Collect(2, GCCollectionMode.Forced, blocking: true, compacting: true);
long heapBytes = GC.GetTotalMemory(forceFullCollection: false);
Console.WriteLine($"Heap after GC: {heapBytes / 1_048_576:F2} MB");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you optimize memory usage in .NET Core applications?

```cs
// 1. Use Span<T> / Memory<T> to avoid allocations
byte[] buffer = new byte[4096];
ReadOnlySpan<byte> slice = buffer.AsSpan(0, 100); // zero allocation

// 2. ArrayPool<T> — reuse buffers instead of allocating
using System.Buffers;

byte[] rented = ArrayPool<byte>.Shared.Rent(4096);
try   { /* use rented */ }
finally { ArrayPool<byte>.Shared.Return(rented, clearArray: false); }

// 3. ObjectPool<T> — reuse expensive objects
using Microsoft.Extensions.ObjectPool;

var policy = new DefaultPooledObjectPolicy<StringBuilder>();
var pool   = new DefaultObjectPool<StringBuilder>(policy, maximumRetained: 10);
var sb     = pool.Get();
try   { sb.Append("work"); var result = sb.ToString(); }
finally { pool.Return(sb); } // sb.Clear() called automatically

// 4. Use struct instead of class for small, short-lived value containers
// (avoids heap allocation when stored as local / struct field)
public readonly record struct Point(double X, double Y);

// 5. Avoid unnecessary LINQ materialisation
//  Loads everything into memory
var names = db.Products.ToList().Select(p => p.Name).ToList();
// … Projection at DB level
var names2 = await db.Products.Select(p => p.Name).ToListAsync();

// 6. String interning for frequently repeated strings
string s = string.Intern(someString);

// 7. Weak references — allow GC to collect when under pressure
var weak = new WeakReference<ExpensiveObject>(new ExpensiveObject());
if (weak.TryGetTarget(out var obj))
    obj.Use();
// else obj was collected — recreate if needed

// 8. Dispose IDisposable resources promptly
await using var stream = new FileStream("file.txt", FileMode.Open);
// stream disposed at end of using block
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are some best practices for managing memory in .NET Core?

```cs
// 1. Implement IDisposable / IAsyncDisposable for unmanaged resources
public sealed class ResourceHolder : IAsyncDisposable
{
    private readonly FileStream _stream = File.Open("data.bin", FileMode.OpenOrCreate);
    private bool _disposed;

    public async ValueTask DisposeAsync()
    {
        if (_disposed) return;
        await _stream.DisposeAsync();
        _disposed = true;
    }
}

// 2. Avoid static collections that grow without bound (memory leaks)
//  Unbounded static cache — never cleaned up
private static readonly Dictionary<int, byte[]> _cache = new();

// … Use IMemoryCache with expiry
builder.Services.AddMemoryCache();
// cache.Set(key, value, TimeSpan.FromMinutes(5));

// 3. Prefer value types for hot-path data
record struct Coordinate(double Lat, double Lon); // stack-allocated when local

// 4. GC tuning for server workloads
// In runtimeconfig.json or environment variables:
// DOTNET_GCConserveMemory=5      (0-9, higher = more aggressive GC)
// DOTNET_GCHeapHardLimit=1073741824  (1 GB hard cap)
// DOTNET_gcServer=1              (server GC — better throughput)

// 5. Use cancellation tokens to abort long operations
async Task<string> FetchWithTimeoutAsync(string url, CancellationToken ct)
{
    using var client = new HttpClient();
    using var cts = CancellationTokenSource.CreateLinkedTokenSource(ct);
    cts.CancelAfter(TimeSpan.FromSeconds(10));
    return await client.GetStringAsync(url, cts.Token);
}

// 6. Avoid finalizers — use IDisposable pattern instead
// Finalizers delay GC (object survives to Gen 1 before collection)

// 7. Pre-size collections when count is known
var list = new List<int>(capacity: 10_000);   // avoids re-allocations
var dict = new Dictionary<string, int>(capacity: 500);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you reduce the memory footprint of a .NET Core application?

```bash
# 1. Publish as trimmed self-contained (remove unused framework code)
dotnet publish -c Release -r linux-x64 --self-contained \
  -p:PublishTrimmed=true \
  -p:TrimMode=full \
  -o ./publish
# Reduces from ~80 MB to ~15-20 MB for a minimal API

# 2. Native AOT — no JIT compiler or reflection metadata in memory
dotnet publish -c Release -r linux-x64 -p:PublishAot=true

# 3. Use minimal framework components
```

```xml
<!-- .csproj — exclude unused features -->
<PropertyGroup>
  <InvariantGlobalization>true</InvariantGlobalization>  <!-- save ~10 MB -->
  <MetadataUpdaterSupport>false</MetadataUpdaterSupport>
  <UseSystemResourceKeys>true</UseSystemResourceKeys>
</PropertyGroup>
```

```cs
// 4. Server GC with memory limit
// DOTNET_GCHeapHardLimitPercent=75   — use max 75% of container memory
// DOTNET_gcServer=1                  — server GC (one heap per CPU core)
// DOTNET_GCConserveMemory=5          — favour smaller heap over throughput

// 5. Reduce per-request allocations
// Use IObjectPool, Span<T>, ArrayPool<T> (covered above)

// 6. Disable features not needed
var builder = WebApplication.CreateSlimBuilder(args); // .NET 8+ minimal builder
// CreateSlimBuilder omits: Kestrel auto-config, startup filters, hosting startup
// Result: faster startup + lower baseline memory

// 7. Monitor allocation rate with dotnet-counters
// dotnet-counters monitor -p <PID> System.Runtime
// Target: alloc-rate < 1 MB/s for steady-state server
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you optimize CPU usage in .NET Core applications?

```cs
// 1. Offload CPU-bound work to thread pool with Task.Run
app.MapGet("/compute", async () =>
{
    var result = await Task.Run(() => ExpensiveCpuWork());
    return result;
});

// 2. PLINQ for data-parallel CPU work
var results = data.AsParallel()
    .WithDegreeOfParallelism(Environment.ProcessorCount)
    .Select(item => Transform(item))
    .ToList();

// 3. Aggressive JIT inlining for hot paths
[System.Runtime.CompilerServices.MethodImpl(
    System.Runtime.CompilerServices.MethodImplOptions.AggressiveInlining)]
static int FastMath(int x) => x * x + 2 * x + 1;

// 4. SIMD / hardware intrinsics (.NET 10)
using System.Runtime.Intrinsics;
using System.Runtime.Intrinsics.X86;

static float SumSimd(float[] values)
{
    if (Avx.IsSupported && values.Length >= 8)
    {
        var sum = Vector256<float>.Zero;
        int i = 0;
        for (; i <= values.Length - 8; i += 8)
            sum = Avx.Add(sum, Vector256.LoadUnsafe(ref values[i]));
        // handle remainder
        float total = 0;
        for (int j = 0; j < Vector256<float>.Count; j++) total += sum[j];
        for (; i < values.Length; i++) total += values[i];
        return total;
    }
    return values.Sum();
}

// 5. Use Vector<T> / generic SIMD (works on all platforms)
using System.Numerics;

static long SumVector(int[] data)
{
    var vSum = Vector<long>.Zero;
    int vLen = Vector<int>.Count;
    int i = 0;
    for (; i <= data.Length - vLen; i += vLen)
    {
        var v = new Vector<int>(data, i);
        Vector.Widen(v, out var lo, out var hi);
        vSum += Vector.ConvertToInt64(lo) + Vector.ConvertToInt64(hi);
    }
    long sum = 0;
    for (int j = 0; j < Vector<long>.Count; j++) sum += vSum[j];
    for (; i < data.Length; i++) sum += data[i];
    return sum;
}

// 6. Avoid unnecessary thread context switches
// Use SemaphoreSlim instead of Monitor for async-compatible locking
var semaphore = new SemaphoreSlim(1, 1);
await semaphore.WaitAsync();
try { /* protected work */ }
finally { semaphore.Release(); }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are some best practices for optimizing CPU-bound operations?

```cs
// 1. Profile before optimising — measure first with BenchmarkDotNet or dotnet-trace
// 2. Use correct data structures — O(1) HashSet lookup vs O(n) List.Contains
var set = new HashSet<int>(data);
bool found = set.Contains(target); // O(1)

// 3. Avoid LINQ in the tightest loops — use plain for loops
//  LINQ in hot loop — delegate invocation overhead
for (int i = 0; i < 1_000_000; i++)
    total += data.Where(x => x > 0).Sum(); // re-evaluates every iteration

// … Pre-filter, use for loop in hot path
var positive = data.Where(x => x > 0).ToArray();
for (int i = 0; i < positive.Length; i++) total += positive[i];

// 4. Prefer stackalloc for small temporary buffers
Span<int> buf = stackalloc int[32]; // no heap allocation

// 5. Use ReadOnlySpan<char> for string parsing (no substrings)
ReadOnlySpan<char> input = "2026-04-19".AsSpan();
int year  = int.Parse(input[..4]);
int month = int.Parse(input[5..7]);
int day   = int.Parse(input[8..]);

// 6. Frozen collections for read-only lookup tables (.NET 8+)
using System.Collections.Frozen;
FrozenDictionary<string, int> lookup =
    new Dictionary<string, int> { ["a"] = 1, ["b"] = 2 }.ToFrozenDictionary();
// Lookup is ~30% faster than Dictionary<K,V> for read-only scenarios

// 7. Use object pooling for expensive-to-create objects
// (see ObjectPool<T> example above)

// 8. Prefer ValueTask over Task for frequently-completing async paths
public ValueTask<int> GetCachedValueAsync(int key)
{
    if (_cache.TryGetValue(key, out int v))
        return ValueTask.FromResult(v); // no Task allocation
    return new ValueTask<int>(FetchFromDbAsync(key));
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use asynchronous programming to improve performance in .NET Core?

```cs
// 1. async/await — releases thread during I/O wait
app.MapGet("/data", async (HttpClient client) =>
{
    // Thread returned to pool while waiting — handles more concurrent requests
    string data = await client.GetStringAsync("https://api.example.com/data");
    return data;
});

// 2. Parallel async I/O with Task.WhenAll
async Task<(string, string)> FetchTwoAsync()
{
    var t1 = httpClient.GetStringAsync("https://api1.example.com");
    var t2 = httpClient.GetStringAsync("https://api2.example.com");
    var results = await Task.WhenAll(t1, t2);   // both run concurrently
    return (results[0], results[1]);
}

// 3. Streaming with IAsyncEnumerable (avoids loading all data at once)
async IAsyncEnumerable<Product> StreamProductsAsync(
    [EnumeratorCancellation] CancellationToken ct = default)
{
    await foreach (var product in dbContext.Products.AsAsyncEnumerable().WithCancellation(ct))
        yield return product;
}

app.MapGet("/products/stream", (AppDbContext db) =>
    db.Products.AsAsyncEnumerable()); // ASP.NET Core streams the JSON array

// 4. ValueTask for cached/synchronous fast paths (avoid Task allocation)
private int _cached;
public ValueTask<int> GetValueAsync()
{
    if (_cached != 0) return ValueTask.FromResult(_cached); // no allocation
    return new ValueTask<int>(LoadFromDbAsync());
}

// 5. ConfigureAwait(false) in library code (avoid context capture overhead)
public async Task<string> LibraryMethodAsync()
{
    var data = await FetchAsync().ConfigureAwait(false);
    return Process(data);
}

// 6. CancellationToken — abort abandoned requests
app.MapGet("/slow", async (CancellationToken ct) =>
{
    await Task.Delay(5000, ct); // if client disconnects, ct is cancelled
    return "done";
});
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `async` and `await` keywords and how are they used?

`async` marks a method as asynchronous. `await` suspends execution of the method until the awaited task completes, **without blocking the thread**.

```cs
// Basic pattern
public async Task<string> FetchDataAsync(string url)
{
    using var client = new HttpClient();
    string data = await client.GetStringAsync(url); // thread released here
    return data.ToUpper(); // resumes here when response arrives
}

// Return types
async Task DoWorkAsync() { /* no return value */ }
async Task<int> GetCountAsync() { return 42; }
async ValueTask<int> GetCachedAsync() { return _cached; } // avoids Task alloc
async IAsyncEnumerable<int> GenerateAsync()
{
    for (int i = 0; i < 10; i++) { await Task.Delay(10); yield return i; }
}

// Awaiting multiple tasks
var t1 = FetchDataAsync("https://api1.example.com");
var t2 = FetchDataAsync("https://api2.example.com");
string[] results = await Task.WhenAll(t1, t2);   // parallel

// WhenAny — first to complete wins
var fastest = await Task.WhenAny(t1, t2);
Console.WriteLine(await fastest);

// Exception handling
try
{
    await RiskyOperationAsync();
}
catch (HttpRequestException ex)
{
    Console.WriteLine($"HTTP error: {ex.StatusCode}");
}

// Async in a console app (.NET 10 supports top-level await)
await foreach (int n in GenerateAsync())
    Console.Write($"{n} ");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use the `Task` class to perform asynchronous operations?

```cs
// 1. Task.Run — offload CPU-bound work to thread pool
var result = await Task.Run(() =>
{
    return Enumerable.Range(1, 1_000_000).Sum(x => (long)x);
});
Console.WriteLine(result); // 500000500000

// 2. Task.Delay — non-blocking pause (replaces Thread.Sleep)
await Task.Delay(TimeSpan.FromSeconds(1));

// 3. Task.WhenAll — wait for all tasks (parallel)
var tasks = urls.Select(url => httpClient.GetStringAsync(url));
string[] responses = await Task.WhenAll(tasks);

// 4. Task.WhenAny — first task to complete
var timeout = Task.Delay(5000);
var work    = DoWorkAsync();
if (await Task.WhenAny(work, timeout) == timeout)
    throw new TimeoutException();

// 5. Task<T> — task with return value
Task<int> countTask = CountItemsAsync();
int count = await countTask;

// 6. TaskCompletionSource — wrap callback-based APIs
Task<string> WrapCallback()
{
    var tcs = new TaskCompletionSource<string>();
    LegacyLibrary.DoWork(
        onSuccess: result => tcs.SetResult(result),
        onError:   ex     => tcs.SetException(ex));
    return tcs.Task;
}

// 7. Task.FromResult / Task.CompletedTask — completed tasks (no allocation for common values)
Task<int> zero = Task.FromResult(0);
Task done      = Task.CompletedTask;

// 8. Chaining with ContinueWith (prefer await over this)
Task<string> chain = Task.Run(() => 42)
    .ContinueWith(t => $"Result: {t.Result}");
Console.WriteLine(await chain);

// 9. Task cancellation
using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(5));
try
{
    await LongRunningAsync(cts.Token);
}
catch (OperationCanceledException)
{
    Console.WriteLine("Cancelled after 5 seconds");
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you optimize I/O operations in .NET Core applications?

```cs
// 1. Always use async I/O — never block on file/network/DB
//  Blocks a thread pool thread
string text = File.ReadAllText("file.txt");

// … Async — thread returns to pool during I/O wait
string text2 = await File.ReadAllTextAsync("file.txt");

// 2. Buffer reads/writes — reduce syscall count
await using var reader = new StreamReader(
    new FileStream("large.csv", FileMode.Open, FileAccess.Read,
        FileShare.Read, bufferSize: 65536, useAsync: true));
string? line;
while ((line = await reader.ReadLineAsync()) is not null)
    Process(line);

// 3. Pipelines API — zero-copy parsing (System.IO.Pipelines)
using System.IO.Pipelines;

async Task ProcessPipeAsync(PipeReader reader, CancellationToken ct)
{
    while (true)
    {
        ReadResult result = await reader.ReadAsync(ct);
        ReadOnlySequence<byte> buffer = result.Buffer;
        // Process buffer without copying
        reader.AdvanceTo(buffer.End);
        if (result.IsCompleted) break;
    }
}

// 4. HttpClient — reuse, configure timeouts, use IHttpClientFactory
builder.Services.AddHttpClient("api", client =>
{
    client.BaseAddress = new Uri("https://api.example.com");
    client.Timeout = TimeSpan.FromSeconds(10);
    client.DefaultRequestHeaders.Add("Accept", "application/json");
});

// 5. Parallel downloads with bounded parallelism
var semaphore = new SemaphoreSlim(10); // max 10 concurrent downloads
var tasks = urls.Select(async url =>
{
    await semaphore.WaitAsync();
    try   { return await client.GetStringAsync(url); }
    finally { semaphore.Release(); }
});
string[] results = await Task.WhenAll(tasks);

// 6. SocketsHttpHandler — fine-grained connection pool tuning
var handler = new SocketsHttpHandler
{
    PooledConnectionLifetime     = TimeSpan.FromMinutes(2),
    PooledConnectionIdleTimeout  = TimeSpan.FromMinutes(1),
    MaxConnectionsPerServer      = 20,
    EnableMultipleHttp2Connections = true,
};
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are some best practices for optimizing disk and network I/O?

```cs
// DISK I/O

// 1. Async file operations
await File.WriteAllTextAsync("output.txt", content);
await File.AppendAllTextAsync("log.txt", $"{DateTime.UtcNow}: event\n");

// 2. File.OpenHandle + RandomAccess (.NET 6+) — best performance, no Stream overhead
using var handle = File.OpenHandle("data.bin",
    FileMode.Open, FileAccess.Read, FileShare.Read,
    FileOptions.Asynchronous | FileOptions.SequentialScan);
var buffer = new byte[4096];
int read = await RandomAccess.ReadAsync(handle, buffer, fileOffset: 0);

// 3. MemoryMappedFile — for very large files or shared memory
using var mmf = MemoryMappedFile.CreateFromFile("huge.dat");
using var accessor = mmf.CreateViewAccessor(offset: 0, size: 4096);
byte value = accessor.ReadByte(0);

// NETWORK I/O

// 4. Reuse HttpClient via IHttpClientFactory (never new HttpClient() per request)
public class MyService(IHttpClientFactory factory)
{
    public async Task<string> GetAsync(string url)
    {
        using var client = factory.CreateClient("api");
        return await client.GetStringAsync(url);
    }
}

// 5. HTTP/2 and HTTP/3 reduce connection overhead
builder.WebHost.ConfigureKestrel(opts =>
    opts.ListenLocalhost(8080, o =>
        o.Protocols = Microsoft.AspNetCore.Server.Kestrel.Core
            .HttpProtocols.Http1AndHttp2AndHttp3));

// 6. Response compression — reduce bytes over the wire
builder.Services.AddResponseCompression(opts =>
{
    opts.EnableForHttps = true;
    opts.Providers.Add<BrotliCompressionProvider>();
    opts.Providers.Add<GzipCompressionProvider>();
});
app.UseResponseCompression();

// 7. Output caching — serve cached responses without hitting the app
builder.Services.AddOutputCache();
app.UseOutputCache();
app.MapGet("/products", GetProducts).CacheOutput(p => p.Expire(TimeSpan.FromMinutes(5)));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use caching to improve performance in .NET Core applications?

```cs
// 1. IMemoryCache — in-process, fast
builder.Services.AddMemoryCache();

public class ProductService(IMemoryCache cache, IProductRepository repo)
{
    public async Task<Product?> GetByIdAsync(int id)
    {
        return await cache.GetOrCreateAsync($"product:{id}", async entry =>
        {
            entry.AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(5);
            entry.SlidingExpiration               = TimeSpan.FromMinutes(1);
            entry.Priority                        = CacheItemPriority.Normal;
            return await repo.GetByIdAsync(id);
        });
    }
}

// 2. Output caching (.NET 7+) — cache full HTTP responses
builder.Services.AddOutputCache(options =>
{
    options.AddPolicy("ProductsCache", b => b
        .Expire(TimeSpan.FromMinutes(10))
        .SetVaryByQuery("category", "page")
        .Tag("products"));
});
app.UseOutputCache();

app.MapGet("/products", async (AppDbContext db) =>
    await db.Products.ToListAsync())
  .CacheOutput("ProductsCache");

// Evict by tag when data changes
app.MapPost("/products", async (Product p, IOutputCacheStore store, CancellationToken ct) =>
{
    // ... save product ...
    await store.EvictByTagAsync("products", ct); // invalidate cached /products responses
    return Results.Created($"/products/{p.Id}", p);
});
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `IMemoryCache` interface and how is it used?

`IMemoryCache` is the ASP.NET Core in-process cache abstraction. It stores key-value pairs in the process\'s memory with expiry policies.

```cs
// Register
builder.Services.AddMemoryCache(options =>
{
    options.SizeLimit = 1024;               // max 1024 size units
    options.CompactionPercentage = 0.25;    // remove 25% when full
});

// Inject and use
public class WeatherService(IMemoryCache cache)
{
    private static readonly string CacheKey = "weather:london";

    // Pattern 1: GetOrCreate (most common)
    public async Task<WeatherData> GetWeatherAsync()
    {
        return await cache.GetOrCreateAsync(CacheKey, async entry =>
        {
            entry.AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(10);
            entry.SlidingExpiration               = TimeSpan.FromMinutes(3);
            entry.Size                            = 1;  // counts toward SizeLimit
            entry.RegisterPostEvictionCallback((key, value, reason, state) =>
                Console.WriteLine($"Cache evicted: {key}, reason: {reason}"));

            return await FetchFromApiAsync();
        }) ?? throw new InvalidOperationException();
    }

    // Pattern 2: TryGetValue + Set
    public WeatherData? GetCachedWeather()
    {
        if (cache.TryGetValue(CacheKey, out WeatherData? data))
            return data;
        return null;
    }

    // Pattern 3: explicit Set
    public void SetWeather(WeatherData data)
    {
        var options = new MemoryCacheEntryOptions
        {
            AbsoluteExpiration = DateTimeOffset.UtcNow.AddHours(1),
            Priority = CacheItemPriority.High,
        };
        cache.Set(CacheKey, data, options);
    }

    // Invalidate
    public void InvalidateWeather() => cache.Remove(CacheKey);
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use distributed caching in .NET Core?

Distributed caching stores data outside the process — shared across multiple app instances. ASP.NET Core provides `IDistributedCache` backed by Redis, SQL Server, or NCache.

```cs
// 1. Redis distributed cache (recommended for production)
// dotnet add package Microsoft.Extensions.Caching.StackExchangeRedis
builder.Services.AddStackExchangeRedisCache(options =>
{
    options.Configuration = builder.Configuration["Redis:ConnectionString"];
    options.InstanceName  = "MyApp:";
});

// 2. SQL Server distributed cache
// dotnet add package Microsoft.Extensions.Caching.SqlServer
builder.Services.AddDistributedSqlServerCache(options =>
{
    options.ConnectionString = builder.Configuration.GetConnectionString("Default");
    options.SchemaName = "dbo";
    options.TableName  = "Cache";
});

// 3. In-memory (development / single-instance only)
builder.Services.AddDistributedMemoryCache();

// Usage via IDistributedCache
public class SessionService(IDistributedCache cache)
{
    public async Task SetUserDataAsync(string userId, UserData data, CancellationToken ct)
    {
        var json  = JsonSerializer.SerializeToUtf8Bytes(data);
        var opts  = new DistributedCacheEntryOptions
        {
            AbsoluteExpirationRelativeToNow = TimeSpan.FromHours(1),
            SlidingExpiration               = TimeSpan.FromMinutes(20),
        };
        await cache.SetAsync($"user:{userId}", json, opts, ct);
    }

    public async Task<UserData?> GetUserDataAsync(string userId, CancellationToken ct)
    {
        byte[]? bytes = await cache.GetAsync($"user:{userId}", ct);
        return bytes is null ? null : JsonSerializer.Deserialize<UserData>(bytes);
    }

    public async Task RemoveAsync(string userId, CancellationToken ct) =>
        await cache.RemoveAsync($"user:{userId}", ct);
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `IDistributedCache` interface and how is it used?

`IDistributedCache` is the abstraction for distributed caching in ASP.NET Core. It stores `byte[]` values by string key with expiry support.

```cs
// Core methods:
// Get / GetAsync           — retrieve bytes (null if not found)
// Set / SetAsync           — store bytes with options
// Refresh / RefreshAsync   — reset sliding expiration
// Remove / RemoveAsync     — delete entry

public class ProductCacheService(IDistributedCache cache)
{
    private static string Key(int id) => $"product:{id}";

    public async Task<Product?> GetAsync(int id, CancellationToken ct = default)
    {
        byte[]? bytes = await cache.GetAsync(Key(id), ct);
        return bytes is null
            ? null
            : JsonSerializer.Deserialize<Product>(bytes);
    }

    public async Task SetAsync(Product product, CancellationToken ct = default)
    {
        byte[] bytes = JsonSerializer.SerializeToUtf8Bytes(product);
        await cache.SetAsync(Key(product.Id), bytes,
            new DistributedCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(30),
                SlidingExpiration               = TimeSpan.FromMinutes(5),
            }, ct);
    }

    // Get-or-create pattern
    public async Task<Product> GetOrCreateAsync(int id,
        Func<CancellationToken, Task<Product>> factory, CancellationToken ct = default)
    {
        var cached = await GetAsync(id, ct);
        if (cached is not null) return cached;

        var product = await factory(ct);
        await SetAsync(product, ct);
        return product;
    }
}

// Extension method (HybridCache — .NET 9+, recommended)
// dotnet add package Microsoft.Extensions.Caching.Hybrid
builder.Services.AddHybridCache();

// HybridCache combines IMemoryCache (L1) + IDistributedCache (L2) automatically
public class CachedService(HybridCache hybridCache)
{
    public async Task<Product?> GetProductAsync(int id, CancellationToken ct)
        => await hybridCache.GetOrCreateAsync(
            $"product:{id}",
            async token => await FetchFromDbAsync(id, token),
            cancellationToken: ct);
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use the `ResponseCaching` middleware to cache responses?

**Response caching** stores complete HTTP responses and serves them for subsequent identical requests — reduces server-side processing.

```cs
// ResponseCaching — HTTP cache headers based (RFC 7234)
builder.Services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 64 * 1024; // max cached body = 64 KB
    options.UseCaseSensitivePaths = false;
});

app.UseResponseCaching();

// Controller — add cache headers
[ResponseCache(Duration = 60, VaryByQueryKeys = ["category"])]
[HttpGet]
public IActionResult GetProducts(string? category) => Ok();

// Minimal API
app.MapGet("/products", async (AppDbContext db) =>
    await db.Products.ToListAsync())
  .WithMetadata(new ResponseCacheAttribute
  {
      Duration = 300,         // cache for 5 minutes
      Location = ResponseCacheLocation.Any,
      VaryByHeader = "Accept-Language",
  });

//  ResponseCaching has limitations:
// - Only caches GET/HEAD responses with 200 status
// - Does NOT work with authenticated requests by default
// - Cannot be invalidated programmatically

// … Output Caching (.NET 7+) — recommended replacement
builder.Services.AddOutputCache(opts =>
{
    opts.AddBasePolicy(b => b.Expire(TimeSpan.FromMinutes(5)));
    opts.AddPolicy("Short", b => b.Expire(TimeSpan.FromSeconds(30)));
});
app.UseOutputCache();

app.MapGet("/products", GetProducts).CacheOutput();
app.MapGet("/prices",   GetPrices).CacheOutput("Short");

// Invalidate output cache programmatically
app.MapPost("/products", async (Product p, IOutputCacheStore store, CancellationToken ct) =>
{
    // ... save ...
    await store.EvictByTagAsync("products", ct);
    return Results.Created($"/products/{p.Id}", p);
});
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you optimize database access in .NET Core applications?

```cs
// 1. AsNoTracking — skip change tracking for read-only queries
var products = await db.Products
    .AsNoTracking()
    .Where(p => p.Category == "Electronics")
    .ToListAsync();

// 2. Projection — fetch only needed columns
var dtos = await db.Products
    .AsNoTracking()
    .Select(p => new ProductDto(p.Id, p.Name, p.Price))
    .ToListAsync();

// 3. Compiled queries — avoid repeated query compilation (.NET 7+)
private static readonly Func<AppDbContext, int, Task<Product?>> _getById =
    EF.CompileAsyncQuery((AppDbContext db, int id) =>
        db.Products.AsNoTracking().FirstOrDefault(p => p.Id == id));

var product = await _getById(db, 42);

// 4. Batch operations — EF Core 7+ ExecuteUpdate / ExecuteDelete
//  Load-modify-save (N round trips)
var prods = await db.Products.Where(p => p.Price < 10).ToListAsync();
foreach (var p in prods) p.Price *= 1.1m;
await db.SaveChangesAsync();

// … Single SQL UPDATE
await db.Products
    .Where(p => p.Price < 10)
    .ExecuteUpdateAsync(s => s.SetProperty(p => p.Price, p => p.Price * 1.1m));

// … Single SQL DELETE
await db.Products.Where(p => p.Stock == 0).ExecuteDeleteAsync();

// 5. Eager loading to avoid N+1
var orders = await db.Orders
    .Include(o => o.Customer)
    .Include(o => o.Lines).ThenInclude(l => l.Product)
    .AsNoTracking()
    .ToListAsync();

// 6. Pagination — never load all rows
var page = await db.Products
    .OrderBy(p => p.Name)
    .Skip((pageNumber - 1) * pageSize)
    .Take(pageSize)
    .AsNoTracking()
    .ToListAsync();

// 7. Raw SQL for complex queries
var results = await db.Database
    .SqlQuery<ProductSummary>($"SELECT Id, Name, SUM(Qty) AS TotalSold FROM ...")
    .ToListAsync();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use connection pooling to improve database performance?

Connection pooling reuses existing database connections rather than creating a new one per request. **ADO.NET and EF Core use pooling by default** — the key is configuring it correctly.

```cs
// 1. ADO.NET — pooling is automatic when using connection strings
// The connection string controls pool size:
var connStr = "Server=myserver;Database=mydb;User=sa;Password=pass;" +
              "Min Pool Size=5;Max Pool Size=100;Connection Timeout=30;";

// Open/Close does NOT destroy the connection — it returns it to the pool
await using var conn = new SqlConnection(connStr);
await conn.OpenAsync();
// ... query ...
// conn.Close() / dispose ’ returned to pool

// 2. EF Core — uses ADO.NET pooling automatically
builder.Services.AddDbContext<AppDbContext>(opt =>
    opt.UseSqlServer(connStr,
        sql => sql.CommandTimeout(30)));

// 3. DbContext pooling — EF Core level (reuse DbContext instances, .NET 6+)
// Use AddDbContextPool instead of AddDbContext
builder.Services.AddDbContextPool<AppDbContext>(opt =>
    opt.UseSqlServer(connStr), poolSize: 128);

// With factory (for Blazor, manual scope control)
builder.Services.AddPooledDbContextFactory<AppDbContext>(opt =>
    opt.UseSqlServer(connStr));

// Usage with factory
public class ProductService(IDbContextFactory<AppDbContext> factory)
{
    public async Task<List<Product>> GetAllAsync()
    {
        await using var db = await factory.CreateDbContextAsync();
        return await db.Products.AsNoTracking().ToListAsync();
    }
}

// 4. Monitor pool health
// dotnet-counters monitor -p <PID> Microsoft.Data.SqlClient
// Watch: active-hard-connects, active-soft-connects, number-of-pooled-connections

// 5. Connection resiliency (transient fault handling)
builder.Services.AddDbContext<AppDbContext>(opt =>
    opt.UseSqlServer(connStr, sql =>
        sql.EnableRetryOnFailure(
            maxRetryCount: 3,
            maxRetryDelay: TimeSpan.FromSeconds(5),
            errorNumbersToAdd: null)));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use the `AsNoTracking` method to optimize query performance?

`AsNoTracking()` tells EF Core to **skip change tracking** for returned entities — the context does not monitor them for modifications. This saves memory and CPU for read-only queries.

```cs
//  Default — EF Core tracks all returned entities (needed for updates)
var tracked = await db.Products.ToListAsync();
// EF holds a snapshot of each entity for change detection

// … AsNoTracking — no snapshot stored, faster and less memory
var readOnly = await db.Products
    .AsNoTracking()
    .ToListAsync();

// … AsNoTrackingWithIdentityResolution — avoids duplicates in navigation props
// Useful when Include() returns the same entity multiple times
var orders = await db.Orders
    .AsNoTrackingWithIdentityResolution()
    .Include(o => o.Lines)
    .ThenInclude(l => l.Product)
    .ToListAsync();

// … Global setting — all queries in this context are non-tracked
db.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

// Benchmark context (queries per second):
// With tracking    : ~15,000 QPS
// AsNoTracking     : ~25,000 QPS  (+67%)
// Projection only  : ~35,000 QPS  (+133%)

// When NOT to use AsNoTracking:
//  When you need to modify and save the entity
var product = await db.Products.FindAsync(id); // tracked — needed for update
product!.Price = newPrice;
await db.SaveChangesAsync(); // EF detects change via tracking

// AsNoTracking + manual update (EF Core 7+)
await db.Products
    .Where(p => p.Id == id)
    .ExecuteUpdateAsync(s => s.SetProperty(p => p.Price, newPrice));
// No loading needed — single UPDATE statement
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the secure coding practices (XSS, CSRF, SQL Injection) available in .NET?

```cs
//  1. XSS (Cross-Site Scripting) ——————————————————————————————————
// ASP.NET Core Razor auto-encodes output by default
@Model.UserInput  // HTML-encoded automatically — safe

// HtmlEncoder for manual encoding
using System.Text.Encodings.Web;
string safe = HtmlEncoder.Default.Encode("<script>alert(1)</script>");
// ’ &lt;script&gt;alert(1)&lt;/script&gt;

// Content Security Policy header
app.Use(async (ctx, next) =>
{
    ctx.Response.Headers.Append("Content-Security-Policy",
        "default-src 'self'; script-src 'self'; style-src 'self'");
    await next(ctx);
});

//  2. CSRF (Cross-Site Request Forgery) ———————————————————————————
// MVC — Antiforgery token (automatic with [ValidateAntiForgeryToken])
builder.Services.AddAntiforgery(opt =>
{
    opt.HeaderName = "X-XSRF-TOKEN";  // for SPA clients
    opt.Cookie.SecurePolicy = CookieSecurePolicy.Always;
    opt.Cookie.SameSite = SameSiteMode.Strict;
});

// Controller
[HttpPost, ValidateAntiForgeryToken]
public IActionResult Create(ProductForm form) { /* safe */ return Ok(); }

// Minimal API — SameSite cookie + antiforgery
app.MapPost("/products", (IAntiforgery af, HttpContext ctx) =>
{
    af.ValidateRequestAsync(ctx);
    // ...
}).RequireAuthorization();

//  3. SQL Injection ————————————————————————————————————————————————
//  Vulnerable — string interpolation into SQL
var name = userInput;
var sql = $"SELECT * FROM Products WHERE Name = '{name}'";  // NEVER DO THIS

// … EF Core — parameterised automatically
var products = await db.Products
    .Where(p => p.Name == name)
    .ToListAsync();

// … Raw SQL with parameters (EF Core 7+)
var results = await db.Products
    .FromSql($"SELECT * FROM Products WHERE Name = {name}")  // interpolated ’ parameterised
    .ToListAsync();

// … ADO.NET — explicit parameters
await using var cmd = new SqlCommand("SELECT * FROM Products WHERE Name = @name", conn);
cmd.Parameters.AddWithValue("@name", name);

//  4. Additional practices —————————————————————————————————————————
// Enforce HTTPS
app.UseHttpsRedirection();
app.UseHsts(); // HTTP Strict Transport Security

// Secure cookies
builder.Services.ConfigureApplicationCookie(opts =>
{
    opts.Cookie.HttpOnly   = true;
    opts.Cookie.SecurePolicy = CookieSecurePolicy.Always;
    opts.Cookie.SameSite  = SameSiteMode.Strict;
});

// Security headers
app.Use(async (ctx, next) =>
{
    ctx.Response.Headers.Append("X-Content-Type-Options", "nosniff");
    ctx.Response.Headers.Append("X-Frame-Options", "DENY");
    ctx.Response.Headers.Append("Referrer-Policy", "no-referrer");
    await next(ctx);
});

// Input validation at boundary
app.MapPost("/products", ([FromBody] CreateProductRequest req) =>
{
    // Data annotations validated by [ApiController] before action runs
    return Results.Ok();
});
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Span\<T\> and Memory\<T\> and when should you use them?

`Span<T>` and `Memory<T>` are stack-friendly, allocation-free views over contiguous memory. They enable high-performance parsing and buffer manipulation without heap allocations.

```cs
using System;
using System.Buffers;

//  Span<T> — stack-only, synchronous code ——————————————————————————
// Points to: stack memory (stackalloc), array slices, or unmanaged memory
// Cannot be stored in class fields or used across await boundaries

// 1. Slice an array without allocation
int[] data = { 10, 20, 30, 40, 50 };
Span<int> slice = data.AsSpan(1, 3); // no copy — just a view of [20, 30, 40]
Console.WriteLine(slice[0]); // 20
slice[0] = 99;               // mutates the original array
Console.WriteLine(data[1]);  // 99

// 2. Stack allocation — zero heap allocation
Span<byte> buffer = stackalloc byte[256];
int length = Encoding.UTF8.GetBytes("Hello, World!", buffer);
string result = Encoding.UTF8.GetString(buffer[..length]);
Console.WriteLine(result); // Hello, World!

// 3. High-performance string parsing (no substring allocations)
ReadOnlySpan<char> csv = "Alice,30,Engineer".AsSpan();
int comma1 = csv.IndexOf(',');
ReadOnlySpan<char> name = csv[..comma1];       // "Alice" — no allocation
ReadOnlySpan<char> rest = csv[(comma1 + 1)..];
int comma2 = rest.IndexOf(',');
ReadOnlySpan<char> age = rest[..comma2];       // "30"
Console.WriteLine(name.ToString()); // Alice

// 4. Span<T> in methods
static int SumSpan(ReadOnlySpan<int> numbers)
{
    int total = 0;
    foreach (var n in numbers) total += n;
    return total;
}
int[] arr = [1, 2, 3, 4, 5];
Console.WriteLine(SumSpan(arr));           // works with array
Console.WriteLine(SumSpan(arr.AsSpan(1, 3))); // slice without allocation

//  Memory<T> — can cross await, can be stored in fields ————————————
// Use Memory<T> when you need to store the view or use it with async code
public class DataProcessor
{
    private readonly Memory<byte> _buffer;

    public DataProcessor(byte[] data) => _buffer = data.AsMemory();

    public async Task ProcessAsync(CancellationToken ct)
    {
        // Memory<T> CAN cross await — Span<T> cannot
        await Task.Delay(10, ct);

        Span<byte> span = _buffer.Span; // get Span for synchronous work
        span[0] = 0xFF;
    }
}

//  MemoryPool<T> and ArrayPool<T> — reuse buffers ——————————————————
// ArrayPool<T>.Shared — pool of reusable byte arrays
async Task ProcessRequestAsync(Stream requestStream)
{
    byte[] buffer = ArrayPool<byte>.Shared.Rent(4096); // borrow from pool
    try
    {
        int read = await requestStream.ReadAsync(buffer.AsMemory(0, 4096));
        // Process buffer[0..read]
        var data = buffer.AsSpan(0, read);
        // ...
    }
    finally
    {
        ArrayPool<byte>.Shared.Return(buffer, clearArray: true); // return to pool
    }
}

//  ReadOnlySequence<T> — for fragmented memory (e.g., Pipelines) ———
// System.IO.Pipelines — zero-copy network I/O
async Task ReadPipeAsync(PipeReader reader)
{
    while (true)
    {
        ReadResult result = await reader.ReadAsync();
        ReadOnlySequence<byte> buffer = result.Buffer;

        // Parse data without copying
        if (TryParseMessage(buffer, out var message, out var consumed))
        {
            ProcessMessage(message);
            reader.AdvanceTo(consumed);
        }

        if (result.IsCompleted) break;
    }
    await reader.CompleteAsync();
}

bool TryParseMessage(ReadOnlySequence<byte> buffer, out string message, out SequencePosition consumed)
{
    message = string.Empty;
    consumed = buffer.Start;
    // ... parse logic
    return false;
}

void ProcessMessage(string msg) { }
```

**When to use:**

| Type | Use when |
|------|----------|
| `Span<T>` | Synchronous, hot-path, zero-allocation buffer work |
| `ReadOnlySpan<T>` | Parsing strings/bytes without copying |
| `Memory<T>` | Async code, stored in fields, cross-await |
| `ArrayPool<T>` | Frequently allocating large temporary arrays |
| `System.IO.Pipelines` | High-throughput network/stream parsing |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use ObjectPool\<T\> and ArrayPool\<T\> for object pooling?

**Object pooling** reuses expensive-to-create objects instead of allocating and garbage-collecting them on each use. .NET provides `ArrayPool<T>` for arrays and `ObjectPool<T>` (from `Microsoft.Extensions.ObjectPool`) for general objects.

```cs
using Microsoft.Extensions.ObjectPool;
using System.Buffers;
using System.Text;

//  ArrayPool<T> — reuse arrays, avoid GC pressure ——————————————————
public class CsvParser
{
    public List<string[]> Parse(string csvContent)
    {
        var rows = new List<string[]>();
        // Rent a buffer instead of creating new char[]
        char[] buffer = ArrayPool<char>.Shared.Rent(csvContent.Length);
        try
        {
            csvContent.CopyTo(0, buffer, 0, csvContent.Length);
            // Process buffer...
            rows.Add(new[] { "parsed", "row" }); // example
        }
        finally
        {
            ArrayPool<char>.Shared.Return(buffer, clearArray: false);
        }
        return rows;
    }
}

//  ObjectPool<T> for StringBuilder ——————————————————————————————————
// Register pool in DI
builder.Services.AddSingleton<ObjectPoolProvider, DefaultObjectPoolProvider>();
builder.Services.AddSingleton(sp =>
    sp.GetRequiredService<ObjectPoolProvider>().CreateStringBuilderPool());

// Usage in a service
public class HtmlRenderer(ObjectPool<StringBuilder> sbPool)
{
    public string Render(IEnumerable<string> items)
    {
        StringBuilder sb = sbPool.Get(); // borrow from pool
        try
        {
            sb.Append("<ul>");
            foreach (var item in items)
                sb.Append("<li>").Append(HtmlEncode(item)).Append("</li>");
            sb.Append("</ul>");
            return sb.ToString();
        }
        finally
        {
            sbPool.Return(sb); // return (pool resets it automatically)
        }
    }

    private static string HtmlEncode(string s)
        => System.Net.WebUtility.HtmlEncode(s);
}

//  Custom ObjectPool policy —————————————————————————————————————————
public class HttpClientPolicy : IPooledObjectPolicy<HttpClient>
{
    public HttpClient Create() => new HttpClient
    {
        Timeout = TimeSpan.FromSeconds(30),
        DefaultRequestHeaders = { { "User-Agent", "MyApp/1.0" } }
    };

    public bool Return(HttpClient obj)
    {
        // Reset state before returning to pool
        obj.DefaultRequestHeaders.Clear();
        return true; // return true to keep in pool, false to discard
    }
}

// Register custom pool
builder.Services.AddSingleton<ObjectPoolProvider, DefaultObjectPoolProvider>();
builder.Services.AddSingleton<ObjectPool<HttpClient>>(sp =>
    sp.GetRequiredService<ObjectPoolProvider>().Create(new HttpClientPolicy()));

//  MemoryPool<T> for streaming scenarios ———————————————————————————
async Task ProcessStreamAsync(Stream stream, CancellationToken ct)
{
    using IMemoryOwner<byte> owner = MemoryPool<byte>.Shared.Rent(8192);
    Memory<byte> buffer = owner.Memory[..8192];

    int read;
    while ((read = await stream.ReadAsync(buffer, ct)) > 0)
    {
        ProcessChunk(buffer.Span[..read]);
    }
}

void ProcessChunk(ReadOnlySpan<byte> data) { /* process */ }

//  Benchmark: pool vs new allocation ———————————————————————————————
// Without pooling: new StringBuilder() per request
// With pooling:    ~6— faster, near-zero GC for StringBuilder operations
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you benchmark .NET code with BenchmarkDotNet?

**BenchmarkDotNet** is the standard .NET benchmarking library that measures method execution time, memory allocations, and GC pressure with statistical accuracy.

```bash
dotnet add package BenchmarkDotNet
dotnet add package BenchmarkDotNet.Diagnostics.Windows  # for memory profiling on Windows
```

```cs
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;
using BenchmarkDotNet.Configs;
using BenchmarkDotNet.Diagnosers;
using System.Text;

//  1. Basic benchmark ——————————————————————————————————————————————
[MemoryDiagnoser]          // track GC allocations
[SimpleJob(launchCount: 1, warmupCount: 3, iterationCount: 10)]
[RankColumn]               // show relative rank
public class StringBenchmarks
{
    private const int N = 10_000;
    private readonly string[] _words;

    public StringBenchmarks()
    {
        _words = Enumerable.Range(0, N)
            .Select(i => $"word{i}")
            .ToArray();
    }

    [Benchmark(Baseline = true)]
    public string StringConcat()
    {
        string result = "";
        foreach (var w in _words) result += w + " ";
        return result;
    }

    [Benchmark]
    public string StringBuilderAppend()
    {
        var sb = new StringBuilder();
        foreach (var w in _words) sb.Append(w).Append(' ');
        return sb.ToString();
    }

    [Benchmark]
    public string StringJoin() => string.Join(" ", _words);

    [Benchmark]
    public string StringCreate()
    {
        int totalLen = _words.Sum(w => w.Length + 1);
        return string.Create(totalLen, _words, (span, words) =>
        {
            int pos = 0;
            foreach (var w in words)
            {
                w.CopyTo(span[pos..]);
                pos += w.Length;
                span[pos++] = ' ';
            }
        });
    }
}

//  2. Parametrized benchmark ———————————————————————————————————————
[MemoryDiagnoser]
public class CollectionBenchmarks
{
    [Params(100, 1_000, 10_000)]
    public int Size { get; set; }

    private int[] _data = null!;

    [GlobalSetup]
    public void Setup() => _data = Enumerable.Range(0, Size).ToArray();

    [Benchmark(Baseline = true)]
    public int LinqSum() => _data.Sum();

    [Benchmark]
    public int ForLoopSum()
    {
        int sum = 0;
        for (int i = 0; i < _data.Length; i++) sum += _data[i];
        return sum;
    }

    [Benchmark]
    public int SpanSum()
    {
        ReadOnlySpan<int> span = _data;
        int sum = 0;
        foreach (var n in span) sum += n;
        return sum;
    }

    [Benchmark]
    public int ParallelSum() => _data.AsParallel().Sum();
}

//  3. Advanced config with categories and filters ——————————————————
[Config(typeof(AntiVirusFriendlyConfig))]
[CategoriesColumn]
[GroupBenchmarksBy(BenchmarkLogicalGroupRule.ByCategory)]
public class SerializationBenchmarks
{
    private static readonly object _data = new { Name = "Alice", Age = 30 };

    [Benchmark, BenchmarkCategory("JSON")]
    public string SystemTextJson()
        => System.Text.Json.JsonSerializer.Serialize(_data);

    [Benchmark, BenchmarkCategory("JSON")]
    public string NewtonsoftJson()
        => Newtonsoft.Json.JsonConvert.SerializeObject(_data);
}

class AntiVirusFriendlyConfig : ManualConfig
{
    public AntiVirusFriendlyConfig()
    {
        AddJob(BenchmarkDotNet.Jobs.Job.MediumRun
            .WithEnvironmentVariable("COMPlus_EnableAVX2", "1"));
    }
}

//  4. Run benchmarks ———————————————————————————————————————————————
// Program.cs — must run in Release mode: dotnet run -c Release
class Program
{
    static void Main(string[] args)
    {
        // Run all benchmarks in the assembly
        var summary = BenchmarkSwitcher
            .FromAssembly(typeof(Program).Assembly)
            .Run(args);

        // Or run a specific class
        // BenchmarkRunner.Run<StringBenchmarks>();
    }
}

/*
Sample output:
| Method              | N      | Mean        | Allocated |
|---------------------|--------|-------------|-----------|
| StringConcat        | 10000  | 52,834.3 us | 500.1 MB  |   baseline
| StringBuilderAppend | 10000  |    281.6 us |   0.7 MB  |   187x faster
| StringJoin          | 10000  |    178.4 us |   0.4 MB  |   296x faster
| StringCreate        | 10000  |    121.3 us |   0.2 MB  |   435x faster
*/
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you profile .NET applications for performance issues?

.NET provides built-in CLI tools (`dotnet-trace`, `dotnet-counters`, `dotnet-dump`) and integrates with Visual Studio and PerfView.

```bash
#  Install .NET diagnostic tools ——————————————————————————————————
dotnet tool install --global dotnet-trace
dotnet tool install --global dotnet-counters
dotnet tool install --global dotnet-dump
dotnet tool install --global dotnet-gcdump

#  dotnet-counters — real-time metrics monitoring ——————————————————
# List available counters
dotnet-counters list

# Monitor a running process (by PID or process name)
dotnet-counters monitor --process-id 12345 \
  --counters System.Runtime,Microsoft.AspNetCore.Hosting

# Key counters to watch:
# cpu-usage                 ’ CPU %
# gc-heap-size              ’ total managed heap
# gen-0/1/2-gc-count        ’ GC frequency
# exception-count           ’ exception rate
# threadpool-thread-count   ’ thread saturation
# requests-per-second       ’ ASP.NET Core throughput
# requests-current          ’ in-flight requests

#  dotnet-trace — CPU sampling and event tracing ———————————————————
# Collect CPU profile (30 seconds)
dotnet-trace collect --process-id 12345 \
  --profile cpu-sampling \
  --duration 00:00:30 \
  --output trace.nettrace

# Collect with GC events
dotnet-trace collect --process-id 12345 \
  --providers "Microsoft-Windows-DotNETRuntime:0x1:5" \
  --output gc-trace.nettrace

# Convert to speedscope format (view at speedscope.app)
dotnet-trace convert trace.nettrace --format Speedscope

#  dotnet-dump — memory analysis ———————————————————————————————————
# Capture memory dump
dotnet-dump collect --process-id 12345 --output dump.dmp

# Analyze dump
dotnet-dump analyze dump.dmp

# Useful commands inside the analyzer:
# dumpheap -stat          ’ objects by type and size
# dumpheap -type string   ’ all string objects
# gcroot <address>        ’ find what\'s keeping an object alive
# finalizequeue           ’ objects with finalizers
# sos threads             ’ all managed threads and stack traces

#  dotnet-gcdump — GC heap snapshot ———————————————————————————————
dotnet-gcdump collect --process-id 12345 --output heap.gcdump
# Open in Visual Studio or dotnet-gcdump report heap.gcdump
```

```cs
//  In-code diagnostics ——————————————————————————————————————————————
using System.Diagnostics;
using System.Diagnostics.Metrics;

// 1. Activity (distributed tracing)
private static readonly ActivitySource _activitySource = new("MyApp.OrderService");

public async Task<Order> ProcessOrderAsync(Guid orderId, CancellationToken ct)
{
    using var activity = _activitySource.StartActivity("ProcessOrder");
    activity?.SetTag("order.id", orderId.ToString());
    activity?.SetTag("order.source", "api");

    try
    {
        var order = await GetOrderAsync(orderId, ct);
        activity?.SetTag("order.total", order.Total);
        activity?.SetStatus(ActivityStatusCode.Ok);
        return order;
    }
    catch (Exception ex)
    {
        activity?.SetStatus(ActivityStatusCode.Error, ex.Message);
        activity?.RecordException(ex);
        throw;
    }
}

// 2. Custom metrics with System.Diagnostics.Metrics (.NET 8+)
private static readonly Meter _meter = new("MyApp.OrderService", "1.0");
private static readonly Counter<long> _ordersPlaced = _meter.CreateCounter<long>(
    "orders.placed",
    description: "Total orders placed");
private static readonly Histogram<double> _processingTime = _meter.CreateHistogram<double>(
    "orders.processing_time_ms",
    unit: "ms",
    description: "Order processing time");

public async Task<Order> PlaceOrderAsync(PlaceOrderRequest req, CancellationToken ct)
{
    var sw = Stopwatch.StartNew();
    try
    {
        var order = await CreateOrderInternalAsync(req, ct);
        _ordersPlaced.Add(1, new TagList { { "status", "success" } });
        return order;
    }
    catch
    {
        _ordersPlaced.Add(1, new TagList { { "status", "error" } });
        throw;
    }
    finally
    {
        _processingTime.Record(sw.Elapsed.TotalMilliseconds);
    }
}

// 3. EventCounters for lightweight runtime monitoring
public class RequestEventCounters : EventSource
{
    public static readonly RequestEventCounters Log = new();
    private EventCounter? _requestDuration;
    private IncrementingEventCounter? _requestCount;

    protected override void OnEventSourceCreated()
    {
        _requestDuration = new EventCounter("request-duration", this);
        _requestCount    = new IncrementingEventCounter("request-count", this);
    }

    public void RecordRequest(double durationMs)
    {
        _requestDuration?.WriteMetric(durationMs);
        _requestCount?.Increment();
    }
}
```

**Common profiling tools:**

| Tool | Best for |
|------|----------|
| `dotnet-counters` | Live monitoring, CPU/GC/requests |
| `dotnet-trace` | CPU flame graphs, hot method identification |
| `dotnet-dump` | Memory leaks, large heap analysis |
| Visual Studio Profiler | Detailed call tree, allocation tracking |
| PerfView | Advanced ETW/GC event analysis |
| JetBrains dotMemory | Memory snapshots, object retention |
| Application Insights | Production distributed tracing |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 17. MICROSERVICES AND DISTRIBUTED SYSTEMS

<br>

## Q. What are microservices and why are they used?

**Microservices** is an architectural style where an application is composed of small, independently deployable services, each responsible for a specific business capability and communicating via APIs.

```
Monolith                         Microservices
—————————————————————————      ———————————  ———————————  ———————————
  UI + Business + Data           Order        Catalog      Payment  
  (all in one process)     ’     Service      Service      Service  
—————————————————————————      ———————————  ———————————  ———————————
                                                                   
                                 Each has its own DB, deploy, scale, team
```

**Why use microservices?**

| Benefit | Detail |
|---------|--------|
| **Independent deployment** | Deploy Order Service without touching Payment Service |
| **Independent scaling** | Scale only the Catalog Service during a sale |
| **Technology diversity** | Each service can use a different language/DB |
| **Fault isolation** | Catalog failure doesn\'t bring down Orders |
| **Team autonomy** | Small teams own end-to-end services |
| **Faster release cycles** | Smaller, focused deployments |

**When NOT to use microservices:**
- Small teams / early-stage products — start with a monolith
- When services need very frequent synchronous coordination (distributed monolith anti-pattern)
- When operational complexity (containers, service mesh, distributed tracing) outweighs benefits

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement microservices in .NET Core?

Each microservice is a separate ASP.NET Core Web API project with its own database, deployed independently as a Docker container.

```cs
// 1. Create a minimal microservice (OrderService)
// dotnet new webapi -n OrderService

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddDbContext<OrderDbContext>(opt =>
    opt.UseNpgsql(builder.Configuration.GetConnectionString("Orders")));
builder.Services.AddScoped<IOrderRepository, OrderRepository>();
builder.Services.AddScoped<OrderService>();

// Register HttpClient for inter-service calls
builder.Services.AddHttpClient<ICatalogClient, CatalogClient>(client =>
    client.BaseAddress = new Uri(builder.Configuration["Services:Catalog"]!));

var app = builder.Build();
app.MapOrderEndpoints(); // feature-sliced minimal API endpoints
app.Run();

// 2. Typed HttpClient for service-to-service communication
public class CatalogClient(HttpClient client)
{
    public async Task<CatalogItem?> GetItemAsync(int id, CancellationToken ct = default)
    {
        var response = await client.GetAsync($"/api/catalog/{id}", ct);
        response.EnsureSuccessStatusCode();
        return await response.Content.ReadFromJsonAsync<CatalogItem>(cancellationToken: ct);
    }
}

// 3. Simple endpoint
app.MapPost("/orders", async (CreateOrderRequest req, OrderService svc, CancellationToken ct) =>
{
    var order = await svc.CreateAsync(req, ct);
    return Results.Created($"/orders/{order.Id}", order);
});

// 4. Health checks — required for Kubernetes probes
builder.Services.AddHealthChecks()
    .AddNpgSql(connStr, name: "database")
    .AddUrlGroup(new Uri("http://catalog-service/health"), name: "catalog");

app.MapHealthChecks("/health");
app.MapHealthChecks("/health/ready", new() { Predicate = r => r.Tags.Contains("ready") });
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the main advantages of using microservices architecture?

| Advantage | Description |
|-----------|-------------|
| **Independent scaling** | Scale only bottleneck services |
| **Independent deployment** | Deploy/rollback one service without affecting others |
| **Technology flexibility** | Python ML model + C# API + Go service in same system |
| **Fault isolation** | Circuit breakers prevent cascade failures |
| **Team autonomy** | Teams own and deploy their service end-to-end |
| **Smaller codebases** | Easier to understand, test, and onboard |
| **Faster iteration** | Frequent small releases without full-system regression |
| **Horizontal scalability** | Run 10 instances of Order Service, 2 of Admin |

```
Example: E-Commerce Platform

—————————————   ——————————————   —————————————   —————————————
  API Gateway – Order Service–Catalog Svc     Payment Svc  
 (YARP/Ocelot)     (C# + PG)     (C# + PG)       (C# + Redis) 
—————————————   ——————————————   —————————————   —————————————
                                                               
                  ————–——————                    ———————–——————
                   RabbitMQ /                        Notification  
                   Azure SB    ————————————————–  Service       
                  —————————————                    ————————————————

Each service:
 - Owns its database (no shared DB)
 - Has its own Docker image
 - Has its own CI/CD pipeline
 - Scales independently in Kubernetes
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle communication between microservices in .NET Core?

**Synchronous** (request-response): HTTP/REST, gRPC
**Asynchronous** (event-driven): message queues (RabbitMQ, Azure Service Bus, Kafka)

```cs
// 1. HTTP REST — typed HttpClient via IHttpClientFactory
builder.Services.AddHttpClient<ICatalogClient, CatalogClient>(client =>
    client.BaseAddress = new Uri("http://catalog-service"));

public class CatalogClient(HttpClient client)
{
    public Task<Product?> GetProductAsync(int id) =>
        client.GetFromJsonAsync<Product>($"/products/{id}");
}

// 2. gRPC — binary protocol, strongly-typed contracts (.proto files)
// dotnet add package Grpc.AspNetCore
// Service: catalog.proto ’ generated CatalogService.CatalogServiceClient

builder.Services.AddGrpcClient<CatalogService.CatalogServiceClient>(opts =>
    opts.Address = new Uri("https://catalog-service:5001"));

public class OrderService(CatalogService.CatalogServiceClient grpcClient)
{
    public async Task<ProductInfo> GetProductInfoAsync(int id)
    {
        var reply = await grpcClient.GetProductAsync(new ProductRequest { Id = id });
        return new ProductInfo(reply.Name, reply.Price);
    }
}

// 3. Async messaging — MassTransit + RabbitMQ
// dotnet add package MassTransit.RabbitMQ
builder.Services.AddMassTransit(x =>
{
    x.AddConsumer<OrderCreatedConsumer>();
    x.UsingRabbitMq((ctx, cfg) =>
    {
        cfg.Host("rabbitmq://localhost");
        cfg.ConfigureEndpoints(ctx);
    });
});

// Publisher
public class OrderService(IPublishEndpoint publish)
{
    public async Task CreateOrderAsync(CreateOrderRequest req)
    {
        // ... create order in DB ...
        await publish.Publish(new OrderCreated(orderId, req.Items));
    }
}

// Consumer in another service
public class OrderCreatedConsumer : IConsumer<OrderCreated>
{
    public async Task Consume(ConsumeContext<OrderCreated> ctx)
    {
        var msg = ctx.Message;
        // process the event (e.g., send confirmation email)
    }
}

record OrderCreated(int OrderId, List<OrderItem> Items);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the role of API Gateway in microservices architecture?

The **API Gateway** is the single entry point for all clients. It handles routing, authentication, rate limiting, SSL termination, and request aggregation — preventing clients from knowing about individual services.

```
Client (React App / Mobile)
          
          –
  ———————————————
    API Gateway     YARP / Ocelot / Azure API Management
                     - Route /orders ’ OrderService
    Auth (JWT)       - Route /catalog ’ CatalogService
    Rate Limit       - Aggregate /dashboard ’ multiple services
    Load Balance     - Strip/add headers
  ———————————————
     /      |      \
Order   Catalog  Payment
Service Service  Service
```

```cs
// YARP (Yet Another Reverse Proxy) — Microsoft\'s API Gateway (.NET 10)
// dotnet add package Yarp.ReverseProxy

builder.Services.AddReverseProxy()
    .LoadFromConfig(builder.Configuration.GetSection("ReverseProxy"));

app.MapReverseProxy();

// appsettings.json
{
  "ReverseProxy": {
    "Routes": {
      "orders-route": {
        "ClusterId": "orders-cluster",
        "Match": { "Path": "/api/orders/{**catch-all}" },
        "AuthorizationPolicy": "default"
      },
      "catalog-route": {
        "ClusterId": "catalog-cluster",
        "Match": { "Path": "/api/catalog/{**catch-all}" }
      }
    },
    "Clusters": {
      "orders-cluster": {
        "Destinations": {
          "primary": { "Address": "http://order-service:8080/" }
        }
      },
      "catalog-cluster": {
        "Destinations": {
          "primary":   { "Address": "http://catalog-service:8080/" },
          "secondary": { "Address": "http://catalog-service-2:8080/" }
        },
        "LoadBalancingPolicy": "RoundRobin"
      }
    }
  }
}

// Add rate limiting
builder.Services.AddRateLimiter(opts =>
    opts.AddFixedWindowLimiter("api", o =>
    {
        o.PermitLimit = 100;
        o.Window = TimeSpan.FromMinutes(1);
    }));
app.UseRateLimiter();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you manage data consistency in microservices?

Each microservice owns its database — no shared DB. Consistency is maintained through **eventual consistency** patterns.

```cs
// 1. Saga Pattern (Choreography) — services react to events
// OrderService publishes ’ InventoryService and PaymentService consume

// OrderService
await publishEndpoint.Publish(new OrderPlaced(orderId, customerId, items));

// InventoryService consumer
public class OrderPlacedConsumer : IConsumer<OrderPlaced>
{
    public async Task Consume(ConsumeContext<OrderPlaced> ctx)
    {
        var reserved = await inventory.ReserveAsync(ctx.Message.Items);
        if (reserved)
            await ctx.Publish(new InventoryReserved(ctx.Message.OrderId));
        else
            await ctx.Publish(new InventoryFailed(ctx.Message.OrderId));
    }
}

// 2. Saga Pattern (Orchestration) — MassTransit StateMachine
public class OrderStateMachine : MassTransitStateMachine<OrderState>
{
    public OrderStateMachine()
    {
        Initially(
            When(OrderPlacedEvent)
                .Activity(x => x.OfInstanceType<ReserveInventoryActivity>())
                .TransitionTo(AwaitingInventory));

        During(AwaitingInventory,
            When(InventoryReservedEvent)
                .Activity(x => x.OfInstanceType<ChargePaymentActivity>())
                .TransitionTo(AwaitingPayment),
            When(InventoryFailedEvent)
                .TransitionTo(Cancelled));
    }

    public State AwaitingInventory { get; private set; } = default!;
    public State AwaitingPayment   { get; private set; } = default!;
    public State Cancelled         { get; private set; } = default!;
    public Event<OrderPlaced>         OrderPlacedEvent         { get; private set; } = default!;
    public Event<InventoryReserved>   InventoryReservedEvent   { get; private set; } = default!;
    public Event<InventoryFailed>     InventoryFailedEvent     { get; private set; } = default!;
}

// 3. Outbox Pattern — guarantee event delivery even if service crashes
// Store event in DB (same transaction as business data), then publish
await using var tx = await db.Database.BeginTransactionAsync();
db.Orders.Add(newOrder);
db.OutboxMessages.Add(new OutboxMessage(
    nameof(OrderCreated),
    JsonSerializer.Serialize(new OrderCreated(newOrder.Id))));
await db.SaveChangesAsync(); // atomic: order + outbox message
await tx.CommitAsync();
// Background worker reads outbox and publishes to message bus
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are some common challenges when working with microservices?

| Challenge | Description | Solution |
|-----------|-------------|---------|
| **Distributed tracing** | Requests span multiple services | OpenTelemetry + Jaeger/Zipkin |
| **Data consistency** | No shared DB, eventual consistency | Saga pattern, outbox |
| **Network failures** | Inter-service calls can fail | Retry, circuit breaker (Polly) |
| **Service discovery** | Services need to find each other | Kubernetes DNS, Consul |
| **Testing complexity** | Integration tests across services | Contract testing (Pact), test containers |
| **Security** | JWT propagation, mTLS | JWT forwarding, service mesh |
| **Configuration** | Many services, many configs | Kubernetes ConfigMaps, Azure App Config |
| **Versioning** | API changes break consumers | Versioned APIs, backward compat |
| **Operational overhead** | Many deployments to manage | Kubernetes, Helm, GitOps |

```cs
// Polly — resilience library for network failures
// dotnet add package Microsoft.Extensions.Http.Resilience (.NET 8+)

builder.Services.AddHttpClient<ICatalogClient, CatalogClient>()
    .AddStandardResilienceHandler(opts =>
    {
        opts.Retry.MaxRetryAttempts = 3;
        opts.Retry.Delay = TimeSpan.FromMilliseconds(200);
        opts.Retry.BackoffType = DelayBackoffType.Exponential;
        opts.CircuitBreaker.FailureRatio = 0.5;
        opts.CircuitBreaker.SamplingDuration = TimeSpan.FromSeconds(10);
        opts.TotalRequestTimeout.Timeout = TimeSpan.FromSeconds(30);
    });

// OpenTelemetry — distributed tracing across services
builder.Services.AddOpenTelemetry()
    .WithTracing(tracing => tracing
        .AddAspNetCoreInstrumentation()
        .AddHttpClientInstrumentation()
        .AddEntityFrameworkCoreInstrumentation()
        .AddOtlpExporter(opts => opts.Endpoint = new Uri("http://jaeger:4317")));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you deploy microservices in a containerized environment?

```dockerfile
# Dockerfile — multi-stage build for OrderService
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src
COPY ["OrderService/OrderService.csproj", "OrderService/"]
RUN dotnet restore "OrderService/OrderService.csproj"
COPY . .
RUN dotnet publish "OrderService/OrderService.csproj" -c Release -o /app/publish \
    --no-restore /p:UseAppHost=false

FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS runtime
WORKDIR /app
COPY --from=build /app/publish .
EXPOSE 8080
ENTRYPOINT ["dotnet", "OrderService.dll"]
```

```yaml
# Kubernetes deployment — order-service.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: order-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: order-service
  template:
    metadata:
      labels:
        app: order-service
    spec:
      containers:
      - name: order-service
        image: myregistry.azurecr.io/order-service:1.2.0
        ports:
        - containerPort: 8080
        env:
        - name: ConnectionStrings__Orders
          valueFrom:
            secretKeyRef:
              name: db-secrets
              key: orders-conn-string
        resources:
          requests: { cpu: "100m", memory: "128Mi" }
          limits:   { cpu: "500m", memory: "512Mi" }
        livenessProbe:
          httpGet: { path: /health, port: 8080 }
          initialDelaySeconds: 10
        readinessProbe:
          httpGet: { path: /health/ready, port: 8080 }
---
apiVersion: v1
kind: Service
metadata:
  name: order-service
spec:
  selector:
    app: order-service
  ports:
  - port: 80
    targetPort: 8080
```

```bash
# Deploy
kubectl apply -f order-service.yaml

# Rolling update to new version
kubectl set image deployment/order-service order-service=myregistry.azurecr.io/order-service:1.3.0

# Scale
kubectl scale deployment order-service --replicas=5
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of service discovery in microservices?

**Service discovery** allows microservices to find each other\'s network locations dynamically — without hardcoded IP addresses — as services scale, restart, or move.

```
Without service discovery:
  OrderService ’ "http://192.168.1.42:8080" (hardcoded — breaks on redeploy)

With service discovery:
  OrderService ’ "http://catalog-service" ’ Discovery resolves ’ "http://10.0.0.15:8080"
```

| Approach | Tools | .NET Integration |
|----------|-------|-----------------|
| **Kubernetes DNS** | K8s built-in | `http://catalog-service` resolves via kube-dns |
| **Consul** | HashiCorp Consul | `Steeltoe.Discovery.Consul` |
| **Eureka** | Netflix Eureka | `Steeltoe.Discovery.Eureka` |
| **Azure Service Fabric** | Service Fabric DNS | Built-in naming service |

```cs
// Kubernetes — simplest; use service name as hostname
builder.Services.AddHttpClient<ICatalogClient, CatalogClient>(client =>
{
    // K8s DNS resolves "catalog-service" to the ClusterIP
    client.BaseAddress = new Uri(
        builder.Configuration["Services:Catalog"] ?? "http://catalog-service");
});

// Consul service discovery (Steeltoe)
// dotnet add package Steeltoe.Discovery.Consul

builder.Services.AddServiceDiscovery(b => b.UseConsul());
builder.Services.AddHttpClient<ICatalogClient, CatalogClient>()
    .AddServiceDiscovery(); // resolves "catalog-service" via Consul

// appsettings.json
{
  "Consul": { "Host": "consul-server", "Port": 8500 },
  "Spring": {
    "Application": { "Name": "order-service" },
    "Cloud": { "Discovery": { "Enabled": true } }
  }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement logging and monitoring in microservices?

```cs
// 1. Structured logging with Serilog — correlate across services
// dotnet add package Serilog.AspNetCore Serilog.Sinks.OpenTelemetry

builder.Host.UseSerilog((ctx, cfg) => cfg
    .ReadFrom.Configuration(ctx.Configuration)
    .Enrich.FromLogContext()
    .Enrich.WithProperty("Service", "OrderService")
    .Enrich.WithProperty("Environment", ctx.HostingEnvironment.EnvironmentName)
    .WriteTo.Console(new JsonFormatter())
    .WriteTo.OpenTelemetry(opts =>
        opts.Endpoint = "http://otel-collector:4317"));

// Use correlation ID middleware
app.Use(async (ctx, next) =>
{
    var correlationId = ctx.Request.Headers["X-Correlation-ID"].FirstOrDefault()
                        ?? Guid.NewGuid().ToString();
    ctx.Response.Headers["X-Correlation-ID"] = correlationId;
    using (Serilog.Context.LogContext.PushProperty("CorrelationId", correlationId))
        await next(ctx);
});

// 2. OpenTelemetry — metrics + tracing + logs
builder.Services.AddOpenTelemetry()
    .WithTracing(t => t
        .AddAspNetCoreInstrumentation()
        .AddHttpClientInstrumentation()
        .AddSource("OrderService")
        .AddOtlpExporter(o => o.Endpoint = new Uri("http://otel-collector:4317")))
    .WithMetrics(m => m
        .AddAspNetCoreInstrumentation()
        .AddRuntimeInstrumentation()
        .AddOtlpExporter());

// 3. Health checks with detailed status
builder.Services.AddHealthChecks()
    .AddNpgSql(connStr)
    .AddRabbitMQ(rabbitUri)
    .AddCheck("self", () => HealthCheckResult.Healthy());

app.MapHealthChecks("/health/live",  new() { Predicate = _ => false });  // liveness
app.MapHealthChecks("/health/ready", new() { Predicate = _ => true  });  // readiness

// 4. Custom metrics
var meter = new System.Diagnostics.Metrics.Meter("OrderService");
var ordersCreated = meter.CreateCounter<long>("orders.created");
ordersCreated.Add(1, new("status", "success"));

// 5. Propagate trace context across HTTP calls (automatic with HttpClient instrumentation)
// X-B3-TraceId / traceparent headers forwarded automatically
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between monolithic and microservices architecture?

| Aspect | Monolith | Microservices |
|--------|----------|---------------|
| **Codebase** | Single deployable unit | Multiple independent services |
| **Deployment** | Deploy entire app for any change | Deploy only changed service |
| **Scaling** | Scale the whole app | Scale individual services |
| **Technology** | Single stack | Polyglot — each service chooses |
| **Data** | Single shared database | Each service owns its DB |
| **Failure** | One bug can crash everything | Failures isolated per service |
| **Complexity** | Simple locally, hard to scale | Complex ops, easy to scale |
| **Team size** | Small-medium teams | Large orgs, multiple teams |
| **Testing** | Simpler end-to-end | Complex distributed testing |
| **Latency** | In-process calls (fast) | Network calls (slower) |

```
When to choose what:

Monolith …                    Microservices …
———————————————              ———————————————————————————————
Early-stage startup            Large org with multiple teams
Small team (<10 devs)          High scale requirements
Unclear domain boundaries      Well-understood bounded contexts
Simple operational needs       Independent release cadence needed
Proof of concept               Different scaling needs per component

Migration path:
Monolith ’ Strangler Fig Pattern ’ Microservices
  1. Identify bounded context (e.g., Payment)
  2. Wrap it behind an interface
  3. Extract to separate service behind API Gateway
  4. Route requests to new service
  5. Remove from monolith
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle security in microservices?

```cs
// 1. JWT authentication — validate in each service
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(opts =>
    {
        opts.Authority = "https://identity-server"; // OIDC discovery
        opts.Audience  = "order-service";
        opts.TokenValidationParameters = new()
        {
            ValidateIssuer   = true,
            ValidateAudience = true,
            ValidateLifetime = true,
        };
    });

builder.Services.AddAuthorization(opts =>
    opts.AddPolicy("orders:write", p => p.RequireClaim("scope", "orders:write")));

app.UseAuthentication();
app.UseAuthorization();

app.MapPost("/orders", CreateOrder).RequireAuthorization("orders:write");

// 2. Forward JWT between services (propagate identity)
builder.Services.AddHttpClient<ICatalogClient, CatalogClient>()
    .AddHttpMessageHandler<JwtForwardingHandler>();

public class JwtForwardingHandler(IHttpContextAccessor accessor) : DelegatingHandler
{
    protected override Task<HttpResponseMessage> SendAsync(
        HttpRequestMessage request, CancellationToken ct)
    {
        var token = accessor.HttpContext?.Request.Headers.Authorization.ToString();
        if (!string.IsNullOrEmpty(token))
            request.Headers.Authorization =
                System.Net.Http.Headers.AuthenticationHeaderValue.Parse(token);
        return base.SendAsync(request, ct);
    }
}

// 3. mTLS — mutual TLS for service-to-service (via service mesh: Istio / Linkerd)
// Zero-code change; sidecar proxy handles certificate verification

// 4. Secrets management — never store secrets in code
// Kubernetes secrets
var connStr = builder.Configuration["ConnectionStrings__Orders"]; // from K8s Secret
// Azure Key Vault
builder.Configuration.AddAzureKeyVault(new Uri("https://myvault.vault.azure.net/"),
    new DefaultAzureCredential());
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the role of Docker in microservices?

Docker packages each microservice and its dependencies into an **image** — a portable, reproducible unit that runs consistently everywhere.

```dockerfile
# Each microservice has its own Dockerfile
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src
COPY . .
RUN dotnet publish -c Release -o /app

FROM mcr.microsoft.com/dotnet/aspnet:10.0
WORKDIR /app
COPY --from=build /app .
EXPOSE 8080
ENTRYPOINT ["dotnet", "CatalogService.dll"]
```

```yaml
# docker-compose.yml — run all services locally
version: "3.9"
services:
  api-gateway:
    build: ./ApiGateway
    ports: ["5000:8080"]
    depends_on: [order-service, catalog-service]

  order-service:
    build: ./OrderService
    environment:
      - ConnectionStrings__Orders=Host=postgres;Database=orders;Username=app;Password=secret
    depends_on: [postgres, rabbitmq]

  catalog-service:
    build: ./CatalogService
    depends_on: [postgres]

  postgres:
    image: postgres:16-alpine
    volumes: ["pgdata:/var/lib/postgresql/data"]
    environment:
      POSTGRES_PASSWORD: secret

  rabbitmq:
    image: rabbitmq:3-management
    ports: ["15672:15672"]

volumes:
  pgdata:
```

```bash
# Build and run all services
docker compose up --build

# Build a single image
docker build -t myregistry.azurecr.io/order-service:1.0.0 ./OrderService

# Push to registry
docker push myregistry.azurecr.io/order-service:1.0.0

# Run a single service
docker run -p 8080:8080 myregistry.azurecr.io/order-service:1.0.0
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement resilience and fault tolerance in microservices?

```cs
// Microsoft.Extensions.Http.Resilience (.NET 8+ / Polly v8)
// dotnet add package Microsoft.Extensions.Http.Resilience

builder.Services.AddHttpClient<ICatalogClient, CatalogClient>()
    .AddStandardResilienceHandler(opts =>
    {
        // Retry — 3 times with exponential backoff + jitter
        opts.Retry.MaxRetryAttempts = 3;
        opts.Retry.Delay            = TimeSpan.FromMilliseconds(200);
        opts.Retry.BackoffType      = DelayBackoffType.Exponential;
        opts.Retry.UseJitter        = true;
        opts.Retry.ShouldHandle     = args =>
            ValueTask.FromResult(args.Outcome.Exception is HttpRequestException);

        // Circuit Breaker — open after 50% failure in 10-second window
        opts.CircuitBreaker.FailureRatio    = 0.5;
        opts.CircuitBreaker.SamplingDuration = TimeSpan.FromSeconds(10);
        opts.CircuitBreaker.MinimumThroughput = 10;
        opts.CircuitBreaker.BreakDuration    = TimeSpan.FromSeconds(30);

        // Timeout per attempt
        opts.AttemptTimeout.Timeout = TimeSpan.FromSeconds(5);

        // Total timeout across all retries
        opts.TotalRequestTimeout.Timeout = TimeSpan.FromSeconds(30);
    });

// Fallback / graceful degradation
builder.Services.AddResiliencePipeline("catalog-fallback", builder =>
{
    builder.AddFallback(new FallbackStrategyOptions<Product?>
    {
        FallbackAction = _ => ValueTask.FromResult<Product?>(Product.Default),
        ShouldHandle   = args => ValueTask.FromResult(
            args.Outcome.Exception is BrokenCircuitException),
    });
});

// Bulkhead — limit concurrent requests to a service
builder.Services.AddResiliencePipeline("bulkhead", b =>
    b.AddConcurrencyLimiter(permitLimit: 10, queueLimit: 20));

// Health checks for circuit breaker status
builder.Services.AddHealthChecks()
    .AddCheck("catalog-circuit-breaker", () =>
        circuitBreakerState == CircuitState.Closed
            ? HealthCheckResult.Healthy()
            : HealthCheckResult.Degraded("Circuit breaker is open"));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are some best practices for designing microservices?

| Practice | Description |
|----------|-------------|
| **Design around business domains** | Use Domain-Driven Design bounded contexts |
| **Single responsibility** | Each service does one thing well |
| **Own your data** | No shared databases between services |
| **API-first** | Define contracts (OpenAPI/gRPC) before implementation |
| **Async by default** | Prefer events over synchronous calls |
| **Design for failure** | Retry, circuit breaker, fallback everywhere |
| **Health checks** | Liveness + readiness probes |
| **Structured logging** | Correlation IDs, JSON logs |
| **Distributed tracing** | OpenTelemetry propagation |
| **Versioned APIs** | Never break consumers |
| **Small, frequent releases** | CI/CD per service |
| **Automate everything** | Docker + Kubernetes + GitOps |

```cs
// Checklist for a new microservice:

// … 1. Health endpoints
app.MapHealthChecks("/health/live");
app.MapHealthChecks("/health/ready");

// … 2. Structured logging with correlation
builder.Host.UseSerilog((ctx, cfg) => cfg
    .Enrich.FromLogContext()
    .WriteTo.Console(new JsonFormatter()));

// … 3. OpenTelemetry tracing
builder.Services.AddOpenTelemetry()
    .WithTracing(t => t.AddAspNetCoreInstrumentation().AddOtlpExporter());

// … 4. Resilient HTTP clients
builder.Services.AddHttpClient<IDownstreamClient, DownstreamClient>()
    .AddStandardResilienceHandler();

// … 5. Versioned API
app.MapGroup("/api/v1").MapOrderEndpoints();

// … 6. Graceful shutdown
app.Lifetime.ApplicationStopping.Register(() =>
    logger.LogInformation("Shutting down gracefully..."));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Name the key components of Microservices?

```
Key Components of a Microservices System:

—————————————————————————————————————————————————————————————————————
  CLIENT (Browser / Mobile / 3rd-party)                              
————————————————————————————————————————————————————————————————————
                       
              ——————–—————————
                 API Gateway      Routing, Auth, Rate Limit, SSL
                (YARP / Ocelot) 
              —————————————————
          ————————————————————————
    ———–————— ——–———— ——–——————
      Order     Catalog    Payment       Individual Services
      Service    Service   Service   
    —————————— ———————— ——————————
                                 
    ———–——   ———–——  ——–———
     Orders    Products  Payments    Per-service Databases
      DB          DB        DB   
    ————————   ————————  ————————
                                 
          ———————————————————————
                  ——–—————
                   Message      Async Communication (RabbitMQ / Kafka)
                     Bus    
                  ——————————
                       
         ————————————————————————————
   ———–——————              —————–——————
    Notification               Audit / Log     Event Consumers
     Service                   Service    
   ————————————              —————————————
```

| Component | Role |
|-----------|------|
| **API Gateway** | Single entry point — routing, auth, rate limiting |
| **Services** | Independent business capabilities |
| **Service Registry** | Service discovery (Consul, K8s DNS) |
| **Message Bus** | Async communication (RabbitMQ, Kafka, Azure Service Bus) |
| **Configuration Server** | Centralised config (Azure App Config, Consul KV) |
| **Identity Provider** | Authentication/authorisation (IdentityServer, Azure AD B2C) |
| **Container Runtime** | Docker for packaging, Kubernetes for orchestration |
| **Observability** | Logs (Serilog), Metrics (Prometheus), Traces (Jaeger) |
| **CI/CD Pipeline** | Per-service build and deploy (GitHub Actions, Azure DevOps) |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the tools commonly used tools for Microservices?

| Category | Tool | Purpose |
|----------|------|---------|
| **Service framework** | ASP.NET Core Minimal API | Build HTTP microservices |
| **API Gateway** | YARP, Ocelot, Azure APIM | Routing, auth, rate limiting |
| **gRPC** | Grpc.AspNetCore | High-performance service-to-service |
| **Messaging** | MassTransit + RabbitMQ | Async pub/sub, saga orchestration |
| **Messaging (cloud)** | Azure Service Bus, Amazon SQS | Managed message queues |
| **Streaming** | Apache Kafka, Azure Event Hubs | High-throughput event streaming |
| **Containers** | Docker, containerd | Package and run services |
| **Orchestration** | Kubernetes, Azure AKS | Scale, deploy, manage containers |
| **Service mesh** | Istio, Linkerd | mTLS, traffic management, observability |
| **Service discovery** | K8s DNS, Consul | Locate services dynamically |
| **Configuration** | Azure App Config, Consul KV | Centralised config + feature flags |
| **Identity** | IdentityServer, Azure AD B2C | OAuth2/OIDC for authentication |
| **Resilience** | Polly / M.E.Http.Resilience | Retry, circuit breaker, timeout |
| **Tracing** | OpenTelemetry + Jaeger/Zipkin | Distributed request tracing |
| **Metrics** | Prometheus + Grafana | Dashboards and alerting |
| **Logging** | Serilog + ELK / Azure Monitor | Structured log aggregation |
| **CI/CD** | GitHub Actions, Azure DevOps | Automated build and deploy |
| **Secrets** | Azure Key Vault, HashiCorp Vault | Secrets management |
| **Health** | ASP.NET Core Health Checks | Liveness/readiness probes |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the key principles to follow when designing microservices?

```cs
// 1. Single Responsibility — one service, one bounded context
// OrderService handles order lifecycle only; Catalog handles product info

// 2. Database per service — no shared DB
//  Shared DB creates coupling
// … Each service owns its schema; communicate via events/API

// 3. Design for failure
builder.Services.AddHttpClient<ICatalogClient, CatalogClient>()
    .AddStandardResilienceHandler(); // retry + circuit breaker + timeout

// 4. Async-first communication
// Prefer events over synchronous calls for non-critical paths
await publishEndpoint.Publish(new OrderShipped(orderId, trackingNumber));

// 5. API versioning — never break consumers
var v1 = app.MapGroup("/api/v1");
var v2 = app.MapGroup("/api/v2");
v1.MapGet("/orders/{id}", GetOrderV1);
v2.MapGet("/orders/{id}", GetOrderV2); // new shape, v1 still works

// 6. Observability from day one
builder.Services.AddOpenTelemetry()
    .WithTracing(t => t.AddAspNetCoreInstrumentation().AddOtlpExporter())
    .WithMetrics(m => m.AddAspNetCoreInstrumentation().AddOtlpExporter());

builder.Host.UseSerilog((ctx, cfg) => cfg
    .Enrich.FromLogContext()
    .WriteTo.Console(new JsonFormatter()));

// 7. Idempotent consumers — safe to process same message twice
public class OrderCreatedConsumer : IConsumer<OrderCreated>
{
    public async Task Consume(ConsumeContext<OrderCreated> ctx)
    {
        // Check if already processed (idempotency key)
        if (await db.ProcessedEvents.AnyAsync(e => e.Id == ctx.MessageId))
            return; // duplicate — skip

        // ... process ...

        db.ProcessedEvents.Add(new ProcessedEvent(ctx.MessageId!.Value));
        await db.SaveChangesAsync();
    }
}

// 8. Health checks for Kubernetes
app.MapHealthChecks("/health/live",  new() { Predicate = _ => false }); // always alive
app.MapHealthChecks("/health/ready", new() { Predicate = _ => true  }); // checks deps

// 9. Use semantic versioning for Docker images + chart versions
// v1.2.3 — never use :latest in production

// 10. 12-Factor App principles
// Config from environment; logs to stdout; stateless processes
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement gRPC in .NET Core for service-to-service communication?

**gRPC** is a high-performance RPC framework using Protocol Buffers (protobuf) for serialization. It provides strongly-typed contracts, bi-directional streaming, and is significantly faster than REST for internal service calls.

```bash
# Create gRPC server
dotnet new grpc -n OrderGrpcService
cd OrderGrpcService
dotnet add package Grpc.AspNetCore
```

```proto
// Protos/order.proto — shared contract (copy to both projects or use NuGet)
syntax = "proto3";
option csharp_namespace = "OrderGrpcService";

package order;

service OrderService {
  rpc CreateOrder (CreateOrderRequest) returns (CreateOrderReply);
  rpc GetOrder    (GetOrderRequest)    returns (OrderReply);
  rpc StreamOrders(StreamRequest)      returns (stream OrderReply); // server streaming
}

message CreateOrderRequest {
  string customer_id = 1;
  repeated OrderItem items = 2;
}
message CreateOrderReply { string order_id = 1; }
message GetOrderRequest  { string order_id = 1; }
message OrderReply       { string order_id = 1; string status = 2; double total = 3; }
message OrderItem        { string product_id = 1; int32 quantity = 2; }
message StreamRequest    { string customer_id = 1; }
```

```xml
<!-- Server .csproj — auto-generates C# from proto -->
<ItemGroup>
  <Protobuf Include="Protos\order.proto" GrpcServices="Server" />
</ItemGroup>
```

```cs
//  gRPC SERVER ——————————————————————————————————————————————————————
// Services/OrderGrpcService.cs
using Grpc.Core;
using OrderGrpcService;

public class OrderGrpcServiceImpl(IOrderRepository repo) : OrderService.OrderServiceBase
{
    public override async Task<CreateOrderReply> CreateOrder(
        CreateOrderRequest request, ServerCallContext ctx)
    {
        var order = await repo.CreateAsync(request.CustomerId,
            request.Items.Select(i => (i.ProductId, i.Quantity)).ToList(),
            ctx.CancellationToken);

        return new CreateOrderReply { OrderId = order.Id.ToString() };
    }

    public override async Task<OrderReply> GetOrder(
        GetOrderRequest request, ServerCallContext ctx)
    {
        var order = await repo.GetAsync(Guid.Parse(request.OrderId), ctx.CancellationToken)
            ?? throw new RpcException(new Status(StatusCode.NotFound, "Order not found"));

        return new OrderReply
        {
            OrderId = order.Id.ToString(),
            Status  = order.Status.ToString(),
            Total   = (double)order.Total
        };
    }

    // Server-side streaming — push multiple responses
    public override async Task StreamOrders(
        StreamRequest request,
        IServerStreamWriter<OrderReply> stream,
        ServerCallContext ctx)
    {
        await foreach (var order in repo.GetByCustomerAsync(request.CustomerId, ctx.CancellationToken))
        {
            await stream.WriteAsync(new OrderReply
            {
                OrderId = order.Id.ToString(),
                Status  = order.Status.ToString(),
                Total   = (double)order.Total
            });
        }
    }
}

// Program.cs (server)
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddGrpc(opt =>
{
    opt.EnableDetailedErrors = builder.Environment.IsDevelopment();
    opt.MaxReceiveMessageSize = 4 * 1024 * 1024; // 4 MB
});
builder.Services.AddScoped<IOrderRepository, OrderRepository>();

var app = builder.Build();
app.MapGrpcService<OrderGrpcServiceImpl>();
app.MapGet("/", () => "gRPC server. Use a gRPC client to communicate.");
app.Run();
```

```xml
<!-- Client .csproj -->
<ItemGroup>
  <PackageReference Include="Grpc.Net.ClientFactory" Version="2.*" />
  <PackageReference Include="Google.Protobuf"         Version="3.*" />
  <PackageReference Include="Grpc.Tools"              Version="2.*" PrivateAssets="All" />
  <Protobuf Include="Protos\order.proto" GrpcServices="Client" />
</ItemGroup>
```

```cs
//  gRPC CLIENT ——————————————————————————————————————————————————————
// Program.cs (consumer service)
builder.Services.AddGrpcClient<OrderService.OrderServiceClient>(o =>
{
    o.Address = new Uri(builder.Configuration["Services:OrderGrpc"]!);
})
.ConfigurePrimaryHttpMessageHandler(() => new SocketsHttpHandler
{
    PooledConnectionIdleTimeout = TimeSpan.FromMinutes(5),
    KeepAlivePingDelay          = TimeSpan.FromSeconds(60),
    KeepAlivePingTimeout        = TimeSpan.FromSeconds(30),
    EnableMultipleHttp2Connections = true
})
.AddStandardResilienceHandler(); // retry + circuit breaker

// Usage in a controller / service
public class CheckoutService(OrderService.OrderServiceClient grpcClient)
{
    public async Task<string> PlaceOrderAsync(string customerId, List<(string, int)> items)
    {
        var request = new CreateOrderRequest { CustomerId = customerId };
        request.Items.AddRange(items.Select(i =>
            new OrderItem { ProductId = i.Item1, Quantity = i.Item2 }));

        var reply = await grpcClient.CreateOrderAsync(request);
        return reply.OrderId;
    }

    // Consume server-side stream
    public async IAsyncEnumerable<OrderReply> StreamCustomerOrdersAsync(string customerId)
    {
        using var stream = grpcClient.StreamOrders(new StreamRequest { CustomerId = customerId });
        await foreach (var order in stream.ResponseStream.ReadAllAsync())
            yield return order;
    }
}
```

**gRPC vs REST:**
| | gRPC | REST |
|--|------|------|
| **Protocol** | HTTP/2 + protobuf | HTTP/1.1 + JSON |
| **Performance** | ~7–10— faster | Baseline |
| **Streaming** | Client/server/bidirectional | Limited (SSE) |
| **Contract** | Strongly typed `.proto` | OpenAPI/Swagger |
| **Browser support** | Limited (needs gRPC-Web) | Universal |
| **Best for** | Internal service mesh | Public APIs |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use message brokers with MassTransit and RabbitMQ in .NET?

**MassTransit** is an open-source service bus abstraction for .NET that supports RabbitMQ, Azure Service Bus, Kafka, and more. It provides publish/subscribe, request/reply, and saga patterns.

```bash
dotnet add package MassTransit.RabbitMQ
dotnet add package MassTransit.EntityFrameworkCore  # for saga persistence
```

```cs
//  1. DEFINE MESSAGES (contracts — shared library) —————————————————
namespace Contracts;

// Events (past tense — something happened)
public record OrderPlaced(Guid OrderId, string CustomerId, decimal Total, DateTimeOffset PlacedAt);
public record OrderShipped(Guid OrderId, string TrackingNumber, DateTimeOffset ShippedAt);
public record PaymentProcessed(Guid OrderId, bool Success, string? FailureReason);

// Commands (imperative — do something)
public record ProcessPayment(Guid OrderId, decimal Amount, string PaymentToken);
public record SendOrderConfirmation(Guid OrderId, string CustomerEmail);

//  2. PRODUCER — PUBLISH EVENT —————————————————————————————————————
public class OrderService(IPublishEndpoint publishEndpoint, AppDbContext db)
{
    public async Task<Order> PlaceOrderAsync(PlaceOrderRequest req, CancellationToken ct)
    {
        var order = new Order(req.CustomerId, req.Items);
        db.Orders.Add(order);
        await db.SaveChangesAsync(ct);

        // Publish event — all subscribers receive it
        await publishEndpoint.Publish(
            new OrderPlaced(order.Id, order.CustomerId, order.Total, DateTimeOffset.UtcNow),
            ct);

        return order;
    }
}

//  3. CONSUMER — HANDLE EVENT ——————————————————————————————————————
public class OrderPlacedConsumer(IEmailService email, ILogger<OrderPlacedConsumer> logger)
    : IConsumer<OrderPlaced>
{
    public async Task Consume(ConsumeContext<OrderPlaced> context)
    {
        var evt = context.Message;
        logger.LogInformation("Processing OrderPlaced {OrderId}", evt.OrderId);

        // Send confirmation email
        await email.SendOrderConfirmationAsync(evt.CustomerId, evt.OrderId);

        // Optionally respond (for request/reply pattern)
        // await context.RespondAsync(new OrderConfirmationSent(evt.OrderId));
    }
}

// Payment consumer with retry/error handling
public class ProcessPaymentConsumer : IConsumer<ProcessPayment>
{
    public async Task Consume(ConsumeContext<ProcessPayment> context)
    {
        var cmd = context.Message;
        try
        {
            var result = await ProcessPaymentInternalAsync(cmd);
            await context.Publish(new PaymentProcessed(cmd.OrderId, result.Success, null));
        }
        catch (PaymentGatewayException ex)
        {
            // Throw to trigger MassTransit retry policy
            throw new Exception($"Payment gateway error: {ex.Message}", ex);
        }
    }

    private Task<PaymentResult> ProcessPaymentInternalAsync(ProcessPayment cmd)
        => Task.FromResult(new PaymentResult(true)); // stub
}

record PaymentResult(bool Success);

//  4. SAGA — COORDINATE LONG-RUNNING WORKFLOW —————————————————————
public class OrderStateMachine : MassTransitStateMachine<OrderSagaState>
{
    public State Placed    { get; private set; } = null!;
    public State Paid      { get; private set; } = null!;
    public State Shipped   { get; private set; } = null!;

    public Event<OrderPlaced>       OrderPlaced       { get; private set; } = null!;
    public Event<PaymentProcessed>  PaymentProcessed  { get; private set; } = null!;
    public Event<OrderShipped>      OrderShipped      { get; private set; } = null!;

    public OrderStateMachine()
    {
        InstanceState(x => x.CurrentState);

        Event(() => OrderPlaced,      e => e.CorrelateById(m => m.Message.OrderId));
        Event(() => PaymentProcessed, e => e.CorrelateById(m => m.Message.OrderId));
        Event(() => OrderShipped,     e => e.CorrelateById(m => m.Message.OrderId));

        Initially(
            When(OrderPlaced)
                .Then(ctx => ctx.Saga.CustomerId = ctx.Message.CustomerId)
                .Publish(ctx => new ProcessPayment(ctx.Saga.CorrelationId, ctx.Message.Total, "token"))
                .TransitionTo(Placed));

        During(Placed,
            When(PaymentProcessed, ctx => ctx.Message.Success)
                .TransitionTo(Paid),
            When(PaymentProcessed, ctx => !ctx.Message.Success)
                .Finalize()); // failed — end saga

        During(Paid,
            When(OrderShipped)
                .TransitionTo(Shipped)
                .Finalize());
    }
}

public class OrderSagaState : SagaStateMachineInstance
{
    public Guid   CorrelationId { get; set; }
    public string CurrentState  { get; set; } = null!;
    public string CustomerId    { get; set; } = null!;
}

//  5. REGISTRATION —————————————————————————————————————————————————
builder.Services.AddMassTransit(x =>
{
    x.AddConsumer<OrderPlacedConsumer>();
    x.AddConsumer<ProcessPaymentConsumer>();
    x.AddSagaStateMachine<OrderStateMachine, OrderSagaState>()
        .EntityFrameworkRepository(r =>
        {
            r.ExistingDbContext<AppDbContext>();
            r.UsePostgres();
        });

    x.UsingRabbitMq((ctx, cfg) =>
    {
        cfg.Host("rabbitmq://localhost", h =>
        {
            h.Username("guest");
            h.Password("guest");
        });

        // Retry policy — exponential backoff, 3 attempts
        cfg.UseMessageRetry(r => r.Exponential(3,
            TimeSpan.FromSeconds(1),
            TimeSpan.FromSeconds(10),
            TimeSpan.FromSeconds(2)));

        // Dead-letter queue after exhausted retries
        cfg.UseDelayedRedelivery(r => r.Intervals(
            TimeSpan.FromMinutes(5),
            TimeSpan.FromMinutes(15),
            TimeSpan.FromHours(1)));

        cfg.ConfigureEndpoints(ctx); // auto-configure queues from registered consumers
    });
});
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are distributed patterns in microservices (Outbox, Circuit Breaker, Idempotency)?

**Distributed patterns** address the fundamental challenges of reliability and consistency when services communicate over a network.

```cs
//  1. OUTBOX PATTERN — guaranteed event delivery ———————————————————
// Problem: DB save and message publish can fail independently ’ lost messages
// Solution: Write event to outbox table in SAME transaction as domain changes

// Outbox message entity
public class OutboxMessage
{
    public Guid     Id           { get; init; } = Guid.NewGuid();
    public string   Type         { get; init; } = null!; // full type name
    public string   Payload      { get; init; } = null!; // JSON
    public DateTime CreatedAt    { get; init; } = DateTime.UtcNow;
    public DateTime? ProcessedAt { get; set; }
}

// Service — writes domain change + outbox in same transaction
public class OrderService(AppDbContext db)
{
    public async Task PlaceOrderAsync(PlaceOrderRequest req, CancellationToken ct)
    {
        var order = new Order(req.CustomerId, req.Items);
        var evt   = new OrderPlaced(order.Id, order.CustomerId, order.Total, DateTimeOffset.UtcNow);

        db.Orders.Add(order);
        db.OutboxMessages.Add(new OutboxMessage
        {
            Type    = typeof(OrderPlaced).FullName!,
            Payload = JsonSerializer.Serialize(evt)
        });

        await db.SaveChangesAsync(ct); // atomic — either both succeed or both fail
    }
}

// Background processor — reads outbox and publishes to broker
public class OutboxProcessor(AppDbContext db, IPublishEndpoint bus) : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken ct)
    {
        while (!ct.IsCancellationRequested)
        {
            var messages = await db.OutboxMessages
                .Where(m => m.ProcessedAt == null)
                .OrderBy(m => m.CreatedAt)
                .Take(50)
                .ToListAsync(ct);

            foreach (var msg in messages)
            {
                var type    = Type.GetType(msg.Type)!;
                var payload = JsonSerializer.Deserialize(msg.Payload, type)!;
                await bus.Publish(payload, type, ct);
                msg.ProcessedAt = DateTime.UtcNow;
            }

            await db.SaveChangesAsync(ct);
            await Task.Delay(TimeSpan.FromSeconds(5), ct);
        }
    }
}

//  2. CIRCUIT BREAKER — stop cascading failures ————————————————————
// Using Microsoft.Extensions.Http.Resilience (.NET 8+)
builder.Services.AddHttpClient<ICatalogClient, CatalogClient>()
    .AddResilienceHandler("catalog-pipeline", p =>
    {
        // Retry: 3 attempts, exponential backoff
        p.AddRetry(new HttpRetryStrategyOptions
        {
            MaxRetryAttempts = 3,
            BackoffType      = DelayBackoffType.Exponential,
            UseJitter        = true,
            Delay            = TimeSpan.FromMilliseconds(500)
        });

        // Circuit breaker: open after 50% failure rate over 10-second sampling
        p.AddCircuitBreaker(new HttpCircuitBreakerStrategyOptions
        {
            SamplingDuration          = TimeSpan.FromSeconds(10),
            FailureRatio              = 0.5,
            MinimumThroughput         = 5,
            BreakDuration             = TimeSpan.FromSeconds(30)
        });

        // Total timeout per full request (including retries)
        p.AddTimeout(TimeSpan.FromSeconds(10));
    });

//  3. IDEMPOTENCY KEY — safe retries ———————————————————————————————
// Ensure duplicate requests produce the same result
public class IdempotentOrderService(AppDbContext db)
{
    public async Task<OrderResult> PlaceOrderAsync(
        PlaceOrderRequest req,
        Guid idempotencyKey, // client-generated key
        CancellationToken ct)
    {
        // Check if already processed
        var existing = await db.IdempotencyRecords
            .FirstOrDefaultAsync(r => r.Key == idempotencyKey, ct);
        if (existing != null)
            return JsonSerializer.Deserialize<OrderResult>(existing.Response)!;

        var order = new Order(req.CustomerId, req.Items);
        db.Orders.Add(order);

        var result = new OrderResult(order.Id, order.Status.ToString());
        db.IdempotencyRecords.Add(new IdempotencyRecord
        {
            Key      = idempotencyKey,
            Response = JsonSerializer.Serialize(result),
            ExpiresAt = DateTime.UtcNow.AddDays(1)
        });

        await db.SaveChangesAsync(ct);
        return result;
    }
}

//  4. CORRELATION ID — trace requests across services ——————————————
public class CorrelationIdMiddleware(RequestDelegate next)
{
    public async Task InvokeAsync(HttpContext ctx)
    {
        const string header = "X-Correlation-Id";
        if (!ctx.Request.Headers.TryGetValue(header, out var correlationId))
            correlationId = Guid.NewGuid().ToString();

        ctx.Response.Headers[header] = correlationId;

        using (Serilog.Context.LogContext.PushProperty("CorrelationId", (string)correlationId!))
            await next(ctx);
    }
}

// Registration
app.UseMiddleware<CorrelationIdMiddleware>();

// Propagate to downstream HTTP calls
builder.Services.AddHttpClient<ICatalogClient, CatalogClient>()
    .AddHttpMessageHandler<CorrelationIdPropagationHandler>();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 18. ARCHITECTURE AND DESIGN PATTERNS

<br>

## Q. What is Clean Architecture and how do you implement it in .NET?

**Clean Architecture** (Robert C. Martin) organizes code into concentric layers where inner layers define abstractions and outer layers provide implementations. Dependencies always point inward.

```
—————————————————————————————————————————————————
  Infrastructure  (EF Core, HTTP, Serilog, etc.) 
  ———————————————————————————————————————————  
    Application  (use cases, CQRS handlers)    
    —————————————————————————————————————    
      Domain  (entities, value objects,      
      domain events, business rules)         
    —————————————————————————————————————    
  ———————————————————————————————————————————  
  Presentation  (API Controllers / Minimal API)  
—————————————————————————————————————————————————
         Dependencies flow INWARD only ’
```

```
MyApp.sln
 src/
    MyApp.Domain/           # No external dependencies
       Entities/
       ValueObjects/
       Enums/
       Events/
       Exceptions/
    MyApp.Application/      # Depends only on Domain
       Interfaces/         # IOrderRepository, IEmailService
       Commands/
       Queries/
       DTOs/
       Behaviors/          # MediatR pipeline behaviors
    MyApp.Infrastructure/   # Implements Application interfaces
       Persistence/        # EF Core, repositories
       Messaging/          # RabbitMQ, SendGrid
       Identity/
    MyApp.Api/              # ASP.NET Core host
        Controllers/
        Middleware/
        Program.cs
 tests/
     MyApp.Domain.Tests/
     MyApp.Application.Tests/
     MyApp.Api.Tests/
```

```cs
//  Domain Layer — pure business logic, no framework dependencies ———
namespace MyApp.Domain.Entities;

public sealed class Order : AggregateRoot
{
    private readonly List<OrderLine> _lines = [];

    public Guid       Id         { get; private set; }
    public string     CustomerId { get; private set; } = null!;
    public OrderStatus Status    { get; private set; }
    public decimal    Total      => _lines.Sum(l => l.Total);
    public IReadOnlyList<OrderLine> Lines => _lines.AsReadOnly();

    private Order() { } // EF Core constructor

    public static Order Create(string customerId, IEnumerable<OrderLine> lines)
    {
        if (string.IsNullOrWhiteSpace(customerId))
            throw new DomainException("CustomerId is required.");

        var order = new Order
        {
            Id         = Guid.NewGuid(),
            CustomerId = customerId,
            Status     = OrderStatus.Pending
        };
        order._lines.AddRange(lines);
        order.AddDomainEvent(new OrderCreatedEvent(order.Id, customerId));
        return order;
    }

    public void Ship(string trackingNumber)
    {
        if (Status != OrderStatus.Paid)
            throw new DomainException("Order must be paid before shipping.");
        Status = OrderStatus.Shipped;
        AddDomainEvent(new OrderShippedEvent(Id, trackingNumber));
    }
}

//  Application Layer — use case orchestration —————————————————————
namespace MyApp.Application.Interfaces;

public interface IOrderRepository
{
    Task<Order?> GetByIdAsync(Guid id, CancellationToken ct = default);
    Task AddAsync(Order order, CancellationToken ct = default);
    Task<IReadOnlyList<Order>> GetByCustomerAsync(string customerId, CancellationToken ct = default);
}

//  Infrastructure Layer — concrete implementations —————————————————
namespace MyApp.Infrastructure.Persistence;

public class OrderRepository(AppDbContext db) : IOrderRepository
{
    public Task<Order?> GetByIdAsync(Guid id, CancellationToken ct)
        => db.Orders
             .Include(o => o.Lines)
             .FirstOrDefaultAsync(o => o.Id == id, ct);

    public async Task AddAsync(Order order, CancellationToken ct)
    {
        db.Orders.Add(order);
        await db.SaveChangesAsync(ct);
    }

    public Task<IReadOnlyList<Order>> GetByCustomerAsync(string customerId, CancellationToken ct)
        => db.Orders
             .Where(o => o.CustomerId == customerId)
             .ToListAsync(ct)
             .ContinueWith(t => (IReadOnlyList<Order>)t.Result, ct);
}

//  Presentation Layer — thin controllers, delegate to application ———
[ApiController, Route("api/orders")]
public class OrdersController(ISender mediator) : ControllerBase
{
    [HttpPost]
    public async Task<IActionResult> Create(
        [FromBody] CreateOrderCommand cmd, CancellationToken ct)
    {
        var orderId = await mediator.Send(cmd, ct);
        return CreatedAtAction(nameof(GetById), new { id = orderId }, new { Id = orderId });
    }

    [HttpGet("{id:guid}")]
    public async Task<IActionResult> GetById(Guid id, CancellationToken ct)
    {
        var order = await mediator.Send(new GetOrderQuery(id), ct);
        return order is null ? NotFound() : Ok(order);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is CQRS and how do you implement it with MediatR in .NET?

**CQRS (Command Query Responsibility Segregation)** separates read operations (queries) from write operations (commands). MediatR provides an in-process mediator for dispatching commands and queries.

```bash
dotnet add package MediatR
dotnet add package FluentValidation.DependencyInjectionExtensions
```

```cs
//  1. COMMANDS — change state, return minimal result ———————————————
// Command DTO
public sealed record CreateOrderCommand(
    string CustomerId,
    IReadOnlyList<OrderLineDto> Lines) : IRequest<Guid>;

public sealed record OrderLineDto(string ProductId, int Quantity, decimal UnitPrice);

// Command Handler
public sealed class CreateOrderHandler(
    IOrderRepository repository,
    IPublishEndpoint  publishEndpoint,
    ILogger<CreateOrderHandler> logger) : IRequestHandler<CreateOrderCommand, Guid>
{
    public async Task<Guid> Handle(CreateOrderCommand cmd, CancellationToken ct)
    {
        logger.LogInformation("Creating order for customer {CustomerId}", cmd.CustomerId);

        var lines = cmd.Lines.Select(l =>
            OrderLine.Create(l.ProductId, l.Quantity, Money.Of(l.UnitPrice, "GBP")));

        var order = Order.Create(cmd.CustomerId, lines);
        await repository.AddAsync(order, ct);

        return order.Id;
    }
}

//  2. QUERIES — read state, never mutate ———————————————————————————
// Query DTO
public sealed record GetOrderQuery(Guid OrderId) : IRequest<OrderDetailDto?>;

public sealed record OrderDetailDto(
    Guid     OrderId,
    string   CustomerId,
    string   Status,
    decimal  Total,
    IReadOnlyList<OrderLineDetailDto> Lines);

public sealed record OrderLineDetailDto(string ProductId, int Quantity, decimal UnitPrice, decimal Total);

// Query Handler — can use read-optimized data access (dapper, projections)
public sealed class GetOrderHandler(AppDbContext db) : IRequestHandler<GetOrderQuery, OrderDetailDto?>
{
    public async Task<OrderDetailDto?> Handle(GetOrderQuery query, CancellationToken ct)
    {
        return await db.Orders
            .AsNoTracking()
            .Where(o => o.Id == query.OrderId)
            .Select(o => new OrderDetailDto(
                o.Id,
                o.CustomerId,
                o.Status.ToString(),
                o.Lines.Sum(l => l.Quantity * l.UnitPrice),
                o.Lines.Select(l => new OrderLineDetailDto(
                    l.ProductId, l.Quantity, l.UnitPrice, l.Quantity * l.UnitPrice))
                .ToList()))
            .FirstOrDefaultAsync(ct);
    }
}

//  3. PIPELINE BEHAVIORS — cross-cutting concerns ——————————————————
// Validation behavior — run FluentValidation before every command
public sealed class ValidationBehavior<TRequest, TResponse>(
    IEnumerable<IValidator<TRequest>> validators)
    : IPipelineBehavior<TRequest, TResponse>
    where TRequest : notnull
{
    public async Task<TResponse> Handle(
        TRequest request, RequestHandlerDelegate<TResponse> next, CancellationToken ct)
    {
        if (!validators.Any()) return await next();

        var context = new ValidationContext<TRequest>(request);
        var failures = validators
            .Select(v => v.Validate(context))
            .SelectMany(r => r.Errors)
            .Where(f => f is not null)
            .ToList();

        if (failures.Count != 0)
            throw new ValidationException(failures);

        return await next();
    }
}

// Logging behavior — log every request/response
public sealed class LoggingBehavior<TRequest, TResponse>(
    ILogger<LoggingBehavior<TRequest, TResponse>> logger)
    : IPipelineBehavior<TRequest, TResponse>
    where TRequest : notnull
{
    public async Task<TResponse> Handle(
        TRequest request, RequestHandlerDelegate<TResponse> next, CancellationToken ct)
    {
        var name = typeof(TRequest).Name;
        logger.LogInformation("Handling {Request}", name);
        var sw = System.Diagnostics.Stopwatch.StartNew();

        var response = await next();

        sw.Stop();
        logger.LogInformation("Handled {Request} in {Ms}ms", name, sw.ElapsedMilliseconds);
        return response;
    }
}

// FluentValidation for command
public sealed class CreateOrderCommandValidator : AbstractValidator<CreateOrderCommand>
{
    public CreateOrderCommandValidator()
    {
        RuleFor(c => c.CustomerId).NotEmpty().MaximumLength(100);
        RuleFor(c => c.Lines).NotEmpty().WithMessage("Order must have at least one line.");
        RuleForEach(c => c.Lines).ChildRules(line =>
        {
            line.RuleFor(l => l.ProductId).NotEmpty();
            line.RuleFor(l => l.Quantity).GreaterThan(0);
            line.RuleFor(l => l.UnitPrice).GreaterThanOrEqualTo(0);
        });
    }
}

//  4. REGISTRATION —————————————————————————————————————————————————
builder.Services.AddMediatR(cfg =>
{
    cfg.RegisterServicesFromAssemblyContaining<CreateOrderHandler>();
    cfg.AddBehavior(typeof(IPipelineBehavior<,>), typeof(ValidationBehavior<,>));
    cfg.AddBehavior(typeof(IPipelineBehavior<,>), typeof(LoggingBehavior<,>));
});
builder.Services.AddValidatorsFromAssemblyContaining<CreateOrderCommandValidator>();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Domain-Driven Design (DDD) and what are its core building blocks?

**DDD** (Eric Evans) is a software design approach that focuses on modeling complex business domains. The code structure mirrors the business language (Ubiquitous Language).

```cs
//  1. VALUE OBJECT — defined by its attributes, immutable ——————————
public sealed class Money : IEquatable<Money>
{
    public decimal Amount   { get; }
    public string  Currency { get; }

    private Money(decimal amount, string currency)
    {
        if (amount < 0)    throw new DomainException("Amount cannot be negative.");
        if (string.IsNullOrWhiteSpace(currency)) throw new DomainException("Currency required.");
        Amount   = amount;
        Currency = currency.ToUpperInvariant();
    }

    public static Money Of(decimal amount, string currency) => new(amount, currency);
    public static Money Zero(string currency) => new(0, currency);

    public Money Add(Money other)
    {
        if (Currency != other.Currency) throw new DomainException("Currency mismatch.");
        return new Money(Amount + other.Amount, Currency);
    }

    public bool Equals(Money? other) => other is not null
        && Amount == other.Amount && Currency == other.Currency;

    public override bool Equals(object? obj) => Equals(obj as Money);
    public override int GetHashCode() => HashCode.Combine(Amount, Currency);
    public override string ToString() => $"{Amount:F2} {Currency}";
}

//  2. ENTITY — defined by identity, mutable state ——————————————————
public abstract class Entity
{
    public Guid Id { get; protected set; } = Guid.NewGuid();

    private readonly List<IDomainEvent> _domainEvents = [];
    public IReadOnlyList<IDomainEvent> DomainEvents => _domainEvents.AsReadOnly();
    public void AddDomainEvent(IDomainEvent evt) => _domainEvents.Add(evt);
    public void ClearDomainEvents() => _domainEvents.Clear();
}

//  3. AGGREGATE ROOT — consistency boundary, only accessible entry —
public sealed class Order : Entity
{
    private readonly List<OrderLine> _lines = [];

    public string      CustomerId { get; private set; } = null!;
    public OrderStatus Status     { get; private set; }
    public Money       Total      => _lines.Aggregate(
        Money.Zero("GBP"), (acc, l) => acc.Add(l.Total));

    private Order() { }

    public static Order Create(string customerId)
    {
        var order = new Order { CustomerId = customerId, Status = OrderStatus.Pending };
        order.AddDomainEvent(new OrderCreatedEvent(order.Id, customerId, DateTime.UtcNow));
        return order;
    }

    public OrderLine AddLine(string productId, int quantity, Money unitPrice)
    {
        if (Status != OrderStatus.Pending)
            throw new DomainException("Cannot modify order that is not pending.");
        var line = new OrderLine(Id, productId, quantity, unitPrice);
        _lines.Add(line);
        return line;
    }

    public void Submit()
    {
        if (!_lines.Any()) throw new DomainException("Cannot submit empty order.");
        Status = OrderStatus.Submitted;
        AddDomainEvent(new OrderSubmittedEvent(Id, Total.Amount, DateTime.UtcNow));
    }
}

//  4. DOMAIN EVENTS — something significant happened ——————————————
public interface IDomainEvent { }
public sealed record OrderCreatedEvent(Guid OrderId, string CustomerId, DateTime OccurredAt) : IDomainEvent;
public sealed record OrderSubmittedEvent(Guid OrderId, decimal Total, DateTime OccurredAt) : IDomainEvent;

// Publish domain events after saving (via EF Core interceptor or unit of work)
public class DomainEventPublisher(IPublishEndpoint bus) : SaveChangesInterceptor
{
    public override async ValueTask<int> SavedChangesAsync(
        SaveChangesCompletedEventData data, int result, CancellationToken ct = default)
    {
        var aggregates = data.Context?.ChangeTracker.Entries<Entity>()
            .Select(e => e.Entity)
            .Where(e => e.DomainEvents.Any())
            .ToList() ?? [];

        foreach (var aggregate in aggregates)
        {
            foreach (var evt in aggregate.DomainEvents)
                await bus.Publish(evt, evt.GetType(), ct);
            aggregate.ClearDomainEvents();
        }

        return result;
    }
}

//  5. REPOSITORY — abstracts persistence for aggregates only ———————
public interface IOrderRepository
{
    Task<Order?> FindAsync(Guid id, CancellationToken ct = default);
    Task SaveAsync(Order order, CancellationToken ct = default);
}

//  6. DOMAIN SERVICE — logic that doesn\'t belong to a single entity 
public class PricingService(IProductRepository products)
{
    public async Task<Money> CalculateDiscountedPriceAsync(
        string productId, int quantity, string customerId, CancellationToken ct)
    {
        var product  = await products.FindAsync(productId, ct)
            ?? throw new DomainException("Product not found.");
        var basePrice = product.Price.Amount;
        decimal discount = quantity >= 10 ? 0.1m : quantity >= 5 ? 0.05m : 0m;
        return Money.Of(basePrice * quantity * (1 - discount), product.Price.Currency);
    }
}

//  7. BOUNDED CONTEXT MAP ——————————————————————————————————————————
/*
 ——————————————————         ——————————————————
   Order Context   ACL—–  Catalog Context  
   (Order, Line)              (Product, Stock) 
 ——————————————————         ——————————————————
         Domain Events
        –
 ——————————————————
  Shipping Context 
 ——————————————————

 ACL = Anti-Corruption Layer (translates between contexts)
*/
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are enterprise integration patterns and how do you implement them in .NET?

**Enterprise Integration Patterns (EIP)** (Hohpe & Woolf) provide a vocabulary for designing messaging systems. Key patterns: Message Channel, Message Router, Aggregator, Saga, and Dead Letter Queue.

```cs
//  1. MESSAGE ROUTER — route messages by content ———————————————————
public class OrderPriorityRouter(
    IMessageChannel standardQueue,
    IMessageChannel priorityQueue) : IConsumer<OrderPlaced>
{
    public Task Consume(ConsumeContext<OrderPlaced> ctx)
    {
        // Route high-value orders to priority processing
        var channel = ctx.Message.Total > 1000m ? priorityQueue : standardQueue;
        return channel.SendAsync(ctx.Message);
    }
}

//  2. AGGREGATOR — collect related messages, emit combined result ———
// Collect all items for an order, then process when complete
public class OrderAggregatorSaga : MassTransitStateMachine<OrderAggregatorState>
{
    public State Aggregating { get; private set; } = null!;
    public Event<OrderItemReceived> ItemReceived { get; private set; } = null!;
    public Schedule<OrderAggregatorState, AggregationTimeout> Timeout { get; private set; } = null!;

    public OrderAggregatorSaga()
    {
        InstanceState(x => x.CurrentState);
        Event(() => ItemReceived, e => e.CorrelateById(m => m.Message.OrderId));
        Schedule(() => Timeout, x => x.TimeoutToken, s =>
        {
            s.Delay  = TimeSpan.FromSeconds(30);
            s.Received = r => r.CorrelateById(m => m.Message.OrderId);
        });

        Initially(
            When(ItemReceived)
                .Then(ctx =>
                {
                    ctx.Saga.OrderId      = ctx.Message.OrderId;
                    ctx.Saga.ExpectedCount = ctx.Message.TotalItems;
                    ctx.Saga.Items.Add(ctx.Message.ItemId);
                })
                .Schedule(Timeout, ctx => new AggregationTimeout(ctx.Saga.CorrelationId))
                .TransitionTo(Aggregating));

        During(Aggregating,
            When(ItemReceived)
                .Then(ctx => ctx.Saga.Items.Add(ctx.Message.ItemId))
                .IfElse(
                    ctx => ctx.Saga.Items.Count >= ctx.Saga.ExpectedCount,
                    complete => complete
                        .Unschedule(Timeout)
                        .Publish(ctx => new AllOrderItemsReceived(ctx.Saga.OrderId, ctx.Saga.Items))
                        .Finalize(),
                    waiting => waiting.TransitionTo(Aggregating)),
            When(Timeout!.Received)
                .Publish(ctx => new OrderAggregationTimedOut(ctx.Saga.OrderId, ctx.Saga.Items))
                .Finalize());
    }
}

//  3. DEAD LETTER QUEUE — handle unprocessable messages ————————————
public class FaultConsumer<T> : IConsumer<Fault<T>> where T : class
{
    private readonly IDeadLetterStore _store;
    private readonly ILogger<FaultConsumer<T>> _logger;

    public FaultConsumer(IDeadLetterStore store, ILogger<FaultConsumer<T>> logger)
        => (_store, _logger) = (store, logger);

    public async Task Consume(ConsumeContext<Fault<T>> context)
    {
        var fault = context.Message;
        _logger.LogError("Message {MessageId} of type {Type} failed after {Retries} retries. Exceptions: {Errors}",
            fault.FaultedMessageId,
            typeof(T).Name,
            fault.RetryCount,
            string.Join("; ", fault.Exceptions.Select(e => e.Message)));

        await _store.StoreAsync(new DeadLetterMessage
        {
            MessageId  = fault.FaultedMessageId?.ToString(),
            MessageType = typeof(T).Name,
            Payload    = System.Text.Json.JsonSerializer.Serialize(fault.Message),
            Errors     = fault.Exceptions.Select(e => e.Message).ToArray(),
            FailedAt   = DateTime.UtcNow
        });
    }
}

// Register fault consumers
x.AddConsumer(typeof(FaultConsumer<OrderPlaced>));
x.AddConsumer(typeof(FaultConsumer<ProcessPayment>));

//  4. REQUEST-REPLY — synchronous over async messaging —————————————
// Requester
public class InventoryCheckService(IRequestClient<CheckInventory> client)
{
    public async Task<bool> IsAvailableAsync(string productId, int quantity, CancellationToken ct)
    {
        var response = await client.GetResponse<InventoryCheckResult>(
            new CheckInventory(productId, quantity), ct,
            timeout: RequestTimeout.After(s: 5));

        return response.Message.Available;
    }
}

// Responder
public class InventoryConsumer(IInventoryRepository repo) : IConsumer<CheckInventory>
{
    public async Task Consume(ConsumeContext<CheckInventory> ctx)
    {
        var stock = await repo.GetStockAsync(ctx.Message.ProductId);
        await ctx.RespondAsync(
            new InventoryCheckResult(ctx.Message.ProductId, stock >= ctx.Message.Quantity));
    }
}

record CheckInventory(string ProductId, int Quantity);
record InventoryCheckResult(string ProductId, bool Available);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 19. DEPLOYMENT

<br>

## Q. What is the `dotnet publish` command and how is it used?

`dotnet publish` compiles the application and copies the output (binaries, dependencies, assets) to a folder ready for deployment.

```bash
# Framework-dependent (requires runtime on target machine — smaller output)
dotnet publish -c Release -o ./publish

# Self-contained (includes the runtime — no .NET needed on target)
dotnet publish -c Release -r linux-x64 --self-contained -o ./publish

# Single-file executable (everything packed into one .exe/.bin)
dotnet publish -c Release -r win-x64 \
  --self-contained \
  -p:PublishSingleFile=true \
  -p:IncludeNativeLibrariesForSelfExtract=true \
  -o ./publish

# Native AOT (.NET 7+ — no JIT, fast startup, small binary)
dotnet publish -c Release -r linux-x64 -p:PublishAot=true -o ./publish

# Trim unused code (reduces binary size)
dotnet publish -c Release -r linux-x64 --self-contained \
  -p:PublishTrimmed=true -o ./publish

# ReadyToRun — pre-JIT code for faster startup (not as small as AOT)
dotnet publish -c Release -r win-x64 --self-contained \
  -p:PublishReadyToRun=true -o ./publish
```

**Or configure in `.csproj`:**

```xml
<PropertyGroup>
  <RuntimeIdentifier>linux-x64</RuntimeIdentifier>
  <SelfContained>true</SelfContained>
  <PublishSingleFile>true</PublishSingleFile>
  <PublishTrimmed>true</PublishTrimmed>
</PropertyGroup>
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `WebHostBuilder` class and how is it used?

`WebHostBuilder` is the legacy (.NET Core 1.x–2.x) host builder. In .NET 6+, it was superseded by `WebApplication.CreateBuilder()` (minimal hosting model). The older `Host.CreateDefaultBuilder()` + `ConfigureWebHostDefaults()` pattern from .NET 3.1–5 is also still supported.

```cs
//  Legacy — .NET Core 2.x WebHostBuilder (avoid in new code)
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .Build();

//   .NET 3.1 / 5 — Generic Host + ConfigureWebHostDefaults
Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
        webBuilder.UseKestrel(opts => opts.Limits.MaxRequestBodySize = 10 * 1024 * 1024);
    })
    .Build().Run();

// … .NET 10 — WebApplication.CreateBuilder (current best practice)
var builder = WebApplication.CreateBuilder(args);

builder.WebHost.ConfigureKestrel(opts =>
    opts.Limits.MaxConcurrentConnections = 1000);

builder.Services.AddControllers();

var app = builder.Build();
app.UseHttpsRedirection();
app.MapControllers();
app.Run();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you deploy a .NET Core application to Azure?

```bash
# 1. Azure App Service — simplest option
# Publish directly from CLI
dotnet publish -c Release -o ./publish
az login
az webapp deployment source config-zip \
  --resource-group myRG \
  --name myAppService \
  --src ./publish.zip

# Or use the Azure Web Apps deploy action (see GitHub Actions below)
```

```yaml
# 2. GitHub Actions — deploy to Azure App Service
name: Deploy to Azure
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4

    - name: Setup .NET 10
      uses: actions/setup-dotnet@v4
      with:
        dotnet-version: '10.0.x'

    - name: Publish
      run: dotnet publish -c Release -o ./publish

    - name: Deploy to Azure App Service
      uses: azure/webapps-deploy@v3
      with:
        app-name: 'myAppService'
        publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
        package: ./publish
```

```bash
# 3. Azure Container Apps — deploy as Docker container
docker build -t myapp:latest .
az acr login --name myRegistry
docker tag myapp:latest myregistry.azurecr.io/myapp:latest
docker push myregistry.azurecr.io/myapp:latest

az containerapp update \
  --name myapp \
  --resource-group myRG \
  --image myregistry.azurecr.io/myapp:latest

# 4. Azure Kubernetes Service (AKS)
kubectl apply -f deployment.yaml
kubectl set image deployment/myapp myapp=myregistry.azurecr.io/myapp:v2
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `Azure App Service` and how is it used?

**Azure App Service** is a fully managed PaaS (Platform-as-a-Service) for hosting web apps, REST APIs, and mobile backends. It handles OS patching, scaling, SSL, and load balancing automatically.

```bash
# Create and deploy via Azure CLI
az group create --name myRG --location uksouth

az appservice plan create \
  --name myPlan \
  --resource-group myRG \
  --sku B1 \          # Free(F1), Basic(B1), Standard(S1), Premium(P1v3)
  --is-linux

az webapp create \
  --resource-group myRG \
  --plan myPlan \
  --name my-dotnet-app \
  --runtime "DOTNET|10.0"

# Deploy a ZIP package
dotnet publish -c Release -o ./publish
Compress-Archive ./publish/* publish.zip
az webapp deploy \
  --resource-group myRG \
  --name my-dotnet-app \
  --src-path publish.zip

# Set environment variables / app settings
az webapp config appsettings set \
  --resource-group myRG \
  --name my-dotnet-app \
  --settings \
    ASPNETCORE_ENVIRONMENT=Production \
    ConnectionStrings__Default="Server=mydb.database.windows.net;..."

# Enable auto-scaling (Standard plan or higher)
az monitor autoscale create \
  --resource-group myRG \
  --resource my-dotnet-app \
  --resource-type Microsoft.Web/serverfarms \
  --name myAutoscale \
  --min-count 1 --max-count 5 --count 1
```

```cs
// Read App Service environment variables in code (same as any config)
var builder = WebApplication.CreateBuilder(args);
// App settings from Azure portal automatically appear as environment variables
string? dbConn = builder.Configuration.GetConnectionString("Default");
string? env    = builder.Configuration["ASPNETCORE_ENVIRONMENT"];
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use Docker to containerize a .NET Core application?

```dockerfile
# Dockerfile — multi-stage build for minimal final image (.NET 10)
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS build
WORKDIR /src

# Restore dependencies (cached layer)
COPY *.csproj ./
RUN dotnet restore

# Build and publish
COPY . ./
RUN dotnet publish -c Release -o /app/publish --no-restore

# Runtime image — smaller than sdk image
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS runtime
WORKDIR /app
COPY --from=build /app/publish .

# Non-root user (security best practice)
RUN adduser --disabled-password appuser
USER appuser

EXPOSE 8080
ENV ASPNETCORE_URLS=http://+:8080
ENTRYPOINT ["dotnet", "MyApp.dll"]
```

```bash
# Build image
docker build -t myapp:1.0 .

# Run locally
docker run -d -p 8080:8080 \
  -e ASPNETCORE_ENVIRONMENT=Development \
  -e ConnectionStrings__Default="Server=host.docker.internal;..." \
  --name myapp myapp:1.0

# Test
curl http://localhost:8080/health

# Push to registry
docker tag myapp:1.0 myregistry.azurecr.io/myapp:1.0
docker push myregistry.azurecr.io/myapp:1.0
```

```yaml
# docker-compose.yml — local dev with database
version: '3.8'
services:
  api:
    build: .
    ports: ["8080:8080"]
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ConnectionStrings__Default=Server=db;Database=MyDb;User=sa;Password=Pass@123
    depends_on: [db]

  db:
    image: mcr.microsoft.com/mssql/server:2022-latest
    environment:
      SA_PASSWORD: "Pass@123"
      ACCEPT_EULA: "Y"
    ports: ["1433:1433"]
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a Dockerfile for a .NET Core application?

```dockerfile
# Dockerfile — production-grade .NET 10 Web API

#  Stage 1: Restore ————————————————————————————————————————
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS restore
WORKDIR /src
COPY ["MyApi/MyApi.csproj", "MyApi/"]
COPY ["MyApi.Core/MyApi.Core.csproj", "MyApi.Core/"]
RUN dotnet restore "MyApi/MyApi.csproj"

#  Stage 2: Build ——————————————————————————————————————————
FROM restore AS build
COPY . .
RUN dotnet build "MyApi/MyApi.csproj" -c Release --no-restore

#  Stage 3: Publish ————————————————————————————————————————
FROM build AS publish
RUN dotnet publish "MyApi/MyApi.csproj" \
    -c Release \
    --no-build \
    -o /app/publish

#  Stage 4: Runtime (final, smallest image) ————————————————
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS final

# Security: non-root user
RUN groupadd -r appgroup && useradd -r -g appgroup appuser
WORKDIR /app
COPY --from=publish /app/publish .

# Health check
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD curl -f http://localhost:8080/health || exit 1

USER appuser
EXPOSE 8080
ENV ASPNETCORE_URLS=http://+:8080 \
    DOTNET_RUNNING_IN_CONTAINER=true

ENTRYPOINT ["dotnet", "MyApi.dll"]
```

```bash
# .dockerignore — exclude unnecessary files
```

```text
# .dockerignore
**/.git
**/.vs
**/bin
**/obj
**/*.user
**/node_modules
**/Dockerfile*
**/.gitignore
README.md
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you deploy a .NET Core application using Docker?

```bash
# 1. Build and run locally
docker build -t myapi:latest .
docker run -d -p 8080:8080 --name myapi myapi:latest
curl http://localhost:8080/health

# 2. Push to Docker Hub
docker login
docker tag myapi:latest username/myapi:1.0
docker push username/myapi:1.0

# 3. Push to Azure Container Registry
az acr login --name myRegistry
docker tag myapi:latest myregistry.azurecr.io/myapi:1.0
docker push myregistry.azurecr.io/myapi:1.0

# 4. Deploy to Azure Container Apps
az containerapp create \
  --name myapi \
  --resource-group myRG \
  --environment myEnv \
  --image myregistry.azurecr.io/myapi:1.0 \
  --target-port 8080 \
  --ingress external \
  --min-replicas 1 \
  --max-replicas 10 \
  --cpu 0.5 --memory 1Gi

# 5. Deploy to Kubernetes (AKS)
kubectl apply -f - <<EOF
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapi
spec:
  replicas: 3
  selector:
    matchLabels: { app: myapi }
  template:
    metadata:
      labels: { app: myapi }
    spec:
      containers:
      - name: myapi
        image: myregistry.azurecr.io/myapi:1.0
        ports:
        - containerPort: 8080
        resources:
          requests: { cpu: "100m", memory: "128Mi" }
          limits:   { cpu: "500m", memory: "512Mi" }
        livenessProbe:
          httpGet: { path: /health, port: 8080 }
          initialDelaySeconds: 10
EOF
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Kubernetes and how is it used with .NET Core applications?

**Kubernetes (K8s)** is an open-source container orchestration platform that automates deployment, scaling, and management of containerised applications. For .NET apps it handles rolling updates, health monitoring, auto-scaling, service discovery, and secrets management.

```yaml
# deployment.yaml — full .NET 10 Web API Kubernetes manifest
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapi
  labels: { app: myapi }
spec:
  replicas: 3
  selector:
    matchLabels: { app: myapi }
  template:
    metadata:
      labels: { app: myapi }
    spec:
      containers:
      - name: myapi
        image: myregistry.azurecr.io/myapi:1.0
        ports:
        - containerPort: 8080
        env:
        - name: ASPNETCORE_ENVIRONMENT
          value: Production
        - name: ConnectionStrings__Default
          valueFrom:
            secretKeyRef:
              name: myapi-secrets
              key: db-connection
        resources:
          requests: { cpu: "100m", memory: "128Mi" }
          limits:   { cpu: "500m", memory: "512Mi" }
        readinessProbe:
          httpGet: { path: /health/ready, port: 8080 }
          initialDelaySeconds: 5
        livenessProbe:
          httpGet: { path: /health/live, port: 8080 }
          initialDelaySeconds: 15
---
apiVersion: v1
kind: Service
metadata:
  name: myapi-svc
spec:
  selector: { app: myapi }
  ports:
  - port: 80
    targetPort: 8080
  type: LoadBalancer
```

```bash
# Apply manifests
kubectl apply -f deployment.yaml

# Rolling update (zero-downtime)
kubectl set image deployment/myapi myapi=myregistry.azurecr.io/myapi:1.1

# Scale
kubectl scale deployment myapi --replicas=5

# View logs
kubectl logs -l app=myapi --tail=100 -f
```

```cs
// Health check endpoints for Kubernetes probes
builder.Services.AddHealthChecks()
    .AddDbContextCheck<AppDbContext>();

app.MapHealthChecks("/health/live",  new() { Predicate = _ => false });   // liveness
app.MapHealthChecks("/health/ready", new() { Predicate = r => r.Tags.Contains("ready") }); // readiness
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use CI/CD pipelines to deploy .NET Core applications?

```yaml
# GitHub Actions — CI/CD pipeline for .NET 10 API ’ Azure App Service
name: CI/CD Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

env:
  DOTNET_VERSION: '10.0.x'
  AZURE_WEBAPP_NAME: 'my-dotnet-api'

jobs:
  #  CI: Build & Test ————————————————————————————————————
  build-and-test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4

    - name: Setup .NET ${{ env.DOTNET_VERSION }}
      uses: actions/setup-dotnet@v4
      with:
        dotnet-version: ${{ env.DOTNET_VERSION }}

    - name: Restore
      run: dotnet restore

    - name: Build
      run: dotnet build -c Release --no-restore

    - name: Test
      run: dotnet test -c Release --no-build \
             --logger "trx;LogFileName=results.trx" \
             --collect:"XPlat Code Coverage"

    - name: Upload test results
      uses: actions/upload-artifact@v4
      with:
        name: test-results
        path: "**/*.trx"

    - name: Publish
      run: dotnet publish -c Release -o ./publish --no-build

    - name: Upload artifact
      uses: actions/upload-artifact@v4
      with:
        name: app
        path: ./publish

  #  CD: Deploy (main branch only) ——————————————————————
  deploy:
    needs: build-and-test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    environment: production

    steps:
    - name: Download artifact
      uses: actions/download-artifact@v4
      with:
        name: app
        path: ./publish

    - name: Deploy to Azure App Service
      uses: azure/webapps-deploy@v3
      with:
        app-name: ${{ env.AZURE_WEBAPP_NAME }}
        publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
        package: ./publish
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `Azure DevOps` service and how is it used for deployment?

**Azure DevOps** is a Microsoft platform providing: **Boards** (work tracking), **Repos** (Git), **Pipelines** (CI/CD), **Test Plans**, and **Artifacts** (NuGet/npm feeds).

```yaml
# azure-pipelines.yml — CI/CD for .NET 10 API
trigger:
  branches:
    include: [main]

pool:
  vmImage: 'ubuntu-latest'

variables:
  buildConfiguration: 'Release'
  dotnetVersion: '10.0.x'

stages:
#  Stage 1: Build & Test ————————————————————————————————
- stage: Build
  jobs:
  - job: BuildAndTest
    steps:
    - task: UseDotNet@2
      inputs:
        version: $(dotnetVersion)

    - script: dotnet restore
      displayName: Restore

    - script: dotnet build -c $(buildConfiguration) --no-restore
      displayName: Build

    - script: |
        dotnet test -c $(buildConfiguration) --no-build \
          --logger trx \
          --collect "XPlat Code Coverage"
      displayName: Test

    - task: PublishTestResults@2
      inputs:
        testResultsFormat: VSTest
        testResultsFiles: '**/*.trx'

    - script: dotnet publish -c $(buildConfiguration) -o $(Build.ArtifactStagingDirectory)/publish
      displayName: Publish

    - task: PublishBuildArtifacts@1
      inputs:
        PathtoPublish: $(Build.ArtifactStagingDirectory)/publish
        ArtifactName: drop

#  Stage 2: Deploy to Staging ——————————————————————————
- stage: DeployStaging
  dependsOn: Build
  jobs:
  - deployment: DeployToStaging
    environment: staging
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            inputs:
              azureSubscription: 'MyServiceConnection'
              appName: 'my-api-staging'
              package: $(Pipeline.Workspace)/drop

#  Stage 3: Deploy to Production (with approval) ———————
- stage: DeployProd
  dependsOn: DeployStaging
  jobs:
  - deployment: DeployToProduction
    environment: production   # configure approvals in Azure DevOps portal
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            inputs:
              azureSubscription: 'MyServiceConnection'
              appName: 'my-api-production'
              package: $(Pipeline.Workspace)/drop
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you configure a build pipeline in Azure DevOps?

A **build pipeline** compiles code, runs tests, and produces deployable artifacts. It is defined in `azure-pipelines.yml` at the repo root.

```yaml
# azure-pipelines.yml — Build pipeline for .NET 10
trigger:
  branches:
    include: [main, develop]
  paths:
    exclude: ['*.md', 'docs/**']

pr:
  branches:
    include: [main]

pool:
  vmImage: 'ubuntu-latest'

variables:
  buildConfiguration: 'Release'

steps:
# 1. Use specified .NET SDK
- task: UseDotNet@2
  displayName: 'Install .NET 10 SDK'
  inputs:
    version: '10.0.x'
    includePreviewVersions: false

# 2. Restore
- script: dotnet restore --locked-mode
  displayName: 'dotnet restore'

# 3. Build
- script: dotnet build -c $(buildConfiguration) --no-restore
  displayName: 'dotnet build'

# 4. Test with coverage
- script: |
    dotnet test -c $(buildConfiguration) --no-build \
      --logger "trx;LogFileName=TestResults.trx" \
      --collect "XPlat Code Coverage" \
      -- DataCollectionRunSettings.DataCollectors.DataCollector.Configuration.Format=cobertura
  displayName: 'dotnet test'

- task: PublishTestResults@2
  inputs:
    testResultsFormat: VSTest
    testResultsFiles: '**/TestResults.trx'
    failTaskOnFailedTests: true

- task: PublishCodeCoverageResults@2
  inputs:
    summaryFileLocation: '**/coverage.cobertura.xml'

# 5. Publish
- script: dotnet publish src/MyApi -c $(buildConfiguration) --no-build -o $(Build.ArtifactStagingDirectory)
  displayName: 'dotnet publish'

# 6. Archive artifact
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: $(Build.ArtifactStagingDirectory)
    ArtifactName: 'myapi-$(Build.BuildNumber)'
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you configure a release pipeline in Azure DevOps?

A **release pipeline** (or deployment stage in YAML) takes a build artifact and deploys it to one or more environments, optionally with approval gates.

```yaml
# Deployment stages added to azure-pipelines.yml (single YAML pipeline)
stages:
- stage: Build
  jobs:
  - job: Build
    steps:
    - script: dotnet publish -c Release -o $(Build.ArtifactStagingDirectory)
    - task: PublishBuildArtifacts@1
      inputs: { ArtifactName: drop }

- stage: DeployDev
  displayName: 'Deploy to Dev'
  dependsOn: Build
  condition: succeeded()
  variables:
    webAppName: 'myapi-dev'
  jobs:
  - deployment: Deploy
    environment: dev        # no approval needed
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            inputs:
              azureSubscription: 'MyServiceConnection'
              appName: $(webAppName)
              package: $(Pipeline.Workspace)/drop

          - task: AzureAppServiceSettings@1
            inputs:
              azureSubscription: 'MyServiceConnection'
              appName: $(webAppName)
              appSettings: |
                [
                  {"name": "ASPNETCORE_ENVIRONMENT", "value": "Development"}
                ]

- stage: DeployProd
  displayName: 'Deploy to Production'
  dependsOn: DeployDev
  condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
  variables:
    webAppName: 'myapi-prod'
  jobs:
  - deployment: Deploy
    environment: production   # configure approval in Environments ’ Approvals & Checks
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            displayName: 'Blue-Green slot swap'
            inputs:
              azureSubscription: 'MyServiceConnection'
              appName: $(webAppName)
              deployToSlotOrASE: true
              resourceGroupName: myRG
              slotName: staging        # deploy to staging slot first
              package: $(Pipeline.Workspace)/drop

          - task: AzureAppServiceManage@0
            displayName: 'Swap staging ’ production'
            inputs:
              azureSubscription: 'MyServiceConnection'
              Action: 'Swap Slots'
              WebAppName: $(webAppName)
              ResourceGroupName: myRG
              SourceSlot: staging
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use GitHub Actions for deploying .NET Core applications?

```yaml
# .github/workflows/deploy.yml — Full CI/CD with GitHub Actions
name: Build, Test & Deploy

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    outputs:
      artifact-path: ${{ steps.publish.outputs.artifact-path }}

    steps:
    - uses: actions/checkout@v4

    - uses: actions/setup-dotnet@v4
      with:
        dotnet-version: '10.0.x'

    - run: dotnet restore
    - run: dotnet build -c Release --no-restore
    - run: dotnet test -c Release --no-build --logger "trx" --collect "XPlat Code Coverage"

    - uses: actions/upload-artifact@v4        # upload test results
      with:
        name: test-results
        path: "**/*.trx"

    - name: Publish
      id: publish
      run: dotnet publish -c Release -o ./publish --no-build

    - uses: actions/upload-artifact@v4
      with:
        name: app
        path: ./publish

  deploy-azure:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    environment:
      name: production
      url: https://my-dotnet-api.azurewebsites.net

    steps:
    - uses: actions/download-artifact@v4
      with: { name: app, path: ./publish }

    # Option A — Azure App Service
    - uses: azure/webapps-deploy@v3
      with:
        app-name: 'my-dotnet-api'
        publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
        package: ./publish

  deploy-docker:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
    - uses: actions/checkout@v4

    # Option B — Build & push Docker image to GHCR
    - uses: docker/login-action@v3
      with:
        registry: ghcr.io
        username: ${{ github.actor }}
        password: ${{ secrets.GITHUB_TOKEN }}

    - uses: docker/build-push-action@v6
      with:
        context: .
        push: true
        tags: ghcr.io/${{ github.repository }}:${{ github.sha }}
        cache-from: type=gha
        cache-to: type=gha,mode=max
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `Octopus Deploy` tool and how is it used?

**Octopus Deploy** is a release management and deployment automation tool that complements CI systems (Azure DevOps, GitHub Actions, Jenkins). It models environments, targets, deployment processes, and variable sets separately from build pipelines.

```yaml
# GitHub Actions ’ Octopus Deploy integration
- name: Push package to Octopus
  uses: OctopusDeploy/push-package-action@v3
  with:
    api_key: ${{ secrets.OCTOPUS_API_KEY }}
    server: https://mycompany.octopus.app
    packages: myapi.1.0.${{ github.run_number }}.zip

- name: Create release in Octopus
  uses: OctopusDeploy/create-release-action@v3
  with:
    api_key: ${{ secrets.OCTOPUS_API_KEY }}
    server: https://mycompany.octopus.app
    project: MyApi
    release_number: 1.0.${{ github.run_number }}

- name: Deploy to Staging
  uses: OctopusDeploy/deploy-release-action@v3
  with:
    api_key: ${{ secrets.OCTOPUS_API_KEY }}
    server: https://mycompany.octopus.app
    project: MyApi
    release_number: 1.0.${{ github.run_number }}
    environments: Staging
```

**Key Octopus concepts:**
| Concept | Description |
|---------|-------------|
| **Project** | Deployment process definition (steps, variables) |
| **Environment** | Dev / Staging / Production logical grouping |
| **Release** | Snapshot of project + package versions |
| **Deployment** | Running a release against an environment |
| **Runbook** | Operational scripts (db backup, restart service) |
| **Variable Sets** | Shared variables across projects |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the best practices for deploying .NET Core applications?

```bash
# 1. Always publish in Release configuration
dotnet publish -c Release -o ./publish

# 2. Use health checks for readiness/liveness probes
```

```cs
// Health checks
builder.Services.AddHealthChecks()
    .AddDbContextCheck<AppDbContext>(tags: ["ready"])
    .AddUrlGroup(new Uri("https://ext-service/health"), tags: ["ready"]);

app.MapHealthChecks("/health/live",  new() { Predicate = _ => false });
app.MapHealthChecks("/health/ready", new() { Predicate = r => r.Tags.Contains("ready") });
```

```bash
# 3. Secrets management — never commit secrets; use environment vars / Key Vault
az keyvault secret set --vault-name myVault --name db-password --value "s3cret"
# In app: builder.Configuration.AddAzureKeyVault(...)

# 4. Use structured logging (Serilog/OpenTelemetry)
# 5. Enable HTTPS everywhere
# 6. Multi-stage Docker builds (keep images small)
# 7. Pin runtime versions in Dockerfile FROM mcr.microsoft.com/dotnet/aspnet:10.0
# 8. Blue-green or canary deployments via deployment slots (App Service) or K8s
# 9. Database migrations — apply via startup or migration job, never in production manually
# 10. Enable AOT/ReadyToRun for faster cold starts
dotnet publish -c Release -r linux-x64 -p:PublishAot=true
```

```cs
// Graceful shutdown
builder.Services.Configure<HostOptions>(o =>
    o.ShutdownTimeout = TimeSpan.FromSeconds(30)); // allow in-flight requests to complete

// Environment-specific config
if (app.Environment.IsProduction())
{
    app.UseExceptionHandler("/error");
    app.UseHsts();
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the different types of hosting models available in .NET Core?

| Hosting model | Description | Use case |
|--------------|-------------|---------|
| **In-process (IIS)** | App runs inside IIS worker process (`w3wp.exe`) | Windows IIS hosting, better performance |
| **Out-of-process (IIS)** | IIS proxies to Kestrel running as separate process | Isolation, Linux-compatible workflow |
| **Kestrel (edge)** | Kestrel directly exposed to internet | Linux, Docker, Cloud |
| **Kestrel + reverse proxy** | Nginx/IIS/YARP in front of Kestrel | Production recommended |
| **Self-contained** | App includes .NET runtime — no SDK needed on host | Offline, locked environments |
| **Framework-dependent** | Uses installed .NET runtime | Shared hosting, smaller package |
| **Docker container** | App + runtime in container image | K8s, Azure Container Apps |
| **Native AOT** | Compiled to native binary, no JIT/runtime | Ultra-fast startup, serverless, CLI tools |
| **Background Service / Worker** | `IHostedService` without HTTP | Message queues, scheduled jobs |

```cs
// In-process IIS hosting — .csproj
// <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>

// Out-of-process IIS hosting
// <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>

// Worker Service (no HTTP)
var builder = Host.CreateApplicationBuilder(args);
builder.Services.AddHostedService<MyWorker>();
await builder.Build().RunAsync();

// Self-contained publish
// dotnet publish -r win-x64 --self-contained -o ./publish

// Native AOT
// dotnet publish -r linux-x64 -p:PublishAot=true -o ./publish

// Kestrel with Unix socket (Nginx upstream)
builder.WebHost.ConfigureKestrel(opts =>
    opts.ListenUnixSocket("/tmp/myapp.sock"));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you build a CI/CD pipeline with GitHub Actions for a .NET application?

A CI/CD pipeline automates build, test, and deployment on every push. GitHub Actions uses YAML workflow files in `.github/workflows/`.

```yaml
# .github/workflows/ci-cd.yml
name: .NET CI/CD

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

env:
  DOTNET_VERSION: '10.0.x'
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  #  1. BUILD & TEST ————————————————————————————————————————————————
  build-and-test:
    name: Build and Test
    runs-on: ubuntu-latest

    services:
      postgres:
        image: postgres:16
        env:
          POSTGRES_PASSWORD: testpassword
          POSTGRES_DB: testdb
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports: ['5432:5432']

    steps:
      - uses: actions/checkout@v4

      - name: Setup .NET ${{ env.DOTNET_VERSION }}
        uses: actions/setup-dotnet@v4
        with:
          dotnet-version: ${{ env.DOTNET_VERSION }}

      - name: Cache NuGet packages
        uses: actions/cache@v4
        with:
          path: ~/.nuget/packages
          key: ${{ runner.os }}-nuget-${{ hashFiles('**/*.csproj') }}
          restore-keys: ${{ runner.os }}-nuget-

      - name: Restore dependencies
        run: dotnet restore

      - name: Build
        run: dotnet build --no-restore -c Release

      - name: Run unit tests
        run: dotnet test --no-build -c Release \
          --filter "Category!=Integration" \
          --logger "trx;LogFileName=unit-results.trx" \
          --collect:"XPlat Code Coverage" \
          --results-directory ./test-results

      - name: Run integration tests
        env:
          ConnectionStrings__Default: "Host=localhost;Port=5432;Database=testdb;Username=postgres;Password=testpassword"
        run: dotnet test --no-build -c Release \
          --filter "Category=Integration" \
          --logger "trx;LogFileName=integration-results.trx" \
          --results-directory ./test-results

      - name: Publish test results
        uses: dorny/test-reporter@v1
        if: always()
        with:
          name: Test Results
          path: ./test-results/*.trx
          reporter: dotnet-trx

      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v4
        with:
          directory: ./test-results
          token: ${{ secrets.CODECOV_TOKEN }}

  #  2. CODE QUALITY ————————————————————————————————————————————————
  code-quality:
    name: Code Analysis
    runs-on: ubuntu-latest
    needs: build-and-test

    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0  # needed for SonarCloud

      - name: Setup .NET
        uses: actions/setup-dotnet@v4
        with:
          dotnet-version: ${{ env.DOTNET_VERSION }}

      - name: Run Roslyn analyzers
        run: dotnet build -c Release -p:TreatWarningsAsErrors=true

  #  3. BUILD DOCKER IMAGE ——————————————————————————————————————————
  build-image:
    name: Build Docker Image
    runs-on: ubuntu-latest
    needs: build-and-test
    if: github.event_name != 'pull_request'

    outputs:
      image-digest: ${{ steps.push.outputs.digest }}

    steps:
      - uses: actions/checkout@v4

      - name: Log in to Container Registry
        uses: docker/login-action@v3
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Extract metadata
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
          tags: |
            type=ref,event=branch
            type=sha,prefix=sha-
            type=semver,pattern={{version}}

      - name: Build and push
        id: push
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
          cache-from: type=gha
          cache-to: type=gha,mode=max

  #  4. DEPLOY TO STAGING ———————————————————————————————————————————
  deploy-staging:
    name: Deploy to Staging
    runs-on: ubuntu-latest
    needs: build-image
    environment:
      name: staging
      url: https://staging.myapp.com
    if: github.ref == 'refs/heads/develop'

    steps:
      - uses: actions/checkout@v4

      - name: Deploy to Azure Container Apps
        uses: azure/container-apps-deploy-action@v1
        with:
          appSourcePath: ${{ github.workspace }}
          acrName: myregistry
          containerAppName: myapp-staging
          resourceGroup: myapp-staging-rg
          imageToDeploy: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:sha-${{ github.sha }}

  #  5. DEPLOY TO PRODUCTION ————————————————————————————————————————
  deploy-production:
    name: Deploy to Production
    runs-on: ubuntu-latest
    needs: deploy-staging
    environment:
      name: production
      url: https://myapp.com
    if: github.ref == 'refs/heads/main'

    steps:
      - name: Deploy (blue-green via App Service slots)
        run: |
          az webapp deployment slot swap \
            --resource-group myapp-prod-rg \
            --name myapp \
            --slot staging \
            --target-slot production
        env:
          AZURE_CREDENTIALS: ${{ secrets.AZURE_CREDENTIALS }}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a Docker multi-stage build for a .NET application?

A **multi-stage Dockerfile** uses separate build and runtime images, keeping the final image small and free of SDK tools.

```dockerfile
#  Dockerfile ——————————————————————————————————————————————————————
# Stage 1: Restore dependencies (cached unless .csproj changes)
FROM mcr.microsoft.com/dotnet/sdk:10.0 AS restore
WORKDIR /src

# Copy only project files first — layer cached until .csproj changes
COPY ["src/Api/Api.csproj",         "src/Api/"]
COPY ["src/Core/Core.csproj",        "src/Core/"]
COPY ["src/Infrastructure/Infrastructure.csproj", "src/Infrastructure/"]
COPY ["Directory.Packages.props",    "."]
RUN dotnet restore "src/Api/Api.csproj"

# Stage 2: Build
FROM restore AS build
COPY . .
WORKDIR /src/src/Api
RUN dotnet build "Api.csproj" -c Release --no-restore -o /app/build

# Stage 3: Publish (optimized, trimmed binary)
FROM build AS publish
RUN dotnet publish "Api.csproj" \
    -c Release \
    --no-build \
    -o /app/publish \
    -p:PublishSingleFile=false \
    -p:PublishTrimmed=false

# Stage 4: Runtime image (no SDK — much smaller)
FROM mcr.microsoft.com/dotnet/aspnet:10.0 AS final

# Security: run as non-root
RUN addgroup --system appgroup && adduser --system --ingroup appgroup appuser
WORKDIR /app
COPY --from=publish /app/publish .

# Security: drop all capabilities, run as non-root
USER appuser

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
  CMD wget --quiet --tries=1 --spider http://localhost:8080/health/live || exit 1

EXPOSE 8080
ENV ASPNETCORE_URLS=http://+:8080
ENV DOTNET_RUNNING_IN_CONTAINER=true

ENTRYPOINT ["dotnet", "Api.dll"]
```

```yaml
# docker-compose.yml — local development
services:
  api:
    build:
      context: .
      target: final          # use 'build' stage for debugging
    ports:
      - "8080:8080"
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ConnectionStrings__Default=Host=db;Database=myapp;Username=postgres;Password=secret
    depends_on:
      db:
        condition: service_healthy
    volumes:
      - ~/.aspnet/https:/https:ro    # dev certs

  db:
    image: postgres:16
    environment:
      POSTGRES_DB: myapp
      POSTGRES_PASSWORD: secret
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 5s
      retries: 5
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

```bash
# Build and run
docker build -t myapp:latest .
docker run -p 8080:8080 --env ASPNETCORE_ENVIRONMENT=Production myapp:latest

# Multi-platform build (for ARM64 / Apple Silicon)
docker buildx build --platform linux/amd64,linux/arm64 -t myapp:latest --push .

# Inspect image layers and size
docker history myapp:latest
docker images myapp:latest   # SDK: ~800MB ’ Runtime: ~220MB ’ Trimmed: ~80MB
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you manage environment-specific configuration in .NET Core?

.NET Core uses a **layered configuration** system where later sources override earlier ones. Environment-specific overrides are applied automatically.

```cs
//  Configuration loading order (last wins) —————————————————————————
// 1. appsettings.json                    (all environments)
// 2. appsettings.{Environment}.json      (env-specific)
// 3. User Secrets (Development only)
// 4. Environment variables
// 5. Command-line arguments

// WebApplication.CreateBuilder() sets this up automatically

//  appsettings.json ————————————————————————————————————————————————
{
  "Logging": { "LogLevel": { "Default": "Information" } },
  "ConnectionStrings": {
    "Default": "Host=localhost;Database=myapp;Username=postgres;Password=devpass"
  },
  "FeatureFlags": { "NewCheckout": false },
  "EmailSettings": { "SmtpHost": "localhost", "Port": 1025 }
}

//  appsettings.Production.json —————————————————————————————————————
{
  "Logging": { "LogLevel": { "Default": "Warning" } },
  "FeatureFlags": { "NewCheckout": true }
  // ConnectionStrings come from environment variable, not file
}

//  Binding configuration to strongly-typed classes —————————————————
public class EmailSettings
{
    public string SmtpHost { get; init; } = null!;
    public int    Port     { get; init; }
    public string? Username { get; init; }
    public string? Password { get; init; }
}

// Program.cs
builder.Services.Configure<EmailSettings>(
    builder.Configuration.GetSection("EmailSettings"));

// Or use Options pattern with validation
builder.Services.AddOptions<EmailSettings>()
    .Bind(builder.Configuration.GetSection("EmailSettings"))
    .ValidateDataAnnotations()
    .ValidateOnStart(); // validate at startup, not first use

// Usage (inject IOptions<T> or IOptionsSnapshot<T>)
public class EmailService(IOptions<EmailSettings> opts)
{
    private readonly EmailSettings _settings = opts.Value;

    public Task SendAsync(string to, string subject, string body)
    {
        Console.WriteLine($"SMTP: {_settings.SmtpHost}:{_settings.Port}");
        return Task.CompletedTask;
    }
}

//  User Secrets (Development only — not committed to source control) 
// dotnet user-secrets init
// dotnet user-secrets set "EmailSettings:Password" "mysecretpassword"
// Stored in: %APPDATA%\Microsoft\UserSecrets\{userSecretsId}\secrets.json

//  Environment Variables — override any key ————————————————————————
// Flat key: EMAILSETTINGS__PASSWORD=secret   (double underscore = section separator)
// Connection string: ConnectionStrings__Default=Host=prod-db;...

//  Azure Key Vault (production secrets) ————————————————————————————
if (builder.Environment.IsProduction())
{
    builder.Configuration.AddAzureKeyVault(
        new Uri($"https://{builder.Configuration["KeyVaultName"]}.vault.azure.net/"),
        new DefaultAzureCredential());
}

// Key Vault maps: "EmailSettings--SmtpHost" ’ EmailSettings:SmtpHost

//  Feature flags ———————————————————————————————————————————————————
builder.Services.AddFeatureManagement(builder.Configuration.GetSection("FeatureFlags"));

// Controller
public class CheckoutController(IFeatureManager features) : ControllerBase
{
    [HttpGet("checkout")]
    public async Task<IActionResult> Checkout()
    {
        if (await features.IsEnabledAsync("NewCheckout"))
            return Ok("New checkout flow");
        return Ok("Classic checkout");
    }
}

//  IConfiguration direct access ————————————————————————————————————
public class StartupInfo(IConfiguration config)
{
    public void Log()
    {
        string? connStr = config.GetConnectionString("Default");
        string? env     = config["ASPNETCORE_ENVIRONMENT"];
        bool    flag    = config.GetValue<bool>("FeatureFlags:NewCheckout");
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 20. .NET Core

<br>

## Q. What is .NET Core?

**.NET Core** (now simply called **.NET**) is Microsoft\'s open-source, cross-platform successor to .NET Framework. Starting with .NET 5, the "Core" name was dropped and it became the single unified runtime. The current version is **.NET 10** (2026).

**Key characteristics:**
- **Cross-platform** — Windows, Linux, macOS, ARM64
- **Open source** — hosted on GitHub (dotnet/runtime, dotnet/aspnetcore)
- **High performance** — consistently top-ranked in TechEmpower benchmarks
- **Modular** — NuGet-based; only include what you need
- **Cloud-native** — Docker, Kubernetes, Azure-first design
- **Unified** — one SDK for web, desktop, mobile, cloud, IoT, AI

```bash
# Check installed runtimes
dotnet --list-runtimes

# Check SDK versions
dotnet --list-sdks

# Current version
dotnet --version  # e.g. 10.0.100
```

```cs
// Minimal .NET 10 console app (top-level statements, no boilerplate)
using Microsoft.Extensions.Hosting;

var host = Host.CreateApplicationBuilder(args);
host.Services.AddHostedService<MyWorker>();
await host.Build().RunAsync();

class MyWorker : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken ct)
    {
        while (!ct.IsCancellationRequested)
        {
            Console.WriteLine($"[{DateTime.UtcNow:u}] Worker running");
            await Task.Delay(1000, ct);
        }
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the main differences between .NET Framework and .NET Core?

| Feature | .NET Framework | .NET (Core / 5+) |
|---------|---------------|-----------------|
| Platform | Windows only | Cross-platform |
| Open source | Partially | Fully (GitHub) |
| Deployment | GAC / machine-wide | Self-contained / side-by-side |
| Performance | Good | Significantly faster |
| Current status | Maintenance mode (4.8.x) | Active development (.NET 10) |
| ASP.NET | System.Web (heavy) | ASP.NET Core (lightweight, Kestrel) |
| WPF / WinForms | … | … (Windows only) |
| Xamarin / MAUI |  | … |
| AOT compilation |  | … (.NET 7+) |
| Containers | Limited | First-class Docker support |

```bash
# .NET Framework — Windows only, targeting net48
<TargetFramework>net48</TargetFramework>

# .NET 10 — cross-platform
<TargetFramework>net10.0</TargetFramework>

# Multi-targeting both
<TargetFrameworks>net48;net10.0</TargetFrameworks>
```

```cs
// .NET 10 — self-contained publish (no runtime needed on target machine)
// dotnet publish -r linux-x64 --self-contained -p:PublishSingleFile=true

// .NET 10 — Native AOT (no JIT, instant startup)
// dotnet publish -r linux-x64 -p:PublishAot=true
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the .NET Standard?

**.NET Standard** is a formal specification of .NET APIs that all .NET implementations must support. It was the bridge that allowed a single library to run on .NET Framework, .NET Core, Xamarin, and Unity simultaneously.

**Status:** With .NET 5+ unifying all platforms, .NET Standard is **no longer evolving**. New libraries should target `net10.0` (or a specific TFM). .NET Standard 2.0 is still useful for libraries that must support legacy .NET Framework 4.6.1+.

| .NET Standard | .NET Framework | .NET Core / .NET |
|--------------|---------------|-----------------|
| 2.0 | 4.6.1+ | 2.0+ |
| 2.1 |  (never) | 3.0+ |
| *(no 3.0)* | — | Use net10.0 TFM |

```xml
<!-- Library targeting .NET Standard 2.0 — works on .NET Framework 4.6.1+ and .NET 10 -->
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
  </PropertyGroup>
</Project>

<!-- Modern library — no legacy support needed -->
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net10.0</TargetFramework>
  </PropertyGroup>
</Project>

<!-- Multi-target for maximum compatibility -->
<TargetFrameworks>netstandard2.0;net10.0</TargetFrameworks>
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle configuration in a .NET Core application?

.NET uses a layered configuration system via `Microsoft.Extensions.Configuration`. Sources are stacked — later sources override earlier ones.

```cs
// appsettings.json
{
  "App": {
    "Name": "MyApi",
    "Timeout": 30
  },
  "ConnectionStrings": {
    "Default": "Server=localhost;Database=MyDb"
  }
}
```

```cs
// Program.cs (.NET 10 — WebApplication.CreateBuilder sets up config automatically)
var builder = WebApplication.CreateBuilder(args);

// Config sources (in priority order, lowest ’ highest):
// 1. appsettings.json
// 2. appsettings.{Environment}.json
// 3. Environment variables
// 4. Command-line args

// Bind to a strongly-typed options class
builder.Services.Configure<AppOptions>(
    builder.Configuration.GetSection("App"));

var app = builder.Build();

// Read raw value
string name = builder.Configuration["App:Name"]!;
string conn = builder.Configuration.GetConnectionString("Default")!;

// Inject IOptions<T> in a service
public class MyService(IOptions<AppOptions> opts)
{
    private readonly AppOptions _opts = opts.Value;
    public string AppName => _opts.Name;
}

public class AppOptions
{
    public string Name { get; set; } = "";
    public int Timeout { get; set; }
}

// Add custom JSON config source
builder.Configuration.AddJsonFile("custom.json", optional: true, reloadOnChange: true);

// Add environment variable with prefix
builder.Configuration.AddEnvironmentVariables(prefix: "MYAPP_");
// MYAPP_App__Name=Override  ’  Config["App:Name"] = "Override"
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the .NET Core CLI and how is it used?

The **.NET CLI** (`dotnet`) is the cross-platform command-line interface for creating, building, running, testing, and publishing .NET applications.

```bash
# --- Project Management ---
dotnet new console -n MyApp          # create console app
dotnet new webapi -n MyApi           # create Web API
dotnet new classlib -n MyLib         # create class library
dotnet new sln -n MySolution         # create solution file
dotnet sln add MyApp/MyApp.csproj    # add project to solution

# --- Build & Run ---
dotnet build                         # compile
dotnet run                           # build + run
dotnet run --project MyApp           # specify project
dotnet watch run                     # hot reload on file changes

# --- Testing ---
dotnet test                          # run all tests
dotnet test --filter "Category=Unit" # filter tests
dotnet test --collect "Code Coverage"

# --- Package Management ---
dotnet add package Serilog            # install NuGet package
dotnet remove package Serilog         # remove package
dotnet list package --outdated        # show outdated packages
dotnet restore                        # restore dependencies

# --- Publish ---
dotnet publish -c Release -r linux-x64 --self-contained
dotnet publish -p:PublishSingleFile=true
dotnet publish -p:PublishAot=true     # Native AOT (.NET 7+)

# --- Tools ---
dotnet tool install -g dotnet-ef      # install global tool
dotnet ef migrations add InitialCreate
dotnet ef database update

# --- Info ---
dotnet --version
dotnet --list-sdks
dotnet --list-runtimes
dotnet nuget list source
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a new .NET Core project?

```bash
# Create console app
dotnet new console -n HelloWorld
cd HelloWorld
dotnet run
# Output: Hello, World!

# Create ASP.NET Core Web API
dotnet new webapi -n MyApi --use-minimal-apis
cd MyApi
dotnet run
# Swagger at https://localhost:5001/swagger

# Create solution with multiple projects
mkdir MySolution && cd MySolution
dotnet new sln -n MySolution
dotnet new webapi -n MyApi
dotnet new classlib -n MyApi.Core
dotnet new xunit -n MyApi.Tests
dotnet sln add MyApi MyApi.Core MyApi.Tests
dotnet add MyApi/MyApi.csproj reference MyApi.Core/MyApi.Core.csproj
dotnet add MyApi.Tests/MyApi.Tests.csproj reference MyApi/MyApi.csproj

# Build and test everything
dotnet build
dotnet test
```

**Minimal Web API (generated by `dotnet new webapi --use-minimal-apis`):**

```cs
// Program.cs — .NET 10 minimal API
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddOpenApi();

var app = builder.Build();
app.MapOpenApi();

app.MapGet("/hello", () => new { Message = "Hello, .NET 10!" });

app.MapGet("/weather", () =>
{
    var forecasts = Enumerable.Range(1, 5).Select(i => new WeatherForecast(
        DateOnly.FromDateTime(DateTime.Now.AddDays(i)),
        Random.Shared.Next(-20, 55),
        "Sunny"));
    return forecasts;
});

app.Run();

record WeatherForecast(DateOnly Date, int TemperatureC, string Summary);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of the Program.cs and Startup.cs files in a .NET Core application?

In modern .NET (6+), **`Startup.cs` was eliminated** and its responsibilities merged into `Program.cs` using the minimal hosting model.

**Before .NET 6 (two files):**

```cs
// Program.cs — entry point, created the host
public class Program
{
    public static void Main(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(web => web.UseStartup<Startup>())
            .Build().Run();
}

// Startup.cs — service registration + middleware pipeline
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllers();
    }
    public void Configure(IApplicationBuilder app)
    {
        app.UseRouting();
        app.UseEndpoints(e => e.MapControllers());
    }
}
```

**.NET 10 (single Program.cs):**

```cs
// Program.cs — everything in one place
var builder = WebApplication.CreateBuilder(args);

// === ConfigureServices equivalent ===
builder.Services.AddControllers();
builder.Services.AddOpenApi();
builder.Services.AddScoped<IOrderService, OrderService>();

var app = builder.Build();

// === Configure equivalent (middleware pipeline) ===
if (app.Environment.IsDevelopment())
    app.MapOpenApi();

app.UseHttpsRedirection();
app.UseAuthentication();
app.UseAuthorization();
app.MapControllers();

app.Run();
```

**`Program.cs` roles in .NET 10:**
- Sets up the `WebApplicationBuilder` (config, DI, logging)
- Registers services into the DI container
- Builds and configures the middleware pipeline
- Starts the server

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you identify duplicated code in C#, and what techniques can be used to remove it?

**Identifying duplicates:**
- Visual Studio / Rider — built-in "Find Code Clones" / duplicate detection
- **SonarQube/SonarCloud** — detects duplicated blocks with metrics
- **ReSharper** — highlights duplicate code fragments
- **Roslyn Analyzers** — custom rules targeting repetitive patterns

**Techniques to remove duplication:**

```cs
//  Duplicated logic
public decimal CalculateUkTax(decimal amount) => amount * 0.20m;
public decimal CalculateUsTax(decimal amount) => amount * 0.10m;
// Same structure repeated for each region

// … 1. Extract Method / Helper
public decimal CalculateTax(decimal amount, decimal rate) => amount * rate;
// Usage:
decimal uk = CalculateTax(100m, 0.20m);
decimal us = CalculateTax(100m, 0.10m);

// … 2. Generic method
public static T Clamp<T>(T value, T min, T max) where T : IComparable<T>
    => value.CompareTo(min) < 0 ? min : value.CompareTo(max) > 0 ? max : value;

// … 3. Strategy pattern for varying behavior
public interface ITaxStrategy { decimal Calculate(decimal amount); }
public class UkTax : ITaxStrategy { public decimal Calculate(decimal a) => a * 0.20m; }
public class UsTax : ITaxStrategy { public decimal Calculate(decimal a) => a * 0.10m; }

// … 4. Extension methods for repeated operations on types
public static class StringExtensions
{
    public static bool IsNullOrEmpty(this string? s) => string.IsNullOrEmpty(s);
    public static string ToTitleCase(this string s) =>
        System.Globalization.CultureInfo.CurrentCulture.TextInfo.ToTitleCase(s.ToLower());
}

// … 5. Base class / template method for duplicated class structures
public abstract class ReportBase
{
    public string Generate()   // template method
    {
        var data    = FetchData();
        var body    = FormatBody(data);
        return $"<report>{body}</report>";
    }
    protected abstract IEnumerable<object> FetchData();
    protected abstract string FormatBody(IEnumerable<object> data);
}

// … 6. Generic repository to remove per-entity CRUD duplication
public class Repository<T>(AppDbContext db) where T : class
{
    public Task<T?> GetByIdAsync(int id) => db.Set<T>().FindAsync(id).AsTask();
    public void Add(T entity) => db.Set<T>().Add(entity);
    public Task SaveAsync() => db.SaveChangesAsync();
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Kestrel?

**Kestrel** is the cross-platform, high-performance HTTP server built into ASP.NET Core. It is the default web server and handles incoming HTTP connections directly without requiring IIS or Apache.

**Key features:**
- HTTP/1.1, HTTP/2, HTTP/3 (QUIC) support
- TLS termination
- Unix domain sockets, named pipes
- Used as edge server or behind a reverse proxy (Nginx, IIS, Azure Front Door)

```cs
// Program.cs — Kestrel is used by default
var builder = WebApplication.CreateBuilder(args);

// Configure Kestrel explicitly
builder.WebHost.ConfigureKestrel(options =>
{
    // HTTP on port 5000
    options.ListenLocalhost(5000);

    // HTTPS on port 5001 with certificate
    options.ListenLocalhost(5001, listenOptions =>
    {
        listenOptions.UseHttps("cert.pfx", "password");
        listenOptions.Protocols = Microsoft.AspNetCore.Server.Kestrel
            .Core.HttpProtocols.Http1AndHttp2AndHttp3;
    });

    // Limits
    options.Limits.MaxConcurrentConnections = 1000;
    options.Limits.MaxRequestBodySize = 10 * 1024 * 1024; // 10 MB
    options.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
});

// Or configure via appsettings.json
/*
"Kestrel": {
  "Endpoints": {
    "Http":  { "Url": "http://0.0.0.0:80" },
    "Https": { "Url": "https://0.0.0.0:443",
               "Certificate": { "Path": "cert.pfx", "Password": "pw" } }
  }
}
*/

var app = builder.Build();
app.MapGet("/", () => "Running on Kestrel!");
app.Run();
```

**Kestrel vs IIS:**

| Feature | Kestrel | IIS |
|---------|---------|-----|
| Platform | Cross-platform | Windows only |
| Performance | Very high | Good |
| Edge server | … | … |
| Process management | Manual / systemd | Built-in |
| Reverse proxy | Recommended pairing | Built-in |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Dependency Injection in .NET Core?

**Dependency Injection (DI)** is a design pattern where a class receives its dependencies from an external source (the DI container) rather than creating them itself. ASP.NET Core has a built-in DI container via `IServiceCollection`.

```cs
// 1. Define abstraction and implementation
public interface IEmailService
{
    Task SendAsync(string to, string subject, string body);
}

public class SmtpEmailService(IConfiguration config) : IEmailService
{
    public async Task SendAsync(string to, string subject, string body)
    {
        // Use config["Smtp:Host"] etc.
        Console.WriteLine($"Sending email to {to}: {subject}");
        await Task.CompletedTask;
    }
}

// 2. Register in DI container (Program.cs)
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddScoped<IEmailService, SmtpEmailService>();

// 3. Inject via constructor (C# 12 primary constructor)
public class OrderService(IEmailService emailService, ILogger<OrderService> logger)
{
    public async Task PlaceOrderAsync(Order order)
    {
        // ... business logic ...
        await emailService.SendAsync(order.CustomerEmail,
            "Order Confirmed", $"Order #{order.Id} confirmed.");
        logger.LogInformation("Order {OrderId} placed", order.Id);
    }
}

// 4. Inject into minimal API endpoints
app.MapPost("/orders", async (Order order, IOrderService svc) =>
{
    await svc.PlaceOrderAsync(order);
    return Results.Created($"/orders/{order.Id}", order);
});

// 5. Inject into controllers
[ApiController]
[Route("api/[controller]")]
public class OrdersController(IOrderService orderService) : ControllerBase
{
    [HttpPost]
    public async Task<IActionResult> Create(Order order)
    {
        await orderService.PlaceOrderAsync(order);
        return CreatedAtAction(nameof(GetById), new { id = order.Id }, order);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you add services to the dependency injection container?

```cs
var builder = WebApplication.CreateBuilder(args);
var services = builder.Services;

// 1. Lifetime registrations
services.AddTransient<IOrderService, OrderService>();   // new instance every request
services.AddScoped<ICartService, CartService>();         // one per HTTP request
services.AddSingleton<ICacheService, MemoryCacheService>(); // one per app lifetime

// 2. Register with factory (for complex construction)
services.AddScoped<IDbConnection>(_ =>
    new SqlConnection(builder.Configuration.GetConnectionString("Default")));

// 3. Register multiple implementations
services.AddScoped<INotifier, EmailNotifier>();
services.AddScoped<INotifier, SmsNotifier>();
// Inject IEnumerable<INotifier> to get all

// 4. Named/keyed services (.NET 8+)
services.AddKeyedScoped<IPaymentGateway, StripeGateway>("stripe");
services.AddKeyedScoped<IPaymentGateway, PayPalGateway>("paypal");
// Inject: ([FromKeyedServices("stripe")] IPaymentGateway gateway)

// 5. Options pattern
services.Configure<SmtpOptions>(builder.Configuration.GetSection("Smtp"));
// Inject: IOptions<SmtpOptions>, IOptionsSnapshot<T>, IOptionsMonitor<T>

// 6. Extension methods for clean grouping
services.AddApplicationServices(builder.Configuration);

// Extension method
public static class ServiceCollectionExtensions
{
    public static IServiceCollection AddApplicationServices(
        this IServiceCollection services, IConfiguration config)
    {
        services.AddScoped<IOrderService, OrderService>();
        services.AddScoped<IProductRepository, ProductRepository>();
        services.Configure<AppSettings>(config.GetSection("App"));
        return services;
    }
}

// 7. Auto-registration with scrutor
// dotnet add package Scrutor
services.Scan(scan => scan
    .FromAssemblyOf<IOrderService>()
    .AddClasses(c => c.AssignableTo(typeof(IRepository<>)))
    .AsImplementedInterfaces()
    .WithScopedLifetime());
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `IServiceCollection` and `IServiceProvider`?

| | `IServiceCollection` | `IServiceProvider` |
|-|---------------------|-------------------|
| **Role** | Registration — add/configure services | Resolution — retrieve service instances |
| **When used** | Startup / `Program.cs` (before `Build()`) | Runtime — after `Build()` |
| **Key methods** | `AddScoped`, `AddSingleton`, `AddTransient`, `Configure` | `GetService<T>`, `GetRequiredService<T>`, `CreateScope` |
| **Mutability** | Mutable — add services | Read-only — resolve services |

```cs
var builder = WebApplication.CreateBuilder(args);

// IServiceCollection — registration phase
IServiceCollection services = builder.Services;
services.AddScoped<IOrderService, OrderService>();
services.AddSingleton<IClock, SystemClock>();

var app = builder.Build();

// IServiceProvider — resolution phase (after Build)
IServiceProvider provider = app.Services;

// Resolve a singleton directly (rare — prefer injection)
var clock = provider.GetRequiredService<IClock>();
Console.WriteLine(clock.UtcNow);

// Resolve scoped service correctly — create a scope
using var scope = provider.CreateScope();
var orderService = scope.ServiceProvider.GetRequiredService<IOrderService>();
await orderService.ProcessAsync();

// GetService<T> vs GetRequiredService<T>
var optional = provider.GetService<IOptionalService>(); // null if not registered
var required = provider.GetRequiredService<IOrderService>(); // throws if not registered

// Anti-pattern: Service Locator — avoid in application code
// … Prefer constructor injection over IServiceProvider in services
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you configure logging in .NET Core?

.NET\'s built-in logging uses `ILogger<T>` / `ILoggerFactory` from `Microsoft.Extensions.Logging`. The default builder configures console, debug, and event source providers.

```cs
// Program.cs
var builder = WebApplication.CreateBuilder(args);

// Configure logging
builder.Logging
    .ClearProviders()                          // remove defaults
    .AddConsole()                              // console output
    .AddDebug()                                // VS Output window
    .AddEventSourceLogger()                    // ETW / dotnet-trace
    .SetMinimumLevel(LogLevel.Information);    // global minimum

// Per-category level from appsettings.json:
// "Logging": {
//   "LogLevel": { "Default": "Information", "Microsoft.AspNetCore": "Warning" }
// }

// Structured logging with Serilog (recommended for production)
// dotnet add package Serilog.AspNetCore
builder.Host.UseSerilog((ctx, cfg) =>
    cfg.ReadFrom.Configuration(ctx.Configuration)
       .WriteTo.Console(outputTemplate:
           "[{Timestamp:HH:mm:ss} {Level:u3}] {SourceContext}: {Message}{NewLine}{Exception}")
       .WriteTo.File("logs/app-.log", rollingInterval: RollingInterval.Day));

// Use ILogger<T> in services
public class OrderService(ILogger<OrderService> logger)
{
    public async Task PlaceOrderAsync(int orderId)
    {
        logger.LogInformation("Placing order {OrderId}", orderId);   // structured

        try
        {
            // ... work ...
            logger.LogInformation("Order {OrderId} placed successfully", orderId);
        }
        catch (Exception ex)
        {
            logger.LogError(ex, "Failed to place order {OrderId}", orderId);
            throw;
        }
    }
}

// Log levels (lowest ’ highest severity):
// Trace, Debug, Information, Warning, Error, Critical, None
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you perform database migrations in EF Core?

EF Core migrations track schema changes as versioned C# files and apply them to the database.

```bash
# Install EF Core tools
dotnet tool install -g dotnet-ef

# Add EF Core packages to project
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Design
```

```cs
// DbContext
public class AppDbContext(DbContextOptions<AppDbContext> options) : DbContext(options)
{
    public DbSet<Product> Products => Set<Product>();
    public DbSet<Order>   Orders   => Set<Order>();
}

public class Product
{
    public int Id { get; set; }
    public required string Name { get; set; }
    public decimal Price { get; set; }
}
```

```bash
# Create the initial migration
dotnet ef migrations add InitialCreate --output-dir Data/Migrations

# Apply migrations to the database
dotnet ef database update

# Add a new migration after model changes
dotnet ef migrations add AddOrderDate

# Roll back to a specific migration
dotnet ef database update InitialCreate

# Generate SQL script (for DBA review / production)
dotnet ef migrations script --output migration.sql --idempotent

# Remove last unapplied migration
dotnet ef migrations remove

# List all migrations and their status
dotnet ef migrations list
```

```cs
// Apply migrations programmatically at startup (dev/staging only)
var app = builder.Build();

using (var scope = app.Services.CreateScope())
{
    var db = scope.ServiceProvider.GetRequiredService<AppDbContext>();
    await db.Database.MigrateAsync(); // apply pending migrations
}

app.Run();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between `AddTransient`, `AddScoped`, and `AddSingleton`?

| Lifetime | Instance created | Shared within | Use for |
|----------|-----------------|---------------|---------|
| `AddTransient` | Every injection | Never shared | Lightweight, stateless services |
| `AddScoped` | Once per HTTP request (scope) | Same request | DB contexts, unit-of-work |
| `AddSingleton` | Once per application lifetime | Everyone | Caches, config, expensive shared state |

```cs
public interface ICounter { int Next(); }
public class Counter : ICounter
{
    private int _count;
    public int Next() => ++_count;
}

// Register all three lifetimes (for demo)
builder.Services.AddTransient<ICounter, Counter>();   // change to test others

// Controller demonstrating behavior
[ApiController, Route("api/[controller]")]
public class DemoController(
    ICounter counter1,
    ICounter counter2) : ControllerBase
{
    [HttpGet]
    public IActionResult Get()
    {
        return Ok(new
        {
            Counter1 = counter1.Next(),
            Counter2 = counter2.Next(),
            // Transient: counter1  counter2 (different instances)
            // Scoped: counter1 == counter2 (same instance within request)
            // Singleton: counter1 == counter2, and increments across requests
        });
    }
}

// Scoped services must NOT be injected into Singletons (captive dependency)
//  Singleton capturing Scoped ’ Scoped outlives its intended scope
builder.Services.AddSingleton<IBadSingleton, BadSingleton>(); // has IScoped inside — BAD
// … Use IServiceScopeFactory inside a singleton to create scopes manually
public class SafeSingleton(IServiceScopeFactory scopeFactory)
{
    public async Task DoWorkAsync()
    {
        using var scope = scopeFactory.CreateScope();
        var scoped = scope.ServiceProvider.GetRequiredService<IScopedService>();
        await scoped.WorkAsync();
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a RESTful API in ASP.NET Core?

```cs
// Program.cs — .NET 10 minimal API (recommended for new APIs)
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddDbContext<AppDbContext>(opt =>
    opt.UseInMemoryDatabase("Products"));
builder.Services.AddOpenApi();

var app = builder.Build();
app.MapOpenApi();

// Route group — /api/products
var products = app.MapGroup("/api/products").WithTags("Products");

products.MapGet("/", async (AppDbContext db) =>
    await db.Products.ToListAsync());

products.MapGet("/{id:int}", async (int id, AppDbContext db) =>
    await db.Products.FindAsync(id) is Product p
        ? Results.Ok(p)
        : Results.NotFound());

products.MapPost("/", async (Product product, AppDbContext db) =>
{
    db.Products.Add(product);
    await db.SaveChangesAsync();
    return Results.Created($"/api/products/{product.Id}", product);
});

products.MapPut("/{id:int}", async (int id, Product updated, AppDbContext db) =>
{
    var product = await db.Products.FindAsync(id);
    if (product is null) return Results.NotFound();
    product.Name  = updated.Name;
    product.Price = updated.Price;
    await db.SaveChangesAsync();
    return Results.NoContent();
});

products.MapDelete("/{id:int}", async (int id, AppDbContext db) =>
{
    var product = await db.Products.FindAsync(id);
    if (product is null) return Results.NotFound();
    db.Products.Remove(product);
    await db.SaveChangesAsync();
    return Results.NoContent();
});

app.Run();

public class Product
{
    public int Id { get; set; }
    public required string Name { get; set; }
    public decimal Price { get; set; }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you read configuration values from `appsettings.json`?

```json
// appsettings.json
{
  "App": {
    "Name": "MyApi",
    "MaxPageSize": 100,
    "FeatureFlags": {
      "EnableNewUI": true
    }
  },
  "ConnectionStrings": {
    "Default": "Server=localhost;Database=MyDb;Trusted_Connection=true"
  }
}
```

```cs
// 1. Raw IConfiguration (simple, no type safety)
var name = builder.Configuration["App:Name"];
var conn = builder.Configuration.GetConnectionString("Default");
var maxPage = builder.Configuration.GetValue<int>("App:MaxPageSize", defaultValue: 50);

// 2. Strongly-typed options (recommended)
public class AppOptions
{
    public string Name { get; set; } = "";
    public int MaxPageSize { get; set; }
    public FeatureFlagsOptions FeatureFlags { get; set; } = new();
}
public class FeatureFlagsOptions { public bool EnableNewUI { get; set; } }

builder.Services.Configure<AppOptions>(builder.Configuration.GetSection("App"));

// Inject and use
public class MyService(IOptionsSnapshot<AppOptions> opts)
{
    // IOptions<T> — singleton, does not update on file change
    // IOptionsSnapshot<T> — scoped, updates per request if reloadOnChange: true
    // IOptionsMonitor<T> — singleton, updates in real-time via OnChange callback
    public string AppName => opts.Value.Name;
    public bool NewUI     => opts.Value.FeatureFlags.EnableNewUI;
}

// 3. Bind directly
var appOptions = builder.Configuration
    .GetSection("App")
    .Get<AppOptions>()!;

// 4. Validate options on startup (.NET 8+ AddOptionsWithValidateOnStart)
builder.Services
    .AddOptions<AppOptions>()
    .Bind(builder.Configuration.GetSection("App"))
    .ValidateDataAnnotations()   // use [Required], [Range] on the class
    .ValidateOnStart();          // fail fast if config is invalid

// 5. Environment variable overrides (: ’ __ in env vars)
// App__Name=Override  ’  Config["App:Name"] = "Override"
// ASPNETCORE_ENVIRONMENT=Production
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use middleware in ASP.NET Core?

**Middleware** is software assembled into the request pipeline that handles requests and responses. Each component can short-circuit or pass to the next middleware via `next()`.

```cs
var app = builder.Build();

// Built-in middleware (order matters!)
app.UseExceptionHandler("/error");
app.UseHttpsRedirection();
app.UseStaticFiles();
app.UseRouting();
app.UseAuthentication();
app.UseAuthorization();
app.UseOutputCache();

// Inline middleware with app.Use
app.Use(async (context, next) =>
{
    var watch = System.Diagnostics.Stopwatch.StartNew();
    await next(context);  // call next middleware
    watch.Stop();
    var path = context.Request.Path;
    Console.WriteLine($"{context.Response.StatusCode} {path} — {watch.ElapsedMilliseconds}ms");
});

// Short-circuit middleware with app.Run (terminal — no next)
app.MapGet("/health", () => Results.Ok(new { Status = "Healthy" }));

// app.UseWhen — conditional branch
app.UseWhen(ctx => ctx.Request.Path.StartsWithSegments("/api"),
    branch => branch.Use(async (ctx, next) =>
    {
        ctx.Response.Headers["X-Api-Version"] = "v1";
        await next(ctx);
    }));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a custom middleware?

```cs
// 1. Class-based middleware (recommended — testable, DI-friendly)
public class RequestTimingMiddleware(RequestDelegate next,
    ILogger<RequestTimingMiddleware> logger)
{
    public async Task InvokeAsync(HttpContext context)
    {
        var sw = System.Diagnostics.Stopwatch.StartNew();

        await next(context);  // call rest of pipeline

        sw.Stop();
        logger.LogInformation("{Method} {Path} ’ {Status} in {Ms}ms",
            context.Request.Method,
            context.Request.Path,
            context.Response.StatusCode,
            sw.ElapsedMilliseconds);
    }
}

// Extension method for clean registration
public static class MiddlewareExtensions
{
    public static IApplicationBuilder UseRequestTiming(
        this IApplicationBuilder app) =>
        app.UseMiddleware<RequestTimingMiddleware>();
}

// Register in Program.cs
app.UseRequestTiming();

// 2. Middleware with scoped dependencies — use IMiddleware + factory pattern
public class AuditMiddleware(ILogger<AuditMiddleware> logger) : IMiddleware
{
    public async Task InvokeAsync(HttpContext context, RequestDelegate next)
    {
        logger.LogInformation("Audit: {Path}", context.Request.Path);
        await next(context);
    }
}

// Register as scoped (IMiddleware uses DI per request)
builder.Services.AddScoped<AuditMiddleware>();
app.UseMiddleware<AuditMiddleware>();

// 3. Exception-handling middleware example
public class GlobalExceptionMiddleware(RequestDelegate next, ILogger<GlobalExceptionMiddleware> logger)
{
    public async Task InvokeAsync(HttpContext context)
    {
        try
        {
            await next(context);
        }
        catch (Exception ex)
        {
            logger.LogError(ex, "Unhandled exception");
            context.Response.StatusCode = 500;
            context.Response.ContentType = "application/json";
            await context.Response.WriteAsJsonAsync(new
            {
                Error = "An unexpected error occurred.",
                TraceId = context.TraceIdentifier
            });
        }
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use dependency injection in controllers?

```cs
// Constructor injection (primary constructor syntax, C# 12 / .NET 8+)
[ApiController]
[Route("api/[controller]")]
public class OrdersController(
    IOrderService orderService,
    ILogger<OrdersController> logger) : ControllerBase
{
    [HttpGet]
    public async Task<IActionResult> GetAll()
    {
        var orders = await orderService.GetAllAsync();
        return Ok(orders);
    }

    [HttpGet("{id:int}")]
    public async Task<ActionResult<OrderDto>> GetById(int id)
    {
        var order = await orderService.GetByIdAsync(id);
        return order is null ? NotFound() : Ok(order);
    }

    [HttpPost]
    public async Task<IActionResult> Create(CreateOrderRequest request)
    {
        var order = await orderService.CreateAsync(request);
        logger.LogInformation("Order {OrderId} created", order.Id);
        return CreatedAtAction(nameof(GetById), new { id = order.Id }, order);
    }
}

// Inject from DI in action parameter — [FromServices]
[HttpGet("summary")]
public IActionResult GetSummary([FromServices] IReportService reportService)
    => Ok(reportService.GetSummary());

// Keyed services (.NET 8+)
[HttpPost("pay/stripe")]
public IActionResult PayWithStripe(
    [FromKeyedServices("stripe")] IPaymentGateway gateway,
    PaymentRequest request)
{
    gateway.Charge(request);
    return Ok();
}

// Register controller services
builder.Services.AddControllers();
builder.Services.AddScoped<IOrderService, OrderService>();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you return JSON from a controller action?

```cs
// 1. Minimal API — returns JSON automatically for objects
app.MapGet("/products", async (AppDbContext db) =>
    await db.Products.ToListAsync()); // serialized to JSON automatically

// 2. Controller — Ok() wraps object in 200 JSON response
[ApiController, Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    [HttpGet]
    public IActionResult GetAll() =>
        Ok(new[] { new { Id = 1, Name = "Laptop", Price = 999m } });

    // 3. Typed ActionResult<T>
    [HttpGet("{id:int}")]
    public ActionResult<ProductDto> GetById(int id)
    {
        var product = new ProductDto(id, "Laptop", 999m);
        return Ok(product); // 200 with JSON body
    }

    // 4. Custom JSON options for an action
    [HttpGet("formatted")]
    public IActionResult GetFormatted()
    {
        var data = new { Message = "Hello", Date = DateTime.UtcNow };
        return new JsonResult(data, new System.Text.Json.JsonSerializerOptions
        {
            WriteIndented = true,
            PropertyNamingPolicy = System.Text.Json.JsonNamingPolicy.CamelCase,
        });
    }
}

// 5. Configure global JSON options (.NET 10)
builder.Services.ConfigureHttpJsonOptions(opt =>
{
    opt.SerializerOptions.PropertyNamingPolicy = JsonNamingPolicy.CamelCase;
    opt.SerializerOptions.WriteIndented = false;
    opt.SerializerOptions.DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull;
});

// 6. Results.Json for minimal APIs with custom options
app.MapGet("/custom", () => Results.Json(
    new { Status = "ok" },
    new JsonSerializerOptions { WriteIndented = true }));

record ProductDto(int Id, string Name, decimal Price);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is attribute routing?

**Attribute routing** places route templates directly on controllers and actions using `[Route]`, `[HttpGet]`, `[HttpPost]`, etc. It gives fine-grained control over URL patterns and is the standard approach in ASP.NET Core Web APIs.

```cs
// Conventional routing (MVC) — defined globally in Program.cs
app.MapControllerRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Attribute routing — defined on the controller/action (preferred for APIs)
[ApiController]
[Route("api/v{version:apiVersion}/[controller]")] // token replacement
public class ProductsController : ControllerBase
{
    // GET api/v1/products
    [HttpGet]
    public IActionResult GetAll() => Ok();

    // GET api/v1/products/42
    [HttpGet("{id:int}")]
    public IActionResult GetById(int id) => Ok(id);

    // GET api/v1/products/sku/ABC-123
    [HttpGet("sku/{sku:regex(^[A-Z]{{3}}-\\d{{3}}$)}")]
    public IActionResult GetBySku(string sku) => Ok(sku);

    // POST api/v1/products
    [HttpPost]
    [ProducesResponseType<ProductDto>(StatusCodes.Status201Created)]
    [ProducesResponseType(StatusCodes.Status400BadRequest)]
    public IActionResult Create(CreateProductRequest request) => Created();

    // PUT api/v1/products/42
    [HttpPut("{id:int}")]
    public IActionResult Update(int id, UpdateProductRequest request) => NoContent();

    // DELETE api/v1/products/42
    [HttpDelete("{id:int}")]
    public IActionResult Delete(int id) => NoContent();

    // Multiple routes on one action
    [HttpGet("search")]
    [HttpGet("find")]           // both routes map here
    public IActionResult Search([FromQuery] string q) => Ok(q);
}

// Route constraints
// {id:int}           — integer only
// {name:alpha}       — letters only
// {code:length(5)}   — exactly 5 chars
// {date:datetime}    — valid datetime
// {price:decimal}    — decimal number
// {id:min(1)}        — minimum value
// {id:guid}          — GUID format
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a custom route?

```cs
// 1. Custom route constraint — restrict route parameter values
public class EvenNumberConstraint : IRouteConstraint
{
    public bool Match(HttpContext? context, IRouter? route, string routeKey,
        RouteValueDictionary values, RouteDirection routeDirection)
    {
        if (values.TryGetValue(routeKey, out var value)
            && int.TryParse(value?.ToString(), out int num))
            return num % 2 == 0;
        return false;
    }
}

// Register constraint
builder.Services.Configure<RouteOptions>(opt =>
    opt.ConstraintMap["even"] = typeof(EvenNumberConstraint));

// Use in route template
app.MapGet("/items/{id:even}", (int id) => $"Even item {id}");
// /items/2  ’ matches
// /items/3  ’ 404

// 2. Custom route in controller
[HttpGet("reports/{year:int:min(2000)}/{month:int:range(1,12)}")]
public IActionResult GetMonthlyReport(int year, int month) =>
    Ok(new { Year = year, Month = month });

// 3. Minimal API with complex route pattern
app.MapGet("/files/{**path}", (string path) => $"Requested: {path}");
// Catch-all: /files/docs/2026/report.pdf

// 4. Route groups with shared prefix and metadata
var v2 = app.MapGroup("/api/v2")
    .RequireAuthorization()
    .WithOpenApi()
    .AddEndpointFilter<ValidationFilter>();

v2.MapGet("/products", () => Results.Ok());
v2.MapPost("/products", (Product p) => Results.Created($"/api/v2/products/{p.Id}", p));

// 5. Custom route transformer (slug-case URLs)
public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string? TransformOutbound(object? value) =>
        value?.ToString() is string s
            ? System.Text.RegularExpressions.Regex.Replace(s, "([a-z])([A-Z])", "$1-$2").ToLower()
            : null;
}

builder.Services.Configure<RouteOptions>(opt =>
    opt.ConstraintMap["slugify"] = typeof(SlugifyParameterTransformer));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle form submissions in ASP.NET Core?

```cs
// 1. Minimal API — [FromForm] with IFormCollection
app.MapPost("/contact", async (IFormCollection form) =>
{
    var name    = form["name"].ToString();
    var email   = form["email"].ToString();
    var message = form["message"].ToString();
    return Results.Ok(new { name, email });
}).DisableAntiforgery(); // disable for API endpoints; enable for MVC forms

// 2. Strongly-typed form model
app.MapPost("/signup", async ([FromForm] SignupRequest req) =>
{
    Console.WriteLine($"Signup: {req.Name}, {req.Email}");
    return Results.Redirect("/welcome");
}).DisableAntiforgery();

public record SignupRequest([FromForm] string Name, [FromForm] string Email);

// 3. File upload via IFormFile
app.MapPost("/upload", async (IFormFile file) =>
{
    if (file.Length > 10 * 1024 * 1024)
        return Results.BadRequest("File too large (max 10 MB)");

    var ext = Path.GetExtension(file.FileName).ToLower();
    if (ext is not ".jpg" and not ".png" and not ".pdf")
        return Results.BadRequest("Invalid file type");

    var path = Path.Combine("uploads", Guid.NewGuid() + ext);
    await using var stream = File.Create(path);
    await file.CopyToAsync(stream);
    return Results.Ok(new { FilePath = path });
}).DisableAntiforgery();

// 4. MVC controller form handling with anti-forgery
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Create([FromForm] ProductFormModel model)
{
    if (!ModelState.IsValid)
        return View(model);

    await _service.CreateAsync(model);
    return RedirectToAction(nameof(Index));
}

// 5. Razor form with anti-forgery token
// @using Microsoft.AspNetCore.Mvc.Rendering
// <form method="post" action="/contact">
//   @Html.AntiForgeryToken()
//   <input name="name" />
//   <button type="submit">Submit</button>
// </form>
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is model binding?

**Model binding** is the process by which ASP.NET Core automatically maps HTTP request data (route values, query strings, form fields, JSON body, headers) to action method parameters.

```cs
// Sources (in binding order by default):
// 1. [FromRoute]   — /products/42  ’ id = 42
// 2. [FromQuery]   — ?page=2       ’ page = 2
// 3. [FromBody]    — JSON body     ’ complex object
// 4. [FromForm]    — form data
// 5. [FromHeader]  — request header
// 6. [FromServices]— DI container

[ApiController, Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    // Route + query binding — automatic
    [HttpGet("{id:int}")]
    public IActionResult Get(
        int id,                           // [FromRoute] implied
        [FromQuery] string? currency,     // ?currency=GBP
        [FromHeader(Name = "X-Tenant")] string? tenant) // from header
    {
        return Ok(new { id, currency, tenant });
    }

    // Body binding — JSON deserialized automatically ([ApiController] implies [FromBody])
    [HttpPost]
    public IActionResult Create(CreateProductRequest request) => Ok(request);

    // Mixed binding
    [HttpPut("{id:int}")]
    public IActionResult Update(
        [FromRoute] int id,
        [FromBody]  UpdateProductRequest body,
        [FromQuery] bool notify = false) => NoContent();
}

// Custom model binder — e.g., CSV to List<int>
public class CsvIntListBinder : IModelBinder
{
    public Task BindModelAsync(ModelBindingContext context)
    {
        var raw = context.ValueProvider.GetValue(context.ModelName).FirstValue;
        if (string.IsNullOrEmpty(raw))
        {
            context.Result = ModelBindingResult.Success(new List<int>());
            return Task.CompletedTask;
        }
        var list = raw.Split(',')
            .Select(s => int.TryParse(s.Trim(), out int n) ? (int?)n : null)
            .Where(n => n.HasValue).Select(n => n!.Value).ToList();
        context.Result = ModelBindingResult.Success(list);
        return Task.CompletedTask;
    }
}

// Use custom binder
[HttpGet("batch")]
public IActionResult GetBatch(
    [ModelBinder(typeof(CsvIntListBinder))] List<int> ids)
    => Ok(ids); // GET /batch?ids=1,2,3,4

record CreateProductRequest(string Name, decimal Price);
record UpdateProductRequest(string Name, decimal Price);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you validate models in ASP.NET Core?

```cs
// 1. Data annotations on model (automatic with [ApiController])
public class CreateProductRequest
{
    [Required(ErrorMessage = "Name is required")]
    [StringLength(100, MinimumLength = 2)]
    public string Name { get; set; } = "";

    [Range(0.01, 999999.99, ErrorMessage = "Price must be between 0.01 and 999,999.99")]
    public decimal Price { get; set; }

    [RegularExpression(@"^[A-Z]{3}$", ErrorMessage = "Currency must be 3 uppercase letters")]
    public string Currency { get; set; } = "GBP";
}

// [ApiController] automatically returns 400 if ModelState is invalid
[HttpPost]
public IActionResult Create(CreateProductRequest request)
{
    // ModelState.IsValid is guaranteed true here (ApiController handles it)
    return Ok(request);
}

// 2. Manual ModelState check (MVC controllers without [ApiController])
[HttpPost]
public IActionResult CreateMvc(CreateProductRequest request)
{
    if (!ModelState.IsValid)
        return BadRequest(ModelState);
    return Ok();
}

// 3. Custom validation attribute
public class FutureDateAttribute : ValidationAttribute
{
    protected override ValidationResult? IsValid(object? value, ValidationContext ctx)
    {
        if (value is DateTime date && date > DateTime.UtcNow)
            return ValidationResult.Success;
        return new ValidationResult("Date must be in the future");
    }
}

// 4. IValidatableObject — cross-property validation
public class OrderRequest : IValidatableObject
{
    public DateOnly StartDate { get; set; }
    public DateOnly EndDate { get; set; }

    public IEnumerable<ValidationResult> Validate(ValidationContext context)
    {
        if (EndDate <= StartDate)
            yield return new ValidationResult(
                "End date must be after start date",
                [nameof(EndDate)]);
    }
}

// 5. FluentValidation (popular alternative, .NET 10)
// dotnet add package FluentValidation.AspNetCore
public class ProductValidator : AbstractValidator<CreateProductRequest>
{
    public ProductValidator()
    {
        RuleFor(x => x.Name).NotEmpty().Length(2, 100);
        RuleFor(x => x.Price).GreaterThan(0).LessThan(1_000_000);
        RuleFor(x => x.Currency).Matches(@"^[A-Z]{3}$");
    }
}

builder.Services.AddValidatorsFromAssemblyContaining<ProductValidator>();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `ModelState` property?

`ModelState` is a `ModelStateDictionary` available on `ControllerBase` that contains the state of model binding and validation for the current request. It holds errors for each property and a boolean `IsValid` flag.

```cs
[HttpPost]
public IActionResult Create(CreateProductRequest request)
{
    // Check validity
    if (!ModelState.IsValid)
    {
        // Collect all errors
        var errors = ModelState
            .Where(e => e.Value?.Errors.Count > 0)
            .ToDictionary(
                e => e.Key,
                e => e.Value!.Errors.Select(err => err.ErrorMessage).ToArray()
            );
        return BadRequest(new { Errors = errors });
    }

    return Ok(request);
}

// Add errors manually
[HttpPost("custom")]
public IActionResult CustomValidation(CreateProductRequest request)
{
    if (request.Name == "forbidden")
        ModelState.AddModelError(nameof(request.Name), "This name is not allowed");

    if (!ModelState.IsValid)
        return ValidationProblem(ModelState); // RFC 7807 Problem Details format

    return Ok();
}

// ValidationProblem — returns standardised ProblemDetails (RFC 7807)
// {
//   "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",
//   "title": "One or more validation errors occurred.",
//   "status": 400,
//   "errors": { "Name": ["Name is required"] }
// }

// [ApiController] automatically calls ValidationProblem when ModelState is invalid
// — you don\'t need to check it manually for [ApiController] controllers
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use data annotations for validation?

```cs
using System.ComponentModel.DataAnnotations;

public class UserRegistration
{
    [Required(ErrorMessage = "Username is required")]
    [StringLength(50, MinimumLength = 3, ErrorMessage = "Username must be 3–50 characters")]
    public string Username { get; set; } = "";

    [Required]
    [EmailAddress(ErrorMessage = "Invalid email address")]
    public string Email { get; set; } = "";

    [Required]
    [MinLength(8, ErrorMessage = "Password must be at least 8 characters")]
    [RegularExpression(@"^(?=.*[A-Z])(?=.*\d).+$",
        ErrorMessage = "Password must contain an uppercase letter and a digit")]
    public string Password { get; set; } = "";

    [Compare(nameof(Password), ErrorMessage = "Passwords do not match")]
    public string ConfirmPassword { get; set; } = "";

    [Range(18, 120, ErrorMessage = "Age must be between 18 and 120")]
    public int Age { get; set; }

    [Url(ErrorMessage = "Invalid URL")]
    public string? Website { get; set; }

    [Phone(ErrorMessage = "Invalid phone number")]
    public string? Phone { get; set; }

    [DataType(DataType.Date)]
    public DateTime BirthDate { get; set; }

    [CreditCard(ErrorMessage = "Invalid credit card number")]
    public string? CardNumber { get; set; }
}

// Manual validation (outside ASP.NET pipeline)
var user = new UserRegistration { Username = "a", Email = "bad", Age = 15 };
var context = new ValidationContext(user);
var results = new List<ValidationResult>();

bool isValid = Validator.TryValidateObject(user, context, results, validateAllProperties: true);

if (!isValid)
    foreach (var r in results)
        Console.WriteLine($"{string.Join(", ", r.MemberNames)}: {r.ErrorMessage}");

// All common annotations:
// [Required]           — not null/empty
// [StringLength(n)]    — max (and optional min) length
// [MinLength(n)]       — min collection/string length
// [MaxLength(n)]       — max collection/string length
// [Range(min, max)]    — numeric/date range
// [RegularExpression]  — regex pattern
// [EmailAddress]       — email format
// [Url]                — URL format
// [Phone]              — phone format
// [Compare("Prop")]    — must equal another property
// [CreditCard]         — credit card format
// [EnumDataType]       — valid enum value
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `ViewResult` class?

`ViewResult` is an `ActionResult` that renders a **Razor view** (`.cshtml` file) as the HTTP response. It is returned by MVC controller actions that produce HTML.

```cs
// Controller returning a view
public class ProductsController : Controller  // Controller, not ControllerBase
{
    // View() returns ViewResult — renders Views/Products/Index.cshtml
    public IActionResult Index()
    {
        var products = new List<ProductViewModel>
        {
            new(1, "Laptop",  999m),
            new(2, "Monitor", 450m),
        };
        return View(products);              // passes model to the view
    }

    // Specify view name explicitly
    public IActionResult Details(int id)
    {
        var product = new ProductViewModel(id, "Laptop", 999m);
        return View("ProductDetails", product); // renders ProductDetails.cshtml
    }

    // ViewResult properties
    public IActionResult WithViewData()
    {
        ViewData["PageTitle"] = "Products";    // dynamic view data
        ViewBag.Count = 5;                     // dynamic property syntax

        var result = new ViewResult
        {
            ViewName = "Index",
            ViewData = ViewData,              // includes Model + ViewData
            StatusCode = 200,
        };
        return result;
    }
}

// Views/Products/Index.cshtml
@model List<ProductViewModel>
@{
    ViewData["Title"] = "Products";
}
<h1>Products (@Model.Count)</h1>
<ul>
    @foreach (var p in Model)
    {
        <li>@p.Name — @p.Price.ToString("C")</li>
    }
</ul>

record ProductViewModel(int Id, string Name, decimal Price);
```

**`ViewResult` vs other results:**

| Return | Use |
|--------|-----|
| `View()` | Render Razor `.cshtml` |
| `PartialView()` | Render partial view |
| `Json()` | Return JSON |
| `Redirect()` | HTTP 302 redirect |
| `File()` | Return file download |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you return a view from a controller action?

```cs
public class HomeController : Controller
{
    // 1. Default — renders Views/Home/Index.cshtml
    public IActionResult Index() => View();

    // 2. With model
    public IActionResult Products()
    {
        var model = new List<string> { "Laptop", "Mouse", "Monitor" };
        return View(model);
    }

    // 3. Explicit view name
    public IActionResult About() => View("AboutUs"); // Views/Home/AboutUs.cshtml

    // 4. View in different folder
    public IActionResult Shared() => View("~/Views/Shared/Info.cshtml");

    // 5. With ViewData / ViewBag
    public IActionResult Dashboard()
    {
        ViewData["Title"] = "Dashboard";
        ViewBag.UserId = 42;
        return View(new DashboardModel());
    }

    // 6. Conditional view
    public IActionResult Profile(int id)
    {
        var user = GetUser(id);
        if (user is null) return NotFound();
        return user.IsAdmin
            ? View("AdminProfile", user)
            : View("UserProfile", user);
    }
}
```

```cshtml
@* Views/Home/Products.cshtml *@
@model List<string>
@{
    ViewData["Title"] = "Products";
    Layout = "_Layout";      // use shared layout
}

<h1>@ViewData["Title"]</h1>
<ul>
    @foreach (var item in Model)
    {
        <li>@item</li>
    }
</ul>
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Razor syntax?

**Razor** is ASP.NET Core\'s server-side templating syntax that mixes C# and HTML using the `@` symbol as the transition character. Razor files have the `.cshtml` extension.

```cshtml
@* This is a Razor comment *@

@* Declare model type *@
@model List<Product>

@* Code block *@
@{
    ViewData["Title"] = "Product List";
    var count = Model.Count;
    string cssClass = count > 10 ? "many" : "few";
}

@* Inline expression — renders value *@
<h1>Products (@count)</h1>
<p class="@cssClass">Showing @Model.Count items</p>

@* Control flow *@
@if (Model.Any())
{
    <ul>
        @foreach (var product in Model)
        {
            <li>
                <strong>@product.Name</strong> — @product.Price.ToString("C")
                @if (product.Price > 500)
                {
                    <span class="badge">Premium</span>
                }
            </li>
        }
    </ul>
}
else
{
    <p>No products found.</p>
}

@* Explicit expression (multi-token) — use parentheses *@
<p>Tax: @(Model.Sum(p => p.Price) * 0.20m)</p>

@* Render raw HTML (avoid unless trusted content) *@
@Html.Raw("<strong>bold</strong>")

@* Tag helpers (preferred over HTML helpers) *@
<a asp-controller="Products" asp-action="Details" asp-route-id="@product.Id">
    View Details
</a>

@* Partial view *@
@await Html.PartialAsync("_ProductCard", product)

@* Section — inject content into layout *@
@section Scripts {
    <script src="~/js/products.js"></script>
}
```

```cshtml
@* Views/Shared/_Layout.cshtml — master layout *@
<!DOCTYPE html>
<html>
<head><title>@ViewData["Title"]</title></head>
<body>
    <nav>@await Html.PartialAsync("_Nav")</nav>
    @RenderBody()
    @await RenderSectionAsync("Scripts", required: false)
</body>
</html>
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a Razor view?

```bash
# 1. Create via dotnet CLI
dotnet new page -n Index -na MyApp.Pages      # Razor Page
# For MVC views — create .cshtml manually (no template)
```

```cshtml
@* Views/Products/Create.cshtml — MVC Razor view *@
@model CreateProductRequest
@{
    ViewData["Title"] = "Create Product";
    Layout = "~/Views/Shared/_Layout.cshtml";
}

<h2>Create Product</h2>

<form asp-action="Create" asp-controller="Products" method="post">
    @Html.AntiForgeryToken()

    <div asp-validation-summary="ModelOnly" class="text-danger"></div>

    <div class="form-group">
        <label asp-for="Name"></label>
        <input asp-for="Name" class="form-control" />
        <span asp-validation-for="Name" class="text-danger"></span>
    </div>

    <div class="form-group">
        <label asp-for="Price"></label>
        <input asp-for="Price" class="form-control" type="number" step="0.01" />
        <span asp-validation-for="Price" class="text-danger"></span>
    </div>

    <button type="submit" class="btn btn-primary">Create</button>
    <a asp-action="Index">Cancel</a>
</form>

@section Scripts {
    @{ await Html.RenderPartialAsync("_ValidationScriptsPartial"); }
}
```

```cs
// Controller action that returns this view
[HttpGet]
public IActionResult Create() => View();

[HttpPost, ValidateAntiForgeryToken]
public async Task<IActionResult> Create(CreateProductRequest model)
{
    if (!ModelState.IsValid) return View(model);
    await _service.CreateAsync(model);
    return RedirectToAction(nameof(Index));
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use partial views in ASP.NET Core?

**Partial views** are reusable `.cshtml` fragments rendered inside other views. They do not have a layout and are ideal for repeating UI components.

```cshtml
@* Views/Shared/_ProductCard.cshtml — partial view *@
@model Product

<div class="card">
    <div class="card-body">
        <h5 class="card-title">@Model.Name</h5>
        <p class="card-text">@Model.Price.ToString("C")</p>
        <a asp-action="Details" asp-route-id="@Model.Id"
           class="btn btn-primary">View</a>
    </div>
</div>
```

```cshtml
@* Parent view — render partial *@
@model List<Product>

@* 1. Tag helper (preferred, .NET Core) *@
@foreach (var product in Model)
{
    <partial name="_ProductCard" model="product" />
}

@* 2. await Html.PartialAsync (async, preferred in code) *@
@foreach (var product in Model)
{
    @await Html.PartialAsync("_ProductCard", product)
}

@* 3. Pass ViewData to partial *@
@await Html.PartialAsync("_ProductCard", product,
    new ViewDataDictionary(ViewData) { { "ShowBadge", true } })
```

```cs
// Return partial from controller (for AJAX requests)
[HttpGet("card/{id:int}")]
public async Task<IActionResult> GetCard(int id)
{
    var product = await _repo.GetByIdAsync(id);
    return PartialView("_ProductCard", product);
}

// Fetch partial via JavaScript (HTMX or fetch)
// fetch('/products/card/42').then(r => r.text()).then(html => div.innerHTML = html)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a view component?

A **View Component** is like a mini-controller + partial view, suitable for complex reusable UI sections (e.g., shopping cart, navigation menu, notification badge) that require business logic or dependency injection.

```cs
// 1. View Component class
using Microsoft.AspNetCore.Mvc;

public class CartSummaryViewComponent(ICartService cartService) : ViewComponent
{
    public async Task<IViewComponentResult> InvokeAsync()
    {
        var userId = HttpContext.User.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier)?.Value;
        var cart   = await cartService.GetCartAsync(userId ?? "guest");
        return View(cart); // renders Views/Shared/Components/CartSummary/Default.cshtml
    }
}

// ICartService
public interface ICartService
{
    Task<CartViewModel> GetCartAsync(string userId);
}
public record CartViewModel(int ItemCount, decimal Total);
```

```cshtml
@* Views/Shared/Components/CartSummary/Default.cshtml *@
@model CartViewModel

<div class="cart-badge">
    <span class="icon">’</span>
    <span class="count">@Model.ItemCount</span>
    <span class="total">@Model.Total.ToString("C")</span>
</div>
```

```cshtml
@* Use in any view *@
@* Tag helper syntax (recommended) *@
<vc:cart-summary></vc:cart-summary>

@* Method syntax *@
@await Component.InvokeAsync("CartSummary")

@* With parameters *@
<vc:recent-products count="5" category="Electronics"></vc:recent-products>
```

```cs
// View component with parameters
public class RecentProductsViewComponent(IProductRepository repo) : ViewComponent
{
    public async Task<IViewComponentResult> InvokeAsync(int count = 4, string? category = null)
    {
        var products = await repo.GetRecentAsync(count, category);
        return View(products);
    }
}

// Register services
builder.Services.AddScoped<ICartService, CartService>();
builder.Services.AddScoped<IProductRepository, ProductRepository>();
// View components are discovered automatically — no explicit registration needed
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a custom tag helper?

**Tag helpers** are C# classes that target HTML elements and transform them server-side. They are the Razor replacement for HTML helpers.

```cs
// 1. Simple custom tag helper — <alert type="success">message</alert>
using Microsoft.AspNetCore.Razor.TagHelpers;

[HtmlTargetElement("alert")]
public class AlertTagHelper : TagHelper
{
    public string Type { get; set; } = "info"; // maps to type attribute

    public override async Task ProcessAsync(TagHelperContext context, TagHelperOutput output)
    {
        var content = await output.GetChildContentAsync();

        output.TagName = "div";
        output.Attributes.SetAttribute("class", $"alert alert-{Type} alert-dismissible");
        output.Attributes.SetAttribute("role", "alert");
        output.Content.SetHtmlContent($"""
            {content.GetContent()}
            <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
            """);
    }
}

// Usage in Razor: <alert type="warning">Watch out!</alert>
// Renders: <div class="alert alert-warning alert-dismissible" role="alert">
//            Watch out!
//            <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
//          </div>

// 2. Tag helper targeting existing element — format bytes
[HtmlTargetElement("span", Attributes = "file-size")]
public class FileSizeTagHelper : TagHelper
{
    [HtmlAttributeName("file-size")]
    public long FileSizeBytes { get; set; }

    public override void Process(TagHelperContext context, TagHelperOutput output)
    {
        output.Attributes.RemoveAll("file-size");
        string formatted = FileSizeBytes switch
        {
            < 1024               => $"{FileSizeBytes} B",
            < 1024 * 1024        => $"{FileSizeBytes / 1024.0:F1} KB",
            < 1024 * 1024 * 1024 => $"{FileSizeBytes / (1024.0 * 1024):F1} MB",
            _                    => $"{FileSizeBytes / (1024.0 * 1024 * 1024):F1} GB",
        };
        output.Content.SetContent(formatted);
    }
}

// Usage: <span file-size="1536000"></span>  ’  <span>1.5 MB</span>

// 3. Register tag helpers in _ViewImports.cshtml
// @addTagHelper *, MyApp        — all tag helpers in MyApp assembly
// @addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers  — built-in
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does the ASP.NET Core middleware pipeline work?

The **middleware pipeline** is a chain of components that process HTTP requests and responses in order. Each middleware can call `next()` to pass control to the next component or short-circuit the pipeline.

```cs
//  Request flow ————————————————————————————————————————————————————
// Request ’ Middleware1 ’ Middleware2 ’ Middleware3 ’ Endpoint
//                                                         “
// Response  Middleware1  Middleware2  Middleware3  (response built)

//  Built-in middleware order matters ——————————————————————————————
var app = builder.Build();

app.UseExceptionHandler("/error"); // 1. Catch unhandled exceptions first
app.UseHsts();                     // 2. HTTPS security header
app.UseHttpsRedirection();         // 3. Redirect HTTP to HTTPS
app.UseStaticFiles();              // 4. Serve wwwroot before routing
app.UseRouting();                  // 5. Match route to endpoint
app.UseCors("MyPolicy");           // 6. CORS after routing, before auth
app.UseAuthentication();           // 7. Identify the user
app.UseAuthorization();            // 8. Enforce permissions
app.UseOutputCache();              // 9. Cache responses
app.MapControllers();              // 10. Execute controller endpoints

//  Writing custom middleware ———————————————————————————————————————
// Option A: Middleware class (recommended for reusability)
public class RequestTimingMiddleware(RequestDelegate next, ILogger<RequestTimingMiddleware> logger)
{
    public async Task InvokeAsync(HttpContext ctx)
    {
        var sw = System.Diagnostics.Stopwatch.StartNew();

        ctx.Response.OnStarting(() =>
        {
            ctx.Response.Headers["X-Response-Time"] = $"{sw.ElapsedMilliseconds}ms";
            return Task.CompletedTask;
        });

        await next(ctx); // call the next middleware

        sw.Stop();
        logger.LogInformation("{Method} {Path} ’ {StatusCode} in {Ms}ms",
            ctx.Request.Method, ctx.Request.Path,
            ctx.Response.StatusCode, sw.ElapsedMilliseconds);
    }
}

// Option B: Inline middleware (good for simple, one-off logic)
app.Use(async (ctx, next) =>
{
    ctx.Response.Headers["X-Powered-By"] = "ASP.NET Core 10";
    await next(ctx);
});

// Option C: Terminal middleware — does NOT call next (short-circuits)
app.Run(async ctx =>
{
    ctx.Response.StatusCode  = 200;
    ctx.Response.ContentType = "text/plain";
    await ctx.Response.WriteAsync("Hello from terminal middleware!");
});

// Registration
app.UseMiddleware<RequestTimingMiddleware>();

//  Conditional middleware ———————————————————————————————————————————
// Map — branch by path prefix
app.Map("/admin", adminApp =>
{
    adminApp.UseMiddleware<AdminAuthMiddleware>();
    adminApp.MapControllerRoute("admin", "{controller}/{action}");
});

// MapWhen — branch by custom predicate
app.MapWhen(
    ctx => ctx.Request.Headers.ContainsKey("X-Webhook-Signature"),
    webhookApp => webhookApp.UseMiddleware<WebhookVerificationMiddleware>());

// UseWhen — branch and REJOIN the pipeline (unlike MapWhen)
app.UseWhen(
    ctx => ctx.Request.Path.StartsWithSegments("/api"),
    apiApp => apiApp.UseMiddleware<ApiRateLimiterMiddleware>());

//  Middleware with scoped dependencies —————————————————————————————
// Inject scoped services via InvokeAsync parameters (not constructor)
public class AuditMiddleware(RequestDelegate next)
{
    // IMyService is scoped — cannot inject in constructor (middleware is singleton)
    public async Task InvokeAsync(HttpContext ctx, IAuditService auditService)
    {
        await next(ctx);
        if (ctx.User.Identity?.IsAuthenticated == true)
            await auditService.LogRequestAsync(ctx.Request.Path, ctx.User.Identity.Name!);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does the built-in DI container work in .NET Core?

The built-in **IoC container** (`IServiceCollection` / `IServiceProvider`) supports three service lifetimes and provides constructor injection throughout the application.

```cs
//  Service lifetimes ———————————————————————————————————————————————
// Singleton  — one instance for the entire application lifetime
// Scoped     — one instance per HTTP request (or explicit scope)
// Transient  — new instance every time it is requested

builder.Services.AddSingleton<IMemoryCacheService, MemoryCacheService>();
builder.Services.AddScoped<IOrderRepository, OrderRepository>();
builder.Services.AddTransient<IEmailSender, SmtpEmailSender>();

//  Registering with factory / implementation instance ——————————————
// Factory — called once (Singleton) or per request (Scoped/Transient)
builder.Services.AddScoped<IDbConnection>(sp =>
    new NpgsqlConnection(sp.GetRequiredService<IConfiguration>()
        .GetConnectionString("Default")));

// Pre-created instance (always Singleton)
builder.Services.AddSingleton<IConfiguration>(builder.Configuration);

//  Registering multiple implementations ———————————————————————————
builder.Services.AddScoped<INotificationHandler, EmailNotificationHandler>();
builder.Services.AddScoped<INotificationHandler, SmsNotificationHandler>();
builder.Services.AddScoped<INotificationHandler, PushNotificationHandler>();

// Inject all: IEnumerable<INotificationHandler>
public class NotificationService(IEnumerable<INotificationHandler> handlers)
{
    public async Task NotifyAllAsync(Notification n, CancellationToken ct)
    {
        var tasks = handlers.Select(h => h.HandleAsync(n, ct));
        await Task.WhenAll(tasks);
    }
}

//  Keyed services (.NET 8+) ————————————————————————————————————————
builder.Services.AddKeyedScoped<IPaymentGateway, StripeGateway>("stripe");
builder.Services.AddKeyedScoped<IPaymentGateway, PayPalGateway>("paypal");

// Resolve by key
public class CheckoutService(
    [FromKeyedServices("stripe")] IPaymentGateway stripe,
    [FromKeyedServices("paypal")] IPaymentGateway paypal)
{ }

//  Options pattern with DI ——————————————————————————————————————————
builder.Services.AddOptions<SmtpOptions>()
    .Bind(builder.Configuration.GetSection("Smtp"))
    .Validate(o => o.Port > 0 && o.Port < 65536, "Invalid SMTP port")
    .ValidateOnStart();

public class SmtpEmailSender(IOptions<SmtpOptions> opts)
{
    private readonly SmtpOptions _opts = opts.Value;
}

//  Avoiding captive dependency anti-pattern ————————————————————————
//  WRONG: Singleton captures Scoped service — Scoped lives too long
builder.Services.AddSingleton<OrderService>(sp =>
    new OrderService(sp.GetRequiredService<IOrderRepository>())); // scoped repo in singleton!

// … CORRECT: use IServiceScopeFactory to create explicit scopes in Singleton
public class BackgroundOrderProcessor(IServiceScopeFactory scopeFactory) : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken ct)
    {
        while (!ct.IsCancellationRequested)
        {
            using var scope = scopeFactory.CreateScope();
            var repo = scope.ServiceProvider.GetRequiredService<IOrderRepository>();
            await repo.ProcessPendingAsync(ct);
            await Task.Delay(TimeSpan.FromSeconds(30), ct);
        }
    }
}

//  Manual resolution (avoid — prefer constructor injection) ————————
using var scope = app.Services.CreateScope();
var dbContext   = scope.ServiceProvider.GetRequiredService<AppDbContext>();
await dbContext.Database.MigrateAsync();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement background services using IHostedService and BackgroundService?

`IHostedService` runs code when the host starts/stops. `BackgroundService` is a base class that simplifies long-running background work.

```cs
//  Option 1: Simple IHostedService —————————————————————————————————
public class DatabaseMigrationService(IServiceScopeFactory scopeFactory)
    : IHostedService
{
    public async Task StartAsync(CancellationToken ct)
    {
        using var scope = scopeFactory.CreateScope();
        var db = scope.ServiceProvider.GetRequiredService<AppDbContext>();
        await db.Database.MigrateAsync(ct);
        Console.WriteLine("Database migration complete.");
    }

    public Task StopAsync(CancellationToken ct) => Task.CompletedTask;
}

//  Option 2: BackgroundService — long-running loop —————————————————
public class OrderOutboxProcessor(
    IServiceScopeFactory scopeFactory,
    ILogger<OrderOutboxProcessor> logger) : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken ct)
    {
        logger.LogInformation("Outbox processor started.");

        while (!ct.IsCancellationRequested)
        {
            try
            {
                await ProcessBatchAsync(ct);
            }
            catch (OperationCanceledException)
            {
                break; // graceful shutdown
            }
            catch (Exception ex)
            {
                logger.LogError(ex, "Error processing outbox batch");
            }

            await Task.Delay(TimeSpan.FromSeconds(5), ct);
        }

        logger.LogInformation("Outbox processor stopped.");
    }

    private async Task ProcessBatchAsync(CancellationToken ct)
    {
        using var scope = scopeFactory.CreateScope();
        var db  = scope.ServiceProvider.GetRequiredService<AppDbContext>();
        var bus = scope.ServiceProvider.GetRequiredService<IPublishEndpoint>();

        var messages = await db.OutboxMessages
            .Where(m => m.ProcessedAt == null)
            .Take(20)
            .ToListAsync(ct);

        foreach (var msg in messages)
        {
            var type    = Type.GetType(msg.Type)!;
            var payload = System.Text.Json.JsonSerializer.Deserialize(msg.Payload, type)!;
            await bus.Publish(payload, type, ct);
            msg.ProcessedAt = DateTime.UtcNow;
        }

        await db.SaveChangesAsync(ct);
    }
}

//  Option 3: Timed background service ——————————————————————————————
public class CacheWarmupService(IServiceScopeFactory scopeFactory) : BackgroundService
{
    private readonly PeriodicTimer _timer = new(TimeSpan.FromMinutes(5));

    protected override async Task ExecuteAsync(CancellationToken ct)
    {
        // Run immediately on startup, then every 5 minutes
        await DoWorkAsync(ct);

        while (await _timer.WaitForNextTickAsync(ct))
            await DoWorkAsync(ct);
    }

    private async Task DoWorkAsync(CancellationToken ct)
    {
        using var scope = scopeFactory.CreateScope();
        var cache = scope.ServiceProvider.GetRequiredService<IProductCacheService>();
        await cache.WarmupAsync(ct);
    }

    public override void Dispose()
    {
        _timer.Dispose();
        base.Dispose();
    }
}

//  Registration ————————————————————————————————————————————————————
builder.Services.AddHostedService<DatabaseMigrationService>(); // runs at startup
builder.Services.AddHostedService<OrderOutboxProcessor>();
builder.Services.AddHostedService<CacheWarmupService>();

//  Worker Service — standalone background process ——————————————————
// dotnet new worker -n MyWorker
// Generates a minimal Host with BackgroundService — no HTTP stack
var builder = Host.CreateApplicationBuilder(args);
builder.Services.AddHostedService<MyWorker>();
builder.Services.AddSingleton<IMessageBusClient, RabbitMqClient>();

// Configure graceful shutdown timeout
builder.Services.Configure<HostOptions>(opts =>
    opts.ShutdownTimeout = TimeSpan.FromSeconds(30));

await builder.Build().RunAsync();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 21. MISCELLANEOUS

<br>

## Q. What is NuGet?

**NuGet** is the official package manager for .NET. It enables developers to create, share, and consume reusable libraries and tools. NuGet packages are ZIP files with the `.nupkg` extension containing compiled code (DLLs), related files, and a manifest that describes the package.

**Key features:**
- Centrally hosted on [nuget.org](https://www.nuget.org) (public) or private feeds (Azure Artifacts, GitHub Packages, etc.)
- Integrated into `dotnet` CLI, Visual Studio, and MSBuild
- Handles transitive dependencies automatically

```bash
# Install a package
dotnet add package Newtonsoft.Json

# Install a specific version
dotnet add package Microsoft.EntityFrameworkCore --version 9.0.0

# Remove a package
dotnet remove package Newtonsoft.Json

# Restore all packages
dotnet restore

# List outdated packages
dotnet list package --outdated

# Search NuGet
dotnet package search Serilog
```

**In .csproj (central package management, .NET 10):**

```xml
<!-- Directory.Packages.props — define versions once for the entire repo -->
<Project>
  <PropertyGroup>
    <ManagePackageVersionsCentrally>true</ManagePackageVersionsCentrally>
  </PropertyGroup>
  <ItemGroup>
    <PackageVersion Include="Serilog" Version="4.2.0" />
    <PackageVersion Include="System.Text.Json" Version="10.0.0" />
  </ItemGroup>
</Project>
```

```xml
<!-- Individual .csproj — just reference, no version needed -->
<ItemGroup>
  <PackageReference Include="Serilog" />
</ItemGroup>
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between ToString() and Convert.ToString()?

| Feature | `obj.ToString()` | `Convert.ToString(obj)` |
|---------|-----------------|------------------------|
| Null handling | Throws `NullReferenceException` | Returns `""` (empty string) |
| Defined on | `object` | `System.Convert` static class |
| Works on | Any object | Any base type + nullable |
| Overridable | … Yes |  No (calls ToString internally) |

```cs
string? s = null;

// ToString() — throws NullReferenceException on null
try
{
    string result = s!.ToString(); //  NullReferenceException
}
catch (NullReferenceException ex)
{
    Console.WriteLine($"Exception: {ex.Message}");
}

// Convert.ToString() — safe on null
string safe = Convert.ToString(s)!; // returns ""
Console.WriteLine($"Result: '{safe}'"); // Result: ''

// With value types — same result
int number = 42;
Console.WriteLine(number.ToString());          // "42"
Console.WriteLine(Convert.ToString(number));   // "42"

// Custom formatting — prefer ToString(format)
double pi = Math.PI;
Console.WriteLine(pi.ToString("F2"));          // "3.14"
Console.WriteLine(Convert.ToString(pi));       // "3.141592653589793"
```

**Best practice:** Use `ToString()` for non-nullable types. Use `Convert.ToString()` or null-conditional `?.ToString() ?? ""` when the object may be null.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between int.Parse() and Convert.ToInt32()?

| Feature | `int.Parse(s)` | `Convert.ToInt32(s)` |
|---------|---------------|---------------------|
| Input type | `string` only | Any base type (string, double, bool, etc.) |
| Null input | Throws `ArgumentNullException` | Returns `0` |
| Empty string | Throws `FormatException` | Throws `FormatException` |
| Invalid format | Throws `FormatException` | Throws `FormatException` |
| Performance | Slightly faster (no boxing) | Slightly slower |

```cs
// int.Parse — string only
Console.WriteLine(int.Parse("42"));     // 42
Console.WriteLine(int.Parse("-10"));    // -10

try { int.Parse(null!); }               //  ArgumentNullException
catch (ArgumentNullException) { Console.WriteLine("null throws!"); }

// Convert.ToInt32 — handles null
Console.WriteLine(Convert.ToInt32(null)); // 0 (no exception)
Console.WriteLine(Convert.ToInt32(3.9));  // 4 (rounds!)
Console.WriteLine(Convert.ToInt32(true)); // 1
Console.WriteLine(Convert.ToInt32(false));// 0

// Best practice — TryParse for user input (no exceptions)
string input = "abc";
if (int.TryParse(input, out int value))
    Console.WriteLine($"Parsed: {value}");
else
    Console.WriteLine("Invalid input");

// .NET 7+ — TryParse with generic NumberStyles
if (int.TryParse("FF", System.Globalization.NumberStyles.HexNumber,
    null, out int hex))
    Console.WriteLine(hex); // 255
```

**Rule:** Use `int.TryParse()` for user input. Use `int.Parse()` only when you are certain the string is a valid integer. Use `Convert.ToInt32()` when the source may be non-string types.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the use of Code Snippets?

**Code snippets** are predefined reusable templates for commonly typed code patterns. In Visual Studio, typing a shortcut and pressing **Tab twice** expands the snippet.

**Built-in C# snippets:**

| Shortcut | Expands to |
|---------|-----------|
| `cw` | `Console.WriteLine()` |
| `for` | `for` loop |
| `foreach` | `foreach` loop |
| `if` | `if` statement |
| `ctor` | Constructor |
| `prop` | Auto-property |
| `propg` | Get-only property |
| `try` | `try/catch` block |
| `switch` | `switch` statement |
| `class` | Class definition |
| `interface` | Interface definition |

```cs
// Typing 'prop' + Tab + Tab generates:
public int MyProperty { get; set; }

// Typing 'ctor' + Tab + Tab inside a class generates:
public ClassName()
{
}

// Typing 'foreach' + Tab + Tab generates:
foreach (var item in collection)
{
}
```

**Custom snippet file (.snippet XML):**

```xml
<?xml version="1.0" encoding="utf-8"?>
<CodeSnippets xmlns="http://schemas.microsoft.com/VisualStudio/2005/CodeSnippet">
  <CodeSnippet Format="1.0.0">
    <Header>
      <Title>Guard Clause</Title>
      <Shortcut>guard</Shortcut>
    </Header>
    <Snippet>
      <Code Language="CSharp">
        <![CDATA[ArgumentNullException.ThrowIfNull($param$);]]>
      </Code>
      <Declarations>
        <Literal><ID>param</ID><Default>value</Default></Literal>
      </Declarations>
    </Snippet>
  </CodeSnippet>
</CodeSnippets>
```

Import via **Tools ’ Code Snippets Manager ’ Import**.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Write a program to get the range of Byte Datatype?

```cs
// Byte (byte) — unsigned 8-bit integer
Console.WriteLine($"byte Min: {byte.MinValue}");   // 0
Console.WriteLine($"byte Max: {byte.MaxValue}");   // 255
Console.WriteLine($"byte Size: {sizeof(byte)} byte(s)");

// Signed byte (sbyte) — signed 8-bit integer
Console.WriteLine($"sbyte Min: {sbyte.MinValue}"); // -128
Console.WriteLine($"sbyte Max: {sbyte.MaxValue}"); // 127

// All numeric type ranges (.NET 10)
var types = new (string Name, long Min, ulong Max)[]
{
    ("byte",   byte.MinValue,   byte.MaxValue),
    ("sbyte",  sbyte.MinValue,  (ulong)sbyte.MaxValue),
    ("short",  short.MinValue,  (ulong)short.MaxValue),
    ("ushort", ushort.MinValue, ushort.MaxValue),
    ("int",    int.MinValue,    (ulong)int.MaxValue),
    ("uint",   uint.MinValue,   uint.MaxValue),
    ("long",   long.MinValue,   (ulong)long.MaxValue),
};

Console.WriteLine($"{"Type",-8} {"Min",22} {"Max",22}");
Console.WriteLine(new string('-', 55));
foreach (var (name, min, max) in types)
    Console.WriteLine($"{name,-8} {min,22} {max,22}");

// Overflow behavior
byte b = byte.MaxValue; // 255
b++;                    // overflow — wraps to 0
Console.WriteLine(b);   // 0

// Checked context — throws OverflowException
try
{
    byte c = checked((byte)(byte.MaxValue + 1));
}
catch (OverflowException)
{
    Console.WriteLine("Overflow caught!");
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are attributes in C# and how can they be used?

**Attributes** are metadata decorators applied to types, methods, properties, and assemblies using `[AttributeName]` syntax. They are inspected at runtime via reflection or at compile time by source generators/analyzers.

```cs
// 1. Built-in attributes
[Obsolete("Use NewMethod() instead", error: false)]
public void OldMethod() { }

[Serializable]
public class LegacyData { public int Value; }

// 2. Validation with [Required], [Range] (System.ComponentModel.DataAnnotations)
using System.ComponentModel.DataAnnotations;

public class Product
{
    [Required]
    [StringLength(100, MinimumLength = 2)]
    public string Name { get; set; } = "";

    [Range(0.01, 99999.99)]
    public decimal Price { get; set; }
}

// Validate manually
var p = new Product { Name = "", Price = -5 };
var context = new ValidationContext(p);
var results = new List<ValidationResult>();
bool valid = Validator.TryValidateObject(p, context, results, true);
foreach (var r in results)
    Console.WriteLine(r.ErrorMessage);

// 3. Custom attribute
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method,
    AllowMultiple = false, Inherited = true)]
public sealed class AuditAttribute(string author) : Attribute
{
    public string Author { get; } = author;
    public DateTime CreatedAt { get; } = DateTime.UtcNow;
}

[Audit("Alice")]
public class OrderService
{
    [Audit("Bob")]
    public void ProcessOrder() { }
}

// 4. Read attributes via reflection
var attr = typeof(OrderService)
    .GetCustomAttribute<AuditAttribute>();
Console.WriteLine(attr?.Author); // Alice

// 5. Conditional compilation attribute
[System.Diagnostics.Conditional("DEBUG")]
public static void DebugLog(string msg) =>
    Console.WriteLine($"[DEBUG] {msg}");

// Called only in DEBUG builds; no-op in Release
DebugLog("Processing started");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Why can\'t you specify the accessibility modifier for methods inside the interface?

In C#, **all interface members are implicitly `public`** by default, because an interface defines a contract — a promise that any implementing type exposes those members publicly. Restricting access would make the contract meaningless.

```cs
public interface IAnimal
{
    void Speak();     // implicitly public
    // private void Speak(); //  Compile error — cannot be private
    // protected void Speak(); //  Compile error
}

public class Dog : IAnimal
{
    public void Speak() => Console.WriteLine("Woof!"); // must be public
}
```

**Exception — C# 8+ Default Interface Methods (DIM):**

Since C# 8, interfaces *can* have `private` members, but only as helpers for default implementations:

```cs
public interface ILogger
{
    void Log(string message);

    // private helper — only usable inside the interface body
    private static string Format(string msg) => $"[{DateTime.UtcNow:u}] {msg}";

    // default implementation uses the private helper
    void LogInfo(string message) => Log(Format(message));
}

public class ConsoleLogger : ILogger
{
    public void Log(string message) => Console.WriteLine(message);
    // LogInfo is inherited from the interface default implementation
}

var logger = new ConsoleLogger();
logger.LogInfo("Application started");
// [2026-04-19 12:00:00Z] Application started
```

**Summary:** Interface members are public by design (the contract must be accessible). Only `private` and `static` members added in C# 8+ for default implementation support are allowed to restrict visibility — and they are internal to the interface body only.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement a custom IComparer<T> for sorting in C#?

`IComparer<T>` provides a `Compare(T x, T y)` method that returns negative (x < y), zero (x == y), or positive (x > y). Implement it when the default ordering doesn\'t fit your requirements.

```cs
record Product(string Name, decimal Price, int Stock);

// 1. Implement IComparer<T> as a class
public class ProductByPriceDescending : IComparer<Product>
{
    public int Compare(Product? x, Product? y)
    {
        if (x is null && y is null) return 0;
        if (x is null) return 1;
        if (y is null) return -1;
        return y.Price.CompareTo(x.Price); // descending
    }
}

var products = new List<Product>
{
    new("Laptop",  999m, 10),
    new("Mouse",    25m, 200),
    new("Monitor", 450m, 50),
};

products.Sort(new ProductByPriceDescending());
foreach (var p in products)
    Console.WriteLine($"{p.Name}: {p.Price:C}");
// Laptop: £999.00, Monitor: £450.00, Mouse: £25.00

// 2. Comparer<T>.Create — inline lambda (preferred for simple cases)
var byStockAsc = Comparer<Product>.Create((a, b) => a.Stock.CompareTo(b.Stock));
products.Sort(byStockAsc);
foreach (var p in products)
    Console.WriteLine($"{p.Name}: {p.Stock} units");

// 3. Multi-key sort — name length then alphabetical
var multiKey = Comparer<Product>.Create((a, b) =>
{
    int byLen = a.Name.Length.CompareTo(b.Name.Length);
    return byLen != 0 ? byLen : string.Compare(a.Name, b.Name, StringComparison.Ordinal);
});
products.Sort(multiKey);

// 4. Use with SortedSet<T>
var sortedSet = new SortedSet<Product>(new ProductByPriceDescending())
{
    new("Keyboard", 75m, 150),
    new("Webcam",  120m, 80),
};
foreach (var p in sortedSet)
    Console.WriteLine(p.Name);

// 5. LINQ OrderBy with IComparer<T>
var orderedByName = products
    .OrderBy(p => p.Name, StringComparer.OrdinalIgnoreCase)
    .ToList();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the use of conditional preprocessor directive in C#?

Conditional preprocessor directives instruct the compiler to include or exclude code blocks based on defined symbols. They are evaluated at **compile time**, so excluded code is never compiled.

```cs
// Define symbols in .csproj
// <DefineConstants>DEBUG;LOGGING</DefineConstants>
// Or from command line: dotnet build -p:DefineConstants=STAGING

// #if / #elif / #else / #endif
#if DEBUG
    Console.WriteLine("Debug build");
#elif STAGING
    Console.WriteLine("Staging build");
#else
    Console.WriteLine("Production build");
#endif

// #define and #undef (file-scope)
#define FEATURE_NEW_UI
#undef DEBUG

#if FEATURE_NEW_UI
    Console.WriteLine("New UI enabled");
#endif

// Practical: environment-specific configuration
public class AppConfig
{
    public static string ApiBaseUrl =>
#if DEBUG
        "https://localhost:5001";
#elif STAGING
        "https://staging.api.example.com";
#else
        "https://api.example.com";
#endif
}

// #warning and #error — compiler diagnostics
#if !NET10_0_OR_GREATER
#warning This code targets .NET 10+. Older runtimes may not work correctly.
#endif

// Preferred modern alternative — [Conditional] attribute
[System.Diagnostics.Conditional("DEBUG")]
static void Trace(string msg) => Console.WriteLine($"[TRACE] {msg}");

Trace("Starting..."); // compiled out in Release builds

// Target framework checks (built-in .NET symbols)
#if NET10_0_OR_GREATER
    Console.WriteLine("Running on .NET 10+");
#elif NET8_0
    Console.WriteLine("Running on .NET 8");
#endif
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are pointer types in C#?

**Pointer types** are variables that hold the memory address of another variable. They require the `unsafe` context and are primarily used for interoperability with unmanaged code or low-level performance optimizations.

```cs
// Enable unsafe code in .csproj
// <AllowUnsafeBlocks>true</AllowUnsafeBlocks>

unsafe
{
    // Pointer declaration and usage
    int x = 42;
    int* p = &x;           // p holds address of x

    Console.WriteLine(*p); // dereference — prints 42
    *p = 100;              // modify via pointer
    Console.WriteLine(x);  // 100

    // Pointer arithmetic
    int[] arr = [10, 20, 30, 40, 50];
    fixed (int* start = arr) // pin array in memory (prevent GC moving it)
    {
        int* current = start;
        for (int i = 0; i < arr.Length; i++, current++)
            Console.Write($"{*current} "); // 10 20 30 40 50
    }

    // Struct pointer
    var point = new System.Drawing.Point(3, 4);
    System.Drawing.Point* pp = &point;
    Console.WriteLine($"X={pp->X}, Y={pp->Y}"); // X=3, Y=4 (-> dereference)
}

// Span<T> and Memory<T> — safe zero-copy alternatives (preferred in .NET 10)
int[] data = [1, 2, 3, 4, 5];
Span<int> slice = data.AsSpan(1, 3); // [2, 3, 4] — no pointer needed
slice[0] = 99;
Console.WriteLine(data[1]); // 99 — modified original

// P/Invoke with pointers for unmanaged interop
using System.Runtime.InteropServices;

[DllImport("msvcrt.dll", CallingConvention = CallingConvention.Cdecl)]
static extern unsafe int memcmp(void* b1, void* b2, nint count);

unsafe
{
    byte[] a = [1, 2, 3];
    byte[] b = [1, 2, 3];
    fixed (byte* pa = a, pb = b)
        Console.WriteLine(memcmp(pa, pb, a.Length) == 0 ? "Equal" : "Different");
}
```

**When to use:** P/Invoke interop, embedded/systems programming, performance-critical buffer manipulation. For most scenarios, prefer `Span<T>`, `Memory<T>`, or `Marshal` class.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is marshalling and why do we need it?

**Marshalling** is the process of transforming data types between managed (.NET) code and unmanaged (native/COM) code. The .NET runtime automatically marshals simple types, but complex types need explicit configuration.

**Why we need it:** Managed memory is controlled by the GC (objects can be moved/collected). Unmanaged code operates with raw memory pointers. Marshalling bridges this gap safely.

```cs
using System.Runtime.InteropServices;

// 1. Simple P/Invoke — primitives marshalled automatically
[DllImport("kernel32.dll", SetLastError = true)]
static extern uint GetCurrentThreadId();

Console.WriteLine($"Thread ID: {GetCurrentThreadId()}");

// 2. String marshalling
[DllImport("kernel32.dll", CharSet = CharSet.Unicode, SetLastError = true)]
static extern bool GetComputerName(
    System.Text.StringBuilder lpBuffer,
    ref uint nSize);

var sb = new System.Text.StringBuilder(256);
uint size = 256u;
GetComputerName(sb, ref size);
Console.WriteLine($"Computer: {sb}");

// 3. Struct marshalling — layout must match native struct
[StructLayout(LayoutKind.Sequential)]
public struct SystemTime
{
    public ushort Year, Month, DayOfWeek, Day;
    public ushort Hour, Minute, Second, Milliseconds;
}

[DllImport("kernel32.dll")]
static extern void GetSystemTime(out SystemTime st);

GetSystemTime(out SystemTime time);
Console.WriteLine($"{time.Year}-{time.Month:D2}-{time.Day:D2} {time.Hour:D2}:{time.Minute:D2}");

// 4. Marshal class — manual marshalling
IntPtr ptr = Marshal.AllocHGlobal(100);     // allocate unmanaged memory
try
{
    Marshal.WriteInt32(ptr, 42);
    int value = Marshal.ReadInt32(ptr);
    Console.WriteLine(value);               // 42
}
finally
{
    Marshal.FreeHGlobal(ptr);               // always free unmanaged memory
}

// 5. Modern alternative — LibraryImport (.NET 7+, source-generated, AOT-compatible)
public partial class NativeMethods
{
    [LibraryImport("kernel32.dll")]
    public static partial uint GetCurrentProcessId();
}
Console.WriteLine($"Process ID: {NativeMethods.GetCurrentProcessId()}");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How to calculate the code execution time in C#?

```cs
using System.Diagnostics;

// 1. Stopwatch — most precise, recommended
var sw = Stopwatch.StartNew();

// Code to measure
long sum = 0;
for (int i = 0; i < 10_000_000; i++) sum += i;

sw.Stop();
Console.WriteLine($"Elapsed: {sw.ElapsedMilliseconds} ms");
Console.WriteLine($"Elapsed: {sw.Elapsed.TotalMicroseconds:F0} s");
Console.WriteLine($"Sum: {sum}");

// 2. Measure a specific block with a helper
static T Measure<T>(string label, Func<T> action)
{
    var sw = Stopwatch.StartNew();
    T result = action();
    sw.Stop();
    Console.WriteLine($"{label}: {sw.Elapsed.TotalMilliseconds:F3} ms");
    return result;
}

var result = Measure("Enumerable.Sum", () =>
    Enumerable.Range(0, 10_000_000).Sum(x => (long)x));

// 3. High-resolution timestamp (nanoseconds, .NET 7+)
long start = Stopwatch.GetTimestamp();
// ... work ...
long end = Stopwatch.GetTimestamp();
double nanoseconds = (end - start) * 1_000_000_000.0 / Stopwatch.Frequency;
Console.WriteLine($"Elapsed: {nanoseconds:F0} ns");

// 4. BenchmarkDotNet — production-grade micro-benchmarking
// Install: dotnet add package BenchmarkDotNet
/*
[MemoryDiagnoser]
public class MyBenchmarks
{
    [Benchmark]
    public long LinqSum() => Enumerable.Range(0, 10_000_000).Sum(x => (long)x);

    [Benchmark]
    public long LoopSum()
    {
        long s = 0;
        for (int i = 0; i < 10_000_000; i++) s += i;
        return s;
    }
}
BenchmarkRunner.Run<MyBenchmarks>();
*/

// 5. ActivitySource — distributed tracing (.NET 5+)
using var activity = new System.Diagnostics.ActivitySource("MyApp")
    .StartActivity("ComputeSum");
activity?.SetTag("iterations", 1_000_000);
// ... work ...
activity?.Stop();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the distinction between DirectCast and CType?

`DirectCast` and `CType` are **VB.NET** operators. In C# the equivalents are **direct cast** `(T)obj` and **`Convert` / `as` / `is`** expressions.

| VB.NET | C# Equivalent | Behavior |
|--------|--------------|---------|
| `DirectCast(obj, T)` | `(T)obj` | Requires exact or inheritance relationship; throws `InvalidCastException` on failure |
| `CType(obj, T)` | `Convert.ToT(obj)` or operator | Performs data conversion (e.g., `double` ’ `int`); wider compatibility |
| `TryCast(obj, T)` | `obj as T` | Returns `null` on failure; reference types only |

```cs
// C# explicit cast ( DirectCast) — requires compatible types
object obj = "Hello";
string s = (string)obj;  // … succeeds
// int n = (int)obj;     //  InvalidCastException at runtime

// C# Convert ( CType) — performs data conversion
double d = 3.9;
int i = (int)d;               // truncates ’ 3
int j = Convert.ToInt32(d);   // rounds    ’ 4
string str = Convert.ToString(123); // int ’ string

// C# 'as' ( TryCast) — null on failure, reference types
object value = 42;
string? result = value as string; // null (not a string)
Console.WriteLine(result is null); // True

// C# 'is' pattern matching — recommended modern approach
object item = "World";
if (item is string text && text.Length > 3)
    Console.WriteLine(text.ToUpper()); // WORLD

// Switch expression with type patterns
object data = 3.14;
string description = data switch
{
    int n    => $"Integer: {n}",
    double d => $"Double: {d:F2}",
    string s => $"String: {s}",
    _        => "Unknown"
};
Console.WriteLine(description); // Double: 3.14
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain how to implement a custom serializer and deserializer for a complex object in C#?

`System.Text.Json` (.NET 10) is the recommended serialization library. For complex or non-standard types, implement a custom `JsonConverter<T>`.

```cs
using System.Text.Json;
using System.Text.Json.Serialization;

// Complex type to serialize
public record Money(decimal Amount, string Currency)
{
    public override string ToString() => $"{Amount:F2} {Currency}";
}

public class Order
{
    public int Id { get; init; }
    public Money Total { get; init; } = new(0, "GBP");
    public DateOnly Date { get; init; }
}

// Custom converter for Money — serialize as "99.99 GBP"
public class MoneyConverter : JsonConverter<Money>
{
    public override Money Read(ref Utf8JsonReader reader, Type typeToConvert,
        JsonSerializerOptions options)
    {
        var raw = reader.GetString()
            ?? throw new JsonException("Expected a string for Money");

        var parts = raw.Split(' ', 2);
        if (parts.Length != 2 || !decimal.TryParse(parts[0], out decimal amount))
            throw new JsonException($"Invalid Money format: '{raw}'");

        return new Money(amount, parts[1]);
    }

    public override void Write(Utf8JsonWriter writer, Money value,
        JsonSerializerOptions options)
    {
        writer.WriteStringValue($"{value.Amount:F2} {value.Currency}");
    }
}

// Configure options
var options = new JsonSerializerOptions
{
    WriteIndented = true,
    Converters = { new MoneyConverter() },
    PropertyNamingPolicy = JsonNamingPolicy.CamelCase,
};

var order = new Order
{
    Id   = 1001,
    Total = new Money(249.99m, "GBP"),
    Date  = new DateOnly(2026, 4, 19),
};

// Serialize
string json = JsonSerializer.Serialize(order, options);
Console.WriteLine(json);
// {
//   "id": 1001,
//   "total": "249.99 GBP",
//   "date": "2026-04-19"
// }

// Deserialize
var restored = JsonSerializer.Deserialize<Order>(json, options);
Console.WriteLine(restored?.Total); // 249.99 GBP

// Source-generated serializer (.NET 6+ — AOT & trimming friendly)
[JsonSourceGenerationOptions(WriteIndented = true,
    PropertyNamingPolicy = JsonKnownNamingPolicy.CamelCase)]
[JsonSerializable(typeof(Order))]
public partial class AppJsonContext : JsonSerializerContext { }

string json2 = JsonSerializer.Serialize(order, AppJsonContext.Default.Order);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Explain the differences between Memory<T> and Span<T> in C#. When would you use one over the other?

Both provide **zero-copy, allocation-free views** over contiguous memory (arrays, strings, native memory), but they differ in where they can live.

| Feature | `Span<T>` | `Memory<T>` |
|---------|-----------|------------|
| Allocation | Stack only (ref struct) | Stack or heap |
| Use in `async` methods |  Cannot cross `await` | … Safe across `await` |
| Use as class field |  | … |
| Use in lambdas/closures |  | … |
| Convert to Span | N/A | `.Span` property |
| Performance | Slightly faster | Slight overhead |
| Best for | Synchronous, local processing | Async pipelines, fields |

```cs
// Span<T> — synchronous, stack-only
void ProcessSync(Span<byte> buffer)
{
    for (int i = 0; i < buffer.Length; i++)
        buffer[i] ^= 0xFF; // in-place XOR — no allocation

    Console.WriteLine($"Processed {buffer.Length} bytes");
}

byte[] data = [0x01, 0x02, 0x03];
ProcessSync(data.AsSpan());         // array ’ Span
ProcessSync(stackalloc byte[4]);    // stack memory ’ Span
Console.WriteLine(string.Join(",", data)); // 254,253,252

// ReadOnlySpan<T> — zero-copy string slicing
ReadOnlySpan<char> greeting = "Hello, World!".AsSpan();
ReadOnlySpan<char> hello    = greeting[..5];  // "Hello" — no allocation
Console.WriteLine(hello.ToString());

// Memory<T> — safe across await
async Task ProcessAsync(Memory<byte> buffer)
{
    // Can hold Memory<T> across await (Span<T> cannot)
    await Task.Delay(10);
    buffer.Span[0] = 42; // access via .Span when needed synchronously
}

byte[] asyncData = new byte[10];
await ProcessAsync(asyncData.AsMemory());
Console.WriteLine(asyncData[0]); // 42

// Memory<T> as a class field — Span<T> cannot be a field
public class DataProcessor
{
    private readonly Memory<byte> _buffer; // … Memory<T> as field

    public DataProcessor(byte[] data) => _buffer = data.AsMemory();

    public async Task RunAsync()
    {
        await Task.Yield(); // simulate async work
        _buffer.Span.Fill(0xFF); // zero all bytes
    }
}

// MemoryPool<T> — reusable memory without GC pressure
using System.Buffers;
using IMemoryOwner<byte> owner = MemoryPool<byte>.Shared.Rent(1024);
Memory<byte> rented = owner.Memory;
rented.Span.Fill(0);
Console.WriteLine($"Rented: {rented.Length} bytes");
// owner disposed ’ memory returned to pool
```

**Decision:** Use `Span<T>` for synchronous, in-method processing. Use `Memory<T>` when you need to store the view across async boundaries or as a field.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a DTO?

A **DTO (Data Transfer Object)** is a simple object used to transfer data between layers or across process/network boundaries. DTOs contain only data (properties) — no business logic, no methods, no validation.

**Purpose:**
- Decouple API contracts from domain models
- Reduce over-posting / under-posting in APIs
- Shape data for specific consumers (e.g., a mobile app response)

```cs
// Domain model — rich, with business logic
public class Order
{
    public int Id { get; private set; }
    public string CustomerId { get; private set; } = "";
    public List<OrderLine> Lines { get; } = [];
    private decimal _discount;

    public decimal Total => Lines.Sum(l => l.Subtotal) * (1 - _discount);
    public void ApplyDiscount(decimal pct) { /* validation logic */ }
}

// DTO — thin, serialization-friendly
public record OrderSummaryDto(
    int Id,
    string CustomerId,
    decimal Total,
    int ItemCount,
    DateTimeOffset CreatedAt);

// Mapping — manually or via AutoMapper / Mapperly
public static class OrderMapper
{
    public static OrderSummaryDto ToDto(Order order) => new(
        order.Id,
        order.CustomerId,
        order.Total,
        order.Lines.Count,
        DateTimeOffset.UtcNow);
}

// ASP.NET Core minimal API — return DTO, not domain model
app.MapGet("/orders/{id}", (int id, IOrderRepository repo) =>
{
    var order = repo.GetById(id);
    return order is null
        ? Results.NotFound()
        : Results.Ok(OrderMapper.ToDto(order));
});

// Request DTO — controls what clients can send (anti-over-posting)
public record CreateOrderRequest(
    string CustomerId,
    IReadOnlyList<OrderLineRequest> Lines);

public record OrderLineRequest(string ProductId, int Quantity);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What does POCO mean?

**POCO** stands for **Plain Old CLR Object** (or **Plain Old C# Object**). A POCO is a simple class that:
- Has no dependency on any framework base class or interface
- Contains only properties and possibly simple methods
- Is not tied to a specific persistence or serialization framework

```cs
// … POCO — no framework dependency
public class Customer
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
    public string Email { get; set; } = "";
    public DateTime CreatedAt { get; set; }
}

// … POCO record (C# 9+) — immutable POCO
public record ProductPoco(int Id, string Name, decimal Price);

//  Not a POCO — inherits from framework class
public class LegacyController : System.Web.Mvc.Controller { }

// POCOs work with EF Core without inheriting DbContext entities
// EF Core maps POCOs via convention or configuration
public class AppDbContext : DbContext
{
    public DbSet<Customer> Customers => Set<Customer>();
}

// POCOs work with System.Text.Json without attributes
var customer = new Customer
{
    Id = 1,
    Name = "Alice",
    Email = "alice@example.com",
    CreatedAt = DateTime.UtcNow,
};

string json = JsonSerializer.Serialize(customer);
Console.WriteLine(json);
// {"Id":1,"Name":"Alice","Email":"alice@example.com","CreatedAt":"..."}

var restored = JsonSerializer.Deserialize<Customer>(json);
Console.WriteLine(restored?.Name); // Alice
```

**POCO vs DTO:** A POCO is a design principle (framework-free class). A DTO is a pattern (data carrier between layers). A DTO is usually a POCO, but not all POCOs are DTOs.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Define Parsing? Explain how to Parse a DateTime String?

**Parsing** is the process of converting a string representation into a typed value. For `DateTime`/`DateTimeOffset`, .NET provides several methods with different trade-offs.

```cs
using System.Globalization;

// 1. DateTime.Parse — lenient, throws on failure
DateTime dt1 = DateTime.Parse("2026-04-19");
DateTime dt2 = DateTime.Parse("April 19, 2026");
Console.WriteLine(dt1); // 4/19/2026 12:00:00 AM

// 2. DateTime.TryParse — safe, no exception
if (DateTime.TryParse("2026-04-19 14:30:00", out DateTime result))
    Console.WriteLine(result); // 4/19/2026 2:30:00 PM

// 3. DateTime.ParseExact — strict format, throws on mismatch
DateTime exact = DateTime.ParseExact(
    "19/04/2026 14:30",
    "dd/MM/yyyy HH:mm",
    CultureInfo.InvariantCulture);
Console.WriteLine(exact);

// 4. DateTime.TryParseExact — strict + safe
bool ok = DateTime.TryParseExact(
    "19-04-2026",
    "dd-MM-yyyy",
    CultureInfo.InvariantCulture,
    DateTimeStyles.None,
    out DateTime strict);
Console.WriteLine($"Parsed: {ok}, Value: {strict:yyyy-MM-dd}");

// 5. Multiple format candidates
string[] formats = ["dd/MM/yyyy", "MM-dd-yyyy", "yyyy.MM.dd"];
if (DateTime.TryParseExact("2026.04.19", formats,
    CultureInfo.InvariantCulture, DateTimeStyles.None, out DateTime multi))
    Console.WriteLine(multi);

// 6. DateOnly / TimeOnly (.NET 6+) — preferred for date/time-only values
DateOnly date = DateOnly.Parse("2026-04-19");
TimeOnly time = TimeOnly.Parse("14:30:00");
Console.WriteLine($"{date}, {time}");

DateOnly.TryParseExact("19/04/2026", "dd/MM/yyyy",
    CultureInfo.InvariantCulture, DateTimeStyles.None, out DateOnly d);
Console.WriteLine(d); // 4/19/2026

// 7. DateTimeOffset — timezone-aware (preferred for APIs)
DateTimeOffset dto = DateTimeOffset.Parse("2026-04-19T14:30:00+05:30");
Console.WriteLine(dto.ToUniversalTime()); // converted to UTC

// 8. ISO 8601 round-trip format ("O")
string iso = DateTime.UtcNow.ToString("O");
DateTime restored = DateTime.Parse(iso, null, DateTimeStyles.RoundtripKind);
Console.WriteLine(restored.Kind); // Utc
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is IL (Intermediate Language) Code?

**IL (Intermediate Language)**, also called **CIL (Common Intermediate Language)** or historically **MSIL (Microsoft Intermediate Language)**, is the CPU-independent bytecode that .NET compilers (C#, F#, VB.NET) produce. It is not machine code — the JIT compiler converts IL to native machine code at runtime.

```
Source Code (C#/F#/VB)
       “ compile
  IL (.dll / .exe)           platform-independent
       “ JIT / AOT
Native Machine Code          platform-specific (x64, ARM64, etc.)
```

**Example — C# and its IL:**

```cs
// C# source
public static int Add(int a, int b) => a + b;
```

```il
// Corresponding IL (viewed in ILDASM or ILSpy)
.method public hidebysig static int32 Add(int32 a, int32 b) cil managed
{
    .maxstack 2
    ldarg.0      // push a
    ldarg.1      // push b
    add          // pop both, push sum
    ret          // return
}
```

**View IL in practice:**

```bash
# ILSpy CLI
dotnet tool install -g ilspycmd
ilspycmd MyApp.dll --il

# Built-in ILDASM (Windows .NET SDK)
ildasm MyApp.dll /output:MyApp.il

# dotnet-ildasm
dotnet tool install -g dotnet-ildasm
dotnet-ildasm MyApp.dll
```

**Characteristics of IL:**
- Stack-based virtual machine instructions
- Strongly typed (includes type information)
- Verifiable — the runtime checks type safety before execution
- Enables cross-language interoperability (all .NET languages target IL)
- Inspectable — you can decompile `.dll` back to C# using ILSpy or dotPeek

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the use of JIT (Just-in-Time compiler)?

The **JIT (Just-in-Time) compiler** is part of the .NET CLR. It converts IL (Intermediate Language) bytecode into native machine code **on demand** — the first time each method is called — and caches the result for subsequent calls.

**JIT compilation pipeline:**

```
IL bytecode ’ [JIT Compiler] ’ Native x64/ARM64 code ’ CPU execution
                  
         (first call only — cached after)
```

```cs
// Demonstrate JIT behavior with RuntimeHelpers
using System.Runtime.CompilerServices;

// Force JIT compilation of a method before timing it
RuntimeHelpers.PrepareMethod(typeof(Program)
    .GetMethod(nameof(HotPath))!.MethodHandle);

static long HotPath()
{
    long sum = 0;
    for (int i = 0; i < 10_000_000; i++) sum += i;
    return sum;
}

// [MethodImpl] hints to the JIT
[MethodImpl(MethodImplOptions.AggressiveInlining)]
static int FastAdd(int a, int b) => a + b; // JIT will inline this call

[MethodImpl(MethodImplOptions.AggressiveOptimization)]
static void HeavyLoop()
{
    // JIT uses Tier-2 optimizations immediately (skip warm-up)
    for (int i = 0; i < 100_000; i++) { /* work */ }
}

[MethodImpl(MethodImplOptions.NoInlining)]
static void AlwaysCallStack() { } // prevents inlining (useful for profiling)
```

**JIT tiers (.NET 6+):**

| Tier | Trigger | Optimization |
|------|---------|-------------|
| Tier 0 | First call | Minimal (fast compilation) |
| Tier 1 | After ~30 calls | Full optimizations (inlining, unrolling, vectorization) |

**AOT (Ahead-of-Time) — .NET 7+ alternative to JIT:**

```bash
# Publish as native AOT — compiles IL to machine code at build time
dotnet publish -r win-x64 -p:PublishAot=true
# Result: single native .exe, no JIT at runtime, faster startup
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Is it possible to view IL code?

**Yes.** Multiple tools can inspect IL from any .NET assembly (`.dll`/`.exe`).

```bash
# 1. ILSpy CLI — cross-platform, decompiles back to C# or raw IL
dotnet tool install -g ilspycmd
ilspycmd MyApp.dll --il               # raw IL output
ilspycmd MyApp.dll -l CSharp          # decompile to C#

# 2. dotnet-ildasm — .NET global tool
dotnet tool install -g dotnet-ildasm
dotnet-ildasm MyApp.dll

# 3. Built-in ILDASM (ships with .NET SDK on Windows)
# Run from Developer Command Prompt:
ildasm MyApp.dll

# 4. dotnet-dump / SOS — inspect IL at runtime
dotnet tool install -g dotnet-dump
dotnet-dump collect -p <PID>
dotnet-dump analyze <dump-file>
# Then: clrthreads, dumpil, etc.
```

**In Visual Studio:**
- **ILSpy extension** — right-click method ’ "Open in ILSpy"
- **Disassembly window** (Debug ’ Windows ’ Disassembly) — shows JIT-compiled native code

**SharpLab.io** — paste C# and instantly view IL, JIT ASM, or decompiled output online.

```cs
// C# source
public static int Square(int x) => x * x;

// IL (as seen in ILSpy):
// .method public hidebysig static int32 Square(int32 x) cil managed
// {
//     ldarg.0
//     ldarg.0
//     mul
//     ret
// }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the benefit of compiling into IL code?

Compiling to IL provides several advantages over compiling directly to native machine code:

| Benefit | Explanation |
|---------|------------|
| **Platform independence** | Same IL runs on Windows, Linux, macOS, ARM — JIT produces native code per platform |
| **Language interoperability** | C#, F#, VB.NET all compile to the same IL — can call each other\'s types seamlessly |
| **Runtime optimizations** | JIT knows the exact CPU (AVX-512, etc.) and optimizes better than a cross-compiled binary |
| **Security / verification** | CLR verifies IL for type safety before execution |
| **Reflection** | IL preserves metadata — types, methods, attributes queryable at runtime |
| **Dynamic code** | `System.Reflection.Emit` generates IL at runtime for dynamic proxies, expression trees |
| **AOT-ready** | Same IL can be JIT-compiled, interpreted (Mono), or AOT-compiled (NativeAOT) |

```cs
// Benefit: Language interoperability
// F# library (compiled to IL):
// module MathLib
// let square x = x * x

// C# consuming F# IL seamlessly:
// int result = MathLib.square(5); // 25

// Benefit: Runtime IL generation with Reflection.Emit
using System.Reflection.Emit;

var method = new DynamicMethod("Add", typeof(int),
    [typeof(int), typeof(int)]);
var il = method.GetILGenerator();
il.Emit(OpCodes.Ldarg_0);
il.Emit(OpCodes.Ldarg_1);
il.Emit(OpCodes.Add);
il.Emit(OpCodes.Ret);

var add = (Func<int, int, int>)method.CreateDelegate(typeof(Func<int, int, int>));
Console.WriteLine(add(3, 4)); // 7

// Benefit: JIT knows the target CPU
// On a machine with AVX-512, JIT auto-vectorizes loops:
int[] arr = Enumerable.Range(0, 1024).ToArray();
int sum = 0;
foreach (var n in arr) sum += n; // JIT may emit VPADDD (SIMD) instruction
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the importance of CTS (Common Type System)?

The **CTS (Common Type System)** is the specification that defines how types are declared, used, and managed in the .NET runtime. It ensures that types from different .NET languages (C#, F#, VB.NET) are compatible with each other.

**Key responsibilities:**
- Defines all type categories (value types, reference types, interfaces, delegates, enums)
- Establishes rules for type inheritance and method overriding
- Enables cross-language type sharing — a C# `int` and a VB.NET `Integer` are both `System.Int32`

```cs
// CTS type mapping — all languages share the same underlying CLR types
// C#      VB.NET     F#        CTS (CLR) Type
// int   = Integer  = int    = System.Int32
// long  = Long     = int64  = System.Int64
// string= String   = string = System.String
// bool  = Boolean  = bool   = System.Boolean

// Proof: C# int IS System.Int32
int x = 42;
System.Int32 y = 42;
Console.WriteLine(x.GetType() == y.GetType()); // True
Console.WriteLine(x.GetType().FullName);        // System.Int32

// CTS value types vs reference types
int    vt = 10;          // value type — CTS ValueType
string rt = "hello";     // reference type — CTS object
Console.WriteLine(vt.GetType().IsValueType); // True
Console.WriteLine(rt.GetType().IsValueType); // False

// CTS type hierarchy — everything derives from System.Object
Console.WriteLine(typeof(int).BaseType?.Name);    // ValueType
Console.WriteLine(typeof(string).BaseType?.Name); // Object
Console.WriteLine(typeof(bool).IsSubclassOf(typeof(object))); // True (via ValueType)

// Cross-language interop enabled by CTS
// A C# class can be used by F# or VB.NET code without any adapter
public class Calculator
{
    public int Add(int a, int b) => a + b;    // System.Int32 — universal in .NET
}
```

**CTS vs CLS:** CTS defines all possible types. The **CLS (Common Language Specification)** is a subset that all languages must support for cross-language compatibility (e.g., avoid unsigned types in public APIs since VB.NET doesn\'t have `uint`).

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How are initializers executed?

In C#, initializers run in a specific order before the constructor body executes. Understanding this order prevents subtle bugs.

**Execution order:**
1. **Static field initializers** (once, before first use of the type)
2. **Static constructor** (`static MyClass()`)
3. **Instance field initializers** (top to bottom, before constructor body)
4. **Base class constructor** (`base(...)`)
5. **Derived class constructor body**

```cs
public class Base
{
    public int BaseField = Log("Base field init", 10);
    public int BaseProp { get; } = Log("Base prop init", 20);

    static int Log(string msg, int val)
    {
        Console.WriteLine(msg);
        return val;
    }

    public Base()  => Console.WriteLine("Base constructor");
}

public class Derived : Base
{
    public int DerivedField = Log("Derived field init", 30);

    public Derived() : base()
    {
        Console.WriteLine("Derived constructor");
    }

    static int Log(string msg, int val)
    {
        Console.WriteLine(msg);
        return val;
    }
}

var d = new Derived();
// Output:
// Derived field init     instance field initializer runs FIRST (before base ctor)
// Base field init        base field initializers run when base() is called
// Base prop init
// Base constructor       base constructor body
// Derived constructor    derived constructor body

// Object / collection initializers — syntactic sugar, run after constructor
var list = new List<int> { 1, 2, 3 }; // equivalent to: Add(1); Add(2); Add(3);

record Point(int X, int Y);
var p = new Point(1, 2) { X = 10 }; // with expression — creates new Point(10, 2)

// Required init (C# 11+)
public class Config
{
    public required string ConnectionString { get; init; }
    public int Timeout { get; init; } = 30; // field initializer with default
}

var cfg = new Config { ConnectionString = "Server=localhost" };
Console.WriteLine(cfg.Timeout); // 30
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Shadowing?

**Shadowing** (method hiding) is the practice of declaring a member in a derived class with the **same name** as a member in a base class using the `new` keyword. Unlike `override`, shadowing does not participate in polymorphism — the choice of method depends on the **compile-time type** of the variable.

```cs
public class Animal
{
    public void Speak() => Console.WriteLine("Animal speaks");
    public virtual void Move() => Console.WriteLine("Animal moves");
}

public class Dog : Animal
{
    // Shadowing — hides Animal.Speak (compiler warning without 'new')
    public new void Speak() => Console.WriteLine("Dog barks");

    // Overriding — participates in polymorphism
    public override void Move() => Console.WriteLine("Dog runs");
}

var dog = new Dog();
dog.Speak();  // Dog barks        Dog.Speak (compile-time type = Dog)
dog.Move();   // Dog runs         Dog.Move (override — polymorphic)

Animal animal = new Dog();
animal.Speak(); // Animal speaks  Animal.Speak (compile-time type = Animal — shadowing!)
animal.Move();  // Dog runs       Dog.Move (override — polymorphic)

// Shadowing in practice — useful when extending sealed/framework types
public class MyList<T> : List<T>
{
    // Shadow List<T>.Add to add logging without override
    public new void Add(T item)
    {
        Console.WriteLine($"Adding: {item}");
        base.Add(item);
    }
}

var myList = new MyList<int>();
myList.Add(42);      // Adding: 42 (shadowed version called)

List<int> asList = myList;
asList.Add(99);      // original List<T>.Add called (no log — shadowing, not override)

// Shadowing vs Overriding
Console.WriteLine("--- Key Difference ---");
// Shadow: method selected by compile-time type
// Override: method selected by runtime type (true polymorphism)
```

**Best practice:** Prefer `override` over shadowing. Use `new` (shadowing) only when you cannot or should not override (e.g., method is not `virtual`, or you intentionally want different behavior per reference type).

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does Reflection work in C# and when should you use it?

**Reflection** allows you to inspect and manipulate types, methods, properties, and fields at runtime. It is used for serializers, ORMs, DI containers, and plugin systems.

```cs
using System.Reflection;

//  1. Inspect a type ———————————————————————————————————————————————
Type type = typeof(string);

Console.WriteLine(type.FullName);          // System.String
Console.WriteLine(type.IsClass);           // True
Console.WriteLine(type.IsValueType);       // False
Console.WriteLine(type.BaseType?.Name);    // Object

// All public methods
foreach (MethodInfo method in type.GetMethods(BindingFlags.Public | BindingFlags.Instance))
    Console.WriteLine($"  {method.Name}({string.Join(", ", method.GetParameters().Select(p => p.ParameterType.Name))})");

//  2. Create instance and invoke method dynamically ————————————————
public class Calculator
{
    public int Add(int a, int b) => a + b;
    private string _secret = "hidden";

    [Obsolete("Use Add instead")]
    public int Sum(int a, int b) => a + b;
}

// Create instance via reflection
Type calcType = typeof(Calculator);
object? calc = Activator.CreateInstance(calcType);

// Invoke public method
MethodInfo? addMethod = calcType.GetMethod("Add");
object? result = addMethod!.Invoke(calc, [3, 7]);
Console.WriteLine(result); // 10

// Access private field
FieldInfo? secretField = calcType.GetField("_secret",
    BindingFlags.NonPublic | BindingFlags.Instance);
Console.WriteLine(secretField?.GetValue(calc));   // hidden
secretField?.SetValue(calc, "modified");
Console.WriteLine(secretField?.GetValue(calc));   // modified

//  3. Read attributes via reflection ———————————————————————————————
foreach (MethodInfo method in calcType.GetMethods())
{
    var obsolete = method.GetCustomAttribute<ObsoleteAttribute>();
    if (obsolete != null)
        Console.WriteLine($"{method.Name} is obsolete: {obsolete.Message}");
}

//  4. Generic reflection ———————————————————————————————————————————
// Create List<int> dynamically
Type listType = typeof(List<>).MakeGenericType(typeof(int));
object list = Activator.CreateInstance(listType)!;
MethodInfo addItem = listType.GetMethod("Add")!;
addItem.Invoke(list, [42]);
addItem.Invoke(list, [99]);
Console.WriteLine(listType.GetProperty("Count")?.GetValue(list)); // 2

//  5. Property access and setting ——————————————————————————————————
public class Person { public string Name { get; set; } = ""; public int Age { get; set; } }

var person = new Person();
Type personType = typeof(Person);

// Set properties by name (useful for generic mappers)
var values = new Dictionary<string, object> { ["Name"] = "Alice", ["Age"] = 30 };
foreach (var (key, value) in values)
{
    PropertyInfo? prop = personType.GetProperty(key);
    prop?.SetValue(person, Convert.ChangeType(value, prop.PropertyType));
}
Console.WriteLine($"{person.Name}, {person.Age}"); // Alice, 30

//  6. Cached reflection with compiled expressions (fast path) ——————
// Raw reflection is ~100-300x slower than direct calls
// Cache with compiled delegates for hot paths
var compiled = CreateSetter<Person, string>(p => p.Name);
compiled(person, "Bob"); // fast — no reflection overhead

Func<T, TProp> CreateGetter<T, TProp>(System.Linq.Expressions.Expression<Func<T, TProp>> expr)
    => expr.Compile();

Action<T, TProp> CreateSetter<T, TProp>(System.Linq.Expressions.Expression<Func<T, TProp>> expr)
{
    var param  = System.Linq.Expressions.Expression.Parameter(typeof(T));
    var value  = System.Linq.Expressions.Expression.Parameter(typeof(TProp));
    var member = (System.Linq.Expressions.MemberExpression)expr.Body;
    var assign = System.Linq.Expressions.Expression.Assign(
        System.Linq.Expressions.Expression.Property(param, member.Member.Name), value);
    return System.Linq.Expressions.Expression.Lambda<Action<T, TProp>>(assign, param, value).Compile();
}
```

**Performance note:** Use reflection sparingly. Cache `Type`, `MethodInfo`, and `PropertyInfo` objects. For hot paths, compile to delegates or use source generators instead.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create and use custom attributes in C#?

**Custom attributes** are metadata annotations that can be attached to types, methods, properties, parameters, etc., and read at runtime via reflection or at compile time via source generators / Roslyn analyzers.

```cs
using System;
using System.Reflection;

//  1. Define a custom attribute ———————————————————————————————————
[AttributeUsage(
    AttributeTargets.Class | AttributeTargets.Method,  // where it can be applied
    AllowMultiple = false,                             // one per target
    Inherited     = true)]                             // derived classes inherit it
public class AuditAttribute : Attribute
{
    public string  Action   { get; }
    public string? Category { get; set; }
    public bool    LogArgs  { get; set; } = true;

    public AuditAttribute(string action) => Action = action;
}

//  2. Apply the attribute ——————————————————————————————————————————
[Audit("Order", Category = "Commerce")]
public class OrderController
{
    [Audit("PlaceOrder", LogArgs = true)]
    public Task<Order> PlaceOrderAsync(PlaceOrderRequest req) => Task.FromResult(new Order());
}

//  3. Read attributes at runtime via reflection ————————————————————
Type type = typeof(OrderController);

// Class-level attribute
var classAudit = type.GetCustomAttribute<AuditAttribute>();
Console.WriteLine($"Class action: {classAudit?.Action}"); // Order

// Method-level attribute
foreach (MethodInfo method in type.GetMethods())
{
    var methodAudit = method.GetCustomAttribute<AuditAttribute>();
    if (methodAudit != null)
        Console.WriteLine($"{method.Name}: {methodAudit.Action}, LogArgs={methodAudit.LogArgs}");
}

//  4. Validation attribute (like DataAnnotations) ——————————————————
[AttributeUsage(AttributeTargets.Property)]
public class MustBePastAttribute : ValidationAttribute
{
    public MustBePastAttribute() : base("The date must be in the past.") { }

    public override bool IsValid(object? value)
        => value is DateTime dt && dt < DateTime.UtcNow;
}

public class CreateEventRequest
{
    [Required]
    public string Name { get; set; } = null!;

    [MustBePast]
    public DateTime StartedAt { get; set; }
}

// ASP.NET Core validates automatically via [ApiController]
// Manual validation:
var request = new CreateEventRequest { Name = "Conf", StartedAt = DateTime.UtcNow.AddDays(1) };
var ctx     = new ValidationContext(request);
var results = new List<ValidationResult>();
bool valid  = Validator.TryValidateObject(request, ctx, results, validateAllProperties: true);
Console.WriteLine(valid);        // False
Console.WriteLine(results[0].ErrorMessage); // The date must be in the past.

//  5. Parameter attribute ——————————————————————————————————————————
[AttributeUsage(AttributeTargets.Parameter)]
public class NotEmptyAttribute : Attribute { }

public static class Guard
{
    public static string NotEmpty([NotEmpty] string value, [CallerArgumentExpression(nameof(value))] string? name = null)
    {
        if (string.IsNullOrWhiteSpace(value))
            throw new ArgumentException($"'{name}' must not be empty.", name);
        return value;
    }
}

var name = Guard.NotEmpty(""); // throws: 'value' must not be empty.
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are source generators and how do you create one?

**Source generators** run during compilation and add new C# source files to the project. They eliminate runtime reflection overhead and enable compile-time code generation (serializers, mappers, DI wiring, etc.).

```bash
dotnet new classlib -n MySourceGenerator
dotnet add MySourceGenerator package Microsoft.CodeAnalysis.CSharp
dotnet add MySourceGenerator package Microsoft.CodeAnalysis.Analyzers
```

```xml
<!-- MySourceGenerator.csproj -->
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>  <!-- required for generators -->
    <LangVersion>latest</LangVersion>
    <Nullable>enable</Nullable>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.CodeAnalysis.CSharp" Version="4.*" PrivateAssets="all" />
    <PackageReference Include="Microsoft.CodeAnalysis.Analyzers" Version="3.*" PrivateAssets="all" />
  </ItemGroup>
</Project>
```

```cs
//  1. Incremental Source Generator (recommended — .NET 6+) —————————
using Microsoft.CodeAnalysis;
using Microsoft.CodeAnalysis.CSharp.Syntax;
using System.Collections.Immutable;
using System.Text;

[Generator]
public class ToStringGenerator : IIncrementalGenerator
{
    public void Initialize(IncrementalGeneratorInitializationContext context)
    {
        // 1. Find classes marked with [GenerateToString]
        IncrementalValuesProvider<ClassDeclarationSyntax> classDeclarations =
            context.SyntaxProvider
                .CreateSyntaxProvider(
                    predicate: static (node, _) => node is ClassDeclarationSyntax cls
                        && cls.AttributeLists.Count > 0,
                    transform: static (ctx, _) => GetSemanticTarget(ctx))
                .Where(static m => m is not null)!;

        // 2. Combine with compilation and generate
        IncrementalValueProvider<(Compilation, ImmutableArray<ClassDeclarationSyntax>)> compilation =
            context.CompilationProvider.Combine(classDeclarations.Collect());

        context.RegisterSourceOutput(compilation,
            static (spc, source) => Execute(source.Item1, source.Item2, spc));
    }

    private static ClassDeclarationSyntax? GetSemanticTarget(GeneratorSyntaxContext ctx)
    {
        var classDecl = (ClassDeclarationSyntax)ctx.Node;
        var model     = ctx.SemanticModel;
        var symbol    = model.GetDeclaredSymbol(classDecl);

        return symbol?.GetAttributes()
            .Any(a => a.AttributeClass?.Name == "GenerateToStringAttribute") == true
            ? classDecl
            : null;
    }

    private static void Execute(
        Compilation compilation,
        ImmutableArray<ClassDeclarationSyntax> classes,
        SourceProductionContext ctx)
    {
        foreach (var classDecl in classes)
        {
            var model  = compilation.GetSemanticModel(classDecl.SyntaxTree);
            var symbol = model.GetDeclaredSymbol(classDecl) as INamedTypeSymbol;
            if (symbol is null) continue;

            var source = GenerateToString(symbol);
            ctx.AddSource($"{symbol.Name}.g.cs", source);
        }
    }

    private static string GenerateToString(INamedTypeSymbol symbol)
    {
        
        var ns = symbol.ContainingNamespace.IsGlobalNamespace 
            ? null 
            : symbol.ContainingNamespace.ToDisplayString();

        var props = symbol.GetMembers()
            .OfType<IPropertySymbol>()
            .Where(p => p.DeclaredAccessibility == Accessibility.Public);

        
        var accessibility = symbol.DeclaredAccessibility.ToString().ToLower();

        var propString = string.Join(", ", props.Select(p => $"{p.Name} = {{{p.Name}}}"));

        var sb = new StringBuilder();
        sb.AppendLine("// <auto-generated/>");
        
        if (ns != null)
        {
            sb.AppendLine($"namespace {ns};");
            sb.AppendLine();
        }

        sb.Append($$"""
        {{accessibility}} partial class {{symbol.Name}}
        {
            public override string ToString() => $"{{symbol.Name}} {{ {{propString}} }}";
        }
        """);

        return sb.ToString();
    }

}

//  2. Attribute trigger (add to generator project) —————————————————
[AttributeUsage(AttributeTargets.Class)]
public sealed class GenerateToStringAttribute : Attribute { }

//  3. Consumer project —————————————————————————————————————————————
// Add reference to generator:
// <ProjectReference Include="../MySourceGenerator/MySourceGenerator.csproj"
//                   OutputItemType="Analyzer" ReferenceOutputAssembly="false" />

[GenerateToString]
public partial class Product    // must be partial
{
    public string Name  { get; set; } = null!;
    public decimal Price { get; set; }
    public int Stock { get; set; }
}

// At compile time, generator adds:
// public override string ToString() => $"Product { Name = {Name}, Price = {Price}, Stock = {Stock} }";

var p = new Product { Name = "Laptop", Price = 999m, Stock = 5 };
Console.WriteLine(p); // Product { Name = Laptop, Price = 999, Stock = 5 }
```

**Real-world source generators in .NET:**
- `System.Text.Json` — `[JsonSerializable]` for AOT-safe JSON
- `Microsoft.Extensions.Logging` — `[LoggerMessage]` for high-performance logging  
- Entity Framework Core — compiled models
- `AutoMapper` — mapping code generation

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does the dynamic keyword work in C# and when should you use it?

The `dynamic` keyword bypasses compile-time type checking. Member resolution happens at runtime via the **Dynamic Language Runtime (DLR)**.

```cs
using System.Dynamic;
using Microsoft.CSharp.RuntimeBinder;

//  1. Basic dynamic usage ——————————————————————————————————————————
dynamic value = 42;
Console.WriteLine(value + 8);   // 50 — resolved as int addition at runtime

value = "Hello";
Console.WriteLine(value.Length); // 5 — string.Length resolved at runtime

value = new DateTime(2026, 1, 1);
Console.WriteLine(value.Year);   // 2026 — DateTime.Year at runtime

//  2. COM Interop (primary use case) ———————————————————————————————
// Without dynamic (verbose)
var excel = (Microsoft.Office.Interop.Excel.Application)
    Activator.CreateInstance(Type.GetTypeFromProgID("Excel.Application")!);

// With dynamic (clean)
dynamic excelDyn = Activator.CreateInstance(
    Type.GetTypeFromProgID("Excel.Application")!)!;
excelDyn.Visible = true;
dynamic workbook = excelDyn.Workbooks.Add();
dynamic sheet = workbook.Worksheets[1];
sheet.Cells[1, 1] = "Hello from C#!";

//  3. Working with JSON / dictionary structures —————————————————————
// ExpandoObject — dynamic dictionary that works like an object
dynamic person = new ExpandoObject();
person.Name = "Alice";
person.Age  = 30;
person.Greet = (Func<string>)(() => $"Hi, I'm {person.Name}!");
Console.WriteLine(person.Greet()); // Hi, I'm Alice!

// ExpandoObject implements IDictionary<string, object>
var dict = (IDictionary<string, object?>)person;
dict["Email"] = "alice@example.com";
Console.WriteLine(dict.ContainsKey("Email")); // True

//  4. DynamicObject — custom dynamic behavior ——————————————————————
public class DynamicConfig : DynamicObject
{
    private readonly Dictionary<string, object?> _data = new();

    public override bool TrySetMember(SetMemberBinder binder, object? value)
    {
        _data[binder.Name] = value;
        return true;
    }

    public override bool TryGetMember(GetMemberBinder binder, out object? result)
        => _data.TryGetValue(binder.Name, out result);

    public override bool TryInvokeMember(InvokeMemberBinder binder,
        object?[]? args, out object? result)
    {
        result = $"Invoked: {binder.Name}({string.Join(", ", args ?? [])})";
        return true;
    }
}

dynamic config = new DynamicConfig();
config.ConnectionString = "Server=localhost;Database=mydb";
config.MaxRetries = 3;
Console.WriteLine(config.ConnectionString); // Server=localhost;Database=mydb
Console.WriteLine(config.DoSomething("a", "b")); // Invoked: DoSomething(a, b)

//  5. Calling private/internal members (advanced) ——————————————————
// Use reflection with dynamic for cleaner syntax on legacy APIs
var internalObj = CreateInternalInstance();
dynamic d = internalObj;
// d.InternalMethod(); // Works if member exists — RuntimeBinderException if not

//  6. Pitfalls ————————————————————————————————————————————————————
dynamic x = "hello";
try
{
    int num = x + 5; // RuntimeBinderException — cannot add string and int
}
catch (RuntimeBinderException ex)
{
    Console.WriteLine($"Runtime error: {ex.Message}");
}

// dynamic is ~10-100x slower than static dispatch — avoid in hot paths
// No IntelliSense, no compile-time errors for typos
// Prefer: pattern matching, generics, interfaces over dynamic

object CreateInternalInstance() => new object();
```

**When to use `dynamic`:**

| Use case | Recommended |
|----------|-------------|
| COM interop (Office, legacy) | … Yes |
| `ExpandoObject` for flexible data bags | … Acceptable |
| Unknown JSON structure (prefer `JsonNode`/`JsonElement`) |  Prefer typed approach |
| Reflection replacement in hot paths |  No — use compiled delegates |
| Plugin systems |  Prefer interfaces |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>
