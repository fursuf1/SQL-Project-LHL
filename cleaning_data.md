**What issues will you address by cleaning the data?**

**
__*Creating new columns with updated data types and converting between data types to make it easily manageable.
-Finding relationships in diffrent table of databases, removing Nulls and converting to Zero for few columns, changing data types.
-Creating new cleaned table
-Add new columns__



Queries:
Below, provide the SQL queries you used to clean your data.

*Finding relationships in diffrent table of databases, removing Nulls and converting to Zero for few columns, changing data types*

UPDATE all_sessions 
SET timeonsite = COALESCE(timeonsite, '0')

UPDATE analytics
SET timeonsite = COALESCE(timeonsite, '0')

UPDATE products
SET sentimentmagnitude = COALESCE(sentimentmagnitude, '0')

UPDATE analytics
SET unit_price = 0
WHERE unit_price IS NULL;

UPDATE analytics
SET fullvisitoridtemp = fullvisitorid;

SELECT MAX(LENGTH(fullvisitoridtemp)) AS max_length_analytics
FROM analytics;

SELECT MAX(LENGTH(fullvisitorid_tmp::text)) AS max_length_all_sessions
FROM all_sessions;

UPDATE analytics SET
    new_unit_price=new_unit_price/1000000;

UPDATE all_sessions SET
	SET city = 0
	WHERE city LIKE '%no%'

UPDATE all_sessions SET
	SET country = 0
	WHERE country LIKE '%no%'

UPDATE public.all_sessions WHERE "v2ProductName" ILIKE '%Google%'

UPDATE sales_by_sku SET
	SET country = 0
	WHERE country LIKE '%no%'

DELETE FROM all_sessions
WHERE fullvisitorid IN (
    SELECT fullvisitorid
    FROM all_sessions
    GROUP BY fullvisitorid
    HAVING COUNT(*) > 1);

*Creating new columns with updated data types and converting between data types to make it easily manageable.


UPDATE all_sessions 
SET totaltransactionrevenue_new = CAST(totaltransactionrevenue AS DOUBLE PRECISION)
WHERE totaltransactionrevenue <> '';

ALTER TABLE all_sessions DROP COLUMN totaltransactionrevenue;

ALTER TABLE all_sessions RENAME COLUMN totaltransactionrevenue_new TO totaltransactionrevenue;

ALTER TABLE all_sessions ADD COLUMN productrevenue_new DOUBLE PRECISION;

ALTER TABLE analytics ADD COLUMN fullvisitoridtemp VARCHAR(100);

ALTER TABLE all_sessions ADD COLUMN totaltransactionrevenue_new DOUBLE PRECISION;

UPDATE all_sessions 
SET productrevenue_new = CAST(productrevenue AS DOUBLE PRECISION)
WHERE productrevenue <> '';

ALTER TABLE all_sessions DROP COLUMN productrevenue ;

ALTER TABLE all_sessions RENAME COLUMN productrevenue_new TO productrevenue;

UPDATE all_sessions SET ecommerceactionoption = COALESCE (ecommerceactionoption, 'NA')


*verifying if all went ok and data searchable

SELECT fullvisitorid FROM all_sessions GROUP BY fullvisitorid HAVING COUNT(*) > 1



*Creating new cleaned table

CREATE TABLE salesbysku AS 
SELECT DISTINCT productsku, CAST(totalordered AS integer) AS totalordered
 FROM sales_by_sku
 WHERE productsku IS NOT NULL
    AND totalordered != 0

*Probbing data further*

SELECT a.visitid, a.fullvisitorid
FROM analytics a
LEFT JOIN all_sessions s ON a.visitid = s.visitid
WHERE s.visitid IS NULL

*Creating new columns for different data types*

ALTER TABLE all_sessions DROP COLUMN fullvisitorid_tmp;

ALTER TABLE analytics ADD COLUMN fullvisitoridtemp VARCHAR(100); -- Use an appropriate size for your data

UPDATE analytics
SET fullvisitoridtemp = fullvisitorid;

SELECT MAX(LENGTH(fullvisitoridtemp)) AS max_length_analytics
FROM analytics;

SELECT MAX(LENGTH(fullvisitorid_tmp::text)) AS max_length_all_sessions
FROM all_sessions;

SELECT 
  CASE 
    WHEN transactionrevenue = '' THEN 0 
    ELSE CAST(COALESCE(transactionrevenue::numeric, 0) / 1000000 AS INTEGER)
  END AS revenue_in_millions
FROM all_sessions;

*OR with a new column*

ALTER TABLE all_sessions
ADD COLUMN transactionrevenueinmillions INTEGER;


UPDATE all_sessions
SET transactionrevenueinmillions = 
  CASE 
    WHEN transactionrevenue = '' THEN 0 
    ELSE CAST(COALESCE(transactionrevenue::numeric, 0) / 1000000 AS INTEGER)
  END;