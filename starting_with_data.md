Q11. Display the type of products ordered by customers as per product names and category.

SELECT
    visitid,all_sessions.productsku,v2productname AS prodname,v2productcategory AS prodcategory
FROM     all_sessions
JOIN     products ON all_sessions.productsku = products.productsku
GROUP BY visitid, all_sessions.productsku, prodname, prodcategory
ORDER BY visitid
LIMIT 100;

Ans:
![Q11](https://github.com/fursuf1/SQL-Project-LHL/blob/88525671a2f3a511590182424c76937f41e5c6b7/Q11.jpg)


Q22: Checking the products which includes the names for women?


SELECT DISTINCT v2productname 
FROM all_sessions WHERE v2productname ILIKE '%women%'

Ans:
![Q22](https://github.com/fursuf1/SQL-Project-LHL/blob/88525671a2f3a511590182424c76937f41e5c6b7/Q22.jpg)

Q33. Which products stock from the last?


SELECT  orderedquantity, sk.productsku, totalordered, name, stocklevel
FROM  	products pd
JOIN    salesbysku sk
ON	pd.productsku = sk.productsku
WHERE   totalordered > stocklevel
ORDER BY sk.totalordered DESC 

Ans:
![Q33](https://github.com/fursuf1/SQL-Project-LHL/blob/88525671a2f3a511590182424c76937f41e5c6b7/Q33.jpg)


Q44. Checking time spent on the website by ascending.


SELECT *
FROM analytics
WHERE unitssold > 1
ORDER BY timeonsite ASC NULLS LAST;

Ans:
![Q44](https://github.com/fursuf1/SQL-Project-LHL/blob/88525671a2f3a511590182424c76937f41e5c6b7/Q44.jpg)


Q55: What are the months with the highest revenue?"?

SELECT EXTRACT(MONTH FROM TO_DATE(CAST("date" AS VARCHAR), 'YYYYMMDD')) AS month, SUM(revenue) AS revenue
FROM analytics
GROUP BY EXTRACT(MONTH FROM TO_DATE(CAST("date" AS VARCHAR), 'YYYYMMDD'));

Ans:
![Q55](https://github.com/fursuf1/SQL-Project-LHL/blob/88525671a2f3a511590182424c76937f41e5c6b7/Q55.jpg)


Q66.Checking ordered quantities as per stock levels.


SELECT restockingleadtime, stocklevel, orderedquantity, name
FROM public.products
WHERE orderedquantity != 0 AND stocklevel != 0
ORDER BY orderedquantity DESC;

Ans:
![Q66](https://github.com/fursuf1/SQL-Project-LHL/blob/88525671a2f3a511590182424c76937f41e5c6b7/Q66.jpg)
