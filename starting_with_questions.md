Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

CREATE OR REPLACE VIEW highest_transaction_revenues_view AS
WITH max_revenues AS (
    SELECT country, city, MAX(totaltransactionrevenue) AS highest_transaction_revenues
    FROM all_sessions 
    WHERE totaltransactionrevenue IS NOT NULL AND city != '0'
    GROUP BY country, city)
SELECT al.country, al.city, mr.highest_transaction_revenues
FROM all_sessions AS al
JOIN max_revenues AS mr
ON    al.country = mr.country AND al.city = mr.city
ORDER BY mr.highest_transaction_revenues DESC, al.country, al.city;

SELECT DISTINCT *
FROM highest_transaction_revenues_view
ORDER BY highest_transaction_revenues DESC;


Answer:
![IQ1](https://github.com/fursuf1/SQL-Project-LHL/blob/88525671a2f3a511590182424c76937f41e5c6b7/Q1A.jpg)




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

CREATE OR REPLACE VIEW top_selling_products_avg_view AS
SELECT al.country, al.city, ROUND(AVG(pr.orderedquantity), 5) AS avg_ordered_quantity
FROM all_sessions AS al
JOIN products AS pr 
ON al.productsku = pr.productsku
WHERE al.city <> '(not set)' AND al.city <> '0'
GROUP BY al.country, al.city;


SELECT * FROM top_selling_products_avg_view
ORDER BY avg_ordered_quantity DESC;



Answer:
![Q2](https://github.com/fursuf1/SQL-Project-LHL/blob/88525671a2f3a511590182424c76937f41e5c6b7/Q2A.jpg)





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

CREATE OR REPLACE VIEW top_selling_products_by_city_view AS
SELECT  country, city, v2productname, v2productcategory
FROM all_sessions 
WHERE city <> '(not set)' AND city <> '0'
GROUP BY v2productcategory, country, city, v2productname

SELECT * FROM top_selling_products_by_city_view;


Answer:
![Q3](https://github.com/fursuf1/SQL-Project-LHL/blob/88525671a2f3a511590182424c76937f41e5c6b7/Q3A.jpg)




**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

CREATE OR REPLACE VIEW top_selling_products_view AS
WITH top_products AS (
    SELECT  al.v2productname, al.country, al.city,
           SUM(pr.orderedquantity) AS total_ordered_quantity,
           RANK() OVER (PARTITION BY al.country, al.city ORDER BY SUM(pr.orderedquantity) DESC) AS rank
    FROM all_sessions AS al
    JOIN products AS pr ON al.productsku = pr.productsku
    WHERE al.city <> '0'   GROUP BY al.v2productname, al.country, al.city)
SELECT v2productname, country, city, total_ordered_quantity
FROM top_products
ORDER BY country, city, total_ordered_quantity DESC;


SELECT * 
FROM top_selling_products_view
ORDER BY total_ordered_quantity DESC;



Answer:
![Q4](https://github.com/fursuf1/SQL-Project-LHL/blob/88525671a2f3a511590182424c76937f41e5c6b7/Q4A.jpg)




**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

WITH regional_revenue_cte AS (
    SELECT
        al.country, al.city,
        SUM((aly.unitprice * aly.unitssold) / 1000000) AS total_regional_revenue, al.fullvisitorid
    FROM all_sessions al
    LEFT JOIN analytics aly ON al.fullvisitorid = aly.fullvisitoridtemp
    GROUP BY
        al.country, al.city, al.productsku, al.fullvisitorid )
SELECT country,city,  total_regional_revenue, fullvisitorid
FROM regional_revenue_cte
ORDER BY total_regional_revenue DESC NULLS LAST;


Answer:
![Q5](https://github.com/fursuf1/SQL-Project-LHL/blob/88525671a2f3a511590182424c76937f41e5c6b7/Q5A.jpg)







