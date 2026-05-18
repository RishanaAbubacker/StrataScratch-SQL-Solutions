# SQL Solution — Using Window Functions

```sql
WITH monthly_sales AS (
SELECT
    MONTH(invoicedate) AS sold_month,
    description,
    SUM(unitprice * quantity) AS total_paid,
    RANK() OVER(
        PARTITION BY MONTH(invoicedate)
        ORDER BY SUM(unitprice * quantity) DESC
    ) AS total_rank
FROM
    online_retail
WHERE quantity > 0
GROUP BY sold_month, description
)

SELECT 
    sold_month,
    description,
    total_paid
FROM monthly_sales
WHERE total_rank = 1;
```

---

## Concepts Covered

- Common Table Expressions (CTEs)
- Window Functions
- Ranking Functions
- Aggregations
- Date Functions
