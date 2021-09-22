![image](https://user-images.githubusercontent.com/51500878/134391053-511432dc-2259-495d-9908-3081a2a7ee9a.png)

```sql
select trade, count(*) as Kount
from input
group by trade
having count(*) > 1;
```

![image](https://user-images.githubusercontent.com/51500878/134391359-89538167-707b-4036-b20b-fb80935337ea.png)

```sql
select a.*, case 
            when b.ee is null then 0
            else (a.ee - b.ee)/b.ee 
            end as '%Difference'
from input a
left join input b
on a.date = b.date + 1;
```

Write a function f(n) that takes argument n and returns the n’th Fibonacci number.
You can assume that f(0)=0 and f(1)=1.

```python
def f(n):
    if n = 0:
      return 0
    if n = 1:
      return 1
    a = 0
    b = 1
    for i in range(1, n):
      c = a + b
      a = b
      b = c
    return c
```

```python
def f(n):
    if n = 0:
      return 0
    if n = 1:
      return 1
    
    return f(n-1) + f(n-2)
```

You have a flowerbed represented by an array of 0 (spot in flowerbed not occupied) and 1 (spot in flowerbed occupied).
Given n new flowers, can you plant them without having two flowers side by side?
 
 
Example 1:
    Input: flowerbed = [1,0,0,0,1], n = 1
    Output: true  
    Explanation [1,0,1,0,1] is a good planting configuration
 
Example 2:
    Input: flowerbed = [1,0,0,0,1], n = 2
    Output: false  
    Explanation wherever you plant the 2 flowers, two will be side by side
    For example, [1,1,0,1,1], [1,1,1,0,1], etc

```python
def f(fb, n):
  
```


Write a query that prints out prime numbers between 1 and 100 without using for loops or defining functions.

```sql
select distinct a.num
from input a, input b
group by a.num
having a.num > b.num and a.num % b.num != 0;
```




