## Cathay Shop Reward Points & Miles Plus Cash – End-to-End Flow

# ORM

        from django.db import models
        from django.contrib.auth.models import User
        from django.db.models import F
        from django.utils import timezone
        from datetime import timedelta
        
        # -------------------------------
        # 1. Points Configuration (Admin)
        # -------------------------------
        class RewardPointsRule(models.Model):
            points_per_amount = models.PositiveIntegerField(default=10)  # 1 point per ₹10
            max_points_per_order = models.PositiveIntegerField(default=500)
            expiry_months = models.PositiveIntegerField(default=12)  # Points expire after 12 months
            created_at = models.DateTimeField(auto_now_add=True)
            updated_at = models.DateTimeField(auto_now=True)
        
            def __str__(self):
                return f"{self.points_per_amount} points per amount, Max {self.max_points_per_order}"
        
        
        # -------------------------------
        # 2. User Points Balance
        # -------------------------------
        class UserPoints(models.Model):
            user = models.OneToOneField(User, on_delete=models.CASCADE, related_name="reward_points")
            points = models.PositiveIntegerField(default=0)
            updated_at = models.DateTimeField(auto_now=True)
        
            def add_points(self, points_to_add):
                self.points = F('points') + points_to_add
                self.save(update_fields=['points'])
                self.refresh_from_db()
        
            def redeem_points(self, points_to_use):
                if points_to_use > self.points:
                    raise ValueError("Insufficient points")
                self.points = F('points') - points_to_use
                self.save(update_fields=['points'])
                self.refresh_from_db()
        
            def __str__(self):
                return f"{self.user.username} - {self.points} points"
        
        
        # -------------------------------
        # 3. Reward Transactions Log
        # -------------------------------
        class RewardTransaction(models.Model):
            TRANSACTION_TYPES = (
                ('earn', 'Earn'),
                ('use', 'Use'),
                ('expire', 'Expire'),
                ('manual', 'Manual Adjustment')
            )
        
            user = models.ForeignKey(User, on_delete=models.CASCADE, related_name="reward_transactions")
            points = models.PositiveIntegerField()
            transaction_type = models.CharField(max_length=10, choices=TRANSACTION_TYPES)
            reference = models.CharField(max_length=100, null=True, blank=True)  # e.g., Order ID
            created_at = models.DateTimeField(auto_now_add=True)
            expiry_date = models.DateTimeField(null=True, blank=True)  # Only for earned points
        
            def __str__(self):
                return f"{self.user.username} - {self.transaction_type} - {self.points}"
        
        
        # -------------------------------
        # 4. Order Model (Simplified)
        # -------------------------------
        class Order(models.Model):
            user = models.ForeignKey(User, on_delete=models.CASCADE)
            total_amount = models.DecimalField(max_digits=10, decimal_places=2)
            points_used = models.PositiveIntegerField(default=0)
            points_earned = models.PositiveIntegerField(default=0)
            payment_status = models.CharField(max_length=20, choices=(('pending', 'Pending'),
                                                                     ('paid', 'Paid'),
                                                                     ('failed', 'Failed')))
            created_at = models.DateTimeField(auto_now_add=True)
        
            def __str__(self):
                return f"Order {self.id} by {self.user.username}"
        
        
        # -------------------------------
        # 5. Core Functions
        # -------------------------------
        
        # Calculate points earned for an order
        def calculate_points_earned(order_amount):
            rule = RewardPointsRule.objects.last()
            if not rule:
                return 0
            return min(order_amount // rule.points_per_amount, rule.max_points_per_order)
        
        
        # Earn points after order
        def earn_points(user, order):
            points = calculate_points_earned(order.total_amount)
            expiry_date = timezone.now() + timedelta(days=30*RewardPointsRule.objects.last().expiry_months)
            
            # Update UserPoints
            user_points, created = UserPoints.objects.get_or_create(user=user)
            user_points.add_points(points)
            
            # Log transaction
            RewardTransaction.objects.create(
                user=user,
                points=points,
                transaction_type='earn',
                reference=f'ORDER{order.id}',
                expiry_date=expiry_date
            )
            order.points_earned = points
            order.save(update_fields=['points_earned'])
            return points
        
        
        # Redeem points during checkout
        def redeem_points(user, order, points_to_use):
            user_points = UserPoints.objects.get(user=user)
            if points_to_use > user_points.points:
                raise ValueError("Insufficient points")
            
            # Deduct points
            user_points.redeem_points(points_to_use)
            
            # Log transaction
            RewardTransaction.objects.create(
                user=user,
                points=points_to_use,
                transaction_type='use',
                reference=f'ORDER{order.id}'
            )
            
            # Update order
            order.points_used = points_to_use
            order.save(update_fields=['points_used'])
            
            # Calculate remaining amount to charge via credit card
            remaining_amount = order.total_amount - points_to_use
            return remaining_amount
        
        
        # Expire points (run via cron or daily job)
        def expire_points():
            now = timezone.now()
            expired_transactions = RewardTransaction.objects.filter(
                transaction_type='earn',
                expiry_date__lt=now
            )
            for txn in expired_transactions:
                user_points = UserPoints.objects.get(user=txn.user)
                user_points.redeem_points(txn.points)
                RewardTransaction.objects.create(
                    user=txn.user,
                    points=txn.points,
                    transaction_type='expire',
                    reference=f'EXPIRED-{txn.id}'
                )
        
        
        # Manual adjustment by admin
        def add_manual_points(user, points, note="Manual adjustment"):
            user_points, _ = UserPoints.objects.get_or_create(user=user)
            user_points.add_points(points)
            
            RewardTransaction.objects.create(
                user=user,
                points=points,
                transaction_type='manual',
                reference=note
            )

---

# 1️⃣ Admin Panel Setup

**Purpose:** System तयार करण्यासाठी आणि rules define करण्यासाठी.

**Admin Tasks:**

1. Configure reward points rules (किती points per ₹ spent, maximum points per order, expiry rules)
2. Add points manually for promotions or corrections
3. View user points balances
4. Monitor all points transactions (earn/use)

**Example:** Admin sets 1 point per ₹10. User spends ₹500 → 50 points earned.

---

# 2️⃣ User Browsing & Product Selection

* User opens Cathay Shop web/app.
* Browses products (fast loading because product APIs optimized).
* Chooses a product to purchase.

**Backend Notes:** Product detail API fetches only necessary fields. Search API supports filters (category, brand, price).

---

# 3️⃣ Checkout – Miles Plus Cash

**Step 1:** User goes to checkout.

**Step 2:** System fetches user reward points from `UserPoints` table.

**Step 3:** User selects points to use. Remaining amount charged via credit card.

**Backend Calculation:**

```python
user_points = get_user_points(user)
if points_to_use <= user_points.points:
    remaining_amount = order_total - points_to_use
    user_points.points -= points_to_use
    user_points.save()
    RewardTransaction.objects.create(user=user, points=points_to_use, type='use', reference=order_id)
```

**Step 4:** Charge remaining amount via payment gateway.

**Step 5:** Update `Order` table and log points transaction.

---

# 4️⃣ Points Earning

* Points earned automatically on purchase.
* **Backend logic:**

```python
def earn_points(user, order_amount):
    points_earned = order_amount // 10
    UserPoints.objects.update_or_create(user=user, defaults={"points": F('points') + points_earned})
    RewardTransaction.objects.create(user=user, points=points_earned, transaction_type='earn', reference='ORDER123')
```

* Admin can also add points manually for promotions.

---

# 5️⃣ Points Redemption & Expiry

* Users can only redeem points ≤ available points.
* Maximum points per order enforced.
* Expiry: Points expire after configured months. Admin panel displays expired points.

---

# 6️⃣ Backend-to-Frontend Integration

* APIs designed for fast frontend consumption:

  * `/products/` – fetch products
  * `/products/search` – search & filter
  * `/checkout/` – Miles + Cash API
  * `/user/points` – fetch current points
  * `/admin/transactions` – points transactions
* JSON responses optimized for minimal data transfer.

---

# 7️⃣ Admin Monitoring

* Admin panel shows:

  1. User Points balance
  2. Earned points per order
  3. Redeemed points per order
  4. Expired points
  5. Manual adjustments

**Example:**

```
User: Sachin
Available Points: 150
Transactions:
- Earned 50 points (ORDER123)
- Used 30 points (ORDER124)
- Expired 20 points
```

---

# 8️⃣ End-to-End Summary Flow

1. Admin configures points rules → system ready
2. User browses products → optimized product APIs
3. User selects product → goes to checkout
4. Backend fetches user points → user chooses points to redeem
5. Remaining amount charged via credit card
6. Points deducted and logged in `RewardTransaction`
7. New points earned from purchase → added to user balance
8. Admin panel shows full transaction history and user points balance

---

# Key Tables / Models

1. `UserPoints` – track balance
2. `RewardTransaction` – earn/use history
3. `Order` – link points with order

---

# Interview-Friendly Explanation

"In Cathay Shop, I built the complete reward points system. Admin configures points rules and monitors balances. Users earn points on purchases, redeem them during checkout using Miles Plus Cash, and backend ensures accurate calculation, transaction logging, and integration with payment gateways. I also optimized APIs so product listing, search, and checkout are fast, and ensured admin panel can monitor every transaction."
