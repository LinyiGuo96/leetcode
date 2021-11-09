**1412. Find the Quiet Students in All Exams (hard)**

![image](https://user-images.githubusercontent.com/51500878/140747913-b566ea8b-22fe-44dd-9009-f20255a9d4b6.png)

![image](https://user-images.githubusercontent.com/51500878/140747945-c9a3277a-891c-4cb7-a792-06a89cde632f.png)

![image](https://user-images.githubusercontent.com/51500878/140747962-3baeb789-c63e-4fa9-a929-173a02b90039.png)

**Solution**

```sql
# Method 1
with cte as (
    select exam_id, e.student_id, student_name, score, 
            rank() over (partition by exam_id order by score) rk1,
            rank() over (partition by exam_id order by score desc) rk2
    from exam e left join student s
    on e.student_id = s.student_id
)                         
                           
select distinct student_id, student_name
from cte
where student_id not in (select student_id from cte where rk1 = 1 or rk2 = 1)
order by student_id
```

```sql
# Method 2
with cte as (
    select exam_id, student_id, score, 
            rank() over (partition by exam_id order by score) rk1,
            rank() over (partition by exam_id order by score desc) rk2
    from exam 
)                         
                           
select *
from student
where student_id in (select student_id from cte group by student_id having min(rk1)>1 and min(rk2)>1)
order by student_id
```


**Note**

- The key idea is to use the window function `rank()` to flag the minimal and maximum student, and then exclude them from the student list.
- `CTE`(common table expression) table could help us/readers understand the program/code better.
- The second method avoids using _left join_ here.






