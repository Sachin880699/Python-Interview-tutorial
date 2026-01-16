# Interview Questions & Answers – Django Backend (Simple Language)

This document contains **simple interview questions and answers** based on your resume points.
Use this for **quick revision** and **confidence building**.

---

## 1. What is the "Miles Plus Cash" feature?

**Answer:**
Miles Plus Cash is a flexible payment feature that allows users to complete a purchase by using a combination of reward points and a credit card. Instead of forcing users to pay fully with points or fully with cash, the system intelligently applies available reward points first (up to an allowed limit) and then charges the remaining amount to the credit card.

From a backend perspective, this feature required careful handling of payment flow, reward balance validation, and transaction safety to ensure that points and money were deducted correctly.

---

## 2. How does the system decide how much to pay using points and cash?

**Answer:**
The system first validates the user’s reward point balance. Based on predefined business rules (such as maximum points allowed per transaction), it calculates how many points can be applied.

If the user has sufficient points, they are partially or fully applied. Any remaining amount is automatically sent to the credit card payment gateway. This logic is handled at the backend to ensure accuracy and consistency.

---

## 3. How did you handle payment failure scenarios?

**Answer:**
Payment failures were handled carefully to avoid incorrect point deductions. If the credit card payment failed, the reward points were not permanently deducted.

We used transaction handling and status checks so that reward point deduction was confirmed only after the payment gateway returned a successful response. This ensured financial safety and user trust.

---

## 4. How did you prevent duplicate payment or double deduction?

**Answer:**
To prevent duplicate payments, each transaction was assigned a unique payment ID. Backend APIs checked this ID before processing any request.

Database-level transactions and idempotency checks ensured that repeated requests (due to retries or network issues) did not result in double charging or double point deduction.

---

## 5. How did you design APIs to fetch product details?

**Answer:**
I designed REST APIs that return product details such as name, price, brand, category, and availability. These APIs were optimized to return only required fields needed by the frontend.

Pagination and efficient serializers were used to reduce response size and improve speed, especially for product listing pages.

---

## 6. How did you ensure fast loading times for product pages?

**Answer:**
Fast loading was achieved by optimizing database queries, using caching for frequently accessed product data, and minimizing unnecessary joins.

I also reduced payload size by returning only essential data and ensured APIs were efficiently structured for frontend consumption.

---

## 7. How did you implement product search functionality?

**Answer:**
Product search was implemented using Django ORM and REST APIs. Users could search products using filters such as category, price range, and brand.

The backend dynamically built querysets based on user input, ensuring accurate and relevant search results.

---

## 8. How did you optimize search performance?

**Answer:**
Search performance was optimized using database indexing on frequently filtered fields like category and price.

I also used optimized ORM queries, avoided unnecessary joins, and tested performance with large datasets to ensure fast response times.

---

## 9. What challenges did you face while implementing search filters?

**Answer:**
The main challenge was handling multiple filters together without slowing down the query.

I solved this by building dynamic querysets, optimizing database indexes, and validating filter inputs to avoid expensive queries.

---

## 10. How did you support backend-to-frontend integration for Lex portal?

**Answer:**
I worked closely with frontend teams to ensure APIs returned clean, structured, and consistent data.

I supported REST API consumption by documenting endpoints, handling edge cases, and ensuring backward compatibility during updates.

---

## 11. How did you improve data rendering performance?

**Answer:**
Data rendering performance was improved by reducing API response size and optimizing serializers.

I ensured that only required fields were sent to the frontend, which reduced processing time and improved UI responsiveness.

---

## 12. How did you handle high-traffic backend systems?

**Answer:**
To handle high traffic, I designed APIs to be stateless and scalable.

Caching, optimized database queries, and efficient error handling helped maintain stable performance during peak usage.

---

## 13. How did you ensure secure API communication?

**Answer:**
I secured APIs using authentication and authorization mechanisms.

HTTPS, input validation, and proper error handling were used to protect sensitive data and prevent misuse.

---

## 14. How did you test these APIs?

**Answer:**
APIs were tested using unit tests and manual testing tools like Postman.

Test cases covered success scenarios, edge cases, and failure handling to ensure reliability.

---

## 15. What would you improve if you had more time?

**Answer:**
With more time, I would add more automated tests, improve monitoring and logging, and further enhance caching strategies.

I would also review the architecture for future scalability and maintainability.
