**1393. Capital Gain/Loss (medium)**

![image](https://user-images.githubusercontent.com/51500878/139617921-29d54837-6e42-46b4-a819-f048afd87d2a.png)

![image](https://user-images.githubusercontent.com/51500878/139617941-213132cc-9c2f-4d75-9b76-0d642e65faa2.png)

![image](https://user-images.githubusercontent.com/51500878/139617962-0ba7bd6c-c4ae-45c1-8ed0-d213cf861011.png)

**Solution**

```sql
# my solution
select a.stock_name, sellprice-buyprice as capital_gain_loss
from (select stock_name, sum(price) as buyprice from stocks where operation='Buy' group by stock_name) a
left join (select stock_name, sum(price) as sellprice from stocks where operation='Sell' group by stock_name) b
on a.stock_name = b.stock_name
```

```sql
# some genius solution
SELECT stock_name, SUM(
    CASE
        WHEN operation = 'Buy' THEN -price
        ELSE price
    END
) AS capital_gain_loss
FROM Stocks
GROUP BY stock_name
```











