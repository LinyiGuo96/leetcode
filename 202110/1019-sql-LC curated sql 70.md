**1294. Weather Type in Each Country**

![image](https://user-images.githubusercontent.com/51500878/137947970-8794eaa3-3475-4422-a4de-731f8328aba0.png)

![image](https://user-images.githubusercontent.com/51500878/137948015-979d9879-b057-42ea-89f3-3920155f2c94.png)

![image](https://user-images.githubusercontent.com/51500878/137948050-286242bf-0ce4-4493-b8cd-ff63a2069250.png)

Explanation:  

- Average weather_state in USA in November is (15) / 1 = 15 so weather type is Cold.  
- Average weather_state in Austraila in November is (-2 + 0 + 3) / 3 = 0.333 so weather type is Cold.  
- Average weather_state in Peru in November is (25) / 1 = 25 so the weather type is Hot.  
- Average weather_state in China in November is (16 + 18 + 21) / 3 = 18.333 so weather type is Warm.  
- Average weather_state in Morocco in November is (25 + 27 + 31) / 3 = 27.667 so weather type is Hot.  
- We know nothing about the average weather_state in Spain in November so we do not include it in the result table.  

**Solution**

```sql
select b.country_name, case when avgtmp <= 15 then 'Cold'
                            when avgtmp >= 25 then 'Hot'
                            else 'Warm' end as weather_type
from (select country_id, avg(weather_state) as avgtmp 
      from weather 
      where day between '2019-11-01' and '2019-11-30'
      group by country_id) a
left join countries b
on a.country_id = b.country_id
```

**1303. Find the Team Size**

![image](https://user-images.githubusercontent.com/51500878/137951595-7528a718-a9bc-459b-802a-9d08c0fc4844.png)

![image](https://user-images.githubusercontent.com/51500878/137951611-46bd53d9-5ace-49b0-aa6a-73190712de43.png)

**Solution**

```sql
select a.employee_id, b.total as team_size
from employee a
left join (select team_id, count(*) as total from employee group by team_id) b
on a.team_id = b.team_id
```

```sql
# a faster method
select e.employee_id, (select count(team_id) from Employee where e.team_id = team_id) as team_size
from Employee e
```

```sql
# a much faster method
select employee_id, count(employee_id) over (partition by team_id) as team_size
from employee 
```


**1308. Running Total for Different Genders (medium)**

![image](https://user-images.githubusercontent.com/51500878/137957553-bf074e1a-d40b-4b50-ae89-555b4bd16e29.png)

![image](https://user-images.githubusercontent.com/51500878/137957589-8b102592-1ec8-4b61-a015-86a191fad866.png)

**Solution**

```sql
select a.gender, a.day, sum(b.score_points) as total
from scores a, scores b
where a.day >= b.day and a.gender = b.gender
group by a.gender, a.day
order by a.gender, a.day
```

```sql
# faster version with window function
select gender, day, sum(score_points) over (partition by gender order by day) as total
from scores
```

**Note**

- After both questions above, I found the `window function` is really a useful and powerful tool. I absolutely need to get more used to it.
- One thing need to mentioned here is, after using `sum(score_points) over (partition by gender order by day)` the final output is order according to the `gender` and `day`. I don't need to call `order by` clause anymore.


**1321. Restaurant Growth （medium）**

![image](https://user-images.githubusercontent.com/51500878/137959972-04c956fb-ead8-48c1-8210-436e846be233.png)

![image](https://user-images.githubusercontent.com/51500878/137960003-3ac22b66-45f5-4f1e-b3db-fe57aec536c7.png)

**Solution**

```sql


```









