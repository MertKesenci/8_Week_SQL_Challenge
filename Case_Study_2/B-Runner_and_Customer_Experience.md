<span>•</span> **Question_1:** How many runners signed up for each 1 week period? (i.e. week starts 2021-01-01)
```sql
-- Solution_1

WITH date_interval AS(

  SELECT
    MIN(registration_date) AS start_date
  FROM
    runners)

SELECT
  FLOOR((registration_date - start_date) / 7) +1 AS week_number,
  COUNT(runner_id) AS runner_registered_count
FROM
  runners
CROSS JOIN
  date_interval
GROUP BY
  week_number
ORDER BY
  runner_registered_count DESC,
  week_number ASC;
```

![](https://github.com/MertKesenci/Images/blob/main/case_2_B_1.PNG?raw=true)

<span>•</span> **Question_2:**  What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pickup the order?

The critical issue here is that we need to aggregate orders first because some orders have multiple pizzas in the customer order table. I did that using the ``` DISTINCT ``` operator while joining the tables. If we don't do that, the average time value will be shown with a weighted average calculation (the code will consider all pizza orders separately), which will be misleading because there is no importance if the order has multiple pizzas for that specific demand.
```sql
-- Solution_2

WITH joined AS(
  SELECT 
    co.order_id, ro.runner_id, co.order_time, ro.pickup_time, 
    EXTRACT(EPOCH FROM (ro.pickup_time - co.order_time) / 60) AS time_difference_minutes
  FROM 
    runner_orders AS ro
  LEFT JOIN(
    SELECT
      DISTINCT(order_id), order_time
    FROM
      customer_orders) AS co
  ON
    co.order_id = ro.order_id
  WHERE
    pickup_time IS NOT NULL)

SELECT
  runner_id, ROUND(AVG(time_difference_minutes), 1) AS avg_pickup_minutes
FROM
  joined
GROUP BY
  runner_id
ORDER BY
  avg_pickup_minutes;
```

![](https://github.com/MertKesenci/Images/blob/main/case_2_B_2.PNG?raw=true)

<span>•</span> **Question_3:** Is there any relationship between the number of pizzas and how long the order takes to prepare?

There is a relationship as expected. But preparing 2 pizzas at the same time seems the most efficient time consumption per pizza.

```sql
-- Solution_3

WITH pizza_prep_time AS(
  SELECT
    co.order_id, COUNT(co.pizza_id) AS pizza_count, co.order_time, ro.pickup_time
  FROM
    customer_orders AS co
  LEFT JOIN
    runner_orders AS ro
  ON
    co.order_id = ro.order_id
  GROUP BY
    co.order_id, co.order_time, ro.pickup_time
  HAVING
    ro.pickup_time IS NOT NULL),

avg_pizza_prep_time AS (SELECT
    pizza_count,
    ROUND(AVG(EXTRACT(EPOCH FROM (pickup_time - order_time) / 60)), 1) AS avg_prep_time_minutes
  FROM
    pizza_prep_time
  GROUP BY
    pizza_count)

SELECT
  pizza_count, avg_prep_time_minutes, ROUND((avg_prep_time_minutes / pizza_count), 1) AS avg_prep_time_per_pizza_minutes
FROM
  avg_pizza_prep_time
ORDER BY
  pizza_count ASC;
```

![](https://github.com/MertKesenci/Images/blob/main/case_2_B_3.PNG?raw=true)

<span>•</span> **Question_4:** What was the average distance travelled for each customer?
```sql
-- Solution_4

SELECT
  customer_id, ROUND(AVG(distance)::numeric, 1) AS average_distance_km
FROM
  runner_orders AS ro
INNER JOIN
  customer_orders AS co
ON
  ro.order_id = co.order_id
WHERE
  distance IS NOT NULL
GROUP BY
  customer_id
ORDER BY
  average_distance_km ASC;
```

![](https://github.com/MertKesenci/Images/blob/main/case_2_B_4.PNG?raw=true)

<span>•</span> **Question_5:** What was the difference between the longest and shortest delivery times for all orders?
```sql
-- Solution_5

SELECT
  (MAX(duration) - MIN(duration)) AS time_difference
FROM
  runner_orders;
```

![](https://github.com/MertKesenci/Images/blob/main/case_2_B_5.PNG?raw=true)

<span>•</span> **Question_6:** What was the average speed for each runner for each delivery and do you notice any trend for these values?
```sql
-- Solution_6

SELECT
  runner_id,
  ROUND((distance / (duration::float / 60))::numeric, 2) AS speed_km_h
FROM
  runner_orders
WHERE
  duration IS NOT NULL
ORDER BY
  runner_id;
```

![](https://github.com/MertKesenci/Images/blob/main/case_2_B_6.PNG?raw=true)

<span>•</span> **Question_7:**  What is the successful delivery percentage for each runner?
```sql
-- Solution_7

WITH success AS(

  SELECT
    runner_id,
    COUNT(CASE WHEN distance IS NULL THEN 1 END) AS fail,
    COUNT(CASE WHEN distance IS NOT NULL THEN 1 END) AS successful
  FROM
    runner_orders
  GROUP BY
    runner_id)

SELECT
  runner_id,
  (CAST(successful AS float)/(CAST(successful+fail AS float)))*100 AS success_percentage
FROM
  success
ORDER BY
  success_percentage DESC;
```

![](https://github.com/MertKesenci/Images/blob/main/Case_2_B_7.PNG?raw=true)