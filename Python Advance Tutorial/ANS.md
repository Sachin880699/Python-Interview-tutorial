## प्रश्न: तुम्ही सुधारण केलेला सर्वात संथ (slow) Django ORM query कोणता होता?

खाली दिलेले उत्तर **पूर्ण practical explanation सह** आहे, जे interview मध्ये **4 years Django experience justify** करण्यासाठी पुरेसे आहे.

---

### Context (परिस्थिती)

Production मधील एका API मध्ये users सोबत त्यांचे orders, last order आणि order count दाखवायचे होते.  
हा API admin dashboard साठी वापरला जात होता.

- Data वाढल्यानंतर API **5–6 सेकंद** घेत होता  
- हा API वारंवार hit होत असल्यामुळे system वर load वाढत होता  

यामुळे performance issue business impact करत होता.

---

### Problem (समस्या काय होती?)

Query logging enable केल्यानंतर आणि production logs तपासल्यानंतर खालील गोष्टी दिसल्या:

- एका API request मध्ये **शेकडो SQL queries** execute होत होत्या  
- प्रत्येक user साठी वेगळी query order table वर जात होती  
- Loop च्या आत ORM queries चालत होत्या  
- हा एक **classic N+1 query problem** होता  

ORM code वाचायला clean वाटत होता, पण internally तो inefficient होता.

---

### Problematic Code (मूळ चुकीचा कोड)

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
