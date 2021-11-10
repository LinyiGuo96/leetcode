**1440. Evaluate Boolean Expression (medium)**

![image](https://user-images.githubusercontent.com/51500878/141020518-f4519e71-a49b-4d46-8773-ed10049d8496.png)

![image](https://user-images.githubusercontent.com/51500878/141020545-22a73ebd-9843-49c6-84d6-2db241a201be.png)

![image](https://user-images.githubusercontent.com/51500878/141020572-84a48b9e-fa3b-4621-888b-98f0d20d5378.png)

**Solution**

```sql
# my code, a little bit boring
select e.*, case when operator = '>' and v1.value > v2.value then 'true'
                 when operator = '=' and v1.value = v2.value then 'true'
                 when operator = '<' and v1.value < v2.value then 'true'
                 else 'false' end as value
from expressions e
left join variables v1 
on e.left_operand = v1.name
left join variables v2
on e.right_operand = v2.name
```



**1445. Apples & Oranges (medium)**

![image](https://user-images.githubusercontent.com/51500878/141020960-3df24965-8879-404f-bc7a-bbb3d9aa39e4.png)

![image](https://user-images.githubusercontent.com/51500878/141020985-ad4e77db-6676-4c08-b856-d930f720d927.png)


**Solution**

```sql
# My method
select sale_date, sum(case when fruit = 'apples' then sold_num
                        else -sold_num end) as diff 
from sales
group by sale_date
order by sale_date
```

```sql
# Method 2
select sale_date, sum(if(fruit = 'apples', sold_num, -sold_num) as diff 
from sales
group by sale_date
order by sale_date
```

**Note**

- If there is only two cases used in `case when`, we could use `if(condition, do, else-do)` to replace them to make code clean _(but seems to be slower)_.









