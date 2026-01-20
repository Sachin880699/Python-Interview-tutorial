# Tell me about the slowest Django ORM query you fixed (Marathi)

> खाली दिलेले उत्तर **पूर्ण `.md` format मध्ये**, **theory + practical + code + explanation + interview-ready structure** सह आहे.  
> हे answer थेट copy–paste करून interview preparation साठी वापरता येईल.

---

## प्रश्न
**Tell me about the slowest Django ORM query you fixed.**

---

## Context (परिस्थिती)

Production मधील एका **admin dashboard API** मध्ये users सोबत खालील माहिती दाखवली जात होती:
- User details
- Order count
- Last order

हा API खूप frequently hit होत होता.

### Problem Statement
- Data वाढल्यानंतर API response time **5–6 सेकंद** झाला
- Business users कडून performance complaints येत होत्या

---

## Problem (समस्या काय होती?)

Debugging केल्यानंतर खालील issues दिसले:

- एका API request मध्ये **200+ SQL queries** execute होत होत्या
- Django ORM queries loop च्या आत चालत होत्या
- प्रत्येक user साठी वेगळी order query hit होत होती
- हा एक **classic N+1 query problem** होता

ORM code readable होता, पण internally खूप inefficient होता.

---

## Problematic Code (चुकीचा कोड)

```python
users = User.objects.all()

result = []
for user in users:
    orders = Order.objects.filter(user=user)
    result.append({
        "user_id": user.id,
        "email": user.email,
        "order_count": orders.count(),
        "last_order": orders.order_by('-created_at').first()
    })
```

### Query Breakdown
जर 100 users असतील तर:
- 1 query → users table
- 100 queries → order count
- 100 queries → last order

➡️ एकूण **201 SQL queries**

---

## Root Cause (मुळ कारण)

- Database-level aggregation Python loop मध्ये केली जात होती
- Django ORM चा **lazy evaluation** चुकीच्या पद्धतीने वापरला गेला
- Database ची ताकद (aggregation, joins) वापरली नव्हती

---

## Solution (उपाय काय केला?)

मी सर्व database काम **single optimized queryset** मध्ये हलवले.

### Optimized Code

```python
from django.db.models import Count, Max

users = (
    User.objects
    .annotate(
        order_count=Count('order'),
        last_order_date=Max('order__created_at')
    )
)
```

### Additional Optimizations
- `Order.created_at` वर **database index add केला**
- Loop मधील database calls पूर्णपणे काढून टाकल्या
- API वर **pagination implement** केली
- Unnecessary fields fetch होऊ नयेत म्हणून serializer optimize केला

---

## Result (परिणाम)

- Query count: **200+ → फक्त 2 queries**
- API response time:
  - आधी: **5–6 सेकंद**
  - नंतर: **~400 ms**
- Production database CPU usage लक्षणीयरीत्या कमी झाला
- Frontend team कडून performance complaints बंद झाल्या

---

## Validation (Production मध्ये तपासणी कशी केली?)

- Django Debug Toolbar वापरून query count verify केला
- Production logs आणि APM वापरून API latency monitor केली
- Peak traffic मध्ये API stable राहिली

---

## Key Takeaways (महत्त्वाचे मुद्दे)

- Django ORM code clean दिसत असला तरी performance खराब असू शकते
- Loop च्या आत ORM queries लिहिणे हा **सर्वात मोठा performance trap** आहे
- Aggregation, filtering शक्यतो **database मध्येच** करावी
- हा अनुभव **2 years आणि 4 years developer** मधला फरक स्पष्ट दाखवतो

---

## Interview Tip

Interview मध्ये हे उत्तर सांगताना:
- Numbers वापरा (queries, response time)
- Before vs After clearly explain करा
- Production impact आणि learning highlight करा

👉 हे उत्तर confidently सांगितल्यास **4 years Django experience justify** होते.
