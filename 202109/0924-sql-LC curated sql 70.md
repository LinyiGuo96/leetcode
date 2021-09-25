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
select actor_id, director_id
from actordirector
group by actor_id, director_id
having count(timestamp) >= 3
```

**1068. Product Sales Analysis I**

![image](https://user-images.githubusercontent.com/51500878/134755027-7ab17cde-83d3-4733-a3ec-da5effa6044c.png)

![image](https://user-images.githubusercontent.com/51500878/134755041-7d492e51-779c-4df1-8675-5f1aa348f187.png)

![image](https://user-images.githubusercontent.com/51500878/134755045-87a88235-2118-4ad0-987e-b8dab3e65599.png)

**Solution**

```sql
select p.product_name, s.year, s.price
from sales s
left join product p
on s.product_id = p.product_id;
```


