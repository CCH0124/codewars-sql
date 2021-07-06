Given a table of random numbers as follows:

** numbers table schema **

- id
- number1
- number2

Your job is to return table with similar structure and headings, where if the sum of a column is odd, the column shows the minimum value for that column, and when the sum is even, it shows the max value. You must use a case statement.

** output table schema **

- number1
- number2


判斷該列數字是奇數則找出該列最小值；否則反之。題目說需使用 `case` 方式


```sql
/*  SQL  */

SELECT
  CASE 
    WHEN  SUM(number1) % 2 != 0 THEN MIN(number1)
    WHEN  SUM(number1) % 2 = 0 THEN MAX(number1)
  END  number1,
  CASE
    WHEN SUM(number2) % 2 != 0 THEN MIN(number2)
    WHEN SUM(number2) % 2 = 0 THEN MAX(number2)
  END number2
FROM numbers;

```