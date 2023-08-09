What issues will you address by cleaning the data?


1. Date values need to be structured in date datatype format.

Query used:
```SQL
SELECT DATE(date) AS clean_date
FROM all_sessions
```


2. Time values also need to be restructured.

Query used:
```SQL
SELECT TO_CHAR(TO_TIMESTAMP(time), 'HH:MI:SS AM') 
FROM all_sessions
```


3. Columns with dollar amounts needed to be reduced to the proper units.

Filtering and cleaning revenue to the right units 

Query used:
```SQL
SELECT total_transaction_revenue, 
		ROUND((total_transaction_revenue / 1000000), 2) clean_total_revenue
FROM all_sessions
WHERE total_transaction_revenue IS NOT NULL
```

Filtering and cleaning product_price to the right units 

Query used:
```SQL
SELECT product_price, 
		ROUND((product_price / 1000000), 2) product_price_clean
FROM all_sessions
WHERE product_price IS NOT NULL
```

Filtering and cleaning unit_price to the right units 

Query used:
```SQL
SELECT unit_price, ROUND((unit_price / 1000000), 2) unit_price_clean
FROM analytics
WHERE unit_price IS NOT NULL
```


4. Country and city columns contain null entries in different structures.

Query used:
```SQL
SELECT 
		country,
		CASE WHEN country IN ('(not set)', 'not available in demo dataset')
				THEN NULL
				ELSE country
		END AS clean_country,
		city,
		CASE WHEN city IN ('(not set)', 'not available in demo dataset')
				THEN (CASE WHEN country NOT IN ('(not set)', 'not available in demo dataset')
						 	THEN country
					 END)
			ELSE COALESCE(city, country)
		END AS clean_city
FROM all_sessions
```


5. Proper structure for NULL entries in product categories column.

Query used:
```SQL
SELECT 
		v2_product_category,
		CASE WHEN v2_product_category IN ('(not set)', '${escCatTitle}')
				THEN NULL
				ELSE v2_product_category
		END AS clean_category
FROM all_sessions
```