# Snapshots

- Snapshots are used to track slowly changing dimensions (SCDs) — they record how a row’s values change over time.
- They are not the same as incremental models, though under the hood they also use incremental strategies
- Good for dimensions like customers, products, employees — where attributes change (e.g., address, status, age)
- Snapshots preserve history
Example code:

{% snapshot customer_snapshot %}
  {{
    config(
      target_schema='snapshots',
      unique_key='customer_id',
      strategy='timestamp',
      updated_at='updated_at'
    )
  }}

  select * from {{ source('app', 'customers') }}
{% endsnapshot %}

This creates a history table — if a customer changes segment, a new row is written with a valid-from and valid-to timestamp.