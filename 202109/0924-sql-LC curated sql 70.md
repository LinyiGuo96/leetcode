**1045. Customers Who Bought All Products**

![image](https://user-images.githubusercontent.com/51500878/134754597-a871b882-af89-4f1f-b368-a27fbf68e832.png)

![image](https://user-images.githubusercontent.com/51500878/134754603-48593908-8786-4225-b284-3fdfd8195c4a.png)

**Solution**

```sql
select customer_id 
from customer
group by customer_id
having count(distinct product_key) = (select count(*) from product)
```

**1050. Actors and Directors Who Cooperated At Least Three Times**

![image](https://user-images.githubusercontent.com/51500878/134754726-ae514a5a-ec89-452b-a9d6-3488b5f258dc.png)

![image](https://user-images.githubusercontent.com/51500878/134754732-c65cf7bd-5e6d-4e80-824a-23da76ae8952.png)

**Solution**

```sql


```
