# What are your risk areas? Identify and describe them.

## Identifying Primary Keys. By removing all duplcates and replacing Null and empty values with 0 for the column which i need, making sure the relationship in tables matches each other

## Risk area for Data Inconsistency with Nullable values


SELECT column_name, data_type, is_nullable
FROM information_schema.columns
WHERE table_name = 'all_sessions';

SELECT column_name, data_type, is_nullable
FROM information_schema.columns
WHERE table_name = 'analytics';

SELECT column_name, data_type, is_nullable
FROM information_schema.columns
WHERE table_name = 'products';

SELECT column_name, data_type, is_nullable
FROM information_schema.columns
WHERE table_name = 'sales_by_sku';

SELECT column_name, data_type, is_nullable
FROM information_schema.columns
WHERE table_name = 'sales_report';


## Searching for columns in each table

SELECT column_name
FROM information_schema.columns
WHERE table_name = 'all_sessions';

SELECT column_name
FROM information_schema.columns
WHERE table_name = 'analytics';

SELECT column_name
FROM information_schema.columns
WHERE table_name = 'products';


SELECT column_name
FROM information_schema.columns
WHERE table_name = 'salesbysku';

SELECT column_name
FROM information_schema.columns
WHERE table_name = 'sales_report';

## Checking zeros for any columns and counting them

SELECT 
    (SELECT COUNT(*) FROM sales_report WHERE "ratio" = 0) AS ratio_zeros,
    (SELECT COUNT(*) FROM sales_report WHERE "totalordered" = 0) AS totalordered_zeros,
    (SELECT COUNT(*) FROM sales_report WHERE "sentimentscore" = 0) AS sentimentscore_zeros,
    (SELECT COUNT(*) FROM sales_report WHERE "sentimentmagnitude" = 0) AS sentimentmagnitude_zeros,
    (SELECT COUNT(*) FROM sales_report WHERE "stocklevel" = 0) AS stocklevel_zeros;



SELECT 
    (SELECT COUNT(*) FROM analytics WHERE "visitnumber" = 0) AS visitnumber_zeros,
    (SELECT COUNT(*) FROM analytics WHERE "visitid" = 0) AS visitid_zeros,
    (SELECT COUNT(*) FROM analytics WHERE "visitstarttime" = 0) AS visitstarttime_zeros,
    (SELECT COUNT(*) FROM analytics WHERE "date" = 0) AS date_zeros,
    (SELECT COUNT(*) FROM analytics WHERE "fullvisitorid" = 0) AS fullvisitorid_zeros,
    (SELECT COUNT(*) FROM analytics WHERE "userid" = 0) AS userid_zeros,
    (SELECT COUNT(*) FROM analytics WHERE "revenue" = 0) AS revenue_zeros,
    (SELECT COUNT(*) FROM analytics WHERE "unitprice" = 0) AS unitprice_zeros,
    (SELECT COUNT(*) FROM analytics WHERE "newunitprice" = 0) AS newunitprice_zeros,
    (SELECT COUNT(*) FROM analytics WHERE "unitssold" = 0) AS unitssold_zeros,
    (SELECT COUNT(*) FROM analytics WHERE "pageviews" = 0) AS pageviews_zeros,
    (SELECT COUNT(*) FROM analytics WHERE "timeonsite" = 0) AS timeonsite_zeros,
    (SELECT COUNT(*) FROM analytics WHERE "bounces" = 0) AS bounces_zeros,
    (SELECT COUNT(*) FROM analytics WHERE "channelgrouping" = '0') AS channelgrouping_zeros,
    (SELECT COUNT(*) FROM analytics WHERE "socialengagementtype" = '0') AS socialengagementtype_zeros,
    (SELECT COUNT(*) FROM analytics WHERE "fullvisitoridtemp" = '0') AS fullvisitoridtemp_zeros;


SELECT 
    (SELECT COUNT(*) FROM products WHERE "stocklevel" = 0) AS stocklevel_zeros,
    (SELECT COUNT(*) FROM products WHERE "sentimentscore" = 0) AS sentimentscore_zeros,
    (SELECT COUNT(*) FROM products WHERE "sentimentmagnitude" = 0) AS sentimentmagnitude_zeros,
    (SELECT COUNT(*) FROM products WHERE "orderedquantity" = 0) AS orderedquantity_zeros,
    (SELECT COUNT(*) FROM products WHERE "restockingleadtime" = 0) AS restockingleadtime_zeros,
    (SELECT COUNT(*) FROM products WHERE "name" = '0') AS name_zeros,
    (SELECT COUNT(*) FROM products WHERE "productsku" = '0') AS productsku_zeros;


SELECT 
    (SELECT COUNT(*) FROM salesbysku WHERE "totalordered" = 0) AS totalordered_zeros,
    (SELECT COUNT(*) FROM salesbysku WHERE "productsku" = '0') AS productsku_zeros;

SELECT 
    (SELECT COUNT(*) FROM sales_report WHERE "ratio" = 0) AS ratio_zeros,
    (SELECT COUNT(*) FROM sales_report WHERE "totalordered" = 0) AS totalordered_zeros,
    (SELECT COUNT(*) FROM sales_report WHERE "sentimentscore" = 0) AS sentimentscore_zeros,
    (SELECT COUNT(*) FROM sales_report WHERE "sentimentmagnitude" = 0) AS sentimentmagnitude_zeros,
    (SELECT COUNT(*) FROM sales_report WHERE "stocklevel" = 0) AS stocklevel_zeros,
    (SELECT COUNT(*) FROM sales_report WHERE "restockingleadtime" = 0) AS restockingleadtime_zeros,
    (SELECT COUNT(*) FROM sales_report WHERE "name" = '0') AS name_zeros,
    (SELECT COUNT(*) FROM sales_report WHERE "productsku" = '0') AS productsku_zeros;


## Checking duplicates and if any selecting distinct


SELECT productsku, COUNT(*) AS duplicate_count
FROM sales_report
GROUP BY productsku
HAVING COUNT(*) > 1;

SELECT productsku, COUNT(*) AS duplicate_count
FROM sales_by_sku
GROUP BY productsku
HAVING COUNT(*) > 1;

SELECT productsku, COUNT(*) AS duplicate_count
FROM products
GROUP BY productsku
HAVING COUNT(*) > 1;

SELECT fullvisitorid, COUNT(*) AS duplicate_count
FROM analytics
GROUP BY fullvisitorid
HAVING COUNT(*) > 1;

SELECT fullvisitorid, COUNT(*) AS duplicate_count
FROM all_sessions
GROUP BY fullvisitorid
HAVING COUNT(*) > 1;


SELECT DISTINCT fullvisitorid FROM all_sessions

SELECT DISTINCT fullvisitorid FROM analytics

SELECT DISTINCT productsku FROM products

SELECT DISTINCT productsku FROM sales_by_sku

SELECT DISTINCT productsku FROM sales_report