**1251. Average Selling Price**

![image](https://user-images.githubusercontent.com/51500878/136640783-96b8e369-0d9a-4b01-94c9-9e4c573f2248.png)

![image](https://user-images.githubusercontent.com/51500878/136640796-114f717b-7705-42a2-b8d3-6278a72a3bde.png)

![image](https://user-images.githubusercontent.com/51500878/136640804-781fed60-8156-47ba-82b7-7c472c3bed05.png)

![image](https://user-images.githubusercontent.com/51500878/136640811-649b1539-f846-49ea-8ca0-84421a25cbf9.png)


**Solution**

```sql
select p.product_id, round(sum(p.price * u.units)/sum(u.units), 2) as average_price 
from prices p
left join unitssold u
on p.product_id = u.product_id and u.purchase_date between p.start_date and p.end_date
group by p.product_id
```


**1225. Report Contiguous Dates (hard)**

![image](https://user-images.githubusercontent.com/51500878/136641119-0392f6cb-6c34-4264-8637-9ebd6273b2d6.png)

![image](https://user-images.githubusercontent.com/51500878/136641140-6f074213-0159-426a-937e-f1b5a456765f.png)

**Solution**

```sql

```
