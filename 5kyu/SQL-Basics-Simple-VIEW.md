For this challenge you need to create a VIEW. This VIEW is used by a sales store to give out vouches to members who have spent over $1000 in departments that have brought in more than $10000 total ordered by the members id. The VIEW must be called `members_approved_for_voucher` then you must create a SELECT query using the view.

Tables and relationship below:
![](http://i.imgur.com/hkEkGg1.png)

resultant table schema
- id
- name
- email
- total_spending

```sql
CREATE VIEW members_approved_for_voucher AS
  SELECT m.id, m.name, m.email, SUM(p.price) AS total_spending
  FROM members m
  JOIN sales s ON s.member_id = m.id
  JOIN products p ON p.id = s.product_id
  WHERE s.department_id IN (
    SELECT s2.department_id
    FROM sales s2
    INNER JOIN products p2 ON p2.id = s2.product_id
    GROUP BY s2.department_id
    HAVING SUM(p2.price) > 10000
  )
  GROUP BY m.id, m.name, m.email
  HAVING SUM(p.price) > 1000
  ORDER BY m.id;

SELECT * FROM members_approved_for_voucher;
```