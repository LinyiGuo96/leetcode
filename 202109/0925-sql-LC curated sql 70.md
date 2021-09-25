**1075. Project Employees I**

![image](https://user-images.githubusercontent.com/51500878/134785937-b12f5db9-138f-4792-a40d-f17fdb3c2e18.png)

![image](https://user-images.githubusercontent.com/51500878/134785950-1cfaa743-2aec-4862-8d8a-c8ed9a625ad2.png)

**Solutions**

```sql
select p.project_id, round(avg(e.experience_years),2) as average_years
from project p
# inner join is faster 
left join employee e 
on p.employee_id = e.employee_id
group by p.project_id;
```

**Note**

- **IMPORTANT** A LEFT JOIN is absolutely not faster than an INNER JOIN . In fact, it's slower; by definition, an outer join ( LEFT JOIN or RIGHT JOIN ) has to do all the work of an INNER JOIN plus the extra work of null-extending the results


**1076. Project Employees II**

![image](https://user-images.githubusercontent.com/51500878/134786103-4d959251-6f5d-4aca-8c7c-43ea9a22f200.png)

![image](https://user-images.githubusercontent.com/51500878/134786109-5d1a0165-a1a3-4a2d-bf36-c4fec21e4801.png)


**Solutions**

```sql
select p.project_id
from project p
group by p.project_id
having count(p.employee_id) = (
    select count(employee_id)
    from project 
    group by project_id
    order by count(employee_id) desc
    limit 1
)
```

**1077. Project Employees III**

![image](https://user-images.githubusercontent.com/51500878/134786275-f781bf90-e885-4db6-8258-1052c0ae06ca.png)

![image](https://user-images.githubusercontent.com/51500878/134786289-1ba44efb-8989-4143-b10c-2b79c5a81bfd.png)

**Solution**

```sql
select p.project_id, e.employee_id
from project p
left join employee e
on p.employee_id = e.employee_id
where (p.project_id, e.experience_years) in (
    select p.project_id, max(e.experience_years)
    from project p
    left join employee e
    on p.employee_id = e.employee_id
    group by p.project_id
);
```

**1082. Sales Analysis I**

![image](https://user-images.githubusercontent.com/51500878/134786662-b7d0eee8-17b1-405e-ba58-9783169db209.png)

![image](https://user-images.githubusercontent.com/51500878/134786668-4c8da346-b91a-4019-938e-c6571690c1c0.png)

