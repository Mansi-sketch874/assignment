Tasks
1.
Create two tables, influencers and brands, with at least 3 sample rows each. Use a FULL OUTER JOIN to list all influencers and brands, showing influencer_name and brand_name, matching on city. If there is no match, display NULL for the missing side.<br><br><em><strong>Hint:</strong> Use LEFT JOIN, RIGHT JOIN, and UNION if your SQL dialect does not support FULL OUTER JOIN directly.</em>

```sql
CREATE TABLE influencers (
    influencer_id INT PRIMARY KEY,
    influencer_name VARCHAR(100),
    city VARCHAR(100)
);

CREATE TABLE brands (
    brand_id INT PRIMARY KEY,
    brand_name VARCHAR(100),
    city VARCHAR(100)
);

INSERT INTO influencers (influencer_id, influencer_name, city) VALUES
    (1, 'Ava Sharma', 'Mumbai'),
    (2, 'Noah Patel', 'Delhi'),
    (3, 'Mia Chen', 'Bengaluru'),
    (4, 'Liam Singh', 'Pune');

INSERT INTO brands (brand_id, brand_name, city) VALUES
    (1, 'Glow Cosmetics', 'Mumbai'),
    (2, 'Urban Threads', 'Delhi'),
    (3, 'TechNest', 'Hyderabad'),
    (4, 'Bean & Brew', 'Chennai');

```
SELECT i.influencer_name, b.brand_name
FROM influencers AS i
LEFT JOIN brands AS b
ON i.city = b.city

UNION

SELECT i.influencer_name, b.brand_name
FROM influencers AS i
RIGHT JOIN brands AS b
ON i.city = b.city;
```

The `FULL OUTER JOIN` includes matching cities and all non-matching rows; the unmatched side is returned as `NULL`.

2.
Given a table called playlists with columns (id, playlist_name, parent_playlist_id), write a SELF JOIN query to display each playlist alongside its parent playlist's name, similar to how Spotify might nest playlists.

```
SELECT
    p.id AS playlist_id,
    p.playlist_name AS playlist_name,
    parent.playlist_name AS parent_playlist_name
FROM playlists AS p
 JOIN playlists AS parent
    ON p.parent_playlist_id = parent.id;
```


3.
Create two tables: users and offers. Write a CROSS JOIN query to generate all possible combinations of users and offers, displaying user_name and offer_title. Explain in a comment how this could be used for a Flipkart-style personalized offer campaign.

```
CREATE TABLE users (
    user_id   INT PRIMARY KEY,
    user_name VARCHAR(100) NOT NULL
);

CREATE TABLE offers (
    offer_id    INT PRIMARY KEY,
    offer_title VARCHAR(200) NOT NULL
);

SELECT
    u.user_name,
    o.offer_title
FROM users AS u
CROSS JOIN offers AS o;

-- A Flipkart-style campaign could use these user-offer combinations as
-- candidate pairings, then filter them by each user's interests, eligibility,
-- or purchase history before sending personalized offers.

```
4.
You have an employees table with columns (id, name, manager_id). Write a SELF JOIN to display each employee's name along with their manager's name. Then, modify your query to only show employees who do not have a manager (i.e., top-level managers).
```
-- Show every employee and their manager, if they have one.
SELECT
    e.name AS employee_name,
    m.name AS manager_name
FROM employees AS e
LEFT JOIN employees AS m
    ON e.manager_id = m.id;

-- Show only employees who do not have a manager (top-level managers).
SELECT
    e.name AS employee_name,
    m.name AS manager_name
FROM employees AS e
LEFT JOIN employees AS m
    ON e.manager_id = m.id
WHERE e.manager_id IS NULL;
```


5.
Use ChatGPT or Copilot to help you write a SQL query that finds all pairs of users from a users table who live in the same city (excluding pairs where the user is compared with themselves). Paste the query and briefly describe how the AI helped you improve or debug it.

```
SELECT
    u1.user_id AS user1_id,
    u1.user_name AS user1_name,
    u2.user_id AS user2_id,
    u2.user_name AS user2_name,
    u1.city
FROM users AS u1
JOIN users AS u2
    ON u1.city = u2.city
   AND u1.user_id < u2.user_id;

   ```

   AI helped me choose a self-join to compare users in the same city. It also suggested using u1.user_id < u2.user_id to exclude matching a user with themselves and prevent each pair from appearing twice.