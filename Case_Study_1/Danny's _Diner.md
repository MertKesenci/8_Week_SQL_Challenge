## **DANNY'S DINER**

![](https://private-user-images.githubusercontent.com/99183873/395327174-0f42d933-ecc3-4f75-b3eb-5b460b0d7189.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwMzc2NjgsIm5iZiI6MTczNDAzNzM2OCwicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjcxNzQtMGY0MmQ5MzMtZWNjMy00Zjc1LWIzZWItNWI0NjBiMGQ3MTg5LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEyVDIxMDI0OFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWUyOTVjZDEzZjEwODdlYjZhZTdlNDhlYTAzZGNiNWQ1MzcwYzQxMWQxOTFiNWY4ZTgwZjQ0N2FiMWE2ZWExNDMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.szWv6Jx1sM2AGewydmIWwlHOHngWnMIQgK6mXuIWIrM)

[case_link](https://8weeksqlchallenge.com/case-study-1/)

### **Entity Relationship Diagram**

![](https://private-user-images.githubusercontent.com/99183873/395327186-268f08ff-66ed-4a29-a8a6-86e0105d1699.PNG?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MzQwMzc2NjgsIm5iZiI6MTczNDAzNzM2OCwicGF0aCI6Ii85OTE4Mzg3My8zOTUzMjcxODYtMjY4ZjA4ZmYtNjZlZC00YTI5LWE4YTYtODZlMDEwNWQxNjk5LlBORz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEyMTIlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMjEyVDIxMDI0OFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTI0ZjU1ZWZjMWYyOTg3Zjk5YzYzMmYxODY1YTYzNGZjN2FmNWEzMmJkZmFjMTkwZDliNDhjMzRhNTliMWU3N2QmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.xOgJSPR13F8NpXJb5ejhWO9d3QHONMiSaRVF7JTwukE)

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