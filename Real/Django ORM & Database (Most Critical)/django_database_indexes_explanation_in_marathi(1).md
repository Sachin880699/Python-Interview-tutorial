# Django मधील Database Indexes

## प्रश्न: *What indexes did you add and how did you decide?*

हा प्रश्न सहसा Django प्रोजेक्टच्या **performance optimization**, **database design**, किंवा **interview** मध्ये विचारला जातो. खाली याचे **पूर्ण स्पष्टीकरण**, **code examples**, आणि **theory** मराठीमध्ये दिले आहे.

---

## 1. Database Index म्हणजे काय?

Database Index म्हणजे **डेटाबेसमधील शोध जलद (fast) करण्यासाठी वापरली जाणारी data structure**.

सोप्या भाषेत:
- Index = पुस्तकाच्या शेवटी असलेली *अनुक्रमणिका*
- Index नसल्यास = पूर्ण पुस्तक वाचावे लागते (Full Table Scan)

### Index का वापरतात?
- `SELECT`, `WHERE`, `ORDER BY`, `JOIN` queries जलद होतात
- Large tables मध्ये performance सुधारते

### Index चे तोटे
- `INSERT`, `UPDATE`, `DELETE` थोडे slow होतात
- Extra storage लागते

---

## 2. Django मध्ये Index कसा काम करतो?

Django ORM वापरून आपण:
- Field level index
- Composite (multi-column) index
- Unique index
- ForeignKey index

define करू शकतो.

---

## 3. मी कोणते Indexes add केले?

खाली एक **realistic Django example** दिला आहे.

### Example Model: `Order`

```python
from django.db import models
from django.contrib.auth.models import User

class Order(models.Model):
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    order_id = models.CharField(max_length=50, unique=True)
    status = models.CharField(max_length=20)
    created_at = models.DateTimeField(auto_now_add=True)

    class Meta:
        indexes = [
            models.Index(fields=['status']),
            models.Index(fields=['created_at']),
            models.Index(fields=['user', 'status']),
        ]
```

---

## 4. कोणते Indexes add केले आणि का?

### 1️⃣ `order_id` (Unique Index)

```python
order_id = models.CharField(max_length=50, unique=True)
```

**का?**
- `order_id` वापरून order शोधली जाते
- प्रत्येक order unique असते
- Django automatically **unique index** create करतो

📌 Query example:
```sql
SELECT * FROM order WHERE order_id = 'ORD123';
```

---

### 2️⃣ `status` Field वर Index

```python
models.Index(fields=['status'])
```

**का?**
- Orders `PENDING`, `COMPLETED`, `CANCELLED` अशा status ने filter होतात
- Admin dashboard मध्ये frequently वापर

📌 Query example:
```python
Order.objects.filter(status='COMPLETED')
```

---

### 3️⃣ `created_at` Field वर Index

```python
models.Index(fields=['created_at'])
```

**का?**
- Date range queries खूप common असतात
- Reports आणि analytics साठी वापर

📌 Query example:
```python
Order.objects.filter(created_at__gte=start_date)
```

---

### 4️⃣ Composite Index (`user`, `status`)

```python
models.Index(fields=['user', 'status'])
```

**का?**
- Specific user चे specific status orders शोधले जातात

📌 Query example:
```python
Order.objects.filter(user=request.user, status='PENDING')
```

➡️ Composite index मुळे database एकाच वेळी दोन columns वापरतो

---

## 5. Index add करण्याचा निर्णय कसा घेतला?

### 🔍 1. Query Patterns Analyze केल्या
- कोणते filters जास्त वापरले जातात?
- कोणते fields `WHERE` clause मध्ये आहेत?

---

### 🐢 2. Slow Queries ओळखल्या

```python
from django.db import connection
print(connection.queries)
```

किंवा production मध्ये:
- Django Debug Toolbar
- Database EXPLAIN ANALYZE

---

### 📊 3. Table Size विचारात घेतला
- Small table → index ची गरज नाही
- Large table (1L+ records) → index आवश्यक

---

### ⚖️ 4. Read vs Write Ratio
- Read-heavy app → जास्त indexes OK
- Write-heavy app → मर्यादित indexes

---

## 6. Django मध्ये Index चे Types (Theory)

### 1. Single Field Index
```python
models.Index(fields=['email'])
```

### 2. Composite Index
```python
models.Index(fields=['user', 'created_at'])
```

### 3. Unique Index
```python
models.CharField(unique=True)
```

### 4. ForeignKey Index
- Django automatically create करतो

---

## 7. Interview Answer (Short Version)

> *I added indexes on frequently queried fields such as status, created_at, and composite indexes on user and status. I decided based on query patterns, slow query analysis, and table size to improve read performance while balancing write cost.*

---

## 8. निष्कर्ष (Conclusion)

- Index = Performance boost tool
- Blindly index लावू नये
- Query usage समजून index design करावा
- Django ORM index support खूप powerful आहे

---

✅ **हा .md file interview, documentation, किंवा assignment साठी वापरता येईल**

जर हवे असल्यास:
- PostgreSQL specific indexes
- Real production case study
- EXPLAIN ANALYZE output

ते सुद्धा तयार करून देऊ शकतो 🙂


---

# 9. Have you optimized annotation or aggregation queries?

## उत्तर (Django – Marathi Explanation)

होय, मी Django मध्ये **annotation** आणि **aggregation queries optimize केल्या आहेत**, कारण large datasets वर या queries performance bottleneck बनू शकतात.

---

## 9.1 Annotation म्हणजे काय?

`annotation` वापरून आपण **प्रत्येक row साठी calculated field** add करतो.

### Example:
```python
from django.db.models import Count

Order.objects.annotate(item_count=Count('order_items'))
```

➡️ प्रत्येक Order साठी `item_count` calculate होतो

---

## 9.2 Aggregation म्हणजे काय?

`aggregate` वापरून आपण **पूर्ण queryset वर summary value** काढतो.

### Example:
```python
from django.db.models import Sum

Order.objects.aggregate(total_amount=Sum('amount'))
```

➡️ संपूर्ण table वर एकच result

---

## 9.3 Annotation / Aggregation Slow का होतात?

- Large tables (lakhs of rows)
- JOIN असलेले relations
- Unindexed foreign keys
- Unnecessary calculations
- Python-side processing

---

## 9.4 मी Annotation Queries कशा Optimize केल्या?

### ✅ 1. Database Level Calculation वापरली

❌ Wrong (Python side):
```python
orders = Order.objects.all()
for o in orders:
    o.total = o.items.count()
```

✅ Correct (DB side):
```python
from django.db.models import Count

Order.objects.annotate(total_items=Count('items'))
```

➡️ एकाच SQL query मध्ये calculation

---

### ✅ 2. Required Fields वरच Annotation

❌ Unnecessary annotation:
```python
Order.objects.annotate(total=Sum('items__price'))
```

✅ Optimized:
```python
Order.objects.filter(status='COMPLETED').annotate(
    total=Sum('items__price')
)
```

➡️ आधी filter, मग annotate

---

### ✅ 3. Proper Indexes वापरले

```python
class OrderItem(models.Model):
    order = models.ForeignKey(Order, on_delete=models.CASCADE, db_index=True)
```

➡️ JOIN आणि COUNT fast झाले

---

### ✅ 4. `values()` + `annotate()` Combination

```python
Order.objects.values('status').annotate(count=Count('id'))
```

➡️ GROUP BY database मध्ये efficiently execute

---

### ✅ 5. `select_related` / `prefetch_related`

❌ N+1 problem:
```python
for o in Order.objects.all():
    print(o.user.username)
```

✅ Optimized:
```python
Order.objects.select_related('user')
```

---

## 9.5 Aggregation Queries कशा Optimize केल्या?

### ✅ 1. Limited Dataset वर Aggregate

❌ Slow:
```python
Order.objects.aggregate(total=Sum('amount'))
```

✅ Fast:
```python
Order.objects.filter(status='COMPLETED').aggregate(total=Sum('amount'))
```

---

### ✅ 2. Database Index on Aggregated Fields

```python
class Order(models.Model):
    created_at = models.DateTimeField(db_index=True)
```

➡️ Date range aggregation fast

---

### ✅ 3. Avoid Repeated Aggregations

❌ Multiple calls:
```python
Order.objects.count()
Order.objects.aggregate(Sum('amount'))
```

✅ Single query pattern (where possible)

---

## 9.6 EXPLAIN ANALYZE वापरून Optimization

```sql
EXPLAIN ANALYZE
SELECT status, COUNT(*) FROM order GROUP BY status;
```

➡️ Query plan पाहून index missing आहे का ते ठरवले

---

## 9.7 Interview Ready Answer (Short)

> *Yes, I optimized annotation and aggregation queries by pushing calculations to the database, filtering before annotating, using proper indexes on foreign keys and grouped fields, and avoiding unnecessary joins. I also used select_related, prefetch_related, and EXPLAIN ANALYZE to identify bottlenecks.*

---

## 9.8 Conclusion

- Annotation = per-row calculation
- Aggregation = summary calculation
- Always filter before annotate
- Indexes are critical
- Database should do heavy work, not Python

---

✅ This section can be directly used in **interviews, system design rounds, and Django documentation**

