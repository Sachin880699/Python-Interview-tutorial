# When did you choose raw SQL instead of Django ORM? (Marathi)

> खाली दिलेले उत्तर **पूर्ण `.md` format मध्ये**, **theory + real production use-case + code + explanation + interview-ready structure** सह आहे.  
> हे answer थेट copy–paste करून interview preparation साठी वापरता येईल आणि **4+ years Django experience justify** करेल.

---

## प्रश्न
**When did you choose raw SQL instead of Django ORM?**

---

## Django ORM vs Raw SQL (Theory)

### Django ORM कधी best असतो?
- CRUD operations
- Simple joins
- Maintainable आणि readable code
- Security (SQL injection protection)

### Raw SQL कधी योग्य ठरतो?
- Complex joins / subqueries
- Performance-critical reporting queries
- Database-specific features (CTE, window functions)
- ORM ने inefficient SQL generate केल्यावर

👉 Senior developer म्हणून **ORM first, Raw SQL last option** हा approach अपेक्षित असतो.

---

## Context (परिस्थिती)

Production मधील एका **financial reporting API** मध्ये:
- Monthly revenue report
- User-wise totals
- Ranking based on revenue

हा API मोठ्या dataset वर (millions of rows) चालत होता.

---

## Problem (समस्या)

ORM वापरून query लिहिल्यावर:
- Multiple annotations
- Nested subqueries
- Generated SQL खूप complex आणि slow

### ORM Code (Inefficient)

```python
from django.db.models import Sum

Order.objects.values('user_id') \
    .annotate(total_amount=Sum('amount')) \
    .order_by('-total_amount')
```

### Issue
- Query execution time: **8–10 seconds**
- Database CPU spike
- Query planner inefficient execution plan निवडत होता

---

## Identification (Raw SQL का निवडलं?)

मी खालील गोष्टी लक्षात घेतल्या:
- ORM SQL debug करून actual SQL तपासली
- `EXPLAIN ANALYZE` वापरून execution plan पाहिला
- ORM query optimize करूनही अपेक्षित performance मिळत नव्हती

➡️ त्या point ला **Raw SQL justified** होतं.

---

## Solution: Raw SQL वापरण्याचा निर्णय

### Raw SQL Query

```python
query = """
SELECT user_id, SUM(amount) AS total_amount
FROM orders
WHERE status = 'PAID'
GROUP BY user_id
ORDER BY total_amount DESC
LIMIT 100;
"""

results = Order.objects.raw(query)
```

### Improvements
- Database-level aggregation fully controlled
- Cleaner execution plan
- Indexes effectively वापरले गेले

---

## Result (परिणाम)

- Query time: **10 seconds → ~700 ms**
- Database CPU usage stable
- API response predictable झाला
- Business reports fast generate होऊ लागले

---

## Safety Measures (Important for Interview)

Raw SQL वापरताना मी खालील गोष्टी पाळल्या:

- ❌ User input direct SQL मध्ये वापरला नाही
- ✅ Parameterized queries वापरल्या
- ✅ SQL isolated ठेवली (service layer)
- ✅ Clear documentation लिहिली

---

## When NOT to use Raw SQL

- Simple CRUD queries
- Business logic heavy operations
- जे ORM easily handle करू शकतो

ORM maintainability जास्त असते.

---

## Key Takeaways (महत्त्वाचे मुद्दे)

- Django ORM powerful आहे पण सर्व problems साठी नाही
- Raw SQL हा **last-resort performance tool** आहे
- Decision data-driven असावा, preference-driven नाही
- हा answer **senior Django mindset** दाखवतो

---

## Interview Tip

Interview मध्ये हे answer सांगताना:
- ORM optimize केल्यावरही issue का राहिला ते explain करा
- EXPLAIN ANALYZE mention करा
- Raw SQL वापरण्याची maturity दाखवा

👉 हे उत्तर confidently सांगितल्यास **4+ years Django experience strongly justify** होते.
