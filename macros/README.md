# Macros

- A macro is a piece of reusable code written in Jinja (templating language) inside dbt. Work like functions in SQL.
- Instead of repeating the same SQL everywhere, you can write it once as a macro and call it in multiple models.
- Why use Macros?

Reusability → don’t repeat the same calculation in 5 different models.
Consistency → one definition of business logic (e.g., revenue, discount calculation).
Maintainability → change logic in one place, it updates everywhere.
Abstraction → handle repetitive SQL patterns (like surrogate keys, SCDs - slow changing dimensions).