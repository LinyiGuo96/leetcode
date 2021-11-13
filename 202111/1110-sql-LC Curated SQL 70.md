**1468. Calculate Salaries (medium)**

![image](https://user-images.githubusercontent.com/51500878/141233361-cebf6739-0575-4ad1-b0ea-5ca23644f382.png)

![image](https://user-images.githubusercontent.com/51500878/141233380-c053b3a0-426a-40b2-b0ef-81f0d1fa5334.png)

![image](https://user-images.githubusercontent.com/51500878/141233388-c12e06cf-6068-47b9-ab51-94e776ec20d2.png)


**Solution**

```sql
select a.company_id, a.employee_id, a.employee_name, round(case when b.class = 1 then a.salary
                                                        when b.class = 2 then a.salary * 0.76
                                                        else a.salary * 0.51 end, 0) as salary
from salaries a 
left join (select company_id, case when max(salary) < 1000 then 1
                        when max(salary) > 10000 then 3
                        else 2 end as class
            from salaries
            group by company_id) b
on a.company_id = b.company_id
```


**1484. Group Sold Products By The Date**

![image](https://user-images.githubusercontent.com/51500878/141236021-877c5a68-31e4-472d-9940-c747ffd5116d.png)

![image](https://user-images.githubusercontent.com/51500878/141596888-894334ae-716b-4295-98e1-eeb70b6e5f0c.png)

**Solution**

```sql
select sell_date, count(distinct product) as num_sold, group_concat(distinct product order by product) as products
from activities
group by sell_date
```

**Note**

- `group_concat()`: an aggregate function that concatenates strings from a `group` into a single string with various options.
```sql
GROUP_CONCAT(
    DISTINCT expression
    ORDER BY expression
    SEPARATOR sep
);
```
The default value of `sep` is `,`. Notice it belongs to the `aggregate` function family, which is another big group of functions compared with `window` functions. The aggregate function is often used with `group by` statement, including `avg(), min(), max()`. Please refer to [aggregate](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html) and [window](https://dev.mysql.com/doc/refman/8.0/en/window-function-descriptions.html) funtions.













