**1112. Highest Grade For Each Student**

![image](https://user-images.githubusercontent.com/51500878/135364812-959c26e8-419c-497d-b779-4470ac95b45d.png)

![image](https://user-images.githubusercontent.com/51500878/135364833-d641ad47-136a-41a8-8b58-4fe0b977b562.png)


**Solutions**

```sql
select student_id, min(course_id) as course_id, grade
from enrollments
where (student_id, grade) in (select student_id, max(grade) from enrollments group by student_id)
group by student_id
order by student_id
```

**Note**

- I am so tired!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! 
- But I still can't sleep!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! 
- Because I need to stand by for the project in my current company!!!!!!!!!!!!!!!!!!!
- And I won't get any reward, because this is the `company culture` !!!!!!!!!!!!!!!!! 


**1113. Reported Posts**

![image](https://user-images.githubusercontent.com/51500878/135380999-a62ac7b2-8965-42dc-b84a-b9db2ea95e63.png)

![image](https://user-images.githubusercontent.com/51500878/135381134-c3373d1e-c5f2-4ea2-9e46-4fb9075254d9.png)

**Solution**

```sql
# my method
select extra as report_reason, count(distinct post_id) as report_count
from actions
where action_date = '2019-07-04' and action = 'report' and extra is not null
group by extra
```
```sql
select extra as report_reason, count(distinct post_id) as report_count
from actions
where action_date = '2019-07-04' and action = 'report'
group by extra
```

**Note**

- I am going to sleep :( I just checked the team lead and my manager's status, they are all leave now. Lol, they told me to stand by this evening and after I submitted my programs, they even didn't notice me(or us) whether we could leave or not? Ridiculous. Bye-Bye, I am gonna leave, absolutely.
- We don't need to add `extra is not null`, because when using `action = 'report'`, we already assume extra is not null















