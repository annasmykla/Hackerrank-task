# SQL Task and Solutions

## Task Description

A median is defined as a number separating the higher half of a data set from the lower half. Query the median of the Northern Latitudes (LAT_N) from STATION and round your answer to  decimal places.
[HackerRank Problem Link](https://www.hackerrank.com/challenges/weather-observation-station-20/problem?isFullScreen=true)

## Solutions

### Combined Solution 1 and 2

```sql
With liczba as (
    select 
        count(lat_n)as liczbaszer from station
), 
ranking as (
    select
        lat_n as szer,
        rank() over (order by lat_n ) as rank
    from station
), 
mediana1 as ( 
    select
        r.szer as mnp
    from ranking r
    where r.rank=(select (l.liczbaszer+1)/2 from liczba l) 
), 
mediana2 as ( 
    select
        r.szer as mp1
    from ranking r
    where r.rank=(select l.liczbaszer/2 from liczba l) 
), 
mediana3 as ( 
    select
        r.szer as mp2
    from ranking r
    where r.rank=(select l.liczbaszer/2+1 from liczba l) 
), 
mediana4 as ( 
    select
        ((select m2.mp1 from mediana2 m2)+(select m3.mp2 from mediana3 m3))/2 as mp
), 
medianabz as (
    select 
        case 
            when (select l.liczbaszer from liczba l) %2=0
                then (select m4.mp from mediana4 m4) 
            else (select m1.mnp from mediana1 m1) 
        end as mediana5) 
select
    cast(round(mbz.mediana5, 4) as decimal (10,4))
from medianabz mbz;
```
OR
```sql
WITH liczba AS
(
    SELECT
        COUNT(lat_n) AS liczbaszer 
    FROM station
),
ranking AS (
    SELECT
        lat_n AS szer,
        RANK() OVER (ORDER BY lat_n) AS rank 
    FROM station
),
mediana AS (
    SELECT 
        CASE 
            WHEN l.liczbaszer % 2 = 0 
                THEN (SELECT AVG(r.szer) FROM ranking r WHERE r.rank IN (l.liczbaszer / 2, l.liczbaszer / 2 + 1))
            ELSE (SELECT r.szer FROM ranking r WHERE r.rank = (l.liczbaszer + 1) / 2)
        END AS mediana5
    FROM liczba l
)
SELECT
    CAST(ROUND(m.mediana5, 4) AS DECIMAL(10, 4)) AS zaokraglona_mediana5
FROM mediana m;
```
