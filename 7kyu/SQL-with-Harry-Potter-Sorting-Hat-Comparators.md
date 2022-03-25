There is truly no magic in the world; the Hogwarts Sorting Hat is SQL-based, its decision-making powers are common operators and prospectIve students are merely data - names, and two columns of qualities.

**students**

- id
- name
- quality1
- quality2

Slytherin are being quite strict this year and will only take students who are evil AND cunning.
Gryffindor will take students who are brave but only if their second quality is NOT evil.
Ravenclaw accepts students who are studious OR intelligent.
Hufflepuff will simply take those who have the quality hufflepuff.

(don't worry, for simplicity's sake 'brave' and 'studious' will only appear in quality1, and 'cunning' and 'intelligent' will only appear in quality2.)

Return the id, name, quality1 and quality2 of all the students who'll be accepted, ordered by ascending id.

簡單的過濾和基本的排序
```sql
/* Oh you may not think I'm pretty,
But don't judge on what you see,
I'll eat myself if you can find
A smarter hat than me. */
SELECT * FROM students
WHERE ((quality1 = 'evil' and quality2 = 'cunning')
OR (quality1 = 'brave' and quality2 != 'evil')
OR (quality1 = 'studious' or quality2 = 'intelligent')
OR (quality1 = 'hufflepuff' or quality2 = 'hufflepuff'))
ORDER BY id;
```