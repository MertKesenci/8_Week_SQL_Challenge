## **Data Preprocessing for Pizza Runner**

<span>•</span> It seems we have problems with the data types and "null" values for some tables. We need to fix these issues first so that the queries give us accurate responses. A query like the one below can be written to see all data types for all columns in the schema.

```sql
SELECT 
  table_name, 
  column_name, 
  data_type
FROM 
  information_schema.columns
WHERE 
  table_schema = 'pizza_runner'
ORDER BY 
  table_name, ordinal_position;
```

![](https://github.com/MertKesenci/Images/blob/main/data_types_old.PNG?raw=true)

![](https://github.com/MertKesenci/Images/blob/main/customer_orders_old.PNG?raw=true)

![](https://github.com/MertKesenci/Images/blob/main/runner_orders_old.PNG?raw=true)

### **The Problems**

**1)** runner_orders:<br>
- pickup_time data type needs to be timestamp without time zone like order_time in the cutomer_orders table
- distance data type needs to be integer without **"km"** expression
- duration data type needs to be float(real) without **"minutes"** , **"minute"** or **"mins"** expressions
- **null** values for pickup_time, distance, duration and cancellation need to be organized

**2)** customer_orders:<br>
- **null** values for exclusions and extras need to be organized<br><br>

**Step-1: Dealing with the null values**
```sql
UPDATE customer_orders
SET 
    exclusions = CASE
        WHEN exclusions IS NULL OR TRIM(exclusions) = '' OR LOWER(exclusions) = 'null' THEN NULL
        ELSE exclusions
    END,
    extras = CASE
        WHEN extras IS NULL OR TRIM(extras) = '' OR LOWER(extras) = 'null' THEN NULL
        ELSE extras
    END;



UPDATE runner_orders
SET 
    pickup_time = CASE
        WHEN pickup_time IS NULL OR TRIM(pickup_time) = '' OR LOWER(pickup_time) = 'null' THEN NULL
        ELSE pickup_time
    END,
    distance = CASE
        WHEN distance IS NULL OR TRIM(distance) = '' OR LOWER(distance) = 'null' THEN NULL
        ELSE distance
    END,
    duration = CASE
        WHEN duration IS NULL OR TRIM(duration) = '' OR LOWER(duration) = 'null' THEN NULL
        ELSE duration
    END,
    cancellation = CASE
        WHEN cancellation IS NULL OR TRIM(cancellation) = '' OR LOWER(cancellation) = 'null' THEN NULL
        ELSE cancellation
    END;
```

<span>•</span> The null or empty texts will be shown as **"NULL"**<br><br>

**Step-2: Fixing the values and the data types**
```sql
ALTER TABLE runner_orders
    ALTER COLUMN pickup_time TYPE TIMESTAMP USING 
        CASE 
            WHEN pickup_time IS NULL THEN NULL
            ELSE pickup_time::TIMESTAMP
        END,
    ALTER COLUMN distance TYPE REAL USING
        CASE 
            WHEN distance IS NULL THEN NULL
            WHEN distance LIKE '%km%' THEN TRIM('km' FROM distance)::REAL
            ELSE distance::REAL
        END,
    ALTER COLUMN duration TYPE INTEGER USING
        CASE 
            WHEN duration IS NULL THEN NULL
            WHEN duration LIKE '%min%' THEN REGEXP_REPLACE(duration, '[^0-9]', '', 'g')::INTEGER
            ELSE duration::INTEGER
        END;
```

![](https://github.com/MertKesenci/Images/blob/main/data_types_new.PNG?raw=true)

![](https://github.com/MertKesenci/Images/blob/main/customer_orders_new.PNG?raw=true)

![](https://github.com/MertKesenci/Images/blob/main/runner_orders_new.PNG?raw=true)

<span>•</span> All issues are solved.