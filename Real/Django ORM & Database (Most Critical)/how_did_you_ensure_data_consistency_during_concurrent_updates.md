# How did you ensure data consistency during concurrent updates?

## उत्तर (Django / Database – Marathi Explanation)

Concurrent updates म्हणजे **एकाच वेळी multiple users / processes एकाच data वर write करत असणे**. High-traffic Django applications मध्ये data consistency राखणे हे खूप critical असते. मी खालील **practical + production-proven strategies** वापरल्या.

---

## 1. Concurrent Updates मुळे काय Problems येतात?

- 🔴 Lost updates (एक update दुसऱ्यावर overwrite होणे)
- 🔴 Dirty reads / non-repeatable reads
- 🔴 Race conditions
- 🔴 Inconsistent aggregates (count, balance, stock)

---

## 2. Database Transactions वापरल्या

### ✅ Atomic Transactions (Most Important)

```python
from django.db import transaction

with transaction.atomic():
    order = Order.objects.get(id=order_id)
    order.status = 'CONFIRMED'
    order.save()
```

➡️ पूर्ण block एकाच transaction मध्ये execute होतो

---

## 3. Row-Level Locking (`select_for_update`)

### 🔴 Problem
- Multiple processes same row update करतात

### ✅ Solution

```python
from django.db import transaction

with transaction.atomic():
    product = Product.objects.select_for_update().get(id=product_id)
    product.stock -= 1
    product.save()
```

➡️ एक transaction complete होईपर्यंत दुसरा wait करतो

---

## 4. Optimistic Locking (Versioning)

### Use Case
- High-read, low-conflict systems

```python
class Order(models.Model):
    version = models.IntegerField(default=0)
```

```python
updated = Order.objects.filter(id=oid, version=old_version) \
    .update(status='DONE', version=old_version + 1)

if updated == 0:
    raise Exception('Concurrent update detected')
```

➡️ Silent overwrite टाळले

---

## 5. Database-Level Constraints

### ✅ Unique Constraints

```python
class Meta:
    constraints = [
        models.UniqueConstraint(fields=['user', 'date'], name='unique_user_date')
    ]
```

➡️ Duplicate writes रोखले

---

## 6. Atomic Updates with `F()` Expressions

### 🔴 Problem
- Read → modify → write race condition

### ✅ Solution

```python
from django.db.models import F

Product.objects.filter(id=pid).update(stock=F('stock') - 1)
```

➡️ Database स्वतः calculation करते

---

## 7. Isolation Levels (When Needed)

- PostgreSQL default: `READ COMMITTED`
- Critical flows साठी:
  - `REPEATABLE READ`
  - `SERIALIZABLE`

➡️ Rare cases मध्ये वापरले

---

## 8. Idempotent APIs

### 🔴 Problem
- Same request multiple times hit होणे

### ✅ Solution

- Idempotency keys
- Safe retries

---

## 9. Background Jobs Consistency

- Celery tasks retry-safe लिहिल्या
- Locks / unique task IDs

---

## 10. Monitoring & Detection

- Deadlock logs
- Transaction retry metrics
- Error alerts

---

## 11. Real Production Example

### Stock Management

```python
with transaction.atomic():
    item = Item.objects.select_for_update().get(id=item_id)
    if item.stock <= 0:
        raise OutOfStock()
    item.stock -= qty
    item.save()
```

➡️ Overselling टाळले

---

## 12. Interview Ready Answer (Short)

> *I ensured data consistency during concurrent updates by using database transactions, row-level locking with select_for_update, atomic updates using F expressions, optimistic locking where appropriate, and enforcing database-level constraints. For critical sections, I relied on the database rather than application-level locks and monitored for deadlocks and retries.*

---

## Conclusion

- Concurrency problems inevitable आहेत
- Database is the source of truth
- Atomicity + locking is key
- Django ORM provides strong tools if used correctly

---

✅ This file is ideal for **senior Django interviews, concurrency discussions, and production system design reviews**

