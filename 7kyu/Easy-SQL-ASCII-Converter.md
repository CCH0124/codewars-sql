Given a demographics table in the following format:

** demographics table schema **

- id
- name
- birthday
- race

you need to return the same table where all text fields (name & race) are changed to the ascii code of their first byte.

e.g. Verlie = 86 Warren = 87 Horace = 72 Tracy = 84

```sql
/*  SQL  */
SELECT id, ASCII(LEFT(name,1)) as name, birthday, ASCII(LEFT(race,1)) as race
FROM demographics
```
>LEFT 表示從 name 欄位值取出第一個字元