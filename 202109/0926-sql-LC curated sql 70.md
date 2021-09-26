**1097. Game Play Analysis V (hard)**

![image](https://user-images.githubusercontent.com/51500878/134821903-07d2956d-af69-4118-aee0-94cf35cda548.png)

![image](https://user-images.githubusercontent.com/51500878/134821912-dec1f53c-d241-476b-b9eb-30ab9e5c7abc.png)


**Solution**

```sql
select a.firstlogin as install_dt, count(a.player_id) as installs, round(count(b.player_id)/count(a.player_id),2) as Day1_retention
from (select player_id, min(event_date) as firstlogin from activity group by player_id) a 
left join activity b
on a.player_id = b.player_id and a.firstlogin - b.event_date = -1
group by a.firstlogin
```

**Note**

- Congrats! I solved this hard SQL problem by myself.
- Since we need to compute the proportion, the first idea in my mind is to use the statement like ` ... from table1, table2 ...`. But then I found I could use `... from table1 left join table2 on ... ` to achieve this. (Well the truth is I didn't solve it with my first method.)
- Note, when using left join, there would be some `NULL` values in `b.player_id`. Using `count()` here would compute those `null` values.

**1098. Unpopular Books**

![image](https://user-images.githubusercontent.com/51500878/134824165-f3718b13-d51a-4580-ae0d-0111a434ada9.png)

![image](https://user-images.githubusercontent.com/51500878/134824192-8e3eb07e-c5d8-453a-97be-9ff0d219d639.png)

![image](https://user-images.githubusercontent.com/51500878/134824195-d07f2291-2862-4e73-817e-fb7efe2b5119.png)

**Solution**

```sql
```

**Note**
