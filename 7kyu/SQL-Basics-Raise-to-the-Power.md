Given the following table 'decimals':

### decimals table schema
- id
- number1
- number2

Return a table with a single column `result` which is the output of number1 raised to the power of number2.

```sql
/*  SQL  */
SELECT POWER(number1,number2) as result FROM decimals
```