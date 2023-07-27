What are your risk areas? Identify and describe them.
referential integrity check
incomplete data
duplicate data
orders out of range



QA Process:
Describe your QA process and include the SQL queries used to execute it.

Performed referential integrity Check for "sales_by_sku" and "all_sessions" tables.

WITH CTE AS( SELECT 'sales_by_sku' AS table_name, COUNT() AS count_rows, 'productsku' AS key_name, COUNT(DISTINCT(productsku)) AS count_of_productsku FROM sales_by_sku

UNION ALL

SELECT 'sales_report' AS table_name,
--f COUNT() AS count_rows,
'sales_report' AS key_name, 
COUNT(DISTINCT(productsku)) AS count_of_productsku,
FROM sales_report ),

QA_raw AS( SELECT COUNT(DISTINCT(count_of_productsku)) as test_result FROM cte )

SELECT 'referential_integrity_check' AS qa_test_name,
CASE WHEN test_result = 1, 
THEN 'pass' ELSE 'fail', 
END AS qa_result FROM qa_raw;
