What issues will you address by cleaning the data? 
Recalculating errornous data by Divide unit_cost by 1,000,000 to get the accurate unit_cost of procucts. 
updating blank cells.
All sessions - updating country column containing values '(not set)' to NULL '''








Queries:
Below, provide the SQL queries you used to clean your data.
Analytics
UPDATE analytics SET unit_price = unit_price/1000000

All_sessions
 UPDATE all_sessions SET country = CASE,
  WHEN country = '(not set)',
  THEN 'NULL' ELSE country END;
