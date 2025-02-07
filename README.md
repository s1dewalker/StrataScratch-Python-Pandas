# StrataScratch Analytical Questions | Python-Pandas

#### View StrataScratch Analytical Questions | Python-Pandas, here: [stratascratch.com](https://platform.stratascratch.com/coding?code_type=2&is_freemium=1&order_field=difficulty)
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
`.size()` counts all occurrences, including duplicates.
`.nunique()` counts only distinct (unique) values.
<br/>
