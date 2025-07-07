# Q7 – Top 5 Artists with Most Songs in Global Top 10  
**Source:** Spotify  
**Difficulty:** Medium  

---

## ✅ Goal  
You are given three tables:

- `artists(artist_id, artist_name)`
- `songs(song_id, artist_id)`
- `global_song_rank(song_id, rank, day)`

Your task is to:
- Find the **top 5 artists** who have the most song appearances in the **Top 10** global rankings.
- Return each artist’s name and their rank (`artist_rank`).
- If artists tie, use `DENSE_RANK()` so that the rank numbers are not skipped (e.g., 1, 2, 2, 3).

---

## 🔍 Solution – Using DENSE_RANK

```sql
SELECT artist_name, artist_rank
FROM (
  SELECT 
    a.artist_name,
    DENSE_RANK() OVER (
      ORDER BY COUNT(*) DESC
    ) AS artist_rank
  FROM global_song_rank g
  JOIN songs s ON g.song_id = s.song_id
  JOIN artists a ON s.artist_id = a.artist_id
  WHERE g.rank <= 10
  GROUP BY a.artist_name
) AS ranked
WHERE artist_rank <= 5
ORDER BY artist_rank, artist_name;
```

💡 Explanation

We join all 3 tables to link each song in the Top 10 to its artist.

We count how many times each artist's song appears in the Top 10.

DENSE_RANK() is used to assign ranking based on appearance count.

The final query filters to only include the top 5 ranked artists.
