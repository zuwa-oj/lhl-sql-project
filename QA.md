### What are your risk areas? Identify and describe them.

The data was inspected and observed to be fully anonymized as it did not contain any personal user_id information, so there were no data privacy risks associated with the data set.

The data was observed to have many structural issues ranging from column names not stored in acceptable case; duplicate and null entries in different formats; and inconsistent units for time, and financial data.

Overall, the data was observed to contain an unusally high proportion of null entries and inconsistencies so the integrity of the data would not be expected to yield high confidence results after data analysis.

### QA Process:
Describe your QA process and include the SQL queries used to execute it.

Data validation was carried out before proceeding to check for completeness, uniqueness and consistency.

Columns were inspected for uniqueness using the DISTINCT keyword and the product_sku column in the products table passed the uniqueness test so it could be assigned as the primary key on the products table.
Other columns were identifed to contain duplicates including the full_visitor_id column which resulted in not satisfying constraints for being a primary key in the all_sessions table.
Completeness checks were carried out on columns by filtering for nulls before analysing the sales revenue data.

#### Queries used:

```SQL
SELECT 'all_sessions' as table_name, 
        COUNT(*) AS count_rows, 
        COUNT(DISTINCT(full_visitor_id)) AS count_distinct_id 
FROM all_sessions

UNION ALL

SELECT 'products' as table_name, 
       COUNT(*) AS count_rows, 
       COUNT(DISTINCT(product_sku)) AS count_distinct_id
FROM products

UNION ALL

SELECT 'sales_by_sku' as table_name, 
       COUNT(*) AS count_rows, 
       COUNT(DISTINCT(product_sku)) AS count_distinct_id
FROM sales_by_sku

UNION ALL 

SELECT 'analytics' as table_name, 
       COUNT(*) AS count_rows, 
       COUNT(DISTINCT(full_visitor_id)) AS count_distinct_id
FROM analytics

UNION ALL 

SELECT 'sales_report' as table_name, 
       COUNT(*) AS count_rows, 
       COUNT(DISTINCT(product_sku)) AS count_distinct_id
FROM sales_report
```
