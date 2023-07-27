What issues will you address by cleaning the data? 

Recalculating errornous data by dividing unit_cost by 1,000,000 to get the accurate unit_cost of procucts. 
Updating blank cells.
All sessions - updating country column containing values '(not set)' to NULL '''








Queries:
Below, provide the SQL queries you used to clean your data.
Analytics table:
UPDATE analytics SET unit_price = unit_price/1000000;


  UPDATE analytics SET units_sold  
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

