# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals

To examine if the product revenue, name, category patterns can be extrapolated from the website data provided? What are the patterns with respect to user, country, and/or city.


## Process


When importing data, in order for the process to succeed, all columns with characters needed to have appropriate lengths.

The pageTitle column in the all_sessions table needed to be extended to 600 characters in order to import the data, needed to find area where pageTitle > 500 characters.
Column titles needed to be converted to lowercase for easier usage and so the queries tool can recognize the titles typed out after SELECT.

A list of all each file's column names in lowercase was provided by the "read_table_column_name_and_type_function.py", so that the column names and data types could be known before being inserted into the table values in SQL.
During QA, in pgAdmin, list the column names, data types, character max length or number precision, if column is nullable, and if it is updateable.

Divide the unit_price by 1000000 in cleaning_data.

date column reformatted in analytics and all_sessions tables. For full detail on what was done real the cleaning_data file.

Test if conversion to big_int is allowable for fullvisitorid without losing unique IDs for analytics and all_sessions tables.

Reformatted fullvisitorid in tables analytics and all_sessions.

Change the visitstarttime column in analytics from unix epoch to a timestamp.

Check if the visitstarttime timestamp and date columns have mismatching dates.

The date in all_sessions also has the same error as in analytics.

After fixing the dates in the analytics table,

An interim table is created to store all the visitid values and their dates.

The CTE takes the visitid's and dates from both the all_sessions and analytics tables.

The values from the CTE are inserted into the interim table so that keys from both the tables are preserved.

The units_sold value needs to be an integer and the column has negative values.

Need to remove negative values in units sold during data cleansing.

Check if negative values are gone in units_sold.

The pageviews need to be converted to an int.

The transactionrevenue column is redundant in all_sessions and all the information in totaltransactionrevenue.

The data is preserved in totaltransactionrevenue after removing the transactionrevenue column.

Remove the brackets and 'not available in demo ...' from the city column in all_sessions.

totaltransactionrevenue is divided by 1000000, no values of totaltransactionrevenue is less than 0.

transactions is either 1 or NULL so it is converted from float to integer.

Alter time in all_sessions to a time interval.

Alter timeonsite in all_sessions to a time interval.

sessionqualitydim have whole numbers so they are converted to int.

all_sessions, type column looks like a boolean but the character titles may still retain some

important information. The VARCHAR was limited to 10 characters.

productrefundamount is only null but may still be important later if refunds are given.

divide productprice by 1000000.

no values of productprice is less than 0.

There was an error with the time and timeonsite, but the values are still preseverd on the all_sessions_time table.

A query is created in cleaning_data to match up the all_sessions_time.time, all_sessions_time.timeonsite,

all_sessions_date_time.date, analytics.visitstarttime USING(visitid).

productrevenue is divided by 1000000.

no values of productrevenue is less than 0.

all revenue contains valid decimal or integer values.

A query shows there are unique values in sales_by_sku which are not in the tables products or sales_report.

The table should not be deleted because of this but it should be deleted after preserving those values.

This one-to-one table seems redundant and everything could be placed into the sales_report table.

Products should get a unique products key and not the sku column, while sales_report should get a sales key.

In this case, for now sales_report can have productsku as its primary key which link to all_sessions productsku as a foreign key.

the character limit in all the columns with sku was shortened to 20.

v2productname can be shortened to 100 characters.

Reveiwing the format of v2productname to make sure they are capitalized at the start or start with a number. The remaining products start with two capitals.

A CTE to add up all the products with the right format, adds to the same count of rows in v2productname. Therefore all rows have proper format at the start.

Removed the (not set) category from v2productcategory.

Removed missing title from v2productcategory.

Removed slashes from v2productcategory.

Removed single option only and (not set) from productvarient and lower characters to 10.

Character length was decreased to 10 in currencycode.

itemquantity was changed to int and values were left NULL.

transactionid character length was decreased to 20.

A value longer than 100 characters was removed from pagetitle.

pagetitle was set to 100 characters.

seachkeyword as altered to VARCHAR(20) from real.

ecommerceaction_option was set to 20 characters in lenght.

Remove leading spaces in products.name using trim.

I am unsure of the scoring in the sentiment score, if the negative values are necessary.

The score was converted to a scale betwen 0 and 100. The column was then converted to an integer value.

All the numeric values are in valid numeric format.

All the rows are preserved testing each numeric range.

Remove leading spaces in sales_report.name using trim.

sentimentscore in sales_report was altered the same method used in the products table.

Removed (not set) from column country in table all_sessions.

Divided revenue by 1000000.

Verified there are no locations with cities without countries in all_sessions table for QA.

During cleaning_data, found some cities which have more than one distinct country. In some cases this could be true.

Corrected city names and their locations, set city to country.

Fix fullvisitorid's and fill in NULL information if there are missing cities or there are two countries per single id.

London, US may mean London, Ohio.


## Results

The transactionrevenue column is redundant in all_sessions and all the information is stored in totaltransactionrevenue.

 Discreet units were converted to integer while continuous were real.
 
  VARCHAR were shortened to meet the needs of table column with characters and values far above that were reformatted or removed. Remove leading spaces in product names using trim. 
  
  A lot of the product names and categories were cleaned up. 
  
  Verified there are no locations with cities without countries in all_sessions table. Fixed fullvisitorid with country and city, filled in NULL information if there are missing cities or there are two countries per single id. 
  
  Found some cities which have more than one distinct country, and corrected this in cases where this wasn't true. Sentimentscore was converted to a scale betwen 0 and 100.
  
   Duplicate rows are removed from analytics when transferring to new_analytics. Instead of millions of rows there are now over 200000.


## Challenges 

More work needs to go into gathering data to determine if there are multiple orders per visit or if a lot of the information is redundant.

There are five main tables now, all_sessions, new_analytics, product_information, product_order, and fullvisitorid_location. In order to preserve data I still have duplicate product_sku's, fullvisitorid's and visitid's and was not able to get to 3NF.

More work also needs to be done to label fullvisitorid's with a country and city. There is still 116144 fullvisitorid's without a country.

## Future Goals
(what would you do if you had more time?)
Since an approximate estimate showed 99% of the revenue is going to countries and cities which are not listed more work needs to be done to fill in those values, as well as account for the missing revenue.
Need to determine if all_sessions is undercounting revenue or new_analytics is overcounting.

The purchase revenue data is a small snapshot of what the total global purchases done are, and as a result can have a false positive with what items are popular.

More work needs to go into clarifying how much revenue is generated per visitid and to which country and city that revenue is coming from. After data is gathered on revenue, country and city, only then can new_analytics have visitid as its primary key.
