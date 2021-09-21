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


**550. Game Play Analysis IV (Medium)**

Similar to the previous question, same `Activity` table.

![image](https://user-images.githubusercontent.com/51500878/134183708-ebaa35ce-9c6b-4e3d-9d93-de83e90c5df5.png)

**Solution**

```sql

```
