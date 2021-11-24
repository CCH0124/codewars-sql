For this challenge you need to create a simple SELECT statement. Your task is to calculate the MIN, MEDIAN and MAX scores of the students from the results table.

### Tables and relationship below:
![](https://i.imgur.com/Qdt9DqU.png)
### Resultant table:
- min
- median
- max


```sql
-- Create your SELECT statement here
SELECT 
  MIN(score) AS min, 
  PERCENTILE_CONT(0.5) WITHIN GROUP(ORDER BY score) AS median,
  MAX(score) AS max
FROM result;
```

- [calculate median postgresql](https://ubiq.co/database-blog/calculate-median-postgresql/)