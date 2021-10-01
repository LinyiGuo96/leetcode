**1126. Active Businesses**

![image](https://user-images.githubusercontent.com/51500878/135509947-2e7a0463-d5e1-49b5-9a49-2ecd9c5b9a07.png)

![image](https://user-images.githubusercontent.com/51500878/135509979-445f1157-f5b0-4244-92be-a19295731fb9.png)

**Solution**

```sql
select a.business_id
from events a left join (select event_type, avg(occurences) as average from events group by event_type) b
on a.event_type = b.event_type
where a.occurences > b.average
group by a.business_id
having count(a.event_type) > 1
```

**1132. Reported Posts II (medium)**

![image](https://user-images.githubusercontent.com/51500878/135513424-cf3eefe6-66cf-4729-a3e6-85f9ad3b6cb1.png)

![image](https://user-images.githubusercontent.com/51500878/135513453-939ba081-3c0c-494d-8f11-cc2abed3ae6a.png)

![image](https://user-images.githubusercontent.com/51500878/135513474-791d273f-535d-4fd3-96cb-c5373047ec00.png)

**Solution**

```sql
select round(avg(perc), 2) as average_daily_percent
from (select (count(distinct r.post_id)/count(distinct a.post_id))*100 as perc
        from actions a
        left join removals r
        on a.post_id = r.post_id
        where a.extra = 'spam' and a.action = 'report'
        group by action_date) tmp
```

**Note**

- This is still to compute an _average proportion_. The outer structure is always something like:
```sql
select avg(props) as prop
from (

A derived dataset

)
;
```

So the important thing is to build the derived dataset. And the important thing is to compute the **props** that we need. In this problem, we take the advantage of the `left join` 














