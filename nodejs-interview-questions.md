# 30 Node.js Interview Questions

## Fundamentals & Core Concepts

### 1. What is Node.js and how does it work?
**Answer:** Node.js is a JavaScript runtime built on Chrome's V8 engine that executes JavaScript outside the browser. It uses an event-driven, non-blocking I/O model making it efficient for I/O-heavy operations. The event loop handles asynchronous operations, allowing Node.js to handle thousands of concurrent connections with a single thread.

### 2. Explain the Event Loop in Node.js.
**Answer:** The event loop is the core mechanism that handles asynchronous operations. Phases: timers (setTimeout/setInterval), pending callbacks, idle/prepare, poll (I/O operations), check (setImmediate), close callbacks. Node.js processes callbacks from each phase before moving to the next. Microtasks (Promises, process.nextTick) run between phases.

### 3. What is the difference between process.nextTick() and setImmediate()?
**Answer:** process.nextTick() executes callback after the current operation completes, before the event loop continues (microtask queue). setImmediate() executes in the check phase of the next event loop iteration. nextTick has higher priority and can cause I/O starvation if used recursively. Use setImmediate for deferring execution to next iteration.

### 4. Explain the difference between require() and import in Node.js.
**Answer:** require() is CommonJS syntax, synchronous, cached after first load, can be called conditionally anywhere in code. import is ES6 modules syntax, supports tree-shaking, static analysis, must be at top level. Node.js supports both: use .mjs extension or "type": "module" in package.json for ES modules.

### 5. What is the purpose of package.json and package-lock.json?
**Answer:** package.json defines project metadata, dependencies, scripts, and configuration. It lists dependency versions with semantic versioning (^, ~, exact). package-lock.json locks exact versions of all dependencies and sub-dependencies, ensuring consistent installs across environments. Always commit package-lock.json for reproducible builds.

## Asynchronous Programming

### 6. Explain callbacks, promises, and async/await in Node.js.
**Answer:** Callbacks are functions passed as arguments, leading to callback hell. Promises represent eventual completion/failure with .then()/.catch(), enabling chaining. Async/await is syntactic sugar over promises, making async code look synchronous. Use async/await for readability, promises for multiple parallel operations with Promise.all().

### 7. What is callback hell and how do you avoid it?
**Answer:** Callback hell (pyramid of doom) occurs with nested callbacks, making code hard to read/maintain. Solutions: use promises, async/await, modularize functions, use named functions instead of anonymous ones, or libraries like async.js. Modern approach: convert callbacks to promises with util.promisify().

### 8. How do you handle errors in asynchronous code?
**Answer:** Callbacks: first parameter is error (err, result). Promises: use .catch() or second argument in .then(). Async/await: use try-catch blocks. Always handle promise rejections to avoid unhandled rejection warnings. Use process.on('unhandledRejection') as last resort for logging. Never ignore errors.

### 9. Explain Promise.all(), Promise.race(), Promise.allSettled(), and Promise.any().
**Answer:** Promise.all() waits for all promises, fails if any rejects. Promise.race() returns first settled promise (resolved/rejected). Promise.allSettled() waits for all, returns all results regardless of status. Promise.any() returns first fulfilled promise, ignores rejections unless all fail. Use based on requirement.

### 10. What are streams in Node.js and what types exist?
**Answer:** Streams process data in chunks rather than loading entirely in memory. Types: Readable (read data), Writable (write data), Duplex (both), Transform (modify data while reading/writing). Examples: fs.createReadStream(), process.stdin, http requests/responses. Use .pipe() to connect streams. Efficient for large files.

## Modules & Architecture

### 11. Explain the module system in Node.js.
**Answer:** Node.js uses CommonJS module system. Each file is a module with its own scope. module.exports or exports defines what's accessible externally. require() loads modules. Node.js caches modules after first load. Core modules (fs, http), local modules (./file), and npm modules (node_modules) are available.

### 12. What is the difference between module.exports and exports?
**Answer:** module.exports is the actual object returned by require(). exports is a reference to module.exports. You can add properties to exports, but reassigning exports breaks the reference. Always use module.exports for exporting a single function/class. Use exports for multiple exports (though module.exports works for both).

### 13. How does Node.js handle child processes?
**Answer:** Use child_process module with exec() (shell command, buffered), execFile() (direct execution), spawn() (streaming, large data), fork() (Node.js processes with IPC). spawn() is most memory efficient. fork() enables communication between parent/child via message passing. Use for CPU-intensive tasks or running external programs.

### 14. Explain cluster module and its use cases.
**Answer:** Cluster module creates child processes (workers) sharing the same server port, utilizing multiple CPU cores. Master process manages workers, distributes load. Handles worker crashes with automatic restart. Use for CPU-bound operations, maximizing throughput on multi-core systems. Alternative: PM2 for production process management.

### 15. What is middleware in Express.js?
**Answer:** Middleware are functions with access to req, res, and next. They execute in order, processing requests before reaching route handlers. Types: application-level (app.use()), router-level, error-handling (4 parameters), built-in (express.json()), third-party (cors, helmet). Call next() to pass control or send response to end chain.

## File System & Networking

### 16. Explain the difference between fs.readFile() and fs.createReadStream().
**Answer:** fs.readFile() loads entire file into memory (buffer), suitable for small files, simpler API. fs.createReadStream() reads in chunks, memory-efficient for large files, returns stream. Use readFile for small files or when you need all content at once. Use createReadStream for large files, piping, or progressive processing.

### 17. How do you create an HTTP server in Node.js?
**Answer:** Use http module: http.createServer((req, res) => { res.writeHead(200); res.end('Hello'); }).listen(3000). req is IncomingMessage (readable stream), res is ServerResponse (writable stream). Parse req.url for routing, req.method for HTTP method. For production, use Express.js or other frameworks for routing, middleware, and features.

### 18. What is the difference between req.params, req.query, and req.body in Express?
**Answer:** req.params: route parameters from URL path (/users/:id). req.query: query string parameters (/users?page=1). req.body: request body data (POST/PUT), requires body-parser middleware (express.json()). req.headers for headers. Use appropriate one based on data source and HTTP method conventions.

### 19. How do you handle file uploads in Node.js?
**Answer:** Use middleware like multer (multipart/form-data). Configure storage (disk/memory), file filters, size limits. multer.single('fieldname') for one file, multer.array() for multiple. Access file via req.file or req.files. Validate file types, sanitize filenames, use streams for large files. Store on filesystem, S3, or cloud storage.

### 20. Explain CORS and how to enable it in Express.
**Answer:** CORS (Cross-Origin Resource Sharing) allows controlled access from different origins. Enable with cors middleware: app.use(cors()) for all routes or cors(options) for specific configuration. Options: origin (allowed domains), methods, credentials, headers. For production, whitelist specific origins. Handles preflight OPTIONS requests automatically.

## Security & Best Practices

### 21. How do you secure a Node.js application?
**Answer:** Use helmet for security headers, validate/sanitize input, use parameterized queries, implement rate limiting, enable CORS properly, use HTTPS, keep dependencies updated (npm audit), use environment variables for secrets, implement proper authentication/authorization, log security events, use CSP headers, prevent XSS and injection attacks.

### 22. What is JWT and how do you implement it in Node.js?
**Answer:** JWT (JSON Web Token) for stateless authentication. Use jsonwebtoken library: jwt.sign(payload, secret, options) to create, jwt.verify(token, secret) to validate. Store in HTTP-only cookies or Authorization header. Include expiration, use refresh tokens, never store sensitive data in payload. Validate on protected routes via middleware.

### 23. How do you handle environment variables in Node.js?
**Answer:** Use dotenv package to load variables from .env file. Access via process.env.VARIABLE_NAME. Never commit .env to version control. Use different files for environments (.env.development, .env.production). Validate required variables at startup. Use config management for complex scenarios. Deploy platforms often provide native env var management.

### 24. Explain SQL injection and how to prevent it in Node.js.
**Answer:** SQL injection exploits unsanitized user input in queries. Prevention: use parameterized queries/prepared statements (?, placeholders), ORMs (Sequelize, TypeORM), validate/sanitize input, use principle of least privilege for DB users, avoid string concatenation in queries. ORMs provide automatic escaping and query building.

### 25. What is helmet and why should you use it?
**Answer:** Helmet is Express middleware that sets security-related HTTP headers. It includes 15+ middlewares: Content-Security-Policy, X-DNS-Prefetch-Control, X-Frame-Options, X-Content-Type-Options, Strict-Transport-Security, etc. Use app.use(helmet()) to enable defaults. Protects against XSS, clickjacking, MIME sniffing. Essential for production applications.

## Performance & Debugging

### 26. How do you optimize Node.js application performance?
**Answer:** Use async operations, implement caching (Redis), enable compression (gzip), use CDN for static files, optimize database queries, use connection pooling, implement pagination, profile with clinic.js or node --inspect, use clustering, minimize synchronous operations, use streams for large data, keep dependencies minimal, use production mode.

### 27. What is memory leak and how do you detect it in Node.js?
**Answer:** Memory leak occurs when memory isn't released, causing growing memory usage. Causes: global variables, closures, event listeners not removed, cached data without limits. Detect with: process.memoryUsage(), heap snapshots, Chrome DevTools, clinic.js, memwatch-next. Use --inspect flag, take heap snapshots, compare over time. Fix by proper cleanup, weak references.

### 28. How do you debug a Node.js application?
**Answer:** Use console.log for simple debugging. Node.js debugger: node --inspect or --inspect-brk, connect with Chrome DevTools or VS Code. Use breakpoints, step through code, inspect variables. Debug module for conditional logging. Use winston/morgan for structured logging. Profile performance with node --prof. Monitor with APM tools like New Relic, Datadog.

### 29. Explain worker threads in Node.js.
**Answer:** Worker threads enable parallel execution for CPU-intensive tasks without blocking the event loop. Use worker_threads module: create workers with new Worker(filename), communicate via postMessage/on('message'). Share memory with SharedArrayBuffer. Unlike child processes, workers share same process and memory. Use for compute-heavy operations, not I/O.

### 30. What is the purpose of the Buffer class in Node.js?
**Answer:** Buffer handles binary data directly, as JavaScript doesn't have native byte array. Used for file operations, network protocols, cryptography, streams. Create with Buffer.alloc(), Buffer.from(). Methods: write(), toString(), slice(), concat(). In streams, data events emit buffers. Modern code often uses TypedArray or ArrayBuffer for binary data.
