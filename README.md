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

First attempt:
```python
c1 = worker['department'] == 'Admin'
c2 = worker['joining_date'].dt.month >= 4

worker[(c1) & (c2)].shape[0]
```
<br/>
