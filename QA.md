What are your risk areas? Identify and describe them.

The main goal was to normalize the data to become more structured and less redundant, and a lot of this was done during QA and data cleaning. However, two tables still have duplicate values, the reason for this are described below or in Challenges and Future Goals in README.

THE FOLLOWING RISK AREAS ARE LISTED UNDER FUTURE GOALS IN README. Product_information still has a lot of duplicate rows for product_sku to be a primary key, it still requires a lot of work to filter through all the productnames and productcategory values. Also filtering down the productnames may could cause a loss in information, such as brand names. More time is needed to go through the product_information table before product_sku can be a primary key. However, the tables products, sales_report, and sales_by_sku can now be removed because all the infomation is preserved in product_information. The order details are removed from product_information are now in the table product_order with product_sku being the primary key.

Referential integrity check : to identify relationship between tables to enable successful performance of queries.
Incomplete data: A situation where by the columns are not having data to run queries
Duplicate data: A situation of having a data appear more than once in a column/row 
Orders out of range: As the name implies, a situation where by data is misrepresented in a column .Example; having a negative value on orders column. 




QA Process:
Describe your QA process and include the SQL queries used to execute it.

Executing referential integrity Check for "sales_report" and "all_sessions" tables using ‘productsku’.

``` WITH CTE AS(
 SELECT 'sales_report' AS table_name,
 COUNT() AS count_rows,
 'productsku' AS key_name, 
COUNT(DISTINCT(productsku)) AS count_distinct_productsku
 FROM sales_by_sku
```
UNION ALL

``` SELECT 'all_sessions' AS table_name,  --f
COUNT() AS count_rows,
'productsku' AS key_name, 
COUNT(DISTINCT(productsku)) AS count_distinct_productsku,
FROM all_sessions 
),

``` QA_raw AS( SELECT COUNT(DISTINCT(count_of_productsku)) as test_result FROM cte 
)

SELECT 'referential_integrity_check' AS qa_test_name,
CASE WHEN test_result = 1, 
THEN 'pass' ELSE 'fail', 
END AS qa_result FROM qa_raw;
```
