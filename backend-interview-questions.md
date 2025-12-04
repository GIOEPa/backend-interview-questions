# 79 Backend Interview Questions

## Object-Oriented Programming (OOP)

### 1. What are the four pillars of Object-Oriented Programming?
**Answer:** The four pillars are: **Encapsulation** (bundling data and methods, hiding internal state), **Abstraction** (hiding complex implementation, showing only essential features), **Inheritance** (creating new classes from existing ones, promoting code reuse), and **Polymorphism** (objects taking multiple forms, same interface with different implementations). These principles promote modularity, reusability, and maintainability.

### 2. Explain the difference between abstraction and encapsulation.
**Answer:** Abstraction focuses on hiding complexity and showing only relevant features (what an object does). Encapsulation focuses on hiding internal state and requiring interaction through methods (how it's implemented). Example: a car's interface (steering, pedals) is abstraction; the engine's internal workings being hidden is encapsulation. Abstraction is design-level, encapsulation is implementation-level.

### 3. What is the difference between an abstract class and an interface?
**Answer:** Abstract classes can have concrete methods, constructors, and state (fields). Interfaces define contracts with only method signatures (pre-Java 8/C# 8). A class can implement multiple interfaces but inherit from one abstract class. Use interfaces for "can-do" relationships (Flyable), abstract classes for "is-a" with shared implementation. Modern languages blur this distinction with default methods.

### 4. Explain method overloading vs method overriding.
**Answer:** **Overloading** (compile-time polymorphism): multiple methods with same name but different parameters in the same class. **Overriding** (runtime polymorphism): subclass provides specific implementation of parent's method with same signature. Overloading is resolved at compile time, overriding at runtime. Overriding requires inheritance, overloading doesn't.

### 5. What is the SOLID principle?
**Answer:** SOLID is five design principles: **S**ingle Responsibility (class has one reason to change), **O**pen/Closed (open for extension, closed for modification), **L**iskov Substitution (subclasses substitutable for parent), **I**nterface Segregation (many specific interfaces vs one general), **D**ependency Inversion (depend on abstractions, not concretions). Following SOLID creates maintainable, scalable, testable code.

### 6. Explain composition vs inheritance and when to use each.
**Answer:** Inheritance represents "is-a" relationship, composition represents "has-a". Composition provides greater flexibility, loose coupling, and avoids deep inheritance hierarchies. Use inheritance for clear hierarchical relationships with shared behavior. Use composition for code reuse without tight coupling. Favor composition over inheritance to avoid fragile base class problem.

### 7. What are design patterns? Name and explain three common ones.
**Answer:** Design patterns are reusable solutions to common problems. **Singleton**: ensures one instance of a class (database connections). **Factory**: creates objects without specifying exact class (different payment processors). **Observer**: one-to-many dependency where observers are notified of state changes (event systems). Patterns promote best practices and common vocabulary.

### 8. Explain the Singleton pattern and its drawbacks.
**Answer:** Singleton ensures only one instance exists globally, providing a global access point. Implementation: private constructor, static instance, getInstance() method. Drawbacks: violates Single Responsibility, makes unit testing difficult (global state), can cause coupling, thread-safety concerns, and hides dependencies. Consider dependency injection instead.

### 9. What is dependency injection and why is it useful?
**Answer:** DI is providing dependencies to a class rather than creating them internally. Benefits: loose coupling, easier testing (mock dependencies), follows Dependency Inversion principle, promotes single responsibility. Types: constructor injection (preferred), setter injection, interface injection. Frameworks handle DI automatically (Spring, .NET Core, NestJS).

### 10. Explain the difference between static and instance members.
**Answer:** Static members belong to the class itself, shared across all instances, accessed via class name. Instance members belong to specific objects, each object has its own copy. Static: utilities, constants, shared state (use carefully). Instance: object-specific state and behavior. Static methods can't access instance members directly.

## Fundamentals & Architecture

### 11. What is the difference between monolithic and microservices architecture?
**Answer:** Monolithic architecture is a single unified application where all components are interconnected and interdependent. Microservices architecture breaks the application into small, independent services that communicate via APIs. Microservices offer better scalability, fault isolation, and technology flexibility, but add complexity in deployment and inter-service communication.

### 12. Explain the CAP theorem and its implications for distributed systems.
**Answer:** CAP theorem states that a distributed system can only guarantee two of three properties: Consistency (all nodes see the same data), Availability (every request gets a response), and Partition tolerance (system continues despite network failures). Most modern systems choose CP (like MongoDB) or AP (like Cassandra) based on their requirements.

### 13. What is REST and what are its key constraints?
**Answer:** REST (Representational State Transfer) is an architectural style for web services. Key constraints include: statelessness, client-server separation, cacheability, uniform interface, layered system, and optional code-on-demand. RESTful APIs use HTTP methods (GET, POST, PUT, DELETE) and are resource-based.

### 14. What is the difference between authentication and authorization?
**Answer:** Authentication verifies who you are (identity verification through credentials like username/password). Authorization determines what you can access (permission to perform actions or access resources). Authentication happens first, then authorization checks permissions.

### 15. Explain the concept of idempotency in API design.
**Answer:** An idempotent operation produces the same result regardless of how many times it's executed. GET, PUT, and DELETE are idempotent HTTP methods, while POST is not. Idempotency is crucial for handling network failures and retries without unintended side effects.

## Networking & HTTP

### 16. Explain the HTTP request lifecycle from browser to server and back.
**Answer:** **DNS Resolution**: browser resolves domain to IP. **TCP Connection**: three-way handshake (SYN, SYN-ACK, ACK). **TLS Handshake** (HTTPS): certificate exchange, key negotiation. **HTTP Request**: browser sends request (method, headers, body). **Server Processing**: routing, middleware, business logic, database queries. **HTTP Response**: status code, headers, body. **Connection Close/Keep-Alive**: connection management. Browser renders response.

### 17. What are the different HTTP methods and their purposes?
**Answer:** **GET**: retrieve resource (idempotent, cacheable). **POST**: create resource, submit data (non-idempotent). **PUT**: update/replace entire resource (idempotent). **PATCH**: partial update (may be idempotent). **DELETE**: remove resource (idempotent). **HEAD**: like GET but no body (metadata only). **OPTIONS**: supported methods (CORS preflight). Use appropriate method for semantic correctness and client expectations.

### 18. Explain HTTP status codes and their categories.
**Answer:** **1xx Informational**: 100 Continue, 101 Switching Protocols. **2xx Success**: 200 OK, 201 Created, 204 No Content. **3xx Redirection**: 301 Permanent, 302 Found, 304 Not Modified. **4xx Client Error**: 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 429 Too Many Requests. **5xx Server Error**: 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable. Use correct codes for API clarity.

### 19. What is the difference between HTTP/1.1, HTTP/2, and HTTP/3?
**Answer:** **HTTP/1.1**: text-based, one request per connection (or pipelining), head-of-line blocking. **HTTP/2**: binary protocol, multiplexing (multiple requests per connection), server push, header compression (HPACK), stream prioritization. **HTTP/3**: uses QUIC over UDP instead of TCP, faster connection establishment, better performance on poor networks, no head-of-line blocking at transport layer. HTTP/2 and HTTP/3 significantly improve performance.

### 20. Explain the OSI model and its seven layers.
**Answer:** **7. Application** (HTTP, FTP, SMTP). **6. Presentation** (encryption, encoding). **5. Session** (connection management). **4. Transport** (TCP, UDP, ports). **3. Network** (IP, routing). **2. Data Link** (MAC addresses, switches). **1. Physical** (cables, signals). Each layer serves the layer above. Understanding helps troubleshoot network issues and design protocols.

### 21. What is TCP vs UDP and when would you use each?
**Answer:** **TCP** (Transmission Control Protocol): connection-oriented, reliable, ordered delivery, error checking, slower, used for HTTP, email, file transfer. **UDP** (User Datagram Protocol): connectionless, unreliable, no ordering, faster, lower overhead, used for streaming, gaming, DNS, VoIP. Use TCP when reliability matters, UDP when speed and low latency are critical and some data loss is acceptable.

### 22. Explain DNS and how domain name resolution works.
**Answer:** DNS (Domain Name System) translates domain names to IP addresses. Process: **Browser cache** → **OS cache** → **Router cache** → **ISP DNS resolver** (recursive query) → **Root DNS servers** → **TLD servers** (.com) → **Authoritative name servers** (domain's DNS). Response cached at multiple levels with TTL. DNS uses UDP port 53 for queries, TCP for zone transfers. Can be secured with DNSSEC.

### 23. What is a CDN and how does it work?
**Answer:** CDN (Content Delivery Network) distributes content across geographically distributed servers (edge servers). User requests routed to nearest edge server based on DNS, Anycast, or geolocation. Benefits: reduced latency, decreased origin load, DDoS protection, better availability. Caches static assets (images, CSS, JS), some CDNs cache dynamic content. Popular: CloudFlare, Akamai, AWS CloudFront. Use for global applications.

### 24. Explain WebSockets and how they differ from HTTP.
**Answer:** WebSockets provide full-duplex, persistent connections over single TCP connection. Unlike HTTP's request-response, WebSockets enable real-time bidirectional communication. Handshake starts with HTTP Upgrade request, then switches to WebSocket protocol. Lower overhead, no repeated headers. Use for: chat, live updates, gaming, real-time dashboards. Alternative: Server-Sent Events (SSE) for one-way server-to-client.

### 25. What are common network security threats and how do you mitigate them?
**Answer:** **DDoS attacks**: rate limiting, CDN, traffic filtering. **Man-in-the-Middle**: use HTTPS/TLS, certificate pinning. **DNS attacks**: DNSSEC, use reputable DNS providers. **Port scanning**: firewall rules, close unused ports. **Packet sniffing**: encryption (TLS/VPN). **IP spoofing**: ingress/egress filtering. Implement defense in depth: firewalls, IDS/IPS, network segmentation, monitoring, regular security audits.

## Databases & Data Management

### 26. What is database normalization and why is it important?
**Answer:** Normalization is organizing data to reduce redundancy and improve data integrity. It involves dividing tables and establishing relationships through normal forms (1NF, 2NF, 3NF, BCNF). Benefits include eliminating data anomalies, reducing storage, and maintaining consistency, though it may require more joins.

### 27. Explain ACID properties in databases.
**Answer:** ACID ensures reliable database transactions: Atomicity (all or nothing), Consistency (valid state transitions), Isolation (concurrent transactions don't interfere), and Durability (committed changes persist). These properties guarantee data integrity in relational databases.

### 28. What is the N+1 query problem and how do you solve it?
**Answer:** N+1 problem occurs when fetching a list of N items requires N additional queries to fetch related data. For example, loading 100 users then querying their profiles separately (101 total queries). Solutions include eager loading, join queries, or using batch loaders/DataLoader patterns.

### 29. When would you use SQL vs NoSQL databases?
**Answer:** Use SQL for: structured data, complex queries, ACID compliance, relationships. Use NoSQL for: unstructured/semi-structured data, horizontal scaling, high write loads, flexible schemas, specific use cases (document stores, key-value, graph, time-series).

### 30. Explain database indexing and its trade-offs.
**Answer:** Indexes are data structures (typically B-trees) that improve query performance by allowing faster data retrieval. Trade-offs: faster reads but slower writes (indexes must be updated), additional storage space, and maintenance overhead. Index strategically on frequently queried columns.

## Performance & Scalability

### 31. What is caching and what are common caching strategies?
**Answer:** Caching stores frequently accessed data in fast storage to reduce latency and load. Strategies include: Cache-aside (lazy loading), Write-through (write to cache and DB), Write-back (async write), and Read-through (cache loads from DB). Common tools: Redis, Memcached.

### 32. Explain horizontal vs vertical scaling.
**Answer:** Vertical scaling adds more resources (CPU, RAM) to existing servers - simpler but has limits. Horizontal scaling adds more servers - unlimited scalability, fault tolerance, but requires load balancing, distributed data, and complexity in consistency.

### 33. What is a load balancer and what algorithms does it use?
**Answer:** Load balancers distribute traffic across multiple servers. Algorithms include: Round Robin (sequential), Least Connections (fewest active), IP Hash (client IP based), Weighted (by server capacity), and Least Response Time. They improve availability, performance, and enable scaling.

### 34. How do you handle rate limiting in APIs?
**Answer:** Rate limiting controls request frequency per user/IP. Approaches: Fixed window (requests per time window), Sliding window (moving time frame), Token bucket (refilling tokens), Leaky bucket (constant rate). Implement using middleware, Redis counters, or API gateways.

### 35. What are database connection pools and why use them?
**Answer:** Connection pools maintain reusable database connections instead of creating/closing for each request. Benefits: reduced overhead, better performance, controlled resource usage, faster response times. Configure pool size based on concurrent users and database capacity.

## Security

### 36. Explain JWT (JSON Web Tokens) and their use cases.
**Answer:** JWTs are self-contained tokens with header, payload, and signature. Used for stateless authentication - server doesn't store sessions. Benefits: scalable, cross-domain, mobile-friendly. Considerations: token size, can't revoke easily, store securely on client, use short expiration with refresh tokens.

### 37. What is the structure of a JWT and how is it verified?
**Answer:** JWT has three Base64URL-encoded parts separated by dots: **Header** (algorithm and type), **Payload** (claims: user data, expiration, issuer), **Signature** (HMAC or RSA signature of header+payload). Verification: decode header, use algorithm and secret/public key to verify signature matches header+payload. Check expiration (exp), issuer (iss), audience (aud) claims. Never trust unsigned tokens.

### 38. What are the security concerns with JWT and how do you address them?
**Answer:** Concerns: **Token theft** (use HTTPS, HttpOnly cookies, short expiration), **XSS attacks** (sanitize input, CSP headers), **Algorithm confusion** (explicitly verify algorithm, reject 'none'), **Token size** (keep payload minimal), **Can't revoke** (use short-lived access tokens with refresh tokens, maintain token blacklist for critical scenarios), **Sensitive data exposure** (never store passwords/secrets in payload).

### 39. How do refresh tokens work with JWT?
**Answer:** Access tokens are short-lived (15 mins - 1 hour), refresh tokens are long-lived (days/weeks) and stored securely. When access token expires, client sends refresh token to get new access token without re-authentication. Refresh tokens can be revoked (stored in DB), enabling logout. Use rotating refresh tokens for enhanced security - issue new refresh token with each refresh, invalidate old one.

### 40. What are alternatives to JWT for authentication?
**Answer:** **Session-based auth**: server stores session, client has session ID in cookie - revocable, smaller payload, but not stateless. **Opaque tokens**: random strings, server validates against database - revocable, more secure. **PASETO** (Platform-Agnostic Security Tokens): JWT alternative with better security defaults, versioned, no algorithm confusion. **Macaroons**: attenuated tokens with contextual caveats. **OAuth tokens**: for third-party access.

### 41. When should you use JWT vs session-based authentication?
**Answer:** **Use JWT for**: microservices, mobile apps, cross-domain scenarios, stateless systems, third-party API access, when scalability without shared state is critical. **Use sessions for**: traditional web apps, when revocation is frequent, smaller token size needed, simpler security model. **Hybrid**: sessions with JWT claims. Consider: infrastructure, revocation needs, token size, security requirements, team expertise.

### 42. What is JWE (JSON Web Encryption) and when to use it?
**Answer:** JWE encrypts JWT payload for confidentiality, unlike JWS (JSON Web Signature) which only signs. Structure: Protected Header, Encrypted Key, Initialization Vector, Ciphertext, Authentication Tag. Use when: transmitting sensitive data in token, compliance requires encryption, public channels, untrusted intermediaries. More complex than JWS, larger tokens. Most JWTs use JWS; add JWE only when encryption is necessary.

### 43. How do you implement token revocation with JWT?
**Answer:** Strategies: **Token blacklist**: store revoked tokens in Redis with TTL matching token expiration. **Short expiration**: 5-15 min tokens reduce revocation window. **Token versioning**: increment user's token version in DB, include in JWT, validate on each request. **Refresh token rotation**: revoke refresh tokens in DB. **Event-driven**: pub/sub to notify services of revocations. Trade-off: adds state, but enables true revocation.

### 44. What is the 'none' algorithm vulnerability in JWT?
**Answer:** Critical vulnerability where attacker sets algorithm to 'none' in header, removes signature, and server accepts unsigned token if not properly validated. Prevention: **Always explicitly specify and verify algorithm**, reject 'none' algorithm, use strong signature algorithms (RS256, ES256 over HS256 when possible), validate algorithm matches expected, use well-tested JWT libraries with secure defaults. Part of CVE-2015-9235 and similar vulnerabilities.

### 45. What are JWT claims and which are standard?
**Answer:** Claims are statements about an entity. **Registered claims**: iss (issuer), sub (subject/user ID), aud (audience), exp (expiration), nbf (not before), iat (issued at), jti (JWT ID for preventing replay). **Public claims**: defined in IANA registry. **Private claims**: custom application-specific. Best practices: include exp always, validate iss/aud for security, use sub for user identification, keep payload minimal, don't duplicate info available elsewhere.

### 46. What is SQL injection and how do you prevent it?
**Answer:** SQL injection exploits unsanitized user input to execute malicious SQL. Prevention: use parameterized queries/prepared statements, ORM frameworks, input validation, principle of least privilege for DB users, and never concatenate user input into queries.

### 47. Explain CORS and how to handle it?
**Answer:** Cross-Origin Resource Sharing (CORS) is a security mechanism that restricts web pages from making requests to different domains. Handle by: configuring proper headers (Access-Control-Allow-Origin), specifying allowed methods/headers, handling preflight OPTIONS requests, and using credentials carefully.

### 48. What is HTTPS and how does it work?
**Answer:** HTTPS encrypts HTTP traffic using SSL/TLS. Process: client initiates handshake, server sends certificate, client verifies certificate, asymmetric encryption establishes session key, symmetric encryption for data transfer. Provides confidentiality, integrity, and authentication.

### 49. How do you securely store passwords?
**Answer:** Never store plaintext passwords. Use strong hashing algorithms (bcrypt, Argon2, PBKDF2) with salt. Hashing is one-way, salts prevent rainbow table attacks, and adaptive algorithms resist brute force. Avoid MD5/SHA1 for passwords.

### 50. What is XSS (Cross-Site Scripting) and how do you prevent it?
**Answer:** XSS injects malicious scripts into web pages viewed by users. Types: Stored (persisted in DB), Reflected (in URL/input), DOM-based (client-side). Prevention: escape output, use Content Security Policy (CSP), validate input, sanitize HTML, use frameworks with auto-escaping (React, Angular), HttpOnly cookies, and avoid innerHTML with user data.

### 51. Explain CSRF (Cross-Site Request Forgery) and mitigation strategies.
**Answer:** CSRF tricks authenticated users into performing unwanted actions. Attacker crafts malicious request using victim's credentials. Prevention: CSRF tokens (synchronizer token pattern), SameSite cookie attribute, verify Origin/Referer headers, require re-authentication for sensitive actions, use custom headers for AJAX. Token-based auth (JWT) is naturally resistant.

### 52. What is the OWASP Top 10 and why is it important?
**Answer:** OWASP Top 10 lists critical web application security risks: Broken Access Control, Cryptographic Failures, Injection, Insecure Design, Security Misconfiguration, Vulnerable Components, Authentication Failures, Data Integrity Failures, Logging Failures, SSRF. It's a standard awareness document guiding secure development practices and security testing priorities.

### 53. Explain the principle of least privilege and its importance.
**Answer:** Users, processes, and systems should have minimum access rights necessary to perform their functions. Benefits: limits damage from accidents/attacks, reduces attack surface, contains breaches. Apply to: database users (read-only when possible), API permissions, file system access, network access, service accounts. Implement with role-based access control (RBAC).

### 54. What is OAuth 2.0 and how does it work?
**Answer:** OAuth 2.0 is authorization framework for delegated access. Roles: Resource Owner (user), Client (app), Authorization Server (issues tokens), Resource Server (API). Flows: Authorization Code (web apps), Implicit (deprecated), Client Credentials (machine-to-machine), PKCE (mobile/SPA). Provides access tokens without sharing passwords. Different from authentication (use OpenID Connect for that).

### 55. How do you prevent and detect brute force attacks?
**Answer:** Prevention: rate limiting, account lockout after failed attempts, CAPTCHA, progressive delays, multi-factor authentication (MFA), strong password policies, monitor suspicious patterns. Detection: log failed attempts, anomaly detection, geographic analysis, velocity checks. Use tools like Fail2Ban, implement alerts for unusual activity patterns.

### 56. What is encryption at rest vs encryption in transit?
**Answer:** **At rest**: encrypts stored data (databases, files, backups) protecting against physical theft or unauthorized access. Use AES-256, database encryption (TDE), full-disk encryption. **In transit**: encrypts data moving between systems (HTTPS, TLS, VPN) protecting against interception. Both are essential; compliance standards (PCI-DSS, HIPAA, GDPR) often require both.

### 57. Explain security headers and their purposes.
**Answer:** Security headers protect against attacks: **Strict-Transport-Security** (force HTTPS), **X-Content-Type-Options** (prevent MIME sniffing), **X-Frame-Options** (prevent clickjacking), **Content-Security-Policy** (XSS protection), **X-XSS-Protection** (legacy XSS filter), **Referrer-Policy** (control referrer info), **Permissions-Policy** (feature control). Implement via server config or middleware (Helmet for Node.js).

### 58. What is API key security and best practices?
**Answer:** API keys authenticate applications accessing APIs. Best practices: never commit to version control, use environment variables, rotate regularly, implement key expiration, scope permissions (read-only when possible), use separate keys for dev/prod, monitor usage, implement rate limiting, consider OAuth for user-delegated access, use HTTPS only, hash keys in database.

### 59. How do you handle sensitive data in logs and error messages?
**Answer:** Never log sensitive data: passwords, API keys, tokens, PII, credit cards, SSNs. Sanitize before logging, redact sensitive fields, use structured logging with field-level control. In errors: show generic messages to users, log detailed info server-side, avoid stack traces in production, implement log access controls, use log retention policies, encrypt logs containing any sensitive context.

## System Design & Patterns

### 60. What is the purpose of message queues?
**Answer:** Message queues enable asynchronous communication between services. Benefits: decoupling, load leveling, reliability (retries), scalability, buffering during spikes. Common tools: RabbitMQ, Apache Kafka, AWS SQS. Use for background jobs, event-driven architectures, and distributed systems.

### 61. Explain the Circuit Breaker pattern.
**Answer:** Circuit Breaker prevents cascading failures by monitoring for failures and "opening" (stopping calls) when threshold is exceeded. States: Closed (normal), Open (blocking calls), Half-Open (testing recovery). Protects systems from repeated failures and allows recovery time.

### 62. What is event-driven architecture?
**Answer:** EDA uses events to trigger and communicate between services. Components: event producers, event routers/brokers, event consumers. Benefits: loose coupling, scalability, real-time processing, audit trail. Patterns include pub/sub, event sourcing, and CQRS.

### 63. Explain the difference between synchronous and asynchronous communication.
**Answer:** Synchronous: caller waits for response (blocking), tighter coupling, simpler but can cause delays. Asynchronous: caller continues without waiting (non-blocking), better performance, loose coupling, but adds complexity in error handling and eventual consistency.

### 64. What is API versioning and what strategies exist?
**Answer:** API versioning manages changes without breaking clients. Strategies: URI versioning (/v1/users), header versioning (Accept: application/vnd.api+json;version=1), query parameter (?version=1), content negotiation. Consider backward compatibility, deprecation policies, and documentation.

## DevOps & Infrastructure

### 65. What is the difference between Docker containers and virtual machines?
**Answer:** Containers share the host OS kernel, are lightweight, start quickly, and use fewer resources. VMs include full OS, are isolated at hardware level, heavier, slower to start. Containers are ideal for microservices, VMs for different OS requirements or stronger isolation.

### 66. Explain CI/CD and its benefits.
**Answer:** Continuous Integration (CI) automatically builds and tests code changes. Continuous Deployment (CD) automatically deploys to production. Benefits: faster releases, early bug detection, reduced risk, automated testing, consistent deployments. Tools: Jenkins, GitLab CI, GitHub Actions, CircleCI.

### 67. What is the purpose of health checks in services?
**Answer:** Health checks monitor service availability and readiness. Types: liveness (is service running), readiness (can it handle traffic), startup (has initialization completed). Used by load balancers, orchestrators (Kubernetes), and monitoring systems for automatic recovery.

### 68. Explain the concept of Infrastructure as Code (IaC).
**Answer:** IaC manages infrastructure through code instead of manual processes. Benefits: version control, repeatability, consistency, automation, documentation. Tools: Terraform, CloudFormation, Ansible, Pulumi. Enables infrastructure versioning, testing, and quick disaster recovery.

### 69. What is observability and what are its three pillars?
**Answer:** Observability is understanding system internal state from external outputs. Three pillars: Logs (discrete events), Metrics (numerical measurements over time), Traces (request journey through system). Together they enable debugging, performance optimization, and issue detection. Tools: Prometheus, Grafana, ELK Stack, Jaeger.

## Version Control (Git)

### 70. What is Git and how does it differ from other version control systems?
**Answer:** Git is a distributed version control system where each developer has a complete repository copy with full history. Unlike centralized systems (SVN), Git enables offline work, faster operations, better branching/merging, and distributed backups. Key concepts: commits, branches, remotes, staging area. Git tracks snapshots, not differences, making operations efficient.

### 71. Explain the difference between git pull and git fetch.
**Answer:** git fetch downloads changes from remote but doesn't merge them into your working branch. git pull is fetch + merge in one command (git pull = git fetch + git merge). Use fetch when you want to review changes before integrating, pull for quick updates. fetch is safer as it doesn't modify working directory automatically.

### 72. What is the difference between git merge and git rebase?
**Answer:** git merge creates a merge commit combining two branches, preserving history. git rebase moves commits to a new base, rewriting history for linear progression. Merge: safe, keeps complete history, creates merge commits. Rebase: cleaner history, no merge commits, but rewrites commits (use only on local branches). Never rebase shared/public branches.

### 73. Explain the Git branching strategy (Git Flow, GitHub Flow, Trunk-Based).
**Answer:** **Git Flow**: main, develop, feature, release, hotfix branches - complex but structured. **GitHub Flow**: main branch with feature branches, simpler, continuous deployment. **Trunk-Based**: frequent commits to main with short-lived branches - fastest integration. Choose based on team size, release frequency, and deployment process.

### 74. What is a Git conflict and how do you resolve it?
**Answer:** Conflicts occur when same lines are modified in different branches. Git marks conflicts with <<<<<<<, =======, >>>>>>>. Resolve by: editing files to keep desired changes, removing conflict markers, staging resolved files (git add), and completing merge/rebase. Use git status to track progress. Tools like VSCode, GitKraken help visualize conflicts.

### 75. What is the difference between git reset, git revert, and git checkout?
**Answer:** git reset moves HEAD and optionally modifies staging/working directory (--soft, --mixed, --hard) - rewrites history. git revert creates new commit undoing changes - preserves history, safe for shared branches. git checkout switches branches or restores files. Use revert for shared branches, reset for local changes.

### 76. Explain Git tags and when to use them.
**Answer:** Tags mark specific points in history, typically releases. Two types: lightweight (pointer to commit) and annotated (full objects with metadata, message, tagger). Create with git tag v1.0.0 or git tag -a v1.0.0 -m "Release 1.0". Push with git push --tags. Use for versioning, marking stable releases, deployment points.

### 77. What is git stash and when would you use it?
**Answer:** git stash temporarily saves uncommitted changes, cleaning working directory. Use when: switching branches with uncommitted work, pulling updates, testing clean state. Commands: git stash (save), git stash pop (apply and remove), git stash list, git stash apply. Can stash specific files, create named stashes, and stash untracked files with -u.

### 78. How do you undo the last commit in Git?
**Answer:** Multiple approaches: git reset --soft HEAD~ (keep changes staged), git reset --mixed HEAD~ (unstage changes), git reset --hard HEAD~ (discard changes). For pushed commits: git revert HEAD (creates new commit). Use --amend to modify last commit message/content. Choose based on whether commit is local or pushed, and if you want to preserve changes.

### 79. Explain Git hooks and provide use cases.
**Answer:** Git hooks are scripts triggered by Git events. Client-side: pre-commit (linting, tests), commit-msg (message validation), pre-push (run tests). Server-side: pre-receive, post-receive (CI/CD triggers). Located in .git/hooks/. Use for: enforcing code quality, running tests, validating commits, triggering deployments. Tools like Husky simplify hook management.
