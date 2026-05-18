## Friend-Based Page Recommendation

Company: Meta Platforms / Facebook | Difficulty: Medium | Topic: Anti Join, Recommendation Logic

### 🛠️ The Logic

#### Step 1: Find Pages Followed by Friends
Join the friendship table with the pages table to identify pages followed by each user's friends.

#### Step 2: Remove Already Followed Pages
Use `NOT EXISTS` to exclude pages already followed by the user.

#### Step 3: Generate Recommendations
Return unique user-page combinations as recommendations.

---

## 💻 Solution (SQL)


SELECT DISTINCT
    f.user_id,
    p.page_id
FROM facebook_friends AS f
JOIN facebook_pages AS p
    ON f.friend_id = p.user_id
WHERE NOT EXISTS (
    SELECT 1
    FROM facebook_pages AS fp
    WHERE fp.user_id = f.user_id
      AND fp.page_id = p.page_id
);
