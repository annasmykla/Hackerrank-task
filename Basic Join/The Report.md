# SQL Task and Solutions

## Task Description

Ketty gives Eve a task to generate a report containing three columns: **Name**, **Grade**, and **Mark**.

Ketty doesn't want the **NAMES** of those students who received a grade lower than 8. The report must be in descending order by grade — i.e., higher grades are entered first.

- If there is more than one student with the same grade (8-10) assigned to them, order those particular students by their name alphabetically.
- If the grade is lower than 8, use "NULL" as their name and list them by their grades in descending order.
- If there is more than one student with the same grade (1-7) assigned to them, order those particular students by their marks in ascending order.

[HackerRank Problem Link](https://www.hackerrank.com/challenges/the-report/problem?isFullScreen=true)

---

## Solutions

### Combined Solution 1 and 2

```sql
WITH lista AS (
    SELECT 
        s.name AS NAME, 
        g.grade AS GRADE, 
        s.marks AS punkty 
    FROM students s 
    JOIN grades g ON s.marks BETWEEN g.min_mark AND g.max_mark
    WHERE g.grade >= 8

    UNION ALL

    SELECT 
        NULL AS NAME, 
        g.grade AS GRADE, 
        s.marks AS punkty 
    FROM students s 
    JOIN grades g ON s.marks BETWEEN g.min_mark AND g.max_mark 
    WHERE g.grade < 8
)
SELECT 
    name, 
    grade, 
    punkty 
FROM lista
ORDER BY 
    grade DESC, 
    name ASC, 
    punkty ASC;

-- OR

WITH lista AS (
    SELECT 
        s.name AS NAME, 
        g.grade AS GRADE, 
        s.marks AS MARKS 
    FROM students s 
    JOIN grades g ON s.marks BETWEEN g.min_mark AND g.max_mark 
    WHERE g.grade >= 8

    UNION 

    SELECT 
        NULL AS NAME, 
        g.grade AS GRADE, 
        s.marks AS MARKS 
    FROM students s 
    JOIN grades g ON s.marks BETWEEN g.min_mark AND g.max_mark 
    WHERE g.grade < 8
) 
SELECT 
    name, 
    grade, 
    marks 
FROM lista 
ORDER BY 
    CASE 
        WHEN name IS NULL THEN 1 ELSE 0 
    END, 
    grade DESC, 
    CASE 
        WHEN name IS NULL THEN marks ELSE NULL 
    END ASC, 
    CASE 
        WHEN name IS NOT NULL THEN name ELSE NULL 
    END;
