Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
SELECT city,
	   country,
       totaltransactionrevenue
FROM all_sessions
WHERE totaltransactionrevenue IS NOT NULL
	  AND city NOT LIKE '%demo%' 
	  AND city NOT LIKE '%set%' 
ORDER BY 3 DESC
LIMIT 5;



Answer:


| City          | Country              | Revenue         |
|:--------------|:---------------------|:---------------:|
| Atlanta       | United States        | 742480000       |
| Sunnyvale     | United States        | 649240000       |
| Tel Aviv-Yafo | Isreal               | 602000000       |
| Los Angeles   | United States        | 363000000       |
| Sydney        | Australia            | 358000000       |


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
SELECT ss.city,
	   ss.country,
       AVG(sr.total_ordered) AS avg_product_qty
FROM all_sessions ss 
JOIN sales_report sr ON sr.productsku = ss.productsku
WHERE sr.total_ordered IS NOT NULL
	  AND city NOT LIKE 'set%' 
	  AND city NOT LIKE 'set)%' 
GROUP BY ss.city, ss.country
ORDER BY AVG(sr.total_ordered) DESC
LIMIT 5;



Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
SELECT ss.city,
       ss.country,
       ss.v2productcategory as product_category,
       AVG(sr.total_ordered) AS avg_product_qty
FROM all_sessions ss 
JOIN sales_report sr ON sr.productsku = ss.productsku
WHERE sr.total_ordered IS NOT NULL
	  AND city NOT LIKE 'set%' 
	  AND city NOT LIKE 'set)%' 
	  AND v2productcategory NOT LIKE 'set)%'
GROUP BY ss.city, ss.country, ss.v2productcategory
ORDER BY AVG(sr.total_ordered) DESC
LIMIT 100;
```


Answer:
Items purchase were mostly home related as seen on table




**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
SELECT
	   alls.city, alls.country,
	   sr.name, 
	   MAX(sr.total_ordered)AS max_product
	   FROM all_sessions AS alls
JOIN sales_report AS sr ON alls.productsku = sr.productsku
GROUP BY alls.city, alls.country, sr.name


Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
SELECT 
		alls.city, alls.country,
SUM(alls.productprice*sr.total_ordered) AS total_revenue
FROM all_sessions AS alls
JOIN sales_report AS sr ON alls.productsku = sr.productsku
GROUP BY alls.city, alls.country;



Answer:







