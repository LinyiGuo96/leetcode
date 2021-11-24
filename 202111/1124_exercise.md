**1**

![image](https://user-images.githubusercontent.com/51500878/143318486-bcde5e00-a34b-4cd0-aa10-67809555078a.png)

```sql
select a.*, sum(b.total) as runtot
from table1 a, table1 b
where a.channel = b.channel and a.month >= b.month
group by a.channel, a.month
order by a.channel, a.month
```

**2 ？**

![image](https://user-images.githubusercontent.com/51500878/143319025-466fff67-1712-456b-9dff-9d85c53c784a.png)

```sql
update t1
set t1.total = t1.total - t2.total 
from table1 t1, table1 t2
where t1.channel = 'Branch' and t2.channel = 'Retail' and t1.month = t2.month
```


**3**

![image](https://user-images.githubusercontent.com/51500878/143320133-6cb83a53-bce4-46f7-9134-9d79070876a1.png)

```sql
select a.*, ifnull(b.score, c.score) as score
from tabl1 a
left join lookup1 b
on a.clientno = b.clientno and a.license = b.license
left join lookup2 c
on a.clientno = c.clientno and a.lname = b.lname
```


**4**

![image](https://user-images.githubusercontent.com/51500878/143320605-65026859-d9f7-44bf-9aae-25a346edee82.png)


```sql
select a, b, c, d
from step99 
group by a, b, c, d
having count(*) > 1
```

**5**

![image](https://user-images.githubusercontent.com/51500878/143320888-75e36a44-fdc9-43bb-81fc-b19d22c22c53.png)

```sql
with cte as (
  select company, row_number() over (order by sales desc) as rank2010
  from table1
  where year = 2010
)

select a.*
from table1 a
left join cte b
on a.company = b.company
order by b.rank2010, a.year desc
```

```sql
select a.*
from table1 a
left join (select t.company, row_number() over (order by t.sales desc) as rank2010
          from table1 t
          where year = 2010) b
on a.company = b.company
order by b.rank2010, a.year desc
```


**6**

![image](https://user-images.githubusercontent.com/51500878/143321465-b0028f45-558b-4f69-baee-27ad4c439e51.png)

1. Check the keyboard switch and power light is on 
2. Check the wireless keyboard connection type, via bluetooth or signal receiver.
    a. Turn on the bluetooth of the computer, and make sure the device connected
    b. Plug in the signal receiver 


