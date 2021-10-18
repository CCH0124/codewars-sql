Write a select statement that takes `name` from `person` table and return `"Hello, <name> how are you doing today?" `results in a column named greeting

>[Make sure you type the exact thing I wrote or the program may not execute properly]

```sql
--person table has name data

SELECT CONCAT('Hello, ',name,' how are you doing today?') as greeting FROM person 
SELECT FORMAT('Hello, %s how are you doing today?', name) as greeting FROM person
SELECT 'Hello, ' || name || ' how are you doing today?' as greeting FROM person
```