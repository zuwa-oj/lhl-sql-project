Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
```sql
WITH clean_cte AS
(	
	SELECT 
		CASE WHEN country IN ('(not set)', 'not available in demo dataset')
				THEN NULL
				ELSE country
		END AS country,
		CASE WHEN city IN ('(not set)', 'not available in demo dataset')
				THEN (CASE WHEN country NOT IN ('(not set)', 'not available in demo dataset')
						 	THEN country
					 END)
			ELSE COALESCE(city, country)
		END AS city,
		ROUND((total_transaction_revenue / 1000000), 2) total_transaction_revenue
	FROM all_sessions
	WHERE total_transaction_revenue IS NOT NULL
)
SELECT 
	country, 
	city, 
	MAX(total_transaction_revenue) max_total_revenue
FROM clean_cte
GROUP BY country, city
ORDER BY max_total_revenue DESC
```


Answer:
The highest transaction revenue was observed to be from United States with a wide range of cities. Other countries with high level of transaction revenues include Israel (Tel Aviv-Yafo) and Australia (Sydney).



**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
```sql
WITH all_sessions_clean AS
	(	
		SELECT 
			CASE WHEN country IN ('(not set)', 'not available in demo dataset')
					THEN NULL
					ELSE country
			END AS country,
			CASE WHEN city IN ('(not set)', 'not available in demo dataset')
					THEN (CASE WHEN country NOT IN ('(not set)', 'not available in demo dataset')
								THEN country
						 END)
				ELSE COALESCE(city, country)
			END AS city,
			product_sku
		FROM all_sessions
	)
SELECT 
	al.country, 
	al.city, 
	ROUND(AVG(pr.ordered_quantity)) average_product_quantity
FROM all_sessions_clean al
JOIN products pr USING(product_sku)
GROUP BY al.country, al.city
ORDER BY al.country
```


Answer:
The average number of products ordered from visitors in each city and country were observed to highly variable, ranging from an average of 2 products ordered from El Paso, United States; 395 from Vancouver, Canada; 1033 from Wrexham, United Kingdom; and 7589 from Council Bluffs, United States.




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
```sql
WITH clean_cte AS
(	
	SELECT 
		CASE WHEN country IN ('(not set)', 'not available in demo dataset')
				THEN NULL
				ELSE country
		END AS country,
		CASE WHEN city IN ('(not set)', 'not available in demo dataset')
				THEN (CASE WHEN country NOT IN ('(not set)', 'not available in demo dataset')
						 	THEN country
					 END)
			ELSE COALESCE(city, country)
		END AS city,
		CASE WHEN v2_product_category IN ('(not set)', '${escCatTitle}')
				THEN NULL
				ELSE v2_product_category
		END AS v2_product_category	
	FROM all_sessions
	WHERE v2_product_category IS NOT NULL
)
SELECT 
	country, 
	city, 
	v2_product_category,
	COUNT(v2_product_category) OVER (PARTITION BY v2_product_category) AS category_count
FROM clean_cte
ORDER BY category_count DESC
```


Answer:
It was observed that the highest performing product category was the most popular across most of the cities and countries.




**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
```sql
WITH all_sessions_cte AS 
(
	SELECT 
		CASE WHEN country IN ('(not set)', 'not available in demo dataset')
			THEN NULL
			ELSE country
		END AS country, 
		CASE WHEN city IN ('(not set)', 'not available in demo dataset')
				THEN (CASE WHEN country NOT IN ('(not set)', 'not available in demo dataset')
						 	THEN country
					 END)
			ELSE COALESCE(city, country)
		END AS city, 
		v2_product_name product_name, 
		COUNT(v2_product_name) OVER (PARTITION BY v2_product_name) AS product_count
	FROM all_sessions
	ORDER BY product_count DESC, country
)
SELECT 
	country, 
	city, 
	product_name,
	MAX(product_count) max_sold
FROM all_sessions_cte 
GROUP BY country, city, product_name, product_count 
ORDER BY max_sold DESC, country
```


Answer:
The top-selling product from a wide range of cities and countries was observed to be "Google Men's 100% Cotton Short Sleeve Hero Tee White". The "22 oz YouTube Bottle Infuser" is a top-selling product in the United States, and "YouTube Twill Cap" is also another top-selling product across a wide range of cities and countries.



**Question 5: Can we summarize the impact of revenue generated from each city/country?**


SQL Queries:
```sql
WITH clean_cte AS
(	
	SELECT 
		CASE WHEN country IN ('(not set)', 'not available in demo dataset')
				THEN NULL
				ELSE country
		END AS country,
		CASE WHEN city IN ('(not set)', 'not available in demo dataset')
				THEN (CASE WHEN country NOT IN ('(not set)', 'not available in demo dataset')
						 	THEN country
					 END)
			ELSE COALESCE(city, country)
		END AS city,
		ROUND((total_transaction_revenue / 1000000), 2) total_revenue
	FROM all_sessions
	WHERE total_transaction_revenue IS NOT NULL
)
SELECT 
	country, 
	city, 
	SUM(total_revenue) sum_total_revenue
FROM clean_cte
GROUP BY country, city
ORDER BY sum_total_revenue DESC
```


Answer:
The distribution of total revenue generated across countries was observed to be largely skewed towards the United States where most of the revenue comes from. However, the distribution is observed to be broader across cities within the United States.






