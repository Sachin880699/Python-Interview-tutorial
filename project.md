# CATHAY SHOP – Backend Interview Questions & Answers
( Python | Django | DRF | E-Commerce | Loyalty | PostgreSQL | Redis | Celery | AWS )

---

## 🔹 Python & Django (Core)

### 1. What are Django’s main components and how do they interact?

Django follows an architecture called MTV, which stands for Model, Template, and View. In simple terms, each part has a clear responsibility. The Model represents the database layer and defines how data is stored and related. In our e-commerce project, models were used to define products, orders, users, loyalty points, and inventory.

The View contains the business logic. When a request comes, the view decides what needs to be done. For example, during checkout, the view validates the cart, checks inventory, applies loyalty points, and prepares the response. Views communicate with models to read or update data.

The Template is responsible for presentation. Even though our system was API-driven, templates are generally used to render HTML responses. In APIs, views usually return JSON instead of templates.

The flow is very simple: a request comes to Django, the URL configuration maps it to a view, the view interacts with models, and Django sends back a response.

---

### 2. What is the difference between Django ORM and writing raw SQL?

Django ORM allows us to interact with the database using Python code instead of writing SQL queries directly. This makes the code easier to read, maintain, and less error-prone. It also protects against SQL injection automatically.

In our project, ORM was very helpful because the application had complex relationships like orders, order items, vendors, and inventory. ORM allowed us to handle these relationships cleanly and consistently across the codebase. Raw SQL can sometimes be faster for very complex queries, but it becomes harder to maintain and debug. For most production use cases, Django ORM provides a good balance of performance and safety.

---

### 3. How do Django middlewares work?

Middleware in Django acts like a processing layer between the request and the response. Whenever a request comes in, it passes through middleware before reaching the view, and when a response goes out, it again passes through middleware.

In our e-commerce system, middleware was used for authentication, security checks, logging, and request validation. For example, before allowing access to checkout or order APIs, middleware ensured the user was authenticated. This helps keep cross-cutting concerns separate from business logic.

---

### 4. Explain signals in Django. Have you used them in your project?

Django signals are used when we want certain actions to happen automatically in response to events, without tightly coupling the logic. For example, when an order is successfully created, we may want to send a confirmation email or update loyalty points.

In our project, signals were used after successful order placement or payment confirmation. Once the order was saved, a signal triggered background tasks like sending emails or updating reward balances. This kept the core order logic clean and avoided putting too much responsibility inside views.

---

### 5. How do you handle database migrations safely in production?

Database migrations must be handled very carefully in production systems. We always test migrations in staging environments before applying them in production. Destructive changes like dropping columns are avoided during peak traffic.

In production, migrations are applied during low-traffic windows. For complex schema changes, we use data migrations and backward-compatible changes so the application does not break. This approach ensures zero or minimal downtime.

---

### 6. What are QuerySets and how do you optimize slow queries?

QuerySets represent database queries in Django. They are lazy, meaning they don’t hit the database until the data is actually needed.

In a high-traffic system like ours, query optimization is critical. We optimized slow queries by adding proper indexes, reducing unnecessary database hits, and avoiding N+1 query problems. We also used caching with Redis for frequently accessed data like product listings and user loyalty balances.

---

### 7. Difference between select_related and prefetch_related?

`select_related` is used when fetching related objects using SQL joins, typically for foreign key or one-to-one relationships. `prefetch_related` is used when dealing with many-to-many or reverse relationships and executes separate queries.

In our project, when fetching orders along with order items and products, we used these methods to significantly reduce database queries and improve API performance.

---

### 8. How do you handle environment-specific settings in Django?

We separate environment-specific settings using environment variables. Sensitive information like database credentials, API keys, and secret keys are never hardcoded. Instead, they are stored in environment variables or secure services like AWS Parameter Store.

This allows the same codebase to work across development, staging, and production environments safely.

---

### 9. What are Django transactions and when do you use atomic()?

Transactions ensure that multiple database operations either all succeed or all fail. The `atomic()` block is used when operations must be consistent.

In our checkout flow, order creation, inventory deduction, and loyalty point deduction all happen inside an atomic transaction. If any step fails, everything is rolled back, which prevents data inconsistency.

---

### 10. How do you secure sensitive data in Django?

Security was very important in our project. We never stored sensitive payment information like card numbers. All secrets were stored in environment variables. Passwords were hashed using Django’s built-in mechanisms.

We also enforced HTTPS, validated all inputs, used authentication and permissions properly, and relied on Django’s security middleware to protect against common vulnerabilities.

---

## 🔹 Django REST Framework (DRF)

### 11. What is the role of serializers in DRF?

Serializers convert complex Python objects into JSON and validate incoming data. In our project, serializers were responsible for validating order data, payment inputs, and loyalty redemption values before processing them.

They helped keep validation logic clean and consistent across APIs.

---

### 12. Difference between Serializer and ModelSerializer?

A Serializer requires manual field definitions, while a ModelSerializer automatically generates fields based on a model. In our project, we mostly used ModelSerializer because it reduced boilerplate and kept code clean.

---

### 13. What are ViewSets and Routers?

ViewSets group related API logic together, such as list, create, update, and delete operations. Routers automatically generate URLs for these ViewSets.

We used them extensively for orders, products, and loyalty APIs to keep the API structure consistent.

---

### 14. How did you implement authentication and authorization?

We used JWT-based authentication because it is stateless and scalable. Authorization was handled using permission classes and role-based access, ensuring users could only access their own orders and data.

---

### 15. Difference between JWT, Session, and Token authentication?

Session authentication is suitable for web apps, while token and JWT authentication are better for APIs. JWT was ideal for our high-traffic API-based system because it does not require server-side session storage.

---

## 🔹 E-Commerce Architecture

### 16. How did you design the product catalog?

The product catalog was designed to support complex structures. Products had variants, attributes, pricing, and vendor associations. Inventory was handled separately to allow better scalability and vendor-level stock management.

---

### 17. How did you prevent overselling during checkout?

Overselling was prevented using database transactions and row-level locking. Stock was checked and deducted inside an atomic transaction. If two users tried to buy the same product simultaneously, the database ensured consistency.

---

### 18. Explain the order lifecycle.

An order starts in the cart. During checkout, inventory and pricing are validated. Payment is processed through the gateway. Once payment succeeds, the order status changes to confirmed, followed by shipping, delivery, or cancellation based on subsequent events.

---

### 19. How did you manage price changes without affecting existing orders?

We stored a snapshot of the price inside the order item at the time of purchase. This ensured that even if the product price changed later, existing orders remained unaffected.

---

### 20. What challenges did you face in a high-traffic e-commerce system?

The biggest challenges were handling concurrency, ensuring data consistency, optimizing performance, and managing failures gracefully. Redis, Celery, database transactions, and proper API design helped us solve these challenges.

---

## 🔹 Loyalty & Rewards (Miles Plus Cash)

### 21. What is a Miles Plus Cash system?

Miles Plus Cash allows users to pay for an order using a combination of loyalty points and real money. This improves user engagement and flexibility during checkout.

---

### 22. How did you calculate points and cash during checkout?

During checkout, the system calculates the maximum redeemable points based on business rules. These points are converted to a monetary value using a predefined conversion rate, and the remaining amount is paid via the payment gateway.

---

### 23. How did you prevent double redemption of points?

Point deduction happens inside a database transaction. Once points are locked for a transaction, they cannot be reused until the transaction completes. This prevents double redemption even under concurrent requests.

---

### 24. What happens if payment fails after points are deducted?

If payment fails, the entire transaction is rolled back and points are restored automatically. This ensures users never lose points due to payment failures.

---

### 25. How do you store and track loyalty transactions?

We maintained a points ledger system where every credit and debit was recorded. This provided a complete audit trail and made debugging and reconciliation easier.

---

### 26. How do you make loyalty transactions reversible?

All loyalty operations were designed to be atomic and compensatable. If any step failed, the system reverted the changes, ensuring consistency.

---

## 🔹 Behavioral

### 27. What was the biggest challenge in this project?

The biggest challenge was ensuring consistency across payments, inventory, and loyalty systems under high traffic. Solving this required careful transaction management, background processing, and extensive testing.

---

### 28. What did you learn from this project?

This project taught me how to design scalable backend systems, handle real-world failures, and think beyond just writing code. It improved my understanding of system design, performance optimization, and production stability.

---
---

## 🔹 PostgreSQL & Database Design

### 29. Why did you choose PostgreSQL over MySQL?

We chose PostgreSQL because it is more robust and better suited for complex, high-traffic systems. PostgreSQL provides strong support for transactions, advanced indexing, JSONB fields, and better concurrency handling. In our e-commerce platform, we had complex queries involving orders, inventory, and loyalty points, and PostgreSQL handled these efficiently. Its strict consistency and reliability made it a better choice for financial and order-related data.

---

### 30. What are indexes and when do you use them?

Indexes are used to speed up database queries by allowing the database to find records quickly without scanning the entire table. In our system, we added indexes on frequently queried fields such as order ID, user ID, product ID, and order status. Indexes were especially important for order tracking APIs and admin dashboards where fast filtering was required.

---

### 31. What is ACID and how does it apply to orders?

ACID stands for Atomicity, Consistency, Isolation, and Durability. In our order system, ACID properties ensured that an order was either fully created or not created at all. For example, when placing an order, inventory deduction, payment confirmation, and loyalty point deduction all happened together. If any step failed, the database rolled back the entire transaction, keeping the system consistent.

---

### 32. How do you design tables for high-write systems?

For high-write systems, we keep tables normalized but not overly complex. We separate read-heavy and write-heavy data when possible. In our project, order and order item tables were optimized with proper indexes and minimal joins during writes. We also avoided unnecessary triggers and heavy constraints during peak traffic.

---

### 33. What is locking in PostgreSQL?

Locking is a mechanism used by PostgreSQL to control concurrent access to data. When multiple users try to update the same row, PostgreSQL ensures that only one transaction can modify it at a time. This was critical in our inventory system to prevent multiple users from purchasing the same stock simultaneously.

---

### 34. How do you handle deadlocks?

Deadlocks occur when two transactions wait on each other to release locks. We reduced deadlocks by keeping transactions short, accessing tables in a consistent order, and retrying failed transactions when needed. PostgreSQL automatically detects deadlocks and aborts one transaction, which we handled gracefully in the application.

---

### 35. Difference between row-level and table-level locks?

Row-level locks lock only specific rows being modified, while table-level locks block the entire table. In our project, we relied mainly on row-level locks so that multiple users could place orders for different products at the same time without blocking each other.

---

### 36. How do you optimize slow database queries?

We analyzed slow queries using database logs and explain plans. Optimization involved adding indexes, rewriting inefficient queries, reducing joins, and caching frequently accessed data using Redis. This significantly improved response times for product listing and order APIs.

---

### 37. What is JSONB and have you used it?

JSONB is a PostgreSQL data type that stores JSON data in a binary format, allowing indexing and efficient querying. We used JSONB for storing flexible metadata such as payment gateway responses and third-party API payloads where schema could vary.

---

## 🔹 Redis (Caching & Performance)

### 38. Why did you use Redis?

Redis was used to improve performance and reduce database load. Since Redis is an in-memory data store, it provides extremely fast read and write operations. In our system, Redis helped handle high traffic during product browsing and checkout.

---

### 39. What data did you cache?

We cached product listings, frequently accessed product details, user loyalty balances, and configuration data. This reduced repeated database queries and improved API response times significantly.

---

### 40. How do you choose cache keys?

Cache keys were designed to be predictable and unique. For example, product-related cache keys included product IDs, and user-related keys included user IDs. This made cache invalidation and debugging easier.

---

### 41. What is cache invalidation and how did you handle it?

Cache invalidation means removing or updating cached data when the source data changes. In our project, whenever a product price or inventory changed, we invalidated the related cache entries. This ensured users always saw correct and updated information.

---

### 42. Difference between Redis and Memcached?

Redis supports advanced data structures, persistence, and pub-sub mechanisms, while Memcached is a simple key-value store. We chose Redis because we needed more flexibility, reliability, and support for background tasks and rate limiting.

---

### 43. How does Redis help in rate limiting?

Redis can track request counts per user or IP using counters with expiration. This allowed us to prevent abuse of APIs such as checkout and payment endpoints by limiting the number of requests within a time window.

---

### 44. How do you prevent stale data?

We used short TTLs for volatile data and explicit cache invalidation after updates. For critical data like inventory and pricing, we always validated against the database during checkout.

---

### 45. How did Redis improve performance in your project?

Redis reduced database load, improved response times, and allowed the system to scale better during peak traffic. Product listing APIs saw significant latency reduction after caching was introduced.

---

## 🔹 Celery & Background Tasks

### 46. Why did you use Celery?

Celery was used to handle time-consuming tasks asynchronously so that APIs could respond quickly. Tasks like sending emails, syncing inventory, and updating shipment status were moved to background workers.

---

### 47. Difference between synchronous and asynchronous tasks?

Synchronous tasks block the request until completion, while asynchronous tasks run in the background. In our project, checkout APIs remained fast because heavy operations were handled asynchronously using Celery.

---

### 48. What message broker did you use?

We used Redis as the message broker because it was already part of our infrastructure and provided good performance and reliability for task queues.

---

### 49. How do you retry failed tasks?

Celery supports automatic retries with configurable retry limits and delays. We used retries for tasks like external API calls and email sending to handle temporary failures.

---

### 50. How do you schedule periodic tasks?

We used Celery Beat to schedule periodic tasks such as expiring loyalty points, syncing inventory, and cleaning up old data.

---

### 51. How do you ensure task idempotency?

Tasks were designed so that running them multiple times did not cause incorrect results. For example, before sending an email or updating an order, we checked the current state to avoid duplicate actions.

---

### 52. What happens if a task crashes midway?

If a task crashes, Celery marks it as failed and retries it based on configuration. Since tasks were idempotent, retries did not cause data corruption.

---

## 🔹 Payment Gateway & Security

### 53. How does a payment gateway integration work?

The backend creates a payment request and sends it to the payment gateway. The user completes payment on the gateway side, and the gateway sends a callback or webhook to confirm the payment. Based on this confirmation, we update the order status in our system.

---

### 54. How do you ensure secure checkout?

We enforced HTTPS, validated all inputs, verified payment signatures, and never stored sensitive card data. Security checks were applied at every step of the checkout flow.

---

### 55. What is PCI-DSS?

PCI-DSS is a security standard for handling card payment data. Since we did not store or process card details directly, compliance was handled by the payment gateway, reducing our security risk.

---

### 56. How do you validate payment callbacks and webhooks?

We verified the authenticity of callbacks using signatures or HMAC verification. We also checked order IDs, payment amounts, and transaction status before updating our records.

---

### 57. How do you prevent duplicate payments?

We used unique transaction IDs and idempotency checks. If the same payment callback was received multiple times, the system processed it only once.

---

### 58. How do you handle payment failures?

If a payment failed, the order was marked as failed or pending, and inventory and loyalty points were restored. Users were notified so they could retry payment.

---

### 59. How do you handle refunds?

Refunds were initiated through the payment gateway APIs. Once the refund was confirmed, we updated the order status and restored loyalty points if applicable.

---

## 🔹 Third-Party API Integration (Logistics)

### 60. How do you integrate third-party APIs?

We used REST APIs provided by logistics partners. Integration involved authentication, request validation, response handling, and error management.

---

### 61. How do you handle API timeouts and failures?

We implemented timeouts, retries, and fallback mechanisms. If an API failed temporarily, Celery retried the request instead of blocking user requests.

---

### 62. How do you log external API errors?

All external API errors were logged with request and response details. This helped in debugging and monitoring integration issues.

---

### 63. How do you handle API version changes?

We isolated third-party API logic in separate services. This made it easier to upgrade or change API versions without impacting core business logic.

---

### 64. How do you secure API credentials?

API keys and secrets were stored in environment variables or secure AWS services. They were never hardcoded or exposed in logs.

---

### 65. How do you sync shipment status?

Shipment status updates were received via webhooks or periodic API polling. These updates triggered background tasks to update order status and notify users.

---
---

## 🔹 AWS & Deployment

### 66. Which AWS services did you use in this project?

In this project, we used multiple AWS services to build a scalable and reliable system. EC2 was used to run the Django application servers. RDS with PostgreSQL was used for the primary database. S3 was used to store static files, media files, and documents like invoices. CloudWatch was used for logging, monitoring, and setting up alerts. These services together helped us manage infrastructure efficiently without worrying too much about low-level server maintenance.

---

### 67. How did you deploy Django on AWS?

We deployed Django on EC2 instances behind a load balancer. The application was served using a production-grade web server setup, typically Nginx with a WSGI server like Gunicorn. Environment variables were configured on the server for secrets and configuration. Database connections were pointed to RDS, and static files were served from S3. This setup allowed us to scale horizontally by adding more EC2 instances when traffic increased.

---

### 68. What is S3, EC2, RDS, and CloudWatch?

EC2 is a virtual server that runs our application code. RDS is a managed database service that handles backups, replication, and maintenance for PostgreSQL. S3 is an object storage service used for storing static assets and user-uploaded files. CloudWatch is used for monitoring logs, metrics, and setting alerts for CPU usage, memory, and application errors.

---

### 69. How do you manage environment variables?

Environment variables are used to store sensitive and environment-specific data such as database credentials, API keys, and secret keys. We never store these values in code. In AWS, we managed them through EC2 environment configuration and secure storage solutions, ensuring the same codebase works across development, staging, and production.

---

### 70. How do you scale the application?

The application was designed to scale horizontally. We added more EC2 instances behind a load balancer as traffic increased. Redis and Celery workers were scaled separately based on background task load. Caching helped reduce database load, and read-heavy endpoints were optimized to handle high traffic.

---

### 71. How do you handle logs and monitoring?

Application logs, error logs, and access logs were sent to CloudWatch. We monitored key metrics such as response time, error rates, and CPU usage. Alerts were configured so that the team was notified immediately if something went wrong in production.

---

### 72. How do you secure AWS resources?

We followed the principle of least privilege using IAM roles and policies. Only required permissions were granted to services and users. Security groups restricted inbound and outbound traffic. All sensitive data was encrypted at rest and in transit.

---

### 73. How do you handle backups?

RDS automated backups were enabled for the database. Snapshots were taken regularly. S3 data was stored with versioning enabled. This ensured that data could be restored in case of failures or accidental deletions.

---

### 74. What is IAM?

IAM stands for Identity and Access Management. It is used to manage users, roles, and permissions in AWS. IAM helped us control who could access EC2, RDS, S3, and other AWS services securely.

---

### 75. How do you manage zero-downtime deployments?

We used rolling deployments where new versions of the application were deployed gradually while old instances were still serving traffic. Database migrations were backward-compatible to avoid breaking running services. This ensured minimal or no downtime during deployments.

---

## 🔹 System Design (Senior-Level Questions)

### 76. Design a high-traffic e-commerce backend.

To design a high-traffic e-commerce backend, I would start with a stateless API-based architecture. Django REST APIs would handle requests, with authentication using JWT. The database would be PostgreSQL for strong consistency. Redis would be used for caching and rate limiting. Background tasks such as emails, inventory sync, and notifications would be handled by Celery. Load balancers and horizontal scaling would ensure the system can handle peak traffic.

---

### 77. Design a loyalty points system.

A loyalty points system should be ledger-based. Every credit and debit should be stored as a transaction instead of directly updating a balance. This provides traceability and makes rollback easier. Points operations should be wrapped in database transactions to ensure consistency, especially during checkout and refunds.

---

### 78. How would you scale order processing?

Order processing can be scaled by separating order creation from post-order tasks. The core order placement should be fast and synchronous, while tasks like notifications, shipment creation, and analytics should be asynchronous using Celery. Database indexes and partitioning can be used as order volume grows.

---

### 79. How would you design inventory locking?

Inventory locking should be done at the database level using row-level locks. During checkout, stock should be checked and reserved inside a transaction. If payment succeeds, the reservation is confirmed; otherwise, it is released. This prevents overselling during concurrent checkouts.

---

### 80. How would you handle flash sales?

For flash sales, caching and rate limiting are critical. Inventory counters can be managed using Redis for speed, while the final confirmation happens in the database. This hybrid approach helps handle massive traffic spikes without overwhelming the database.

---

### 81. How would you design real-time order tracking?

Real-time order tracking can be implemented using webhook updates from logistics partners and background workers. The backend updates order status asynchronously, and clients can poll or subscribe to updates using WebSockets or push notifications.

---

### 82. How would you prevent fraud?

Fraud prevention involves multiple layers such as rate limiting, transaction monitoring, unusual behavior detection, and strict validation of payment callbacks. Logs and alerts help quickly detect and respond to suspicious activity.

---

### 83. How would you make the system fault-tolerant?

Fault tolerance is achieved by designing the system to expect failures. This includes retries, timeouts, circuit breakers, background processing, and graceful degradation. Using managed services like RDS and Redis also improves reliability.

---

### 84. How would you handle millions of users?

Handling millions of users requires horizontal scaling, aggressive caching, database optimization, and asynchronous processing. Read-heavy operations should be offloaded to caches, and background tasks should handle non-critical work.

---

### 85. What would you improve in your current architecture?

If I were to improve the system today, I would further decouple services using event-driven architecture, improve observability, and introduce better automation around scaling and deployments.

---
---

## 🔹 Behavioral / Experience-Based Questions

### 86. What was the biggest challenge in this project?

The biggest challenge in this project was maintaining data consistency across multiple systems like orders, inventory, payments, and loyalty points, especially under high traffic. A single checkout operation touched several components, and any failure could lead to incorrect stock or point deductions. Solving this required careful use of database transactions, proper error handling, and background task coordination. This challenge taught me how important system thinking is in real production environments.

---

### 87. How did you handle production issues?

When production issues occurred, the first step was always to stabilize the system. We checked logs and monitoring dashboards to understand the impact. If the issue was critical, we rolled back the recent deployment or disabled the problematic feature using configuration flags. Once the system was stable, we performed a root cause analysis and applied a permanent fix. Clear communication with the team was also very important during these incidents.

---

### 88. Describe a bug you fixed under pressure.

Once, we faced an issue where inventory was getting deducted multiple times for the same order due to repeated payment callbacks from the gateway. This happened under heavy traffic. I quickly identified that the callback handling was not idempotent. I fixed it by adding checks based on transaction IDs and order status before processing the callback. This immediately stopped the issue and prevented further data inconsistency.

---

### 89. How did you improve performance in this project?

Performance improvements came from identifying bottlenecks using logs and monitoring tools. We optimized database queries, added proper indexes, and introduced Redis caching for frequently accessed data. We also moved time-consuming tasks like email notifications and shipment updates to Celery background jobs. These changes significantly reduced API response times and improved system stability.

---

### 90. How do you collaborate with frontend teams?

I collaborate closely with frontend teams by clearly defining API contracts, request/response formats, and error handling. We regularly sync to discuss requirements and edge cases. During development, I ensure APIs are predictable and well-documented so frontend integration is smooth and fast.

---

### 91. How do you handle unclear requirements?

When requirements are unclear, I ask clarifying questions early and try to understand the business intent behind the feature. I often propose a simple initial solution and validate it with stakeholders before implementation. This avoids rework and ensures we build something that actually solves the problem.

---

### 92. How do you prioritize tasks?

Task prioritization is based on business impact and urgency. Issues affecting payments, orders, or customer experience are always top priority. I also consider dependencies and effort, so critical paths are addressed first while less urgent improvements are scheduled later.

---

### 93. What trade-offs did you make in this project?

One trade-off was choosing faster delivery over building a fully microservice-based system. We kept a modular monolith architecture initially to reduce complexity and speed up development. This allowed us to move quickly while still keeping the codebase organized and scalable for future refactoring.

---

### 94. What would you redesign today?

If I were redesigning the system today, I would introduce more event-driven components using message queues. This would further decouple systems like orders, loyalty, and notifications, making the platform more scalable and resilient.

---

### 95. What did you learn from this project?

This project taught me how real-world systems behave under load and failure. I learned how to design for scale, handle production incidents, and balance technical perfection with business needs. It also improved my confidence in building and maintaining large, production-grade backend systems.

---

### 96. Why are you confident explaining this project in interviews?

I am confident because I worked on core backend features like order processing, loyalty logic, performance optimization, and third-party integrations. I understand not just how the code works, but why certain decisions were made. This allows me to explain the system clearly and answer follow-up questions in depth.

---
