**1495. Friendly Movies Streamed Last Month**

![image](https://user-images.githubusercontent.com/51500878/141598001-adf8b123-625e-4139-a8b4-05de2733bff3.png)

![image](https://user-images.githubusercontent.com/51500878/141598007-f50dd7d6-55cc-4617-bce6-0a3d56d3ffe2.png)

![image](https://user-images.githubusercontent.com/51500878/141598016-e29af4ca-57a5-49a5-837e-b4bd21254df8.png)

![image](https://user-images.githubusercontent.com/51500878/141598032-b262dbbf-6179-4419-b2b5-e10a164dfe26.png)


**Solution**

```sql
select distinct b.title
from (select * from tvprogram where year(program_date)=2020 and month(program_date)=6) a
left join (select * from content where kids_content='Y' and content_type = 'Movies') b
on a.content_id = b.content_id
where b.title is not null
```


**1501. Countries You Can Safely Invest In (medium)**

![image](https://user-images.githubusercontent.com/51500878/141598065-35d365d5-9e60-4e07-9c66-aa1d9f10e701.png)

![image](https://user-images.githubusercontent.com/51500878/141598075-59f6e45f-e9f0-4b09-ada9-c69835af475c.png)

![image](https://user-images.githubusercontent.com/51500878/141598091-c8836536-7b81-4ca1-bc9f-1e53be58865a.png)

![image](https://user-images.githubusercontent.com/51500878/141598105-b8218ff5-238c-47c2-a153-e60d424e78a0.png)

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


**1511. Customer Order Frequency**

![image](https://user-images.githubusercontent.com/51500878/141600885-250fe2b7-9ca6-42d6-9850-bae29771fc59.png)

![image](https://user-images.githubusercontent.com/51500878/141600906-bf8be0d4-b680-43bd-9454-99e7d29eaf1b.png)

![image](https://user-images.githubusercontent.com/51500878/141600912-54f57646-2b22-44e4-8e36-859f9a778d68.png)

![image](https://user-images.githubusercontent.com/51500878/141600918-45ed7488-e596-4f77-bf0e-6bb58c2349b0.png)


**Solution**

```sql
# my first method, too stupid
select c.customer_id, c.name
from customers c
where c.customer_id in 
    (select comb.customer_id
        from (select o.customer_id, o.month
            from (select *, month(order_date) as month from orders where year(order_date) = 2020 and month(order_date) in (6,7)) o
            left join product p
            on o.product_id = p.product_id
            group by o.customer_id, o.month
            having sum(o.quantity * p.price) >= 100 ) comb
     group by comb.customer_id 
     having count(*) = 2)
```

```sql
# A much straight and smart method
select customer_id, name
from customers 
join orders using(customer_id) 
join product using(product_id)
group by customer_id
having sum(if(left(order_date, 7) = '2020-06', quantity, 0) * price) >= 100 and 
        sum(if(left(order_date, 7) = '2020-07', quantity, 0) * price) >= 100
```

**Note**

- `using` clause: works like ` ... join ... on ... `, but by default, the column(s) after `using` should exist in both tables and have the same name. Therefore, we don't need to write something like `a.col1 = b.col1` anymore.


