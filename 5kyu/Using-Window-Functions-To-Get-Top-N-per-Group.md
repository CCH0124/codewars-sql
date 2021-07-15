### Description
Given the schema presented below write a query, which uses a window function, that returns two most viewed posts for every category.

Order the result set by:

1. category name alphabetically
2. number of post views largest to lowest
3. post id lowest to largest

Note:
- Some categories may have less than two or no posts at all.
- Two or more posts within the category can be tied by (have the same) the number of views. Use post id as a tie breaker - a post with a lower id gets a higher rank.

### Schema
##### categories
```
 Column     | Type                        | Modifiers
------------+-----------------------------+----------
id          | integer                     | not null
category    | character varying(255)      | not null
```
##### posts
```
 Column     | Type                        | Modifiers
------------+-----------------------------+----------
id          | integer                     | not null
category_id | integer                     | not null
title       | character varying(255)      | not null
views       | integer                     | not null
```
### Desired Output
The desired output should look like this:
```
category_id | category | title                             | views | post_id
------------+----------+-----------------------------------+-------+--------
5           | art      | Most viewed post about Art        | 9234  | 234
5           | art      | Second most viewed post about Art | 9234  | 712
2           | business | NULL                              | NULL  | NULL
7           | sport    | Most viewed post about Sport      | 10    | 126
...
```

- `category_id` - category id
- `category` - category name
- `title` - post title
- `views` - the number of post views
- `post_id` - post id


```sql
-- Replace with your SQL query

SELECT c.id as category_id, c.category, post_rank.title, post_rank.views, post_rank.id as post_id FROM (
    SELECT * , RANK() OVER(PARTITION by category_id ORDER BY views DESC, id) as rnk FROM posts
  ) as post_rank
RIGHT JOIN categories AS c ON c.id = post_rank.category_id
WHERE COALESCE(rnk, 0) <= 2
ORDER BY c.category, post_rank.views DESC, post_id;
```

- [參考資源](https://blog.csdn.net/u011944141/article/details/78927715)
- [postgresql 中文官方](https://docs.postgresql.tw/tutorial/advanced-features/window-functions)