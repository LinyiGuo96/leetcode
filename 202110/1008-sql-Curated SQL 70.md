**1251. Average Selling Price**

![image](https://user-images.githubusercontent.com/51500878/136640783-96b8e369-0d9a-4b01-94c9-9e4c573f2248.png)

![image](https://user-images.githubusercontent.com/51500878/136640796-114f717b-7705-42a2-b8d3-6278a72a3bde.png)

![image](https://user-images.githubusercontent.com/51500878/136640804-781fed60-8156-47ba-82b7-7c472c3bed05.png)

![image](https://user-images.githubusercontent.com/51500878/136640811-649b1539-f846-49ea-8ca0-84421a25cbf9.png)


**Solution**

```sql
select p.product_id, round(sum(p.price * u.units)/sum(u.units), 2) as average_price 
from prices p
left join unitssold u
on p.product_id = u.product_id and u.purchase_date between p.start_date and p.end_date
group by p.product_id
```

 
**1225. Report Contiguous Dates (hard)**

![image](https://user-images.githubusercontent.com/51500878/136641119-0392f6cb-6c34-4264-8637-9ebd6273b2d6.png)

![image](https://user-images.githubusercontent.com/51500878/136641140-6f074213-0159-426a-937e-f1b5a456765f.png)

**Solution**

_Note we used MS SQL here not MySQL._

```sql
-- MSSQL

with a  as (
(select fail_date as date,
       'failed' as period_state
       from failed)
union all 
 (select success_date as date,
         'succeeded' as period_state
         from succeeded)
    ),
    
  b as (    
select date,
       period_state,
       row_number() over (order by period_state, date asc) as seq
   from a where date between '2019-01-01' and '2019-12-31'
         )

select period_state, min(date) as start_date, max(date) as end_date from b
-- THINK ABOUT WHY WE USED dateadd(d, -seq, date)
group by dateadd(d, -seq, date),period_state
order by start_date asc
```

**Note**

- `with` clause: The SQL WITH clause allows you to give a sub-query block a name (a process also called sub-query refactoring), which can be referenced in several places within the main SQL query. 

   - The clause is used for defining a temporary relation such that the output of this temporary relation is available and is used by the query that is associated with the WITH clause.
   - Queries that have an associated WITH clause can also be written using nested sub-queries but doing so add more complexity to read/debug the SQL query.
   - WITH clause is not supported by all database system.
   - The name assigned to the sub-query is treated as though it was an inline view or table
   - The SQL WITH clause was introduced by Oracle in the Oracle 9i release 2 database.

```sql
WITH temporaryTable (averageValue) as
    (SELECT avg(Attr1)
    FROM Table)
    SELECT Attr1
    FROM Table, temporaryTable
    WHERE Table.Attr1 > temporaryTable.averageValue;
```    

- `DATEADD(interval, number, date)`: The `DATEADD()` function adds a time/date interval to a date and then returns the date.

- `window functions` are functions that operate on a set of rows and return a single value for each row from the underlying query. A window function uses values from the rows in a window to calculate the returned values. When you use a window function in a query, define the window using the `OVER()` clause. The `OVER()` clause has athe following capabilities:
   - Defines window partitions to form groups of rows (`PARTITION BY` clause).
   - Orders rows within a partition (`ORDER BY` clause).
   
> select emp_name, dealer_id, sales, avg(sales) over() as avgsales from q1_sales;

- The key idea of this problem lies in the final `group clause`: `group by dateadd(day, -seq, date), period_state`. Because if several rows have continuous date and continuous row number, then their date minus the sequence number should be the same (if we didn't care about their states, these obs should belong to the same group). Then we group by their state (fail or succeed) again to avoid.

