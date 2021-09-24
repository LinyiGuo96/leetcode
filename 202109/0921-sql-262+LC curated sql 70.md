**262. Trips and Users (hard)**

![image](https://user-images.githubusercontent.com/51500878/134169058-c0cc1aee-8d57-4c61-9fed-f5752f225969.png)

![image](https://user-images.githubusercontent.com/51500878/134169090-e0f84d08-a3e2-49a9-8381-cda51f02aae3.png)

![image](https://user-images.githubusercontent.com/51500878/134169192-3d34abfc-feb6-43e3-86c3-47ee5eea76a5.png)

![image](https://user-images.githubusercontent.com/51500878/134169225-187cb372-7be3-401a-8e0b-79ea26d14795.png)

**Solution**

```sql
select t.request_at Day,
        round((count(if(t.status != 'completed', true, null))/count(*)), 2) as 'Cancellation Rate'
from trips t
where t.client_id in (select users_id from users where banned = 'No')
and t.driver_id in (select users_id from users where banned = 'No')
and t.request_at between '2013-10-01' and '2013-10-03'
group by t.request_at;
```

**Note**

- This problem only output a number in the end.
- `if(condition, do-when-true, do-when-false)` one line if statement in sql
- `null` means no value. `is null` and `is not null` are usually used as a `where` condition. 


**511. Game Play Analysis I (easy)**

![image](https://user-images.githubusercontent.com/51500878/134176077-da2481fd-f46d-4ed1-8c6d-f70468ede18d.png)

![image](https://user-images.githubusercontent.com/51500878/134176112-a41906e2-7623-494e-ae68-dbba011f662a.png)

**Solution**

```sql
select player_id, min(event_date) as first_login 
from activity
group by player_id
```

**512. Game Play Analysis II (easy)**

Similar to the previous question, same `Activity` table.

![image](https://user-images.githubusercontent.com/51500878/134176622-98b83154-0dca-491d-948a-d22416d6f3ad.png)

**Solution**

```sql
select player_id, device_id
from activity
where (player_id, event_date) in (select player_id, min(event_date) from activity group by player_id)
```

**Note**

- `(var1, var2, var3, ...) in (select ... from ... where ... group by ... order by ...)`. Try to understand the flexible use of the _sub_sql process.

**534. Game Play Analysis III (medium)**

Similar to the previous question, same `Activity` table.

![image](https://user-images.githubusercontent.com/51500878/134179344-d79cd36e-47c8-4641-9fa0-f3fd1220a81d.png)

**Solution**

```sql
select a.player_id, a.event_date, sum(b.games_played) as games_played_so_far
from activity a 
# it turns out if I use inner join here, it will be faster
left join activity b 
on a.player_id = b.player_id and a.event_date >= b.event_date
group by a.player_id, a.event_date
```

**Note**

- When some problem is required to compute the cumulative sum/product, this pattern should be universal, that is, we left(inner) join the same table with the date before that one in the first table within the same subject(ID), and then take the sum.
- Note you need to add a `group by` clause after joining, otherwise the final output would only have one obs


**550. Game Play Analysis IV (Medium)**

Similar to the previous question, same `Activity` table.

![image](https://user-images.githubusercontent.com/51500878/134183708-ebaa35ce-9c6b-4e3d-9d93-de83e90c5df5.png)

**Solution**

```sql
select round(count(t2.player_id)/count(t1.player_id), 2) as fraction
from (select player_id, min(event_date) as first_login from activity group by player_id) t1 
left join activity t2
on t1.player_id = t2.player_id and t1.first_login - t2.event_date = -1
```

**Note**

- This problem is valuable for me. Because I found one problem/inaccurate place of my pervious understanding.
        Take the `left join` as an example: `a.val1` contains all `val1` in a (suppose there is no `where` clause later), but `b.val1` contains only thsoe have been merged to `a` based on the condition `on a.val2 = b.val2`. **Note**, if one obs in `a` doesn't have been merged with `b`, then `b.val1` is `null`.
        
        ```sql
        select a.val1, b.val1
        from (...) a left join (...) b 
        on a.val2 = b.val2
        ...
        ```

**570. Managers with at Least 5 Direct Reports (Medium)**

![image](https://user-images.githubusercontent.com/51500878/134190966-fa540a8d-b85e-47fa-83d7-93fc3f466fa8.png)

**Solution**

```sql
# my program
select b.name
from (select managerid from employee group by managerid having count(managerid) >=5) a
left join employee b
on a.managerid = b.id

# Failed when there is no required manager. The output of my program is [null] but the expected value is []
# because I used b.name here, and b is the second table in the `left join`, so this would generate null value
```

```sql
select name from employee 
where id in 
(select managerId from Employee
group by managerId
having count(managerId)>=5) 
```

**Note**

- `having`: The HAVING clause was added to SQL because the WHERE keyword cannot be used with aggregate functions. **IMPORTANT**


**571. Find Median Given Frequency of Numbers (hard)**

![image](https://user-images.githubusercontent.com/51500878/134193252-d481a5c4-27bc-42ea-9ee2-becb3de34909.png)

**Solution**

```sql
select avg(n.number) as median
from numbers n
where n.frequency >= abs((select sum(frequency) from numbers where number <= n.number) - 
                        (select sum(frequency) from numbers where number >= n.number))
```

**Note**

- Here is an explanation: 
> try to explain this in a hopefully clearer way.
>
> suppose number x has frequency of n, and total frequency of other numbers that are on its left is l, on its right is r.
> 
> the equation above is (n+l) - (n+r) = l - r, x is median if l==r, of course.
> 
> When l != r, as long as n can cover the difference, x is the median.


**580. Count Student Number in Departments**

![image](https://user-images.githubusercontent.com/51500878/134201176-01887b1b-e5a9-4b2d-81f6-c4f55bed0c40.png)

![image](https://user-images.githubusercontent.com/51500878/134201229-2c62a843-725c-4cf9-bfb1-a6ab4e8ebaf9.png)

**Solution**

```sql
# my method
select c.*
from (select a.dept_name, count(b.student_id) as student_number
from department a
left join student b
on a.dept_id = b.dept_id
group by a.dept_name) c
order by c.student_number desc, c.dept_name

# imporved version
select a.dept_name, count(b.student_id) as student_number
from department a
left join student b
on a.dept_id = b.dept_id
group by a.dept_name
order by student_number desc, a.dept_name
```

**Note**

- Great! This is the first problem I solved by myself today! Cheers! (Although it's quite tedious...)
- The improved version shows that we could use derived variable in `order` clause (**important**)
- Also, note the variable used in `group by` clause don't require `distinct` anymore when selecting.
 

**586. Customer Placing the Largest Number of Orders**

![image](https://user-images.githubusercontent.com/51500878/134239417-8493d6e3-c17f-4ecd-8a5e-5b452f8add4c.png)

![image](https://user-images.githubusercontent.com/51500878/134239453-c37cc9db-1db2-41fe-af0a-65a13ccc16dc.png)


**Solution**

```sql
select customer_number 
from orders
group by customer_number
order by count(*) desc
limit 1;
```

```sql
# For the follow-up question
select customer_number
from orders 
group by customer_number
having count(order_number) = (
        select count(order_number) as cnt
        from orders
        group by customer_number
        order by cnt desc
        limit 1
)
```

**Note**

- In the last problem, we noticed that the derived variable could be used in the `order` clause. Similarly, `order by count(*) desc`, `order` clause could also be used with  some function expressions.
- For this type of question, if we need to extract the first or some specific `ith` item(s), use `limit x offset y` clause! 

**603. Consecutive Available Seats**

![image](https://user-images.githubusercontent.com/51500878/134240778-7800bfb0-f6dc-4888-abf3-4eaf0012b9d5.png)

**Solution**

```sql
select distinct a.seat_id
from cinema a, cinema b
where abs(a.seat_id - b.seat_id) = 1 and a.free = 1 and b.free=1
order by a.seat_id
```


**607. Sales Person**

![image](https://user-images.githubusercontent.com/51500878/134242550-c0e42463-cb8e-4b5a-8bf8-370cb1c4598e.png)

![image](https://user-images.githubusercontent.com/51500878/134242591-02e7f566-fac9-445c-8db9-659be796b658.png)


**Solution**

```sql
select s.name
from salesperson s
where s.name not in (
    select s1.name
    from orders o
    left join company c
    on o.com_id = c.com_id
    left join salesperson s1
    on o.sales_id = s1.sales_id
    where c.name = 'RED'
)
```


**608. Tree Node (medium)**

![image](https://user-images.githubusercontent.com/51500878/134243785-0b5fbe8c-fff7-43a6-aacb-7395fc5eb4ce.png)

![image](https://user-images.githubusercontent.com/51500878/134243822-84e5b672-51ba-41cb-8a1d-f22e9dd6e5a5.png)

![image](https://user-images.githubusercontent.com/51500878/134243869-42799a9f-72de-488d-a3aa-8f6b5472c20c.png)

**Solution**

```sql
# My method
select distinct t.id as Id, case
                when p.id is NULL then "Root"
                when c.id is NULL then "Leaf"
                else "Inner"
                end as Type
from tree t
left join tree p
on t.p_id = p.id
left join tree c
on t.id = c.p_id;
```

```sql
# Similar method but simple
SELECT DISTINCT t1.id, (
    CASE
    WHEN t1.p_id IS NULL  THEN 'Root'
    WHEN t1.p_id IS NOT NULL AND t2.id IS NOT NULL THEN 'Inner'
    WHEN t1.p_id IS NOT NULL AND t2.id IS NULL THEN 'Leaf'
    END
) AS Type 
FROM tree t1
LEFT JOIN tree t2
ON t1.id = t2.p_id
```

**Note**

- `case when ... then ... end as VarName`


**610. Triangle Judgement**

![image](https://user-images.githubusercontent.com/51500878/134247528-45d4a629-9210-44e5-8836-142bdc2f3dbf.png)

**Solution**

```sql
# actually alias `t` could be ignored
select t.*, case 
                when t.x + t.y <= t.z or t.x + t.z <= t.y or t.y + t.z <= t.x then "No"
                else "Yes"
                end as triangle
from triangle t 
```


**613. Shortest Distance in a Line**

![image](https://user-images.githubusercontent.com/51500878/134250220-5a17ef06-e3bc-4cd9-abf1-d396519896e7.png)

**Solution**

```sql
select min(abs(a.x - b.x)) as shortest
from point a, point b
where a.x != b.x
```


**615. Average Salary: Departments VS Company**

![image](https://user-images.githubusercontent.com/51500878/134250647-52da3c1c-d0a7-40d2-98ca-1ecfb1c96d49.png)

![image](https://user-images.githubusercontent.com/51500878/134250665-c5118a8b-a9bb-4ae7-8a78-bd4c5bdea0d1.png)

**Solution**

```sql
select distinct pay_month, department_id, (CASE WHEN department_avg_salary > company_avg_salary THEN 'higher'
     WHEN department_avg_salary < company_avg_salary THEN 'lower'
     WHEN department_avg_salary = company_avg_salary THEN 'same' END) AS comparison
     from (select a.employee_id, amount, pay_date, department_id, left(pay_date, 7) as pay_month, 
           avg(amount) over(partition by a.pay_date) as company_avg_salary,
           avg(amount) over(partition by a.pay_date, b.department_id) as department_avg_salary
           from salary a
           left join employee b 
           # using join here is faster
           on a.employee_id = b.employee_id) temp;
```

**Note**

- The `LEFT()` function extracts a number of characters from a string (starting from left). The `RIGHT()` function extracts a number of characters from a string (starting from right).
- **Important** `partition by`: A PARTITION BY clause is used to partition rows of table into groups. It is useful when we have to perform a calculation on individual rows of a group using other rows of that group.

> It is always used inside OVER() clause.  
> The partition formed by partition clause are also known as Window.  
> This clause works on windows functions only. Like- RANK(), LEAD(), LAG() etc.  
> If this clause is omitted in OVER() clause, then whole table is considered as a single partition.  

```
Window_function ( expression ) 
       Over ( partition by expr [order_clause] [frame_clause] ) 
```

