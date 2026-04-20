# .NET Unit Testing using xUnit

> *Click &#9733; if you like the project. Your contributions are heartily ♡ welcome.*

<br/>

## Table of Contents

* [Getting Started](#-1-getting-started)
* [Test Attributes & Lifecycle](#-2-test-attributes--lifecycle)
* [Assertions](#-3-assertions)
* [Data-Driven Tests](#-4-data-driven-tests)
* [Mocking with Moq](#-5-mocking-with-moq)
* [Async Testing](#-6-async-testing)
* [Test Fixtures & Shared Context](#-7-test-fixtures--shared-context)
* [Testing ASP.NET Core](#-8-testing-aspnet-core)
* [Testing EF Core](#-9-testing-ef-core)
* [Code Coverage & Best Practices](#-10-code-coverage--best-practices)

<br/>

## # 1. GETTING STARTED

<br/>

## Q. What is xUnit and how does it differ from NUnit and MSTest?

**xUnit** is a modern, open-source unit testing framework for .NET, created by the original author of NUnit. It is the default testing framework for .NET SDK templates and is widely used in Microsoft\'s own OSS repositories.

**Key differences:**

| Feature | xUnit | NUnit | MSTest |
|---------|-------|-------|--------|
| Per-test instance | ✅ New instance per test | ❌ Shared instance | ❌ Shared instance |
| Setup method | Constructor | `[SetUp]` | `[TestInitialize]` |
| Teardown | `IDisposable.Dispose` | `[TearDown]` | `[TestCleanup]` |
| Fact (single test) | `[Fact]` | `[Test]` | `[TestMethod]` |
| Data-driven test | `[Theory]` + `[InlineData]` | `[TestCase]` | `[DataRow]` |
| Skip a test | `[Fact(Skip="reason")]` | `[Ignore]` | `[Ignore]` |
| Class-level setup | `IClassFixture<T>` | `[OneTimeSetUp]` | `[ClassInitialize]` |
| Output | `ITestOutputHelper` | `Console.WriteLine` | `Console.WriteLine` |

**Install xUnit packages:**

```bash
dotnet add package xunit
dotnet add package xunit.runner.visualstudio
dotnet add package Microsoft.NET.Test.Sdk
```

**Minimal project structure:**

```
MyApp/
  MyApp.csproj
  Services/
    CalculatorService.cs
MyApp.Tests/
  MyApp.Tests.csproj   ← references MyApp
  Services/
    CalculatorServiceTests.cs
```

```cs
// CalculatorService.cs
public class CalculatorService
{
    public int Add(int a, int b) => a + b;
    public int Divide(int a, int b)
    {
        if (b == 0) throw new DivideByZeroException("Divisor cannot be zero.");
        return a / b;
    }
}
```

```cs
// CalculatorServiceTests.cs
public class CalculatorServiceTests
{
    private readonly CalculatorService _sut = new();

    [Fact]
    public void Add_TwoPositiveNumbers_ReturnsSum()
    {
        var result = _sut.Add(3, 4);
        Assert.Equal(7, result);
    }
}
```

Run tests from the CLI:

```bash
dotnet test
dotnet test --verbosity normal
dotnet test --filter "FullyQualifiedName~CalculatorServiceTests"
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the Arrange-Act-Assert (AAA) pattern?

The **AAA** pattern is the standard structure for a unit test, separating setup, execution, and verification into three clearly labelled stages.

| Stage | Purpose |
|-------|---------|
| **Arrange** | Set up dependencies, create the object under test, prepare inputs |
| **Act** | Invoke the method being tested |
| **Assert** | Verify the output or side-effect matches expectations |

```cs
public class BankAccountTests
{
    [Fact]
    public void Deposit_PositiveAmount_IncreasesBalance()
    {
        // Arrange
        var account = new BankAccount(initialBalance: 100m);

        // Act
        account.Deposit(50m);

        // Assert
        Assert.Equal(150m, account.Balance);
    }

    [Fact]
    public void Withdraw_MoreThanBalance_ThrowsInvalidOperationException()
    {
        // Arrange
        var account = new BankAccount(initialBalance: 100m);

        // Act & Assert (combined when testing exceptions)
        Assert.Throws<InvalidOperationException>(() => account.Withdraw(200m));
    }
}
```

**Guidelines:**
- Each test should have exactly **one logical assertion** (it may have multiple `Assert.*` calls if they verify the same concept).
- Test method names should follow the pattern: `MethodName_Scenario_ExpectedBehaviour`.
- Keep each test small, fast (< 1 ms), and independent — tests must not rely on execution order.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between unit tests, integration tests, and end-to-end tests?

The **test pyramid** defines three layers of automated tests, each with different scope, speed, and maintenance cost.

```
         /\
        /  \   E2E Tests (few, slow, brittle)
       /----\
      /      \  Integration Tests (moderate)
     /--------\
    /          \  Unit Tests (many, fast, isolated)
   /____________\
```

| Aspect | Unit Tests | Integration Tests | End-to-End Tests |
|--------|-----------|------------------|-----------------|
| Scope | Single class/method | Multiple components (DB, HTTP) | Full application flow |
| Speed | < 1 ms each | Seconds each | Minutes each |
| Dependencies | All mocked/faked | Real or in-memory | Real system |
| Isolation | Complete | Partial | None |
| xUnit runner | ✅ | ✅ | ✅ (+ tools like Playwright) |

```cs
// Unit test — no real DB or HTTP
[Fact]
public void GetDiscount_PremiumUser_Returns20Percent()
{
    var pricingService = new PricingService();
    var discount = pricingService.GetDiscount(UserType.Premium);
    Assert.Equal(0.20m, discount);
}

// Integration test — real HTTP via WebApplicationFactory
[Fact]
public async Task GetProducts_ReturnsOk()
{
    await using var factory = new WebApplicationFactory<Program>();
    var client = factory.CreateClient();
    var response = await client.GetAsync("/api/products");
    response.EnsureSuccessStatusCode();
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 2. TEST ATTRIBUTES & LIFECYCLE

<br/>

## Q. What is the difference between [Fact] and [Theory]?

| Attribute | Description |
|-----------|-------------|
| `[Fact]` | A single, unconditional test. No parameters. |
| `[Theory]` | A data-driven test. Requires at least one data source attribute. Runs once per data row. |

**`[Fact]` example:**

```cs
[Fact]
public void IsEven_GivenEvenNumber_ReturnsTrue()
{
    Assert.True(MathHelper.IsEven(4));
}
```

**`[Theory]` with `[InlineData]`:**

```cs
[Theory]
[InlineData(2,  true)]
[InlineData(3,  false)]
[InlineData(0,  true)]
[InlineData(-4, true)]
public void IsEven_VariousInputs_ReturnsExpected(int number, bool expected)
{
    Assert.Equal(expected, MathHelper.IsEven(number));
}
```

**Skip a test with a reason:**

```cs
[Fact(Skip = "Feature not yet implemented — tracking issue #42")]
public void FutureFeature_AlwaysSkipped()
{
    Assert.True(false);
}
```

**Naming a test for the test runner display:**

```cs
[Fact(DisplayName = "Adding null product throws ArgumentNullException")]
public void AddProduct_NullInput_Throws()
{
    var service = new ProductService();
    Assert.Throws<ArgumentNullException>(() => service.Add(null!));
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How does xUnit manage test setup and teardown?

xUnit creates a **new instance** of the test class for every test. This ensures test isolation automatically — no shared mutable state between tests.

**Setup → use the constructor. Teardown → implement `IDisposable`.**

```cs
public class DatabaseTests : IDisposable
{
    private readonly SqliteConnection _connection;
    private readonly AppDbContext _db;

    // Runs before EACH test
    public DatabaseTests()
    {
        _connection = new SqliteConnection("DataSource=:memory:");
        _connection.Open();

        var options = new DbContextOptionsBuilder<AppDbContext>()
            .UseSqlite(_connection)
            .Options;

        _db = new AppDbContext(options);
        _db.Database.EnsureCreated();
    }

    [Fact]
    public void AddUser_PersistsToDatabase()
    {
        _db.Users.Add(new User { Name = "Alice" });
        _db.SaveChanges();
        Assert.Equal(1, _db.Users.Count());
    }

    // Runs after EACH test
    public void Dispose()
    {
        _db.Dispose();
        _connection.Dispose();
    }
}
```

**Async setup and teardown** using `IAsyncLifetime`:

```cs
public class AsyncSetupTests : IAsyncLifetime
{
    private HttpClient _client = null!;

    public async Task InitializeAsync()
    {
        // async setup — called before each test
        _client = new HttpClient();
        await _client.GetAsync("https://example.com/warmup");
    }

    [Fact]
    public async Task Fetch_ReturnsOk()
    {
        var response = await _client.GetAsync("https://example.com");
        Assert.Equal(HttpStatusCode.OK, response.StatusCode);
    }

    public async Task DisposeAsync()
    {
        // async teardown — called after each test
        _client.Dispose();
        await Task.CompletedTask;
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you categorize and filter tests using traits?

`[Trait]` attaches metadata (key-value pairs) to tests, enabling selective test runs.

```cs
public class OrderServiceTests
{
    [Fact]
    [Trait("Category", "Unit")]
    [Trait("Feature", "OrderCreation")]
    public void CreateOrder_ValidInput_ReturnsOrder()
    {
        // ...
    }

    [Fact]
    [Trait("Category", "Slow")]
    public void ProcessBulkOrders_ThousandItems_CompletesUnderTenSeconds()
    {
        // ...
    }
}
```

**Filter by trait at the CLI:**

```bash
# Run only Unit tests
dotnet test --filter "Category=Unit"

# Run tests matching a name pattern
dotnet test --filter "FullyQualifiedName~OrderService"

# Run tests NOT in the Slow category
dotnet test --filter "Category!=Slow"

# Combine filters
dotnet test --filter "Category=Unit&Feature=OrderCreation"
```

**Custom trait attribute (cleaner syntax):**

```cs
public class UnitTestAttribute : TraitAttribute
{
    public UnitTestAttribute() : base("Category", "Unit") { }
}

[Fact, UnitTest]
public void MyTest() { /* ... */ }
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 3. ASSERTIONS

<br/>

## Q. What assertion methods does xUnit provide?

xUnit\'s `Assert` class covers equality, exceptions, collections, strings, types, and more.

**Equality:**

```cs
Assert.Equal(42, result);                    // deep equality
Assert.NotEqual(0, result);
Assert.Equal(3.14, result, precision: 2);   // floating-point with tolerance
Assert.StrictEqual(42, result);             // uses == operator
```

**Null / Boolean:**

```cs
Assert.Null(value);
Assert.NotNull(value);
Assert.True(condition);
Assert.False(condition);
```

**Exceptions:**

```cs
// Assert type of exception
var ex = Assert.Throws<ArgumentNullException>(() => service.Process(null));
Assert.Equal("input", ex.ParamName);

// Async exceptions
var ex2 = await Assert.ThrowsAsync<HttpRequestException>(
    async () => await client.GetAsync("/bad-url"));
```

**Collections:**

```cs
Assert.Empty(list);
Assert.NotEmpty(list);
Assert.Single(list);                        // exactly 1 item
Assert.Equal(3, list.Count);
Assert.Contains("apple", fruits);
Assert.DoesNotContain("mango", fruits);
Assert.All(items, item => Assert.NotNull(item));   // all items satisfy predicate
```

**Strings:**

```cs
Assert.Equal("Hello", result, ignoreCase: true);
Assert.StartsWith("Hello", result);
Assert.EndsWith("World", result);
Assert.Contains("lo W", result);
Assert.Matches(@"^\d{4}-\d{2}-\d{2}$", dateString);  // regex
```

**Type checks:**

```cs
Assert.IsType<Order>(result);
Assert.IsAssignableFrom<IEnumerable<int>>(result);
var typed = Assert.IsType<OkObjectResult>(actionResult);
Assert.Equal(200, typed.StatusCode);
```

**Range:**

```cs
Assert.InRange(score, low: 0, high: 100);
Assert.NotInRange(score, low: 101, high: int.MaxValue);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you assert on exceptions including message and inner exception?

```cs
public class ParserTests
{
    [Fact]
    public void Parse_InvalidJson_ThrowsWithExpectedMessage()
    {
        var parser = new JsonParser();

        var ex = Assert.Throws<FormatException>(
            () => parser.Parse("{ invalid }"));

        // Assert on exception message
        Assert.Contains("Invalid JSON", ex.Message);
    }

    [Fact]
    public void Process_DatabaseError_ThrowsServiceExceptionWithInnerCause()
    {
        var mockRepo = new Mock<IOrderRepository>();
        mockRepo.Setup(r => r.Save(It.IsAny<Order>()))
                .Throws(new SqlException("Timeout"));

        var service = new OrderService(mockRepo.Object);

        var ex = Assert.Throws<ServiceException>(() => service.Place(new Order()));
        Assert.IsType<SqlException>(ex.InnerException);
    }

    [Fact]
    public void Divide_ByZero_ThrowsDivideByZeroException()
    {
        var calc = new CalculatorService();

        // Using Assert.Throws with return value for further inspection
        var ex = Assert.Throws<DivideByZeroException>(() => calc.Divide(10, 0));
        Assert.Equal("Divisor cannot be zero.", ex.Message);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you write custom assertions or use FluentAssertions?

**FluentAssertions** provides a more readable assertion DSL on top of xUnit:

```bash
dotnet add package FluentAssertions
```

```cs
using FluentAssertions;

public class ProductServiceTests
{
    [Fact]
    public void CreateProduct_ValidInput_SetsPropertiesCorrectly()
    {
        var service = new ProductService();
        var product = service.Create("Laptop", 999.99m);

        // Fluent chaining
        product.Should().NotBeNull();
        product.Name.Should().Be("Laptop");
        product.Price.Should().BeGreaterThan(0).And.BeLessThan(10_000m);
        product.CreatedAt.Should().BeCloseTo(DateTime.UtcNow, precision: TimeSpan.FromSeconds(1));
    }

    [Fact]
    public void GetTopProducts_ReturnsOrderedByPriceDescending()
    {
        var products = new ProductService().GetTop(3);

        products.Should().HaveCount(3)
                         .And.BeInDescendingOrder(p => p.Price)
                         .And.OnlyContain(p => p.IsActive);
    }

    [Fact]
    public void Parse_InvalidInput_ThrowsWithMessage()
    {
        var parser = new CsvParser();
        Action act = () => parser.Parse("");
        act.Should().Throw<ArgumentException>()
           .WithMessage("*empty*")
           .And.ParamName.Should().Be("csv");
    }
}
```

**Custom xUnit assertion (without FluentAssertions):**

```cs
public static class OrderAssert
{
    public static void ValidOrder(Order order)
    {
        Assert.NotNull(order);
        Assert.True(order.Id > 0, "Order ID must be positive.");
        Assert.NotEmpty(order.Items);
        Assert.True(order.Total > 0, "Order total must be positive.");
    }
}

// In test:
OrderAssert.ValidOrder(result);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 4. DATA-DRIVEN TESTS

<br/>

## Q. What are the data source attributes available in xUnit Theory tests?

xUnit provides three built-in data sources for `[Theory]` tests:

**1. `[InlineData]` — hardcoded inline values:**

```cs
[Theory]
[InlineData("hello", 5)]
[InlineData("",      0)]
[InlineData("xUnit", 5)]
public void StringLength_ReturnsExpected(string input, int expectedLength)
{
    Assert.Equal(expectedLength, input.Length);
}
```

**2. `[MemberData]` — from a static property or method:**

```cs
public class DivisionTests
{
    public static IEnumerable<object[]> DivisionCases =>
    [
        [10, 2,  5],
        [9,  3,  3],
        [100, 4, 25],
    ];

    [Theory]
    [MemberData(nameof(DivisionCases))]
    public void Divide_ReturnsQuotient(int a, int b, int expected)
    {
        Assert.Equal(expected, new CalculatorService().Divide(a, b));
    }
}
```

**3. `[ClassData]` — from a class implementing `IEnumerable<object[]>`:**

```cs
public class ValidEmailData : IEnumerable<object[]>
{
    public IEnumerator<object[]> GetEnumerator()
    {
        yield return ["user@example.com",     true];
        yield return ["invalid-email",        false];
        yield return ["another@domain.co.uk", true];
    }
    IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
}

public class EmailValidatorTests
{
    [Theory]
    [ClassData(typeof(ValidEmailData))]
    public void Validate_Email_ReturnsExpected(string email, bool expected)
    {
        Assert.Equal(expected, EmailValidator.IsValid(email));
    }
}
```

**Using strongly typed `TheoryData<T>` (.NET 5+ xUnit v2.4.1+):**

```cs
public static TheoryData<int, int, int> AddCases => new()
{
    { 1,  2, 3 },
    { -1, 1, 0 },
    { 0,  0, 0 },
};

[Theory]
[MemberData(nameof(AddCases))]
public void Add_ReturnsSum(int a, int b, int expected)
{
    Assert.Equal(expected, new CalculatorService().Add(a, b));
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you load test data from an external file (JSON/CSV)?

```cs
// TestData/products.json
// [{"name":"Laptop","price":999.99},{"name":"Mouse","price":29.99}]

public class ProductTests
{
    public static IEnumerable<object[]> ProductData()
    {
        var json = File.ReadAllText("TestData/products.json");
        var products = JsonSerializer.Deserialize<List<ProductDto>>(json)!;
        return products.Select(p => new object[] { p.Name, p.Price });
    }

    [Theory]
    [MemberData(nameof(ProductData))]
    public void Product_HasPositivePrice(string name, decimal price)
    {
        Assert.True(price > 0, $"Product '{name}' has non-positive price {price}");
    }
}
```

```cs
// Loading from CSV with CsvHelper
public class CsvDataSource : IEnumerable<object[]>
{
    public IEnumerator<object[]> GetEnumerator()
    {
        using var reader = new StreamReader("TestData/users.csv");
        using var csv = new CsvReader(reader, CultureInfo.InvariantCulture);
        foreach (var record in csv.GetRecords<UserDto>())
            yield return [record.Username, record.Email, record.IsActive];
    }
    IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 5. MOCKING WITH MOQ

<br/>

## Q. What is mocking and how do you use Moq with xUnit?

**Mocking** replaces real dependencies with controlled test doubles that return predictable values and record calls.

```bash
dotnet add package Moq
```

**Basic setup and verification:**

```cs
public interface IEmailSender
{
    Task SendAsync(string to, string subject, string body);
}

public class NotificationService(IEmailSender emailSender)
{
    public async Task NotifyAsync(string userEmail, string message)
    {
        ArgumentException.ThrowIfNullOrWhiteSpace(userEmail);
        await emailSender.SendAsync(userEmail, "Notification", message);
    }
}

public class NotificationServiceTests
{
    [Fact]
    public async Task NotifyAsync_ValidInput_CallsSendOnce()
    {
        // Arrange
        var mockSender = new Mock<IEmailSender>();
        mockSender
            .Setup(s => s.SendAsync(It.IsAny<string>(), It.IsAny<string>(), It.IsAny<string>()))
            .Returns(Task.CompletedTask);

        var service = new NotificationService(mockSender.Object);

        // Act
        await service.NotifyAsync("user@example.com", "Hello!");

        // Assert — verify the method was called exactly once with expected args
        mockSender.Verify(
            s => s.SendAsync("user@example.com", "Notification", "Hello!"),
            Times.Once);
    }

    [Fact]
    public async Task NotifyAsync_EmptyEmail_ThrowsArgumentException()
    {
        var service = new NotificationService(Mock.Of<IEmailSender>());
        await Assert.ThrowsAsync<ArgumentException>(
            () => service.NotifyAsync("", "msg"));
    }
}
```

**Setup return values:**

```cs
var mockRepo = new Mock<IProductRepository>();

// Return a specific value
mockRepo.Setup(r => r.GetByIdAsync(1))
        .ReturnsAsync(new Product { Id = 1, Name = "Laptop" });

// Return null for unknown IDs
mockRepo.Setup(r => r.GetByIdAsync(It.Is<int>(id => id != 1)))
        .ReturnsAsync((Product?)null);

// Throw an exception
mockRepo.Setup(r => r.DeleteAsync(It.IsAny<int>()))
        .ThrowsAsync(new UnauthorizedAccessException("No permission"));
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the different Moq verification options?

```cs
var mock = new Mock<IOrderRepository>();

// Called exactly N times
mock.Verify(r => r.SaveAsync(It.IsAny<Order>()), Times.Exactly(2));

// Called at least once
mock.Verify(r => r.SaveAsync(It.IsAny<Order>()), Times.AtLeastOnce);

// Never called
mock.Verify(r => r.DeleteAsync(It.IsAny<int>()), Times.Never);

// Called with specific argument matching
mock.Verify(r => r.SaveAsync(It.Is<Order>(o => o.Total > 100m)), Times.Once);

// Verify ALL setups were called (strict mocking)
mock.VerifyAll();

// MockBehavior.Strict — any un-Setup call throws
var strictMock = new Mock<IOrderRepository>(MockBehavior.Strict);
```

**Capturing arguments with callbacks:**

```cs
Order? capturedOrder = null;
mockRepo.Setup(r => r.SaveAsync(It.IsAny<Order>()))
        .Callback<Order>(order => capturedOrder = order)
        .ReturnsAsync(true);

await service.PlaceOrderAsync(new Order { CustomerId = 99 });

Assert.NotNull(capturedOrder);
Assert.Equal(99, capturedOrder!.CustomerId);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you mock properties and sequences with Moq?

**Mocking properties:**

```cs
var mockConfig = new Mock<IAppConfig>();
mockConfig.Setup(c => c.MaxRetries).Returns(3);
mockConfig.Setup(c => c.ConnectionString).Returns("Server=.;Database=Test");

// Track property changes
mockConfig.SetupProperty(c => c.Timeout, TimeSpan.FromSeconds(30));
```

**Returning different values on successive calls:**

```cs
var mockCounter = new Mock<ICounter>();
mockCounter.SetupSequence(c => c.Next())
           .Returns(1)
           .Returns(2)
           .Returns(3)
           .Throws(new OverflowException());

Assert.Equal(1, mockCounter.Object.Next());
Assert.Equal(2, mockCounter.Object.Next());
Assert.Equal(3, mockCounter.Object.Next());
Assert.Throws<OverflowException>(() => mockCounter.Object.Next());
```

**Auto-mocking with `Mock.Of<T>()` (shorthand for simple stubs):**

```cs
// Inline stub without explicit Mock<T> variable
var config = Mock.Of<IAppConfig>(c =>
    c.MaxRetries == 5 &&
    c.Environment == "Test");

var service = new RetryService(config);
Assert.Equal(5, service.MaxRetries);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 6. ASYNC TESTING

<br/>

## Q. How do you test asynchronous methods in xUnit?

xUnit natively supports `async Task` test methods — just mark the test `async Task` (never `async void`).

```cs
public class WeatherServiceTests
{
    [Fact]
    public async Task GetWeatherAsync_ValidCity_ReturnsTemperature()
    {
        // Arrange
        var mockClient = new Mock<IHttpClientWrapper>();
        mockClient
            .Setup(c => c.GetStringAsync("/weather?city=London"))
            .ReturnsAsync("""{"temp":18,"unit":"C"}""");

        var service = new WeatherService(mockClient.Object);

        // Act
        var weather = await service.GetWeatherAsync("London");

        // Assert
        Assert.Equal(18, weather.Temperature);
        Assert.Equal("C", weather.Unit);
    }

    [Fact]
    public async Task GetWeatherAsync_NetworkError_ThrowsServiceException()
    {
        var mockClient = new Mock<IHttpClientWrapper>();
        mockClient
            .Setup(c => c.GetStringAsync(It.IsAny<string>()))
            .ThrowsAsync(new HttpRequestException("No connection"));

        var service = new WeatherService(mockClient.Object);

        await Assert.ThrowsAsync<ServiceException>(
            () => service.GetWeatherAsync("London"));
    }
}
```

**Testing `IAsyncEnumerable<T>` (streaming):**

```cs
[Fact]
public async Task StreamOrdersAsync_ReturnsAllOrders()
{
    var repo = new Mock<IOrderRepository>();
    repo.Setup(r => r.StreamAsync())
        .Returns(GetOrdersAsync());

    var service = new OrderStreamService(repo.Object);
    var results = new List<Order>();
    await foreach (var order in service.StreamOrdersAsync())
        results.Add(order);

    Assert.Equal(3, results.Count);

    static async IAsyncEnumerable<Order> GetOrdersAsync()
    {
        yield return new Order { Id = 1 };
        yield return new Order { Id = 2 };
        yield return new Order { Id = 3 };
        await Task.CompletedTask;
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you test code with CancellationToken?

```cs
public class FileProcessorTests
{
    [Fact]
    public async Task ProcessAsync_CancellationRequested_ThrowsOperationCanceledException()
    {
        using var cts = new CancellationTokenSource();
        var processor = new FileProcessor();

        // Cancel immediately
        cts.Cancel();

        await Assert.ThrowsAsync<OperationCanceledException>(
            () => processor.ProcessAsync("file.csv", cts.Token));
    }

    [Fact]
    public async Task ProcessAsync_CancelledMidway_StopsProcessing()
    {
        using var cts = new CancellationTokenSource();
        var processor = new FileProcessor();
        int processedCount = 0;

        // Cancel after 100 ms
        cts.CancelAfter(TimeSpan.FromMilliseconds(100));

        try
        {
            await foreach (var _ in processor.StreamAsync(cts.Token))
                processedCount++;
        }
        catch (OperationCanceledException) { /* expected */ }

        // Some items processed before cancellation
        Assert.True(processedCount > 0);
    }

    [Fact]
    public async Task ProcessAsync_PassesCancellationTokenToRepository()
    {
        using var cts = new CancellationTokenSource();
        var mockRepo = new Mock<IFileRepository>();
        mockRepo.Setup(r => r.ReadAsync(It.IsAny<CancellationToken>()))
                .ReturnsAsync("data");

        var processor = new FileProcessor(mockRepo.Object);
        await processor.ProcessAsync(cts.Token);

        // Verify the token was forwarded
        mockRepo.Verify(r => r.ReadAsync(cts.Token), Times.Once);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 7. TEST FIXTURES & SHARED CONTEXT

<br/>

## Q. What is IClassFixture and when should you use it?

`IClassFixture<T>` allows a single instance of `T` to be shared across all tests within a test class. xUnit creates the fixture **once**, injects it into the test class constructor, and disposes it after the last test.

**Use it when setup is expensive (database, HTTP server, seeded data) and safe to share read-only.**

```cs
// Fixture — created once for the entire test class
public class DatabaseFixture : IDisposable
{
    public AppDbContext Db { get; }
    private readonly SqliteConnection _connection;

    public DatabaseFixture()
    {
        _connection = new SqliteConnection("DataSource=:memory:");
        _connection.Open();
        var opts = new DbContextOptionsBuilder<AppDbContext>()
            .UseSqlite(_connection).Options;
        Db = new AppDbContext(opts);
        Db.Database.EnsureCreated();
        SeedData();
    }

    private void SeedData()
    {
        Db.Products.AddRange(
            new Product { Name = "Laptop",  Price = 999m },
            new Product { Name = "Monitor", Price = 299m }
        );
        Db.SaveChanges();
    }

    public void Dispose()
    {
        Db.Dispose();
        _connection.Dispose();
    }
}

// Test class — shares one DatabaseFixture instance
public class ProductRepositoryTests(DatabaseFixture fixture)
    : IClassFixture<DatabaseFixture>
{
    [Fact]
    public void GetAll_ReturnsSeededProducts()
    {
        var products = fixture.Db.Products.ToList();
        Assert.Equal(2, products.Count);
    }

    [Fact]
    public void GetByName_Laptop_Found()
    {
        var product = fixture.Db.Products.First(p => p.Name == "Laptop");
        Assert.Equal(999m, product.Price);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is ICollectionFixture and how does it differ from IClassFixture?

`ICollectionFixture<T>` shares a fixture across **multiple test classes** in a named collection.

```cs
// 1. Declare the collection
[CollectionDefinition("Database")]
public class DatabaseCollection : ICollectionFixture<DatabaseFixture> { }

// 2. Assign test classes to the collection
[Collection("Database")]
public class ProductTests(DatabaseFixture fixture)
{
    [Fact]
    public void Products_SeededCorrectly()
        => Assert.Equal(2, fixture.Db.Products.Count());
}

[Collection("Database")]
public class OrderTests(DatabaseFixture fixture)
{
    [Fact]
    public void Orders_InitiallyEmpty()
        => Assert.Empty(fixture.Db.Orders);
}
```

| | `IClassFixture<T>` | `ICollectionFixture<T>` |
|---|---|---|
| Scope | One test class | Multiple test classes |
| Shared across | Tests in one class | All classes in a `[Collection]` |
| Parallelism | Classes run in parallel by default | Classes in the same collection run **sequentially** |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you write diagnostic output in xUnit tests?

xUnit captures and displays `ITestOutputHelper` output alongside each test result (unlike `Console.WriteLine` which is suppressed by default).

```cs
public class DiagnosticsTests(ITestOutputHelper output)
{
    [Fact]
    public void SortAlgorithm_LogsSteps()
    {
        var data = new[] { 5, 3, 1, 4, 2 };
        output.WriteLine($"Input:  [{string.Join(", ", data)}]");

        Array.Sort(data);

        output.WriteLine($"Output: [{string.Join(", ", data)}]");
        Assert.Equal([1, 2, 3, 4, 5], data);
    }

    [Fact]
    public async Task ApiCall_LogsElapsedTime()
    {
        var sw = Stopwatch.StartNew();
        await Task.Delay(50);
        sw.Stop();

        output.WriteLine($"Elapsed: {sw.ElapsedMilliseconds} ms");
        Assert.True(sw.ElapsedMilliseconds >= 50);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 8. TESTING ASP.NET CORE

<br/>

## Q. How do you write integration tests for ASP.NET Core using WebApplicationFactory?

`WebApplicationFactory<TEntryPoint>` spins up the full ASP.NET Core pipeline in memory — no network, no port, real DI container.

```bash
dotnet add package Microsoft.AspNetCore.Mvc.Testing
```

```cs
// CustomWebApplicationFactory — replace real services with test doubles
public class TestWebAppFactory : WebApplicationFactory<Program>
{
    protected override void ConfigureWebHost(IWebHostBuilder builder)
    {
        builder.ConfigureServices(services =>
        {
            // Replace real DB with in-memory SQLite
            services.RemoveAll<DbContextOptions<AppDbContext>>();
            services.AddDbContext<AppDbContext>(opts =>
                opts.UseSqlite("DataSource=:memory:"));

            // Replace real email sender
            services.AddSingleton<IEmailSender, FakeEmailSender>();
        });
    }
}

public class ProductsApiTests(TestWebAppFactory factory)
    : IClassFixture<TestWebAppFactory>
{
    private readonly HttpClient _client = factory.CreateClient();

    [Fact]
    public async Task GetProducts_ReturnsOkWithList()
    {
        var response = await _client.GetAsync("/api/products");
        response.EnsureSuccessStatusCode();

        var products = await response.Content
            .ReadFromJsonAsync<List<ProductDto>>();
        Assert.NotNull(products);
    }

    [Fact]
    public async Task CreateProduct_ValidInput_Returns201()
    {
        var newProduct = new { Name = "Keyboard", Price = 79.99 };
        var response = await _client.PostAsJsonAsync("/api/products", newProduct);

        Assert.Equal(HttpStatusCode.Created, response.StatusCode);
        Assert.NotNull(response.Headers.Location);
    }

    [Fact]
    public async Task GetProduct_NotFound_Returns404()
    {
        var response = await _client.GetAsync("/api/products/99999");
        Assert.Equal(HttpStatusCode.NotFound, response.StatusCode);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you test a controller action directly (without HTTP)?

For unit testing controllers in isolation (without the full pipeline):

```cs
public class ProductsControllerTests
{
    private readonly Mock<IProductService> _mockService = new();
    private readonly ProductsController _controller;

    public ProductsControllerTests()
    {
        _controller = new ProductsController(_mockService.Object);
        // Set up HttpContext for controller context
        _controller.ControllerContext = new ControllerContext
        {
            HttpContext = new DefaultHttpContext()
        };
    }

    [Fact]
    public async Task GetById_ExistingProduct_ReturnsOk()
    {
        var product = new ProductDto { Id = 1, Name = "Laptop" };
        _mockService.Setup(s => s.GetByIdAsync(1)).ReturnsAsync(product);

        var result = await _controller.GetById(1);

        var ok = Assert.IsType<OkObjectResult>(result);
        var dto = Assert.IsType<ProductDto>(ok.Value);
        Assert.Equal("Laptop", dto.Name);
    }

    [Fact]
    public async Task GetById_MissingProduct_ReturnsNotFound()
    {
        _mockService.Setup(s => s.GetByIdAsync(99)).ReturnsAsync((ProductDto?)null);

        var result = await _controller.GetById(99);

        Assert.IsType<NotFoundResult>(result);
    }

    [Fact]
    public async Task Create_InvalidModel_ReturnsBadRequest()
    {
        _controller.ModelState.AddModelError("Name", "Required");

        var result = await _controller.Create(new CreateProductDto());

        Assert.IsType<BadRequestObjectResult>(result);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you test middleware in ASP.NET Core?

```cs
public class RequestLoggingMiddlewareTests
{
    [Fact]
    public async Task Middleware_LogsRequestPath()
    {
        // Build a minimal pipeline
        var logMessages = new List<string>();
        var mockLogger = new Mock<ILogger<RequestLoggingMiddleware>>();
        mockLogger
            .Setup(l => l.Log(
                LogLevel.Information,
                It.IsAny<EventId>(),
                It.Is<It.IsAnyType>((v, _) => v.ToString()!.Contains("/test")),
                null,
                It.IsAny<Func<It.IsAnyType, Exception?, string>>()))
            .Callback(() => logMessages.Add("/test"));

        var context = new DefaultHttpContext();
        context.Request.Path = "/test";

        var middleware = new RequestLoggingMiddleware(
            next: _ => Task.CompletedTask,
            logger: mockLogger.Object);

        await middleware.InvokeAsync(context);

        Assert.Single(logMessages);
    }

    [Fact]
    public async Task Middleware_CallsNextDelegate()
    {
        bool nextCalled = false;
        var context = new DefaultHttpContext();
        var middleware = new RequestLoggingMiddleware(
            next: _ => { nextCalled = true; return Task.CompletedTask; },
            logger: Mock.Of<ILogger<RequestLoggingMiddleware>>());

        await middleware.InvokeAsync(context);

        Assert.True(nextCalled);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 9. TESTING EF CORE

<br/>

## Q. How do you unit test EF Core repositories using an in-memory database?

**Option 1 — SQLite in-memory (recommended for realistic SQL behaviour):**

```cs
public class ProductRepositoryTests : IDisposable
{
    private readonly SqliteConnection _connection;
    private readonly AppDbContext _db;
    private readonly ProductRepository _repository;

    public ProductRepositoryTests()
    {
        _connection = new SqliteConnection("DataSource=:memory:");
        _connection.Open();

        var opts = new DbContextOptionsBuilder<AppDbContext>()
            .UseSqlite(_connection)
            .Options;

        _db = new AppDbContext(opts);
        _db.Database.EnsureCreated();
        _repository = new ProductRepository(_db);
    }

    [Fact]
    public async Task AddAsync_ValidProduct_PersistsToDb()
    {
        var product = new Product { Name = "Headphones", Price = 149m };
        await _repository.AddAsync(product);

        var saved = await _db.Products.FindAsync(product.Id);
        Assert.NotNull(saved);
        Assert.Equal("Headphones", saved!.Name);
    }

    [Fact]
    public async Task GetAllAsync_ReturnsOnlyActiveProducts()
    {
        _db.Products.AddRange(
            new Product { Name = "A", IsActive = true  },
            new Product { Name = "B", IsActive = false },
            new Product { Name = "C", IsActive = true  }
        );
        await _db.SaveChangesAsync();

        var products = await _repository.GetAllActiveAsync();

        Assert.Equal(2, products.Count);
        Assert.All(products, p => Assert.True(p.IsActive));
    }

    public void Dispose()
    {
        _db.Dispose();
        _connection.Dispose();
    }
}
```

**Option 2 — EF Core InMemory provider (no SQL; avoids relational constraints):**

```cs
private static AppDbContext CreateInMemoryDb()
{
    var opts = new DbContextOptionsBuilder<AppDbContext>()
        .UseInMemoryDatabase(Guid.NewGuid().ToString()) // unique per test
        .Options;
    return new AppDbContext(opts);
}
```

> **Note:** Prefer SQLite in-memory for tests that need FK constraints, indexes, or raw SQL queries. Use the InMemory provider only for simple read/write tests.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you test EF Core queries that involve transactions?

```cs
public class TransactionTests : IDisposable
{
    private readonly SqliteConnection _connection;
    private readonly AppDbContext _db;

    public TransactionTests()
    {
        _connection = new SqliteConnection("DataSource=:memory:");
        _connection.Open();
        var opts = new DbContextOptionsBuilder<AppDbContext>()
            .UseSqlite(_connection).Options;
        _db = new AppDbContext(opts);
        _db.Database.EnsureCreated();
    }

    [Fact]
    public async Task PlaceOrder_DeductsStock_WithinTransaction()
    {
        // Arrange
        var product = new Product { Name = "Laptop", Stock = 10 };
        _db.Products.Add(product);
        await _db.SaveChangesAsync();

        var service = new OrderService(_db);

        // Act
        await service.PlaceOrderAsync(productId: product.Id, quantity: 3);

        // Assert
        var updated = await _db.Products.FindAsync(product.Id);
        Assert.Equal(7, updated!.Stock);
    }

    [Fact]
    public async Task PlaceOrder_InsufficientStock_RollsBack()
    {
        var product = new Product { Name = "Laptop", Stock = 2 };
        _db.Products.Add(product);
        await _db.SaveChangesAsync();

        var service = new OrderService(_db);

        await Assert.ThrowsAsync<InvalidOperationException>(
            () => service.PlaceOrderAsync(productId: product.Id, quantity: 5));

        // Stock unchanged after rollback
        var unchanged = await _db.Products.FindAsync(product.Id);
        Assert.Equal(2, unchanged!.Stock);
    }

    public void Dispose() { _db.Dispose(); _connection.Dispose(); }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

<br/>

## # 10. CODE COVERAGE & BEST PRACTICES

<br/>

## Q. How do you measure code coverage in .NET with xUnit?

```bash
# Install the coverlet collector (included in default test template)
dotnet add package coverlet.collector
dotnet add package coverlet.msbuild

# Run tests and collect coverage
dotnet test --collect:"XPlat Code Coverage"

# Generate HTML report using ReportGenerator
dotnet tool install -g dotnet-reportgenerator-globaltool
reportgenerator -reports:"**/coverage.cobertura.xml" -targetdir:"CoverageReport" -reporttypes:Html

# Open CoverageReport/index.html in browser
```

**Configure coverage thresholds in `csproj`:**

```xml
<PropertyGroup>
    <CollectCoverage>true</CollectCoverage>
    <CoverletOutputFormat>cobertura</CoverletOutputFormat>
    <Threshold>80</Threshold>
    <ThresholdType>line</ThresholdType>
    <ExcludeByAttribute>GeneratedCodeAttribute;ExcludeFromCodeCoverageAttribute</ExcludeByAttribute>
</PropertyGroup>
```

**Exclude a class or method from coverage:**

```cs
[ExcludeFromCodeCoverage]
public class AutoGeneratedMapper
{
    // generated code — excluded from coverage metrics
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are the best practices for writing maintainable unit tests?

**1. One logical concern per test:**

```cs
// ❌ Bad — tests two different behaviours
[Fact]
public void Process_Test()
{
    var result = service.Process(input);
    Assert.NotNull(result);
    Assert.Equal("processed", result.Status); // second concern
}

// ✅ Good — each test has a single focus
[Fact]
public void Process_ValidInput_ReturnsNonNull() { ... }

[Fact]
public void Process_ValidInput_SetsStatusToProcessed() { ... }
```

**2. Name tests clearly with `MethodName_Scenario_ExpectedResult`:**

```cs
// ❌ Bad
[Fact] public void Test1() { }

// ✅ Good
[Fact] public void Withdraw_InsufficientFunds_ThrowsInvalidOperation() { }
```

**3. Avoid logic in tests:**

```cs
// ❌ Bad — if/else in test hides intent
[Fact]
public void GetDiscount_Test()
{
    var discount = service.GetDiscount(user);
    if (user.IsPremium)
        Assert.Equal(0.20m, discount);
    else
        Assert.Equal(0.05m, discount);
}

// ✅ Good — separate test for each case
[Theory]
[InlineData(true,  0.20)]
[InlineData(false, 0.05)]
public void GetDiscount_ReturnsExpectedRate(bool isPremium, decimal expected)
{
    var user = new User { IsPremium = isPremium };
    Assert.Equal(expected, service.GetDiscount(user));
}
```

**4. Keep tests fast and independent:**

```cs
// ❌ Bad — shared static state couples tests
private static List<Order> _sharedOrders = new();

// ✅ Good — each test creates its own state
[Fact]
public void MyTest()
{
    var orders = new List<Order> { new() { Id = 1 } };
    // ...
}
```

**5. Use object builders or AutoFixture for complex objects:**

```cs
// Install: dotnet add package AutoFixture.Xunit2
public class OrderTests
{
    [Theory, AutoData]
    public void CalculateTotal_SumsItemPrices(Order order)
    {
        var total = OrderCalculator.Total(order);
        Assert.Equal(order.Items.Sum(i => i.Price), total);
    }
}
```

**6. Do not test framework or third-party code:**

```cs
// ❌ Bad — testing that List<T>.Add works
[Fact]
public void Add_ItemToList_CountIncreases()
{
    var list = new List<string>();
    list.Add("a");
    Assert.Equal(1, list.Count);
}

// ✅ Good — test your own logic that uses the list
[Fact]
public void CartService_AddItem_UpdatesCartTotal()
{
    var cart = new CartService();
    cart.AddItem(new Item { Price = 10m });
    Assert.Equal(10m, cart.Total);
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you run tests in parallel and control test execution order?

**xUnit runs test classes in parallel by default.** Control this with `[assembly: CollectionBehavior]`:

```cs
// AssemblyInfo.cs or in any file
[assembly: CollectionBehavior(DisableTestParallelization = false, MaxParallelThreads = 4)]
```

**Disable parallelism for a specific collection:**

```cs
[CollectionDefinition("Sequential", DisableParallelization = true)]
public class SequentialCollection { }

[Collection("Sequential")]
public class OrderedTests { ... }
```

**Control test execution order within a class (xUnit.Priority):**

```bash
dotnet add package Xunit.Priority
```

```cs
[TestCaseOrderer(PriorityOrderer.Name, PriorityOrderer.Assembly)]
public class WorkflowTests
{
    [Fact, Priority(1)]
    public void Step1_CreateOrder()    { /* ... */ }

    [Fact, Priority(2)]
    public void Step2_ProcessPayment() { /* ... */ }

    [Fact, Priority(3)]
    public void Step3_ShipOrder()      { /* ... */ }
}
```

> **Note:** Ordered/dependent tests are generally an anti-pattern. Each test should be independent. Use ordered execution only for integration/workflow tests where you explicitly document the dependency.

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you test time-dependent code?

Never use `DateTime.Now` or `DateTimeOffset.UtcNow` directly in testable code. Abstract time behind an interface:

```cs
// Abstraction
public interface IDateTimeProvider
{
    DateTimeOffset UtcNow { get; }
}

// Production implementation
public class DateTimeProvider : IDateTimeProvider
{
    public DateTimeOffset UtcNow => DateTimeOffset.UtcNow;
}

// Service under test
public class SubscriptionService(IDateTimeProvider clock)
{
    public bool IsExpired(Subscription sub)
        => sub.ExpiresAt < clock.UtcNow;
}
```

```cs
// Test with a fake clock
public class SubscriptionServiceTests
{
    [Fact]
    public void IsExpired_PastExpiryDate_ReturnsTrue()
    {
        var fakeClock = Mock.Of<IDateTimeProvider>(c =>
            c.UtcNow == new DateTimeOffset(2025, 1, 1, 0, 0, 0, TimeSpan.Zero));

        var service = new SubscriptionService(fakeClock);
        var sub = new Subscription
        {
            ExpiresAt = new DateTimeOffset(2024, 12, 31, 0, 0, 0, TimeSpan.Zero)
        };

        Assert.True(service.IsExpired(sub));
    }

    [Fact]
    public void IsExpired_FutureExpiryDate_ReturnsFalse()
    {
        var fakeClock = Mock.Of<IDateTimeProvider>(c =>
            c.UtcNow == new DateTimeOffset(2025, 1, 1, 0, 0, 0, TimeSpan.Zero));

        var service = new SubscriptionService(fakeClock);
        var sub = new Subscription
        {
            ExpiresAt = new DateTimeOffset(2026, 6, 1, 0, 0, 0, TimeSpan.Zero)
        };

        Assert.False(service.IsExpired(sub));
    }
}
```

> **.NET 8+**: Consider `TimeProvider` (built-in abstraction) instead of a custom interface — `FakeTimeProvider` from `Microsoft.Extensions.TimeProvider.Testing` provides the same pattern out of the box.

```bash
dotnet add package Microsoft.Extensions.TimeProvider.Testing
```

```cs
[Fact]
public void IsExpired_UsingFakeTimeProvider()
{
    var fakeTime = new FakeTimeProvider(startDateTime: new DateTimeOffset(2025, 1, 1, 0, 0, 0, TimeSpan.Zero));
    var service = new SubscriptionService(fakeTime);
    var sub = new Subscription { ExpiresAt = new DateTimeOffset(2024, 12, 31, 0, 0, 0, TimeSpan.Zero) };
    Assert.True(service.IsExpired(sub));
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>
