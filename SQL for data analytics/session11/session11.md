Tasks
1.Write a SQL query to display the names and ratings of restaurants (from a table named Restaurants) where the rating is higher than the average rating of all restaurants in the table.<br><br><em><strong>Hint:</strong> Use a subquery in the WHERE clause to calculate the average rating.</em>
2.In a Flipkart-style Products table (columns: product_id, name, price, category), write a SQL query to list each product name along with the average price of its category as an additional column using a scalar subquery in the SELECT statement.
3.Given a Playlists table (playlist_id, user_id, playlist_name) and a Users table (user_id, username), write a SQL query using a subquery in the FROM clause to show each username and the number of playlists they have created, displaying only users who have created more playlists than the average number of playlists per user.<br><br><em><strong>Hint:</strong> Use a derived table (subquery in FROM) to count playlists per user, then filter with a subquery in WHERE.</em>
4.Suppose you have an Orders table (order_id, user_id, total_amount) for a food delivery app. Write a query to find all user_ids who have placed at least one order with a total_amount greater than the average order amount, using a subquery in the WHERE clause.

**solution**

1. 
```sql
SELECT name, rating
FROM Restaurants
WHERE rating > (
    SELECT AVG(rating)
    FROM Restaurants
);
```
2. 
```sql
SELECT
    name,
    (
        SELECT AVG(price)
        FROM Products AS p2
        WHERE p2.category = p1.category
    ) AS average_category_price
FROM Products AS p1;
```

3. 
```
SELECT u.username, playlist_counts.playlist_count
FROM (
    SELECT users.user_id, COUNT(playlists.playlist_id) AS playlist_count
    FROM Users AS users
    LEFT JOIN Playlists AS playlists
        ON playlists.user_id = users.user_id
    GROUP BY users.user_id
) AS playlist_counts
JOIN Users AS u
    ON u.user_id = playlist_counts.user_id
WHERE playlist_counts.playlist_count > (
    SELECT AVG(user_counts.playlist_count)
    FROM (
        SELECT users.user_id, COUNT(playlists.playlist_id) AS playlist_count
        FROM Users AS users
        LEFT JOIN Playlists AS playlists
            ON playlists.user_id = users.user_id
        GROUP BY users.user_id
    ) AS user_counts
);
```
4. 
```
SELECT DISTINCT user_id
FROM Orders
WHERE total_amount > (
    SELECT AVG(total_amount)
    FROM Orders
);
```