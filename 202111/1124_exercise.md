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

```
