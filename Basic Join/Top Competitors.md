# SQL Task and Solutions

## Task Description

Julia just finished conducting a coding contest, and she needs your help assembling the leaderboard! 
Write a query to print the respective hacker_id and name of hackers who achieved full scores for more than one challenge. Order your output in descending order by the total number of challenges in which the hacker earned a full score. 
If more than one hacker received full scores in same number of challenges, then sort them by ascending hacker_id.

[HackerRank Problem Link](https://www.hackerrank.com/challenges/full-score/problem?isFullScreen=true)

---

## Solutions
```sql
WITH Lista AS (
    SELECT 
        h.name AS Imie, 
        s.hacker_id AS IdHacker, 
        s.challenge_id AS IdWyzwanie, 
        s.score AS Wynik, 
        d.score AS WynikMax
    FROM hackers h
    JOIN submissions s ON h.hacker_id = s.hacker_id
    JOIN challenges c ON s.challenge_id = c.challenge_id
    JOIN difficulty d ON c.difficulty_level = d.difficulty_level
    WHERE s.score = d.score
),
Lista2 AS (
    SELECT 
        l.IdHacker, 
        l.Imie, 
        COUNT(DISTINCT l.IdWyzwanie) AS LiczbaMax
    FROM Lista l
    GROUP BY l.IdHacker, l.Imie
)
SELECT 
    l2.IdHacker, 
    l2.Imie
FROM Lista2 l2
WHERE l2.LiczbaMax > 1
ORDER BY l2.LiczbaMax DESC, l2.IdHacker ASC;
