## **DANNY'S DINER**

![](https://private-user-images.githubusercontent.com/99183873/395322898-b0422988-178e-4989-9d07-496cc24c905c.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwMzY4NDIsIm5iZiI6MTczNDAzNjU0MiwicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjI4OTgtYjA0MjI5ODgtMTc4ZS00OTg5LTlkMDctNDk2Y2MyNGM5MDVjLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEyVDIwNDkwMlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWJhMTMxZmVhMzFjN2FjZTQxMjJmMmQzMGQ1NzY1NTkyZGNhZjI1NGU2MTgzYzU0ZTNlNzg5MmE2ZjIyNGY3N2UmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.BuxtaWeKttHb7AHwOm1W1oEHoxmndMP7xKxddmPKP04)

[case_link](https://8weeksqlchallenge.com/case-study-1/)

### **Entity Relationship Diagram**

![](https://github.com/MertKesenci/Images/blob/61f4f9e184378b01179fbf9f2046847436db00c1/case_1_entity_diagram.PNG)

### **Creating Schema and the Tables**


```sql
CREATE SCHEMA dannys_diner;

SET search_path = dannys_diner;

CREATE TABLE sales (
  "customer_id" VARCHAR(1),
  "order_date" DATE,
  "product_id" INTEGER
);

INSERT INTO sales
  ("customer_id", "order_date", "product_id")
VALUES
  ('A', '2021-01-01', '1'),
  ('A', '2021-01-01', '2'),
  ('A', '2021-01-07', '2'),
  ('A', '2021-01-10', '3'),
  ('A', '2021-01-11', '3'),
  ('A', '2021-01-11', '3'),
  ('B', '2021-01-01', '2'),
  ('B', '2021-01-02', '2'),
  ('B', '2021-01-04', '1'),
  ('B', '2021-01-11', '1'),
  ('B', '2021-01-16', '3'),
  ('B', '2021-02-01', '3'),
  ('C', '2021-01-01', '3'),
  ('C', '2021-01-01', '3'),
  ('C', '2021-01-07', '3');
 

CREATE TABLE menu (
  "product_id" INTEGER,
  "product_name" VARCHAR(5),
  "price" INTEGER
);

INSERT INTO menu
  ("product_id", "product_name", "price")
VALUES
  ('1', 'sushi', '10'),
  ('2', 'curry', '15'),
  ('3', 'ramen', '12');
  

CREATE TABLE members (
  "customer_id" VARCHAR(1),
  "join_date" DATE
);

INSERT INTO members
  ("customer_id", "join_date")
VALUES
  ('A', '2021-01-07'),
  ('B', '2021-01-09');
```
<span>•</span> You can also access this code above via the case_link.