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



