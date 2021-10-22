**1322. Ads Performance**

![image](https://user-images.githubusercontent.com/51500878/138501190-c31a6854-c6b8-474d-81e8-16dd71e16726.png)

![image](https://user-images.githubusercontent.com/51500878/138501208-11844f2b-9851-4008-9147-1155261a8c63.png)

**Solution**

```sql
# mine
select ad_id, round(ifnull(100 * sum(if(action='Clicked', 1, 0)) / sum(if(action='Ignored', 0 , 1)), 0), 2) as ctr
from ads
group by ad_id
order by ctr desc, ad_id
```

```sql
# another method but slower
SELECT ad_id, IFNULL(ROUND(AVG(CASE WHEN action = 'Clicked' THEN 1
                         WHEN action = 'Viewed' THEN 0
                         ELSE NULL END)*100,2),0) AS ctr
FROM Ads
GROUP BY ad_id
ORDER BY ctr DESC, ad_id
```

**1327. List the Products Ordered in a Period**

![image](https://user-images.githubusercontent.com/51500878/138504258-77c48233-6496-456e-aea1-d9bf21880a6c.png)

![image](https://user-images.githubusercontent.com/51500878/138504298-bae83adc-4ac4-4aef-ae6c-2f3875ef6ef1.png)

![image](https://user-images.githubusercontent.com/51500878/138504319-31bc18e9-2fc2-461d-8690-321f1148c939.png)

**Solution**

```sql
select b.product_name, a.total as unit
from (select product_id, sum(unit) as total
                       from orders 
                       where order_date between '2020-02-01' and '2020-02-29' 
                       group by product_id
                       having sum(unit) >= 100
                      ) a
left join products b
on a.product_id = b.product_id
```



