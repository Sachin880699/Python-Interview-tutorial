# What issues did you face during large database migrations?

## उत्तर (Django – Marathi Explanation)

Large database migrations (लाखो / कोटी records असताना) करताना मला अनेक **real production issues** आले. खाली ते issues, त्यांची कारणे, आणि मी कसे handle केले ते सविस्तर दिले आहे.

---

## 1. Migration खूप Slow होणे

### 🔴 Issue
- `ALTER TABLE` किंवा `ADD COLUMN` migration तासन्‌तास चालणे
- Application downtime

### कारण
- Millions of rows असलेल्या table वर direct schema change

### ✅ Solution

- **Non-blocking migrations** वापरल्या
- Step-by-step migrations

```python
# Step 1: nullable column
models.AddField(
    model_name='order',
    name='tracking_id',
    field=models.CharField(max_length=50, null=True),
)
```

➡️ नंतर background job ने data backfill

---

## 2. Table Locking & Downtime

### 🔴 Issue
- Migration दरम्यान table lock
- APIs fail होणे

### कारण
- PostgreSQL / MySQL मध्ये heavy DDL operations

### ✅ Solution
- Off-peak deployment
- Smaller migrations
- PostgreSQL मध्ये `CONCURRENTLY` index

```python
models.Index(fields=['created_at'], name='idx_created_at')
```

➡️ (Production मध्ये custom SQL वापरला)

---

## 3. Data Backfill Memory Issue

### 🔴 Issue
- Data migrate करताना RAM overflow

### कारण
- `for obj in Model.objects.all()` वापरणे

### ✅ Solution

```python
for order in Order.objects.iterator(chunk_size=2000):
    order.new_field = calculate()
    order.save(update_fields=['new_field'])
```

➡️ Chunk-based processing

---

## 4. Migration Failure & Partial State

### 🔴 Issue
- Migration अर्धवट fail
- Inconsistent schema

### ✅ Solution
- Idempotent migrations
- Backup before migrate

```bash
python manage.py migrate app_name 0012 --fake
```

---

## 5. Adding Index on Huge Table

### 🔴 Issue
- Index creation खूप वेळ घेतो

### कारण
- Millions of existing rows

### ✅ Solution
- Index later, not with column add
- Separate migration

---

## 6. Foreign Key Constraint Failures

### 🔴 Issue
- Orphan records मुळे FK migration fail

### ✅ Solution

```sql
SELECT * FROM order_item
WHERE order_id NOT IN (SELECT id FROM order);
```

➡️ Data cleanup before constraint

---

## 7. Rollback Complexity

### 🔴 Issue
- Production rollback risky

### ✅ Solution
- Backward-compatible migrations
- Feature flags

---

## 8. Migration vs Live Traffic

### 🔴 Issue
- Live writes चालू असताना migration

### ✅ Solution
- Zero-downtime strategy

Steps:
1. Add nullable field
2. Deploy code
3. Backfill data
4. Make field non-null

---

## 9. Monitoring & Validation

### ✅ Tools
- Django logs
- DB slow query logs
- Row count validation

---

## 10. Interview Ready Answer (Short)

> *During large database migrations, I faced issues like slow migrations, table locking, memory spikes, and partial failures. I handled them using zero-downtime migration strategies, chunk-based data backfills, separating schema and data migrations, adding indexes concurrently, and validating data before applying constraints.*

---

## Conclusion

- Large migrations = planning required
- Never mix schema + heavy data change
- Chunking, async jobs, and backups are critical

---

✅ This file is suitable for **senior Django interviews, production postmortems, and system design discussions**

