**1501. Countries You Can Safely Invest In (medium)**

![image](https://user-images.githubusercontent.com/51500878/140455232-8f34a253-a346-46e1-b43c-3b92c56a2d15.png)

![image](https://user-images.githubusercontent.com/51500878/140455249-6f2759b2-34e9-4c03-8f50-bb8b131d6bc8.png)

![image](https://user-images.githubusercontent.com/51500878/140455403-26d595a3-e3ad-4661-91d4-b0f1c56d7379.png)

![image](https://user-images.githubusercontent.com/51500878/140455432-ed9c949e-3965-4a6b-a7ce-327df02cf909.png)

**Solution**

```sql
select c.name as country
from
(select caller_id as caller, duration
from calls
union all
select callee_id as caller, duration
from calls ) a
left join person b
on a.caller = b.id
left join country c
on left(b.phone_number, 3) = c.country_code
group by c.name
having avg(duration) > (select avg(duration) from calls)
```

































