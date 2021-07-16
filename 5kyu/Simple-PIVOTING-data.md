For this challenge you need to PIVOT data. You have two tables, products and details. Your task is to pivot the rows in products to produce a table of products which have rows of their detail. Group and Order by the name of the Product.

### Tables and relationship below:

![](http://i.imgur.com/81Ww3YH.png)

You must use the CROSSTAB statement to create a table that has the schema as below:

CROSSTAB table.
- name
- good
- ok
- bad

Compare your table to the expected table to view the expected results.

[more info can be found here](https://www.postgresql.org/docs/9.2/static/tablefunc.html)


這題其實有點不大懂 `CROSSTAB` 用途，後來看了[這篇](https://www.freecodecamp.org/news/postgresql-tricks/)才有一點了解。

```sql

CREATE EXTENSION tablefunc;

-- Create your CROSSTAB statement here

SELECT * 
FROM  CROSSTAB (
      'SELECT p.name, d.detail, COUNT(d.id)
      FROM products p
      JOIN details d
      ON p.id = d.product_id
      GROUP BY p.name, d.detail
      ORDER BY p.name, d.detail')
AS summary (name text, bad bigint, good bigint, ok bigint)
```