**1341. Movie Rating**

![image](https://user-images.githubusercontent.com/51500878/139560074-d539e46e-4c53-49ae-a3a0-7f3689fd1458.png)

![image](https://user-images.githubusercontent.com/51500878/139560079-76c6a0da-2ffe-4c40-89b0-15b0793e4af1.png)

![image](https://user-images.githubusercontent.com/51500878/139560084-e0217ec8-edf1-4e66-b586-c8faa04f3418.png)

![image](https://user-images.githubusercontent.com/51500878/139560096-9acc2a96-b3af-4b24-9b4f-07c137022b65.png)

**Solution**

```sql
(select b.name as results
from MovieRating a left join users b
on a.user_id = b.user_id
group by a.user_id
order by count(a.movie_id) desc, b.name
limit 1)

union

(
select c.title as results
from MovieRating a left join movies c
on a.movie_id = c.movie_id
where a.created_at between '2020-02-01' and '2020-02-29'
group by a.movie_id
order by avg(a.rating) desc, c.title
limit 1
)
```

**1350. Students With Invalid Departments**

![image](https://user-images.githubusercontent.com/51500878/139560609-e274f952-f940-40b7-9bd3-52760c18bb1e.png)

![image](https://user-images.githubusercontent.com/51500878/139560612-0409d40b-f6a8-4f6e-a5e7-3079f5c69039.png)

![image](https://user-images.githubusercontent.com/51500878/139560619-50e18e99-9aaf-4c42-8cda-0c00bfcbbcec.png)

![image](https://user-images.githubusercontent.com/51500878/139560621-e9e8a948-916e-40fe-8a79-7c91427f4d21.png)

![image](https://user-images.githubusercontent.com/51500878/139560624-95eb2b6d-15ec-4142-8b89-31c877dff378.png)


**Solution**

```sql
select id, name from students
where department_id not in (select id from Departments )
```

**1378. Replace Employee ID With The Unique Identifier**

![image](https://user-images.githubusercontent.com/51500878/139560874-5b2c85b3-3b4b-4a9c-b342-6fcccc05722e.png)

![image](https://user-images.githubusercontent.com/51500878/139560876-e1064950-2c4b-4614-aac5-a48333ea1df9.png)

![image](https://user-images.githubusercontent.com/51500878/139560883-3f5bb2c7-938c-46bd-ae45-8c01f015737d.png)

**Solution**

```sql
select b.unique_id, a.name
from employees a
left join employeeUNI b
on a.id = b.id
```

**1407. Top Travellers**

![image](https://user-images.githubusercontent.com/51500878/139561199-3b9a5985-0e89-4ad8-a413-720bbd7ac230.png)

![image](https://user-images.githubusercontent.com/51500878/139561202-3b0a965f-9bd3-4ef7-9bcd-821d7314b5f3.png)

![image](https://user-images.githubusercontent.com/51500878/139561207-5dd98eb1-2aeb-4333-b75a-cf237bc1d34a.png)

![image](https://user-images.githubusercontent.com/51500878/139561210-52c1e309-1fb2-44d3-81fe-0b4fb51b88e8.png)

**Solution**

```sql
select a.name, ifnull(sum(b.distance), 0) as travelled_distance 
from users a left join rides b
on a.id = b.user_id
group by a.id
order by travelled_distance desc, a.name 
```

```sql
# faster version
select a.name, ifnull(b.tot,0) as travelled_distance
from Users as a
left join (select user_id, sum(distance) as tot from Rides 
          group by user_id ) as b
on a.id = b.user_id
order by 2 desc, 1
```
