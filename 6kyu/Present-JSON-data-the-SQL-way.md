**Task**
Given the database where users are stored in JSON format, fetch the records splitting the data into separate columns.

**Notes**
- The `private` field determines whether the user's email address should be publicly visible
- If the profile is private, `email_address` should equal `"Hidden"`
- The users may have multiple email addresses
- If no email addresses are provided, `email_address` should equal `"None"`
- If there're multiple email addresses, the first one should be shown
- The `date_of_birth` is in the `yyyy-mm-dd` format
- The `age` fields represents the user's age in years
- Order the result by the `first_name`, and `last_name` columns


**Input table**
```
-------------------------
| Table | Column | Type |
|-------+--------+------|
| users | id     | int  |
|       | data   | json |
-------------------------
```

**JSON object format**
```
--------------------------------------
|     Field       |       Type       |
|-----------------+------------------|
| first_name      | string           |
| last_name       | string           |
| date_of_birth   | string           |
| email_addresses | array of strings |
| private         | boolean          |
--------------------------------------
```

**Output table**
```
------------------------
|    Column     | Type |
|---------------+------|
| first_name    | text |
| last_name     | text |
| age           | int  |
| email_address | text |
------------------------
```

```sql
SELECT data->> 'first_name' AS first_name,
      data->> 'last_name' AS last_name,
      EXTRACT(YEAR FROM AGE(now(), (data->>'date_of_birth')::date))::Integer AS age, # 最後要轉 integer 不然是 float
      CASE
        WHEN data->>'private' = 'true' then 'Hidden'
        WHEN data->>'private' = 'false' and data#>>'{email_addresses, 0}' isnull then 'None'
         ELSE data#>>'{email_addresses, 0}'
      END as email_address
FROM users
ORDER BY first_name, last_name;
```

- [postgresqltutorial](https://www.postgresqltutorial.com/postgresql-json/)
- [postgresql 官方 JSON 相關操作](https://www.postgresql.org/docs/current/functions-json.html#FUNCTIONS-JSON-OP-TABLE)
- [年齡轉換](https://stackoverflow.com/questions/40484345/how-to-get-only-year-from-age-function-in-postgresql-select-query/40484590)