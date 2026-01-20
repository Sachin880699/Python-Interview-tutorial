# How did you reduce query count in a single request? (Marathi)

> खाली दिलेले उत्तर **पूर्ण `.md` format मध्ये**, **theory + real production examples + code + explanation + interview-ready structure** सह आहे.  
> हे answer थेट copy–paste करून interview preparation साठी वापरता येईल आणि **4+ years Django experience justify** करेल.

---

## प्रश्न
**How did you reduce query count in a single request?**

---

## Query Count कमी करणं का महत्वाचं आहे? (Theory)

एका HTTP request मध्ये:
- जास्त queries → जास्त DB round-trips
- Latency वाढते
- DB load वाढतो

Senior Django developer म्हणून goal असतो:
> **Minimum queries, maximum work**

---

## Context (परिस्थिती)

Production मधील एका **order listing API** मध्ये:
- Orders
- User details
- Order items
- Order status summary

हे सगळं data एका API call मध्ये येत होतं.

---

## Initial Problem (समस्या)

Django Debug Toolbar वापरून दिसलं:
- एका request मध्ये **40–50 SQL queries**
- Performance inconsistent

---

## Step 1️⃣: N+1 Queries Remove करणे

### Problematic Code

```python
orders = Order.objects.all()
for order in orders:
    print(order.user.email)
```

### Fix

```python
orders = Order.objects.select_related('user')
```

➡️ Queries: **N+1 → 1**

---

## Step 2️⃣: Reverse & Many-to-Many Relations Optimize करणे

### Problematic Code

```python
for order in orders:
    items = order.items.all()
```

### Fix

```python
orders = Order.objects.prefetch_related('items')
```

➡️ Queries: **N → 2**

---

## Step 3️⃣: Aggregation एकाच Query मध्ये

### Before

```python
Order.objects.filter(status='PAID').count()
Order.objects.filter(status='PENDING').count()
```

### After

```python
from django.db.models import Count, Q

Order.objects.aggregate(
    paid=Count('id', filter=Q(status='PAID')),
    pending=Count('id', filter=Q(status='PENDING')),
)
```

➡️ Queries: **2 → 1**

---

## Step 4️⃣: Only Required Fields Fetch करणे

```python
orders = Order.objects.select_related('user').only(
    'id', 'status', 'user__email'
)
```

➡️ Data transfer + memory usage कमी

---

## Step 5️⃣: Duplicate Queries Cache करणे

```python
from django.core.cache import cache

data = cache.get('order_stats')
if not data:
    data = compute_order_stats()
    cache.set('order_stats', data, 300)
```

➡️ Repeated DB hits टळले

---

## Final Result (परिणाम)

- Queries: **45 → 3–4**
- API response time:
  - आधी: **1.8 सेकंद**
  - नंतर: **<300 ms**
- Database load stable झाला

---

## Validation (Production मध्ये तपासणी)

- Django Debug Toolbar वापरून query count verify केला
- Logs मध्ये duplicate queries गायब झाल्या
- APM मध्ये latency graph smooth झाला

---

## Key Takeaways (महत्त्वाचे मुद्दे)

- Query count कमी करणं म्हणजे ORM चा योग्य वापर
- `select_related` + `prefetch_related` हे primary tools
- Aggregation database मध्येच करावी
- Caching हा final performance booster

---

## Interview Tip

Interview मध्ये हे answer सांगताना:
- Step-by-step approach explain करा
- Numbers (queries, time) mention करा
- Decision-making process highlight करा

👉 हे उत्तर confidently सांगितल्यास **4+ years Django experience clearly justify** होते.

