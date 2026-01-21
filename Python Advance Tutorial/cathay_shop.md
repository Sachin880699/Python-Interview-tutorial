# CATHAY SHOP – INTERVIEW EXPLANATION (Backend Role)

**Role:** Backend Developer (Python/Django)  
**Organization:** Infosys  
**Domain:** E-commerce Platform  
**Architecture:** Microservices-based, REST APIs  
**Tech Stack:** Python, Django, Django REST Framework, PostgreSQL, Redis, Celery, AWS

---

## 1. Project Overview

Cathay Shop is a high-traffic e-commerce platform where users browse products and place orders.
The system handles product catalog management, product search, inventory availability,
order processing, and integrations with external systems such as logistics and payment services.

I worked as a backend developer focusing on REST API development, performance optimization,
and backend–frontend integration within an existing enterprise architecture.

---

## 2. E-Commerce Architecture Contribution

- The platform followed a microservices-based architecture with separate services for
  product catalog, search, inventory, order management, and integrations.
- My role was to work within existing services and enhance backend functionality.
- Followed enterprise standards for logging, exception handling, and API validations.

**Day-to-Day Work**
- Understanding existing service flows
- Enhancing APIs based on business requirements
- Fixing bugs raised by QA or production support
- Coordinating with frontend teams for API changes

---

## 3. Product Catalog & Product Detail APIs

- Developed REST APIs for product listing and product detail pages.
- APIs returned structured data such as product name, description, images, price,
  and availability status.
- Focused on minimizing payload size for faster frontend rendering.

**Technical Work**
- Implemented serializers using Django REST Framework
- Optimized ORM queries using `select_related` and `prefetch_related`
- Added indexes on frequently queried fields

---

## 4. Product Search & Filtering

- Implemented backend APIs to support product search with:
  - Category-based filters
  - Brand-based filters
  - Price range filters
- Ensured search accuracy and low response times for large datasets.

**Day-to-Day Work**
- Writing dynamic Django querysets
- Optimizing database queries
- Handling edge cases like empty results and invalid filters
- Testing APIs with real production-like data

---

## 5. Inventory & Availability Management

- Worked on backend logic to ensure accurate product availability across the platform.
- Inventory validation was performed before order confirmation to prevent overselling.
- Stock updates were synchronized with order placement workflows.

**Responsibilities**
- Implementing inventory validation logic
- Ensuring consistency between displayed stock and actual inventory
- Fixing inventory mismatches reported during testing

---

## 6. Order Management System (OMS)

- Developed and optimized APIs for:
  - Order creation
  - Order status updates (placed, confirmed, shipped)
  - Order history retrieval
- Integrated order workflows with inventory and external services.

**Day-to-Day Work**
- Implementing REST endpoints
- Validating request payloads
- Handling failure scenarios and retries
- Supporting QA and UAT testing cycles

---

## 7. Performance Optimization (Redis & Celery)

- Used Redis caching for frequently accessed data such as product details and configurations.
- Implemented Celery background tasks for non-blocking operations like:
  - Email notifications
  - Inventory synchronization
  - Async updates

**Responsibilities**
- Identifying slow APIs
- Offloading heavy logic to background jobs
- Testing async task execution and error handling

---

## 8. Third-Party Integrations (Logistics)

- Integrated logistics and shipping provider APIs to:
  - Fetch real-time shipping rates
  - Track shipment status
- Implemented error handling, retries, and logging for external API failures.

---

## 9. Backend–Frontend Integration

- Worked closely with frontend developers to finalize API contracts.
- Resolved issues related to:
  - Incorrect response formats
  - Missing fields
  - API performance bottlenecks
- Supported frontend teams during releases and production deployments.

---

## 10. Typical Day on Cathay Shop

A typical day involved:
- Picking up backend tasks from Jira
- Working on API enhancements or bug fixes
- Testing APIs using Postman
- Coordinating with frontend teams for integration
- Participating in daily stand-ups and code reviews

---

## 11. Summary

Overall, my role on Cathay Shop focused on backend API development,
performance optimization, inventory and order workflows, and ensuring
reliable integrations in a production-grade e-commerce environment.
