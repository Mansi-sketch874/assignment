Tasks

Write an SQL query using CONCAT to combine first_name and last_name columns from a users table into a single full_name column and display the result.
2.
Given a table called playlists with a column song_title, write a query to display all song titles in uppercase using the UPPER function.
3.
You have a table food_items with a column item_code that sometimes contains extra spaces at the beginning or end. Write an SQL query to select item_code values after removing all leading and trailing spaces using TRIM.
4.
In a table called movies, the column imdb_id contains codes like 'tt1234567'. Write a query to extract just the numeric part (the last 7 characters) using RIGHT and display it as movie_number.<br><br><em><strong>Hint:</strong> Use RIGHT(imdb_id, 7) to get the numeric part.</em>
5.
A products table has a column sku_code where some entries use dashes (e.g. 'MOB-123-XY'). Write an SQL query to replace all dashes '-' with underscores '_' in sku_code using the REPLACE function and display the updated codes.

**solution**

1. 
```
select concat(first_name, last_name) as full_name from users
to give space between names-
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM users;
```
2. select upper (song_title)as song_title from playlists

3. 
```
select trim(item_code) as item code from food_items
```

4. 
```
select right(imdb_id,7) as movie_number from movies
```
5. 
```
select replace(sku_code, '-','_') as sku_code from products
```