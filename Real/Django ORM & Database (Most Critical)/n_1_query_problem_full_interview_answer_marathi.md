# How did you identify and fix N+1 query problems? (Marathi)

> खाली दिलेले उत्तर **पूर्ण `.md` format मध्ये**, **theory + practical + code + explanation + interview-ready structure** सह आहे.  
> हे answer थेट copy–paste करून interview preparation साठी वापरता येईल आणि **4 years Django experience justify** करेल.

---

## प्रश्न
**How did you identify and fix N+1 query problems?**

---

## N+1 Query Problem म्हणजे काय? (थोडक्यात Theory)

N+1 query problem तेव्हा होतो जेव्हा:
- 1 query main table साठी execute होते
- त्यानंतर N queries related table साठी execute होतात

हे सहसा Django ORM मध्ये **loop च्या आत related objects access केल्यामुळे** होते.

---

## Context (परिस्थिती)

Production मधील एका API मध्ये orders ची list आणि त्यासोबत user details दाखवली जात होती.  
डेटा वाढल्यानंतर API खूप slow झाला होता.

---

## Identification (N+1 Problem कसा ओळखला?)

मी खालील पद्धती वापरून N+1 problem identify केला:

1. **Django Debug Toolbar (Local Environment)**
   - एकाच प्रकारची SQL query अनेक वेळा दिसत होती

2. **Production Query Logs**
   - Same SELECT query वारंवार repeat होत होती

3. **Code Review**
   - Loop च्या आत related fields access होत होते (`order.user`, `order.items`)

---

## Problematic Code (चुकीचा कोड)

```python
orders = Order.objects.all()

for order in orders:
    print(order.user.email)
```

### Query Breakdown
जर 100 orders असतील तर:
- 1 query → orders table
- 100 queries → user table

➡️ एकूण **101 SQL queries**

---

## Root Cause (मुळ कारण)

- Django ORM चा **lazy loading behavior**
- Related object (`user`) loop मध्ये access केला गेला
- Database JOIN आधीच वापरला नव्हता

---

## Solution (Fix कसा केला?)

ForeignKey / OneToOne संबंधांसाठी मी `select_related()` वापरले.

### Optimized Code

```python
orders = Order.objects.select_related('user')

for order in orders:
    print(order.user.email)
```

### Resulting Queries
- 1 SQL query with JOIN
- Loop मध्ये अतिरिक्त DB hit नाही

---

## Many-to-Many / Reverse FK Case

Many-to-Many किंवा reverse ForeignKey साठी `prefetch_related()` वापरले.

### Example

```python
orders = Order.objects.prefetch_related('items')

for order in orders:
    for item in order.items.all():
        print(item.name)
```

Django internally:
- 1 query → orders
- 1 query → items
- Python मध्ये mapping

---

## Result (परिणाम)

- Query count: **100+ → 2**
- API response time significantly कमी झाला
- Database load कमी झाला

---

## Validation (Production मध्ये तपासणी)

- Django Debug Toolbar वापरून query count verify केला
- Production logs मध्ये repeated queries बंद झाल्या
- Peak traffic मध्ये API stable राहिला

---

## Key Takeaways (महत्त्वाचे मुद्दे)

- Loop + related object access = **N+1 risk**
- ForeignKey / OneToOne → `select_related`
- ManyToMany / reverse FK → `prefetch_related`
- ORM code clean दिसत असला तरी performance issues असू शकतात

---

## Interview Tip

Interview मध्ये हे answer सांगताना:
- Queries count explain करा
- Before vs After comparison करा
- Which ORM method why वापरली ते स्पष्ट सांगा

👉 हे उत्तर confidently सांगितल्यास **4 years Django experience justify** करता येते.

