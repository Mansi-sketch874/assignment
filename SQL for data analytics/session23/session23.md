Tasks
1.
Write an SQL query to find the top 5 highest-rated restaurants in Koramangala, showing their name, average rating, and number of votes.
2.
Using the Zomato Bangalore dataset, create an SQL query that lists all unique cuisines available in Indiranagar along with the count of restaurants offering each cuisine.
3.
Write an SQL query to calculate the average cost for two people for each restaurant type (such as 'Cafe', 'Casual Dining', etc.) and order the results from most to least expensive.
4.
Find all restaurants that have a rating below 3.0 but more than 200 votes, and suggest a possible marketing action for these based on your findings.<br><br><em><strong>Hint:</strong> Think about discounts, partnerships, or events to improve ratings or attract new customers.</em>
5.
Use ChatGPT to generate an SQL query that segments restaurants into three market categories: 'Budget' (cost for two < 500), 'Mid-range' (500-1500), and 'Premium' (>1500). Test and run the query on your dataset, and paste the working query in your submission.

1. 
SELECT
    name,
    CAST(SUBSTRING_INDEX(rate, '/', 1) AS DECIMAL(3,1)) AS average_rating,
    votes
FROM zomato
WHERE location = 'Koramangala'
  AND rate REGEXP '^[0-5](\\.[0-9]+)?/5$'
ORDER BY average_rating DESC, votes DESC
LIMIT 5;

2. 
   name,
        TRIM(SUBSTRING_INDEX(remaining, ',', 1)) AS cuisine,
        CASE
            WHEN remaining LIKE '%,%'
            THEN SUBSTRING(remaining, LOCATE(',', remaining) + 1)
            ELSE ''
        END AS remaining
    FROM cuisine_split
    WHERE remaining <> ''
)
SELECT
    cuisine,
    COUNT(DISTINCT name) AS restaurant_count
FROM cuisine_split
WHERE cuisine <> ''
GROUP BY cuisine
ORDER BY cuisine;

3. SELECT
    rest_type AS restaurant_type,
    ROUND(
        AVG(CAST(REPLACE(`approx_cost(for two people)`, ',', '') AS DECIMAL(10, 2))),
        2
    ) AS average_cost_for_two
FROM zomato
WHERE rest_type IS NOT NULL
  AND `approx_cost(for two people)` IS NOT NULL
GROUP BY rest_type
ORDER BY average_cost_for_two DESC;

4. SELECT
    name,
    CAST(SUBSTRING_INDEX(rate, '/', 1) AS DECIMAL(3,1)) AS rating,
    votes
FROM zomato
WHERE rate REGEXP '^[0-5](\\.[0-9]+)?/5$'
  AND CAST(SUBSTRING_INDEX(rate, '/', 1) AS DECIMAL(3,1)) < 3.0
  AND votes > 200
ORDER BY rating ASC, votes DESC;

5. 
WITH restaurant_costs AS (
    SELECT
        name,
        CAST(
            REPLACE(TRIM(`approx_cost(for two people)`), ',', '')
            AS UNSIGNED
        ) AS cost_for_two
    FROM zomato
    WHERE `approx_cost(for two people)` IS NOT NULL
      AND TRIM(`approx_cost(for two people)`) <> ''
)
SELECT
    name,
    cost_for_two,
    CASE
        WHEN cost_for_two < 500 THEN 'Budget'
        WHEN cost_for_two <= 1500 THEN 'Mid-range'
        ELSE 'Premium'
    END AS market_category
FROM restaurant_costs
ORDER BY cost_for_two;