## Messi goals function
Messi is a soccer player with goals in three leagues:

- LaLiga
- Copa del Rey
- Champions
Complete the function to return his total number of goals in all three leagues.

Note: the input will always be valid.
For example:

```
5, 10, 2  -->  17
```


```sql
-- you will be given a table, goals, with columns la_liga_goals, copa_del_rey_goals, and champions_league_goals. Return a table with a column, res.
SELECT (la_liga_goals+copa_del_rey_goals+champions_league_goals) as res FROM goals
```