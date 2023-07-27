What are your risk areas? Identify and describe them.
Referential integrity check : to identify relationship between tables to enable successful performance of queries.
Incomplete data: A situation where by the columns are not having data to run queries
Duplicate data: A situation of having a data appear more than once in a column/row 
Orders out of range: As the name implies, a situation where by data is misrepresented in a column .Example; having a negative value on orders column. 




QA Process:
Describe your QA process and include the SQL queries used to execute it.

Executing referential integrity Check for "sales_report" and "all_sessions" tables using ‘productsku’.

WITH CTE AS(
 SELECT 'sales_report' AS table_name,
 COUNT() AS count_rows,
 'productsku' AS key_name, 
COUNT(DISTINCT(productsku)) AS count_distinct_productsku
 FROM sales_by_sku

UNION ALL

SELECT 'all_sessions' AS table_name,  --f
COUNT() AS count_rows,
'productsku' AS key_name, 
COUNT(DISTINCT(productsku)) AS count_distinct_productsku,
FROM all_sessions 
),

QA_raw AS( SELECT COUNT(DISTINCT(count_of_productsku)) as test_result FROM cte 
)

SELECT 'referential_integrity_check' AS qa_test_name,
CASE WHEN test_result = 1, 
THEN 'pass' ELSE 'fail', 
END AS qa_result FROM qa_raw;

