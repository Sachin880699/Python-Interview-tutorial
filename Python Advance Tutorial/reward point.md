## Cathay Shop Reward Points & Miles Plus Cash – End-to-End Flow

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
