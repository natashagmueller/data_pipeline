# Incremental models

- An incremental model is a dbt model with materialized='incremental'
- Instead of rebuilding the entire table every time, dbt only processes new or changed rows and merges them into the existing table.
- Good for large fact tables (orders, events, logs) where full refresh would be too expensive. 
- Incrementals optimise performance

Example in models/ (not in snapshots/):

{{ config(materialized='incremental', unique_key='order_id') }}

select * from {{ source('tpch', 'orders') }}
{% if is_incremental() %}
  where updated_at > (select max(updated_at) from {{ this }})
{% endif %}
