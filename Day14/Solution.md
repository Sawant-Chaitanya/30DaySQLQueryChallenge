```SQL
-- Solution 1:
select
   generate_series(min(serial_no), max(serial_no)) AS missing_serial_no 
from
   invoice 
except
select
   serial_no 
from
   invoice 
order by
   1;

```


```SQL
-- Solution in MSSQL Server:
with cte_data as 
(
   select
      min(serial_no) as min_serial_no,
      max(serial_no) as max_serial_no 
   from
      invoice
)
,
cte as 
(
   select
      min_serial_no as n 
   from
      cte_data 
   union all
   select
      n + 1 as n 
   from
      cte 
   where
      n < (
      select
         max_serial_no 
      from
         cte_data) 
)
select
   n as missing_serial_no 
from
   cte 
except
select
   serial_no 
from
   invoice 
order by
   1;
```
```
