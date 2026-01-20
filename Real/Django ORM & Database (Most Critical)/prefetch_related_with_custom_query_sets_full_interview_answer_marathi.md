# Have you used prefetch_related() with custom querysets? Why? (Marathi)

> खाली दिलेले उत्तर **पूर्ण `.md` format मध्ये**, **theory + real practical example + code + explanation + interview-ready structure** सह आहे.  
> हे answer थेट copy–paste करून interview preparation साठी वापरता येईल आणि **4 years Django experience justify** करेल.

---

## प्रश्न
**Have you used prefetch_related() with custom querysets? Why?**

---

## prefetch_related() म्हणजे काय? (Theory)

`prefetch_related()` ही Django ORM method आहे जी:
- ManyToMany आणि reverse ForeignKey relations साठी वापरली जाते
- Separate SQL queries execute करते
- Python मध्ये related objects mapping करते

👉 Default `prefetch_related()` सर्व related records fetch करते, जे नेहमी efficient नसते.

---

## Custom QuerySet का वापरतो? (Why?)

Custom queryset वापरण्याची कारणे:
- Related table मधील **फक्त आवश्यक data** fetch करायचा असतो
- Unnecessary rows load होऊ नयेत
- Memory usage कमी करायची असते
- Business logic database level वर push करायची असते

---

## Context (परिस्थिती)

Production मधील एका API मध्ये:
- Orders list
- त्यासोबत order items
- पण **फक्त active items** दाखवायचे होते

Default `prefetch_related()` वापरल्यास inactive items सुद्धा येत होते.

---

## Problematic Code (चुकीचा approach)

```python
orders = Order.objects.prefetch_related('items')
```

### Issue
- Active + inactive items सगळे fetch होत होते
- Memory usage जास्त
- Serialization slow

---

## Solution: prefetch_related() with Custom QuerySet

Django चा `Prefetch` object वापरून custom queryset define केला.

### Optimized Code

```python
from django.db.models import Prefetch

active_items = Item.objects.filter(is_active=True)

orders = Order.objects.prefetch_related(
    Prefetch('items', queryset=active_items)
)
```

---

## Another Practical Example (Limited Fields)

```python
from django.db.models import Prefetch

items_qs = Item.objects.filter(is_active=True).only('id', 'name')

orders = Order.objects.prefetch_related(
    Prefetch('items', queryset=items_qs)
)
```

➡️ Unnecessary columns fetch होणे टाळले

---

## Result (परिणाम)

- Related objects fetch: **All → Only required**
- Memory usage significantly कमी
- API response time improve झाला
- Database load stable राहिला

---

## Validation (Production मध्ये तपासणी)

- Django Debug Toolbar वापरून query count verify केला
- Response size compare केला
- Peak traffic मध्ये API performance stable राहिला

---

## Key Takeaways (महत्त्वाचे मुद्दे)

- Default `prefetch_related()` blindly वापरू नये
- `Prefetch` + custom queryset = better control
- Large datasets साठी memory optimization critical
- हा use-case **senior Django developers कडून अपेक्षित** असतो

---

## Interview Tip

Interview मध्ये हे answer सांगताना:
- Why custom queryset needed हे आधी explain करा
- Default vs custom behavior compare करा
- Memory आणि performance impact highlight करा

👉 हे उत्तर confidently सांगितल्यास **4 years Django experience clearly justify** होते.

