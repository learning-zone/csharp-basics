# Design Patterns in .NET Core

> *Click &#9733; if you like the project. Your contributions are heartily ♡ welcome.*

<br/>

## Table of Contents

* [Design Pattern Fundamentals](#-1.design-pattern-fundamentals)
* [Dependency Injection and IoC](#-2.dependency-injection)
* [Singleton Pattern](#-3-singleton-pattern)
* [Factory Pattern](#-4-factory-pattern)
* [Observer Pattern](#-5-observer-pattern)
* [Strategy Pattern](#-6-strategy-pattern)
* [Middleware Pattern](#-7-middleware-pattern)
* [Decorator Pattern](#-8-decorator-pattern)
* [Proxy Pattern](#-9-proxy-pattern)
* [Command Pattern](#-10-command-pattern)
* [Module Pattern](#-11-module-pattern)
* [Dependency Injection Pattern](#-12-dependency-injection-pattern)
* [Repository Pattern](#-13-repository-pattern)
* [Delegate Pattern](#-14-delegate-pattern)
* [Mediator Pattern](#-15-mediator-pattern)
* [Adapter Pattern](#-16-adapter-pattern)
* [Facade Pattern](#-17-Facade-pattern)
* [Builder Pattern](#-18-builder-pattern)
* [Specification Pattern](#-19-specification-pattern)
* [Anti-Pattern & Code Smell](#-20-anti-pattern)

<br/>

## # 1. DESIGN PATTERN FUNDAMENTALS

<br/>

## Q. What are Design Patterns and why are they important in .NET Core?

Design Patterns are reusable solutions to commonly occurring problems in software design. They are not finished code but templates that guide how to solve a problem in a given context.

**Categories:**

| Category    | Description                                         | Examples                                  |
|-------------|-----------------------------------------------------|-------------------------------------------|
| Creational  | Object creation mechanisms                          | Singleton, Factory, Builder, Prototype    |
| Structural  | Class and object composition                        | Adapter, Decorator, Facade, Proxy         |
| Behavioral  | Communication between objects                       | Observer, Strategy, Command, Mediator     |

**Why important in .NET Core:**
- Promotes code reusability and maintainability
- Provides a common vocabulary for developers
- Solves architectural problems proven by experience
- Facilitates testability and loose coupling
- Built into ASP.NET Core (DI, Middleware, Options Pattern)

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between a Design Pattern and an Architecture Pattern?

| Aspect             | Design Pattern                          | Architecture Pattern                        |
|--------------------|-----------------------------------------|---------------------------------------------|
| Scope              | Class/object level                      | System/application level                    |
| Examples           | Singleton, Factory, Observer            | MVC, CQRS, Microservices, Event Sourcing    |
| Purpose            | Solves local design problems            | Defines system structure                    |
| Granularity        | Fine-grained                            | Coarse-grained                              |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 2. DEPENDENCY INJECTION AND IoC

<br/>

## Q. What is Dependency Injection and Inversion of Control (IoC)?

**Inversion of Control (IoC)** is a principle where the control of object creation and lifecycle is inverted from the class itself to a container or framework.

**Dependency Injection (DI)** is the most common implementation of IoC where dependencies are "injected" into a class rather than the class creating them.

**Types of DI:**

```csharp
// 1. Constructor Injection (recommended)
public class OrderService
{
    private readonly IEmailService _emailService;

    public OrderService(IEmailService emailService) // injected via constructor
    {
        _emailService = emailService;
    }
}

// 2. Property Injection
public class OrderService
{
    public IEmailService EmailService { get; set; } // injected via property setter
}

// 3. Method Injection
public class OrderService
{
    public void ProcessOrder(IEmailService emailService) // injected via method
    {
        emailService.Send("Order processed");
    }
}
```

**Registration in .NET Core:**

```csharp
// Program.cs
builder.Services.AddTransient<IEmailService, SmtpEmailService>();
builder.Services.AddScoped<IOrderService, OrderService>();
builder.Services.AddSingleton<ICacheService, MemoryCacheService>();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the DI Lifetime scopes in .NET Core?

```csharp
// Transient: new instance every time it is requested
builder.Services.AddTransient<ITransientService, TransientService>();

// Scoped: one instance per HTTP request
builder.Services.AddScoped<IScopedService, ScopedService>();

// Singleton: one instance for application lifetime
builder.Services.AddSingleton<ISingletonService, SingletonService>();
```

**Comparison:**

| Lifetime   | Instance Created       | Use Case                                    |
|------------|------------------------|---------------------------------------------|
| Transient  | Every time requested   | Lightweight, stateless services             |
| Scoped     | Once per HTTP request  | Database contexts, unit-of-work patterns    |
| Singleton  | Once per app lifetime  | Configuration, caching, logging             |

**Captive Dependency (Anti-pattern):** Injecting a shorter-lived service into a longer-lived one (e.g., Scoped into Singleton) causes bugs and should be avoided.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 3. SINGLETON PATTERN

<br/>

## Q. What is the Singleton Pattern? When should you use it?

The Singleton Pattern ensures a class has **only one instance** throughout the application lifetime and provides a global access point to it.

**Use Cases:**
- Configuration managers
- Logging services
- Connection pool managers
- Cache managers
- Thread pools

**Implementation approaches:**

```csharp
// 1. Thread-safe Singleton with lock
public sealed class ConfigurationManager
{
    private static ConfigurationManager _instance;
    private static readonly object _lock = new object();

    private ConfigurationManager() { }

    public static ConfigurationManager Instance
    {
        get
        {
            lock (_lock)
            {
                _instance ??= new ConfigurationManager();
                return _instance;
            }
        }
    }

    public string GetSetting(string key) => "value";
}

// 2. Lazy<T> - thread-safe and lazy initialization (recommended)
public sealed class AppCache
{
    private static readonly Lazy<AppCache> _lazy =
        new Lazy<AppCache>(() => new AppCache());

    public static AppCache Instance => _lazy.Value;

    private AppCache() { }

    private readonly Dictionary<string, object> _cache = new();

    public void Set(string key, object value) => _cache[key] = value;
    public object Get(string key) => _cache.TryGetValue(key, out var val) ? val : null;
}

// 3. .NET Core DI Singleton (preferred in ASP.NET Core)
public class AppCache
{
    private readonly Dictionary<string, object> _cache = new();

    public void Set(string key, object value) => _cache[key] = value;
    public object Get(string key) => _cache.TryGetValue(key, out var val) ? val : null;
}

// Registration
builder.Services.AddSingleton<AppCache>();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the pitfalls of the Singleton Pattern?

1. **Thread safety issues** — shared mutable state can cause race conditions
2. **Hidden dependencies** — makes unit testing difficult
3. **Global state** — violates Single Responsibility Principle
4. **Tight coupling** — consumers depend on concrete class
5. **Captive dependency** — Singleton holding Scoped service causes bugs

**Better alternative in .NET Core:** Register via DI as `AddSingleton<T>` to keep benefits while maintaining testability.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 4. FACTORY PATTERN

<br/>

## Q. What is the Factory Pattern? Explain its variants.

The Factory Pattern provides an interface for creating objects without specifying the exact class. It promotes loose coupling by eliminating direct instantiation.

**Variants:**

| Variant           | Description                                                  |
|-------------------|--------------------------------------------------------------|
| Simple Factory    | Static method that creates objects (not a GoF pattern)       |
| Factory Method    | Subclasses decide which class to instantiate                 |
| Abstract Factory  | Creates families of related objects                          |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement Factory Method Pattern in .NET Core with a real-world example.

**Use Case:** Payment processing with multiple providers (Stripe, PayPal, Bank Transfer)

```csharp
// Product interface
public interface IPaymentProcessor
{
    Task<PaymentResult> ProcessAsync(decimal amount, string currency);
}

// Concrete products
public class StripePaymentProcessor : IPaymentProcessor
{
    public async Task<PaymentResult> ProcessAsync(decimal amount, string currency)
    {
        return new PaymentResult { Success = true, TransactionId = "stripe_123" };
    }
}

public class PayPalPaymentProcessor : IPaymentProcessor
{
    public async Task<PaymentResult> ProcessAsync(decimal amount, string currency)
    {
        return new PaymentResult { Success = true, TransactionId = "paypal_456" };
    }
}

public class BankTransferProcessor : IPaymentProcessor
{
    public async Task<PaymentResult> ProcessAsync(decimal amount, string currency)
    {
        return new PaymentResult { Success = true, TransactionId = "bank_789" };
    }
}

// Factory
public interface IPaymentProcessorFactory
{
    IPaymentProcessor Create(string paymentMethod);
}

public class PaymentProcessorFactory : IPaymentProcessorFactory
{
    private readonly IServiceProvider _serviceProvider;

    public PaymentProcessorFactory(IServiceProvider serviceProvider)
    {
        _serviceProvider = serviceProvider;
    }

    public IPaymentProcessor Create(string paymentMethod) => paymentMethod switch
    {
        "stripe" => _serviceProvider.GetRequiredService<StripePaymentProcessor>(),
        "paypal" => _serviceProvider.GetRequiredService<PayPalPaymentProcessor>(),
        "bank"   => _serviceProvider.GetRequiredService<BankTransferProcessor>(),
        _        => throw new ArgumentException($"Unknown payment method: {paymentMethod}")
    };
}

// Registration
builder.Services.AddTransient<StripePaymentProcessor>();
builder.Services.AddTransient<PayPalPaymentProcessor>();
builder.Services.AddTransient<BankTransferProcessor>();
builder.Services.AddSingleton<IPaymentProcessorFactory, PaymentProcessorFactory>();

// Usage in controller
public class PaymentController : ControllerBase
{
    private readonly IPaymentProcessorFactory _factory;

    public PaymentController(IPaymentProcessorFactory factory)
    {
        _factory = factory;
    }

    [HttpPost("charge")]
    public async Task<IActionResult> Charge([FromBody] ChargeRequest request)
    {
        var processor = _factory.Create(request.PaymentMethod);
        var result = await processor.ProcessAsync(request.Amount, request.Currency);
        return Ok(result);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the Abstract Factory Pattern? Provide a .NET Core example.

**Use Case:** Cross-platform UI component creation (Windows vs Linux)

```csharp
// Abstract products
public interface IButton  { void Render(); }
public interface ITextBox { void Render(); }

// Windows implementations
public class WindowsButton  : IButton  { public void Render() => Console.WriteLine("Rendering Windows Button"); }
public class WindowsTextBox : ITextBox { public void Render() => Console.WriteLine("Rendering Windows TextBox"); }

// Linux implementations
public class LinuxButton  : IButton  { public void Render() => Console.WriteLine("Rendering Linux Button"); }
public class LinuxTextBox : ITextBox { public void Render() => Console.WriteLine("Rendering Linux TextBox"); }

// Abstract factory
public interface IUIFactory
{
    IButton  CreateButton();
    ITextBox CreateTextBox();
}

// Concrete factories
public class WindowsUIFactory : IUIFactory
{
    public IButton  CreateButton()  => new WindowsButton();
    public ITextBox CreateTextBox() => new WindowsTextBox();
}

public class LinuxUIFactory : IUIFactory
{
    public IButton  CreateButton()  => new LinuxButton();
    public ITextBox CreateTextBox() => new LinuxTextBox();
}

// Registration based on runtime OS
builder.Services.AddSingleton<IUIFactory>(sp =>
    RuntimeInformation.IsOSPlatform(OSPlatform.Windows)
        ? new WindowsUIFactory()
        : new LinuxUIFactory());
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 5. OBSERVER PATTERN

<br/>

## Q. What is the Observer Pattern? How is it implemented in .NET Core?

The Observer Pattern defines a **one-to-many** dependency between objects. When one object (subject/publisher) changes state, all dependents (observers/subscribers) are notified automatically.

**Built-in .NET mechanisms:**
- `IObserver<T>` / `IObservable<T>` interfaces
- C# Events and Delegates
- `INotifyPropertyChanged`
- MediatR (notification handlers)

```csharp
// Using IObservable<T> / IObserver<T>
public class StockTicker : IObservable<StockPrice>
{
    private readonly List<IObserver<StockPrice>> _observers = new();

    public IDisposable Subscribe(IObserver<StockPrice> observer)
    {
        if (!_observers.Contains(observer))
            _observers.Add(observer);
        return new Unsubscriber(_observers, observer);
    }

    public void UpdatePrice(StockPrice price)
    {
        foreach (var observer in _observers)
            observer.OnNext(price);
    }

    private class Unsubscriber : IDisposable
    {
        private readonly List<IObserver<StockPrice>> _observers;
        private readonly IObserver<StockPrice> _observer;

        public Unsubscriber(List<IObserver<StockPrice>> observers, IObserver<StockPrice> observer)
        {
            _observers = observers;
            _observer  = observer;
        }

        public void Dispose()
        {
            if (_observer != null && _observers.Contains(_observer))
                _observers.Remove(_observer);
        }
    }
}

public class StockDisplay : IObserver<StockPrice>
{
    public void OnNext(StockPrice value)    => Console.WriteLine($"Stock: {value.Symbol} @ {value.Price}");
    public void OnError(Exception error)   => Console.WriteLine($"Error: {error.Message}");
    public void OnCompleted()              => Console.WriteLine("Feed ended");
}

public record StockPrice(string Symbol, decimal Price);

// Usage
var ticker  = new StockTicker();
var display = new StockDisplay();
using var subscription = ticker.Subscribe(display);
ticker.UpdatePrice(new StockPrice("MSFT", 420.50m));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does the Observer Pattern relate to C# Events?

```csharp
// Using C# Events (delegate-based Observer)
public class OrderService
{
    public event EventHandler<OrderPlacedEventArgs> OrderPlaced;

    public void PlaceOrder(Order order)
    {
        // Process order...
        OnOrderPlaced(new OrderPlacedEventArgs(order));
    }

    protected virtual void OnOrderPlaced(OrderPlacedEventArgs e)
    {
        OrderPlaced?.Invoke(this, e);
    }
}

public class OrderPlacedEventArgs : EventArgs
{
    public Order Order { get; }
    public OrderPlacedEventArgs(Order order) => Order = order;
}

// Subscribers
public class EmailNotifier
{
    public void OnOrderPlaced(object sender, OrderPlacedEventArgs e)
        => Console.WriteLine($"Email sent for order {e.Order.Id}");
}

public class InventoryUpdater
{
    public void OnOrderPlaced(object sender, OrderPlacedEventArgs e)
        => Console.WriteLine($"Inventory updated for order {e.Order.Id}");
}

// Wiring
var orderService    = new OrderService();
var emailNotifier   = new EmailNotifier();
var inventoryUpdater = new InventoryUpdater();

orderService.OrderPlaced += emailNotifier.OnOrderPlaced;
orderService.OrderPlaced += inventoryUpdater.OnOrderPlaced;

orderService.PlaceOrder(new Order { Id = 1 });
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 6. STRATEGY PATTERN

<br/>

## Q. What is the Strategy Pattern? Provide a real-world .NET Core example.

The Strategy Pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. The strategy lets the algorithm vary independently from the clients that use it.

**Use Cases:**
- Discount/pricing calculations
- Sorting strategies
- Shipping cost calculators
- Compression algorithms
- Authentication mechanisms

```csharp
// Strategy interface
public interface IDiscountStrategy
{
    decimal ApplyDiscount(decimal price);
}

// Concrete strategies
public class NoDiscountStrategy : IDiscountStrategy
{
    public decimal ApplyDiscount(decimal price) => price;
}

public class PercentageDiscountStrategy : IDiscountStrategy
{
    private readonly decimal _percentage;
    public PercentageDiscountStrategy(decimal percentage) => _percentage = percentage;
    public decimal ApplyDiscount(decimal price) => price * (1 - _percentage / 100);
}

public class FlatDiscountStrategy : IDiscountStrategy
{
    private readonly decimal _flatAmount;
    public FlatDiscountStrategy(decimal flatAmount) => _flatAmount = flatAmount;
    public decimal ApplyDiscount(decimal price) => Math.Max(0, price - _flatAmount);
}

public class SeasonalDiscountStrategy : IDiscountStrategy
{
    public decimal ApplyDiscount(decimal price)
    {
        var isHolidaySeason = DateTime.Now.Month == 12;
        return isHolidaySeason ? price * 0.75m : price;
    }
}

// Context
public class ShoppingCart
{
    private IDiscountStrategy _discountStrategy;

    public ShoppingCart(IDiscountStrategy discountStrategy)
    {
        _discountStrategy = discountStrategy;
    }

    public void SetDiscountStrategy(IDiscountStrategy strategy)
    {
        _discountStrategy = strategy;
    }

    public decimal CalculateTotal(IEnumerable<CartItem> items)
    {
        var subtotal = items.Sum(i => i.Price * i.Quantity);
        return _discountStrategy.ApplyDiscount(subtotal);
    }
}

// Usage
var cart = new ShoppingCart(new NoDiscountStrategy());
decimal total = cart.CalculateTotal(items);

cart.SetDiscountStrategy(new PercentageDiscountStrategy(20));
decimal discountedTotal = cart.CalculateTotal(items);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement Strategy Pattern using .NET Core DI and a factory?

```csharp
public interface IShippingCostStrategy
{
    decimal Calculate(Order order);
}

public class StandardShipping : IShippingCostStrategy
{
    public decimal Calculate(Order order) => 5.99m;
}

public class ExpressShipping : IShippingCostStrategy
{
    public decimal Calculate(Order order) => 14.99m;
}

public class FreeShipping : IShippingCostStrategy
{
    public decimal Calculate(Order order) => 0m;
}

public class ShippingStrategyFactory
{
    public IShippingCostStrategy GetStrategy(Order order)
    {
        if (order.Total >= 100) return new FreeShipping();
        if (order.IsExpress)    return new ExpressShipping();
        return new StandardShipping();
    }
}

// Registration
builder.Services.AddSingleton<ShippingStrategyFactory>();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 7. MIDDLEWARE PATTERN

<br/>

## Q. What is the Middleware Pattern in ASP.NET Core?

Middleware in ASP.NET Core is software assembled into a pipeline to handle HTTP requests and responses. Each component can process the request, pass it to the next middleware, or short-circuit the pipeline.

```
Request  → [Auth] → [Logging] → [Compression] → Endpoint
Response ← [Auth] ← [Logging] ← [Compression] ←
```

```csharp
// Custom middleware class
public class RequestLoggingMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<RequestLoggingMiddleware> _logger;

    public RequestLoggingMiddleware(RequestDelegate next, ILogger<RequestLoggingMiddleware> logger)
    {
        _next   = next;
        _logger = logger;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        var sw = Stopwatch.StartNew();
        _logger.LogInformation("Request: {Method} {Path}", context.Request.Method, context.Request.Path);

        await _next(context); // Call next middleware

        sw.Stop();
        _logger.LogInformation("Response: {StatusCode} in {Elapsed}ms",
            context.Response.StatusCode, sw.ElapsedMilliseconds);
    }
}

// Extension method for clean registration
public static class RequestLoggingMiddlewareExtensions
{
    public static IApplicationBuilder UseRequestLogging(this IApplicationBuilder builder)
        => builder.UseMiddleware<RequestLoggingMiddleware>();
}

// Program.cs
app.UseRequestLogging();
app.UseAuthentication();
app.UseAuthorization();
app.MapControllers();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement global exception handling middleware?

```csharp
public class GlobalExceptionMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<GlobalExceptionMiddleware> _logger;

    public GlobalExceptionMiddleware(RequestDelegate next, ILogger<GlobalExceptionMiddleware> logger)
    {
        _next   = next;
        _logger = logger;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        try
        {
            await _next(context);
        }
        catch (NotFoundException ex)
        {
            _logger.LogWarning(ex, "Resource not found");
            await WriteErrorResponse(context, StatusCodes.Status404NotFound, ex.Message);
        }
        catch (ValidationException ex)
        {
            _logger.LogWarning(ex, "Validation failed");
            await WriteErrorResponse(context, StatusCodes.Status400BadRequest, ex.Message);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Unhandled exception");
            await WriteErrorResponse(context, StatusCodes.Status500InternalServerError, "An internal error occurred");
        }
    }

    private static async Task WriteErrorResponse(HttpContext context, int statusCode, string message)
    {
        context.Response.StatusCode  = statusCode;
        context.Response.ContentType = "application/json";
        var response = new { error = message, statusCode };
        await context.Response.WriteAsJsonAsync(response);
    }
}

// Registration in Program.cs (must be first in pipeline)
app.UseMiddleware<GlobalExceptionMiddleware>();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 8. DECORATOR PATTERN

<br/>

## Q. What is the Decorator Pattern? How is it implemented in .NET Core?

The Decorator Pattern attaches additional responsibilities to an object dynamically. It provides a flexible alternative to subclassing for extending functionality.

**Use Cases:**
- Caching layers over repositories
- Logging/auditing wrappers
- Retry and circuit breaker policies
- Compression/encryption layers

```csharp
// Base interface
public interface IProductRepository
{
    Task<Product> GetByIdAsync(int id);
    Task<IEnumerable<Product>> GetAllAsync();
}

// Concrete implementation
public class ProductRepository : IProductRepository
{
    private readonly AppDbContext _context;
    public ProductRepository(AppDbContext context) => _context = context;

    public async Task<Product> GetByIdAsync(int id)
        => await _context.Products.FindAsync(id);

    public async Task<IEnumerable<Product>> GetAllAsync()
        => await _context.Products.ToListAsync();
}

// Caching decorator
public class CachedProductRepository : IProductRepository
{
    private readonly IProductRepository _inner;
    private readonly IMemoryCache _cache;

    public CachedProductRepository(IProductRepository inner, IMemoryCache cache)
    {
        _inner = inner;
        _cache = cache;
    }

    public async Task<Product> GetByIdAsync(int id)
    {
        var cacheKey = $"product_{id}";
        if (_cache.TryGetValue(cacheKey, out Product cached))
            return cached;

        var product = await _inner.GetByIdAsync(id);
        _cache.Set(cacheKey, product, TimeSpan.FromMinutes(10));
        return product;
    }

    public async Task<IEnumerable<Product>> GetAllAsync()
    {
        const string cacheKey = "products_all";
        if (_cache.TryGetValue(cacheKey, out IEnumerable<Product> cached))
            return cached;

        var products = await _inner.GetAllAsync();
        _cache.Set(cacheKey, products, TimeSpan.FromMinutes(5));
        return products;
    }
}

// Logging decorator
public class LoggingProductRepository : IProductRepository
{
    private readonly IProductRepository _inner;
    private readonly ILogger<LoggingProductRepository> _logger;

    public LoggingProductRepository(IProductRepository inner, ILogger<LoggingProductRepository> logger)
    {
        _inner  = inner;
        _logger = logger;
    }

    public async Task<Product> GetByIdAsync(int id)
    {
        _logger.LogInformation("Fetching product {Id}", id);
        var result = await _inner.GetByIdAsync(id);
        _logger.LogInformation("Product {Id} fetched: {Found}", id, result != null);
        return result;
    }

    public async Task<IEnumerable<Product>> GetAllAsync()
    {
        _logger.LogInformation("Fetching all products");
        var results = await _inner.GetAllAsync();
        _logger.LogInformation("Fetched {Count} products", results.Count());
        return results;
    }
}

// Registration with Scrutor library (decorator support)
// Install: dotnet add package Scrutor
builder.Services.AddScoped<IProductRepository, ProductRepository>();
builder.Services.Decorate<IProductRepository, CachedProductRepository>();
builder.Services.Decorate<IProductRepository, LoggingProductRepository>();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 9. PROXY PATTERN

<br/>

## Q. What is the Proxy Pattern? Provide a .NET Core example.

The Proxy Pattern provides a surrogate or placeholder for another object to control access to it.

**Types:**

| Type               | Purpose                                      |
|--------------------|----------------------------------------------|
| Virtual Proxy      | Lazy initialization of expensive objects     |
| Protection Proxy   | Access control / authorization               |
| Remote Proxy       | Represent objects in a different address space |
| Caching Proxy      | Cache results of expensive operations        |
| Logging Proxy      | Record calls to the real subject             |

```csharp
// Subject interface
public interface IExpensiveService
{
    Task<string> GetDataAsync(string query);
}

// Real implementation (expensive/slow)
public class ExpensiveExternalService : IExpensiveService
{
    public async Task<string> GetDataAsync(string query)
    {
        await Task.Delay(2000); // Simulate slow external call
        return $"Data for: {query}";
    }
}

// Caching Proxy
public class CachingProxy : IExpensiveService
{
    private readonly IExpensiveService _realService;
    private readonly IMemoryCache _cache;

    public CachingProxy(IExpensiveService realService, IMemoryCache cache)
    {
        _realService = realService;
        _cache       = cache;
    }

    public async Task<string> GetDataAsync(string query)
    {
        if (_cache.TryGetValue(query, out string cachedResult))
            return cachedResult;

        var result = await _realService.GetDataAsync(query);
        _cache.Set(query, result, TimeSpan.FromMinutes(5));
        return result;
    }
}

// Protection Proxy
public class AuthorizationProxy : IExpensiveService
{
    private readonly IExpensiveService _realService;
    private readonly IHttpContextAccessor _httpContextAccessor;

    public AuthorizationProxy(IExpensiveService realService, IHttpContextAccessor httpContextAccessor)
    {
        _realService         = realService;
        _httpContextAccessor = httpContextAccessor;
    }

    public async Task<string> GetDataAsync(string query)
    {
        var user = _httpContextAccessor.HttpContext?.User;
        if (user?.Identity?.IsAuthenticated != true)
            throw new UnauthorizedAccessException("Access denied");

        return await _realService.GetDataAsync(query);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 10. COMMAND PATTERN

<br/>

## Q. What is the Command Pattern? Implement it with undo/redo support.

The Command Pattern encapsulates a request as an object, thereby allowing parameterization of clients with different requests, queuing of requests, logging, and undo operations.

**Use Cases:**
- Undo/Redo operations (text editors, drawing apps)
- Task queues and job schedulers
- Transaction management
- Audit logging
- CQRS (Command Query Responsibility Segregation)

```csharp
// Command interface
public interface ICommand
{
    void Execute();
    void Undo();
}

// Receiver
public class TextEditor
{
    private readonly StringBuilder _content = new();

    public void InsertText(string text, int position)
    {
        _content.Insert(position, text);
        Console.WriteLine($"Content: {_content}");
    }

    public void DeleteText(int position, int length)
    {
        _content.Remove(position, length);
        Console.WriteLine($"Content: {_content}");
    }

    public string GetContent() => _content.ToString();
}

// Concrete command
public class InsertTextCommand : ICommand
{
    private readonly TextEditor _editor;
    private readonly string _text;
    private readonly int _position;

    public InsertTextCommand(TextEditor editor, string text, int position)
    {
        _editor   = editor;
        _text     = text;
        _position = position;
    }

    public void Execute() => _editor.InsertText(_text, _position);
    public void Undo()    => _editor.DeleteText(_position, _text.Length);
}

// Command Manager (Invoker) with undo/redo stacks
public class CommandManager
{
    private readonly Stack<ICommand> _undoStack = new();
    private readonly Stack<ICommand> _redoStack = new();

    public void Execute(ICommand command)
    {
        command.Execute();
        _undoStack.Push(command);
        _redoStack.Clear();
    }

    public void Undo()
    {
        if (!_undoStack.Any()) return;
        var command = _undoStack.Pop();
        command.Undo();
        _redoStack.Push(command);
    }

    public void Redo()
    {
        if (!_redoStack.Any()) return;
        var command = _redoStack.Pop();
        command.Execute();
        _undoStack.Push(command);
    }
}

// Usage
var editor  = new TextEditor();
var manager = new CommandManager();

manager.Execute(new InsertTextCommand(editor, "Hello ", 0));
manager.Execute(new InsertTextCommand(editor, "World", 6));
manager.Undo(); // removes "World"
manager.Redo(); // re-inserts "World"
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does CQRS relate to the Command Pattern in .NET Core?

CQRS separates read (Query) and write (Command) operations, directly applying the Command Pattern for write side.

```csharp
// Command (write operation)
public record CreateProductCommand(string Name, decimal Price, int Stock);

public class CreateProductCommandHandler
{
    private readonly AppDbContext _context;
    public CreateProductCommandHandler(AppDbContext context) => _context = context;

    public async Task<int> HandleAsync(CreateProductCommand command)
    {
        var product = new Product { Name = command.Name, Price = command.Price, Stock = command.Stock };
        _context.Products.Add(product);
        await _context.SaveChangesAsync();
        return product.Id;
    }
}

// Query (read operation)
public record GetProductByIdQuery(int Id);

public class GetProductByIdQueryHandler
{
    private readonly AppDbContext _context;
    public GetProductByIdQueryHandler(AppDbContext context) => _context = context;

    public async Task<ProductDto?> HandleAsync(GetProductByIdQuery query)
        => await _context.Products
            .Where(p => p.Id == query.Id)
            .Select(p => new ProductDto(p.Id, p.Name, p.Price))
            .FirstOrDefaultAsync();
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 11. MODULE PATTERN

<br/>

## Q. What is the Module Pattern in .NET Core?

The Module Pattern organizes code into self-contained, cohesive units with clear boundaries. In .NET Core, this is achieved through feature folders, extension methods, and modular service registration.

```csharp
// Module interface
public interface IModule
{
    IServiceCollection RegisterModule(IServiceCollection services, IConfiguration configuration);
    WebApplication MapEndpoints(WebApplication app);
}

// Orders feature module
public class OrdersModule : IModule
{
    public IServiceCollection RegisterModule(IServiceCollection services, IConfiguration configuration)
    {
        services.AddScoped<IOrderRepository, OrderRepository>();
        services.AddScoped<IOrderService, OrderService>();
        return services;
    }

    public WebApplication MapEndpoints(WebApplication app)
    {
        app.MapGet("/orders",          GetAllOrders);
        app.MapGet("/orders/{id:int}", GetOrderById);
        app.MapPost("/orders",         CreateOrder);
        return app;
    }

    private static async Task<IResult> GetAllOrders(IOrderService service)
        => Results.Ok(await service.GetAllAsync());

    private static async Task<IResult> GetOrderById(int id, IOrderService service)
    {
        var order = await service.GetByIdAsync(id);
        return order is null ? Results.NotFound() : Results.Ok(order);
    }

    private static async Task<IResult> CreateOrder(CreateOrderRequest req, IOrderService service)
        => Results.Created($"/orders/{req.Id}", await service.CreateAsync(req));
}

// Auto-discovery of all modules in Program.cs
var modules = Assembly.GetExecutingAssembly()
    .GetTypes()
    .Where(t => typeof(IModule).IsAssignableFrom(t) && !t.IsInterface)
    .Select(Activator.CreateInstance)
    .Cast<IModule>()
    .ToList();

foreach (var module in modules)
    module.RegisterModule(builder.Services, builder.Configuration);

var app = builder.Build();

foreach (var module in modules)
    module.MapEndpoints(app);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 12. DEPENDENCY INJECTION PATTERN

<br/>

## Q. How do you implement the Options Pattern in .NET Core?

```csharp
// Options class
public class SmtpOptions
{
    public const string SectionName = "Smtp";

    [Required]
    public string Host { get; init; } = string.Empty;

    [Range(1, 65535)]
    public int Port { get; init; } = 587;

    public bool UseSsl { get; init; } = true;

    [Required]
    public string Username { get; init; } = string.Empty;
}

// appsettings.json
// {
//   "Smtp": { "Host": "smtp.gmail.com", "Port": 587, "UseSsl": true, "Username": "user@gmail.com" }
// }

// Registration with validation
builder.Services
    .AddOptions<SmtpOptions>()
    .BindConfiguration(SmtpOptions.SectionName)
    .ValidateDataAnnotations()
    .ValidateOnStart();

// Consumption
public class EmailService
{
    private readonly SmtpOptions _options;

    public EmailService(IOptions<SmtpOptions> options)
    {
        _options = options.Value;
    }
}
```

| `IOptions<T>` variant  | Reloads on change | Scoped |
|------------------------|-------------------|--------|
| `IOptions<T>`          | No                | No     |
| `IOptionsSnapshot<T>`  | Yes               | Yes    |
| `IOptionsMonitor<T>`   | Yes               | No     |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you auto-register services by convention in .NET Core?

```csharp
public static class ServiceCollectionExtensions
{
    public static IServiceCollection AddApplicationServices(
        this IServiceCollection services,
        Assembly assembly)
    {
        var serviceTypes = assembly.GetTypes()
            .Where(t => t.IsClass && !t.IsAbstract)
            .SelectMany(t => t.GetInterfaces()
                .Where(i => i.Name == $"I{t.Name}")
                .Select(i => new { Interface = i, Implementation = t }));

        foreach (var type in serviceTypes)
            services.AddScoped(type.Interface, type.Implementation);

        return services;
    }
}

// Usage
builder.Services.AddApplicationServices(typeof(Program).Assembly);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 13. REPOSITORY PATTERN

<br/>

## Q. Implement the Repository Pattern with Unit of Work in .NET Core EF Core.

The Repository Pattern abstracts the data access layer, decoupling business logic from data access logic.

```csharp
// Generic repository interface
public interface IRepository<T> where T : class
{
    Task<T?>               GetByIdAsync(int id, CancellationToken ct = default);
    Task<IEnumerable<T>>   GetAllAsync(CancellationToken ct = default);
    Task<IEnumerable<T>>   FindAsync(Expression<Func<T, bool>> predicate, CancellationToken ct = default);
    Task                   AddAsync(T entity, CancellationToken ct = default);
    void                   Update(T entity);
    void                   Remove(T entity);
}

// Generic implementation
public class Repository<T> : IRepository<T> where T : class
{
    protected readonly AppDbContext _context;
    protected readonly DbSet<T> _dbSet;

    public Repository(AppDbContext context)
    {
        _context = context;
        _dbSet   = context.Set<T>();
    }

    public async Task<T?> GetByIdAsync(int id, CancellationToken ct = default)
        => await _dbSet.FindAsync(new object[] { id }, ct);

    public async Task<IEnumerable<T>> GetAllAsync(CancellationToken ct = default)
        => await _dbSet.ToListAsync(ct);

    public async Task<IEnumerable<T>> FindAsync(Expression<Func<T, bool>> predicate, CancellationToken ct = default)
        => await _dbSet.Where(predicate).ToListAsync(ct);

    public async Task AddAsync(T entity, CancellationToken ct = default)
        => await _dbSet.AddAsync(entity, ct);

    public void Update(T entity) => _dbSet.Update(entity);
    public void Remove(T entity) => _dbSet.Remove(entity);
}

// Specific repository with domain queries
public interface IProductRepository : IRepository<Product>
{
    Task<IEnumerable<Product>> GetByCategoryAsync(int categoryId, CancellationToken ct = default);
    Task<IEnumerable<Product>> GetLowStockAsync(int threshold, CancellationToken ct = default);
}

public class ProductRepository : Repository<Product>, IProductRepository
{
    public ProductRepository(AppDbContext context) : base(context) { }

    public async Task<IEnumerable<Product>> GetByCategoryAsync(int categoryId, CancellationToken ct = default)
        => await _dbSet.Where(p => p.CategoryId == categoryId).ToListAsync(ct);

    public async Task<IEnumerable<Product>> GetLowStockAsync(int threshold, CancellationToken ct = default)
        => await _dbSet.Where(p => p.Stock < threshold).ToListAsync(ct);
}

// Unit of Work
public interface IUnitOfWork : IDisposable
{
    IProductRepository Products { get; }
    IOrderRepository   Orders   { get; }
    Task<int> SaveChangesAsync(CancellationToken ct = default);
}

public class UnitOfWork : IUnitOfWork
{
    private readonly AppDbContext _context;

    public UnitOfWork(AppDbContext context)
    {
        _context = context;
        Products = new ProductRepository(context);
        Orders   = new OrderRepository(context);
    }

    public IProductRepository Products { get; }
    public IOrderRepository   Orders   { get; }

    public async Task<int> SaveChangesAsync(CancellationToken ct = default)
        => await _context.SaveChangesAsync(ct);

    public void Dispose() => _context.Dispose();
}

// Registration
builder.Services.AddScoped<IUnitOfWork, UnitOfWork>();

// Usage
public class OrderService
{
    private readonly IUnitOfWork _uow;
    public OrderService(IUnitOfWork uow) => _uow = uow;

    public async Task CreateOrderAsync(CreateOrderRequest request)
    {
        var product = await _uow.Products.GetByIdAsync(request.ProductId);
        if (product is null) throw new NotFoundException("Product not found");

        var order = new Order { ProductId = request.ProductId, Quantity = request.Quantity };
        await _uow.Orders.AddAsync(order);
        await _uow.SaveChangesAsync();
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 14. DELEGATE PATTERN

<br/>

## Q. What is the Delegate Pattern in C#? How does it enable flexible behavior?

Delegates are type-safe function pointers. The Delegate Pattern uses `Func<>`, `Action<>`, and `Predicate<>` to inject behavior at runtime.

```csharp
// Using Func<>, Action<>, Predicate<>
public class OrderProcessor
{
    private readonly Func<decimal, int, decimal> _priceCalculator;
    private readonly Action<Order> _onOrderCreated;
    private readonly Predicate<Order> _orderValidator;

    public OrderProcessor(
        Func<decimal, int, decimal> priceCalculator,
        Action<Order> onOrderCreated,
        Predicate<Order> orderValidator)
    {
        _priceCalculator = priceCalculator;
        _onOrderCreated  = onOrderCreated;
        _orderValidator  = orderValidator;
    }

    public Order Process(decimal price, int qty)
    {
        var total = _priceCalculator(price, qty);
        var order = new Order { Total = total };

        if (!_orderValidator(order))
            throw new ValidationException("Invalid order");

        _onOrderCreated(order);
        return order;
    }
}

// Usage with lambda expressions
var processor = new OrderProcessor(
    priceCalculator: (price, qty) => price * qty * 0.9m,       // 10% bulk discount
    onOrderCreated:  order => Console.WriteLine($"Order {order.Id} created"),
    orderValidator:  order => order.Total > 0
);

// Multicast delegate (event-like behavior)
Action<string> logger = msg => Console.WriteLine($"[Console] {msg}");
logger += msg => File.AppendAllText("log.txt", msg + "\n");
logger += msg => Debug.WriteLine($"[Debug] {msg}");

logger("Application started"); // All three handlers fire
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 15. MEDIATOR PATTERN

<br/>

## Q. What is the Mediator Pattern? How does MediatR implement it in .NET Core?

The Mediator Pattern reduces direct dependencies between objects by making them communicate through a mediator. This promotes loose coupling and is the foundation of CQRS.

```csharp
// Install: dotnet add package MediatR

// Command (request with response)
public record CreateUserCommand(string Name, string Email) : IRequest<int>;

// Command handler
public class CreateUserCommandHandler : IRequestHandler<CreateUserCommand, int>
{
    private readonly AppDbContext _context;
    private readonly IPublisher _publisher;

    public CreateUserCommandHandler(AppDbContext context, IPublisher publisher)
    {
        _context   = context;
        _publisher = publisher;
    }

    public async Task<int> Handle(CreateUserCommand request, CancellationToken cancellationToken)
    {
        var user = new User { Name = request.Name, Email = request.Email };
        _context.Users.Add(user);
        await _context.SaveChangesAsync(cancellationToken);

        await _publisher.Publish(new UserCreatedNotification(user.Id), cancellationToken);
        return user.Id;
    }
}

// Notification (fan-out to multiple handlers)
public record UserCreatedNotification(int UserId) : INotification;

public class SendWelcomeEmailHandler : INotificationHandler<UserCreatedNotification>
{
    public async Task Handle(UserCreatedNotification notification, CancellationToken cancellationToken)
        => Console.WriteLine($"Welcome email sent to user {notification.UserId}");
}

public class CreateDefaultSettingsHandler : INotificationHandler<UserCreatedNotification>
{
    public async Task Handle(UserCreatedNotification notification, CancellationToken cancellationToken)
        => Console.WriteLine($"Default settings created for user {notification.UserId}");
}

// Pipeline behavior (cross-cutting concerns: logging, validation, caching)
public class LoggingBehavior<TRequest, TResponse> : IPipelineBehavior<TRequest, TResponse>
    where TRequest : notnull
{
    private readonly ILogger<LoggingBehavior<TRequest, TResponse>> _logger;

    public LoggingBehavior(ILogger<LoggingBehavior<TRequest, TResponse>> logger)
        => _logger = logger;

    public async Task<TResponse> Handle(
        TRequest request,
        RequestHandlerDelegate<TResponse> next,
        CancellationToken cancellationToken)
    {
        var name = typeof(TRequest).Name;
        _logger.LogInformation("Handling {Request}", name);
        var response = await next();
        _logger.LogInformation("Handled {Request}", name);
        return response;
    }
}

// Registration
builder.Services.AddMediatR(cfg =>
    cfg.RegisterServicesFromAssembly(typeof(Program).Assembly));
builder.Services.AddTransient(typeof(IPipelineBehavior<,>), typeof(LoggingBehavior<,>));

// Usage in controller
public class UserController : ControllerBase
{
    private readonly ISender _sender;
    public UserController(ISender sender) => _sender = sender;

    [HttpPost]
    public async Task<IActionResult> Create([FromBody] CreateUserCommand command)
    {
        var id = await _sender.Send(command);
        return CreatedAtAction(nameof(GetById), new { id }, null);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 16. ADAPTER PATTERN

<br/>

## Q. What is the Adapter Pattern? Provide a .NET Core integration example.

The Adapter Pattern converts the interface of a class into another interface that clients expect. It allows incompatible interfaces to work together.

**Use Cases:**
- Wrapping third-party libraries behind your own interface
- Legacy system integration
- API gateway pattern
- Data format conversion

```csharp
// Target interface (what our application expects)
public interface IPaymentGateway
{
    Task<PaymentResponse> ChargeAsync(string customerId, decimal amount, string currency);
    Task<bool> RefundAsync(string transactionId, decimal amount);
}

// Adaptee — Third-party Stripe SDK (incompatible interface, cannot modify)
public class StripeClient
{
    public StripeCharge CreateCharge(StripeChargeCreateOptions options) => new StripeCharge();
    public StripeRefund CreateRefund(string chargeId, long amountInCents) => new StripeRefund();
}

// Adapter — bridges StripeClient to IPaymentGateway
public class StripePaymentGatewayAdapter : IPaymentGateway
{
    private readonly StripeClient _stripeClient;

    public StripePaymentGatewayAdapter(StripeClient stripeClient)
        => _stripeClient = stripeClient;

    public async Task<PaymentResponse> ChargeAsync(string customerId, decimal amount, string currency)
    {
        var options = new StripeChargeCreateOptions
        {
            Amount   = (long)(amount * 100), // Stripe uses smallest currency unit (cents)
            Currency = currency.ToLower(),
            Customer = customerId
        };

        var charge = _stripeClient.CreateCharge(options);
        return new PaymentResponse
        {
            TransactionId = charge.Id,
            Success       = charge.Status == "succeeded",
            Amount        = amount
        };
    }

    public async Task<bool> RefundAsync(string transactionId, decimal amount)
    {
        var refund = _stripeClient.CreateRefund(transactionId, (long)(amount * 100));
        return refund.Status == "succeeded";
    }
}

// Registration — consumer only knows IPaymentGateway
builder.Services.AddSingleton<StripeClient>();
builder.Services.AddScoped<IPaymentGateway, StripePaymentGatewayAdapter>();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 17. FACADE PATTERN

<br/>

## Q. What is the Facade Pattern? Implement a subsystem facade in .NET Core.

The Facade Pattern provides a **simplified interface** to a complex subsystem. It hides internal complexity behind a single unified interface.

**Use Cases:**
- Simplifying complex library or module usage
- Providing a clean API over internal subsystems
- Anti-corruption layer in Domain-Driven Design

```csharp
// Complex subsystems
public class InventoryService
{
    public async Task<bool> CheckStockAsync(int productId, int quantity) => true;
    public async Task ReserveStockAsync(int productId, int quantity) { }
}

public class PricingService
{
    public async Task<decimal> CalculatePriceAsync(int productId, int quantity, string couponCode)
        => 99.99m;
}

public class PaymentService
{
    public async Task<PaymentResult> ProcessPaymentAsync(string paymentToken, decimal amount)
        => new PaymentResult { Success = true, TransactionId = "tx_123" };
}

public class NotificationService
{
    public async Task SendOrderConfirmationAsync(string email, Order order) { }
}

public class ShippingService
{
    public async Task<string> ScheduleDeliveryAsync(Order order) => "tracking_123";
}

// FACADE — single entry point for the entire order flow
public class OrderFacade
{
    private readonly InventoryService    _inventory;
    private readonly PricingService      _pricing;
    private readonly PaymentService      _payment;
    private readonly NotificationService _notification;
    private readonly ShippingService     _shipping;

    public OrderFacade(
        InventoryService inventory,
        PricingService pricing,
        PaymentService payment,
        NotificationService notification,
        ShippingService shipping)
    {
        _inventory    = inventory;
        _pricing      = pricing;
        _payment      = payment;
        _notification = notification;
        _shipping     = shipping;
    }

    public async Task<OrderResult> PlaceOrderAsync(PlaceOrderRequest request)
    {
        if (!await _inventory.CheckStockAsync(request.ProductId, request.Quantity))
            return OrderResult.Failure("Out of stock");

        var price   = await _pricing.CalculatePriceAsync(request.ProductId, request.Quantity, request.CouponCode);
        var payment = await _payment.ProcessPaymentAsync(request.PaymentToken, price);

        if (!payment.Success)
            return OrderResult.Failure("Payment failed");

        await _inventory.ReserveStockAsync(request.ProductId, request.Quantity);
        var order      = new Order { Id = Guid.NewGuid(), Total = price };
        var trackingId = await _shipping.ScheduleDeliveryAsync(order);
        await _notification.SendOrderConfirmationAsync(request.Email, order);

        return OrderResult.Success(order.Id, trackingId);
    }
}

// Controller talks only to the facade — not to individual subsystems
public class OrderController : ControllerBase
{
    private readonly OrderFacade _facade;
    public OrderController(OrderFacade facade) => _facade = facade;

    [HttpPost]
    public async Task<IActionResult> PlaceOrder([FromBody] PlaceOrderRequest request)
    {
        var result = await _facade.PlaceOrderAsync(request);
        return result.IsSuccess ? Ok(result) : BadRequest(result.Error);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 18. BUILDER PATTERN

<br/>

## Q. What is the Builder Pattern? Provide a fluent builder example in .NET Core.

The Builder Pattern separates the construction of a complex object from its representation, enabling the same construction process to produce different representations.

**Use Cases:**
- Complex object creation (emails, queries, HTTP requests)
- Test data builders
- Configuration construction
- `WebApplicationBuilder` in ASP.NET Core is a built-in example

```csharp
// Product
public class Email
{
    public string From        { get; internal set; }
    public string To          { get; internal set; }
    public string Subject     { get; internal set; }
    public string Body        { get; internal set; }
    public bool   IsHtml      { get; internal set; }
    public List<string> Cc           { get; } = new();
    public List<string> Attachments  { get; } = new();
}

// Fluent Builder
public class EmailBuilder
{
    private readonly Email _email = new();

    public EmailBuilder From(string from)            { _email.From    = from;           return this; }
    public EmailBuilder To(string to)                { _email.To      = to;             return this; }
    public EmailBuilder WithSubject(string subject)  { _email.Subject = subject;        return this; }

    public EmailBuilder WithHtmlBody(string html)
    {
        _email.Body   = html;
        _email.IsHtml = true;
        return this;
    }

    public EmailBuilder WithCc(params string[] addresses)
    {
        _email.Cc.AddRange(addresses);
        return this;
    }

    public EmailBuilder WithAttachment(string filePath)
    {
        _email.Attachments.Add(filePath);
        return this;
    }

    public Email Build()
    {
        if (string.IsNullOrEmpty(_email.From))    throw new InvalidOperationException("From is required");
        if (string.IsNullOrEmpty(_email.To))      throw new InvalidOperationException("To is required");
        if (string.IsNullOrEmpty(_email.Subject)) throw new InvalidOperationException("Subject is required");
        return _email;
    }
}

// Usage
var email = new EmailBuilder()
    .From("sender@example.com")
    .To("recipient@example.com")
    .WithSubject("Order Confirmation #12345")
    .WithHtmlBody("<h1>Your order is confirmed!</h1>")
    .WithCc("support@example.com", "billing@example.com")
    .WithAttachment("/invoices/invoice_12345.pdf")
    .Build();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does the Builder Pattern apply to dynamic query construction?

```csharp
public class ProductQueryBuilder
{
    private IQueryable<Product> _query;

    public ProductQueryBuilder(AppDbContext context)
    {
        _query = context.Products.AsQueryable();
    }

    public ProductQueryBuilder InCategory(int categoryId)
    {
        _query = _query.Where(p => p.CategoryId == categoryId);
        return this;
    }

    public ProductQueryBuilder WithPriceBetween(decimal min, decimal max)
    {
        _query = _query.Where(p => p.Price >= min && p.Price <= max);
        return this;
    }

    public ProductQueryBuilder InStock()
    {
        _query = _query.Where(p => p.Stock > 0);
        return this;
    }

    public ProductQueryBuilder OrderByPrice(bool ascending = true)
    {
        _query = ascending
            ? _query.OrderBy(p => p.Price)
            : _query.OrderByDescending(p => p.Price);
        return this;
    }

    public ProductQueryBuilder Take(int count)
    {
        _query = _query.Take(count);
        return this;
    }

    public Task<List<Product>> ExecuteAsync() => _query.ToListAsync();
}

// Usage
var products = await new ProductQueryBuilder(context)
    .InCategory(5)
    .WithPriceBetween(10, 500)
    .InStock()
    .OrderByPrice()
    .Take(20)
    .ExecuteAsync();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 19. SPECIFICATION PATTERN

<br/>

## Q. What is the Specification Pattern and how does it compose with the Repository Pattern?

The Specification Pattern encapsulates a business rule or query criteria as a reusable, composable, testable object.

```csharp
// Abstract base specification
public abstract class Specification<T>
{
    public abstract Expression<Func<T, bool>> ToExpression();

    public bool IsSatisfiedBy(T entity) => ToExpression().Compile()(entity);

    public Specification<T> And(Specification<T> other) => new AndSpecification<T>(this, other);
    public Specification<T> Or(Specification<T> other)  => new OrSpecification<T>(this, other);
    public Specification<T> Not()                        => new NotSpecification<T>(this);
}

// Composite: AND
public class AndSpecification<T> : Specification<T>
{
    private readonly Specification<T> _left, _right;

    public AndSpecification(Specification<T> left, Specification<T> right)
    {
        _left  = left;
        _right = right;
    }

    public override Expression<Func<T, bool>> ToExpression()
    {
        var leftExpr  = _left.ToExpression();
        var rightExpr = _right.ToExpression();
        var param     = Expression.Parameter(typeof(T));
        var body      = Expression.AndAlso(
            Expression.Invoke(leftExpr, param),
            Expression.Invoke(rightExpr, param));
        return Expression.Lambda<Func<T, bool>>(body, param);
    }
}

// Composite: NOT
public class NotSpecification<T> : Specification<T>
{
    private readonly Specification<T> _inner;
    public NotSpecification(Specification<T> inner) => _inner = inner;

    public override Expression<Func<T, bool>> ToExpression()
    {
        var expr  = _inner.ToExpression();
        var param = Expression.Parameter(typeof(T));
        var body  = Expression.Not(Expression.Invoke(expr, param));
        return Expression.Lambda<Func<T, bool>>(body, param);
    }
}

// Domain-specific specifications
public class ActiveProductSpecification : Specification<Product>
{
    public override Expression<Func<Product, bool>> ToExpression()
        => product => product.IsActive;
}

public class InCategorySpecification : Specification<Product>
{
    private readonly int _categoryId;
    public InCategorySpecification(int categoryId) => _categoryId = categoryId;

    public override Expression<Func<Product, bool>> ToExpression()
        => product => product.CategoryId == _categoryId;
}

public class PriceRangeSpecification : Specification<Product>
{
    private readonly decimal _min, _max;
    public PriceRangeSpecification(decimal min, decimal max) { _min = min; _max = max; }

    public override Expression<Func<Product, bool>> ToExpression()
        => product => product.Price >= _min && product.Price <= _max;
}

// Repository with Specification support
public interface IRepository<T> where T : class
{
    Task<IEnumerable<T>> FindAsync(Specification<T> spec);
}

// Usage — composing business rules
var activeSpec   = new ActiveProductSpecification();
var categorySpec = new InCategorySpecification(categoryId: 3);
var priceSpec    = new PriceRangeSpecification(min: 10, max: 200);

var combinedSpec = activeSpec.And(categorySpec).And(priceSpec);

var products = await _repository.FindAsync(combinedSpec);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 20. ANTI-PATTERN & CODE SMELL

<br/>

## Q. What are common Anti-Patterns in .NET Core applications?

**1. Service Locator Anti-Pattern**

```csharp
// BAD — hides dependencies, makes testing difficult
public class OrderService
{
    public void ProcessOrder(Order order)
    {
        var emailService = ServiceLocator.Resolve<IEmailService>(); // hidden dependency!
        emailService.Send(order.CustomerEmail, "Order confirmed");
    }
}

// GOOD — explicit constructor injection
public class OrderService
{
    private readonly IEmailService _emailService;
    public OrderService(IEmailService emailService) => _emailService = emailService;

    public void ProcessOrder(Order order)
        => _emailService.Send(order.CustomerEmail, "Order confirmed");
}
```

**2. God Class**

```csharp
// BAD — one class does everything (violates SRP)
public class ApplicationManager
{
    public void ProcessOrder(Order order) { }
    public void SendEmail(string to, string body) { }
    public void SaveToDatabase(object entity) { }
    public void LogActivity(string message) { }
    public decimal CalculateTax(decimal amount) => amount * 0.2m;
    // ... dozens more methods
}

// GOOD — focused, single-responsibility classes
public class OrderProcessor    { public void Process(Order o) { } }
public class EmailService      { public void Send(string to, string body) { } }
public class TaxCalculator     { public decimal Calculate(decimal a) => a * 0.2m; }
```

**3. Primitive Obsession**

```csharp
// BAD — raw primitives for domain concepts
public void CreateUser(string email, string phone) { }

// GOOD — value objects with built-in validation
public record Email
{
    public string Value { get; }
    public Email(string value)
    {
        if (!value.Contains('@')) throw new ArgumentException("Invalid email");
        Value = value.ToLowerInvariant();
    }
}

public record PhoneNumber
{
    public string Value { get; }
    public PhoneNumber(string value)
    {
        if (!Regex.IsMatch(value, @"^\+?[\d\s\-]{10,}$"))
            throw new ArgumentException("Invalid phone number");
        Value = value;
    }
}
```

**4. N+1 Query Problem**

```csharp
// BAD — 1 query for orders + N queries for each customer (N+1)
var orders = await _context.Orders.ToListAsync();
foreach (var order in orders)
{
    var customerName = order.Customer.Name; // lazy load triggers a new DB query!
}

// GOOD — eager loading with Include
var orders = await _context.Orders
    .Include(o => o.Customer)
    .ToListAsync();
```

**5. Anemic Domain Model**

```csharp
// BAD — data bag with no behavior; all logic in external services
public class Order
{
    public int    Id     { get; set; }
    public string Status { get; set; } // just data, no rules enforced
}

// GOOD — rich domain model with encapsulated business rules
public class Order
{
    public int         Id     { get; private set; }
    public OrderStatus Status { get; private set; }

    public void Cancel()
    {
        if (Status == OrderStatus.Shipped)
            throw new InvalidOperationException("Cannot cancel a shipped order");
        Status = OrderStatus.Cancelled;
    }

    public void Complete()
    {
        if (Status != OrderStatus.Processing)
            throw new InvalidOperationException("Order must be processing to complete");
        Status = OrderStatus.Completed;
    }
}
```

**6. Magic Strings and Magic Numbers**

```csharp
// BAD
if (user.Role == "admin") { }
var timeout = 3600;

// GOOD — named constants
public static class Roles
{
    public const string Admin = "admin";
    public const string User  = "user";
}

public static class Timeouts
{
    public static readonly TimeSpan Session = TimeSpan.FromHours(1);
}

if (user.Role == Roles.Admin) { }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the Captive Dependency problem in .NET Core DI?

A **Captive Dependency** occurs when a longer-lived service holds a reference to a shorter-lived service, causing the shorter-lived service to live beyond its intended scope.

```csharp
// BAD — Singleton captures a Scoped service (captive dependency)
public class MySingletonService
{
    private readonly IScopedRepository _repository; // Scoped held forever by Singleton!

    public MySingletonService(IScopedRepository repository)
    {
        _repository = repository;
    }
}

// GOOD — use IServiceScopeFactory to create a fresh scope on demand
public class MySingletonService
{
    private readonly IServiceScopeFactory _scopeFactory;

    public MySingletonService(IServiceScopeFactory scopeFactory)
        => _scopeFactory = scopeFactory;

    public async Task DoWorkAsync()
    {
        using var scope      = _scopeFactory.CreateScope();
        var repository       = scope.ServiceProvider.GetRequiredService<IScopedRepository>();
        await repository.DoSomethingAsync();
    } // scope disposed — repository cleaned up correctly
}
```

**Enable detection in development:**
```csharp
builder.Host.UseDefaultServiceProvider(options =>
{
    options.ValidateScopes  = builder.Environment.IsDevelopment();
    options.ValidateOnBuild = true;
});
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>
