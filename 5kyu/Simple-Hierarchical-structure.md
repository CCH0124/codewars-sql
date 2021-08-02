For this challenge you need to create a RECURSIVE Hierarchical query. You have a table of employees, you must order each employee by level. You must use a WITH statement and name it after that has been defined you must select from it.`employees` `employee_levels`

A Level is in correlation what manager managers the employee. e.g. an employee with a manager_id of NULL is at level 1 and then direct employees with the employee at level 1 will be level 2.

### employees table schema
- id
- first_name
- last_name
- manager_id (can be NULL)

### resultant schema
- level
- id
- first_name
- last_name
- manager_id (can be NULL)

>NOTE: refer to the table if you're stuck with how it should be displayed.Results: expected


```sql
-- Create your SELECT statement here
WITH RECURSIVE employee_levels(level, id, first_name, last_name, manager_id) AS (
	SELECT
    1 as level,
		id,
    first_name,
    last_name,
    manager_id
	FROM
		employees
	WHERE
    manager_id is NULL  -- 非遞規
	UNION
		SELECT
			level + 1,
      e.id, 
      e.first_name, 
      e.last_name, 
      e.manager_id
		FROM
			employees e
		INNER JOIN employee_levels el ON e.manager_id = el.id -- 遞規
) SELECT
	*
FROM
	employee_levels;
```

參考資源    
- [postgresqltutorial](https://www.postgresqltutorial.com/postgresql-recursive-query/)