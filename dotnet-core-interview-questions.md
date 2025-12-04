# 30 .NET Core Interview Questions

## Fundamentals & Architecture

### 1. What is .NET Core and how does it differ from .NET Framework?
**Answer:** .NET Core is a cross-platform, open-source framework for building modern applications. Key differences: .NET Core runs on Windows, Linux, and macOS (vs Windows-only), modular design with NuGet packages, better performance, side-by-side versioning, optimized for cloud and containers, and lightweight runtime. .NET 5+ unified the platforms.

### 2. Explain the .NET Core application architecture and hosting model.
**Answer:** .NET Core apps use a host that manages app startup, lifetime, and configuration. The host builds a pipeline of middleware components, configures services via dependency injection, and handles HTTP requests. Two hosting models: in-process (IIS) for better performance, and out-of-process (Kestrel) for cross-platform support.

### 3. What is the purpose of Program.cs and Startup.cs (or Program.cs in .NET 6+)?
**Answer:** Program.cs contains the Main method and creates the host. In .NET 6+, it's simplified with top-level statements. Startup.cs (pre-.NET 6) had ConfigureServices (DI registration) and Configure (middleware pipeline). .NET 6+ merged these into a single Program.cs with WebApplicationBuilder pattern.

### 4. Explain the middleware pipeline in ASP.NET Core.
**Answer:** Middleware are components that handle requests/responses in a pipeline. Each can process requests, call next middleware, or short-circuit. Order matters: exception handling → HTTPS redirection → static files → routing → authentication → authorization → endpoints. Use app.Use, app.Map, or app.Run to configure.

### 5. What is Kestrel and when would you use it?
**Answer:** Kestrel is a cross-platform, high-performance web server built into ASP.NET Core. It can run standalone or behind reverse proxies (IIS, Nginx, Apache). Use standalone for microservices/containers, or with reverse proxy for SSL termination, load balancing, and additional security in production.

## Dependency Injection & Services

### 6. Explain dependency injection in .NET Core and its service lifetimes.
**Answer:** DI is built into .NET Core for loose coupling and testability. Lifetimes: Transient (new instance per request), Scoped (one per HTTP request), Singleton (one for app lifetime). Register in ConfigureServices/Program.cs. Use constructor injection to receive dependencies.

### 7. How do you register services in .NET Core?
**Answer:** Use IServiceCollection in ConfigureServices or Program.cs. Methods: AddTransient, AddScoped, AddSingleton. For interfaces: services.AddScoped<IRepository, Repository>(). For implementations: services.AddSingleton<MyService>(). Can also use extension methods like AddDbContext, AddControllers, AddAuthentication.

### 8. What is the difference between IServiceCollection and IServiceProvider?
**Answer:** IServiceCollection is used to register services during application startup (configuration phase). IServiceProvider is the container that resolves and provides instances at runtime (resolution phase). Build IServiceProvider from IServiceCollection using BuildServiceProvider(), though ASP.NET Core handles this automatically.

### 9. How do you implement and use IHostedService or BackgroundService?
**Answer:** IHostedService runs background tasks in the application lifetime. BackgroundService is a base class that simplifies implementation with ExecuteAsync method. Register with services.AddHostedService<MyService>(). Use for background processing, timed tasks, message queue consumers, or scheduled jobs.

### 10. Explain the Options pattern in .NET Core.
**Answer:** Options pattern strongly types configuration sections using IOptions<T>, IOptionsSnapshot<T>, or IOptionsMonitor<T>. Bind config sections to POCOs: services.Configure<MySettings>(Configuration.GetSection("MySettings")). IOptions is singleton, IOptionsSnapshot is scoped (reloads per request), IOptionsMonitor detects config changes in real-time.

## Web API Development

### 11. What are the differences between [FromBody], [FromQuery], [FromRoute], and [FromForm]?
**Answer:** Model binding attributes specify parameter source: [FromBody] reads from request body (JSON), [FromQuery] from query string, [FromRoute] from URL route template, [FromForm] from form data. [FromHeader] gets from headers, [FromServices] from DI container. ASP.NET Core infers simple types from query, complex from body.

### 12. How do you implement global exception handling in ASP.NET Core?
**Answer:** Use middleware: app.UseExceptionHandler("/error") or custom middleware with try-catch. For APIs, create an exception handler middleware that catches exceptions, logs them, and returns consistent error responses. Use ProblemDetails for RFC 7807 compliance. Can also use IExceptionFilter for MVC/Web API.

### 13. Explain model validation and custom validation attributes.
**Answer:** Use data annotations ([Required], [StringLength], [Range], [EmailAddress]) on models. Check ModelState.IsValid in controllers. Create custom validators by inheriting ValidationAttribute and overriding IsValid. For complex validation, implement IValidatableObject. Use FluentValidation library for more advanced scenarios.

### 14. What is content negotiation in ASP.NET Core Web API?
**Answer:** Content negotiation automatically formats responses based on Accept header. ASP.NET Core supports JSON (default), XML (with AddXmlSerializerFormatters), and custom formatters. Configure in AddControllers with options.ReturnHttpNotAcceptable = true to return 406 for unsupported formats.

### 15. How do you implement versioning in ASP.NET Core APIs?
**Answer:** Use Microsoft.AspNetCore.Mvc.Versioning package. Strategies: URL path (/api/v1/users), query string (?api-version=1.0), header (api-version: 1.0), or media type. Configure in services.AddApiVersioning() with options for default version, reporting, and reader. Use [ApiVersion("1.0")] attribute on controllers.

## Entity Framework Core

### 16. Explain the DbContext in Entity Framework Core.
**Answer:** DbContext represents a session with the database, providing change tracking, query composition, and saving data. Contains DbSet<T> properties for entities. Configure in OnModelCreating for fluent API mappings. Scoped lifetime in DI ensures one instance per request, with automatic disposal.

### 17. What are the different approaches to create models in EF Core?
**Answer:** Code First: create classes, generate database from migrations. Database First: scaffold models from existing database with dotnet ef dbcontext scaffold. Model First (not directly supported): design model, generate code. Code First is most common, offers version control for schema and better CI/CD integration.

### 18. Explain lazy loading, eager loading, and explicit loading in EF Core.
**Answer:** Lazy loading: related data loads automatically when accessed (requires proxies). Eager loading: load related data upfront using Include(). Explicit loading: manually load related data with Entry().Collection().Load(). Eager loading prevents N+1 problem, lazy loading simplifies code but can cause performance issues.

### 19. How do you handle database migrations in EF Core?
**Answer:** Use EF Core migrations to version schema changes. Commands: dotnet ef migrations add MigrationName (create), dotnet ef database update (apply), dotnet ef migrations remove (undo last). Migrations contain Up/Down methods. Apply in production via scripts, startup code, or deployment pipelines.

### 20. What is the Unit of Work pattern and how does EF Core implement it?
**Answer:** Unit of Work tracks changes and commits them as a single transaction. DbContext implements this pattern: tracks entity changes, maintains identity map, coordinates SaveChanges() to execute all changes in one transaction. Ensures data consistency and reduces database roundtrips.

## Security & Authentication

### 21. How do you implement JWT authentication in ASP.NET Core?
**Answer:** Install Microsoft.AspNetCore.Authentication.JwtBearer. Configure in services: AddAuthentication with JwtBearerDefaults, set TokenValidationParameters (issuer, audience, key). Use app.UseAuthentication() and app.UseAuthorization(). Protect endpoints with [Authorize]. Generate tokens with JwtSecurityTokenHandler after validating credentials.

### 22. Explain the difference between Authentication and Authorization middleware.
**Answer:** UseAuthentication() identifies the user from request (sets User/Principal). UseAuthorization() checks if authenticated user has permission. Order matters: authentication must come first. Authentication reads tokens/cookies, authorization evaluates policies and roles against endpoints' [Authorize] attributes.

### 23. What are authorization policies and how do you create custom ones?
**Answer:** Policies define authorization requirements beyond simple roles. Create in services.AddAuthorization with options.AddPolicy(). Use requirements (IAuthorizationRequirement) and handlers (AuthorizationHandler<T>). Apply with [Authorize(Policy = "PolicyName")]. Supports claims-based, role-based, and custom logic authorization.

### 24. How do you protect against CSRF attacks in ASP.NET Core?
**Answer:** ASP.NET Core includes anti-forgery tokens automatically for forms. For APIs, use SameSite cookie attribute, validate Origin/Referer headers, or use token-based auth (JWT) instead of cookies. Configure with services.AddAntiforgery() and validate with [ValidateAntiForgeryToken]. SPAs should use header-based tokens.

### 25. Explain data protection in ASP.NET Core.
**Answer:** Data Protection API provides cryptographic services for protecting data. Automatically used for authentication cookies, anti-forgery tokens, and session state. IDataProtectionProvider creates protectors with purpose strings. Supports key rotation, key ring storage (file system, Azure, Redis), and distributed scenarios.

## Performance & Advanced Topics

### 26. How do you implement caching in ASP.NET Core?
**Answer:** Three types: in-memory cache (IMemoryCache for single server), distributed cache (IDistributedCache for Redis/SQL Server in multi-server), response caching (HTTP caching with [ResponseCache]). Configure in services, inject, and use Set/Get methods. Use cache tags, sliding/absolute expiration, and cache dependencies.

### 27. Explain async/await in .NET Core and best practices.
**Answer:** async/await enables non-blocking asynchronous operations, improving scalability. Return Task or Task<T>, use await for async calls, propagate all the way up. Best practices: don't block with .Result/.Wait(), use ConfigureAwait(false) in libraries, avoid async void (except event handlers), use async throughout I/O operations.

### 28. What is the difference between IActionResult and ActionResult<T>?
**Answer:** IActionResult is non-generic, returns various result types (Ok, BadRequest, NotFound). ActionResult<T> is generic, returns specific type T or IActionResult, providing better documentation and type safety for OpenAPI/Swagger. Enables automatic type inference and implicit conversion from T to ActionResult<T>.

### 29. How do you implement health checks in ASP.NET Core?
**Answer:** Use Microsoft.Extensions.Diagnostics.HealthChecks. Add services.AddHealthChecks() with checks for dependencies (database, Redis, APIs). Configure endpoint with app.MapHealthChecks("/health"). Create custom checks implementing IHealthCheck. Returns Healthy/Degraded/Unhealthy status. Integrate with orchestrators and monitoring tools.

### 30. Explain SignalR and its use cases in ASP.NET Core.
**Answer:** SignalR enables real-time, bidirectional communication between server and clients using WebSockets (falls back to Server-Sent Events or Long Polling). Use cases: chat applications, live dashboards, real-time notifications, collaborative editing, gaming, live data feeds. Create hubs inheriting Hub, configure with app.MapHub<T>(), use JavaScript/C# clients to connect.
