Question 1: To find the total number of unique visitors.

SQL Queries:
```sql
SELECT COUNT(DISTINCT(full_visitor_id)) AS num_unique_visitors
FROM all_sessions
```


Answer: 
14223


Question 2: To find the total number of unique visitors by referring sites.

SQL Queries:
```sql
SELECT channel_grouping,
		COUNT(DISTINCT(full_visitor_id)) AS num_unique_visitors
FROM all_sessions
GROUP BY channel_grouping
HAVING channel_grouping = 'Referral'
```


Answer:
2419


Question 3: To find each unique product viewed by each visitor.


SQL Queries:
```sql
SELECT
     DISTINCT  full_visitor_id, product_sku, v2_product_name 
FROM
    all_sessions
```


Answer:
The list of unique products per visitor was obtained using the DISTINCT keyword.


Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:
