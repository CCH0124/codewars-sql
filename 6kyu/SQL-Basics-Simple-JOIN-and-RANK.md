For this challenge you need to create a simple `SELECT` statement that will return all columns from the `people` table, and join to the `sales` table so that you can return the COUNT of all sales and RANK each person by their `sale_count`.

**people table schema**

- id
- name

**sales table schema**
- id
- people_id
- sale
- price

You should return all people fields as well as the sale count as "sale_count" and the rank as "sale_rank".

>NOTE: Your solution should use pure SQL. Ruby is used within the test cases to do the actual testing.

比較重要的是返回所有銷售額的 `COUNT` 並按每個人的 `sale_count` 排名。排名部分就會使用 [RANK](https://www.postgresqltutorial.com/postgresql-window-function/postgresql-rank-function/)，因為會針對個人因此會在用 `PARTITION` 進行細部的分析。

```sql
-- Create your SELECT statement here
SELECT p.id, p.name, COUNT(s.sale) as sale_count, RANK() OVER(PARTITION BY p.id) as sale_rank 
FROM people as p 
JOIN sales as s ON p.id = s.people_id 
GROUP BY p.id;
```