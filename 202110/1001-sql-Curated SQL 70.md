**1141. User Activity for the Past 30 Days I**

![image](https://user-images.githubusercontent.com/51500878/135635353-b8f2af20-fb85-4728-b7a9-0a8c18edb665.png)

![image](https://user-images.githubusercontent.com/51500878/135635382-e9dc0603-b905-4808-a6c0-7ddd9446f53e.png)

**Solution**

```sql
select activity_date as day, count(distinct user_id) as active_users
from activity
where activity_date > '2019-06-27'
group by activity_date
```

**1148. Article Views I**

![image](https://user-images.githubusercontent.com/51500878/135637345-830cf0b1-521f-4943-8df0-cb6cf5d1add1.png)

![image](https://user-images.githubusercontent.com/51500878/135637366-92862275-ac2f-4ac0-9807-ddebdeb9e625.png)

**Solution**

```sql
select distinct author_id as id
from views
where author_id = viewer_id
order by author_id asc
```

**Note**

- In sql, we can't name the ascending/descending order in the `select` statement


**1149. Article Views II**

![image](https://user-images.githubusercontent.com/51500878/135638293-39c5ebbc-51f3-44f8-859a-b5ce5ed11d4b.png)

![image](https://user-images.githubusercontent.com/51500878/135638309-de6d8292-1e13-46aa-b860-80f61a6eb379.png)

**Solution**

```sql
select distinct viewer_id as id
from views
group by view_date, viewer_id
having count(distinct article_id) > 1
order by viewer_id 
```

**1164. Product Price at a Given Date**

![image](https://user-images.githubusercontent.com/51500878/135657593-bc120d7b-3b7f-4695-9615-b7e0b4721904.png)

![image](https://user-images.githubusercontent.com/51500878/135657615-cef1d447-410b-4152-a57b-8c9911bb8c5d.png)

**Solution**

```sql
select distinct a.product_id, ifnull(b.new_price, 10) as price
from products a
left join (select * from products where (product_id, change_date) in 
          (select product_id, max(change_date) from products where change_date <= '2019-08-16' group by product_id) ) b
on a.product_id = b.product_id 
;
```

**Note**

- `ifnull(expression, alt_value)`: The IFNULL() function returns a specified value if the expression is NULL. If the expression is NOT NULL, this function returns the expression.  
- From this problem, I realize that I should get used to the thriple nested structure. Actually most of time, the innermost sub-query is very simple, like in this problem we have `select product_id, max(change_date) from products where change_date <= '2019-08-16' group by product_id`

**1173. Immediate Food Delivery I**

![image](https://user-images.githubusercontent.com/51500878/135676858-bf550ac1-c6f1-4f0e-b518-2f01c8accc39.png)

![image](https://user-images.githubusercontent.com/51500878/135676871-cb1d4b1d-e8cf-4fd7-8d33-c5a08c22c01c.png)

**Solution**

```sql
select round(100 * sum(if(order_date = customer_pref_delivery_date, 1, 0))/count(order_date), 2) as immediate_percentage 
from delivery
```

**1193. Monthly Transactions I**

![image](https://user-images.githubusercontent.com/51500878/135678031-b1e564bd-a1f3-4bbb-ad1d-1e18651f3cb2.png)

![image](https://user-images.githubusercontent.com/51500878/135678053-3b7d5d80-2ea6-4cdb-9327-7f3c350aaf89.png)

**Solution**

```sql
select a.month, a.country, count(a.id) as trans_count, 
      sum(if(a.state='approved', 1, 0)) as approved_count, 
      sum(amount) as trans_total_amount, 
      sum(if(state='approved', amount, 0)) as approved_total_amount 
from (select *, substring(trans_date, 1, 7) as month from transactions) a
group by a.month, a.country
```

**Note**

- `substring(var, start, length)`: the third argument is not the end position! It's the length!

**1194. Tournament Winners (Hard)**

![image](https://user-images.githubusercontent.com/51500878/135680024-068f96dd-16c0-4722-9d90-fa10df217c4b.png)

![image](https://user-images.githubusercontent.com/51500878/135680063-a25ba3e0-eed4-457a-9fa0-536d7f808e62.png)

![image](https://user-images.githubusercontent.com/51500878/135680081-8f035ed6-9778-49c7-a077-e127566ac71d.png)

**Solution**

```sql
# My method
# WRONG
select a.group_id as 'GROUP_ID', min(a.winner) as 'PLAYER_ID'
from (
    select case when m.first_score > m.second_score then m.first_player
                when m.first_score < m.second_score then m.second_player
                when m.first_score = m.second_score and m.first_player < m.second_player then m.first_player
                else m.second_player
            end as winner, if(m.first_score > m.second_score, m.first_score, m.second_score) as winner_score,
            p.group_id
    from matches m
    left join players p 
    on m.first_player = p.player_id
) a
where (a.group_id, a.winner_score) in (
select tmp.group_id, max(winner_score)
from (
    select case when m.first_score > m.second_score then m.first_player
                when m.first_score < m.second_score then m.second_player
                when m.first_score = m.second_score and m.first_player < m.second_player then m.first_player
                else m.second_player
            end as winner, if(m.first_score > m.second_score, m.first_score, m.second_score) as winner_score,
            p.group_id
    from matches m
    left join players p 
    on m.first_player = p.player_id
) tmp
group by tmp.group_id)
group by a.group_id
```

```sql
# someone's method
select group_id as GROUP_ID, min(player_id) as PLAYER_ID
from Players,
    (select player, sum(score) as score from
        (select first_player as player, first_score as score from Matches
        union all
        select second_player, second_score from Matches) s
    group by player) PlayerScores
where Players.player_id = PlayerScores.player and (group_id, score) in
	(select group_id, max(score)
	from Players,
		(select player, sum(score) as score from
			(select first_player as player, first_score as score from Matches
			union all
			select second_player, second_score from Matches) s
		group by player) PlayerScores
	where Players.player_id = PlayerScores.player
	group by group_id)
group by group_id
```

**Note**

- My idea is to compute the maximum score for each match and then compute the maximum of maxs computed before.
- The second method is to 
- `union`: The UNION command combines the result set of two or more SELECT statements (only **distinct** values)
- `union all`: The UNION ALL command combines the result set of two or more SELECT statements (allows **duplicate** values).


**1204. Last Person to Fit in the Bus**

![image](https://user-images.githubusercontent.com/51500878/135687364-08837037-4b4d-4799-94a5-225db61fd839.png)

![image](https://user-images.githubusercontent.com/51500878/135687389-28f88b00-8e3f-48f3-8bba-74894cba3a13.png)

**Solution**

```sql
# join is slow!
select a.person_name
from queue a join queue b
on a.turn >= b.turn
group by a.person_id
having sum(b.weight) <= 1000
order by sum(b.weight) desc
limit 1;
```

```sql
select person_name from queue a
where (
    select sum(weight) 
    from queue b
    where a.turn >= b.turn
) <= 1000
order by a.turn desc
limit 1
```


**1205. Monthly Transactions II**

![image](https://user-images.githubusercontent.com/51500878/135699714-0092d40c-ac4f-45ec-ad88-83036b6eaf0b.png)

![image](https://user-images.githubusercontent.com/51500878/135699737-02aa1c02-0621-465e-8e5d-3c99118877a5.png)

**Solution**

```sql
SELECT month, country, 
        SUM(CASE WHEN state = "approved" THEN 1 ELSE 0 END) AS approved_count, 
        SUM(CASE WHEN state = "approved" THEN amount ELSE 0 END) AS approved_amount, 
        SUM(CASE WHEN state = "back" THEN 1 ELSE 0 END) AS chargeback_count, 
        SUM(CASE WHEN state = "back" THEN amount ELSE 0 END) AS chargeback_amount
FROM
(
    SELECT LEFT(chargebacks.trans_date, 7) AS month, country, "back" AS state, amount
    FROM chargebacks
    JOIN transactions ON chargebacks.trans_id = transactions.id
    UNION ALL
    SELECT LEFT(trans_date, 7) AS month, country, state, amount
    FROM transactions
    WHERE state = "approved"
) s
GROUP BY month, country
```

**Note**

- The idea of this problem is: we computed the `approved` and `chargeback` tables seperately and then take `union all` of both tables. In the end, we could use `case when` to extract those items we want and then combine them together to generate the final table.


**1212. Team Scores in Football Tournament**

![image](https://user-images.githubusercontent.com/51500878/135700828-48dc6c8b-9dd8-4e19-9360-c200d5fce60e.png)

![image](https://user-images.githubusercontent.com/51500878/135700839-1f272e18-8fc1-456c-aa47-e8ff5f0672d8.png)

![image](https://user-images.githubusercontent.com/51500878/135700842-3f3f98de-c30a-47ed-840e-3baa947588d4.png)

**Solution**

```sql
# my method
select t.team_id, t.team_name, ifnull(sum(a.score), 0) as num_points
from (select host_team as team, case when host_goals > guest_goals then 3
                                when host_goals < guest_goals then 0
                                else 1 end as score
from matches         
union all 
select guest_team as team, case when host_goals > guest_goals then 0
                                when host_goals < guest_goals then 3
                                else 1 end as score
from matches) a right join teams t
on a.team = t.team_id
group by t.team_id
order by num_points desc, t.team_id
```

**Note**

- Good! Ialmost solved this by using union all. There is one problem I didn't notice in the last row `order by num_points desc, t.team_id`, which is `order by sum(a.score) desc, t.team_id` previously. But this is wrong, because there are `null` values that cannot be compared. Thus I have to take it into consideration.
