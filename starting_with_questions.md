Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

```
SELECT city, country,
totaltransactionrevenue,
FROM all_sessions
WHERE totaltransactionrevenue IS NOT NULL
     AND city NOT LIKE '%demo%' 
     AND city NOT LIKE '%set%' 
ORDER BY 3 DESC
LIMIT 5;
```


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

```
SELECT ss.city, ss.country,
AVG(sr.total_ordered) AS avg_product_qty
FROM all_sessions ss 
JOIN sales_report sr ON sr.productsku = ss.productsku
WHERE sr.total_ordered IS NOT NULL
	  AND city NOT LIKE 'set%' 
	  AND city NOT LIKE 'set)%' 
GROUP BY ss.city, ss.country
ORDER BY AVG(sr.total_ordered) DESC
LIMIT 5;
```


Answer:
Including orders from the sales_by_sku and all_sessions, even analytics in case of NULL values, the country with the highest average order quantity is Vietnam, while the city with the highest order quantity is Dallas.

The country with the highest order quantity is the United States with 5840. The city with the highest order quantity is Dallas with 1148, (not including NULL), followed by Dublin Ireland.




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

```
SELECT ss.city, ss.country,
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
The most times a product category was ordered was in the united states and was a YouTube category with it being the most purchased at 653414 times, followed by the Waze category at 451542 times. The country with the least times a product category was ordered was Slovakia with Android being ordered 1 time. The most popular category for all the counties was YouTube, appearing on the top of the list for 114 countries, and Google coming in second at 12 countries.




**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

```
SELECT  alls.city, alls.country,
        sr.name, 
        MAX(sr.total_ordered)AS max_product
        FROM all_sessions AS alls
JOIN sales_report AS sr ON alls.productsku = sr.productsku
GROUP BY alls.city, alls.country, sr.name
```

Answer:
The most popular product sold overall are 'Yoga Supplies' being ordered 589784 in the United States. The next popular supplies after that were umbrellas, being sold 588886 times.

However, sweater products were the most popular in 73 countries while yoga supplies were the second most popular, being 16 countries.

When not ordering the products sold by editing and grouping the original names, the most popular item was YouTube Wool Heather Cap Heather/Black being the most popular in 30 countries. This is followed with YouTube Youth Short Sleeve Tee Red, and YouTube Twill Cap being the most popular in 17 countries. When not grouping the names of the orders sold, the most popular item sold per country was YouTube Youth Short Sleeve Tee Red with it being 653414 times sold in the United States.






**Question 5: Can we summarize the impact of revenue generated from each city/country?**

```
SQL Queries:

SELECT alls.city, alls.country,
SUM(alls.productprice*sr.total_ordered) AS total_revenue
FROM all_sessions AS alls
JOIN sales_report AS sr ON alls.productsku = sr.productsku
GROUP BY alls.city, alls.country;
```


Answer:

The city with the most revenue generated is San Francisco, United States, with 1564.32 which is 11.05% of the total revenue, (in this case the total revenue from all the non-NULL countries, which includes unrecorded cities with NULL).
The next city being Sunnyvale, with 992.23 generated, and it being 7.01% of the revenue generated, followed by Atlanta with 854.44 and 6.04% revenue generated.
The three lowest were Zurich, Switzerland, with 0.096%, Columbus, United States, with 0.125%, and Houston, United States, with 0.221%. There was a total of 19 cities recorded which generated revenue.





