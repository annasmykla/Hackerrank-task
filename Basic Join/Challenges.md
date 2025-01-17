# SQL Task and Solutions

## Task Description

Julia asked her students to create some coding challenges. Write a query to print the hacker_id, name, and the total number of challenges created by each student. 
Sort your results by the total number of challenges in descending order. If more than one student created the same number of challenges, then sort the result by hacker_id. 
If more than one student created the same number of challenges and the count is less than the maximum number of challenges created, then exclude those students from the result.

[HackerRank Problem Link]([[ https://www.hackerrank.com/challenges/challenges/problem?isFullScreen=false)]

---

## Solutions

```sql

WITH lista AS (
    SELECT 
        h.hacker_id AS id, 
        h.name AS imie, 
        COUNT(ch.challenge_id) AS liczba 
    FROM 
        hackers h 
    JOIN 
        challenges ch 
    ON 
        h.hacker_id = ch.hacker_id
    GROUP BY 
        h.hacker_id, h.name
),
lista2 AS (
    SELECT 
        l.liczba AS liczbwykl 
    FROM 
        lista l 
    WHERE 
        l.liczba IN (
            SELECT liczba 
            FROM lista 
            GROUP BY liczba 
            HAVING COUNT(*) >= 2
        )
        AND l.liczba < (SELECT MAX(liczba) FROM lista)
)
SELECT 
    l.id, 
    l.imie, 
    l.liczba 
FROM 
    lista l 
WHERE 
    l.liczba NOT IN (SELECT l2.liczbwykl FROM lista2 l2) 
ORDER BY 
    l.liczba DESC, l.id;
