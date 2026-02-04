# Python, Django, REST API, MySQL Interview Questions (4+ Years Experience)

---

## Python Core

1. What does "everything is an object" mean in Python?
2. Explain mutable vs immutable objects with a real bug example.
3. What is the internal difference between list and tuple?
4. Where does shallow copy cause issues in real projects?
5. What is the cost of deep copy?
6. How does Python manage memory internally?
7. What is reference counting?
8. When can Python garbage collection fail?
9. Why and when do we use **slots**?
10. What is name mangling in Python?
11. What is duck typing?
12. Is Python pass by value or pass by reference?
13. Explain GIL with a practical example.
14. Why is multithreading slow for CPU-bound tasks in Python?
15. Generator vs list: memory comparison.
16. How does yield work internally?
17. How do you implement a custom iterator?
18. How does a decorator work internally?
19. How does the order of multiple decorators matter?
20. What are the limitations of lambda functions?
21. Difference between *args and **kwargs.
22. Difference between is and ==.
23. How can memory leaks occur in Python?
24. What is a circular reference?
25. How do you optimize Python code in production?

---

## Advanced Python / Design

1. What is monkey patching and what are its risks?
2. Why do we use context managers?
3. How does the with statement work internally?
4. What are the risks of using eval and exec?
5. What is a metaclass?
6. When should inheritance not be used?
7. Composition vs inheritance.
8. When do you use an abstract base class?
9. How do you implement interfaces in Python?
10. What is thread safety?
11. Difference between Lock and RLock.
12. What is a deadlock and how does it happen?
13. Async vs threading in Python.
14. When is asyncio a better choice?
15. What is an event loop?
16. Futures vs coroutines.
17. Explain a race condition with example.
18. What would happen if Python removed the GIL?
19. Difference between CPython and PyPy.
20. How do you make Python code production-ready?

---

## Django Core

1. Explain Django request-response lifecycle.
2. Difference between MVT and MVC.
3. Explain Django middleware execution order.
4. Why did you write a custom middleware?
5. What changes are required in settings.py for production?
6. What is WSGI?
7. When should ASGI be used?
8. Django signals: pros and cons.
9. Fat model vs fat view.
10. Best practices for Django app structure.
11. Difference between manage.py and django-admin.
12. What security features does Django provide?
13. How does CSRF token validation work internally?
14. How does Django hash passwords?
15. Difference between static files and media files.
16. How do Django migrations work internally?
17. How do you resolve migration conflicts?
18. How do you use Django shell for debugging?
19. How do you configure logging in Django?
20. How do you scale a Django application?
21. Why use multiple settings files?
22. Why are environment variables important?
23. What usually breaks during Django upgrades?
24. Django monolith vs microservices.
25. What are Django limitations?

---

## Django ORM / Database

1. What is ORM and why avoid raw SQL?
2. select_related vs prefetch_related.
3. Explain the N+1 query problem.
4. Difference between annotate and aggregate.
5. values vs values_list.
6. exists() vs count().
7. What is lazy evaluation in QuerySets?
8. How does Django handle transactions?
9. How does transaction.atomic work internally?
10. Why are indexes important?
11. When should composite indexes be used?
12. Explain on_delete options in ForeignKey.
13. What are the risks of CASCADE?
14. OneToOneField vs ForeignKey.
15. How is ManyToMany stored internally?
16. When do you use raw SQL in Django?
17. Risks of bulk_create.
18. Why can ORM be slow?
19. How do you optimize ORM queries?
20. How do you handle database deadlocks?
21. How does Django manage DB connections?
22. Django ORM vs SQLAlchemy.
23. Why are production migrations risky?
24. When do you use read replicas?
25. Drawbacks of ORM abstraction.

---

## Django REST Framework (DRF)

1. What is REST and why use DRF?
2. Serializer vs ModelSerializer.
3. How do you perform validation in serializers?
4. Custom validation examples.
5. APIView vs ViewSet.
6. When to use generic views?
7. JWT vs session authentication.
8. Why are refresh tokens needed?
9. Authentication vs permission.
10. How do you write custom permissions?
11. Why is throttling required?
12. Types of pagination in DRF.
13. API versioning strategies.
14. PUT vs PATCH.
15. What is an idempotent API?
16. Correct usage of HTTP status codes.
17. Error handling best practices in APIs.
18. How do you maintain backward compatibility?
19. Rate limiting design.
20. File upload API challenges.
21. Performance issues with serializers.
22. Problems with nested serializers.
23. Common API security risks.
24. What is CORS?
25. How do you test APIs?
26. Swagger / OpenAPI usage.
27. How do you optimize API response time?
28. Alternatives to DRF signals.
29. How do you implement API caching?
30. Limitations of DRF.

---

## MySQL / Database

1. Explain ACID properties.
2. How does an index work internally?
3. B-tree vs hash index.
4. Primary key vs unique key.
5. Pros and cons of foreign key constraints.
6. Normalization vs denormalization.
7. What is a deadlock?
8. Transaction isolation levels.
9. Repeatable read vs read committed.
10. How do you debug slow queries?
11. Explain EXPLAIN query output.
12. When does a full table scan occur?
13. Why is pagination costly in SQL?
14. Problems with OFFSET-based pagination.
15. Cursor-based pagination.
16. Data consistency vs performance trade-off.
17. Database connection leaks.
18. Backup strategies.
19. Risks in migration rollback.
20. MySQL vs PostgreSQL.

---

## Multithreading & Multiprocessing

1. Difference between thread and process.
2. Limitations of Python threading.
3. Explain GIL clearly.
4. I/O-bound vs CPU-bound examples.
5. When does threading help?
6. When is multiprocessing better?
7. When should Celery be used?
8. Worker vs thread.
9. Role of Queue in threading.
10. Lock usage with example.
11. Deadlock scenario.
12. What is a daemon thread?
13. What is thread-safe code?
14. Shared memory problems.
15. How do processes communicate?
16. What is a Pool in multiprocessing?
17. How do you handle exceptions in threads?
18. Async vs threading.
19. Django + Celery architecture.
20. Handling background job failures.

---

## Performance & Scaling

1. First steps when a Django app becomes slow.
2. How do you identify DB vs code bottlenecks?
3. Caching strategies.
4. When to use Redis?
5. Per-view vs low-level caching.
6. How do you calculate Gunicorn workers?
7. Horizontal vs vertical scaling.
8. Basics of load balancing.
9. Why should APIs be stateless?
10. CDN usage.
11. Logging overhead concerns.
12. Monitoring tools.
13. How do you detect memory leaks in production?
14. Handling API timeouts.
15. What is graceful shutdown?

---

## Testing & Quality (Additional)

1. Difference between unit test and integration test.
2. How do you test Django views?
3. How do you test DRF APIs?
4. What is mocking and why is it needed?
5. How do you mock external APIs?
6. Test database vs production database.
7. What is pytest vs unittest?
8. What is test coverage?
9. How much test coverage is enough?
10. Common testing mistakes in Django projects.
11. How do you test authentication and permissions?
12. How do you test background tasks (Celery)?
13. What should not be tested?
14. How do you write testable code?
15. CI/CD impact on testing.

---

## Deployment & DevOps Basics (Additional)

1. How do you deploy a Django application?
2. What is Gunicorn and why is it used?
3. Nginx role in Django deployment.
4. WSGI vs ASGI in deployment.
5. Environment-specific settings.
6. How do you manage secrets in production?
7. What is Docker and why use it?
8. Dockerfile vs docker-compose.
9. Zero-downtime deployment strategy.
10. Handling migrations during deployment.
11. Static files handling in production.
12. Media files handling in production.
13. Rollback strategy after failed deployment.
14. What is CI/CD pipeline?
15. Common deployment mistakes.

---

## System Design (Python/Django Focus)

1. Design a simple user authentication system.
2. Design a REST API for file upload.
3. Design a rate-limited API.
4. Design a background job processing system.
5. How would you design email sending service?
6. Design a notification system.
7. Design pagination for large datasets.
8. Design caching layer for APIs.
9. Design audit logging system.
10. Design role-based access control.
11. How do you scale read-heavy APIs?
12. How do you handle high write load?
13. Designing idempotent APIs.
14. Designing retry mechanism.
15. Designing fault-tolerant services.

---

## Production & Real-World Scenarios (Very Important)

1. A migration broke production. What do you do?
2. API response time suddenly doubled. How do you debug?
3. Database CPU is high. What are your steps?
4. Memory usage keeps increasing. How do you investigate?
5. Users report random logout issues.
6. Background jobs are failing silently.
7. Duplicate records are getting created.
8. Data inconsistency between services.
9. Handling partial failures in APIs.
10. How do you handle backward incompatible changes?
11. Handling traffic spikes.
12. Debugging issues without logs.
13. Hotfix vs proper fix decision.
14. Handling long-running requests.
15. Handling timezone-related bugs.

---

## Behavioral (Technical-Focused)

1. A senior disagrees with your implementation. What do you do?
2. You missed a production bug. How do you handle it?
3. How do you review someone else's code?
4. How do you handle pressure during production incidents?
5. How do you estimate task timelines?
6. How do you handle unclear requirements?
7. How do you prioritize bugs vs features?
8. How do you explain technical debt to non-technical people?
9. How do you mentor juniors?
10. Biggest technical mistake you made and learned from.

---

## API Design & Integration (Additional)

1. How do you design APIs for backward compatibility?
2. How do you handle partial success responses?
3. What is HATEOAS? Have you used it?
4. How do you design bulk APIs?
5. How do you handle idempotency keys?
6. API timeout vs long polling.
7. Webhooks vs polling.
8. How do you secure webhooks?
9. How do you design retry-safe APIs?
10. How do you version APIs without breaking clients?
11. Handling third-party API failures.
12. Circuit breaker pattern.
13. API gateway role.
14. Rate limiting strategies.
15. API contract testing.

---

## Data Modeling & Architecture (Additional)

1. How do you design database schema from requirements?
2. When do you choose NoSQL over SQL?
3. How do you handle schema evolution?
4. Soft delete vs hard delete.
5. Audit tables design.
6. Multi-tenant database design.
7. Sharding basics.
8. Read-heavy vs write-heavy schema design.
9. Handling large tables.
10. UUID vs auto-increment IDs.
11. Handling historical data.
12. Data migration strategy.
13. Referential integrity vs performance.
14. Designing for analytics vs transactions.
15. Handling data privacy requirements.

---

## Security (Advanced)

1. OWASP Top 10 relevance to Django.
2. How do you prevent SQL injection in Django?
3. How do you prevent XSS?
4. CSRF attack flow.
5. Token storage best practices.
6. Password hashing algorithms.
7. Secrets rotation strategy.
8. Role-based vs attribute-based access control.
9. API key management.
10. Preventing brute force attacks.
11. Security headers usage.
12. HTTPS enforcement.
13. Handling sensitive data in logs.
14. Security testing approaches.
15. Handling security incidents.

---

## Code Quality & Maintainability

1. What makes code readable?
2. How do you reduce cyclomatic complexity?
3. Refactoring strategy.
4. Handling legacy code.
5. Code review best practices.
6. Common code smells.
7. Dependency management.
8. Handling breaking changes.
9. Writing self-documenting code.
10. Documentation importance.
11. Managing large Django projects.
12. Modularization strategies.
13. Avoiding tight coupling.
14. Backward compatibility in code.
15. Technical debt management.

---

# Python, Django Interview Questions – Advanced / Part 2

---

## Distributed Systems (Advanced)

1. What is a distributed system?
2. CAP theorem and real-world trade-offs.
3. Consistency vs availability.
4. Eventual consistency examples.
5. Synchronous vs asynchronous communication.
6. Message queues vs REST APIs.
7. Kafka vs RabbitMQ basics.
8. Exactly-once vs at-least-once delivery.
9. Handling duplicate messages.
10. Designing idempotent consumers.
11. Distributed transactions problems.
12. Saga pattern basics.
13. Service discovery.
14. Circuit breaker pattern.
15. Backpressure handling.

---

## Caching Deep Dive

1. Cache-aside vs write-through.
2. Cache invalidation strategies.
3. Cache stampede problem.
4. TTL tuning.
5. Redis data structures.
6. Redis persistence options.
7. Redis eviction policies.
8. Hot key problem.
9. Distributed cache consistency.
10. When caching hurts performance.
11. API-level caching vs DB caching.
12. Per-user cache design.
13. Cache warming strategies.
14. Handling stale data.
15. Monitoring cache efficiency.

---

## Django Internals (Advanced)

1. How Django URL resolver works internally.
2. Django ORM query compilation.
3. How QuerySets are evaluated.
4. Django app loading process.
5. Signal dispatch mechanism.
6. Middleware execution chain.
7. Django template rendering flow.
8. How Django handles sessions.
9. Cookie vs session storage.
10. Django authentication backend flow.
11. Permission checks internally.
12. How Django handles file uploads.
13. Static file collection process.
14. Django startup time optimization.
15. Django internals limitations.

---

## Advanced REST Concepts

1. REST vs RPC.
2. GraphQL vs REST trade-offs.
3. gRPC basics.
4. API aggregation pattern.
5. Backend-for-Frontend (BFF).
6. API schema evolution.
7. OpenAPI contract-first approach.
8. Handling breaking API changes.
9. API documentation automation.
10. API mocking strategies.
11. HATEOAS practical issues.
12. Long-running operations APIs.
13. Streaming APIs basics.
14. Pagination consistency.
15. API governance.

---

## Background Processing & Async

1. When background jobs are mandatory.
2. Celery architecture deep dive.
3. Broker vs backend.
4. Task retries and backoff.
5. Idempotent Celery tasks.
6. Task time limits.
7. Monitoring Celery workers.
8. Handling stuck tasks.
9. Async views in Django.
10. When async Django does not help.
11. Async ORM limitations.
12. Thread pool vs process pool.
13. Scheduling periodic jobs.
14. Handling large batch jobs.
15. Graceful worker shutdown.

---

## Failure Handling & Resilience

1. Types of failures in production.
2. Partial failure handling.
3. Retry vs fail-fast.
4. Exponential backoff.
5. Timeout tuning.
6. Bulkhead pattern.
7. Graceful degradation.
8. Fallback strategies.
9. Chaos testing basics.
10. Designing for failure.
11. Preventing cascading failures.
12. Error budgets.
13. Incident severity levels.
14. Root cause analysis.
15. Learning from outages.

---

## Senior Engineer Decision Making

1. How do you choose between two architectures?
2. How do you evaluate technical risk?
3. Build vs buy decision.
4. When to refactor vs rewrite.
5. How to say no to bad ideas.
6. Balancing deadlines vs quality.
7. Handling unclear ownership.
8. Leading technical discussions.
9. Documenting design decisions.
10. Aligning tech with business goals.
11. Estimating unknown tasks.
12. Handling production pressure.
13. Avoiding over-engineering.
14. Choosing the right abstraction.
15. What makes a system "boring" and why it is good.

---

## Interview Trap & Scenario Questions

1. A fix works locally but not in production.
2. A bug appears only under load.
3. Data loss incident – first steps?
4. Rollback is not possible – what now?
5. Third-party API is down.
6. DB migration is slow on prod.
7. Cache outage impact handling.
8. Memory leak but no clear logs.
9. Sporadic authentication failures.
10. Timezone bugs across services.
11. Duplicate payments incident.
12. Sudden spike in error rates.
13. Background jobs stuck.
14. Deployment succeeded but app is broken.
15. CEO wants a risky hotfix.
