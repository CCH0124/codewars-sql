You have access to a table of monsters as follows:

**monsters schema**

- id
- name
- legs
- arms
- characteristics

The monsters in the provided table have too many characteristics, they really only need one each. Your job is to trim the characteristics down so that each monster only has one. If there is only one already, provide that. If there are multiple, provide only the first one (don't leave any commas in there).

You must return a table with the format as follows:

**output schema**

- id
- name
- characteristic
Order by id


characteristics，透過 `,` 來表示多個特徵，因此可以用 [split_part](https://www.postgresqltutorial.com/postgresql-string-functions/postgresql-split_part/) 函數來切割。
```sql
/*  SQL  */
SELECT id , name , split_part(characteristics,',',1) as characteristic FROM monsters ORDER BY id;
```

