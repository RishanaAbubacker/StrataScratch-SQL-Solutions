# Monthly Rank Improvement
**Company:** Facebook/Meta | **Difficulty:** Hard | **Topic:** Window Functions

## 📝 Problem Description
Rank countries by their total number of comments in descending order for Dec 2019 and Jan 2020. Find countries whose rank improved (e.g., moved from Rank 5 to Rank 2).

## 🛠️ The Logic
1. **Aggregate**: Sum comments per country, per month.
2. **Rank**: Use `DENSE_RANK()` partitioned by month to create a leaderboard.
3. **Compare**: Use `LAG()` to pull the December rank onto the January row.
4. **Filter**: Select countries where the January rank is smaller than the December rank.

## 💻 Solution (PostgreSQL)

```sql
WITH monthly_ranks AS (
    SELECT 
        EXTRACT(MONTH FROM cc.created_at) AS month,
        au.country,
        SUM(cc.number_of_comments) AS total_comments,
        DENSE_RANK() OVER (
            PARTITION BY EXTRACT(MONTH FROM cc.created_at) 
            ORDER BY SUM(cc.number_of_comments) DESC
        ) AS rank_val
    FROM fb_comments_count cc
    JOIN fb_active_users au ON cc.user_id = au.user_id
    WHERE cc.created_at >= '2019-12-01' AND cc.created_at < '2020-02-01'
    GROUP BY 1, 2
),
rank_comparison AS (
    SELECT 
        country,
        month,
        rank_val AS current_rank,
        LAG(rank_val) OVER (PARTITION BY country ORDER BY month ASC) AS previous_rank
    FROM monthly_ranks
)
SELECT country
FROM rank_comparison
WHERE month = 1 
  AND current_rank < previous_rank;
