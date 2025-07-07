# Q1 – Histogram of Tweets Per User  
**Source:** Twitter  
**Difficulty:** Easy 

---

## ✅ Goal  
You are given a table `tweets` with columns `user_id`, `msg`, and `tweet_date`.

Your task is to:
1. Count how many tweets each user made in **2022**
2. Group users by that tweet count to generate a **histogram**

**Output Columns:**
- `tweet_bucket`: number of tweets made by a user  
- `users_num`: number of users who made that many tweets

---

## 🔍 Method 1 – Using Subquery

```sql
SELECT 
  num_of_tweets AS tweet_bucket, 
  COUNT(*) AS users_num
FROM (
  SELECT 
    user_id, 
    COUNT(*) AS num_of_tweets
  FROM tweets
  WHERE tweet_date BETWEEN '2022-01-01' AND '2022-12-31'
  GROUP BY user_id
) AS t
GROUP BY tweet_bucket
ORDER BY tweet_bucket;
```

💡 Explanation

Inner query: Counts how many tweets each user posted in 2022

 Outer query: Groups by the number of tweets and counts users in each group

 ## 🔍 Method 2 – Using CTE

```sql
 WITH tweet_counts AS (
  SELECT 
    user_id, 
    COUNT(*) AS num_of_tweets
  FROM tweets
  WHERE tweet_date BETWEEN '2022-01-01' AND '2022-12-31'
  GROUP BY user_id
)
SELECT 
  num_of_tweets AS tweet_bucket, 
  COUNT(*) AS users_num
FROM tweet_counts
GROUP BY num_of_tweets
ORDER BY tweet_bucket;
```
