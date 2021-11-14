**1517. Find Users With Valid E-Mails**

![image](https://user-images.githubusercontent.com/51500878/141665482-cb9c4d59-57c1-4146-ae50-3491f025deff.png)

![image](https://user-images.githubusercontent.com/51500878/141665487-515c9fa8-239b-4c46-8efb-552fdb406b32.png)

**Solution**

```sql
SELECT *
FROM Users
WHERE mail REGEXP '^[A-Za-z][A-Za-z0-9\_\.\-]*@leetcode[\.]com$'
```

```sql
# Method 2
select *
from users
where mail regexp '^[a-zA-Z][a-zA-Z0-9_.-]*@leetcode[\.]com$'
```


**Note**

- Regular expression: a powerful way of specifying a pattern for a complex search.
![image](https://user-images.githubusercontent.com/51500878/141697606-bfe6aba9-7316-4d3e-95c5-cf63b0c19f09.png)

Eg,

```sql
SELECT * FROM author 
WHERE aut_name REGEXP '^w'; 
```

- Look at the `'^[A-Za-z][A-Za-z0-9\_\.\-]*@leetcode[\.]com$'`: `^[A-Za-z]` means the string must begin with letters, `@leetcode[\.]com$` means the string must end with `@leetcode.com`. The backslash `\` is better used here. 



**1532. The Most Recent Three Orders (medium)**

![image](https://user-images.githubusercontent.com/51500878/141665495-ab2c9bea-d242-466c-9ddf-53399cc2659f.png)

![image](https://user-images.githubusercontent.com/51500878/141665503-fd8be5c6-4dfb-4635-81f4-72bd0755b76c.png)

![image](https://user-images.githubusercontent.com/51500878/141665509-282f0bd0-20f4-4303-8d1b-882c874f0d57.png)

![image](https://user-images.githubusercontent.com/51500878/141665513-453fc460-d422-433b-a11a-e17e0cdc2966.png)


**Solution**

```sql
# MS SQL
select customer_name, customer_id, order_id, order_date
from
(select  name customer_name, c.customer_id, order_id, order_date,
rank() over(partition by c.customer_id order by order_date desc) rank
from customers c
inner join
orders o
on c.customer_id = o.customer_id) temp
where rank <=3
order by customer_name, customer_id, order_date desc
```

```sql
select c.name as customer_name, o.customer_id, o.order_id, o.order_date
from
(select order_id, order_date, customer_id, rank() over (partition by customer_id order by order_date desc) ranking from orders ) o 
left join customers c
on o.customer_id = c.customer_id 
where o.ranking <= 3
order by c.name, o.customer_id, o.order_date desc
```



**Note**

- I thought the first method should also work for mysql, but it turned out I was wrong. For some reason, the `rank()` function here has some error in mysql environment.
- Okay... The syntax of `rank()` function should be `rank() over (partition by ... order by ...) new_name`. Please note, I added `as` before the `new_name` before and then it doesn't work! Please remember do NOT add `as` anymore when using window functions. 

