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

![](https://private-user-images.githubusercontent.com/99183873/395327212-c6640f50-6956-4d54-9bbe-e876d62d2468.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwMzc2NjgsIm5iZiI6MTczNDAzNzM2OCwicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjcyMTItYzY2NDBmNTAtNjk1Ni00ZDU0LTliYmUtZTg3NmQ2MmQyNDY4LlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEyVDIxMDI0OFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTM2ZjlhOTZkNDE4NmU4NWNlZGMyOTA5MDRkZWFiMTA3MzkyMWU5MzNhY2Q3YTY0MTUzMzZmZDA1YWQxYTU0YTkmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.j3FL19TFmlCsgD2AURAIyrQbBqDoJdGocdMb-NlaaV0)

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

![](https://private-user-images.githubusercontent.com/99183873/395327223-08dbe435-f677-46fe-9b33-af2da8865960.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwMzc2NjgsIm5iZiI6MTczNDAzNzM2OCwicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjcyMjMtMDhkYmU0MzUtZjY3Ny00NmZlLTliMzMtYWYyZGE4ODY1OTYwLlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEyVDIxMDI0OFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWM3ZjFlMDQzYjI3N2QwYjc1MDM3MDJhNzI5YWE5NWRjZTgzNjliMDVkMDY5OTJhMDg5ZGZlZGVkMGM5NTRhZGImWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.Y8ZKsaJdfDb3bqltXsp4IjEvIeLyDo_szW5yyjgZcec)

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

![](https://private-user-images.githubusercontent.com/99183873/395327239-1c4c8653-f3ee-4f9a-8c8e-959854a02a35.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwMzc2NjgsIm5iZiI6MTczNDAzNzM2OCwicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjcyMzktMWM0Yzg2NTMtZjNlZS00ZjlhLThjOGUtOTU5ODU0YTAyYTM1LlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEyVDIxMDI0OFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTZiNzE5YmJjZWRkMjM2ZThhNzc2M2EwYzc3YWYxYTg5ODA4YzQ1ZjA4MjFlZjEyOTdiNzZkYjI1N2YzZWZhZjkmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.LpXn24G5baHgDKmMI1SEMBK0Ax83rjJat0jLYWeVjWY)

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

![](https://private-user-images.githubusercontent.com/99183873/395327258-c4469aa7-1c54-46fd-98c8-319daae69cd5.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwMzc2NjgsIm5iZiI6MTczNDAzNzM2OCwicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjcyNTgtYzQ0NjlhYTctMWM1NC00NmZkLTk4YzgtMzE5ZGFhZTY5Y2Q1LlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEyVDIxMDI0OFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWYxM2MyZDJmYWFlZjg5YmU0NDU2YmNhNTAwODdmNGUyYzIzNTI2OTI0NWYzMGQzNGMwMmI4YTdiNzA5MjQyYzEmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.V8vDev6m3D9n7CLG5iPcwJch9WasHPgJBBQyiUq7PJY)

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

![](https://private-user-images.githubusercontent.com/99183873/395327273-0765a07f-3082-48f7-b93d-1a64390c3c42.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwMzc2NjgsIm5iZiI6MTczNDAzNzM2OCwicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjcyNzMtMDc2NWEwN2YtMzA4Mi00OGY3LWI5M2QtMWE2NDM5MGMzYzQyLlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEyVDIxMDI0OFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWQ2ODE0NDI0OWRmZjk1MTZiOTJjYjQwMzYyOGYyZjg5MWY2ZDc3MDEyZGNiYjQ2NTcwMjI2ZTdhODUzMGIzZDgmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.ORgfBBKr6HjrYhYA_-5Ug9-INFLKLgh0_j0kqTMfQRI)

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

![](https://private-user-images.githubusercontent.com/99183873/395327292-a4ad599e-28a8-42b8-a715-4d79ffbec2ac.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwMzc2NjgsIm5iZiI6MTczNDAzNzM2OCwicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjcyOTItYTRhZDU5OWUtMjhhOC00MmI4LWE3MTUtNGQ3OWZmYmVjMmFjLlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEyVDIxMDI0OFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTM2ZjA2NDE3NWQxMDJlNDRmZmE5YTE3MmU1MDhiZWIzMTdkOTNiMjViNzY2MTQ2NGQ3Mjg1ZDk1YmU4NzE5ZmYmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.3AFCoFvZvjijq6qaN6mJQiYQnmkRCHQ0SoWHpgy9-G8)

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

![](https://private-user-images.githubusercontent.com/99183873/395327314-2839eec6-9c9f-4ae1-87ff-7aadd37db023.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwMzc2NjgsIm5iZiI6MTczNDAzNzM2OCwicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjczMTQtMjgzOWVlYzYtOWM5Zi00YWUxLTg3ZmYtN2FhZGQzN2RiMDIzLlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEyVDIxMDI0OFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTZlZDRkZTg2N2JiNjk1ZmEzNDkxNzQ2OTUyOWEwZjkyZjZhYmE1Zjc4YzA5NzFmY2ZmNjYyMTM0YTc5ZTRhNDMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.-erp4P5GDTRN6PLer6cWv-JRasmCLXLkRyfde2uC8cI)

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

![](https://private-user-images.githubusercontent.com/99183873/395327333-4f289cc0-dee8-44e1-95e1-c7957dd4c88d.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwMzc2NjgsIm5iZiI6MTczNDAzNzM2OCwicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjczMzMtNGYyODljYzAtZGVlOC00NGUxLTk1ZTEtYzc5NTdkZDRjODhkLlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEyVDIxMDI0OFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTJiZTIwNmY2MjA5OTQ4NWViNjAwODg5ZTNjYjk5MDdlYTYzMWJlMDMyNzM4OWJhMTlhNTBhNTk5OGIyNjgwODkmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.z932vzJdjDQ2JGuyJpzyabKwkU_n6yjZy9eilbPKAVk)

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
![](https://private-user-images.githubusercontent.com/99183873/395327349-13d602db-f032-410f-866a-de9cf66b712a.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwMzc2NjgsIm5iZiI6MTczNDAzNzM2OCwicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjczNDktMTNkNjAyZGItZjAzMi00MTBmLTg2NmEtZGU5Y2Y2NmI3MTJhLlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEyVDIxMDI0OFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPThjMmE5ZWQ4MTk1NjZkNTVkNTc0MzFkYWFlYThkMDllNzhjOWQ3OTgyYWYxZDliZDg4MzVmZGVhMGVlYzBlYWEmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.cdAMyIvjpyqE7KBEaef-Q2XD0g0cU7d9EruG9C442Ts)


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

![](https://private-user-images.githubusercontent.com/99183873/395327382-81e94200-6a84-4714-871a-6f75f93ab54e.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwMzc2NjgsIm5iZiI6MTczNDAzNzM2OCwicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjczODItODFlOTQyMDAtNmE4NC00NzE0LTg3MWEtNmY3NWY5M2FiNTRlLlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEyVDIxMDI0OFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWFhOTQyY2MyMzA4YmMyMGIyMTA4NDkzMzliOTk5YWVmMWM3MjIyMTM0MWZmZTY0YWYwMzVlOWIyMmQyMmQyZWUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.N0CtIV_nmmVKsFbftfN0cpF1DGycNfbfg78hF2w5mao)

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

![](https://private-user-images.githubusercontent.com/99183873/395327402-390f3a41-59d9-422f-be61-8ca03119943e.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwMzc2NjgsIm5iZiI6MTczNDAzNzM2OCwicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjc0MDItMzkwZjNhNDEtNTlkOS00MjJmLWJlNjEtOGNhMDMxMTk5NDNlLlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEyVDIxMDI0OFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWUxMjY0YmNmODQwMDEwOWUwOGI5ZGY5MmJkOGNiMDQxYTY0NDRiY2I0N2E3YTljNDA5YTc4ZTIyZWMwYTA5YjUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.XU3RNkGMH5HB4P6jXH3GIGW0jI29wfp6haLZ_UsXi7o)