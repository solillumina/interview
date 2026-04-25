# SQL: Recursive tree

```sql
-- employees (
--   id,
--   name,
--   manager_id
-- )

-- Find all children
WITH RECURSIVE employee_tree AS (
  -- anchor query: row
  SELECT
    id,
    name,
    manager_id,
    0 AS level
  FROM employees

  UNION ALL

  -- recursive query: หา child ของ row ก่อนหน้า
  SELECT
    e.id,
    e.name,
    e.manager_id,
    et.level + 1 AS level
  FROM employees e
  JOIN employee_tree et
    ON e.manager_id = et.id -- Direction หา parent หรือหา child
)

SELECT *
FROM employee_tree
ORDER BY level, id;
```

# SQL: JSON_AGG

```sql
-- users(
--   id
--   name
-- )

-- orders(
--   id
--   user_id
--   total
-- )

SELECT
  u.id,
  u.name,
  COALESCE(
    json_agg( -- Build array
      json_build_object( -- Build object
        'id', o.id,
        'total', o.total
      )
    ) FILTER (WHERE o.id IS NOT NULL),
    '[]' -- make sure ได้ array data เสมอ
  ) AS orders
FROM users u
LEFT JOIN orders o
  ON o.user_id = u.id
GROUP BY u.id, u.name
ORDER BY u.id;
```
