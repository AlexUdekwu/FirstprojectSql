What issues will you address by cleaning the data? 

Recalculating errornous data by dividing unit_cost by 1,000,000 to get the accurate unit_cost of procucts. 
Updating blank cells.
All sessions - updating country column containing values '(not set)' to NULL '''
-- Check if the visitstarttime timestamp and date columns have mismatching dates. -- Changing where statement to, (WHERE DATE(TO_TIMESTAMP(visitstarttime)) <> date AND DATE(TO_TIMESTAMP(visitstarttime)) > date;), -- gave the same results, so it is most likely that the date column has not matched with the date in viststarttime's time zone. -- Therefore, the dates column is changed to have the dates in visitstarttime.
-- The date in all_sessions also has the same error as analytics. -- After fixing the dates in the analytics table, -- An interim table is created to store all the visitid values and their dates. -- The CTE takes the visitid's and dates from both the all_sessions and analytics tables. -- Insert the values from the CTE into the interim table so that keys from both the tables are preserved. -- The dates from the all_sessions table can be removed now that they are preserved in visitid_values table. -- The date and time are also preserved in a new table "all_sessions_date_time". -- The date and time in all_sessions could be due to an error between time zones. -- However even with adding the time to date in the all_sessions column, it -- still does not match up well with visitstarttime in analytics. -- The query for the all_sessions_date_time table joined with analytics may shows that date and time could -- have been recorded in one timezone while the timestamp in the analytics table is recorded in distinct time zones.

-- The units_sold value needs to be an integer and the column has negative values. -- Remove negative values in units sold. -- Check if negative values are gone in units_sold.

-- The pageviews need to be converted to an int.

-- The transactionrevenue column is redundant and all the information in totaltransactionrevenue. -- The data is preserved in totaltransactionrevenue after removing the transactionrevenue column. -- Remove the brackets and 'not available in demo ...' from the city column in all_sessions. -- totaltransactionrevenue is divided by 1000000. -- no values of totaltransactionrevenue is less than 0. -- transactions is either 1 or NULL so it is converted from float to integer. -- Alter time in all_sessions to a time interval. -- Alter timeonsite in all_sessions to a time interval. -- sessionqualitydim have whole numbers so they are converted to int. -- all_sessions, type column looks like a boolean but the character titles may still retain some -- important information. The VARCHAR was limited to 10 characters. -- productrefundamount is only null but may still be important later if refunds are given. -- divide productprice by 1000000. -- There was an error with the time and timeonsite, but the values are still preseverd on the all_sessions_time table. -- A query is created to match up the all_sessions_time.time, all_sessions_time.timeonsite, -- all_sessions_date_time.date, analytics.visitstarttime USING(visitid). -- productrevenue is divided by 1000000. -- All revenue contains valid decimal or integer values.

-- Removed the (not set) category from v2productcategory -- Removed missing title from v2productcategory. -- Removed slashes from v2productcategory. -- Removed single option only and (not set) from productvarient and lower characters to 10. -- Character length was decreased to 10 in currencycode. -- itemquantity was changed to int and values were left NULL. -- transactionid character length was decreased to 20. -- A value longer than 100 characters was removed from pagetitle. -- pagetitle was set to 100 characters. -- seachkeyword as altered to VARCHAR(20) from real. -- ecommerceaction_option was set to 20 characters in lenght.









Queries:
Below, provide the SQL queries you used to clean your data.
Analytics table:
UPDATE analytics SET unit_price = unit_price/1000000;


 ``` UPDATE analytics SET units_sold  
  CASE, 
  WHEN units_sold = '',
  THEN 'NULL' ELSE units_sold END;

  UPDATE analytics SET revenue 
  CASE, 
  WHEN revenue = '',
  THEN 'NULL' ELSE revenue END;

All_sessions table:
 UPDATE all_sessions SET country = CASE,
  WHEN country = '(not set)',
  THEN 'NULL' ELSE country END;

  UPDATE all_sessions SET city = CASE
  WHEN city = '(not set)', 
  THEN 'NULL' ELSE city END;
```
