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
![](https://private-user-images.githubusercontent.com/99183873/395327199-7983bd9a-a65f-408d-b68f-43a43fc635f5.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwMzgwNzgsIm5iZiI6MTczNDAzNzc3OCwicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjcxOTktNzk4M2JkOWEtYTY1Zi00MDhkLWI2OGYtNDNhNDNmYzYzNWY1LlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEyVDIxMDkzOFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWU2ZmE1NDVkMjI5ZjA4NzVjNzQ0ZDFmYjRjZjU3YmJkYTA3YmQ2MWViYTUwNjU1ZDhjMzQxZjJiNjMyZTk5OGMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.2s0w-NhX8WDfYzfy_FNRZtvzeIrCwvSJ6MuJP7uyA6k)

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

![](https://private-user-images.githubusercontent.com/99183873/395327212-c6640f50-6956-4d54-9bbe-e876d62d2468.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwOTQyNTcsIm5iZiI6MTczNDA5Mzk1NywicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjcyMTItYzY2NDBmNTAtNjk1Ni00ZDU0LTliYmUtZTg3NmQ2MmQyNDY4LlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTMlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEzVDEyNDU1N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTdjZDY5M2Q2OTlkYTVjMTJmMGE1MzlmMzBmMzU0MTUwMmY2ZjcyNmE1NWUxNzc4NmI3NjIzOTE0MjVmZWJjOWYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.YFLnVmLKTix_-BWK9sEmjSef2_3KOHu6AJ6PAtYp-UM)

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

![](https://private-user-images.githubusercontent.com/99183873/395327223-08dbe435-f677-46fe-9b33-af2da8865960.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwOTQyNTcsIm5iZiI6MTczNDA5Mzk1NywicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjcyMjMtMDhkYmU0MzUtZjY3Ny00NmZlLTliMzMtYWYyZGE4ODY1OTYwLlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTMlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEzVDEyNDU1N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWIyYWUzMzRjNTk1Y2FkZWRkYTU3OGQ3OTI3YmQxMjZiZDcwMDE5ODAwZTc5NmFhZDA2NTA4OGI4M2EwMGI0MzEmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.JP1sHwqW1tDk5Yr6cW6AVn1iAU9TcTQwiUfbU3aK6XI)

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

![](https://private-user-images.githubusercontent.com/99183873/395327239-1c4c8653-f3ee-4f9a-8c8e-959854a02a35.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwOTQyNTcsIm5iZiI6MTczNDA5Mzk1NywicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjcyMzktMWM0Yzg2NTMtZjNlZS00ZjlhLThjOGUtOTU5ODU0YTAyYTM1LlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTMlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEzVDEyNDU1N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWM3YmFiOGY0MDliMjAwNjY0N2I5MjE0N2FlYjU4ZTQxZjczNDQ3MTEzYTQzYjYzN2MyNWJmMjY5YjVmNjMxMjkmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.KAMu1I-PMYRw9ZOBGpSSSMddbcPtIpxbYSZcsv1ze5M)

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

![](https://private-user-images.githubusercontent.com/99183873/395327258-c4469aa7-1c54-46fd-98c8-319daae69cd5.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwOTQyNTcsIm5iZiI6MTczNDA5Mzk1NywicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjcyNTgtYzQ0NjlhYTctMWM1NC00NmZkLTk4YzgtMzE5ZGFhZTY5Y2Q1LlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTMlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEzVDEyNDU1N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWY2ZWQ0YTczMGFkMzJmM2E4MzY3ZTU3MjI0NDJmMzVhNTdlY2NmN2UxNmQzYTMwYjEwMDIxZmEwNWUzODM2NzgmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.Ri2tZM2vwjkXVDndaKJMGuJlKnAb9BIpLdlHgjoBGBc)

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

![](https://private-user-images.githubusercontent.com/99183873/395327273-0765a07f-3082-48f7-b93d-1a64390c3c42.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwOTQyNTcsIm5iZiI6MTczNDA5Mzk1NywicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjcyNzMtMDc2NWEwN2YtMzA4Mi00OGY3LWI5M2QtMWE2NDM5MGMzYzQyLlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTMlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEzVDEyNDU1N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWY5ZjE0ZTRmY2U1NjE2MmYwZDMyZmFlYjhjZTQwODQzZjAwYWYyNzkzYTc4Yzg2ZGU4OWZhZjkwM2JkYjJkYzkmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.PQY1dA25t0Va3voMfmEJPO14ZcdXQrs_ZL99n0uW_zc)

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

![](https://private-user-images.githubusercontent.com/99183873/395327292-a4ad599e-28a8-42b8-a715-4d79ffbec2ac.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwOTQyNTcsIm5iZiI6MTczNDA5Mzk1NywicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjcyOTItYTRhZDU5OWUtMjhhOC00MmI4LWE3MTUtNGQ3OWZmYmVjMmFjLlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTMlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEzVDEyNDU1N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWYwOTRkMTk1YTg3N2M0OGY4MjFlODEzYTVhYTgxOWQ4MzJiMDFkNTI3MWVlMzFmNDE0Y2ZiZDNmNmMwMjI4ODMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.8adwXXKPsR422gBT5xu-auvUe6Srb5d_9FC-fwRUS9A)

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

![](https://private-user-images.githubusercontent.com/99183873/395327314-2839eec6-9c9f-4ae1-87ff-7aadd37db023.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwOTQyNTcsIm5iZiI6MTczNDA5Mzk1NywicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjczMTQtMjgzOWVlYzYtOWM5Zi00YWUxLTg3ZmYtN2FhZGQzN2RiMDIzLlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTMlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEzVDEyNDU1N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTYxYmI2Njg1ZTQ5MWQxMTc4YmNmZDY3OTVlYTJjYzMyZWY5ODhlM2E1MjI4ZWEwYjkyY2ZkMWVlMzk5ZDk2MWYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.meg37DvaK53pj205p9hOpl0ZtJ9BFIsLas4MyGpJc8o)

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

![](https://private-user-images.githubusercontent.com/99183873/395327333-4f289cc0-dee8-44e1-95e1-c7957dd4c88d.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwOTQyNTcsIm5iZiI6MTczNDA5Mzk1NywicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjczMzMtNGYyODljYzAtZGVlOC00NGUxLTk1ZTEtYzc5NTdkZDRjODhkLlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTMlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEzVDEyNDU1N1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTkzNjA5ZmM2Zjc5NzcwZTNjNDIxZWRlNjJmZmJlYzhjYzUwMTJhY2EyZTdiOGMyNmJlYzkzODg2MGMyZGUzNWEmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.rK28q8zvRln96QCdWQi_qcO3_ACzOsYM2pvshu2_1Io)

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
![](https://private-user-images.githubusercontent.com/99183873/395327349-13d602db-f032-410f-866a-de9cf66b712a.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwMzg0MDcsIm5iZiI6MTczNDAzODEwNywicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjczNDktMTNkNjAyZGItZjAzMi00MTBmLTg2NmEtZGU5Y2Y2NmI3MTJhLlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEyVDIxMTUwN1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTkwMGVmNDQ0NDdkMDI1MzUyNmM5ZjZmYWQxNTM1M2QyMmJjYmNjOGUwZjQwMzFjNDkwOThiMzJhYjE3MjgwNjcmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.qEtgOD6LxJtMVHNb7PjWVea7kfPsPK5c_2visl5hRx4)


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

![](https://private-user-images.githubusercontent.com/99183873/395327382-81e94200-6a84-4714-871a-6f75f93ab54e.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwMzg0MDcsIm5iZiI6MTczNDAzODEwNywicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjczODItODFlOTQyMDAtNmE4NC00NzE0LTg3MWEtNmY3NWY5M2FiNTRlLlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEyVDIxMTUwN1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWZlNTgxNTkyNWE5NjM0NzEyMDMyNDVjNmIyNDI4NGY0Nzk3YzA2ZmU2NzBjZTJiNGVkOTBlMGM4YzI5YmVhMTgmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.kl1z3Bmw6YZhPNCj8q51aYO06FqWmmoqxPZTIdjddc8)

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

![](https://private-user-images.githubusercontent.com/99183873/395327402-390f3a41-59d9-422f-be61-8ca03119943e.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwMzg0MDcsIm5iZiI6MTczNDAzODEwNywicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjc0MDItMzkwZjNhNDEtNTlkOS00MjJmLWJlNjEtOGNhMDMxMTk5NDNlLlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEyVDIxMTUwN1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWNlM2VmYzk1OWM3ZTUzZjA0NzkwMDYyNjg1Zjk3YTZkZWYzMGI1MmM2MmNhYTYyNDcyMjBmNTY1NDUyMWMyZTUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.aXjYeQJ5J3sueTTVz5f17h1gxXnF3pVbzljrWhH4ojs)