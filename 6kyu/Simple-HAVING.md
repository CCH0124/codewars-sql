For this challenge you need to create a simple HAVING statement, you want to count how many people have the same age and return the groups with 10 or more people who have that age.

### people table schema
- id
- name
- age
### return table schema
- age
- total_people

>NOTE: Your solution should use pure SQL. Ruby is used within the test cases to do the actual testing.

題意是要列出年齡人口是大於等於 10 的人以上。

```sql
-- Create your SELECT statement here

SELECT age, COUNT(*) AS total_people FROM people GROUP BY age HAVING COUNT(*) >= 10;
```

其流程會是

![](https://www.postgresqltutorial.com/wp-content/uploads/2020/07/PostgreSQL-Having-1.png "From postgresqltutorial")

參考資源
- [postgresqltutorial](https://www.postgresqltutorial.com/postgresql-having/)