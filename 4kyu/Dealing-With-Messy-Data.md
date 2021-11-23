Your company has an internal policy to determine your customers' credit limit, but this procedure has been questioned recently by the board as being too conservative.

Your CEO wants to increase the current customer base credit limits in order to upsell a new line of products. In order to do that, the company hired several external consultancies to produce new credit limit estimates.

The problem is that each agency has produced the report in its own format. Some use the format `"First-name Last-name"` to identify a person, others use the format "`Last-name, First-name"`. There is also no consensus on how to capitalize each word, so some used all uppercase, others used all lowercase, and some used mixed-case.

Internally, the data is structured as follows:
```
Table: customers
================

id: INT
first_name: TEXT
last_name: TEXT
credit_limit: FLOAT
```
The data you've received from all agencies was consolidated in the following table:
```
Table: prospects
================
full_name: TEXT
credit_limit: FLOAT
```

Keep in mind that the agencies had access only to a partial customer base. There is also the possibility of more than one agency prospecting the same customer, so it's highly likely that there will be duplicates. Finally, they've prospected customers that were not in your customer base as well.

For this task you are interested in the prospected customers that are already in your customer base and the prospected credit limit is higher than your internal estimate. When more than one agency prospected the same customer, chose the highest estimate.

You have to produce a report with the following fields:

```
first_name
last_name
old_limit [the current credit_limit]
new_limit [the highest credit_limit found]
```

Notes:

only list the customers that a higher credit limit was found.

```sql
create extension pg_trgm; -- 高階索引 --
create index prospects_idx on prospects using gin(full_name gin_trgm_ops);
-- ILIKE 不區分字符串的大小寫
SELECT c.first_name
     , c.last_name
     , c.credit_limit as old_limit
     , MAX(p.credit_limit) as new_limit
FROM customers c JOIN prospects p
ON p.full_name ILIKE CONCAT('%', c.first_name, '%')
and p.full_name ILIKE CONCAT('%', c.last_name, '%')
and p.credit_limit > c.credit_limit
GROUP BY c.first_name, c.last_name, c.credit_limit
ORDER BY c.first_name
```
如果不設定以下會有超時問題
```
create extension pg_trgm; -- 高階索引 --
create index prospects_idx on prospects using gin(full_name gin_trgm_ops);
```

- [Gin](https://www.readfog.com/a/1636747805870624768)
- [Gin](https://www.cnblogs.com/flying-tiger/p/6704931.html)