**1393. Capital Gain/Loss**

![image](https://user-images.githubusercontent.com/51500878/140677591-f88a5cd5-5fb2-41f4-a9e5-b773b2f8cc33.png)

![image](https://user-images.githubusercontent.com/51500878/140677609-618f645c-7507-46ed-a484-c77241c16a65.png)

![image](https://user-images.githubusercontent.com/51500878/140677756-8ce2428e-076a-401e-8127-b50dabe02842.png)

**Solution**

```sql
select stock_name, sum( case when operation = 'Buy' then -price
                             else price
                             end ) as capital_gain_loss
from stocks
group by stock_name
```

**Note**

- Learn how and when to add function out of the `case when`.


**1407. Top Travellers**

![image](https://user-images.githubusercontent.com/51500878/140679000-bc741796-861b-4566-abe5-9275bc8edea8.png)

![image](https://user-images.githubusercontent.com/51500878/140679005-1a200912-bd09-41cf-bf10-1645e27b3a70.png)

![image](https://user-images.githubusercontent.com/51500878/140679014-15a7a9d6-fac2-4ea3-9974-23559db35cb1.png)

![image](https://user-images.githubusercontent.com/51500878/140679034-e49e454b-5407-40f4-83d2-def3f3e7e170.png)

![image](https://user-images.githubusercontent.com/51500878/140679050-93759a05-1447-4b26-a852-5a7e4b4db485.png)

**Solution**

```sql
select a.name, ifnull(b.tot, 0) as travelled_distance
from users a
left join (select user_id, sum(distance) as tot from rides group by user_id) b
on a.id = b.user_id
order by tot desc, a.name
```



