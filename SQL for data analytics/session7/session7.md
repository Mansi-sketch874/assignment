Tasks
1.Write an SQL query to display the total number of orders placed by each user in a 'food_orders' table, grouped by user_id.
2.Using a 'transactions' table with columns (transaction_id, user_id, amount, payment_method), write an SQL query to show the total amount spent by each payment_method.
3.Given a 'movies' table with columns (movie_id, genre, box_office_collection), write an SQL query to display each genre and its total box_office_collection, but only show genres where the total collection is above 10 crore.<br><br><em><strong>Hint:</strong> Use GROUP BY and HAVING together to filter the aggregated results.</em>
4.Suppose you have a 'playlist' table with columns (playlist_id, user_id, song_id, duration). Write an SQL query to find users who have created playlists with a combined song duration of more than 2 hours (7200 seconds), showing user_id and total duration.

**solution**

1. 
```
SELECT user_id,
       COUNT(*) AS total_orders
FROM food_orders
GROUP BY user_id;
```

2. 
```
select payment_method,
sum(amount) as total_amount_spent
from transactions
group by payment_method
```

3. 
```
select genre,
sum(box_office_collection) as total box_office_collection
from movies
group by genre
having sum (box_office_collection) > 10 crore
```

4. 
```
select user_id
sum(duration) as total_duration
from playlist
group by user_id
having sum(duration) > 2  hours
```

