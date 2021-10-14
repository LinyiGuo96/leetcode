**1280. Students and Examinations**

![image](https://user-images.githubusercontent.com/51500878/137134520-2a7bede4-206c-43e1-bd5e-359923799e38.png)

![image](https://user-images.githubusercontent.com/51500878/137134613-1636253c-d3e8-40ad-83fe-8548318ad5ac.png)

![image](https://user-images.githubusercontent.com/51500878/137134665-ff66ba6c-cc35-43e0-a654-b87a27f1dcf7.png)

![image](https://user-images.githubusercontent.com/51500878/137134692-00edcc13-8abf-484f-ae40-c9ec8f6739c5.png)

**Solution**

```sql
select a.*, ifnull(b.attended_exams, 0) as attended_exams
from (select * from students, subjects) a
left join (select *, count(*) as attended_exams from examinations group by student_id, subject_name) b
on a.student_id = b.student_id and a.subject_name = b.subject_name
order by a.student_id, a.subject_name
```

**Note**

- `ifnull()` could be used to replace some `case when` statements
- Don't be fear to use **subqueries**, try to use them boldly!


**1285. Find the Start and End Number of Continuous Ranges**

![image](https://user-images.githubusercontent.com/51500878/137244984-7446d926-f968-494d-9396-6e888536c2e0.png)

![image](https://user-images.githubusercontent.com/51500878/137245004-6fa23593-7914-4f63-a48a-fa02d358bf02.png)

**Solution**

```sql
select min(log_id) as start_id, max(log_id) as end_id
from (select log_id, row_number() over (order by log_id) as num from logs) a
group by log_id - num
```

**Note**

- `row_number() over (order by log_id)`. GET!
- The key idea of this problem is: if log_ids are continuous then their differences regarding the order are the same.
![image](https://user-images.githubusercontent.com/51500878/137246171-1117e616-2f0d-4aa0-a67d-3c3a2625d888.png)






