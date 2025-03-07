# SQL Task and Solutions

## Task Description
New Companies
Amber's conglomerate corporation just acquired some new companies. Each of the companies follows this hierarchy:
   
Founder -> Lead Manager -> Senior Manager -> Manager -> -> Employee

Given the table schemas below, write a query to print the company_code, founder name, total number of lead managers, total number of senior managers, total number of managers, and total number of employees. Order your output by ascending company_code.
Note: 
The tables may contain duplicate records.
The company_code is string, so the sorting should not be numeric. For example, if the company_codes are C_1, C_2, and C_10, then the ascending company_codes will be C_1, C_10, and C_2.

[HackerRank Problem Link](https://www.hackerrank.com/challenges/the-company/problem?isFullScreen=true)

---

## Solutions

```sql
With listalm as (
   select
      c.company_code as kraj,
      c.founder as zalozyciel,
      coalesce(count(distinct l.lead_manager_code), 0) as liczbalm
   from company c left
   join lead_manager l on c.company_code=l.company_code group by c.company_code, c.founder
),
listasm as (
   select
      sm.company_code as kraj2,
      coalesce(count(distinct sm.senior_manager_code), 0) as liczbasm
   from  senior_manager sm group by sm.company_code
),
listam as (
   select
      m.company_code as kraj3,
      coalesce(count( distinct m.manager_code), 0) as liczbam
   from  manager m
   group by m.company_code
),
listae as (
   select
      e.company_code as kraj4,
      coalesce(count(distinct e.employee_code), 0) as liczbae
   from  employee e
   group by e.company_code)
select llm.kraj, 
llm.zalozyciel, 
llm.liczbalm, 
lsm.liczbasm, 
lm.liczbam, 
le.liczbae 
from listalm llm 
left join listasm lsm on llm.kraj=lsm.kraj2 
left join listam lm on lsm.kraj2=lm.kraj3 
left join listae le on lm.kraj3=le.kraj4
order by llm.kraj asc;
