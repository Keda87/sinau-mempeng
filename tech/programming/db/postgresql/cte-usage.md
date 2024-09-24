# CTE

Example:
```SQL
BEGIN;

WITH subquery AS (SELECT member_id, name FROM customers WHERE is_active IS TRUE),
     should_be_deleted AS (SELECT order_id FROM orders WHERE member_id IN (SELECT member_id FROM subquery))

-- example 1.
UPDATE orders SET status = 'CLOSED' WHERE id IN (SELECT order_id FROM should_be_deleted)

-- example 2.
UPDATE orders SET status = subquery.is_active FROM subquery WHERE orders.id = subquery.id;

COMMIT;
```
