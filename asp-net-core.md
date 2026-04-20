# ASP.NET Core Basics

> *Click &#9733; if you like the project. Your contributions are heartily ♡ welcome.*

<br/>

## Q. What is ASP.NET Core and how is it different from ASP.NET? What are its main features?

**ASP.NET Core** is a cross-platform, high-performance, open-source web framework for building modern cloud-based, internet-connected applications. It runs on .NET (formerly .NET Core) and replaces the Windows-only ASP.NET 4.x.

| | ASP.NET (4.x) | ASP.NET Core |
|-|--------------|-------------|
| **Platform** | Windows only | Cross-platform (Windows, Linux, macOS) |
| **Hosting** | IIS only | Kestrel, IIS, Nginx, Docker, Azure |
| **Performance** | Moderate | High (Kestrel is among fastest web servers) |
| **`System.Web`** | Required | Removed — lighter pipeline |
| **Open source** | Partial | Fully open source |
| **Startup** | `Global.asax` + `Web.config` | `Program.cs` (minimal hosting) |
| **DI** | Optional third-party | Built-in, first-class |

**Main features:**
- Unified MVC and Web API programming model
- Built-in dependency injection
- Minimal hosting model (Program.cs, no Startup.cs required in .NET 6+)
- Middleware pipeline
- Kestrel high-performance server
- Built-in logging, configuration, health checks
- Native Docker / container support
- Razor Pages, Blazor, SignalR, gRPC support

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a new ASP.NET Core project?

```bash
# Web API (minimal hosting — .NET 8/9/10)
dotnet new webapi -n MyApi --use-minimal-apis
cd MyApi
dotnet run

# MVC
dotnet new mvc -n MyMvcApp

# Razor Pages
dotnet new razor -n MyRazorApp

# Minimal API (no controllers)
dotnet new web -n MyMinimalApp
```

**Minimal hosting model (Program.cs — .NET 6+):**

```cs
var builder = WebApplication.CreateBuilder(args);

// Register services
builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

// Configure middleware pipeline
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();

app.Run();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the purpose of `Startup.cs` / `Program.cs`? What is `ConfigureServices` and `IApplicationBuilder`?

In **.NET 6+**, `Startup.cs` is replaced by `Program.cs` with a single unified entry point. The `WebApplicationBuilder` handles both service registration and app configuration.

```cs
// .NET 8/9/10 — Program.cs (no Startup.cs needed)
var builder = WebApplication.CreateBuilder(args);

// 1. ConfigureServices equivalent — builder.Services
builder.Services.AddControllers();
builder.Services.AddDbContext<AppDbContext>(o =>
    o.UseSqlServer(builder.Configuration.GetConnectionString("Default")));
builder.Services.AddScoped<IOrderService, OrderService>();
builder.Services.AddMemoryCache();
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(/* options */);

var app = builder.Build();

// 2. Configure equivalent — app (IApplicationBuilder)
// IApplicationBuilder.Use() adds middleware to the pipeline
app.UseHttpsRedirection();
app.UseAuthentication();
app.UseAuthorization();
app.MapControllers();

app.Run();

// IApplicationBuilder key methods:
// app.Use(...)         — add middleware
// app.Run(...)         — terminal middleware (ends pipeline)
// app.Map(...)         — branch pipeline by path
// app.UseMiddleware<T>() — add typed middleware
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is middleware in ASP.NET Core and how do you configure and create custom middleware?

**Middleware** are components assembled into a request pipeline. Each can handle requests, pass to the next component, or short-circuit the pipeline.

```
Request → [Logging] → [Auth] → [Routing] → [Controller] → Response
           ↑___________↑___________↑_______________↑
                  Each calls next()
```

```cs
// Built-in middleware order (important — ordering matters)
var app = builder.Build();

app.UseExceptionHandler("/error");          // 1. catch all unhandled exceptions
app.UseHsts();                              // 2. add HSTS header
app.UseHttpsRedirection();                  // 3. redirect HTTP → HTTPS
app.UseStaticFiles();                       // 4. serve wwwroot files
app.UseRouting();                           // 5. match routes
app.UseCors("MyPolicy");                    // 6. CORS headers
app.UseAuthentication();                    // 7. identify user
app.UseAuthorization();                     // 8. check permissions
app.MapControllers();                       // 9. dispatch to controllers

// Custom middleware — class-based
public class RequestTimingMiddleware(RequestDelegate next, ILogger<RequestTimingMiddleware> logger)
{
    public async Task InvokeAsync(HttpContext context)
    {
        var sw = Stopwatch.StartNew();
        await next(context);                // call next middleware
        sw.Stop();
        logger.LogInformation("{Method} {Path} completed in {Ms}ms",
            context.Request.Method, context.Request.Path, sw.ElapsedMilliseconds);
    }
}

// Extension method for clean registration
public static class MiddlewareExtensions
{
    public static IApplicationBuilder UseRequestTiming(this IApplicationBuilder app)
        => app.UseMiddleware<RequestTimingMiddleware>();
}

// Register
app.UseRequestTiming();

// Inline middleware with Use
app.Use(async (context, next) =>
{
    context.Response.Headers["X-App-Version"] = "1.0";
    await next(context);
});

// Terminal middleware with Run (no next)
app.Run(async context =>
{
    await context.Response.WriteAsync("Fallback response");
});

// Branch pipeline with Map
app.Map("/health", branch =>
{
    branch.Run(async ctx => await ctx.Response.WriteAsync("OK"));
});
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is dependency injection (DI) in ASP.NET Core? What is the difference between `IServiceCollection` and `IServiceProvider`?

**DI** is a built-in first-class feature. Objects declare their dependencies via constructor parameters; the DI container resolves and injects them.

| | `IServiceCollection` | `IServiceProvider` |
|-|---------------------|-------------------|
| **Role** | Registration — register services at startup | Resolution — resolve services at runtime |
| **When used** | `builder.Services.Add*()` in `Program.cs` | `app.Services.GetRequiredService<T>()` |

**Service lifetimes:**

| Lifetime | Created | Use for |
|---------|---------|---------|
| `Singleton` | Once for app lifetime | Config, caches, heavy one-time setup |
| `Scoped` | Once per HTTP request | DbContext, unit-of-work, per-request services |
| `Transient` | Every time resolved | Lightweight stateless services |

```cs
// Registration
builder.Services.AddSingleton<IEmailSender, SmtpEmailSender>();
builder.Services.AddScoped<IOrderRepository, SqlOrderRepository>();
builder.Services.AddTransient<IInvoiceGenerator, PdfInvoiceGenerator>();

// Factory registration
builder.Services.AddScoped<IPaymentGateway>(sp =>
{
    var config = sp.GetRequiredService<IConfiguration>();
    return new StripeGateway(config["Stripe:ApiKey"]!);
});

// Constructor injection (automatic)
public class OrderService(IOrderRepository repo, IEmailSender email, ILogger<OrderService> logger)
{
    public async Task<Order> PlaceOrderAsync(CreateOrderDto dto)
    {
        var order = new Order(dto);
        await repo.AddAsync(order);
        await email.SendAsync(dto.Email, "Order confirmed", $"Order #{order.Id}");
        logger.LogInformation("Order {Id} placed", order.Id);
        return order;
    }
}

// Resolve manually (service locator — use sparingly)
using var scope = app.Services.CreateScope();
var db = scope.ServiceProvider.GetRequiredService<AppDbContext>();
await db.Database.MigrateAsync();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle configuration in ASP.NET Core? What is `appsettings.json`, `IConfiguration`, and the Options pattern?

```cs
// appsettings.json — hierarchical JSON configuration
// appsettings.Development.json overrides for Development environment
// Priority (high to low): Environment vars > CLI args > appsettings.{env}.json > appsettings.json

// appsettings.json
{
  "ConnectionStrings": {
    "Default": "Server=.;Database=MyDb;Trusted_Connection=true"
  },
  "Email": {
    "Host": "smtp.example.com",
    "Port": 587,
    "From": "noreply@example.com"
  },
  "FeatureFlags": {
    "EnableBetaFeatures": true
  }
}
```

```cs
// IConfiguration — read any config value
var host   = builder.Configuration["Email:Host"];
var connStr = builder.Configuration.GetConnectionString("Default");
var port   = builder.Configuration.GetValue<int>("Email:Port");

// Options pattern — strongly typed, validated (PREFERRED)
public class EmailOptions
{
    public const string Section = "Email";

    [Required] public string Host { get; set; } = "";
    [Range(1, 65535)] public int Port { get; set; } = 587;
    [Required, EmailAddress] public string From { get; set; } = "";
}

// Register and validate on startup (.NET 8+)
builder.Services.AddOptions<EmailOptions>()
    .BindConfiguration(EmailOptions.Section)
    .ValidateDataAnnotations()
    .ValidateOnStart();

// Inject IOptions<T>
public class EmailService(IOptions<EmailOptions> opts)
{
    private readonly EmailOptions _opts = opts.Value;

    public Task SendAsync(string to, string subject, string body)
    {
        Console.WriteLine($"Sending from {_opts.From} via {_opts.Host}:{_opts.Port}");
        return Task.CompletedTask;
    }
}

// IOptionsSnapshot<T> — reloads per request (for hot-reload)
// IOptionsMonitor<T>  — reloads on change with callback

// Custom configuration provider
builder.Configuration.AddJsonFile("custom.json", optional: true, reloadOnChange: true);
builder.Configuration.AddEnvironmentVariables(prefix: "MYAPP_");
builder.Configuration.AddCommandLine(args);

// User secrets (development only — keeps secrets out of source)
// dotnet user-secrets set "Stripe:ApiKey" "sk_test_..."
builder.Configuration.AddUserSecrets<Program>();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement routing in ASP.NET Core? What is the difference between conventional and attribute routing?

```cs
// 1. Attribute routing (recommended for Web API)
[ApiController]
[Route("api/[controller]")]  // route template: api/orders
public class OrdersController : ControllerBase
{
    [HttpGet]                          // GET  api/orders
    public IActionResult GetAll() => Ok();

    [HttpGet("{id:int}")]              // GET  api/orders/42
    public IActionResult GetById(int id) => Ok();

    [HttpGet("search")]                // GET  api/orders/search?q=...
    public IActionResult Search([FromQuery] string q) => Ok();

    [HttpPost]                         // POST api/orders
    public IActionResult Create([FromBody] CreateOrderDto dto) => CreatedAtAction(nameof(GetById), new { id = 1 }, dto);

    [HttpPut("{id:int}")]              // PUT  api/orders/42
    public IActionResult Update(int id, [FromBody] UpdateOrderDto dto) => NoContent();

    [HttpDelete("{id:int}")]           // DELETE api/orders/42
    public IActionResult Delete(int id) => NoContent();

    [HttpPatch("{id:int}")]            // PATCH api/orders/42
    public IActionResult Patch(int id, [FromBody] JsonPatchDocument<Order> patch) => NoContent();
}

// 2. Conventional routing (MVC)
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");

// 3. Minimal API routing (.NET 6+)
app.MapGet("/products/{id}", (int id, IProductService svc) => svc.GetById(id));
app.MapPost("/products", async (CreateProductDto dto, IProductService svc) =>
{
    var product = await svc.CreateAsync(dto);
    return Results.CreatedAtRoute("GetProduct", new { id = product.Id }, product);
});

// Route constraints
// {id:int}             — integer
// {id:guid}            — GUID
// {name:alpha}         — letters only
// {age:int:min(18)}    — int >= 18
// {slug:regex(^[a-z-]+$)} — regex

// Named routes and link generation
[HttpGet("{id}", Name = "GetProduct")]
public IActionResult GetProduct(int id) => Ok();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you create a RESTful Web API? What is `IActionResult` and how do you return different responses?

```cs
[ApiController]          // enables automatic model validation, binding inference
[Route("api/[controller]")]
public class ProductsController(IProductService service, ILogger<ProductsController> logger)
    : ControllerBase
{
    // IActionResult — return any HTTP response type
    // ActionResult<T> — return typed result or HTTP response

    [HttpGet]
    [ProducesResponseType<IEnumerable<ProductDto>>(StatusCodes.Status200OK)]
    public async Task<ActionResult<IEnumerable<ProductDto>>> GetAll()
    {
        var products = await service.GetAllAsync();
        return Ok(products);                       // 200 OK + JSON body
    }

    [HttpGet("{id:int}")]
    [ProducesResponseType<ProductDto>(StatusCodes.Status200OK)]
    [ProducesResponseType(StatusCodes.Status404NotFound)]
    public async Task<ActionResult<ProductDto>> GetById(int id)
    {
        var product = await service.GetByIdAsync(id);
        return product is null ? NotFound() : Ok(product); // 404 or 200
    }

    [HttpPost]
    [ProducesResponseType<ProductDto>(StatusCodes.Status201Created)]
    [ProducesResponseType(StatusCodes.Status400BadRequest)]
    public async Task<ActionResult<ProductDto>> Create([FromBody] CreateProductDto dto)
    {
        // [ApiController] validates ModelState automatically — no manual check needed
        var product = await service.CreateAsync(dto);
        return CreatedAtAction(nameof(GetById), new { id = product.Id }, product); // 201 + Location header
    }

    [HttpPut("{id:int}")]
    public async Task<IActionResult> Update(int id, [FromBody] UpdateProductDto dto)
    {
        if (!await service.UpdateAsync(id, dto)) return NotFound();
        return NoContent(); // 204
    }

    [HttpDelete("{id:int}")]
    public async Task<IActionResult> Delete(int id)
    {
        if (!await service.DeleteAsync(id)) return NotFound();
        return NoContent();
    }
}

// Common IActionResult factories:
// Ok(value)                — 200
// Created(uri, value)      — 201
// CreatedAtAction(...)     — 201 + Location header
// Accepted()               — 202
// NoContent()              — 204
// BadRequest(error)        — 400
// Unauthorized()           — 401
// Forbid()                 — 403
// NotFound()               — 404
// Conflict()               — 409
// UnprocessableEntity(...) — 422
// StatusCode(n, value)     — custom status
// File(...)                — file download
// Redirect(url)            — 302
// Json(value)              — JSON
// Content(str, mediaType)  — raw content
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is `[ApiController]`, `[FromBody]`, `[FromQuery]`, and model binding?

```cs
// [ApiController] behaviour:
// 1. Automatic 400 response when ModelState is invalid
// 2. Binding source inference (no need to specify [FromBody]/[FromQuery] in most cases)
// 3. Problem details (RFC 7807) error responses

// Binding attributes
[HttpPost("search")]
public IActionResult Search(
    [FromQuery] string? keyword,        // from ?keyword=...
    [FromQuery] int page = 1,
    [FromHeader(Name = "X-Api-Key")] string apiKey = "",
    [FromRoute] int? categoryId = null, // from route segment
    [FromBody] SearchFilters? filters = null, // from JSON body
    [FromForm] IFormFile? file = null)  // from multipart form
{
    return Ok();
}

// Model validation with Data Annotations
public class CreateProductDto
{
    [Required]
    [StringLength(200, MinimumLength = 3)]
    public string Name { get; set; } = "";

    [Range(0.01, 99999.99)]
    public decimal Price { get; set; }

    [Required]
    [EnumDataType(typeof(ProductCategory))]
    public ProductCategory Category { get; set; }
}

// Custom validation attribute
public class FutureDateAttribute : ValidationAttribute
{
    protected override ValidationResult? IsValid(object? value, ValidationContext ctx)
        => value is DateTime dt && dt > DateTime.UtcNow
            ? ValidationResult.Success
            : new ValidationResult("Date must be in the future");
}

// IValidatableObject — cross-property validation
public class DateRangeDto : IValidatableObject
{
    public DateTime Start { get; set; }
    public DateTime End   { get; set; }

    public IEnumerable<ValidationResult> Validate(ValidationContext ctx)
    {
        if (End <= Start)
            yield return new ValidationResult("End must be after Start", [nameof(End)]);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle exceptions globally in ASP.NET Core?

```cs
// 1. UseExceptionHandler (recommended for production)
app.UseExceptionHandler(errorApp =>
{
    errorApp.Run(async context =>
    {
        var feature = context.Features.Get<IExceptionHandlerFeature>();
        var ex = feature?.Error;

        context.Response.StatusCode = ex switch
        {
            NotFoundException  => 404,
            ValidationException => 422,
            UnauthorizedException => 401,
            _ => 500
        };

        context.Response.ContentType = "application/problem+json";
        await context.Response.WriteAsJsonAsync(new ProblemDetails
        {
            Status = context.Response.StatusCode,
            Title  = "An error occurred",
            Detail = ex?.Message
        });
    });
});

// 2. IExceptionHandler (.NET 8+ — preferred)
public class GlobalExceptionHandler(ILogger<GlobalExceptionHandler> logger)
    : IExceptionHandler
{
    public async ValueTask<bool> TryHandleAsync(
        HttpContext context, Exception ex, CancellationToken ct)
    {
        logger.LogError(ex, "Unhandled exception for {Method} {Path}",
            context.Request.Method, context.Request.Path);

        (int status, string title) = ex switch
        {
            NotFoundException     => (404, "Not Found"),
            ValidationException   => (422, "Validation Error"),
            UnauthorizedException => (401, "Unauthorized"),
            _                     => (500, "Internal Server Error")
        };

        await context.Response.WriteAsJsonAsync(new ProblemDetails
        {
            Status = status, Title = title, Detail = ex.Message
        }, ct);

        return true; // handled
    }
}

// Register
builder.Services.AddExceptionHandler<GlobalExceptionHandler>();
builder.Services.AddProblemDetails();
app.UseExceptionHandler();

// 3. Exception filter
public class ApiExceptionFilter : IExceptionFilter
{
    public void OnException(ExceptionContext context)
    {
        context.Result = context.Exception switch
        {
            NotFoundException e => new NotFoundObjectResult(e.Message),
            _ => new ObjectResult(new ProblemDetails { Status = 500 }) { StatusCode = 500 }
        };
        context.ExceptionHandled = true;
    }
}
builder.Services.AddControllers(o => o.Filters.Add<ApiExceptionFilter>());

public class NotFoundException(string message) : Exception(message);
public class ValidationException(string message) : Exception(message);
public class UnauthorizedException(string message) : Exception(message);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement authentication and authorization? What are JWT tokens and how are they used?

```cs
// JWT — JSON Web Token: Header.Payload.Signature
// Used for stateless authentication: server issues a signed token,
// client sends it on every request, server validates the signature.

// 1. Add JWT authentication
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer           = true,
            ValidIssuer              = builder.Configuration["Jwt:Issuer"],
            ValidateAudience         = true,
            ValidAudience            = builder.Configuration["Jwt:Audience"],
            ValidateIssuerSigningKey = true,
            IssuerSigningKey         = new SymmetricSecurityKey(
                Encoding.UTF8.GetBytes(builder.Configuration["Jwt:Key"]!)),
            ValidateLifetime         = true,
            ClockSkew                = TimeSpan.Zero
        };
    });

builder.Services.AddAuthorization(options =>
{
    options.AddPolicy("AdminOnly",    p => p.RequireRole("Admin"));
    options.AddPolicy("PremiumUser",  p => p.RequireClaim("subscription", "premium"));
    options.AddPolicy("MinAge18",     p => p.Requirements.Add(new MinAgeRequirement(18)));
});

// Middleware order (critical)
app.UseAuthentication(); // identify user from JWT
app.UseAuthorization();  // check policy/role

// 2. Generate JWT token (Login endpoint)
[ApiController, Route("api/auth")]
public class AuthController(IConfiguration config) : ControllerBase
{
    [HttpPost("login")]
    [AllowAnonymous]
    public IActionResult Login([FromBody] LoginDto dto)
    {
        // validate credentials against DB (simplified)
        if (dto.Username != "admin" || dto.Password != "pass") return Unauthorized();

        var key     = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(config["Jwt:Key"]!));
        var creds   = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);
        var claims  = new[]
        {
            new Claim(ClaimTypes.Name, dto.Username),
            new Claim(ClaimTypes.Role, "Admin"),
            new Claim("subscription", "premium"),
        };
        var token = new JwtSecurityToken(
            issuer:   config["Jwt:Issuer"],
            audience: config["Jwt:Audience"],
            claims:   claims,
            expires:  DateTime.UtcNow.AddHours(1),
            signingCredentials: creds);

        return Ok(new { token = new JwtSecurityTokenHandler().WriteToken(token) });
    }
}

// 3. Protect endpoints
[Authorize]                         // any authenticated user
[Authorize(Roles = "Admin")]        // specific role
[Authorize(Policy = "PremiumUser")] // named policy
[AllowAnonymous]                    // override Authorize — public
public class SecureController : ControllerBase { }

// 4. Access claims in controller
string userId = User.FindFirstValue(ClaimTypes.Name)!;
bool isAdmin  = User.IsInRole("Admin");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is ASP.NET Core Identity and how do you configure it?

```cs
// dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore

public class AppUser : IdentityUser
{
    public string? FullName { get; set; }
    public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
}

public class AppDbContext(DbContextOptions<AppDbContext> options)
    : IdentityDbContext<AppUser>(options) { }

// Register
builder.Services.AddDbContext<AppDbContext>(o =>
    o.UseSqlServer(builder.Configuration.GetConnectionString("Default")));

builder.Services.AddIdentity<AppUser, IdentityRole>(options =>
{
    options.Password.RequiredLength         = 8;
    options.Password.RequireUppercase       = true;
    options.Password.RequireNonAlphanumeric = true;
    options.Lockout.MaxFailedAccessAttempts = 5;
    options.Lockout.DefaultLockoutTimeSpan  = TimeSpan.FromMinutes(15);
    options.User.RequireUniqueEmail         = true;
})
.AddEntityFrameworkStores<AppDbContext>()
.AddDefaultTokenProviders();

// Usage
public class AccountController(UserManager<AppUser> userManager, SignInManager<AppUser> signIn)
    : ControllerBase
{
    [HttpPost("register")]
    public async Task<IActionResult> Register([FromBody] RegisterDto dto)
    {
        var user = new AppUser { UserName = dto.Email, Email = dto.Email, FullName = dto.Name };
        var result = await userManager.CreateAsync(user, dto.Password);
        if (!result.Succeeded) return BadRequest(result.Errors);
        await userManager.AddToRoleAsync(user, "User");
        return Ok("Registered");
    }

    [HttpPost("login")]
    public async Task<IActionResult> Login([FromBody] LoginDto dto)
    {
        var result = await signIn.PasswordSignInAsync(dto.Email, dto.Password,
            isPersistent: false, lockoutOnFailure: true);
        if (!result.Succeeded) return Unauthorized();
        return Ok("Logged in");
    }
}

// Two-factor authentication
var token = await userManager.GenerateTwoFactorTokenAsync(user, "Email");
await userManager.SetTwoFactorEnabledAsync(user, true);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement caching in ASP.NET Core? `IMemoryCache`, distributed cache, and `ResponseCache`?

```cs
// 1. IMemoryCache — in-process, single-server cache
builder.Services.AddMemoryCache();

public class ProductService(IMemoryCache cache, IProductRepository repo)
{
    private static readonly string CacheKey = "products:all";

    public async Task<IEnumerable<Product>> GetAllAsync()
    {
        return await cache.GetOrCreateAsync(CacheKey, async entry =>
        {
            entry.AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(5);
            entry.SlidingExpiration = TimeSpan.FromMinutes(2);
            entry.Size = 1;
            return await repo.GetAllAsync();
        }) ?? [];
    }
}

// 2. Distributed cache (Redis — multi-server, survives restarts)
// dotnet add package Microsoft.Extensions.Caching.StackExchangeRedis
builder.Services.AddStackExchangeRedisCache(options =>
    options.Configuration = builder.Configuration.GetConnectionString("Redis"));

public class DistributedService(IDistributedCache cache)
{
    public async Task<string?> GetAsync(string key) => await cache.GetStringAsync(key);

    public async Task SetAsync(string key, string value)
        => await cache.SetStringAsync(key, value, new DistributedCacheEntryOptions
        {
            AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(10)
        });
}

// 3. Response caching — cache HTTP responses (GET only)
builder.Services.AddResponseCaching();
app.UseResponseCaching();

[HttpGet]
[ResponseCache(Duration = 60, VaryByQueryKeys = ["page", "pageSize"])]
public IActionResult GetCached() => Ok("cached response");

// 4. Output caching (.NET 7+ — preferred over ResponseCaching)
builder.Services.AddOutputCache(options =>
{
    options.AddBasePolicy(b => b.Expire(TimeSpan.FromMinutes(10)));
    options.AddPolicy("UserSpecific", b => b.Expire(TimeSpan.FromMinutes(5)).VaryByRouteValue("userId"));
});
app.UseOutputCache();

[HttpGet]
[OutputCache(PolicyName = "UserSpecific", Duration = 300)]
public IActionResult GetWithOutputCache() => Ok("output cached");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement logging in ASP.NET Core? What is `ILogger`?

```cs
// ILogger is built-in — injected automatically
public class OrderService(ILogger<OrderService> logger, IOrderRepository repo)
{
    public async Task<Order> GetOrderAsync(int id)
    {
        logger.LogDebug("Fetching order {OrderId}", id);    // structured logging
        var order = await repo.GetByIdAsync(id);

        if (order is null)
        {
            logger.LogWarning("Order {OrderId} not found", id);
            throw new NotFoundException($"Order {id} not found");
        }

        logger.LogInformation("Order {OrderId} retrieved for {Customer}", id, order.CustomerName);
        return order;
    }
}

// Configure logging providers in Program.cs
builder.Logging
    .ClearProviders()
    .AddConsole()
    .AddDebug()
    .SetMinimumLevel(LogLevel.Information);

// Filter specific categories
builder.Logging.AddFilter("Microsoft.EntityFrameworkCore.Database.Command", LogLevel.Warning);

// appsettings.json logging config
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning",
      "MyApp": "Debug"
    }
  }
}

// Serilog (popular structured logging)
// dotnet add package Serilog.AspNetCore Serilog.Sinks.Console Serilog.Sinks.File
builder.Host.UseSerilog((ctx, cfg) => cfg
    .ReadFrom.Configuration(ctx.Configuration)
    .WriteTo.Console()
    .WriteTo.File("logs/app-.log", rollingInterval: RollingInterval.Day)
    .Enrich.FromLogContext());

// Log levels: Trace, Debug, Information, Warning, Error, Critical
// High-performance logging with LoggerMessage
public static partial class Log
{
    [LoggerMessage(Level = LogLevel.Information, Message = "Order {OrderId} placed by {Customer}")]
    public static partial void OrderPlaced(ILogger logger, int orderId, string customer);
}
Log.OrderPlaced(logger, 42, "Alice");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are health checks and how do you create custom health checks?

```cs
// Register built-in health checks
builder.Services.AddHealthChecks()
    .AddSqlServer(builder.Configuration.GetConnectionString("Default")!)
    .AddRedis(builder.Configuration.GetConnectionString("Redis")!)
    .AddUrlGroup(new Uri("https://api.external.com/health"), "external-api")
    .AddCheck<CustomHealthCheck>("custom", tags: ["ready"]);

// Map health check endpoints
app.MapHealthChecks("/health");                          // all checks
app.MapHealthChecks("/health/ready",                    // readiness probe
    new HealthCheckOptions { Predicate = c => c.Tags.Contains("ready") });
app.MapHealthChecks("/health/live",                     // liveness probe
    new HealthCheckOptions { Predicate = _ => false }); // just alive

// Custom health check
public class CustomHealthCheck(IOrderRepository repo) : IHealthCheck
{
    public async Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context, CancellationToken ct = default)
    {
        try
        {
            bool canConnect = await repo.CanConnectAsync(ct);
            return canConnect
                ? HealthCheckResult.Healthy("Database is reachable")
                : HealthCheckResult.Degraded("Database responding slowly");
        }
        catch (Exception ex)
        {
            return HealthCheckResult.Unhealthy("Database unreachable", ex);
        }
    }
}

// Rich JSON response
app.MapHealthChecks("/health/detail", new HealthCheckOptions
{
    ResponseWriter = UIResponseWriter.WriteHealthCheckUIResponse
});
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is `IHostedService`, `BackgroundService`, and how do you create background services?

```cs
// IHostedService — low-level, start/stop
public class StartupInitializer(ILogger<StartupInitializer> logger, AppDbContext db)
    : IHostedService
{
    public async Task StartAsync(CancellationToken ct)
    {
        logger.LogInformation("Migrating database...");
        await db.Database.MigrateAsync(ct);
    }
    public Task StopAsync(CancellationToken ct) => Task.CompletedTask;
}

// BackgroundService — long-running worker (preferred)
public class OrderProcessingWorker(
    ILogger<OrderProcessingWorker> logger,
    IServiceScopeFactory scopeFactory) : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        logger.LogInformation("Worker started");

        while (!stoppingToken.IsCancellationRequested)
        {
            try
            {
                using var scope = scopeFactory.CreateScope();  // scoped services need scope
                var service = scope.ServiceProvider.GetRequiredService<IOrderService>();
                await service.ProcessPendingOrdersAsync(stoppingToken);
            }
            catch (Exception ex) when (!stoppingToken.IsCancellationRequested)
            {
                logger.LogError(ex, "Error processing orders");
            }

            await Task.Delay(TimeSpan.FromSeconds(30), stoppingToken);
        }
    }
}

// Register
builder.Services.AddHostedService<StartupInitializer>();
builder.Services.AddHostedService<OrderProcessingWorker>();

// Worker service project (standalone — no HTTP)
// dotnet new worker -n MyWorkerService
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is SignalR and how do you implement real-time communication?

```cs
// dotnet add package Microsoft.AspNetCore.SignalR

// Hub — server-side real-time endpoint
public class ChatHub(ILogger<ChatHub> logger) : Hub
{
    public async Task SendMessage(string user, string message)
    {
        logger.LogInformation("{User}: {Message}", user, message);
        await Clients.All.SendAsync("ReceiveMessage", user, message);
    }

    public async Task JoinGroup(string groupName)
    {
        await Groups.AddToGroupAsync(Context.ConnectionId, groupName);
        await Clients.Group(groupName).SendAsync("Notify", $"{Context.ConnectionId} joined");
    }

    public override async Task OnConnectedAsync()
    {
        logger.LogInformation("Connected: {Id}", Context.ConnectionId);
        await base.OnConnectedAsync();
    }
}

// Register and map
builder.Services.AddSignalR();
app.MapHub<ChatHub>("/hubs/chat");

// Push from server (not from hub)
public class NotificationService(IHubContext<ChatHub> hub)
{
    public Task NotifyAllAsync(string message)
        => hub.Clients.All.SendAsync("Notification", message);

    public Task NotifyGroupAsync(string group, string message)
        => hub.Clients.Group(group).SendAsync("Notification", message);
}

// JavaScript client
// const conn = new signalR.HubConnectionBuilder().withUrl("/hubs/chat").build();
// conn.on("ReceiveMessage", (user, msg) => console.log(user, msg));
// await conn.start();
// await conn.invoke("SendMessage", "Alice", "Hello!");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement gRPC in ASP.NET Core? What is gRPC?

```cs
// gRPC — high-performance RPC using Protocol Buffers over HTTP/2
// dotnet add package Grpc.AspNetCore

// 1. Define service in .proto file (Protos/greeter.proto)
// syntax = "proto3";
// service Greeter {
//   rpc SayHello (HelloRequest) returns (HelloReply);
//   rpc SayHellos(HelloRequest) returns (stream HelloReply); // server streaming
// }
// message HelloRequest { string name = 1; }
// message HelloReply   { string message = 1; }

// 2. Implement service
public class GreeterService(ILogger<GreeterService> logger)
    : Greeter.GreeterBase
{
    public override Task<HelloReply> SayHello(HelloRequest request, ServerCallContext context)
    {
        logger.LogInformation("Saying hello to {Name}", request.Name);
        return Task.FromResult(new HelloReply { Message = $"Hello, {request.Name}!" });
    }

    public override async Task SayHellos(
        HelloRequest request, IServerStreamWriter<HelloReply> stream, ServerCallContext context)
    {
        for (int i = 0; i < 5; i++)
        {
            await stream.WriteAsync(new HelloReply { Message = $"Hello #{i}, {request.Name}!" });
            await Task.Delay(1000);
        }
    }
}

// 3. Register and map
builder.Services.AddGrpc();
app.MapGrpcService<GreeterService>();

// 4. gRPC client
// var channel = GrpcChannel.ForAddress("https://localhost:5001");
// var client  = new Greeter.GreeterClient(channel);
// var reply   = await client.SayHelloAsync(new HelloRequest { Name = "World" });
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Kestrel? How do you deploy to IIS, Docker, and Azure?

```cs
// Kestrel — built-in cross-platform web server
// Optimized for .NET, supports HTTP/1.1, HTTP/2, HTTP/3 (QUIC)

// Configure Kestrel in Program.cs
builder.WebHost.ConfigureKestrel(options =>
{
    options.Listen(IPAddress.Any, 5000);               // HTTP
    options.Listen(IPAddress.Any, 5001, listenOptions =>
        listenOptions.UseHttps("cert.pfx", "password")); // HTTPS
    options.Limits.MaxConcurrentConnections = 100;
    options.Limits.RequestHeadersTimeout    = TimeSpan.FromSeconds(30);
});
```

```bash
# Docker
# Dockerfile (multi-stage)
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /src
COPY . .
RUN dotnet publish -c Release -o /app/publish

FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS final
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "MyApi.dll"]

# Build and run
docker build -t myapi .
docker run -p 8080:8080 myapi

# IIS hosting — requires AspNetCoreModule
# web.config:
# <aspNetCore processPath="dotnet" arguments=".\MyApi.dll" />

# Azure App Service
az webapp up --name myapi --resource-group rg --runtime "DOTNET|9.0"

# Publish
dotnet publish -c Release -o ./publish
dotnet ./publish/MyApi.dll
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `Environment` class? How do you handle environment-specific settings?

```cs
// Environments: Development, Staging, Production (set via ASPNETCORE_ENVIRONMENT)

// Check environment
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseDeveloperExceptionPage();
}

if (app.Environment.IsProduction())
{
    app.UseExceptionHandler("/error");
    app.UseHsts();
}

bool isStaging = app.Environment.IsEnvironment("Staging");

// appsettings.json overrides
// appsettings.json             — base (all environments)
// appsettings.Development.json — overrides for Development
// appsettings.Production.json  — overrides for Production

// Environment variables override appsettings
// ASPNETCORE_ENVIRONMENT=Production
// ConnectionStrings__Default=Server=prod.db...
// Email__Host=smtp.prod.com

// launchSettings.json (dev-only, not deployed)
{
  "profiles": {
    "https": {
      "environmentVariables": { "ASPNETCORE_ENVIRONMENT": "Development" }
    }
  }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is CORS, HTTPS/HSTS, CSRF, XSS, and how do you secure an ASP.NET Core app?

```cs
// 1. CORS — Cross-Origin Resource Sharing
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowFrontend", policy =>
        policy.WithOrigins("https://myapp.com", "https://localhost:3000")
              .AllowAnyHeader()
              .AllowAnyMethod()
              .AllowCredentials());

    options.AddPolicy("PublicApi", policy =>
        policy.AllowAnyOrigin()
              .WithMethods("GET")
              .WithHeaders("Content-Type"));
});

app.UseCors("AllowFrontend");

// Per-endpoint CORS
app.MapGet("/public", () => "public").RequireCors("PublicApi");

[EnableCors("AllowFrontend")] // controller or action level
public class SecureController : ControllerBase { }

// 2. HTTPS & HSTS
app.UseHttpsRedirection();       // redirect HTTP to HTTPS
app.UseHsts();                   // Strict-Transport-Security header (prod only)
builder.Services.AddHsts(options =>
{
    options.MaxAge           = TimeSpan.FromDays(365);
    options.IncludeSubDomains = true;
    options.Preload          = true;
});

// 3. CSRF — Anti-Forgery (MVC/Razor Pages)
builder.Services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");

[ValidateAntiForgeryToken]
[HttpPost]
public IActionResult Submit(FormModel model) => Ok();

// API with JWT tokens are NOT vulnerable to CSRF (JWT ≠ cookies)

// 4. XSS — Razor auto-encodes output, but raw HTML needs sanitization
// Install: HtmlSanitizer
// var sanitizer = new HtmlSanitizer();
// string safe = sanitizer.Sanitize(userInput);

// 5. Security headers middleware
app.Use(async (context, next) =>
{
    context.Response.Headers["X-Content-Type-Options"]    = "nosniff";
    context.Response.Headers["X-Frame-Options"]           = "DENY";
    context.Response.Headers["X-XSS-Protection"]          = "1; mode=block";
    context.Response.Headers["Referrer-Policy"]           = "strict-origin";
    context.Response.Headers["Content-Security-Policy"]   = "default-src 'self'";
    context.Response.Headers["Permissions-Policy"]        = "camera=(), microphone=()";
    await next();
});

// 6. Rate limiting (.NET 7+)
builder.Services.AddRateLimiter(options =>
{
    options.AddFixedWindowLimiter("Api", o =>
    {
        o.PermitLimit = 100;
        o.Window      = TimeSpan.FromMinutes(1);
    });
    options.AddSlidingWindowLimiter("Login", o =>
    {
        o.PermitLimit = 5;
        o.Window      = TimeSpan.FromMinutes(15);
    });
    options.RejectionStatusCode = StatusCodes.Status429TooManyRequests;
});
app.UseRateLimiter();

[HttpPost("login"), EnableRateLimiting("Login")]
public IActionResult Login() => Ok();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Data Protection in ASP.NET Core?

```cs
// Data Protection API — encrypt/decrypt data, protect cookies, anti-forgery tokens
builder.Services.AddDataProtection()
    .PersistKeysToFileSystem(new DirectoryInfo("/var/keys"))
    .ProtectKeysWithCertificate("thumbprint")
    .SetApplicationName("MyApp")
    .SetDefaultKeyLifetime(TimeSpan.FromDays(14));

// Usage
public class TokenService(IDataProtectionProvider provider)
{
    private readonly IDataProtector _protector
        = provider.CreateProtector("PasswordReset.v1");

    public string Protect(string value)   => _protector.Protect(value);
    public string Unprotect(string token) => _protector.Unprotect(token);

    // Time-limited token
    public string CreateTimedToken(string userId)
    {
        var timed = _protector.ToTimeLimitedDataProtector();
        return timed.Protect(userId, lifetime: TimeSpan.FromHours(1));
    }

    public string? ValidateTimedToken(string token)
    {
        try
        {
            var timed = _protector.ToTimeLimitedDataProtector();
            return timed.Unprotect(token);
        }
        catch (CryptographicException) { return null; }
    }
}

// User Secrets (development)
// dotnet user-secrets init
// dotnet user-secrets set "Jwt:Key" "super-secret-key"
// Stored in %APPDATA%\Microsoft\UserSecrets\ — NEVER in source control
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are Action Filters, Resource Filters, Result Filters, Exception Filters, and Authorization Filters?

```cs
// Filter pipeline order:
// Auth → Resource → [Model binding] → Action (before + after) → Result → Exception

// 1. Action Filter — runs before/after action method
public class LoggingActionFilter(ILogger<LoggingActionFilter> logger)
    : IAsyncActionFilter
{
    public async Task OnActionExecutionAsync(ActionExecutingContext context, ActionExecutionDelegate next)
    {
        logger.LogInformation("Action {Action} starting", context.ActionDescriptor.DisplayName);
        var result = await next();
        if (result.Exception is not null)
            logger.LogError(result.Exception, "Action failed");
        else
            logger.LogInformation("Action {Action} completed", context.ActionDescriptor.DisplayName);
    }
}

// 2. Exception Filter
public class ApiExceptionFilter : IExceptionFilter
{
    public void OnException(ExceptionContext context)
    {
        context.Result = new ObjectResult(new ProblemDetails
        {
            Status = 500, Title = "Unexpected error", Detail = context.Exception.Message
        }) { StatusCode = 500 };
        context.ExceptionHandled = true;
    }
}

// 3. Result Filter — runs before/after IActionResult execution
public class AddHeaderResultFilter : IResultFilter
{
    public void OnResultExecuting(ResultExecutingContext context)
        => context.HttpContext.Response.Headers["X-Processed-By"] = "MyApi";

    public void OnResultExecuted(ResultExecutedContext context) { }
}

// 4. Resource Filter — runs before model binding (short-circuit capability)
public class CacheResourceFilter : IResourceFilter
{
    public void OnResourceExecuting(ResourceExecutingContext context) { }
    public void OnResourceExecuted(ResourceExecutedContext context) { }
}

// 5. Authorization Filter — runs first, before all other filters
public class ApiKeyAuthorizationFilter(IConfiguration config) : IAuthorizationFilter
{
    public void OnAuthorization(AuthorizationFilterContext context)
    {
        if (!context.HttpContext.Request.Headers.TryGetValue("X-Api-Key", out var key)
            || key != config["ApiKey"])
            context.Result = new UnauthorizedResult();
    }
}

// Register globally
builder.Services.AddControllers(options =>
{
    options.Filters.Add<LoggingActionFilter>();
    options.Filters.Add<ApiExceptionFilter>();
    options.Filters.Add<AddHeaderResultFilter>();
});

// Or per controller/action as attribute
[ServiceFilter(typeof(LoggingActionFilter))]
public class OrdersController : ControllerBase { }

// TypeFilter — with constructor args
[TypeFilter(typeof(ApiKeyAuthorizationFilter))]
public IActionResult Secure() => Ok();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you handle file uploads and downloads in ASP.NET Core?

```cs
// File upload — IFormFile
[HttpPost("upload")]
[RequestSizeLimit(10 * 1024 * 1024)] // 10 MB limit
public async Task<IActionResult> Upload(IFormFile file, CancellationToken ct)
{
    if (file.Length == 0) return BadRequest("Empty file");

    // Validate extension (never trust Content-Type alone)
    var ext = Path.GetExtension(file.FileName).ToLowerInvariant();
    if (ext is not (".jpg" or ".png" or ".pdf"))
        return BadRequest("Invalid file type");

    // Save with a safe, server-generated filename
    var safeName = $"{Guid.NewGuid()}{ext}";
    var path = Path.Combine("uploads", safeName);
    await using var stream = System.IO.File.Create(path);
    await file.CopyToAsync(stream, ct);

    return Ok(new { fileName = safeName, size = file.Length });
}

// Multiple file upload
[HttpPost("upload-many")]
public async Task<IActionResult> UploadMany(IFormFileCollection files)
{
    foreach (var file in files)
    {
        // process each file
    }
    return Ok();
}

// File download
[HttpGet("download/{filename}")]
public IActionResult Download(string filename)
{
    // Prevent path traversal
    var safeName = Path.GetFileName(filename);
    var path = Path.Combine("uploads", safeName);
    if (!System.IO.File.Exists(path)) return NotFound();

    var contentType = "application/octet-stream";
    return PhysicalFile(path, contentType, safeName); // streams the file
}

// Stream large file response
[HttpGet("stream/{id}")]
public async Task StreamLargeFile(int id)
{
    Response.ContentType = "video/mp4";
    await using var fileStream = System.IO.File.OpenRead($"media/{id}.mp4");
    await fileStream.CopyToAsync(Response.Body);
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Swagger/OpenAPI and how do you add API versioning?

```cs
// Swagger — UI to explore and test API
// .NET 9: built-in OpenAPI support (no Swashbuckle needed)

// .NET 9+ built-in
builder.Services.AddOpenApi();
app.MapOpenApi();

// Swashbuckle (pre-.NET 9)
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo { Title = "My API", Version = "v1" });
    c.AddSecurityDefinition("Bearer", new OpenApiSecurityScheme
    {
        Type = SecuritySchemeType.Http, Scheme = "bearer"
    });
});
app.UseSwagger();
app.UseSwaggerUI(c => c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API v1"));

// API Versioning
// dotnet add package Asp.Versioning.Mvc Asp.Versioning.Mvc.ApiExplorer
builder.Services.AddApiVersioning(options =>
{
    options.DefaultApiVersion  = new ApiVersion(1, 0);
    options.AssumeDefaultVersionWhenUnspecified = true;
    options.ReportApiVersions  = true;                  // adds api-supported-versions header
}).AddMvc();

// Version via URL segment
[ApiController]
[Route("api/v{version:apiVersion}/[controller]")]
[ApiVersion("1.0")]
public class ProductsV1Controller : ControllerBase
{
    [HttpGet] public IActionResult Get() => Ok("v1 products");
}

[ApiController]
[Route("api/v{version:apiVersion}/[controller]")]
[ApiVersion("2.0")]
[MapToApiVersion("2.0")]
public class ProductsV2Controller : ControllerBase
{
    [HttpGet] public IActionResult Get() => Ok("v2 products");
}

// Version via query string: GET /api/products?api-version=2.0
// Version via header:       GET /api/products + Header: api-version: 2.0
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use Razor Pages? What is the difference between Razor Pages and MVC?

```cs
// Razor Pages — page-centric model (good for form-heavy, page-based UI)
// MVC — controller-centric model (good for complex routing, API + views)

// Razor Pages: each page has .cshtml + .cshtml.cs
// Pages/Orders/Index.cshtml.cs
public class IndexModel(IOrderService service) : PageModel
{
    public IEnumerable<Order> Orders { get; private set; } = [];

    [BindProperty]
    public CreateOrderDto NewOrder { get; set; } = new();

    public async Task OnGetAsync()
        => Orders = await service.GetAllAsync();

    public async Task<IActionResult> OnPostAsync()
    {
        if (!ModelState.IsValid) return Page();
        await service.CreateAsync(NewOrder);
        return RedirectToPage();
    }
}

// Pages/Orders/Index.cshtml
@page
@model IndexModel
@{
    ViewData["Title"] = "Orders";
}
<h1>Orders</h1>
<form method="post">
    <input asp-for="NewOrder.CustomerName" />
    <button type="submit">Place Order</button>
</form>

// Register
builder.Services.AddRazorPages();
app.MapRazorPages();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement pagination, sorting, filtering, and use AutoMapper?

```cs
// Pagination
public record PagedResult<T>(IEnumerable<T> Items, int TotalCount, int Page, int PageSize)
{
    public int TotalPages => (int)Math.Ceiling((double)TotalCount / PageSize);
    public bool HasNext => Page < TotalPages;
    public bool HasPrev => Page > 1;
}

[HttpGet]
public async Task<ActionResult<PagedResult<ProductDto>>> GetPaged(
    [FromQuery] int page = 1,
    [FromQuery] int pageSize = 20,
    [FromQuery] string? sortBy = "name",
    [FromQuery] bool desc = false,
    [FromQuery] string? search = null)
{
    var query = _context.Products.AsQueryable();

    if (!string.IsNullOrEmpty(search))
        query = query.Where(p => p.Name.Contains(search));

    query = (sortBy, desc) switch
    {
        ("price", true)  => query.OrderByDescending(p => p.Price),
        ("price", false) => query.OrderBy(p => p.Price),
        (_, true)        => query.OrderByDescending(p => p.Name),
        _                => query.OrderBy(p => p.Name),
    };

    int total   = await query.CountAsync();
    var items   = await query.Skip((page - 1) * pageSize).Take(pageSize).ToListAsync();
    var dtos    = _mapper.Map<List<ProductDto>>(items);
    return Ok(new PagedResult<ProductDto>(dtos, total, page, pageSize));
}

// AutoMapper
// dotnet add package AutoMapper
builder.Services.AddAutoMapper(typeof(Program));

public class MappingProfile : Profile
{
    public MappingProfile()
    {
        CreateMap<Product, ProductDto>();
        CreateMap<CreateProductDto, Product>()
            .ForMember(d => d.CreatedAt, o => o.MapFrom(_ => DateTime.UtcNow));
    }
}

// Inject and use
public class ProductService(IMapper mapper, AppDbContext db)
{
    public async Task<ProductDto> GetByIdAsync(int id)
    {
        var product = await db.Products.FindAsync(id) ?? throw new NotFoundException($"{id}");
        return mapper.Map<ProductDto>(product);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement the Repository and Unit of Work patterns in ASP.NET Core?

```cs
// Generic repository
public interface IRepository<T> where T : class
{
    Task<T?> GetByIdAsync(int id, CancellationToken ct = default);
    Task<IEnumerable<T>> GetAllAsync(CancellationToken ct = default);
    Task AddAsync(T entity, CancellationToken ct = default);
    void Update(T entity);
    void Remove(T entity);
}

public class EfRepository<T>(AppDbContext db) : IRepository<T> where T : class
{
    public Task<T?> GetByIdAsync(int id, CancellationToken ct)
        => db.Set<T>().FindAsync([id], ct).AsTask();

    public async Task<IEnumerable<T>> GetAllAsync(CancellationToken ct)
        => await db.Set<T>().ToListAsync(ct);

    public Task AddAsync(T entity, CancellationToken ct)
        => db.Set<T>().AddAsync(entity, ct).AsTask();

    public void Update(T entity) => db.Set<T>().Update(entity);
    public void Remove(T entity) => db.Set<T>().Remove(entity);
}

// Unit of Work
public interface IUnitOfWork : IDisposable
{
    IRepository<Order>   Orders   { get; }
    IRepository<Product> Products { get; }
    Task<int> SaveChangesAsync(CancellationToken ct = default);
}

public class UnitOfWork(AppDbContext db) : IUnitOfWork
{
    public IRepository<Order>   Orders   { get; } = new EfRepository<Order>(db);
    public IRepository<Product> Products { get; } = new EfRepository<Product>(db);
    public Task<int> SaveChangesAsync(CancellationToken ct) => db.SaveChangesAsync(ct);
    public void Dispose() => db.Dispose();
}

// Register
builder.Services.AddScoped<IUnitOfWork, UnitOfWork>();

// Usage in service
public class OrderService(IUnitOfWork uow)
{
    public async Task<Order> PlaceOrderAsync(CreateOrderDto dto)
    {
        var order = new Order(dto);
        await uow.Orders.AddAsync(order);
        await uow.SaveChangesAsync();
        return order;
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is `TempData`, `ViewData`, `ViewBag`? How do you pass data from controller to view?

```cs
// 1. ViewData — dictionary, persists for current request only
public IActionResult Index()
{
    ViewData["Title"]   = "Products";
    ViewData["Count"]   = 42;
    return View();
}
// In view: @ViewData["Title"]

// 2. ViewBag — dynamic wrapper over ViewData
public IActionResult Index()
{
    ViewBag.Title = "Products"; // same as ViewData["Title"]
    ViewBag.Count = 42;
    return View();
}
// In view: @ViewBag.Title

// 3. Strongly-typed model (PREFERRED — IntelliSense + type safety)
public IActionResult Index()
{
    var products = _service.GetAll();
    return View(products); // passes model to view
}
// In view: @model IEnumerable<ProductViewModel>
//          @foreach (var p in Model) { }

// 4. TempData — survives ONE redirect (stored in cookie or session)
public IActionResult Create(CreateProductDto dto)
{
    _service.Create(dto);
    TempData["Message"] = "Product created successfully!";
    return RedirectToAction("Index"); // redirect → TempData available
}
// In Index view: @TempData["Message"]
// TempData is cleared after being read (use TempData.Keep() to preserve)
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement response compression, localization, sessions, and cookies?

```cs
// Response compression
builder.Services.AddResponseCompression(options =>
{
    options.EnableForHttps = true; // careful with CRIME/BREACH attacks for sensitive data
    options.Providers.Add<BrotliCompressionProvider>();
    options.Providers.Add<GzipCompressionProvider>();
    options.MimeTypes = ResponseCompressionDefaults.MimeTypes.Concat(["application/json"]);
});
app.UseResponseCompression();

// Localization
builder.Services.AddLocalization(options => options.ResourcesPath = "Resources");
builder.Services.Configure<RequestLocalizationOptions>(options =>
{
    var cultures = new[] { "en", "fr", "de", "ar" };
    options.SetDefaultCulture("en")
           .AddSupportedCultures(cultures)
           .AddSupportedUICultures(cultures);
});
app.UseRequestLocalization();

// Sessions
builder.Services.AddDistributedMemoryCache();
builder.Services.AddSession(options =>
{
    options.IdleTimeout    = TimeSpan.FromMinutes(30);
    options.Cookie.HttpOnly = true;
    options.Cookie.IsEssential = true;
});
app.UseSession();

// In controller:
HttpContext.Session.SetString("UserId", userId);
HttpContext.Session.SetInt32("CartCount", 5);
string? uid = HttpContext.Session.GetString("UserId");

// Cookie policies
builder.Services.Configure<CookiePolicyOptions>(options =>
{
    options.MinimumSameSitePolicy = SameSiteMode.Strict;
    options.HttpOnly              = HttpOnlyPolicy.Always;
    options.Secure                = CookieSecurePolicy.Always;
});
app.UseCookiePolicy();

// Read/write cookies
Response.Cookies.Append("theme", "dark", new CookieOptions
{
    HttpOnly = true, Secure = true, SameSite = SameSiteMode.Strict,
    Expires  = DateTimeOffset.UtcNow.AddDays(30)
});
string? theme = Request.Cookies["theme"];
Response.Cookies.Delete("theme");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use `HttpClient` to consume a Web API?

```cs
// Named/typed HttpClient (preferred over direct HttpClient)
builder.Services.AddHttpClient<IWeatherClient, WeatherClient>(client =>
{
    client.BaseAddress = new Uri("https://api.weather.com");
    client.DefaultRequestHeaders.Add("Accept", "application/json");
    client.Timeout = TimeSpan.FromSeconds(30);
})
.AddStandardResilienceHandler();  // .NET 8+ — retry + circuit breaker

public class WeatherClient(HttpClient client)
{
    public async Task<WeatherDto?> GetCurrentAsync(string city, CancellationToken ct = default)
    {
        var response = await client.GetAsync($"/v1/current?city={Uri.EscapeDataString(city)}", ct);
        response.EnsureSuccessStatusCode();
        return await response.Content.ReadFromJsonAsync<WeatherDto>(ct);
    }

    public async Task<ForecastDto?> PostForecastAsync(ForecastRequest req, CancellationToken ct = default)
    {
        var response = await client.PostAsJsonAsync("/v1/forecast", req, ct);
        response.EnsureSuccessStatusCode();
        return await response.Content.ReadFromJsonAsync<ForecastDto>(ct);
    }
}

// With JWT auth header
builder.Services.AddHttpClient("SecureApi", client =>
    client.BaseAddress = new Uri("https://secure.api.com"))
    .AddHttpMessageHandler<AuthHeaderHandler>();

public class AuthHeaderHandler(ITokenService tokens) : DelegatingHandler
{
    protected override async Task<HttpResponseMessage> SendAsync(
        HttpRequestMessage request, CancellationToken ct)
    {
        var token = await tokens.GetAccessTokenAsync();
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", token);
        return await base.SendAsync(request, ct);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the `.NET Standard`, `.NET Core`, `.NET Framework`, and Xamarin?

| | `.NET Framework` | `.NET Core / .NET 5+` | `.NET Standard` | `Xamarin` |
|-|----------------|----------------------|----------------|---------|
| **Platform** | Windows only | Cross-platform | Specification | Mobile (iOS/Android) |
| **Open source** | Partial | Full | N/A | Full |
| **Status** | Maintenance | Active (.NET 10) | Superseded by .NET | Superseded by MAUI |
| **Use today** | Legacy apps | ✅ All new development | Library targeting | ❌ Use MAUI |

**.NET Standard** was a specification that allowed class libraries to run on both .NET Framework and .NET Core. With .NET 5+ unifying the platform, new libraries should target `net9.0` (or latest) directly. `.NET Standard 2.0` is still useful only when targeting both .NET Framework 4.x and modern .NET simultaneously.

```xml
<!-- Modern library targeting .NET 9 -->
<TargetFramework>net9.0</TargetFramework>

<!-- Library supporting both .NET Framework 4.8 and .NET 9 -->
<TargetFrameworks>net9.0;net48</TargetFrameworks>

<!-- Legacy: use netstandard2.0 only when net48 compat is needed -->
<TargetFramework>netstandard2.0</TargetFramework>
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement WebSockets and Server-Sent Events (SSE) in ASP.NET Core?

```cs
// WebSockets — full-duplex, bidirectional
app.UseWebSockets(new WebSocketOptions { KeepAliveInterval = TimeSpan.FromMinutes(2) });

app.MapGet("/ws", async (HttpContext context) =>
{
    if (!context.WebSockets.IsWebSocketRequest) { context.Response.StatusCode = 400; return; }

    using var ws = await context.WebSockets.AcceptWebSocketAsync();
    var buffer = new byte[4096];

    while (ws.State == WebSocketState.Open)
    {
        var result = await ws.ReceiveAsync(buffer, CancellationToken.None);
        if (result.MessageType == WebSocketMessageType.Close)
            await ws.CloseAsync(WebSocketCloseStatus.NormalClosure, "Closed", CancellationToken.None);
        else
        {
            string msg = Encoding.UTF8.GetString(buffer, 0, result.Count);
            string reply = $"Echo: {msg}";
            await ws.SendAsync(Encoding.UTF8.GetBytes(reply),
                WebSocketMessageType.Text, true, CancellationToken.None);
        }
    }
});

// Server-Sent Events — server pushes, one-direction
app.MapGet("/sse", async (HttpContext context, CancellationToken ct) =>
{
    context.Response.ContentType  = "text/event-stream";
    context.Response.Headers.CacheControl = "no-cache";
    context.Response.Headers.Connection   = "keep-alive";

    for (int i = 0; i < 100 && !ct.IsCancellationRequested; i++)
    {
        await context.Response.WriteAsync($"data: Event {i}\n\n", ct);
        await context.Response.Body.FlushAsync(ct);
        await Task.Delay(1000, ct);
    }
});
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>
