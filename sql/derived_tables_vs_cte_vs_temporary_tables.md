# Derived tables vs. CTEs vs. temporary tables

For the following schema:

```sql
DROP TABLE IF EXISTS orders;

CREATE TABLE IF NOT EXISTS orders (
  id INT PRIMARY KEY, 
  customer_id INT, 
  amount INT
);

INSERT INTO orders (id, customer_id, amount)
VALUES
    (1, 1, 500),
    (2, 1, 500),
    (3, 1, 500),
    (4, 2, 800),
    (5, 3, 200),
    (6, 3, 600);
```

How would you return all of the customers whose orders amount to more than 1000?

There are at least three ways to solve this problem. 

## Using a derived table 

```sql
-- using a derived `total_spent_by_customer_id` table
SELECT *
FROM (
	SELECT customer_id, SUM(amount) AS total_spent
  	FROM orders
  	GROUP BY customer_id
) AS total_spent_by_customer_id
WHERE total_spent_by_customer_id.total_spent > 1000;
```

## Using a CTE

```sql
-- using a `total_spent_by_customer_id` CTE
WITH total_spent_by_customer_id AS (
	SELECT customer_id, SUM(amount) AS total_spent
	FROM orders
	GROUP BY customer_id
)
SELECT *
FROM total_spent_by_customer_id
WHERE total_spent_by_customer_id.total_spent > 1000;
```

## Using a temporary table

```sql
CREATE TEMPORARY TABLE IF NOT EXISTS total_spent_by_customer_id AS (
	SELECT customer_id, SUM(amount) AS total_spent
	FROM orders
	GROUP BY customer_id
);

SELECT *
FROM total_spent_by_customer_id
WHERE total_spent_by_customer_id.total_spent > 1000;
```

See [thread](https://www.perplexity.ai/search/Whats-a-derived-2HBaiJyLRuOOmjfbBtd.0w?s=c) on perplexity.ai.
