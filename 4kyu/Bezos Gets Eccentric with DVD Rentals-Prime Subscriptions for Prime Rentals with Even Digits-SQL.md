# Bezos Gets Eccentric with DVD Rentals: Prime Subscriptions for Prime Rentals with Even Digits (SQL)

**Introduction**

After conquering the world of online shopping, Jeff Bezos, the billionaire founder of Amazon, decided to expand his empire into the world of DVD rentals. He signed a contract with a local DVD rental shop to provide the latest movies to Amazon customers through a new DVD rental service.

As a way to thank the customers who signed up for the new service, Bezos announced that he would be giving a free Amazon Prime subscription to all customers whose total quantity of rentals added up to a prime number. The catch? Only a select few customers actually qualified for the free subscription, because their total rentals had to be a prime number!

But Jeff didn't stop there. He wanted to make sure that only the most unique and interesting customers received the free subscription. To qualify, not only did the total rentals have to add up to a prime number, but the sum of the digits in their customer ID numbers had to be even.

**Task**

Your task is to write a query that retrieves customer information for a DVD rental service, including their customer ID, full name (concatenated first name and last name), the total number of unique rentals they have made, and the total amount of money they have paid for those rentals.

However, there are two additional conditions that must be met for a customer to be included in the query results:

* The total number of rentals made by the customer must be a prime number.
* The sum of the digits in the customer's ID number must be an even number.

The results should be sorted first by total payments (in descending order), then by total rentals (in descending order), and finally by last name (in ascending order). The query should display the following columns:

* `customer_id`: ID of customer that meets above mentioned condition
* `customer_name`: concatenated first name and last name.
* `all_rentals`: the total number of rentals they have made and this amount meets above mentioned condition
* `total_payments`: total amount of money customer have paid for those rentals. This param should be rounded to 2 decimal places.

**Notes:**
* for the sample tests, static dump of DVD Rental Sample Database is used, for the final solution - random tests.
* please use `numeric` type to display total_amount
* There are rentals without payments in the test data. The tests expect rentals without any payments to not be counted among the total rentals for a customer.

**Schema:**
(data that is needed for the task)

`customer` table:

```sql
   Column    |  Type   | Modifiers
-------------+---------+-----------
 customer_id | integer | not null
 first_name  | text    | not null
 last_name   | text    | not null
```

`rental` table:

```sql
   Column   |  Type   | Modifiers
------------+---------+-----------
 rental_id  | integer | not null
 customer_id| integer | not null
```

`payment` table:

```sql
   Column     |  Type    | Modifiers
--------------+----------+-----------
 payment_id   | integer  | not null
 rental_id    | integer  | not null
 amount       | numeric  | not null
```

**Desired Output**

The desired output should look like this:
```sql
customer_id | customer_name | all_rentals | total_payments |
------------+---------------+-------------+----------------|
   33       | Emily Diaz    | 31          | 0.15271e3      |
   15       | Jackie Lynch  | 29          | 0.14969e3      |
...
```



```sql
WITH customer_stats AS (
    -- 1. 根據 Notes 要求：只計算有付款紀錄的租借
    -- 使用 INNER JOIN 連結 rental 與 payment 會自動排除無付款的租借
    SELECT 
        c.customer_id,
        c.first_name || ' ' || c.last_name AS customer_name,
        c.last_name,
        COUNT(DISTINCT r.rental_id) AS all_rentals,
        SUM(p.amount)::numeric AS total_amount_raw -- 使用 numeric 型別
    FROM customer c
    JOIN rental r ON c.customer_id = r.customer_id
    JOIN payment p ON r.rental_id = p.rental_id
    GROUP BY c.customer_id, c.first_name, c.last_name
),
filtered_customers AS (
    -- 2. 處理條件過濾：ID 數字總和為偶數 & 租借數為質數
    SELECT 
        customer_id,
        customer_name,
        last_name,
        all_rentals,
        ROUND(total_amount_raw, 2) AS total_payments,
        -- 計算 ID 各位數總和
        (SELECT SUM(d::int) FROM regexp_split_to_table(customer_id::text, '') d) AS id_digit_sum
    FROM customer_stats
)
SELECT 
    customer_id,
    customer_name,
    all_rentals,
    total_payments
FROM filtered_customers
WHERE 
    -- 條件一：ID 各位數之和為偶數
    id_digit_sum % 2 = 0
    -- 條件二：租借總數必須是質數
    AND all_rentals > 1 
    AND NOT EXISTS (
        SELECT 1 
        FROM generate_series(2, floor(sqrt(all_rentals))::int) AS i 
        WHERE all_rentals % i = 0
    )
ORDER BY 
    total_payments DESC,   -- 1. 總支付金額（降序）
    all_rentals DESC,      -- 2. 總租借次數（降序）
    last_name ASC;         -- 3. 姓氏（升序）
```


1. 資料彙總 (customer_summary)：

  * 使用 `||` 符號將 first_name 與 last_name 合併為 customer_name。
  * 透過 COUNT(rental_id) 計算租借總數。
  * 使用 ROUND(SUM(amount), 2) 處理支付金額並保留兩位小數。

2. ID 數字總和條件：

  * 使用 `regexp_split_to_table` 將 ID（如 123）拆解為多個橫列（1, 2, 3），再進行 SUM 加總。
  * 最後判斷 `id_digit_sum % 2 = 0` 來確認是否為偶數。

3. 質數判斷條件：
  * generate_series 用來動態生成這段範圍的數字進行除法測試。
