# 30 PHP Laravel Interview Questions

## Laravel Fundamentals

### 1. What is Laravel and what are its key features?
**Answer:** Laravel is a PHP framework following MVC pattern with elegant syntax. Key features: Eloquent ORM, Blade templating, Artisan CLI, routing, middleware, authentication, authorization (Gates/Policies), queue system, events, task scheduling, robust ecosystem (Forge, Vapor, Nova). Built on Symfony components, promotes clean code and rapid development.

### 2. Explain the Laravel request lifecycle.
**Answer:** Request enters through public/index.php → bootstrap/app.php loads framework → HTTP Kernel handles request → service providers registered/booted → middleware processes request → router dispatches to controller → response generated → middleware processes response → response sent. Kernel is the "traffic cop" managing the entire flow.

### 3. What are service providers and service containers in Laravel?
**Answer:** Service container is IoC (Inversion of Control) container for dependency injection and binding. Service providers register bindings, singletons, and boot services in register() and boot() methods. All providers in config/app.php. Container resolves dependencies automatically. Use app()->bind() or App::singleton() to register, resolve with app() or dependency injection.

### 4. Explain dependency injection in Laravel.
**Answer:** Laravel automatically resolves dependencies via type-hinting in constructors/methods. The service container inspects type hints and injects instances. Constructor injection for class dependencies, method injection for controller methods. Use interfaces for flexibility, bind implementations in service providers. Promotes testability and loose coupling.

### 5. What is the difference between bind() and singleton() in service container?
**Answer:** bind() creates new instance each time it's resolved (transient). singleton() creates instance once and returns same instance for subsequent resolutions. Use bind() for stateless services that can be recreated, singleton() for expensive objects or when shared state is needed (DB connections, cache managers).

## Routing & Controllers

### 6. How does routing work in Laravel?
**Answer:** Routes defined in routes/web.php (web middleware, sessions) and routes/api.php (stateless API). Define with Route::get/post/put/delete/patch(). Supports route parameters, constraints, groups, names, prefixes, middleware. Route model binding automatically injects models. Use resource routes for RESTful controllers: Route::resource('posts', PostController::class).

### 7. What is route model binding in Laravel?
**Answer:** Automatically injects model instances in route parameters. Implicit: Route::get('/user/{user}', function(User $user) {}) - Laravel queries by ID. Explicit: Route::model('user', User::class) or custom resolution in RouteServiceProvider. For soft deletes, use withTrashed(). Can customize the column with getRouteKeyName() method.

### 8. Explain middleware in Laravel and create a custom one.
**Answer:** Middleware filters HTTP requests. Create with php artisan make:middleware. Implement handle($request, Closure $next) - process before/after by returning $next($request) at start/end. Register in app/Http/Kernel.php (global, route groups, or individual). Common uses: auth, CORS, logging, rate limiting, role checking.

### 9. What are resource controllers and how do you create them?
**Answer:** Resource controllers handle CRUD operations with conventional method names: index, create, store, show, edit, update, destroy. Create with php artisan make:controller PostController --resource. Use Route::resource('posts', PostController::class). Can limit actions with only/except. Use apiResource for API (excludes create/edit views).

### 10. How do you handle API versioning in Laravel?
**Answer:** Strategies: URI versioning (Route::prefix('v1')), header versioning (Accept: application/vnd.api.v1+json), or subdomain. Organize in route files (api_v1.php, api_v2.php) or route groups. Use API resources for transformations, maintain backward compatibility, document with tools like Swagger/OpenAPI. Consider deprecation policies.

## Eloquent ORM & Database

### 11. What is Eloquent ORM and how does it work?
**Answer:** Eloquent is Laravel's Active Record ORM. Each model represents a database table. Provides intuitive syntax for CRUD: Model::find(), create(), update(), delete(). Supports relationships, eager loading, query scopes, mutators/accessors, events. Uses query builder underneath. Automatically handles timestamps, mass assignment protection, and type casting.

### 12. Explain Eloquent relationships and their types.
**Answer:** One-to-One (hasOne/belongsTo), One-to-Many (hasMany/belongsTo), Many-to-Many (belongsToMany with pivot table), Has-Many-Through, polymorphic (morphTo/morphMany), many-to-many polymorphic. Define in model methods, access as properties. Eager load with with() to prevent N+1 queries. Can customize foreign keys, local keys.

### 13. What is the N+1 query problem and how do you solve it in Laravel?
**Answer:** Occurs when loading relationships in loops. Example: 100 users + 100 queries for posts. Solutions: eager loading with with(['posts']), lazy eager loading with load(), select specific columns. Use debugbar or telescope to detect. Can also use database query log. withCount() for counting relationships without loading data.

### 14. How do migrations work in Laravel?
**Answer:** Migrations version control database schema. Create with php artisan make:migration. Define up() for changes, down() for rollback. Use Schema builder methods: create, table, drop, column types, indexes, foreign keys. Run with migrate, rollback with migrate:rollback. Use seeders for sample data. Track in migrations table.

### 15. Explain query scopes in Eloquent.
**Answer:** Scopes encapsulate query logic for reuse. Local scopes: methods prefixed with scope (scopeActive), call without prefix (User::active()). Global scopes: apply to all queries, implement Scope interface or use anonymous functions in boot(). Use for soft deletes, multi-tenancy, common filters. Chain multiple scopes.

## Blade Templating & Views

### 16. What is Blade templating engine and its advantages?
**Answer:** Blade is Laravel's templating engine with simple yet powerful syntax. Advantages: doesn't restrict PHP code, template inheritance (@extends, @section, @yield), components, slots, conditional directives (@if, @foreach, @auth), CSRF protection, escaping ({{ }} auto-escapes, {!! !!} raw). Compiled to PHP for performance.

### 17. How do you create and use Blade components?
**Answer:** Create with php artisan make:component Button. Class-based or anonymous. Define props, render logic in class. Template in resources/views/components. Use with <x-button>. Pass data via attributes: <x-button color="blue" :user="$user">. Access in component via $attributes, $slot. Support slots for content injection. Promotes reusability.

### 18. Explain @include, @extends, and @component directives.
**Answer:** @extends defines parent layout, child uses @section to fill @yield slots. @include inserts another view inline, can pass data. @component (deprecated in favor of x- syntax) for components. Use @extends for layouts, @include for partials (header, footer), components for reusable UI elements with logic.

### 19. How do you pass data to views in Laravel?
**Answer:** Multiple ways: compact('var1', 'var2'), array ['key' => 'value'], with('key', 'value'), view()->share() for all views, View Composers for specific views, service providers for global data. In controllers return view('name', compact('data')). Can chain methods: view()->with()->with().

### 20. What are view composers and when to use them?
**Answer:** View composers bind data to views before rendering. Register in service provider: View::composer(['profile', 'dashboard'], function($view) { $view->with('data', ...); }). Use for sidebars, navigation, shared data across multiple views. Cleaner than passing same data from every controller. Supports class-based composers.

## Authentication & Authorization

### 21. How does authentication work in Laravel?
**Answer:** Laravel provides complete auth system. Use Laravel Breeze/Jetstream/Fortify starters. Guards define how users are authenticated (session, token). Providers define how users are retrieved (Eloquent, database). Configure in config/auth.php. Auth facade/middleware for checking. Methods: Auth::attempt(), check(), user(), login(), logout(). Supports multiple guards.

### 22. What is the difference between Gates and Policies?
**Answer:** Both handle authorization. Gates are closures for simple checks, defined in AuthServiceProvider: Gate::define('update-post', fn($user, $post) => $user->id === $post->user_id). Policies are classes for model-related authorization, generated per model. Use policies for resources, gates for general actions. Check with $user->can(), @can, or authorize() method.

### 23. How do you implement API authentication using Sanctum?
**Answer:** Sanctum provides token-based auth for SPAs and mobile apps. Install, run migrations. Issue tokens: $user->createToken('token-name')->plainTextToken. Protect routes with sanctum middleware. Store token on client, send in Authorization: Bearer {token} header. Supports token abilities (scopes), revocation, expiration. Simpler alternative to Passport for most use cases.

### 24. Explain Laravel Passport and its use cases.
**Answer:** Passport provides full OAuth2 server implementation. Use for: third-party API access, authorization code grant, password grant, client credentials. Install, migrate, install passport, create clients. More complex than Sanctum. Use when you need OAuth2 compliance, multiple client types, or authorization server. Includes token scopes, refresh tokens.

### 25. How do you implement role-based access control (RBAC) in Laravel?
**Answer:** Use policies/gates with roles. Popular packages: spatie/laravel-permission. Define roles and permissions, attach to users. Check with @role, @can, hasRole(), can(). Store in database with pivot tables. Implement custom middleware for role checking. Can also use relationships: User hasMany Roles, Role hasMany Permissions.

## Advanced Features

### 26. What are Laravel queues and how do they work?
**Answer:** Queues defer time-consuming tasks (emails, API calls) to background processing. Create job with php artisan make:job. Implement handle() method. Dispatch with JobClass::dispatch() or dispatch(new Job()). Configure drivers: database, Redis, SQS, Beanstalkd. Run with php artisan queue:work. Supports delays, retries, priorities, job chaining.

### 27. Explain events and listeners in Laravel.
**Answer:** Events provide observer pattern for decoupling. Create event (make:event) and listener (make:listener). Register in EventServiceProvider. Fire event: event(new OrderShipped($order)). Listener handles logic. Use for: logging, notifications, updating related data. Supports queued listeners for async processing. Can have multiple listeners per event.

### 28. How does task scheduling work in Laravel?
**Answer:** Schedule recurring tasks in app/Console/Kernel.php schedule() method. Define with $schedule->command/job/call()->daily/hourly/cron(). Expressive syntax: dailyAt('13:00'), weekdays(), when(). Run scheduler: php artisan schedule:run (add to cron). Tasks can be queued, run in background, prevent overlaps, maintain on one server.

### 29. What are Laravel collections and their advantages?
**Answer:** Collections are powerful array wrappers with fluent interface. Methods: map(), filter(), reduce(), pluck(), groupBy(), sortBy(), etc. Lazy collections for memory efficiency with large datasets. Eloquent returns collections. Chainable, immutable operations. Higher order messages: $collection->map->name. Provides functional programming capabilities.

### 30. Explain Laravel caching strategies and drivers.
**Answer:** Cache stores expensive computations. Drivers: file, database, Redis, Memcached, DynamoDB. Use Cache::put/get/remember/forever/forget(). remember() fetches or stores if missing. Cache tags for grouping (Redis/Memcached only). Model caching, query caching, full-page caching. Configure in config/cache.php. Use cache:clear command. Essential for performance optimization.
