# Best Selling Item

**Difficulty:** Medium  
**Platform:** StrataScratch  

## Problem Link

https://platform.stratascratch.com/coding/10172-best-selling-item?code_type=3

---

## Problem Statement

Find the best-selling item for each month based on total sales revenue.

Only include valid purchases where quantity is greater than 0.

Return:
- sold_month
- description
- total_paid

---

## Concepts Used

- CTEs
- Aggregation
- Window Functions
- Ranking Functions
- Date Functions

---

## Solution Approach

1. Extract the month from `invoicedate`
2. Calculate total revenue using:
   - `unitprice * quantity`
3. Aggregate revenue by:
   - month
   - product description
4. Rank products within each month based on revenue
5. Select only the top-ranked item per month
