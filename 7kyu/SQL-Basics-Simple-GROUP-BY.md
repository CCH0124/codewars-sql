For this challenge you need to create a simple GROUP BY statement, you want to group all the people by their age and count the people who have the same age.

**people table schema**
- id
- name
- age
**select table schema**
- age [group by]
- people_count (people count)

>NOTE: Your solution should use pure SQL. Ruby is used within the test cases to do the actual testing.



```sql
-- Create your SELECT statement here

SELECT age , COUNT(age) as people_count FROM people GROUP BY age
```