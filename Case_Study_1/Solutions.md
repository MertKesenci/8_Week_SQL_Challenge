<span>•</span> **Question_1:** What is the total amount each customer spent at the restaurant?
```sql
-- Solution_1

SELECT 
  customer_id, SUM(price) AS Total_Spent
FROM 
  sales
LEFT JOIN 
  menu 
ON 
  sales.product_id = menu.product_id
GROUP BY
  customer_id;
```
![](https://github.com/MertKesenci/Images/blob/main/case_1_solution_1.PNG?raw=true)

<span>•</span> **Question_2:** How many days has each customer visited the restaurant?
```sql
-- Solution_2

SELECT 
  customer_id, COUNT(DISTINCT(order_date)) AS total_days_visited 
FROM 
  dannys_diner.sales
GROUP BY
  customer_id;
  ```

![](https://github.com/MertKesenci/Images/blob/main/case_1_solution_2.PNG?raw=true)

<span>•</span> **Question_3:** What was the first item from the menu purchased by each customer?
```sql
-- Solution_3

SELECT
  customer_id, product_name, order_date
FROM
  (SELECT 
    customer_id, order_date, product_name, RANK() OVER(PARTITION BY customer_id ORDER BY order_date ASC)  AS rank
  FROM
    sales
  LEFT JOIN
    menu
  ON
    sales.product_id = menu.product_id)
WHERE
  rank = 1;
```

![](https://github.com/MertKesenci/Images/blob/main/case_1_solution_3.PNG?raw=true)

<span>•</span> **Question_4:** What is the most purchased item on the menu and how many times was it purchased by all customers?
```sql
-- Solution_4

SELECT
  product_name, COUNT(product_name) AS count_product
FROM
  sales
LEFT JOIN
  menu
ON
  sales.product_id = menu.product_id
GROUP BY
  product_name
ORDER BY
  count_product DESC
LIMIT
  1;
```

![](https://github.com/MertKesenci/Images/blob/main/case_1_solution_4.PNG?raw=true)

<span>•</span> **Question_5:** Which item was the most popular for each customer?
```sql
-- Solution_5

WITH seperate_count AS(
  
  SELECT
    customer_id, product_name, COUNT(product_name) AS count_product, RANK() OVER(PARTITION BY customer_id ORDER BY COUNT(product_name) DESC) AS rank
  FROM
    sales
  LEFT JOIN
    menu
  ON
    sales.product_id = menu.product_id
  GROUP BY
    product_name, customer_id)

SELECT
  customer_id, product_name, count_product
FROM
  seperate_count
WHERE
  rank = 1;
```

![](https://github.com/MertKesenci/Images/blob/main/case_1_solution_5.PNG?raw=true)

<span>•</span> **Question_6:** Which item was purchased first by the customer after they became a member?
```sql
-- Solution_6

WITH first_item AS(

SELECT
  sales.customer_id, product_name, order_date, join_date, RANK() OVER(PARTITION BY sales.customer_id ORDER BY order_date) AS rank
FROM
  sales
LEFT JOIN
  menu
ON
  sales.product_id = menu.product_id
LEFT JOIN
  members
ON
  sales.customer_id = members.customer_id
WHERE
  (order_date > join_date))

SELECT 
  customer_id, product_name, order_date, join_date
FROM
  first_item
WHERE
  rank = 1
```

![](https://github.com/MertKesenci/Images/blob/main/case_1_solution_6.PNG?raw=true)

<span>•</span> **Question_7:** Which item was purchased just before the customer became a member?
```sql
-- Solution_7

WITH first_item AS(

SELECT
  sales.customer_id, product_name, order_date, join_date, RANK() OVER(PARTITION BY sales.customer_id ORDER BY order_date DESC) AS rank
FROM
  sales
LEFT JOIN
  menu
ON
  sales.product_id = menu.product_id
LEFT JOIN
  members
ON
  sales.customer_id = members.customer_id
WHERE
  (order_date < join_date))

SELECT 
  customer_id, product_name, order_date, join_date
FROM
  first_item
WHERE
  rank = 1
```

![](https://github.com/MertKesenci/Images/blob/main/case_1_solution_7.PNG?raw=true)

<span>•</span> **Question_8:** What is the total items and amount spent for each member before they became a member?
```sql
-- Solution_8

SELECT
  sales.customer_id, COUNT(sales.customer_id) AS count_item_before_join, SUM(price) AS total_sales_before_join
FROM
  sales
LEFT JOIN
  members
ON
  sales.customer_id = members.customer_id
LEFT JOIN
  menu
ON
  sales.product_id = menu.product_id
WHERE
  order_date < join_date
GROUP BY
  sales.customer_id;
```

![](https://github.com/MertKesenci/Images/blob/main/case_1_solution_8.PNG?raw=true)

<span>•</span> **Question_9:** If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?
```sql
-- Solution_9

SELECT
  customer_id,
  SUM(
    CASE
      WHEN  product_name = 'sushi' THEN price * 20
      ELSE price *10
    END) AS total_points
FROM
  sales
LEFT JOIN
  menu
ON
  sales.product_id = menu.product_id
GROUP BY
  customer_id
ORDER BY
  total_points DESC;
```

![](https://github.com/MertKesenci/Images/blob/main/case_1_solution_9.PNG?raw=true)

<span>•</span> **Question_10:** In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?<br>

**<span>•</span> Assumptions:**  Customers cannot earn points before their joining date. Additionally, starting from the second week after the joining date, sushi points will continue to be calculated with a 2x multiplier.
```sql
-- Solution_10

SELECT
  sales.customer_id,
  SUM(
    CASE
      WHEN product_name IN ('curry', 'ramen') AND (order_date > join_date + 6) THEN price * 10
      ELSE price * 20
    END) AS total_points
FROM
  sales
INNER JOIN
  members
ON
  sales.customer_id = members.customer_id
LEFT JOIN
  menu
ON
  sales.product_id = menu.product_id
WHERE
  (join_date <= order_date) AND (EXTRACT(MONTH FROM order_date) = 1)
GROUP BY
  sales.customer_id
ORDER BY
  total_points DESC;
```
![](https://github.com/MertKesenci/Images/blob/main/case_1_solution_10.PNG?raw=true)


**BONUSES**

<span>•</span>  **Bonus_1:** Join All The Things: Including customer_id, order_date, product_name, price and member
```sql
-- Solution_Bonus_1

SELECT
  sales.customer_id, order_date, product_name, price,
  (CASE
    WHEN join_date <= order_date THEN 'Y'
    ELSE 'N'
  END) AS member
FROM
  sales
LEFT JOIN
  menu
ON
  sales.product_id = menu.product_id
LEFT JOIN
  members
ON
  sales.customer_id =members.customer_id;
```

![](https://github.com/MertKesenci/Images/blob/main/case_1_solution_11.PNG?raw=true)

<span>•</span>  **Bonus_2:** Rank All The Things: Danny also requires further information about the ranking of customer products, but he purposely does not need the ranking for non-member purchases so he expects null ranking values for the records when customers are not yet part of the loyalty program.
```sql
-- Solution_Bonus_2

WITH all_join AS(

SELECT
  sales.customer_id, order_date, product_name, price,
  (CASE
    WHEN join_date <= order_date THEN 'Y'
    ELSE 'N'
  END) AS member           
FROM
  sales
LEFT JOIN
  menu
ON
  sales.product_id = menu.product_id
LEFT JOIN
  members
ON
  sales.customer_id =members.customer_id)

SELECT
  *,
  CASE
    WHEN member = 'N' THEN null
    ELSE RANK() OVER(PARTITION BY all_join.customer_id, member ORDER BY order_date)
  END AS ranking
FROM
  all_join;
```

![](https://github.com/MertKesenci/Images/blob/main/case_1_solution_12.PNG?raw=true)