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

```sql

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
