# Tests

There are two ways of defining data tests in dbt:

# Singular Test
- A singular data test is testing in its simplest form: It’s a query that should return 0 rows to pass.

When to use:
- For very specific business logic checks that don’t fit a generic pattern.
- ✅ Best for complex, one-off validations.

Why:
- Let you express unique business rules directly in SQL.
- Flexible when generic tests can’t capture the condition.

Example of a singular test

-- Refunds have a negative amount, so the total amount should always be >= 0.
-- Therefore return records where total_amount < 0 to make the test fail.
select
    order_id,
    sum(amount) as total_amount
from {{ ref('fct_payments') }}
group by 1
having total_amount < 0

# Generic Test
- These are reusable test macros that you can apply in schema.yml files.
- Examples: unique, not_null, accepted_values, relationships.
- You can also write custom generic tests that accept arguments.

When to use:
- When the same logic applies to many models or columns.
- For constraints/expectations you want consistently applied (e.g., every primary key is unique, every foreign key matches).
- ✅ Best for standard data quality rules.

Why:
- DRY (don’t repeat yourself).
- Easier to maintain → update logic once, it applies everywhere.
- Visible in lineage + dbt docs.

Example of a generic test

{% test not_null(model, column_name) %}

    select *
    from {{ model }}
    where {{ column_name }} is null

{% endtest %}
