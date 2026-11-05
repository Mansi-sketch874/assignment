tasks
1.
Create a SQL view named TopRatedRestaurants that selects the restaurant name, average rating, and total number of reviews from a table of Zomato-style restaurant reviews, showing only restaurants with an average rating above 4.0.
2.
Update the TopRatedRestaurants view to also include the city column from the original restaurants table by joining the relevant tables.<br><br><em><strong>Hint:</strong> Use an INNER JOIN to combine data from both tables in your view definition.</em>
3.
Try to update the average rating column directly through the TopRatedRestaurants view and observe what error or limitation occurs. Write down the exact error message and explain why this happens based on SQL view limitations.
4.
Create a view called DailyOrderSummary that shows, for each date, the total number of food orders and the total revenue from a Swiggy-style orders table. Ensure the view only includes dates from the last 30 days.<br><br><em><strong>Constraint:</strong> Use WHERE and GROUP BY clauses in your view definition.</em>
5.
List 3 good practices you should follow when creating SQL views for analytics dashboards, and for each, give a one-line example related to a Flipkart sales reporting scenario.

1. 
```
CREATE VIEW TopRatedRestaurants AS
SELECT
    restaurant_name,
    AVG(rating) AS average_rating,
    COUNT(*) AS total_reviews
FROM restaurant_reviews
GROUP BY restaurant_name
HAVING AVG(rating) > 4.0;

2. 
CREATE OR REPLACE VIEW TopRatedRestaurants AS
SELECT
    r.restaurant_name,
    AVG(rv.rating) AS average_rating,
    COUNT(*) AS total_reviews,
    r.city
FROM restaurants AS r
INNER JOIN restaurant_reviews AS rv
    ON rv.restaurant_id = r.restaurant_id
GROUP BY
    r.restaurant_id,
    r.restaurant_name,
    r.city
HAVING AVG(rv.rating) > 4.0;

3. 
UPDATE TopRatedRestaurants
SET average_rating = 5.0
WHERE restaurant_name = 'Example Restaurant';

4. 
CREATE VIEW DailyOrderSummary AS
SELECT
    DATE(order_date) AS order_date,
    COUNT(*) AS total_orders,
    SUM(total_amount) AS total_revenue
FROM orders
WHERE order_date >= NOW() - INTERVAL 30 DAY
  AND order_date <= NOW()
GROUP BY DATE(order_date);

5. 
CREATE VIEW DailyOrderSummary AS
SELECT
    DATE(order_date) AS order_date,
    COUNT(*) AS total_orders,
    SUM(total_amount) AS total_revenue
FROM orders
WHERE order_date >= NOW() - INTERVAL 30 DAY
  AND order_date <= NOW()
GROUP BY DATE(order_date);



