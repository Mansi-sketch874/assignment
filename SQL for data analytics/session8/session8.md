Tasks
1.Create two tables in your SQL database: Users (user_id, username, city) and Orders (order_id, user_id, product, amount). Insert at least 3 users and 5 orders, making sure some users have no orders.
2.Write an SQL query using INNER JOIN to list all usernames and their ordered products, showing only users who have placed at least one order.
3.Write an SQL query using LEFT JOIN to display all usernames along with their ordered products. For users who haven't placed any orders, show NULL for the product.
4.Write an SQL query using RIGHT JOIN to show all orders and the corresponding username for each order. If an order has a user_id that doesn't exist in the Users table, display NULL for the username.<br><br><em><strong>Hint:</strong> Try deleting one user and keeping their order to test this case.</em>
5.Suppose you want to analyze food delivery data like Zomato. Create a CustomerSegments table (segment_id, segment_name), and link it to Users with a foreign key. Write an SQL query to show each username, their segment name, and total order amount (use JOINs as needed)

1.
```sql
CREATE TABLE Users (
    user_id INTEGER PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    city VARCHAR(100)
);

CREATE TABLE Orders (
    order_id INTEGER PRIMARY KEY,
    user_id INTEGER NOT NULL,
    product VARCHAR(100) NOT NULL,
    amount DECIMAL(10, 2) NOT NULL
);

INSERT INTO Users (user_id, username, city) VALUES
    (1, 'Ava', 'New York'),
    (2, 'Noah', 'Chicago'),
    (3, 'Mia', 'Seattle');

INSERT INTO Orders (order_id, user_id, product, amount) VALUES
    (101, 1, 'Pizza', 18.50),
    (102, 1, 'Salad', 9.25),
    (103, 2, 'Burger', 14.00),
    (104, 2, 'Pasta', 16.75),
    (105, 1, 'Coffee', 4.50);

```

2. 
```
SELECT u.username,
       p.product_name
FROM users u
INNER JOIN orders o
    ON u.user_id = o.user_id
INNER JOIN products p
    ON o.product_id = p.product_id;
```
3. 
```
select u.username, p.product_name
from users as u
left join orders as o 
on o.user_id=u.user_id
left join product as p
on p.product_name=o.product_name
```

4. 
```
select o.*, u.username
from users as u
right join orders as o
on u.id=o.user_id
```

5. 
```
ALTER TABLE Users
ADD segment_id INT,
ADD CONSTRAINT fk_users_customer_segments
    FOREIGN KEY (segment_id) REFERENCES CustomerSegments(segment_id);

SELECT
    u.username,
    cs.segment_name,
    COALESCE(SUM(o.order_amount), 0) AS total_order_amount
FROM Users AS u
LEFT JOIN CustomerSegmentstable AS cs
    ON cs.segment_id = u.segment_id
LEFT JOIN Orders AS o
    ON o.user_id = u.user_id
GROUP BY
    u.user_id,
    u.username,
    cs.segment_name;

    