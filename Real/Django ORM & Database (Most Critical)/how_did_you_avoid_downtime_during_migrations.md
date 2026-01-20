# How did you avoid downtime during migrations?

## उत्तर (Django – Marathi Explanation)

Production environment मध्ये migrations करताना **downtime टाळणे** हा सर्वात critical भाग असतो, विशेषतः जेव्हा database मध्ये **millions of records** असतात. मी खालील **zero-downtime migration strategy** वापरली.

---

## 1. Downtime का येतो?

Downtime सहसा खालील कारणांमुळे येतो:
- Blocking `ALTER TABLE` operations
- Long-running migrations
- Table locks during index creation
- Live traffic असताना schema changes

---

## 2. Zero-Downtime Migration Strategy (High Level)

मी migrations नेहमी **multiple safe steps** मध्ये केल्या:

1. Backward-compatible schema change
2. Code deploy (old + new schema support)
3. Background data backfill
4. Switch application logic
5. Cleanup migration

---

## 3. Step-by-Step Approach (Practical)

### Step 1️⃣ Nullable Column Add करणे

❌ Wrong (direct non-null):
```python
models.AddField(
    model_name='order',
    name='tracking_id',
    field=models.CharField(max_length=50),
)
```

✅ Correct (safe):
```python
models.AddField(
    model_name='order',
    name='tracking_id',
    field=models.CharField(max_length=50, null=True, blank=True),
)
```

➡️ Table rewrite टाळले

---

### Step 2️⃣ Backward-Compatible Code Deploy

- Code असा लिहिला की:
  - Field present असो किंवा नसो, app चालेल

```python
if hasattr(order, 'tracking_id'):
    use_tracking(order.tracking_id)
```

---

### Step 3️⃣ Background Data Backfill

❌ Migration मध्ये data update नाही

✅ Celery / management command वापरला

```python
for order in Order.objects.iterator(chunk_size=1000):
    order.tracking_id = generate_tracking()
    order.save(update_fields=['tracking_id'])
```

➡️ Live traffic block झाला नाही

---

### Step 4️⃣ Application Logic Switch

- Backfill complete झाल्यावर:
  - Code updated to rely on new column

---

### Step 5️⃣ Constraint Add करणे (Separate Migration)

```python
models.AlterField(
    model_name='order',
    name='tracking_id',
    field=models.CharField(max_length=50, null=False),
)
```

➡️ Safe कारण data आधीच आहे

---

## 4. Index Creation Without Downtime

### 🔴 Problem
- Index creation locks table

### ✅ Solution (PostgreSQL)

- `CONCURRENTLY` index creation

```sql
CREATE INDEX CONCURRENTLY idx_order_created_at
ON order(created_at);
```

➡️ Django मध्ये custom migration वापरली

---

## 5. Schema + Data Migration वेगळी ठेवली

❌ Wrong:
- Schema change + data update in same migration

✅ Correct:
- Schema migration
- Async data migration

---

## 6. Feature Flags वापरले

- New column / logic flag मागे ठेवली
- Gradual rollout

---

## 7. Off-Peak & Progressive Deployment

- Low traffic वेळेत migration
- Blue-Green / Rolling deploy

---

## 8. Rollback Safe Strategy

- Old code still works with new schema
- No destructive changes first

---

## 9. Interview Ready Answer (Short)

> *I avoided downtime during migrations by using zero-downtime migration strategies such as backward-compatible schema changes, adding nullable columns first, separating schema and data migrations, performing data backfills asynchronously, creating indexes concurrently, and deploying code that supports both old and new schemas until the migration is complete.*

---

## Conclusion

- Downtime-free migration = planning
- Never rush schema changes
- Database locks are enemy
- Django supports safe migrations if used correctly

---

✅ This file is ideal for **senior Django interviews, production discussions, and DevOps reviews**

