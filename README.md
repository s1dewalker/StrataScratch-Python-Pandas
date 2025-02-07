# StrataScratch Analytical Questions | Python-Pandas

#### View StrataScratch Analytical Questions | Python-Pandas, here: [stratascratch.com](https://platform.stratascratch.com/coding?code_type=2&is_freemium=1&order_field=difficulty)

## [Most Lucrative Products](https://platform.stratascratch.com/coding/2119-most-lucrative-products?code_type=2)

First attempt:
```python
# online_orders.head()

#online_orders.dtypes

condition1 = online_orders['date'].between('2022-01-01','2022-06-30') # storing condition

online_orders['total'] = online_orders['units_sold']*online_orders['cost_in_dollars'] # storing transformed column

grouped_df = online_orders[condition1].groupby('product_id').sum().reset_index() # storing aggregated df

grouped_df[['product_id','total']].sort_values(by = 'total', ascending = False).head() # printing expected output
```
