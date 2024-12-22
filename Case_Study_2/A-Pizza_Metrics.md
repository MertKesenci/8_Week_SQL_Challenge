<span>•</span> **Question_1:** How many pizzas were ordered?
```sql
-- Solution_1

SELECT
    COUNT(*) AS order_count
FROM
    customer_orders;
```

![](https://github.com/MertKesenci/Images/blob/main/case_2_A_1.PNG?raw=true)

<span>•</span> **Question_2:**  How many unique customer orders were made?
```sql
-- Solution_2

SELECT 
    COUNT(DISTINCT(order_id)) AS unique_order_count
FROM 
    customer_orders;
```

![](https://github.com/MertKesenci/Images/blob/main/case_2_A_2.PNG?raw=true)

<span>•</span> **Question_3:**  How many successful orders were delivered by each runner?
```sql
-- Solution_3

SELECT
    runner_id, COUNT(order_id) AS successful_orders
FROM
    runner_orders
WHERE
    (pickup_time IS NOT NULL)
GROUP BY
  runner_id
ORDER BY
  successful_orders DESC;
```

![](https://github.com/MertKesenci/Images/blob/main/case_2_A_3.PNG?raw=true)

<span>•</span> **Question_4:** How many of each type of pizza was delivered?
```sql
-- Solution_4

SELECT
  pizza_name, COUNT(pizza_names.pizza_id) AS delivered_count
FROM
  runner_orders
LEFT JOIN
  customer_orders
ON
  runner_orders.order_id = customer_orders.order_id
LEFT JOIN
  pizza_names
ON
  customer_orders.pizza_id = pizza_names.pizza_id
WHERE
  runner_orders.pickup_time IS NOT NULL
GROUP BY
  pizza_name;
```

![](https://github.com/MertKesenci/Images/blob/main/case_2_A_4.PNG?raw=true)

<span>•</span> **Question_5:** How many Vegetarian and Meatlovers were ordered by each customer?
```sql
-- Solution_5

SELECT 
  customer_orders.customer_id, pizza_name, COUNT(*) AS count
FROM
  customer_orders
LEFT JOIN
  pizza_names
ON
  customer_orders.pizza_id = pizza_names.pizza_id
GROUP BY
  customer_id, pizza_name
ORDER BY
  customer_id ASC;
```

![](https://github.com/MertKesenci/Images/blob/main/case_2_A_5.PNG?raw=true)

<span>•</span> **Question_6:** What was the maximum number of pizzas delivered in a single order?
```sql
-- Solution_6

WITH count_pizza_per_order AS(

  SELECT
    customer_orders.order_id, COUNT(*) AS count_per_order, RANK() OVER(ORDER BY COUNT(*) DESC) AS rank
  FROM
    customer_orders
  LEFT JOIN
    runner_orders
  ON
    customer_orders.order_id = runner_orders.order_id
  WHERE
    cancellation IS NULL
  GROUP BY
    customer_orders.order_id)

SELECT
  order_id, count_per_order
FROM
  count_pizza_per_order
WHERE
  rank = 1;

--The maximum number of pizzas for one order is 3 and it only belongs to order 4
```

![](https://github.com/MertKesenci/Images/blob/main/case_2_A_6.PNG?raw=true)

<span>•</span> **Question_7:** For each customer, how many delivered pizzas had at least 1 change and how many had no changes?
```sql
-- Solution_7

SELECT
  customer_id,
  SUM(
    CASE 
      WHEN (exclusions IS NOT NULL) OR (extras IS NOT NULL) THEN 1
      ELSE 0
    END) AS count_changed,
   SUM(
    CASE 
      WHEN (exclusions IS NULL) AND (extras IS NULL) THEN 1
      ELSE 0
    END) AS count_not_changed
FROM
  customer_orders
LEFT JOIN
  runner_orders
ON
  customer_orders.order_id = runner_orders.order_id
WHERE
  cancellation IS NULL
GROUP BY
  customer_id;
```

![](https://github.com/MertKesenci/Images/blob/main/case_2_A_7.PNG?raw=true)

<span>•</span> **Question_8:** How many pizzas were delivered that had both exclusions and extras?
```sql
-- Solution_8

SELECT
  COUNT(*) AS count_with_exclusions_and_extras
FROM
  customer_orders
LEFT JOIN
  runner_orders
ON
  customer_orders.order_id = runner_orders.order_id
WHERE
  (exclusions IS NOT NULL) AND (extras IS NOT NULL) AND (cancellation IS NULL);
```

![](https://github.com/MertKesenci/Images/blob/main/case_2_A_8.PNG?raw=true)

<span>•</span> **Question_9:** What was the total volume of pizzas ordered for each hour of the day?
```sql
-- Solution_9

SELECT
  EXTRACT(HOUR FROM order_time) AS hour_of_the_day,
  COUNT(*) AS pizza_count
FROM
  customer_orders
GROUP BY
  hour_of_the_day
ORDER BY
  pizza_count DESC;
```

![](https://github.com/MertKesenci/Images/blob/main/case_2_A_9.PNG?raw=true)

<span>•</span> **Question_10:** What was the volume of orders for each day of the week?
```sql
-- Solution_10

SELECT
  TO_CHAR(order_time, 'Day') AS day,
  COUNT(*) AS pizza_count
FROM
  customer_orders
GROUP BY
  DAY
ORDER BY
  pizza_count DESC;
```

![](https://github.com/MertKesenci/Images/blob/main/case_2_A_10.PNG?raw=true)


