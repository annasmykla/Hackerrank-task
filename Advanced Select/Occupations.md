# SQL Task and Solutions

## Task Description

Pivot the Occupation column in OCCUPATIONS so that each Name is sorted alphabetically and wyświetlane pod odpowiadającym mu zajęciem. 
The output should consist of four columns (Doctor, Professor, Singer, and Actor) in that specific order, with their respective names listed alphabetically under each column.

Note: Print NULL when there are no more names corresponding to an occupation.

[HackerRank Problem Link](https://www.hackerrank.com/challenges/occupations/problem?isFullScreen=true)

---

## Solutions

### Combined Solution 1 and 2

```sql

WITH RankedOccupations AS (
    SELECT 
        name AS imie,
        occupation AS zawod,
        ROW_NUMBER() OVER (PARTITION BY occupation ORDER BY name) AS ranking
    FROM occupations
)
SELECT 
    MAX(CASE WHEN zawod = 'doctor' THEN imie END) AS d,
    MAX(CASE WHEN zawod = 'professor' THEN imie END) AS p,
    MAX(CASE WHEN zawod = 'singer' THEN imie END) AS s,
    MAX(CASE WHEN zawod = 'actor' THEN imie END) AS a
FROM RankedOccupations
GROUP BY ranking;
```
or
```sql
Select
[doctor] as Doktorzy,
[professor] as Profesorowie,
[singer] as Piosenkarze,
[actor] as Aktorzy
from (select name as imie, occupation as zawod, row_number() over (partition by occupation order by name) as ranking from occupations) as tabelarank
pivot
(
    Max(imie)
    for zawod in ([doctor], [professor], [singer], [actor])
) as pivotable;
```
