# Q33 – Team Power Users  
**Source:** Microsoft  
**Difficulty:** Easy  

---

## ✅ Goal  
You're given a `messages` table with:

- `sender_id`
- `sent_date`
- `message_text`

Your task is to:  
> Find the **top 2 users** who sent the **most messages** during **August 2022**.  
> Return their `sender_id` and `message_count`.

---

## 🔍 Solution – Using `GROUP BY` and `ORDER BY`

```sql
SELECT sender_id, COUNT(*) AS message_count
FROM messages
WHERE sent_date BETWEEN '2022-08-01' AND '2022-08-31'
GROUP BY sender_id
ORDER BY message_count DESC
LIMIT 2;
```
💡 Explanation

WHERE sent_date BETWEEN '2022-08-01' AND '2022-08-31'
Filters messages sent in August 2022.

GROUP BY sender_id
Groups records by user to count how many messages each one sent.

COUNT(*) AS message_count
Counts messages per user.

ORDER BY message_count DESC
Sorts senders from most to least messages.

LIMIT 2
Returns top 2 power users.
