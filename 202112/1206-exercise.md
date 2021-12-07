# Question 1 

```sql
select year(order_date) as Year, count(order_id) as Num_Order, sum(order_price) as Tot_Price
from order
where order_status = 'Good'
group by year(order_date);
```

# Question 2

```sql
select a.account_id,
       count(order_id) as Num_Order, 
       sum(order_price) as Tot_Price
from order a
left join merchant b
on a.merchant_id = b.merchant_id
left join account c
on a.account_id = c.account_id
left join dilivery d
on a.order_id = d.order_id
where 
  a.order_status = 'Good' and 
  b.merchant_category = 'Gaming' and 
  datediff(month, c.Date_Opened, now()) <= 6 and 
  d.country = 'Canada' and 
  d.province = 'Ontario'
group by a.account_id
```

# Question 3

1. Use box-plot to view the outliers

```python
import seaborn as sns

sns.boxplot(data_frame['risk_score'])
```

2. Use IQR to find and locate outliers

```python
import numpy as np

perc25 = data_frame['risk_score'].quantile(0.25)
perc75 = data_frame['risk_score'].quantile(0.75)
IQR = perc75 - perc25
LL = perc25 - 1.5*IQR
UL = perc75 + 1.5*IQR # this is what we want

# Extract the outlier risk_score
data_frame[data_frame['risk_score'] >= UL]
```

3. Trace back to order_id and check whether the order is performed by fault in fact.


# Question 4

1. Compute the missing/inaccurate rate within each merchants;
2. Explore the reason behind the missing/inaccurate data within each merchants generally, just to check whether there is any obvious pattern of the missing data;
3. (optional) Having known better about our data issue, there are a bunch of methods to fix it:
    - trace back to the data resources
    - delete
    - fill 
      - mean
      - moving average
      - median
      - linear regression
      - time series
      - ....
   
Refer to this [Link](https://towardsdatascience.com/all-about-missing-data-handling-b94b8b5d2184) for more details regarding outliers.

# Question 5

Several bullet points in my opinion:

Tech: 

1. Logistic regression (remember the result from the sigmoid function is the probability/likelihood)
2. Feature selection (refer to this [link](https://www.analyticsvidhya.com/blog/2020/10/feature-selection-techniques-in-machine-learning/))
3. Cross Validation
4. Gradient descent

General (beacause you are a leader):

1. Divide the projects and assign to team members based on their characteristic (some for data, some for model, some for final check)
2. Keep good communiactions with upsteam/downstream, as well as team members.


# Question 6













