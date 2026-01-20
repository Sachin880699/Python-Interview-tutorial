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

