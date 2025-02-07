# StrataScratch Analytical Questions | Python-Pandas

#### View StrataScratch Analytical Questions | Python-Pandas, here: [stratascratch.com](https://platform.stratascratch.com/coding?code_type=2&is_freemium=1&order_field=difficulty)
<br/>

Easy | Medium | Hard
<br/>

## [Most Lucrative Products](https://platform.stratascratch.com/coding/2119-most-lucrative-products?code_type=2)

First attempt:
```python
condition1 = online_orders['date'].between('2022-01-01','2022-06-30') # storing condition

online_orders['total'] = online_orders['units_sold'] * online_orders['cost_in_dollars'] # storing transformed column

grouped_df = online_orders[condition1].groupby('product_id').sum().reset_index() # storing aggregated df

grouped_df[['product_id','total']].sort_values(by = 'total', ascending = False).head() # printing expected output
```
<br/>

Key Refinements <br/>
1. Grouping with as_index=False: <br/>
Instead of using .reset_index() after groupby(), it's more efficient to specify `as_index=False` inside the groupby(). This avoids needing a second reset. <br/>

2. Use of .sum() with groupby():<br/>
Already aggregating correctly by using .sum(). Just **specify the column you need to aggregate**, i.e., `['total']`. <br/>

3. Variable top_products:<br/>
Changed the final result assignment to top_products for clarity. This makes it easier to reference later and understand. <br/>

```python
# Group by 'product_id', aggregate using sum, and reset index
grouped_df = online_orders[condition1].groupby('product_id', as_index=False)['total'].sum()

# Sort by 'total' in descending order and show top results
top_products = grouped_df[['product_id', 'total']].sort_values(by='total', ascending=False).head()

# Show the result
top_products
```
<br/>

## [April Admin Employees](https://platform.stratascratch.com/coding/9845-find-the-number-of-employees-working-in-the-admin-department?code_type=2)

```python
c1 = worker['department'] == 'Admin'
c2 = worker['joining_date'].dt.month >= 4

worker[(c1) & (c2)].shape[0]
```
<br/>

## [Workers by Department Since April](https://platform.stratascratch.com/coding/9847-find-the-number-of-workers-by-department?code_type=2)


```python
c1 = worker['joining_date'] >= '2014-04-01'

grouped_df = worker[(c1)].groupby('department', as_index = False).size()

grouped_df.sort_values(by = 'size', ascending = False)
```
<br/>

## [Highly Reviewed Hotels](https://platform.stratascratch.com/coding/9871-highly-reviewed-hotels?code_type=2)

```python
grouped_df = hotel_reviews.groupby(['hotel_name', 'total_number_of_reviews'], as_index=False).size()

result = grouped_df.sort_values('total_number_of_reviews', ascending=False).drop(columns='size')
```
Note: to handle unique entries in 'total_number_of_reviews' column, used groupby.size() on multiple columns
<br/>

## [Bikes Last Used](https://platform.stratascratch.com/coding/10176-bikes-last-used?code_type=2)

```python
grouped_df = dc_bikeshare_q1_2012.groupby('bike_number', as_index = False).agg(last_used = ('end_time','max'))

grouped_df.sort_values(by = 'last_used', ascending = False)
```
Notes: To get the latest 'end_time' use `.max()`

<br/>

## [Customer Details](https://platform.stratascratch.com/coding/9891-customer-details?code_type=2)

```python
df1 = pd.merge(customers, orders, how ='left', left_on = 'id', right_on = 'cust_id')

df1[['first_name','last_name', 'city', 'order_details']].sort_values(by = ['first_name','order_details'], ascending = [True, True])
```

<br/>

## [Number Of Bathrooms And Bedrooms](https://platform.stratascratch.com/coding/9622-number-of-bathrooms-and-bedrooms?code_type=2)

```python
airbnb_search_details.groupby(['city','property_type'], as_index = False).agg(
    n_bedrooms_avg = ('bedrooms','mean'),
    n_bathrooms_avg = ('bathrooms', 'mean')
    )
```

<br/>

## [Unique Users Per Client Per Month](https://platform.stratascratch.com/coding/2024-unique-users-per-client-per-month?code_type=2)
```python
fact_events['time_id'] = fact_events['time_id'].dt.month

fact_events.groupby(['client_id', 'time_id'], as_index = False).agg(size = ('user_id','nunique'))
```
Notes:  <br/>
`.size()` counts all occurrences, including duplicates. <br/>
`.nunique()` counts only distinct (unique) values.<br/>
<br/>

Example:<br/>
If client_1 has users user_1, user_2, user_1 in January, here's how .size() and .nunique() would behave:<br/>

`.size()` would return 3 (since there are three records).<br/>
`.nunique()` would return 2 (since there are only two unique users).<br/>
<br/>

## [Order Details](https://platform.stratascratch.com/coding/9913-order-details?code_type=2)

```python
df1 = pd.merge(customers, orders, how = 'right', left_on = 'id', right_on = 'cust_id')

c1 = (df1['first_name'] == 'Jill') | (df1['first_name'] == 'Eva')

sorted_df = df1[c1].sort_values(by = 'cust_id') # first sort

sorted_df[['first_name', 'order_date','order_details', 'total_order_cost']] # then get the required columns not involving the sort by column
```

<br/>


## [Find the most profitable company in the financial sector of the entire world along with its continent](https://platform.stratascratch.com/coding/9663-find-the-most-profitable-company-in-the-financial-sector-of-the-entire-world-along-with-its-continent?code_type=2)

```python
c1 = forbes_global_2010_2014['sector'] == 'Financials'

c2 = forbes_global_2010_2014['profits'] == forbes_global_2010_2014['profits'].max()

forbes_global_2010_2014[(c1) & (c2)][['company','continent']]
```

<br/>

## [Average Salaries](https://platform.stratascratch.com/coding/9917-average-salaries?code_type=2)

```python
avg_sal = employee.groupby('department', as_index = False).agg(avg_salary = ('salary', 'mean'))

joined_df = pd.merge(employee, avg_sal, how  = 'left', on = 'department')

joined_df[['department', 'first_name', 'salary', 'avg_salary']]
```

<br/>

## [Email Preference Missing](https://platform.stratascratch.com/coding/9924-find-libraries-who-havent-provided-the-email-address-in-2016-but-their-notice-preference-definition-is-set-to-email?code_type=2)

```python
c1 = (library_usage['circulation_active_year'] == 2016 ) & (library_usage['provided_email_address'] == False) & (library_usage['notice_preference_definition'] == 'email')

library_usage[c1][['home_library_code']].drop_duplicates() # to get unique values
```

<br/>

## [Salaries Differences](https://platform.stratascratch.com/coding/10308-salaries-differences?code_type=2)

```python
def sal_diff(x,y):
    df_joined = pd.merge(db_employee, db_dept, how = 'left', left_on = 'department_id', right_on = 'id')

    sal_max = df_joined.groupby('department', as_index = False).agg(salary_max = ('salary', 'max'))

    s1 = sal_max[sal_max['department'] == x]['salary_max'].values[0]
    s2 =  sal_max[sal_max['department'] == y]['salary_max'].values[0]
    
    return abs(s2-s1)

sal_diff('marketing', 'engineering')
```

<br/>

## [Number of violations](https://platform.stratascratch.com/coding/9728-inspections-that-resulted-in-violations?code_type=2)

```python
c = sf_restaurant_health_violations['business_name'] == 'Roxanne Cafe'

sf_restaurant_health_violations['year'] = sf_restaurant_health_violations['inspection_date'].dt.year

sf_restaurant_health_violations[c].groupby('year', as_index = False).agg(n_violations = ('violation_id', 'nunique'))
```

<br/>

## [Number of Shipments Per Month](https://platform.stratascratch.com/coding/2056-number-of-shipments-per-month?code_type=2)

```python
amazon_shipment['year_month'] = amazon_shipment['shipment_date'].dt.to_period('M')
amazon_shipment.groupby('year_month', as_index = False).size()
```
Notes:  Use `.dt.to_period('M')` → If you need a period format (for time-series analysis) | this will return "YYYY-MM"

<br/>

## [Find all posts which were reacted to with a heart](https://platform.stratascratch.com/coding/10087-find-all-posts-which-were-reacted-to-with-a-heart?code_type=2)

```python
h = facebook_reactions[facebook_reactions['reaction'] == 'heart']['post_id'] # to extract the common column only (to avoid duplicate columns)

pd.merge(h, facebook_posts, how = 'left', on = 'post_id').drop_duplicates()
```

<br/>
