Tasks
1. Create a table called Orders with columns: order_id, user_id, order_amount, and app_name (e.g., 'Zomato', 'Swiggy', 'Flipkart'). Insert at least 10 sample records with different users and apps. Write an SQL query using the OVER() function to display each order's amount along with the total order amount for all orders.

```sql
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    user_id INT,
    order_amount DECIMAL(10, 2),
    app_name VARCHAR(50)
);

INSERT INTO Orders (order_id, user_id, order_amount, app_name) VALUES
(1, 101, 450.00, 'Zomato'),
(2, 102, 320.50, 'Swiggy'),
(3, 103, 1299.00, 'Flipkart'),
(4, 101, 275.00, 'Swiggy'),
(5, 104, 899.99, 'Flipkart'),
(6, 105, 150.00, 'Zomato'),
(7, 102, 540.00, 'Zomato'),
(8, 106, 2100.00, 'Flipkart'),
(9, 103, 375.25, 'Swiggy'),
(10, 107, 625.00, 'Zomato');

SELECT
    order_id,
    user_id,
    order_amount,
    app_name,
    SUM(order_amount) OVER () AS total_order_amount
FROM Orders;
```

2.Using the Orders table, write an SQL query to show each user's order_id, order_amount, and the average order_amount for that user using the OVER(PARTITION BY user_id) clause.<br><br><em><strong>Hint:</strong> Use AVG(order_amount) OVER(PARTITION BY user_id) to get the per-user average.</em>

```sql
SELECT
    user_id,
    order_id,
    order_amount,
    AVG(order_amount) OVER (PARTITION BY user_id) AS user_avg_order_amount
FROM Orders;
```

3.Suppose you have a table called Playlist with columns: song_id, user_id, and duration_sec. Write an SQL query to display each song's duration, and the total duration of songs added by each user using SUM(duration_sec) OVER(PARTITION BY user_id).

```sql
SELECT
    song_id,
    user_id,
    duration_sec,
    SUM(duration_sec) OVER (PARTITION BY user_id) AS user_total_duration_sec
FROM Playlist;
```

4.Given a table named MovieRatings with columns: rating_id, user_id, movie_name, and rating (1-5), write an SQL query to show each rating, the average rating per movie, and the difference between the user's rating and the movie's average rating using window functions.<br><br><em><strong>Hint:</strong> Use AVG(rating) OVER(PARTITION BY movie_name) and subtract it from the user's rating.</em>

```sql
SELECT
    rating_id,
    user_id,
    movie_name,
    rating,
    AVG(rating) OVER (PARTITION BY movie_name) AS movie_avg_rating,
    rating - AVG(rating) OVER (PARTITION BY movie_name) AS difference_from_movie_avg
FROM MovieRatings;
```

