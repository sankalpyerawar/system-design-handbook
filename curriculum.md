# 5-Week Staff-Level System Design Curriculum

## Overview

This curriculum transforms a senior backend engineer (7+ years experience) into a staff-level architect capable of designing complete production systems. The focus is on **architectural maturity**: making justified decisions through trade-off analysis, quantitative reasoning, and understanding organizational impact.

### Time Commitment
- **Duration:** 5 weeks
- **Daily Effort:** 4-5 hours
- **Split:** 70% High-Level Design (HLD) / 30% Low-Level Design (LLD)

### Learning Philosophy
Staff-level thinking means:
- Justifying every decision with trade-offs (latency, cost, complexity, availability)
- Using math to validate choices (back-of-the-envelope calculations)
- Understanding when NOT to use distributed patterns
- Considering team structure, operational burden, and migration paths

---

## Week 1: Foundations + Object-Oriented Design

### Learning Objectives
- Master scalability fundamentals, CAP theorem, and caching strategies
- Apply SOLID principles and create maintainable class designs
- Perform back-of-the-envelope capacity calculations
- Understand trade-offs quantitatively (not just conceptually)

### Core Concepts

#### High-Level Design (3 hours/day)
**Scalability Fundamentals**
- Client-server architecture and separation of concerns
- Horizontal vs vertical scaling (with cost/latency implications)
- Load balancing algorithms and session affinity trade-offs
- Caching layers (client, CDN, application, database)
- CAP theorem: understanding CP vs AP systems in practice

**Back-of-the-Envelope Math**
- Standard conversion factors (1M users = ? QPS, storage = ?)
- Latency numbers every engineer should know
- Bandwidth, storage, and memory calculations
- Rule-of-thumb cost estimates (AWS/GCP pricing models)

#### Low-Level Design (1.5 hours/day)
**SOLID Principles**
- Single Responsibility: one reason to change
- Open/Closed: extend without modification
- Liskov Substitution: subtype behavior guarantees
- Interface Segregation: small, focused interfaces
- Dependency Inversion: depend on abstractions

**Class Design Basics**
- Abstraction vs encapsulation
- Composition over inheritance (when and why)
- Designing for testability and extensibility

### Resources

#### HLD Resources
- **[Client-Server Architecture](https://www.geeksforgeeks.org/system-design/client-server-architecture-system-design/)** - GeeksforGeeks overview
- **[Horizontal vs Vertical Scaling](https://www.geeksforgeeks.org/system-design/system-design-horizontal-and-vertical-scaling/)** - with trade-off discussion
- **[Caching Fundamentals](https://auth0.com/blog/what-is-caching-and-how-it-works/)** - Auth0 deep dive
- **[Load Balancing Strategies](https://dev.to/iaadidev/load-balancer-ensuring-high-availability-and-scalability-npg)** - algorithms and use cases
- **[CAP Theorem Explained](https://www.ibm.com/think/topics/cap-theorem)** - IBM practical guide
- **[Latency Numbers Every Programmer Should Know](https://gist.github.com/jboner/2841832)** - GitHub Gist (essential reference)
- **Book: [Designing Data-Intensive Applications](https://www.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/)** - Chapter 1 (Reliable, Scalable, Maintainable Applications)

#### LLD Resources
- **[SOLID Principles Explained](https://www.digitalocean.com/community/conceptual-articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design)** - DigitalOcean tutorial
- **[Uncle Bob's SOLID Principles](https://www.youtube.com/watch?v=zHiWqnTWsn4)** - YouTube (60 min)
- **Book: [Head First Design Patterns](https://www.oreilly.com/library/view/head-first-design/9781492077992/)** - Chapter 1 (Intro to Patterns)
- **[Refactoring Guru: SOLID](https://refactoring.guru/design-patterns/principles)** - visual examples

### Practical Exercises

#### HLD Exercises
1. **Capacity Calculation Practice:**
   - Given: 10M daily active users, each posts 3 times/day
   - Calculate: Peak QPS, daily storage (text + images), network bandwidth
   - Estimate: Monthly AWS S3 + EC2 costs

2. **Caching Strategy Design:**
   - Add Redis to a web app; measure cache hit ratio
   - Implement cache-aside pattern in code
   - Analyze: What happens when cache fails? (Thundering herd problem)

3. **Load Balancer Comparison:**
   - Deploy Nginx with round-robin vs least-connections
   - Generate traffic with uneven backend response times
   - Measure: Which algorithm performs better and why?

#### LLD Exercises
1. **SOLID Violation Fixes:**
   - Refactor a God class that handles user auth, email, and logging
   - Apply SRP to separate concerns into cohesive classes

2. **Design a Logger System:**
   - Requirements: log to file, console, remote server
   - Use DIP: depend on ILogger interface, not concrete implementations
   - Make it extensible for new log destinations without modifying existing code

### Problem Sets

#### Trade-off Analysis
**Problem 1: Scaling Decision**
- Scenario: Web server at 80% CPU, response time degrading
- Options: (A) Vertical scale to larger instance, (B) Horizontal scale with load balancer
- **Your task:** Calculate cost, downtime, and failure impact for each option
- **Deliverable:** Decision matrix with quantified trade-offs

**Problem 2: Caching Layer Justification**
- System: E-commerce product catalog (100K products, 1K updates/hour)
- Question: Should you add Redis? Where (app-level or CDN)?
- **Analysis required:**
  - Calculate cache hit ratio needed to justify cost
  - TTL strategy for stale data tolerance
  - Failure mode: what breaks if cache is down?

#### Back-of-the-Envelope Quiz
1. Convert: 500 requests/second to daily total requests
2. Storage: 10M users, 100KB profile data each = ? TB
3. Bandwidth: Streaming 5Mbps video to 10K concurrent users = ? Gbps

#### LLD Problem
**Design a Rate Limiter Class**
- Algorithms: Fixed window, sliding window, token bucket
- Interface: `boolean allowRequest(String userId)`
- Requirements: Thread-safe, configurable limits, unit testable
- Constraint: No external dependencies (in-memory only)

### Daily Schedule

| Day | HLD Focus (3h) | LLD Focus (1.5h) | Deliverable |
|-----|----------------|------------------|-------------|
| Mon | Read client-server, scaling concepts; watch latency numbers video | Study SRP and OCP with examples | Write summary: "When to scale vertically vs horizontally" |
| Tue | Implement toy Redis cache; measure latency improvement | Refactor a God class using SOLID | Code: Cache-aside pattern + violation fixes |
| Wed | Load balancer algorithms; Nginx hands-on with traffic simulation | LSP and ISP: design interface hierarchies | Comparison doc: LB algorithms with test results |
| Thu | CAP theorem deep dive; read DDIA Ch1; analyze CP vs AP systems | DIP: refactor to depend on abstractions | Quiz: CAP scenarios + dependency injection exercise |
| Fri | **Design problem:** URL shortener (full design with capacity math) | Design rate limiter class (multiple algorithms) | Complete design doc + code implementation |

### Trade-off Framework (Use for Every Design)

```
Decision: [What you chose]
Alternatives: [What you rejected]

Trade-offs:
├─ Latency: [p50, p99 impact with numbers]
├─ Availability: [failure scenarios, SLA impact]
├─ Scalability: [at what load does this break?]
├─ Cost: [monthly $ estimate]
├─ Complexity: [operational burden, on-call risk]
└─ Consistency: [data guarantees]

Math: [Back-of-envelope calculations supporting decision]
```

---

## Week 2: Data Storage + Creational Design Patterns

### Learning Objectives
- Compare SQL vs NoSQL with quantified trade-offs
- Master replication and partitioning (sharding) strategies
- Understand consistency models and their cost implications
- Apply creational design patterns (Factory, Builder, Singleton, Prototype)

### Core Concepts

#### High-Level Design (3 hours/day)
**Data Storage Decisions**
- SQL vs NoSQL: ACID vs BASE with real-world scenarios
- When each fails: SQL scaling limits, NoSQL consistency challenges
- Indexing strategies and query performance impact (B-trees vs LSM-trees)

**Replication Strategies**
- Master-slave vs multi-master replication
- Synchronous vs asynchronous replication (latency vs consistency)
- Read replicas: when they help and when they don't
- Failover mechanisms and split-brain scenarios

**Partitioning (Sharding)**
- Horizontal partitioning strategies: range, hash, consistent hashing
- When to shard: cost-benefit analysis (complexity vs performance)
- Shard key selection and hotspot avoidance
- Cross-shard queries and distributed transactions

**Consistency Models**
- Strong consistency: linearizability and its cost
- Eventual consistency: conflict resolution strategies
- Causal consistency and happens-before relationships
- Read-your-writes, monotonic reads guarantees

#### Low-Level Design (1.5 hours/day)
**Creational Patterns**
- **Factory Method:** Creating objects without specifying exact class
- **Abstract Factory:** Families of related objects
- **Builder:** Complex object construction with fluent interfaces
- **Singleton:** Controlled single instance (and why it's often an anti-pattern)
- **Prototype:** Cloning objects for efficiency

### Resources

#### HLD Resources
- **[SQL vs NoSQL Decision Matrix](https://www.geeksforgeeks.org/system-design/which-database-to-choose-while-designing-a-system-sql-or-nosql/)** - GeeksforGeeks
- **[Database Replication Patterns](https://www.geeksforgeeks.org/system-design/database-replication-and-their-types-in-system-design/)** - with failure scenarios
- **[Sharding vs Replication](https://www.geeksforgeeks.org/system-design/difference-between-database-sharding-and-replication/)** - trade-off analysis
- **[Consistency Models Glossary](https://www.scylladb.com/glossary/consistency-models/)** - ScyllaDB visual guide
- **[Eventual Consistency Patterns](https://bytebytego.com/guides/top-eventual-consistency-patterns-you-must-know/)** - ByteByteGo
- **Book: DDIA Chapter 5** - Replication (master-slave, multi-master, leaderless)
- **Book: DDIA Chapter 6** - Partitioning (sharding strategies, secondary indexes)
- **[Consistent Hashing Explained](https://www.youtube.com/watch?v=UF9Iqmg94tk)** - Gaurav Sen YouTube (20 min)

#### LLD Resources
- **[Creational Design Patterns Overview](https://refactoring.guru/design-patterns/creational-patterns)** - Refactoring Guru
- **[Factory Pattern Deep Dive](https://www.youtube.com/watch?v=EcFVTgRHJLM)** - Christopher Okhravi YouTube (20 min)
- **[Builder Pattern Tutorial](https://refactoring.guru/design-patterns/builder)** - with Java/Python examples
- **Book: Head First Design Patterns** - Chapters 4-5 (Factory, Singleton)
- **[Why Singleton is Controversial](https://stackoverflow.com/questions/137975/what-are-drawbacks-or-disadvantages-of-singleton-pattern)** - Stack Overflow discussion

### Practical Exercises

#### HLD Exercises
1. **Replication Failover Test:**
   - Set up MySQL master-slave replication
   - Kill master and observe failover behavior
   - Measure: replication lag, data loss on failover
   - Document: How to minimize downtime and data loss

2. **Sharding Implementation:**
   - Dataset: 10M user records with userId (1-10M)
   - Implement: Hash-based sharding across 4 nodes
   - Test: Query performance for single-shard vs cross-shard queries
   - Calculate: At what data size does sharding pay off?

3. **Consistency Model Exploration:**
   - Deploy a distributed key-value store (e.g., Cassandra)
   - Configure: Quorum reads/writes (R=2, W=2, N=3)
   - Test: Write conflicts and last-write-wins resolution
   - Compare: Strong (R+W>N) vs eventual consistency performance

#### LLD Exercises
1. **Factory Pattern Implementation:**
   - Design a document parser factory (PDF, DOCX, TXT)
   - Use factory method to instantiate correct parser based on file type
   - Extend: Add new parser without modifying existing code

2. **Builder Pattern for Complex Objects:**
   - Create a DatabaseConnection builder with 10+ optional parameters
   - Implement: Fluent interface for readable construction
   - Handle: Required vs optional fields with compile-time safety

### Problem Sets

#### Trade-off Analysis
**Problem 1: SQL vs NoSQL Decision**
- System: Social media user profiles (1B users, high read:write ratio 100:1)
- Data: Structured fields + flexible metadata
- **Deliverable:** Decision matrix comparing PostgreSQL vs MongoDB
  - Query patterns and performance implications
  - Scaling costs (vertical for SQL vs horizontal for NoSQL)
  - Consistency requirements and impact on UX
  - Operational complexity and team expertise

**Problem 2: Sharding Strategy**
- Service: URL shortener with 100M URLs, 1K writes/sec, 10K reads/sec
- Question: Should you shard? If yes, what's the shard key?
- **Analysis:**
  - Calculate: Single DB limit (assume 10K QPS max)
  - Shard key options: Hash(URL), Range(timestamp), Hash(userId)
  - Evaluate: Hotspots, cross-shard queries, rebalancing cost
  - Cost estimate: Single large DB vs 10 small shards

**Problem 3: Replication Configuration**
- System: Financial transaction log (strict consistency required)
- Requirements: No data loss, read scalability
- **Design:**
  - Synchronous vs asynchronous replication trade-offs
  - Failover strategy with zero data loss
  - Performance impact of sync replication (latency numbers)
  - Cost: Additional replicas vs increased write latency

#### Capacity Estimation
Given a Twitter-like service:
- 500M DAU, each user reads 100 tweets/day, posts 1 tweet/day
- Tweet: 280 chars text + optional 2MB image (30% have images)
- Calculate:
  1. Read QPS (peak = 3x average)
  2. Write QPS (peak = 5x average)
  3. Daily storage requirement (30-day retention)
  4. Network bandwidth for media serving
  5. Number of DB shards needed (assume 5K writes/sec per shard)

#### LLD Problem
**Design a Database Connection Pool**
- Requirements:
  - Factory pattern for creating connections
  - Builder pattern for pool configuration (size, timeout, validation)
  - Thread-safe pool operations
  - Connection health checks and recycling
- Deliverable: Class diagram + core interfaces

### Daily Schedule

| Day | HLD Focus (3h) | LLD Focus (1.5h) | Deliverable |
|-----|----------------|------------------|-------------|
| Mon | SQL vs NoSQL comparison; read DDIA Ch5 (replication types) | Factory Method pattern: theory + implementation | Trade-off doc: SQL vs NoSQL with numbers |
| Tue | Set up MySQL replication; test failover scenarios | Abstract Factory for cross-platform UI components | Replication test results + failure analysis |
| Wed | Sharding strategies; consistent hashing deep dive | Builder pattern for complex object construction | Sharding design doc with shard key justification |
| Thu | Consistency models; CAP theorem applied to databases | Singleton pattern (and when NOT to use it) | Quiz: Consistency scenarios + Singleton critique |
| Fri | **Design problem:** Instagram-like photo storage system | Design connection pool (Factory + Builder) | Full design with capacity math + LLD class diagram |

### When NOT to Shard (Critical Thinking)
Sharding adds complexity. Avoid if:
- Data fits on single DB with vertical scaling (< 5TB, < 10K writes/sec)
- Team lacks operational expertise (debugging distributed queries)
- Query patterns require frequent joins across shards
- Alternative: read replicas + caching solve read scaling

**Cost example:** Single PostgreSQL on AWS db.r5.12xlarge ($6K/month) vs 10 sharded instances with operational overhead ($12K/month + 2 engineer-weeks/quarter for maintenance)

---

## Week 3: Distributed Systems + Behavioral Patterns

### Learning Objectives
- Design microservices architectures with justified decomposition
- Master asynchronous messaging and event-driven patterns
- Understand distributed consensus and coordination challenges
- Apply behavioral design patterns (Strategy, Observer, Command, State)

### Core Concepts

#### High-Level Design (3 hours/day)
**Microservices Architecture**
- Monolith vs microservices: when to NOT use microservices
- Service decomposition strategies: by business capability, subdomain
- Data ownership and bounded contexts
- Inter-service communication: sync (REST, gRPC) vs async (events)
- Distributed tracing and observability requirements

**Event-Driven Architecture**
- Message brokers: Kafka, RabbitMQ, AWS SQS/SNS comparison
- Pub/sub vs point-to-point messaging
- Event sourcing and CQRS patterns
- Idempotency and exactly-once delivery challenges
- Saga pattern for distributed transactions

**Distributed Coordination**
- Leader election and consensus (Raft, Paxos concepts)
- Service discovery and health checks
- Distributed locking and coordination (ZooKeeper, etcd)
- Split-brain scenarios and quorum requirements

**Consistency in Microservices**
- Eventual consistency patterns (async replication, event propagation)
- Causal ordering and happened-before relationships
- Conflict resolution strategies (LWW, CRDT, manual)

#### Low-Level Design (1.5 hours/day)
**Behavioral Patterns**
- **Strategy:** Encapsulating algorithms for runtime selection
- **Observer:** Event notification and pub/sub at object level
- **Command:** Encapsulating requests as objects (undo/redo)
- **State:** Object behavior changes based on internal state
- **Chain of Responsibility:** Passing requests through handler chain
- **Template Method:** Defining algorithm skeleton with customizable steps

### Resources

#### HLD Resources
- **[Microservices Patterns Overview](https://www.designgurus.io/answers/detail/key-system-design-patterns-for-tech-interview)** - DesignGurus
- **[When NOT to Use Microservices](https://martinfowler.com/articles/microservice-trade-offs.html)** - Martin Fowler critical analysis
- **[Event-Driven Architecture](https://www.youtube.com/watch?v=STKCRSUsyP0)** - IBM Technology YouTube (15 min)
- **[Asynchronous Messaging Patterns](https://www.cloudamqp.com/blog/how-to-implement-asynchronous-messaging-in-distributed-systems.html)** - CloudAMQP
- **[Saga Pattern Explained](https://microservices.io/patterns/data/saga.html)** - Chris Richardson
- **[Kafka Architecture Deep Dive](https://www.youtube.com/watch?v=UNUz1-msbOM)** - Confluent YouTube (30 min)
- **Book: DDIA Chapter 7** - Transactions (distributed transactions, consensus)
- **Book: DDIA Chapter 9** - Consistency and Consensus
- **[Netflix Microservices Case Study](https://netflixtechblog.com/tagged/microservices)** - Netflix Tech Blog

#### LLD Resources
- **[Behavioral Patterns Overview](https://refactoring.guru/design-patterns/behavioral-patterns)** - Refactoring Guru
- **[Strategy Pattern Tutorial](https://www.youtube.com/watch?v=v9ejT8FO-7I)** - Christopher Okhravi YouTube (18 min)
- **[Observer Pattern Explained](https://refactoring.guru/design-patterns/observer)** - with real-world examples
- **Book: Head First Design Patterns** - Chapters 1-2 (Strategy, Observer)
- **[Command Pattern for Undo/Redo](https://sourcemaking.com/design_patterns/command)** - SourceMaking

### Practical Exercises

#### HLD Exercises
1. **Microservices Decomposition:**
   - Start with a monolithic e-commerce app (User, Product, Order, Payment)
   - Decompose into 4 services with clear boundaries
   - Design: API contracts, data ownership, async event flows
   - Analyze: How many network calls for "place order" operation?
   - Cost: Latency increase, operational complexity, infrastructure cost

2. **Event-Driven System Implementation:**
   - Build: Order service publishes `OrderPlaced` event to Kafka
   - Consumers: Inventory service, Email service, Analytics service
   - Test: What happens if Email service is down? (Dead letter queue)
   - Measure: End-to-end latency vs synchronous REST calls

3. **Saga Pattern for Distributed Transaction:**
   - Scenario: Book flight + hotel + car rental (3 services)
   - Implement: Choreography-based saga with compensating transactions
   - Failure case: Flight booked, hotel fails → trigger flight cancellation
   - Compare: Saga vs 2-phase commit (latency, complexity, partial failures)

#### LLD Exercises
1. **Strategy Pattern for Payment Processing:**
   - Design: PaymentProcessor with multiple strategies (CreditCard, PayPal, Crypto)
   - Each strategy implements: `validate()`, `authorize()`, `capture()`
   - Runtime selection based on user preference
   - Extend: Add new payment method without changing existing code

2. **Observer Pattern for Event System:**
   - Scenario: Stock price updates notify multiple observers (UI, Alert, Logger)
   - Implement: Subject (Stock) and Observer interface
   - Support: Dynamic subscribe/unsubscribe at runtime
   - Thread-safety considerations for concurrent notifications

### Problem Sets

#### Trade-off Analysis
**Problem 1: Microservices vs Monolith**
- System: Medium-sized SaaS (20 engineers, 100K users)
- Current: Django monolith on 4 servers, response time 200ms p99
- Proposal: Split into 8 microservices
- **Your task:**
  - Quantify: Latency impact (assume 3 service calls per request, 10ms overhead each)
  - Operational cost: Additional infrastructure, monitoring, coordination
  - Team velocity: Deployment independence vs debugging complexity
  - Recommendation: Stay monolith or migrate? Justify with numbers and team factors

**Problem 2: Sync vs Async Communication**
- Service: Payment processing between Order → Payment → Inventory
- Options: (A) Synchronous REST calls, (B) Async Kafka events
- **Analysis:**
  - Latency: Best case and failure case for each approach
  - Consistency: How eventual is eventual? Business impact of delay
  - Failure handling: Retry storms, timeouts, circuit breakers
  - Cost: Kafka infrastructure vs simpler REST (but tighter coupling)
  - **Recommendation with trade-off matrix**

**Problem 3: Message Broker Selection**
- Compare: Kafka vs RabbitMQ vs AWS SQS
- Criteria: Throughput (1M msgs/sec), ordering guarantees, operational complexity
- **Deliverable:** Decision matrix
  - Performance benchmarks (messages/sec, latency)
  - Ordering: Kafka (partition-level) vs SQS (FIFO limited to 300 TPS)
  - Cost: Self-hosted Kafka vs managed SQS
  - When each is appropriate (Kafka for event streaming, SQS for job queues)

#### Capacity Estimation
Design an **Uber-like ride-sharing system:**
- 50M active riders, 5M drivers, 10M rides/day
- Each ride: location updates every 5 sec (during trip), avg trip = 20 min
- Calculate:
  1. Location update write QPS
  2. Real-time map tracking read QPS (assume 10:1 read:write)
  3. Historical trip data storage (1KB per trip, 1 year retention)
  4. WebSocket connections for live tracking
  5. Kafka partition strategy for location events (shard by driverId? geohash?)

#### LLD Problem
**Design a Workflow Engine**
- Requirements: Execute tasks in sequence, support conditional branching, error handling
- Patterns to use:
  - Command: Each task as a command object
  - Chain of Responsibility: Task execution pipeline
  - State: Workflow state machine (Pending → Running → Completed/Failed)
- Deliverable: Class diagram with pattern annotations

### Daily Schedule

| Day | HLD Focus (3h) | LLD Focus (1.5h) | Deliverable |
|-----|----------------|------------------|-------------|
| Mon | Microservices architecture; read Martin Fowler's trade-offs | Strategy pattern: multiple algorithm implementations | Decision doc: "When to use microservices" with anti-patterns |
| Tue | Event-driven design; set up Kafka producer-consumer | Observer pattern: pub/sub implementation | Kafka async system with latency measurements |
| Wed | Saga pattern for distributed transactions; compensating actions | Command pattern: undo/redo functionality | Saga implementation with failure scenarios tested |
| Thu | Service mesh and distributed tracing concepts (Istio, Jaeger) | State pattern: state machine design | Quiz: Microservices failure scenarios + State machine code |
| Fri | **Design problem:** Design WhatsApp-like messaging system | Design notification system (Observer + Strategy) | Full architecture + LLD for extensible notifications |

### Critical: When NOT to Use Microservices

Microservices are NOT appropriate when:
1. **Team size < 15 engineers:** Coordination overhead exceeds benefits
2. **Early-stage startup:** Product-market fit unknown, need rapid iteration
3. **Low traffic (< 100K DAU):** Monolith handles load with simpler operations
4. **Domain is unclear:** Premature decomposition leads to constant refactoring

**Migration path:** Start with modular monolith → Extract highest-value services first (e.g., payment processing) → Gradually decompose as team grows

**Cost reality check:** 
- Monolith: 4 servers, 1 DB, 1 cache instance = ~$2K/month
- Microservices (10 services): 20 service instances + 10 DBs + message broker + service mesh = ~$15K/month + 3x operational overhead

---

## Week 4: Infrastructure Patterns + Structural Patterns

### Learning Objectives
- Master infrastructure components (CDNs, API gateways, caching hierarchies)
- Design resilient systems (circuit breakers, rate limiting, bulkheads)
- Understand observability and operational complexity trade-offs
- Apply structural design patterns (Adapter, Decorator, Proxy, Facade)

### Core Concepts

#### High-Level Design (3 hours/day)
**Layered Architecture**
- N-tier separation (presentation, business logic, data)
- Benefits: Clear separation, team independence, technology flexibility
- Costs: Latency overhead, network chattiness, serialization tax

**API Gateway and Proxy Patterns**
- API Gateway responsibilities: routing, auth, rate limiting, aggregation
- Reverse proxy vs forward proxy use cases
- Backend for Frontend (BFF) pattern for client-specific APIs
- When gateways become bottlenecks

**Content Delivery Networks (CDNs)**
- Edge caching for static and dynamic content
- Cache invalidation strategies (TTL, purge, versioned URLs)
- Geographic distribution and latency reduction
- Cost analysis: CDN bandwidth vs origin bandwidth

**Caching Hierarchies**
- Multi-level caching: browser → CDN → app server → database
- Cache patterns: cache-aside, read-through, write-through, write-behind
- Cache eviction policies: LRU, LFU, TTL
- Thundering herd and cache warming strategies

**Resilience Patterns**
- Circuit breakers: fail fast instead of cascading failures
- Bulkheads: isolate resource pools to limit blast radius
- Rate limiting: token bucket, leaky bucket, sliding window
- Retry strategies: exponential backoff, jitter
- Timeouts and deadline propagation

**Observability**
- Metrics: RED (Rate, Errors, Duration), USE (Utilization, Saturation, Errors)
- Logging: structured logs, log levels, sampling
- Distributed tracing: correlation IDs, span propagation
- Cost of observability: storage, performance overhead

#### Low-Level Design (1.5 hours/day)
**Structural Patterns**
- **Adapter:** Converting interfaces for compatibility
- **Decorator:** Adding behavior dynamically without inheritance
- **Proxy:** Controlling access to objects (lazy loading, security, caching)
- **Facade:** Simplifying complex subsystem interactions
- **Bridge:** Separating abstraction from implementation
- **Composite:** Tree structures with uniform interface

### Resources

#### HLD Resources
- **[Layered Architecture Pattern](https://www.designgurus.io/answers/detail/key-system-design-patterns-for-tech-interview)** - DesignGurus analysis
- **[API Gateway Pattern](https://microservices.io/patterns/apigateway.html)** - Chris Richardson
- **[CDN Fundamentals](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/)** - Cloudflare Learning Center
- **[Caching Strategies](https://aws.amazon.com/caching/best-practices/)** - AWS best practices
- **[Circuit Breaker Pattern](https://martinfowler.com/bliki/CircuitBreaker.html)** - Martin Fowler
- **[Rate Limiting Algorithms](https://blog.bytebytego.com/p/rate-limiting-fundamentals)** - ByteByteGo visual guide
- **[Observability Engineering](https://www.oreilly.com/library/view/observability-engineering/9781492076438/)** - O'Reilly book
- **[The Three Pillars of Observability](https://www.oreilly.com/library/view/distributed-systems-observability/9781492033431/)** - O'Reilly
- **[AWS Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html)** - AWS documentation

#### LLD Resources
- **[Structural Patterns Overview](https://refactoring.guru/design-patterns/structural-patterns)** - Refactoring Guru
- **[Decorator Pattern Deep Dive](https://www.youtube.com/watch?v=GCraGHx6gso)** - Christopher Okhravi YouTube (25 min)
- **[Proxy vs Decorator](https://stackoverflow.com/questions/350404/how-do-the-proxy-decorator-adapter-and-bridge-patterns-differ)** - Stack Overflow comparison
- **Book: Head First Design Patterns** - Chapters 3, 6 (Decorator, Adapter)
- **[Facade Pattern Tutorial](https://refactoring.guru/design-patterns/facade)** - simplifying complex systems

### Practical Exercises

#### HLD Exercises
1. **Multi-Level Caching Implementation:**
   - Deploy: Redis (application cache) + CloudFront (CDN)
   - Test: Cache hit ratios at each level
   - Measure: Latency improvement (browser cache: 0ms, CDN: 50ms, origin: 200ms)
   - Calculate: Cost savings from reduced origin load (bandwidth costs)

2. **Circuit Breaker Experimentation:**
   - Service A calls flaky Service B (20% failure rate, 5s timeout)
   - Implement: Circuit breaker with 50% failure threshold, 10s cooldown
   - Measure: Request success rate and latency before/after circuit breaker
   - Observe: How circuit breaker prevents cascading failures

3. **Rate Limiting Design:**
   - Implement: Token bucket algorithm for API (100 req/min per user)
   - Test: Burst traffic handling vs sustained traffic
   - Deploy: At API gateway vs application layer (trade-offs)
   - Calculate: Cost of rate limiter state storage (Redis vs in-memory)

4. **Observability Setup:**
   - Instrument: Service with Prometheus metrics, structured logging
   - Create: Grafana dashboard showing RED metrics
   - Add: Distributed tracing with correlation IDs across 3 services
   - Measure: Performance overhead (latency, CPU) of instrumentation

#### LLD Exercises
1. **Decorator Pattern for Logging:**
   - Base: DataService with `getData()` method
   - Decorators: LoggingDecorator, CachingDecorator, MetricsDecorator
   - Stack decorators: apply logging + caching + metrics transparently
   - Demonstrate: Adding/removing behavior without modifying base class

2. **Proxy Pattern for Lazy Loading:**
   - Design: ImageProxy that loads heavy images on-demand
   - Virtual proxy: delay creation until needed
   - Protection proxy: add access control checks
   - Cache proxy: return cached results for repeated requests

### Problem Sets

#### Trade-off Analysis
**Problem 1: CDN vs Origin Serving**
- System: News website with 10M daily visitors, 500KB avg page size
- Geography: 40% US, 30% EU, 20% Asia, 10% other
- Options: (A) Serve from single US datacenter, (B) Use global CDN
- **Calculate:**
  - Latency: US datacenter to Asia (200ms) vs CDN edge (20ms)
  - Bandwidth cost: Origin bandwidth ($0.08/GB) vs CDN ($0.02/GB at edge)
  - Total monthly cost for both options
  - Break-even point: At what traffic volume does CDN pay off?
- **Deliverable:** Cost model spreadsheet + recommendation

**Problem 2: Circuit Breaker Configuration**
- Service: Payment gateway calling third-party API (99% SLA, 1s timeout)
- Question: What circuit breaker thresholds to set?
- **Analysis:**
  - Failure threshold: 50% vs 80% (false positives vs protection)
  - Cooldown period: 5s vs 30s (quick recovery vs stability)
  - Half-open state: How many test requests before fully closing?
  - Impact: User experience during open circuit (show cached data? error page?)
- **Deliverable:** Configuration with justification based on SLA math

**Problem 3: Rate Limiting Strategy**
- API: Public REST API with free and paid tiers
- Free: 100 req/hour, Paid: 10K req/hour
- Algorithms: Fixed window, sliding window, token bucket
- **Compare:**
  - Burst handling: Token bucket allows temporary bursts, fixed window doesn't
  - Memory: Fixed window (1 counter), sliding window (log of requests), token bucket (token count + timestamp)
  - Distributed rate limiting: Redis vs local in-memory (consistency vs latency)
  - Cost: Redis cluster for global rate limits (~$500/month) vs eventual consistency issues
- **Recommendation with trade-off matrix**

#### Capacity Estimation
Design **Netflix-like Video Streaming:**
- 200M subscribers, 50% daily active, avg 2 hours watch time
- Video quality: 720p (3 Mbps), 1080p (5 Mbps), 4K (25 Mbps)
- Distribution: 70% 1080p, 20% 720p, 10% 4K
- Calculate:
  1. Concurrent streaming users (peak = 3x average)
  2. Total bandwidth requirement (Tbps)
  3. CDN PoPs needed (assume 100 Gbps per PoP)
  4. Storage: 10K movies, avg 2 hours, multiple quality levels
  5. Monthly CDN cost (assume $0.02/GB)
  6. Origin bandwidth vs CDN cache hit ratio impact on cost

#### LLD Problem
**Design a Caching Proxy**
- Requirements:
  - Proxy pattern: intercept requests to remote service
  - Decorator pattern: add caching, logging, metrics
  - Support: TTL-based expiration, LRU eviction
  - Thread-safe for concurrent access
- Deliverable: Class diagram showing Proxy + Decorator collaboration

### Daily Schedule

| Day | HLD Focus (3h) | LLD Focus (1.5h) | Deliverable |
|-----|----------------|------------------|-------------|
| Mon | Layered architecture; API gateway patterns; read microservices.io | Adapter pattern: interface conversion | Doc: "When to use API gateway vs direct service calls" |
| Tue | CDN setup with CloudFront; cache invalidation strategies | Decorator pattern: dynamic behavior addition | CDN performance report with latency/cost analysis |
| Wed | Circuit breaker + bulkhead implementation; resilience testing | Proxy pattern: access control and lazy loading | Working circuit breaker with failure scenario tests |
| Thu | Rate limiting algorithms; distributed rate limiting with Redis | Facade pattern: simplifying complex APIs | Rate limiter implementation + algorithm comparison |
| Fri | **Design problem:** Design YouTube video delivery system | Design extensible caching system (Proxy + Decorator) | Full architecture with CDN, caching, and resilience + LLD |

### Infrastructure Cost Reality

**Small System (100K DAU):**
- 5 application servers: $500/month
- 1 primary DB + 2 replicas: $800/month
- Redis cache: $200/month
- Load balancer: $50/month
- Monitoring (DataDog): $300/month
- **Total: ~$2K/month**

**Medium System (1M DAU):**
- 50 application servers: $5K/month
- Database sharding (10 shards): $8K/month
- Redis cluster: $1K/month
- CDN (100TB/month): $2K/month
- Load balancers + API gateway: $500/month
- Observability stack: $2K/month
- **Total: ~$18.5K/month**

**Large System (10M DAU):**
- Auto-scaling groups (100-500 instances): $50K/month
- Multi-region databases: $40K/month
- Message broker (Kafka): $5K/month
- CDN (1PB/month): $20K/month
- Infrastructure overhead: $10K/month
- Observability at scale: $10K/month
- **Total: ~$135K/month**

**Key insight:** Infrastructure cost grows sublinearly with scale (economies of scale), but operational complexity grows superlinearly (coordination overhead).

---

## Week 5: End-to-End System Design + Complete LLD Practice

### Learning Objectives
- Design complete production systems with HLD + LLD integration
- Perform comprehensive trade-off analysis for real-world scenarios
- Practice staff-level interview communication skills
- Master complex LLD problems (parking lot, chess, elevator, etc.)

### Core Concepts

#### High-Level Design (3 hours/day)
**Complete System Design Process**
1. **Requirements Clarification:**
   - Functional requirements (what features?)
   - Non-functional requirements (latency, availability, consistency)
   - Scale: users, requests/sec, data size
   - Constraints: budget, team size, timeline

2. **Capacity Estimation:**
   - Storage: data size × retention period
   - Bandwidth: requests/sec × payload size
   - Compute: QPS × processing time per request
   - Cost: infrastructure + operational overhead

3. **High-Level Architecture:**
   - Component diagram: clients, servers, databases, caches, queues
   - Data flow: synchronous vs asynchronous paths
   - Technology choices with justification

4. **Detailed Component Design:**
   - API design: endpoints, request/response formats
   - Database schema: tables, indexes, partitioning
   - Caching strategy: what to cache, where, TTL
   - Message flows: event-driven choreography

5. **Trade-off Analysis:**
   - Consistency vs availability (CAP theorem applied)
   - Latency vs cost (caching, CDN, compute resources)
   - Complexity vs maintainability (microservices overhead)

6. **Failure Scenarios:**
   - Single points of failure identification
   - Mitigation strategies: replication, circuit breakers, graceful degradation
   - Disaster recovery and data backup

#### Low-Level Design (1.5 hours/day)
**Complex LLD Problems**
- **Parking Lot System:** Vehicle types, spot allocation, pricing
- **Chess Game:** Pieces, board, move validation, game state
- **Elevator System:** Scheduling algorithms, multi-elevator coordination
- **Library Management:** Books, members, checkout, reservations
- **ATM System:** Transactions, cash dispensing, account management
- **Hotel Booking:** Rooms, reservations, concurrency handling

**LLD Best Practices**
- Start with use cases and requirements
- Identify core entities and relationships
- Define interfaces before implementations
- Apply SOLID principles and design patterns
- Consider concurrency and thread safety
- Write pseudocode for critical algorithms

### Resources

#### HLD Resources
- **[System Design Primer (GitHub)](https://github.com/donnemartin/system-design-primer)** - comprehensive case studies
- **[Grokking the System Design Interview](https://www.designgurus.io/course/grokking-the-system-design-interview)** - Educative course (paid)
- **[ByteByteGo YouTube Channel](https://www.youtube.com/@ByteByteGo)** - visual system design explanations
- **[Design Twitter](https://www.geeksforgeeks.org/interview-experiences/design-twitter-a-system-design-interview-question/)** - GeeksforGeeks walkthrough
- **[System Design Interview by Alex Xu](https://www.oreilly.com/library/view/system-design-interview/9798664653403/)** - O'Reilly book (volumes 1 & 2)
- **Case Studies:**
  - [Instagram Architecture](https://www.youtube.com/watch?v=QmX2NPkJTKg) - ByteByteGo
  - [Netflix Architecture](https://netflixtechblog.com/) - Netflix Tech Blog
  - [Uber Architecture](https://www.youtube.com/watch?v=umWABit-wbk) - Gaurav Sen
  - [WhatsApp Architecture](https://www.youtube.com/watch?v=vvhC64hQZMk) - ByteByteGo

#### LLD Resources
- **[Grokking the Low-Level Design Interview](https://www.designgurus.io/course/grokking-the-low-level-design-interview)** - DesignGurus (paid)
- **[LLD Case Studies (GitHub)](https://github.com/tssovi/grokking-the-object-oriented-design-interview)** - open-source problems
- **[Parking Lot Design](https://www.youtube.com/watch?v=DSGsa0pu8-k)** - Exponent YouTube
- **[Chess Game Design](https://www.youtube.com/watch?v=yBsWza2039o)** - Success in Tech YouTube
- **Book: [Head First Object-Oriented Analysis and Design](https://www.oreilly.com/library/view/head-first-object-oriented/0596008678/)** - O'Reilly

### Practical Exercises

#### Day-by-Day Design Problems
Each day: Complete one full HLD problem (3 hours) + one LLD problem (1.5 hours)

**Monday:**
- **HLD:** Design Twitter-like Social Media Platform
  - Requirements: Post tweets (280 chars), follow users, timeline (following + recommendations)
  - Scale: 500M users, 100M DAU, 1 billion tweets/day
  - Focus: Fan-out write vs fan-out read, timeline generation, caching strategy
- **LLD:** Parking Lot System
  - Requirements: Multiple vehicle types (car, bike, truck), spot allocation, hourly pricing
  - Patterns: Strategy (pricing), Factory (vehicle creation), Singleton (ParkingLot)

**Tuesday:**
- **HLD:** Design YouTube Video Streaming
  - Requirements: Upload videos, transcode multiple qualities, stream to millions
  - Scale: 1B users, 500M DAU, 1M videos uploaded/day
  - Focus: Video processing pipeline, CDN distribution, adaptive bitrate streaming
- **LLD:** Elevator System
  - Requirements: Multiple elevators, optimize for wait time, handle concurrent requests
  - Patterns: State (elevator states), Strategy (scheduling algorithm), Observer (floor requests)

**Wednesday:**
- **HLD:** Design WhatsApp Messaging System
  - Requirements: 1-to-1 chat, group chat, message delivery confirmation, media sharing
  - Scale: 2B users, 100B messages/day
  - Focus: WebSocket connections, message queue, offline message storage, end-to-end encryption
- **LLD:** Chess Game
  - Requirements: 2-player game, move validation, checkmate detection, move history
  - Patterns: Command (moves with undo), Strategy (piece movement), State (game phases)

**Thursday:**
- **HLD:** Design Amazon E-commerce Checkout
  - Requirements: Product catalog, shopping cart, inventory management, payment processing
  - Scale: 300M users, 10M transactions/day, 99.99% availability required
  - Focus: Distributed transactions (saga pattern), inventory reservation, payment idempotency
- **LLD:** Hotel Booking System
  - Requirements: Search rooms, book with date range, handle concurrent bookings
  - Patterns: Strategy (pricing), Observer (booking notifications), Factory (room types)
  - Challenge: Prevent double-booking with pessimistic locking

**Friday:**
- **HLD:** Design Google Drive File Storage
  - Requirements: Upload/download files, folder hierarchy, sharing, version control
  - Scale: 1B users, 10PB total storage
  - Focus: Chunking strategy, deduplication, sync protocol, metadata storage
- **LLD:** ATM System
  - Requirements: Check balance, withdraw cash, deposit, transfer between accounts
  - Patterns: State (ATM states), Strategy (transaction types), Proxy (bank server communication)
  - Challenge: Handle concurrent withdrawals, insufficient funds, hardware failures

### Mock Interview Practice

#### Self-Practice Framework
1. **Setup (5 min):**
   - Read problem statement
   - Clarify ambiguous requirements (write down assumptions)

2. **Requirements (10 min):**
   - List functional requirements (5-7 key features)
   - Define non-functional requirements (latency SLA, availability, consistency model)
   - Perform capacity estimation (storage, bandwidth, QPS)

3. **High-Level Design (20 min):**
   - Draw component diagram (boxes and arrows)
   - Explain data flow for key operations
   - Justify technology choices with trade-offs

4. **Deep Dive (15 min):**
   - Pick 1-2 components to design in detail
   - Database schema or API contracts
   - Discuss failure scenarios and mitigation

5. **Review (10 min):**
   - Identify bottlenecks and optimization opportunities
   - Discuss monitoring and operational concerns
   - Consider cost and scaling strategy

#### Peer Review Checklist
- [ ] Requirements clearly stated and validated
- [ ] Capacity math shown and reasonable
- [ ] Component diagram clear and complete
- [ ] Technology choices justified with trade-offs (not just "use Redis")
- [ ] Failure scenarios discussed with mitigation strategies
- [ ] Consistency model explicitly chosen (strong vs eventual)
- [ ] Cost estimate provided (at least ballpark)
- [ ] Scaling strategy articulated (what breaks at 10x load?)
- [ ] Operational complexity acknowledged (on-call burden, debugging)

### Problem Sets

#### Comprehensive Trade-off Analysis
For each design this week, complete this matrix:

| Decision | Alternative | Latency | Availability | Cost | Complexity | Why Chosen |
|----------|------------|---------|--------------|------|------------|------------|
| Example: Use read replicas | Cache-aside pattern | +20ms (replication lag) | +2 nines (replica failover) | +$500/mo | +2 ops complexity | Better consistency than cache |

#### Failure Scenario Planning
For each system designed, document:
1. **Single Points of Failure:** What breaks the system?
2. **Mitigation:** Replication, circuit breakers, graceful degradation
3. **Detection:** How do you know it failed? (monitoring alerts)
4. **Recovery:** Manual intervention or automatic? (RTO/RPO targets)

#### Cost Optimization Exercise
Given: Your YouTube-like system costs $100K/month
Task: Reduce to $70K/month without degrading UX
Consider:
- CDN optimization (cache hit ratio improvement)
- Video transcoding efficiency (codec selection)
- Database query optimization (reduce read IOPS)
- Auto-scaling tuning (right-size instances)
**Deliverable:** Detailed cost breakdown with optimization plan

### Daily Schedule

| Day | Morning (2.5h) | Afternoon (2h) | Deliverable |
|-----|----------------|----------------|-------------|
| Mon | Design Twitter (HLD): requirements → architecture → deep dive | Parking Lot (LLD): class diagram → code skeleton | Complete design doc + LLD implementation |
| Tue | Design YouTube (HLD): video pipeline → CDN strategy → cost model | Elevator System (LLD): state machine → scheduling algorithm | Architecture diagram + LLD with concurrency handling |
| Wed | Design WhatsApp (HLD): messaging architecture → WebSocket vs polling | Chess Game (LLD): piece hierarchy → move validation | Full system design + chess engine LLD |
| Thu | Design Amazon Checkout (HLD): saga pattern → payment idempotency | Hotel Booking (LLD): prevent double-booking → concurrent reservations | E-commerce architecture + booking system with locking |
| Fri | Design Google Drive (HLD): sync protocol → deduplication | ATM System (LLD): state machine → transaction handling | Storage system design + ATM complete implementation |

### Staff-Level Communication Tips

**What Separates Staff from Senior:**
1. **Proactive Trade-off Discussion:** Don't wait to be asked "what are the trade-offs?" Lead with them.
2. **Quantitative Reasoning:** Use numbers to justify decisions ("reduces latency from 200ms to 50ms" not "makes it faster")
3. **Organizational Awareness:** Acknowledge team size, expertise, and operational capability
4. **Cost Consciousness:** Every architectural decision has a dollar amount
5. **When to NOT Engineer:** Recognize when simpler solutions suffice (premature optimization)

**Example Staff-Level Answer:**
> "I'm proposing read replicas instead of caching because: (1) we need strong consistency for financial data, (2) our read:write ratio is 10:1 so we'd get 90% load reduction, (3) replication lag is acceptable at 20ms vs cache invalidation complexity, (4) cost is $500/month for 2 replicas vs $300/month Redis but easier ops, (5) our team has more MySQL expertise than cache tuning skills. If latency becomes an issue at 10x scale, we can add caching layer later without rearchitecting."

**Avoid Junior Answers:**
- "Let's use Redis" (why? what's the alternative? what does it cost?)
- "Microservices are scalable" (when? at what complexity cost?)
- "NoSQL is better for this" (better than what? in which dimensions?)

---

## Appendix: Reference Materials

### Back-of-the-Envelope Cheat Sheet

#### Powers of Two
- 2^10 = ~1 thousand (1KB)
- 2^20 = ~1 million (1MB)
- 2^30 = ~1 billion (1GB)
- 2^40 = ~1 trillion (1TB)

#### Latency Numbers (2024)
- L1 cache: 0.5 ns
- L2 cache: 7 ns
- RAM: 100 ns
- SSD: 150 μs (150,000 ns)
- HDD: 10 ms (10,000,000 ns)
- Network (same datacenter): 0.5 ms
- Network (cross-continent): 150 ms

#### Throughput Estimates
- Good web server: 1K RPS
- MySQL (well-tuned): 5K writes/sec, 100K reads/sec
- Redis: 100K ops/sec
- Kafka: 1M messages/sec per partition

#### Time Conversions
- 1M seconds = ~12 days
- 1B seconds = ~32 years
- 1 req/sec × 86400 = 86.4K req/day
- 1K QPS × 2.6M = 2.6B req/month

#### Storage Calculations
- 1M users × 1KB profile = 1GB
- 100M tweets/day × 280 chars × 30 days = 840GB text
- 1M images × 2MB = 2TB

#### Cost Estimates (AWS, approximate)
- EC2 t3.medium: $30/month
- RDS db.t3.medium: $100/month
- S3 storage: $0.023/GB/month
- Data transfer: $0.09/GB (out)
- CloudFront CDN: $0.085/GB

### CAP Theorem Quick Reference

| System Type | Consistency | Availability | Partition Tolerance | Use Case |
|-------------|-------------|--------------|---------------------|----------|
| **CP** | ✅ Strong | ❌ May block | ✅ Handles | Banking, inventory |
| **AP** | ❌ Eventual | ✅ Always responds | ✅ Handles | Social feeds, analytics |
| **CA** | ✅ Strong | ✅ Available | ❌ No partition | Single-node (not distributed) |

**Reality:** All distributed systems must tolerate partitions, so choice is really **CP vs AP**.

### Database Selection Matrix

| Factor | SQL (PostgreSQL) | NoSQL Document (MongoDB) | NoSQL Wide-Column (Cassandra) | NoSQL Key-Value (Redis) |
|--------|------------------|-------------------------|------------------------------|------------------------|
| **Schema** | Fixed, normalized | Flexible JSON | Column families | Simple key-value |
| **Transactions** | ACID, multi-row | Single-document ACID | Eventual consistency | No transactions |
| **Scaling** | Vertical, read replicas | Horizontal sharding | Horizontal, masterless | Horizontal, clustering |
| **Query Complexity** | JOINs, complex queries | Rich queries, no JOINs | Simple queries by key | Get/set by key |
| **Consistency** | Strong | Tunable (strong possible) | Eventual (tunable quorum) | Eventual |
| **Best For** | Relational data, complex queries | Flexible schemas, moderate scale | High write throughput, AP | Caching, sessions, real-time |
| **Cost** | Vertical scaling limits | Moderate operational | High operational | Low operational |

### Design Pattern Quick Reference

| Pattern | Category | Problem | Solution | When to Use |
|---------|----------|---------|----------|-------------|
| **Factory** | Creational | Object creation | Factory method creates objects | Multiple related classes |
| **Builder** | Creational | Complex construction | Fluent interface | Many optional parameters |
| **Singleton** | Creational | One instance | Private constructor, static instance | Configuration, connection pools |
| **Strategy** | Behavioral | Algorithm selection | Encapsulate algorithms | Runtime algorithm switching |
| **Observer** | Behavioral | Event notification | Pub/sub pattern | Event-driven systems |
| **Command** | Behavioral | Request as object | Encapsulate request | Undo/redo, queuing |
| **Adapter** | Structural | Interface mismatch | Wrapper converts interface | Legacy integration |
| **Decorator** | Structural | Add behavior | Wrap with additional functionality | Dynamic behavior extension |
| **Proxy** | Structural | Control access | Surrogate controls access | Lazy loading, security |

### Consistency Models Spectrum

```
Strong ←――――――――――――――――――――――――――――――――――→ Weak
│                                            │
Linearizable     Causal        Eventual     No Guarantees
│                │             │
└─ Banking       └─ Social     └─ Analytics
```

- **Linearizable (Strong):** Every read sees most recent write, as if single copy
- **Causal:** Related operations ordered (A happened-before B)
- **Eventual:** All replicas converge eventually (minutes/seconds delay OK)
- **Read-Your-Writes:** User sees their own updates immediately
- **Monotonic Reads:** Time never goes backward for a user

### Recommended Reading List

1. **[Designing Data-Intensive Applications](https://www.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/)** by Martin Kleppmann (O'Reilly)
   - The Bible of distributed systems (Chapters 1-9 essential)

2. **[System Design Interview Volumes 1 & 2](https://www.oreilly.com/library/view/system-design-interview/9798664653403/)** by Alex Xu
   - Polished case studies with visual diagrams

3. **[Head First Design Patterns](https://www.oreilly.com/library/view/head-first-design/9781492077992/)** by Freeman & Robson
   - Engaging OOP pattern introduction

4. **[Microservices Patterns](https://www.oreilly.com/library/view/microservices-patterns/9781617294549/)** by Chris Richardson
   - Practical microservices architecture patterns

5. **[Software Architecture: The Hard Parts](https://www.oreilly.com/library/view/software-architecture-the/9781492086888/)** by Neal Ford et al.
   - Trade-off analysis framework for modern architectures

### YouTube Channels
- **[ByteByteGo](https://www.youtube.com/@ByteByteGo)** - Visual system design explanations
- **[Gaurav Sen](https://www.youtube.com/@gkcs)** - System design fundamentals
- **[Hussein Nasser](https://www.youtube.com/@hnasr)** - Backend engineering deep dives
- **[Martin Kleppmann](https://www.youtube.com/@MartinKleppmann)** - Distributed systems theory

---

## Success Criteria

After completing this curriculum, you should be able to:

### Technical Competence
- [ ] Design complete systems (HLD) with justified technology choices
- [ ] Perform back-of-the-envelope capacity calculations in under 5 minutes
- [ ] Create low-level class diagrams (LLD) applying appropriate design patterns
- [ ] Articulate trade-offs quantitatively (latency, cost, complexity, availability)
- [ ] Identify failure scenarios and propose mitigation strategies
- [ ] Estimate infrastructure costs within 30% accuracy

### Staff-Level Maturity
- [ ] Explain when NOT to use distributed patterns (recognize premature optimization)
- [ ] Consider organizational factors (team size, expertise, on-call burden)
- [ ] Design migration paths from current to target architecture
- [ ] Understand operational complexity trade-offs (debugging, monitoring, coordination)
- [ ] Communicate design decisions with confidence and clarity

### Interview Readiness
- [ ] Complete 15+ end-to-end system design problems
- [ ] Solve 10+ low-level design problems with working code
- [ ] Explain designs clearly within 45-minute interview timeframe
- [ ] Handle follow-up questions about scale, failures, and alternatives

---

## Final Thoughts

Staff-level system design is not about memorizing patterns or tools. It's about:

1. **Quantitative Thinking:** Use math to validate decisions
2. **Trade-off Awareness:** Every choice has costs—make them explicit
3. **Practical Constraints:** Real systems have budgets, teams, and deadlines
4. **Operational Reality:** Someone has to maintain and debug your architecture

The best architects know when to NOT engineer. Complexity is a cost—only pay for it when the value is clear.

**Good luck on your journey to staff-level mastery!**