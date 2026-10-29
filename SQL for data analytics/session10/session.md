tasks
1.Create two tables: AppOrders (for orders placed via a food delivery app like Zomato) and InStoreOrders (for direct restaurant orders), each with columns: order_id, customer_name, amount, and order_date. Insert at least 3 sample records into each table.

2.Write a SQL query using UNION to combine all unique customer names from both AppOrders and InStoreOrders tables into a single list.

3.Write a SQL query using UNION ALL to display every order (including duplicates if any) from both AppOrders and InStoreOrders, showing order_id, customer_name, amount, and order_date.

4.Demonstrate the difference between UNION and UNION ALL by adding a duplicate customer_name in both tables, then running both queries and noting the difference in the result count.<br><br><em><strong>Hint:</strong> UNION removes duplicates, UNION ALL does not.</em>
Topic Repository URL

**solution**

1. 

```sql
CREATE TABLE AppOrders (
    order_id INT PRIMARY KEY,
    customer_name VARCHAR(100),
    amount DECIMAL(10, 2),
    order_date DATE
);

CREATE TABLE InStoreOrders (
    order_id INT PRIMARY KEY,
    customer_name VARCHAR(100),
    amount DECIMAL(10, 2),
    order_date DATE
);

INSERT INTO AppOrders (order_id, customer_name, amount, order_date)
VALUES
    (1, 'Aarav Sharma', 450.00, '2026-10-25'),
    (2, 'Diya Patel', 725.50, '2026-10-26'),
    (3, 'Kabir Mehta', 310.00, '2026-10-27');

INSERT INTO InStoreOrders (order_id, customer_name, amount, order_date)
VALUES
    (1, 'Meera Iyer', 280.00, '2026-10-25'),
    (2, 'Arjun Rao', 560.00, '2026-10-26'),
    (3, 'Sara Khan', 395.75, '2026-10-27');
```

2.

```sql
SELECT customer_name
FROM AppOrders
UNION
SELECT customer_name
FROM InStoreOrders;
```
3.

```sql
SELECT order_id, customer_name, amount, order_date
FROM AppOrders
UNION ALL
SELECT order_id, customer_name, amount, order_date
FROM InStoreOrders;
```

4.

Add the same customer name to both tables, then compare the results:

```sql
INSERT INTO AppOrders (order_id, customer_name, amount, order_date)
VALUES (4, 'Riya Shah', 520.00, '2026-10-28');

INSERT INTO InStoreOrders (order_id, customer_name, amount, order_date)
VALUES (4, 'Riya Shah', 430.00, '2026-10-28');

-- Returns each customer name once.
SELECT customer_name
FROM AppOrders
UNION
SELECT customer_name
FROM InStoreOrders;

-- Returns every customer-name row, including repeated names.
SELECT customer_name
FROM AppOrders
UNION ALL
SELECT customer_name
FROM InStoreOrders;
```

With these sample records, `UNION` returns **7 rows** because `Riya Shah` appears once in its result. `UNION ALL` returns **8 rows** because it preserves both occurrences.