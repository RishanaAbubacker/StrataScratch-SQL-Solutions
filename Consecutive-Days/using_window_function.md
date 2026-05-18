# Using Window Function

```sql
WITH consecutive_days AS (
SELECT
    user_id,
    record_date,
    LAG(record_date,1) OVER(PARTITION BY user_id ORDER BY record_date) AS day_1,
    LAG(record_date,2) OVER(PARTITION BY user_id ORDER BY record_date) AS day_2
FROM
    sf_events
 )

SELECT
    user_id
FROM
    consecutive_days
WHERE DATEDIFF(record_date,day_1)=1 
  AND DATEDIFF(record_date,day_2)=2;
```

## Key Insight

The solution uses `LAG()` to access previous event dates for each user and checks whether the dates form a consecutive 3-day sequence.

## Concepts Covered

- Common Table Expressions (CTEs)
- Window Functions
- Date Difference Logic
- Sequential Event Analysis
