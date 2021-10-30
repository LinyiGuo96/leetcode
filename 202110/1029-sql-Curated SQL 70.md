**1336. Number of Transactions per Visit (hard)**

![image](https://user-images.githubusercontent.com/51500878/139484905-fb4c245f-0866-43d0-bd1a-7ea6461dd932.png)

![image](https://user-images.githubusercontent.com/51500878/139486536-18a9dd26-77d7-4521-8e57-2abf1a82ff44.png)

![image](https://user-images.githubusercontent.com/51500878/139486563-617eea0f-c61a-4bf3-b37f-85c198bc2144.png)

![image](https://user-images.githubusercontent.com/51500878/139486592-71db4f6a-2311-405b-abec-12c15e689f07.png)

**Solution**

```sql
# My first method, but incorrect
select c.freq as transactions_count, count(c.id) as visits_count
from (
select a.user_id as id, count(b.amount) as freq
from visits a
left join transactions b
on a.user_id = b.user_id and a.visit_date = b.transaction_date
group by a.user_id, a.visit_date
) c
group by c.freq
```

```sql
# using CTE
with t as (select v.user_id as user_id, visit_date, count(t.transaction_date) as transaction_count
          from visits v left join transactions t
          on v.visit_date = t.transaction_date and v.user_id = t.user_id
          group by v.user_id, visit_date),
          
    row_nums as (select row_number() over () as rn
                from transactions
                union
                select 0)
                
select row_nums.rn as transactions_count, count(t.transaction_count) as visits_count
from t right join row_nums on transaction_count=rn
where rn <= (select max(transaction_count) from t)
group by rn
order by rn
;
```

**Note**

- CTE: common_table_expression. Take it as the name of pre-defining datasets at the beginning.

```sql
WITH expression_name[(column_name [,...])]
AS
    (CTE_definition)
SQL_statement;
```

```mssql
-- Define the CTE expression name and column list.  
WITH Sales_CTE (SalesPersonID, SalesOrderID, SalesYear)  
AS  
-- Define the CTE query.  
(  
    SELECT SalesPersonID, SalesOrderID, YEAR(OrderDate) AS SalesYear  
    FROM Sales.SalesOrderHeader  
    WHERE SalesPersonID IS NOT NULL  
)  
## Define the outer query referencing the CTE name.  
SELECT SalesPersonID, COUNT(SalesOrderID) AS TotalSales, SalesYear  
FROM Sales_CTE  
GROUP BY SalesYear, SalesPersonID  
ORDER BY SalesPersonID, SalesYear;
```

- The first method is not perfect, because it doesn't output those `transactions_count` with `visits_count = 0`. Remember this questions requires _`transactions_count` should take all values from `0` to `max(transactions_count)` done by one or more users._

- One of the difficulies in this problem is to create the column `transaction_count`. Because the total count of one subject at some day must not exceed the total count in `tranactions` dataset, therefore we could use use the following code to create a sequence from 0 to the total transaction times:

```sql
select row_number() over () as rn
from transactions
union
select 0
```

Then we could use `where rn <= (select max(transaction_count) from t)` to remove the rows with rn greater than max(transaction_count).
 




