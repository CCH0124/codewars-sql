For this challenge you need to create a basic Age Calculator function which calculates the age in years on the age field of the peoples table.

The function should be called `agecalculator`, it needs to take 1 date and calculate the age in years according to the date `NOW` and must return an integer.

You may query the people table while testing but the query must only contain the function on your final submit.

##### people table schema
- id
- name
- age

>NOTE: Your solution should use pure SQL. Ruby is used within the test cases to do the actual testing.

```sql
-- Create your FUNCTION statement here
CREATE FUNCTION agecalculator(d date) 
  returns int
  language plpgsql
  as
  $$
  begin
    RETURN (SELECT EXTRACT(year FROM age(d)))::int;
  end;
  $$;
```

If you want to take the current date as the first argument, you can use the following form of the AGE() function:

```sql
AGE(timestamp);
```