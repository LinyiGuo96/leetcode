**1336. Number of Transactions per Visit (hard)**

![image](https://user-images.githubusercontent.com/51500878/139484905-fb4c245f-0866-43d0-bd1a-7ea6461dd932.png)

![image](https://user-images.githubusercontent.com/51500878/139486536-18a9dd26-77d7-4521-8e57-2abf1a82ff44.png)

![image](https://user-images.githubusercontent.com/51500878/139486563-617eea0f-c61a-4bf3-b37f-85c198bc2144.png)

![image](https://user-images.githubusercontent.com/51500878/139486592-71db4f6a-2311-405b-abec-12c15e689f07.png)

**Solution**

```sql

```

**Note**

- CTE: common_table_expression. 

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






