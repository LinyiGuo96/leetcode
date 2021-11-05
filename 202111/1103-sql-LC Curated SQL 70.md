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
select a.product_id, b.product_name, a.report_year, a.total_amount
from (
    select product_id, '2018' as report_year, 
    average_daily_sales * (datediff(least(period_end, '2018-12-31'), greatest(period_start, '2018-01-01')) + 1) as total_amount
    from sales
    where year(period_start) = 2018
    
    union all
    
    select product_id, '2019' as report_year, 
    average_daily_sales * (datediff(least(period_end, '2019-12-31'), greatest(period_start, '2019-01-01')) + 1) as total_amount
    from sales
    where year(period_start) < 2020 and year(period_end)>2018
    
    union all
    
    select product_id, '2020' as report_year, 
    average_daily_sales * (datediff(least(period_end, '2020-12-31'), greatest(period_start, '2020-01-01')) + 1) as total_amount
    from sales
    where year(period_end) = 2020
) a left join product b
on a.product_id = b.product_id
order by a.product_id, a.report_year
```

**Note**

- `LEAST()`: returns the smallest value of the list of arguments. `GREATEST()`: returns the greatest value of the list of arguments.
- Here is a comparison between `min()` and `least()`, [link](https://database.guide/min-vs-least-in-mysql-whats-the-difference/).
- `datediff(interval, date_end, date_start)` returns `date_end - date_start`.
- The idea of this solution is very simple. The difficulty mainly lies in the 




