# Highest Cost Orders

Company: Facebook/Meta | Difficulty: Medium | Topic: Window Functions, Aggregation

---

## 🛠️ The Logic

### Step 1: Calculate Daily Order Totals
Join the `customers` and `orders` tables and calculate each customer's total order cost per day.

### Step 2: Rank Customers Per Day
Use the `RANK()` window function partitioned by `order_date` to rank customers based on their daily spending.

### Step 3: Select Highest Cost Orders
Filter only rows where the rank is `1` to get the highest daily spenders.

---

## 💻 Solution (SQL)

```sql
WITH daily_total_order AS (
    SELECT
        c.id,
        c.first_name,
        SUM(o.total_order_cost) AS daily_total_order,
        o.order_date
    FROM customers AS c
    JOIN orders AS o
        ON c.id = o.cust_id
    WHERE o.order_date BETWEEN '2019-02-01' AND '2019-05-01'
    GROUP BY
        c.id,
        c.first_name,
        o.order_date
),

daily_total_rank_table AS (
    SELECT
        id,
        first_name,
        order_date,
        daily_total_order,
        RANK() OVER (
            PARTITION BY order_date
            ORDER BY daily_total_order DESC
        ) AS daily_total_rank
    FROM daily_total_order
)

SELECT
    first_name,
    daily_total_order,
    order_date
FROM daily_total_rank_table
WHERE daily_total_rank = 1;
```

---

## 🔍 Key Concepts Used

- Common Table Expressions (CTEs)
- `SUM()` Aggregation
- `RANK()` Window Function
- `PARTITION BY`
- Group-wise Maximum

---

## ✅ Expected Outcome

The query returns customers who had the highest total order cost for each day within the given date range.

