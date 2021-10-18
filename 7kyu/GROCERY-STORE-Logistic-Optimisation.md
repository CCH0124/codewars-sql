You are the owner of the Grocery Store. All your products are in the database, that you have created after CodeWars SQL excercises!:)

You have reached a conclusion that you waste to much time because you have to many different warehouse to visit each week.

You have to find out how many different type of products you buy from each producer. If you take only few items from some of them you will stop going there to save the gasoline:)

In the results show `producer` and `count_products_types` which you buy from him.

Order the result by `count_products_types` (DESC) then by `producer` (ASC) in case there are duplicate amounts,
### products table schema
- id
- name
- price
- stock
- producer

### results table schema
- count_products_types
- producer

必須瞭解從每個 producer 處購買多少種不同類型的產品。因此使用 COUNT 來計算商品名稱。

```sql
-- Replace with your SQL Query
SELECT COUNT(name) as count_products_types, producer 
FROM products 
GROUP BY producer 
ORDER BY count_products_types DESC, producer ASC 
```