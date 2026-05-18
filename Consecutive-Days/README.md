# Consecutive Days

**Difficulty:** Medium  
**Platform:** StrataScratch  
**Problem ID:** 2054

## Problem Statement

Find users who logged events on 3 consecutive days.

## Concepts Used

- Window Functions
- LAG()
- Date Functions
- CTEs

## Approach

1. Use `LAG()` to fetch previous 1-day and 2-day records for each user.
2. Compare date differences using `DATEDIFF()`.
3. Filter users where:
   - Current date and previous date difference = 1
   - Current date and second previous date difference = 2


[View Details Solution](./Solutions/using_window_function.md)

## Folder Structure

```text
consecutive-days-2054/
│
├── README.md
└── solutions/
    └── using_window_function.md
```
