# How did you optimize a dashboard API that hit the database multiple times? (Marathi)

> खाली दिलेले उत्तर **पूर्ण `.md` format मध्ये**, **theory + practical examples + code + explanation + interview-ready structure** सह आहे.  
> हे answer थेट copy–paste करून interview preparation साठी वापरता येईल आणि **4 years Django experience justify** करेल.

---

## प्रश्न
**How did you optimize a dashboard API that hit the database multiple times?**

---

## Dashboard APIs का slow होतात? (Theory)

Dashboard APIs सहसा slow होण्याची कारणे:
- Multiple metrics साठी वेगवेगळ्या ORM queries
- Same table वर repeated hits
- Aggregation Python मध्ये केली जाते
- Caching नसते

Dashboard API हा सहसा **high-traffic endpoint** असतो, त्यामुळे optimization खूप critical असते.

---

## Context (परिस्थिती)

Production मधील एका **admin dashboard API** मध्ये खालील metrics दाखवायचे होते:
- Total users
- Active users
- Total orders
- Orders by status
- Today’s revenue

हा API प्रत्येक page load ला hit होत होता.

---

## Problem (समस्या)

Initial implementation मध्ये प्रत्येक metric साठी वेगळी query होती.

### Problematic Code

```python
from datetime import date

User.objects.count()
User.objects.filter(is_active=True).count()
Order.objects.count()
Order.objects.filter(status='PAID').count()
Order.objects.filter(created_at__date=date.today()).aggregate(total=Sum('amount'))
```

### Issue
- एका API call मध्ये **5–6 SQL queries**
- Data वाढल्यावर response time वाढत गेला
- Peak traffic मध्ये DB load खूप वाढला

---

## Identification (Problem कसा ओळखला?)

- Django Debug Toolbar वापरून query count तपासला
- Production APM मध्ये dashboard endpoint slow दिसत होता
- Logs मध्ये repeated similar queries दिसत होत्या

---

## Solution (Optimization कशी केली?)

### Step 1️⃣: Aggregation एकाच query मध्ये

```python
from django.db.models import Count, Sum

order_stats = (
    Order.objects
    .values('status')
    .annotate(count=Count('id'))
)
```

➡️ Multiple status queries → single aggregated query

---

### Step 2️⃣: Conditional Aggregation

```python
from django.db.models import Q

stats = Order.objects.aggregate(
    total_orders=Count('id'),
    paid_orders=Count('id', filter=Q(status='PAID')),
)
```

---

### Step 3️⃣: Avoid Repeated Queries

```python
users = User.objects.aggregate(
    total_users=Count('id'),
    active_users=Count('id', filter=Q(is_active=True))
)
```

---

### Step 4️⃣: Caching (Major Performance Boost)

```python
from django.core.cache import cache

cache_key = 'dashboard_stats'
data = cache.get(cache_key)

if not data:
    data = compute_dashboard_data()
    cache.set(cache_key, data, timeout=300)
```

➡️ Database hits drastically कमी झाले

---

## Result (परिणाम)

- SQL queries: **6–7 → फक्त 2**
- API response time:
  - आधी: **2.5–3 सेकंद**
  - नंतर: **<300 ms**
- Database CPU load कमी झाला
- Dashboard page instant load होऊ लागला

---

## Validation (Production मध्ये तपासणी)

- Django Debug Toolbar ने query count verify केला
- Production APM मध्ये latency graph stable झाला
- Cache hit ratio monitor केला

---

## Key Takeaways (महत्त्वाचे मुद्दे)

- Dashboard APIs साठी aggregation database मध्ये करा
- Same table वर repeated queries टाळा
- Caching हा dashboard optimization साठी best weapon आहे
- Optimization ही **query कमी करण्याची कला** आहे

---

## Interview Tip

Interview मध्ये हे answer सांगताना:
- Before vs After numbers सांगा
- Aggregation + caching combination explain करा
- Business impact highlight करा

👉 हे उत्तर confidently सांगितल्यास **4 years Django experience strongly justify** होते.
