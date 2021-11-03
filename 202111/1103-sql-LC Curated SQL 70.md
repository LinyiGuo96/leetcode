**1369. Get the Second Most Recent Activity (hard)**

![image](https://user-images.githubusercontent.com/51500878/140226048-bdbc4691-f759-4206-830e-d3a374b33f9a.png)

![image](https://user-images.githubusercontent.com/51500878/140226097-69aff27b-02e2-42bd-b1c3-e54aad84535e.png)

**Solution**

```sql
select username, activity, startdate, enddate
from (select *, 
      count(activity) over (partition by username) as cnt, 
      row_number() over (partition by username order by startdate desc) n 
      from useractivity ) a
where cnt<2 or n = 2  
```

**1384. Total Sales Amount by Year (hard)**

![image](https://user-images.githubusercontent.com/51500878/140231037-9314d872-2439-4c82-a90c-a129a3c5d5f0.png)

![image](https://user-images.githubusercontent.com/51500878/140231056-316e708e-bcf4-4cc1-a0ee-16b1ebc70bdd.png)

![image](https://user-images.githubusercontent.com/51500878/140231074-4706f4dc-a2af-49ee-9c4a-3572f96ebd17.png)

![image](https://user-images.githubusercontent.com/51500878/140231111-9a3d2f09-f078-4639-bc56-597745009bef.png)

**Solution**

```sql

```



