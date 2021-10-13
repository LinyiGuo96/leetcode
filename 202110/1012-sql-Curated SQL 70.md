**1264. Page Recommendations**

![image](https://user-images.githubusercontent.com/51500878/137053933-b5d969df-5768-4a91-918d-fd61b75cfd91.png)

Write an SQL query to recommend pages to the user with user_id = 1 using the pages that your friends liked. It should not recommend pages you already liked.

Return result table in any order without duplicates.

The query result format is in the following example:

![image](https://user-images.githubusercontent.com/51500878/137054000-e6357f75-18bf-4a7f-945e-838f85eacbcf.png)

![image](https://user-images.githubusercontent.com/51500878/137054016-b66f3060-60a1-4e30-95f8-b2f284902b2c.png)

**Solution**

```sql
# MS SQL
# Too slow
with a as (
    select user1_id as id
    from friendship
    where user2_id = 1
    union
    select user2_id as id
    from friendship
    where user1_id = 1
),
b as (
    select *
    from likes
    where page_id not in (select page_id from likes where user_id = 1)
)

select distinct page_id as recommended_page
from a, b
where a.id = b.user_id
```

```sql
# My SQL
select distinct page_id as recommended_page
from
(select
case
when user1_id=1 then user2_id
when user2_id=1 then user1_id
end as user_id
from Friendship) a
join Likes
on a.user_id=Likes.user_id
where page_id not in (select page_id from Likes where user_id=1)
```

**Note**

- When need to combine different columns into one column, try `case when ... end as new_var`


**1270. All People Report to the Given Manager**

![image](https://user-images.githubusercontent.com/51500878/137057832-bdc9626a-4c6e-4341-b786-775f6c68abfc.png)

![image](https://user-images.githubusercontent.com/51500878/137057849-fabbcf8b-24a1-4b34-91e7-a2ed8b3b527c.png)

**Solution**

```sql
# for some reason, too slow
select a.employee_id 
from employees a
left join employees b
on a.manager_id = b.employee_id
left join employees c
on b.manager_id = c.employee_id
where c.manager_id = 1 and a.employee_id != 1;
```

```sql
SELECT e1.employee_id
FROM Employees e1,
     Employees e2,
     Employees e3
WHERE e1.manager_id = e2.employee_id
  AND e2.manager_id = e3.employee_id
  AND e3.manager_id = 1 
  AND e1.employee_id != 1
```
