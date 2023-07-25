# Provided Questions

## Question 1: Which cities and countries have the highest level of transaction revenues on the site?

### **Answer:** In terms of countries the United States makes an overwhelming majority of the revenue on the website, followed up by Hong Kong, Canada, Japan and Mexico. City-wise Mountain View, Chicago, Sunnyvale, New York and Salem make up the top five. (See full list below)

### **SQL Queries:**
```SQL
SELECT sessions.country, ROUND(CAST(SUM(sessions.productprice * distinct_analytics.units_sold) AS numeric), 2) AS revenue
FROM all_sessions as sessions
JOIN (SELECT DISTINCT * FROM analytics WHERE units_sold > 0) AS distinct_analytics
ON sessions.fullvisitorid = distinct_analytics.fullvisitorid
GROUP BY sessions.country
HAVING SUM(sessions.productprice * distinct_analytics.units_sold) IS NOT NULL
ORDER BY revenue DESC;
```

```SQL
SELECT sessions.city, ROUND(CAST(SUM(sessions.productprice * distinct_analytics.units_sold) AS numeric), 2) as revenue
FROM all_sessions as sessions
JOIN (SELECT DISTINCT * FROM analytics WHERE units_sold > 0) AS distinct_analytics 
ON sessions.fullvisitorid = distinct_analytics.fullvisitorid
WHERE sessions.city IS NOT null
GROUP BY sessions.city
HAVING SUM(sessions.productprice * distinct_analytics.units_sold) IS NOT NULL
ORDER BY revenue DESC;
```

### **Output (Countries):**
|country|revenue|
|:----|:----|
|United States|955831.63|
|Hong Kong|2117.04|
|Canada|1448.79|
|Japan|803.22|
|Mexico|743.99|
|United Kingdom|335.91|
|Taiwan|327.88|
|Germany|175.82|
|Singapore|158.58|
|India|157.94|
|Indonesia|143.97|
|Egypt|139.93|
|Thailand|126.97|
|Maldives|119.99|
|Spain|119.00|
|France|109.44|
|Colombia|101.46|
|Ireland|99.99|
|Sri Lanka|99.96|
|Netherlands|77.96|
|Switzerland|76.95|
|Finland|71.97|
|Bulgaria|62.97|
|Australia|61.95|
|Sweden|47.98|
|Dominican Republic|37.98|
|Portugal|34.99|
|Uruguay|29.99|
|Russia|26.97|
|Denmark|24.99|
|Belgium|24.99|
|Israel|20.99|
|Vietnam|19.99|
|South Korea|19.97|
|Panama|18.99|
|Norway|16.99|
|Guatemala|10.99|
|Austria|6.99|
|Belarus|5.50|
|Chile|4.99|
|Czechia|0.00|
|Romania|0.00|

### **Output (Cities):**
|city|revenue|
|:----|:----|
|Mountain View|51356.89|
|Chicago|28993.58|
|Sunnyvale|15026.42|
|New York|13613.99|
|Salem|11121.20|
|San Bruno|8355.77|
|San Francisco|6569.59|
|Charlotte|5889.79|
|Los Angeles|5558.85|
|Palo Alto|3976.99|
|Seattle|3496.46|
|Austin|3178.73|
|Jersey City|2783.42|
|San Jose|2762.33|
|Milpitas|1925.82|
|Nashville|1639.00|
|Kirkland|1246.32|
|Atlanta|1187.45|
|Paris|1067.85|
|Toronto|865.57|
|Hong Kong|600.05|
|Ann Arbor|477.21|
|Yokohama|419.94|
|South San Francisco|419.94|
|Fremont|357.00|
|London|320.92|
|Cambridge|189.49|
|Santa Clara|188.96|
|Bangkok|126.97|
|Minato|120.93|
|Pittsburgh|119.92|
|Madrid|119.00|
|Santa Monica|104.93|
|Dublin|99.99|
|Bogota|99.96|
|Cupertino|79.00|
|Irvine|75.96|
|Helsinki|71.97|
|Denver|65.94|
|Hyderabad|62.99|
|Zurich|55.96|
|Hamburg|49.90|
|Osaka|33.98|
|Montevideo|29.99|
|Munich|27.98|
|Courbevoie|24.99|
|Sydney|20.99|
|Tel Aviv-Yafo|20.99|
|Phoenix|18.99|
|Seoul|17.98|
|Singapore|17.59|
|Houston|17.50|
|Kitchener|15.19|
|Stockholm|12.99|
|Washington|11.99|
|Dallas|6.99|
|Zhongli District|5.99|
|Santiago|4.99|
|Jakarta|3.99|
|Berlin|2.99|
|Detroit|2.99|
|San Diego|2.99|


## Question 2: What is the average number of products ordered from visitors in each city and country?

### **Answer:** *(See output below for full list)* It seems that only Czechia, the United States, Canada, Mexico Bulgaria, Hong Kong, Germany, Japan and India are the only countries to have ordered more that one product during a single transaction. San Bruno, California has an significantly higher average order quantity than any other city in the list (~53 vs 19 in second place).

### **SQL Queries:**
```SQL
SELECT sessions.country, TRUNC(AVG(distinct_analytics.units_sold), 2) as avg_unitssold
FROM all_sessions as sessions
JOIN (SELECT DISTINCT * FROM analytics) as distinct_analytics
ON sessions.fullvisitorid = distinct_analytics.fullvisitorid
WHERE sessions.country IS NOT NULL AND units_sold > 0
GROUP BY sessions.country
HAVING TRUNC(AVG(distinct_analytics.units_sold), 2) IS NOT NULL
ORDER BY avg_unitssold DESC;
```

```SQL
SELECT sessions.city, TRUNC(AVG(distinct_analytics.units_sold), 2) as avg_unitssold
FROM all_sessions as sessions
JOIN (SELECT DISTINCT * FROM analytics) as distinct_analytics
ON sessions.fullvisitorid = distinct_analytics.fullvisitorid
WHERE sessions.city IS NOT NULL AND units_sold > 0
GROUP BY sessions.city
HAVING TRUNC(AVG(distinct_analytics.units_sold), 2) IS NOT NULL
ORDER BY avg_unitssold DESC;
```

### **Output (Countries):**
|country|avg_unitssold|
|:----|:----|
|Czechia|24.50|
|United States|22.33|
|Canada|1.80|
|Mexico|1.75|
|Bulgaria|1.50|
|Hong Kong|1.30|
|Germany|1.28|
|Japan|1.25|
|India|1.12|
|France|1.00|
|Maldives|1.00|
|Israel|1.00|
|Belarus|1.00|
|Netherlands|1.00|
|Australia|1.00|
|Spain|1.00|
|Belgium|1.00|
|Taiwan|1.00|
|Thailand|1.00|
|Uruguay|1.00|
|United Kingdom|1.00|
|South Korea|1.00|
|Egypt|1.00|
|Chile|1.00|
|Vietnam|1.00|
|Denmark|1.00|
|Switzerland|1.00|
|Russia|1.00|
|Norway|1.00|
|Romania|1.00|
|Indonesia|1.00|
|Guatemala|1.00|
|Panama|1.00|
|Austria|1.00|
|Sweden|1.00|
|Dominican Republic|1.00|
|Ireland|1.00|
|Singapore|1.00|
|Sri Lanka|1.00|
|Finland|1.00|
|Portugal|1.00|
|Colombia|1.00|


### **Output (Cities):**
|city|avg_unitssold|
|:----|:----|
|San Bruno|53.26|
|Mountain View|19.95|
|San Jose|11.84|
|New York|8.22|
|Salem|7.69|
|Chicago|6.77|
|Nashville|5.50|
|Jersey City|5.27|
|Charlotte|5.13|
|Sunnyvale|5.12|
|Seattle|4.05|
|Pittsburgh|4.00|
|Austin|3.90|
|Atlanta|3.50|
|Los Angeles|3.50|
|Paris|3.00|
|Cambridge|2.83|
|Toronto|2.52|
|Houston|2.50|
|Denver|2.00|
|Kirkland|1.83|
|Hamburg|1.66|
|Milpitas|1.61|
|Hyderabad|1.50|
|San Francisco|1.40|
|Santa Monica|1.40|
|Hong Kong|1.33|
|Palo Alto|1.14|
|Dublin|1.00|
|Sydney|1.00|
|London|1.00|
|Santa Clara|1.00|
|Jakarta|1.00|
|Dallas|1.00|
|Berlin|1.00|
|Helsinki|1.00|
|Kitchener|1.00|
|Ann Arbor|1.00|
|Tel Aviv-Yafo|1.00|
|Phoenix|1.00|
|Santiago|1.00|
|Detroit|1.00|
|Minato|1.00|
|Montevideo|1.00|
|Zhongli District|1.00|
|Courbevoie|1.00|
|Irvine|1.00|
|Bangkok|1.00|
|Stockholm|1.00|
|Munich|1.00|
|Zurich|1.00|
|Cupertino|1.00|
|Yokohama|1.00|
|Fremont|1.00|
|Madrid|1.00|
|Ahmedabad|1.00|
|Washington|1.00|
|Singapore|1.00|
|South San Francisco|1.00|
|Seoul|1.00|
|San Diego|1.00|
|Osaka|1.00|
|Bogota|1.00|


## Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?

### **Answer:** It appears that apparel is the most popular category across the majority of countries. Overall it does appear that countries in Asia purchase less apparel and more electronics & other goods (a notable example of this is Hong Kong). When broken down by city, the results seem more spread out (most apparel purchasers did not set a city)

### SQL Queries:
```SQL
-- Most to least popular category per country
SELECT country, split_category2, SUM(units_sold) as total_units_sold
FROM all_sessions
JOIN (SELECT DISTINCT * FROM analytics WHERE units_sold > 0) as distinct_analytics
ON all_sessions.fullvisitorid = distinct_analytics.fullvisitorid
WHERE split_category2 IS NOT NULL --AND country = 'Canada'
GROUP BY country, split_category2
ORDER BY country, total_units_sold DESC;

-- Top apparel categories (Men's seems to be the most popular)
SELECT country, split_category3, SUM(units_sold) as total_units_sold
FROM all_sessions
JOIN (SELECT DISTINCT * FROM analytics WHERE units_sold > 0) as distinct_analytics
ON all_sessions.fullvisitorid = distinct_analytics.fullvisitorid
WHERE split_category2 = 'Apparel'
GROUP BY country, split_category3
ORDER BY country, total_units_sold DESC;
```

```SQL
SELECT city, split_category2, SUM(units_sold) as total_units_sold
FROM all_sessions
JOIN (SELECT DISTINCT * FROM analytics WHERE units_sold > 0) as distinct_analytics
ON all_sessions.fullvisitorid = distinct_analytics.fullvisitorid
WHERE split_category2 IS NOT NULL AND city IS NOT NULL
GROUP BY city, split_category2
ORDER BY city, total_units_sold DESC;
```

### Output:
|country|split_category2|total_units_sold|
|:----|:----|:----|
|Australia|Accessories|2|
|Australia|Shop by Brand|2|
|Australia|Apparel|1|
|Austria|Accessories|1|
|Belarus|Shop by Brand|1|
|Belgium|Drinkware|1|
|Bulgaria|Apparel|3|
|Canada|Apparel|35|
|Canada|Shop by Brand|29|
|Canada|Accessories|6|
|Canada|Bags|1|
|Canada|Drinkware|1|
|Canada|Electronics|1|
|Chile|Bags|1|
|Colombia|Apparel|4|
|Colombia|Office|1|
|Denmark|Drinkware|1|
|Dominican Republic|Apparel|2|
|Egypt|Shop by Brand|7|
|France|Accessories|2|
|France|Shop by Brand|2|
|France|Apparel|1|
|Germany|Drinkware|10|
|Germany|Apparel|7|
|Germany|Office|1|
|Guatemala|Apparel|1|
|Hong Kong|Electronics|86|
|Hong Kong|Shop by Brand|8|
|Hong Kong|Nest|6|
|Hong Kong|Apparel|2|
|India|Apparel|4|
|India|Accessories|2|
|India|Shop by Brand|1|
|India|Electronics|1|
|Indonesia|Bags|2|
|Indonesia|Shop by Brand|1|
|Ireland|Bags|1|
|Israel|Apparel|1|
|Japan|Shop by Brand|11|
|Japan|Apparel|8|
|Japan|Accessories|8|
|Japan|Office|2|
|Maldives|Apparel|1|
|Mexico|Nest|6|
|Mexico|Apparel|1|
|Netherlands|Apparel|3|
|Netherlands|Shop by Brand|1|
|Norway|Apparel|1|
|Panama|Apparel|1|
|Portugal|Bags|1|
|Russia|Accessories|3|
|Singapore|Nest|1|
|South Korea|Apparel|2|
|South Korea|Electronics|1|
|Sri Lanka|Apparel|4|
|Sweden|Drinkware|1|
|Sweden|Electronics|1|
|Switzerland|Apparel|3|
|Switzerland|Electronics|1|
|Switzerland|Accessories|1|
|Taiwan|Apparel|7|
|Taiwan|Shop by Brand|2|
|Taiwan|Accessories|1|
|Thailand|Drinkware|2|
|Thailand|Bags|1|
|United Kingdom|Bags|3|
|United Kingdom|Office|1|
|United Kingdom|Shop by Brand|1|
|United States|Accessories|22001|
|United States|Apparel|18895|
|United States|Office|6279|
|United States|Shop by Brand|1823|
|United States|Electronics|1120|
|United States|Nest|1081|
|United States|Bags|572|
|United States|Drinkware|485|
|United States|Spring Sale!|201|
|United States|Lifestyle|32|
|United States|Clearance Sale|15|
|United States|Gift Cards|1|
|Uruguay|Apparel|1|
|Vietnam|Shop by Brand|1|

### Output (Cities): *truncated*
|city|split_category2|total_units_sold|
|:----|:----|:----|
|Ann Arbor|Electronics|21|
|Ann Arbor|Apparel|17|
|Ann Arbor|Accessories|12|
|Atlanta|Drinkware|25|
|Atlanta|Apparel|5|
|Atlanta|Shop by Brand|4|
|Atlanta|Bags|1|
|Austin|Office|105|
|Austin|Drinkware|12|
|Austin|Bags|7|
|Austin|Electronics|2|
|Austin|Nest|2|
|Austin|Apparel|1|
|Bangkok|Drinkware|2|
|Bangkok|Bags|1|
|Berlin|Office|1|
|Bogota|Apparel|4|
|Cambridge|Electronics|47|
|Cambridge|Apparel|4|
|Charlotte|Bags|421|
|Chicago|Shop by Brand|250|
|Chicago|Apparel|210|
|Chicago|Nest|18|
|Chicago|Electronics|10|
|Chicago|Office|7|
|Chicago|Bags|2|
|Courbevoie|Shop by Brand|1|
|Cupertino|Nest|1|
|Dallas|Shop by Brand|1|
|Denver|Apparel|6|
|Dublin|Bags|1|
|Fremont|Nest|3|
|Hamburg|Drinkware|10|
|Hong Kong|Electronics|86|
|Hong Kong|Shop by Brand|8|
|Hong Kong|Apparel|1|
|Hyderabad|Accessories|2|
|Hyderabad|Electronics|1|
|Irvine|Apparel|4|
|Jakarta|Shop by Brand|1|
|Jersey City|Bags|58|
|Kirkland|Apparel|56|
|Kirkland|Office|9|
|Kirkland|Drinkware|3|
|Kitchener|Apparel|1|
|London|Bags|3|
|London|Office|1|
|Los Angeles|Electronics|107|
|Los Angeles|Nest|25|
|Los Angeles|Apparel|8|
|Milpitas|Apparel|18|
|Milpitas|Nest|16|
|Minato|Apparel|6|
|Minato|Office|1|
|Montevideo|Apparel|1|
|Mountain View|Accessories|5368|
|Mountain View|Spring Sale!|201|
|Mountain View|Nest|178|
|Mountain View|Electronics|125|
|Mountain View|Apparel|57|
|Mountain View|Lifestyle|30|
|Mountain View|Bags|20|
|Mountain View|Drinkware|14|
|Mountain View|Shop by Brand|11|
|Mountain View|Clearance Sale|6|
|Mountain View|Office|5|
|Munich|Apparel|2|
|New York|Office|66|
|New York|Electronics|66|
|New York|Apparel|47|
|New York|Shop by Brand|22|
|New York|Nest|15|
|New York|Accessories|7|
|New York|Bags|5|
|Osaka|Apparel|2|
|Palo Alto|Nest|20|
|Palo Alto|Apparel|1|
|Paris|Apparel|14|
|Paris|Shop by Brand|1|
|Phoenix|Apparel|1|
|Pittsburgh|Office|8|
|Salem|Apparel|200|
|San Bruno|Shop by Brand|1202|
|San Bruno|Electronics|15|
|San Bruno|Bags|6|
|San Diego|Accessories|1|
|San Francisco|Nest|25|
|San Francisco|Apparel|25|
|San Francisco|Shop by Brand|4|
|San Francisco|Accessories|4|
|San Francisco|Electronics|2|
|San Francisco|Drinkware|1|
|San Francisco|Office|1|

## Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?

### Answer: One obvious data point is that the US continues to dominate this companies purchase history with 173 Twill Caps while most other products have less than 10 total purchases. Another notable detail is that India has a very wide spread in products, each only having a single purchase. It appears that the Twill Cap purchases did not have a city associated with them, this means the second query will show slightly different results. I was not able to see any obvious pattern in the City breakdown.
*See output below for full list*. 

### SQL Queries: 
```SQL
-- Caveat: Will show a country multiple times if there are multiple top-selling products 
SELECT country, v2ProductName, purchase_count
FROM (
    SELECT country, v2ProductName, COUNT(*) as purchase_count,
    RANK() OVER (PARTITION BY country ORDER BY COUNT(*) DESC) as rank
    FROM all_sessions
    JOIN (SELECT DISTINCT * FROM analytics WHERE units_sold > 0) as distinct_analytics
    ON all_sessions.fullvisitorid = distinct_analytics.fullvisitorid
    GROUP BY country, v2ProductName
) products_ranked
WHERE rank = 1
ORDER BY purchase_count DESC
;
```

```SQL
-- Caveat: Will show a city multiple times if there are multiple top-selling products 
SELECT city, v2ProductName, purchase_count
FROM (
    SELECT city, v2ProductName, COUNT(*) as purchase_count,
    RANK() OVER (PARTITION BY city ORDER BY COUNT(*) DESC) as rank
    FROM all_sessions
    JOIN (SELECT DISTINCT * FROM analytics WHERE units_sold > 0) as distinct_analytics
    ON all_sessions.fullvisitorid = distinct_analytics.fullvisitorid
    GROUP BY city, v2ProductName
) products_ranked
WHERE rank = 1 AND city IS NOT NULL
ORDER BY purchase_count DESC
;
```
### **Output (Countries):**
|country|v2productname|purchase_count|
|:----|:----|:----|
|United States|Twill Cap|173|
|Hong Kong|Device Stand|62|
|Canada|Men's Engineer Short Sleeve Tee Charcoal|15|
|Egypt|RFID Journal|7|
|Japan|5-Panel Snapback Cap|6|
|Germany|22 oz Infuser Bottle|6|
|Japan|Rucksack|6|
|Japan|Lunch Bag|6|
|Sri Lanka|Men's 3/4 Sleeve Henley|4|
|Colombia|Men's 3/4 Sleeve Henley|4|
|Czechia|Snapback Hat Black|4|
|France|Wool Heather Cap Heather/Black|3|
|Finland|Onesie Gold|3|
|Taiwan|5-Panel Snapback Cap|3|
|Russia|Luggage Tag|3|
|Netherlands|Men's Vintage Tank|3|
|Mexico|Nest® Cam Indoor Security Camera - USA|3|
|United Kingdom|Rucksack|2|
|Bulgaria|Men's Vintage Tank|2|
|Australia|Sticker Sheet Ultra Removable|2|
|Dominican Republic|Men's Vintage Badge Tee Black|2|
|United Kingdom|Hard Cover Journal|2|
|United Kingdom|Sticker Sheet Ultra Removable|2|
|South Korea|Waze Dress Socks|2|
|Indonesia|Rucksack|2|
|United Kingdom|Men's Lightweight Microfleece Jacket Black|2|
|Sweden|Women's Short Sleeve Hero Tee Sky Blue|2|
|Switzerland|Men's 3/4 Sleeve Henley|2|
|Thailand|Foam Can and Bottle Cooler|1|
|Uruguay|Vintage Henley Grey/Black|1|
|Vietnam|RFID Journal|1|
|Austria|Car Clip Phone Holder|1|
|Belarus|Stylus Pen w/ LED Light|1|
|Belgium|17 oz Double Wall Stainless Steel Insulated Bottle|1|
|Chile|Sport Bag|1|
|Denmark|26 oz Double Wall Insulated Bottle|1|
|Guatemala|Twill Cap|1|
|India|Men's Vintage Henley|1|
|India|Bongo Cupholder Bluetooth Speaker|1|
|India|Custom Decals|1|
|India|Stretch Fit Hat Black|1|
|India|Men's Vintage Tank|1|
|India|Men's Engineer Short Sleeve Tee Charcoal|1|
|India|Keyboard DOT Sticker|1|
|India|Youth Short Sleeve T-shirt Royal Blue|1|
|Ireland|Laptop Backpack|1|
|Israel|Men's Vintage Tank|1|
|Maldives|Men's Performance Full Zip Jacket Black|1|
|Norway|Women's Short Sleeve Hero Tee Black|1|
|Panama|Men's Short Sleeve Hero Tee Light Blue|1|
|Portugal|Large Zipper Top Tote Bag|1|
|Romania|8 pc Sticker Sheet|1|
|Singapore|Nest® Protect Smoke + CO White Battery Alarm-USA|1|
|Singapore|Stretch Fit Hat Black|1|
|Singapore|Men's Short Sleeve Performance Badge Tee Navy|1|
|Spain|Nest® Protect Smoke + CO White Wired Alarm-USA|1|
|Thailand|26 oz Double Wall Insulated Bottle|1|
|Thailand|Waterproof Backpack|1|

### Output (Cities):
|city|v2productname|purchase_count|
|:----|:----|:----|
|Charlotte|Lunch Bag|82|
|Hong Kong|Device Stand|62|
|Sunnyvale|SPF-15 Slim & Slender Lip Balm|51|
|Chicago|Custom Decals|32|
|Mountain View|Nest® Protect Smoke + CO White Battery Alarm-USA|29|
|New York|Collapsible Shopping Bag|29|
|Salem|Men's  Zip Hoodie|23|
|Ann Arbor|Recycled Mouse Pad|18|
|Los Angeles|Basecamp Explorer Powerbank Flashlight|18|
|Milpitas|Women's Short Sleeve Hero Dark Grey|18|
|Seattle|Men's  Zip Hoodie|17|
|Cambridge|Galaxy Screen Cleaning Cloth|14|
|Kirkland|Women's Long Sleeve Tee Lavender|12|
|San Bruno|22 oz Infuser Bottle|12|
|Austin|RFID Journal|11|
|Jersey City|Laptop Tech Backpack|11|
|Palo Alto|Nest® Cam Outdoor Security Camera - USA|9|
|Toronto|Men's 3/4 Sleeve Raglan Henley Grey|8|
|San Francisco|Windup Android|8|
|San Francisco|Women's Scoop Neck Tee White|8|
|Yokohama|Rucksack|6|
|South San Francisco|Rucksack|6|
|San Jose|Lunch Bag|6|
|Hamburg|22 oz Infuser Bottle|6|
|Minato|5-Panel Snapback Cap|6|
|Santa Monica|Slim Utility Travel Bag|5|
|Irvine|Men's Vintage Badge Tee Sage|4|
|Paris|Women's Quilted Insulated Vest Black|4|
|Atlanta|Onesie Green|4|
|Bogota|Men's 3/4 Sleeve Henley|4|
|Santa Clara|Clip-on Compact Charger|3|
|Denver|Twill Cap|3|
|Fremont|Nest® Protect Smoke + CO White Battery Alarm-USA|3|
|Helsinki|Onesie Gold|3|
|Pittsburgh|Hard Cover Journal|2|
|London|Sticker Sheet Ultra Removable|2|
|Houston|Sunglasses|2|
|Osaka|Infant Short Sleeve Tee Red|2|
|Seoul|Waze Dress Socks|2|
|London|Men's Lightweight Microfleece Jacket Black|2|
|London|Rucksack|2|
|Nashville|Nest® Learning Thermostat 3rd Gen-USA - Stainless Steel|2|
|Zurich|Men's 3/4 Sleeve Henley|2|
|San Diego|Doodle Decal|1|
|Dublin|Laptop Backpack|1|
|Madrid|Nest® Protect Smoke + CO White Wired Alarm-USA|1|
|Dallas|Leatherette Notebook Combo|1|
|Ahmedabad|Youth Short Sleeve T-shirt Royal Blue|1|
|Cupertino|Nest® Protect Smoke + CO White Wired Alarm-USA|1|
|Santiago|Sport Bag|1|
|Courbevoie|Wool Heather Cap Heather/Black|1|
|Singapore|Men's Short Sleeve Performance Badge Tee Navy|1|
|Berlin|Doodle Decal|1|
|Stockholm|Rise 14 oz Mug|1|
|Bangkok|Foam Can and Bottle Cooler|1|
|Sydney|Men's Vintage Tank|1|
|Sydney|Women's Short Sleeve Hero Tee Sky Blue|1|
|Tel Aviv-Yafo|Men's Vintage Tank|1|
|Bangkok|Waterproof Backpack|1|
|Washington|Bib White|1|
|Bangkok|26 oz Double Wall Insulated Bottle|1|
|Zhongli District|Spiral Notebook and Pen Set|1|
|Detroit|22 oz Water Bottle|1|
|Montevideo|Vintage Henley Grey/Black|1|
|Munich|Men's 100% Cotton Short Sleeve Hero Tee Red|1|
|Munich|Twill Cap|1|
|Kitchener|5-Panel Cap|1|
|Jakarta|Windup Android|1|
|Hyderabad|Keyboard DOT Sticker|1|
|Phoenix|Men's Short Sleeve Hero Tee Heather|1|
|Hyderabad|Bongo Cupholder Bluetooth Speaker|1|


## Question 5: Can we summarize the impact of revenue generated from each city/country?

### **Answer:** It seems that the majority of this companies revenue is generated in the summer months between May-July, with almost no international revenue (assuming based in N.A.) coming in outside of that period.

### SQL Queries:
```SQL
--- Calculates the most profitable month for each country
SELECT 	country,
		TO_CHAR(all_sessions.date, 'Month'),
		EXTRACT(MONTH FROM all_sessions.date) AS month, 
		ROUND(CAST(SUM(all_sessions.productprice * distinct_analytics.units_sold) AS numeric), 2) AS revenue
FROM all_sessions
JOIN (SELECT DISTINCT * FROM analytics WHERE units_sold > 0) as distinct_analytics 
ON all_sessions.fullvisitorid = distinct_analytics.fullvisitorid
GROUP BY country, TO_CHAR(all_sessions.date, 'Month'), EXTRACT(MONTH FROM all_sessions.date)
ORDER BY country ASC, revenue DESC;
```

```SQL
--- Calculates the most profitable month for each city
--- It's worth noting there are very many missing city names (I chose not to remove them from the output)
SELECT 	country,
		city,
		TO_CHAR(all_sessions.date, 'Month') as monthtext,
		EXTRACT(MONTH FROM all_sessions.date) AS month, 
		ROUND(CAST(SUM(all_sessions.productprice * distinct_analytics.units_sold) AS numeric), 2) AS revenue
FROM all_sessions
JOIN (SELECT DISTINCT * FROM analytics WHERE units_sold > 0) as distinct_analytics 
ON all_sessions.fullvisitorid = distinct_analytics.fullvisitorid
GROUP BY country, city, TO_CHAR(all_sessions.date, 'Month'), EXTRACT(MONTH FROM all_sessions.date)
ORDER BY country, city ASC, revenue DESC;
```

### **Output: (Countries)**
|country|to_char|month|revenue|
|:----|:----|:----|:----|
|Australia|July     |7|34.98|
|Australia|May      |5|26.97|
|Austria|May      |5|6.99|
|Belarus|July     |7|5.50|
|Belgium|May      |5|24.99|
|Bulgaria|June     |6|62.97|
|Canada|June     |6|635.60|
|Canada|March    |3|479.76|
|Canada|May      |5|193.72|
|Canada|July     |7|124.52|
|Canada|August   |8|15.19|
|Chile|May      |5|4.99|
|Colombia|May      |5|99.96|
|Colombia|July     |7|1.50|
|Czechia|April    |4|0.00|
|Denmark|July     |7|24.99|
|Dominican Republic|July     |7|37.98|
|Egypt|July     |7|139.93|
|Finland|June     |6|71.97|
|France|May      |5|35.48|
|France|July     |7|24.99|
|France|February |2|24.99|
|France|April    |4|23.98|
|Germany|June     |6|94.95|
|Germany|May      |5|49.90|
|Germany|April    |4|16.99|
|Germany|July     |7|13.98|
|Guatemala|May      |5|10.99|
|Hong Kong|July     |7|1923.14|
|Hong Kong|May      |5|158.91|
|Hong Kong|June     |6|34.99|
|India|June     |6|111.96|
|India|July     |7|24.99|
|India|May      |5|20.99|
|Indonesia|May      |5|139.98|
|Indonesia|June     |6|3.99|
|Ireland|May      |5|99.99|
|Israel|May      |5|20.99|
|Japan|June     |6|419.94|
|Japan|May      |5|260.85|
|Japan|July     |7|115.44|
|Japan|August   |8|6.99|
|Maldives|July     |7|119.99|
|Mexico|July     |7|714.00|
|Mexico|June     |6|29.99|
|Netherlands|May      |5|77.96|
|Norway|January  |1|16.99|
|Panama|March    |3|18.99|
|Portugal|May      |5|34.99|
|Romania|June     |6|0.00|
|Russia|June     |6|26.97|
|Singapore|April    |4|119.00|
|Singapore|May      |5|21.99|
|Singapore|July     |7|17.59|
|South Korea|July     |7|19.97|
|Spain|May      |5|119.00|
|Sri Lanka|May      |5|99.96|
|Sweden|July     |7|34.99|
|Sweden|May      |5|12.99|
|Switzerland|May      |5|49.98|
|Switzerland|July     |7|22.98|
|Switzerland|June     |6|3.99|
|Taiwan|July     |7|283.92|
|Taiwan|May      |5|16.99|
|Taiwan|June     |6|13.99|
|Taiwan|April    |4|12.98|
|Thailand|May      |5|99.99|
|Thailand|July     |7|26.98|
|United Kingdom|May      |5|170.95|
|United Kingdom|July     |7|164.96|
|United States|June     |6|357634.47|
|United States|August   |8|226444.37|
|United States|May      |5|106327.84|
|United States|November |11|98629.91|
|United States|April    |4|64324.48|
|United States|July     |7|60681.94|
|United States|December |12|15956.89|
|United States|February |2|12462.30|
|United States|March    |3|5448.03|
|United States|January  |1|5360.94|
|United States|September|9|2154.68|
|United States|October  |10|405.78|
|Uruguay|May      |5|29.99|
|Vietnam|July     |7|19.99|


### **Output: (Cities)**
|country|city|monthtext|month|revenue|
|:----|:----|:----|:----|:----|
|Australia|Sydney|May      |5|20.99|
|Canada|Kitchener|August   |8|15.19|
|Canada|Toronto|March    |3|479.76|
|Canada|Toronto|June     |6|380.82|
|Canada|Toronto|May      |5|4.99|
|Chile|Santiago|May      |5|4.99|
|Colombia|Bogota|May      |5|99.96|
|Finland|Helsinki|June     |6|71.97|
|France|Courbevoie|July     |7|24.99|
|France|Paris|May      |5|17.99|
|Germany|Berlin|July     |7|2.99|
|Germany|Hamburg|May      |5|49.90|
|Germany|Munich|April    |4|16.99|
|Germany|Munich|July     |7|10.99|
|Hong Kong|Hong Kong|July     |7|429.14|
|Hong Kong|Hong Kong|May      |5|135.92|
|Hong Kong|Hong Kong|June     |6|34.99|
|India|Ahmedabad|June     |6|0.00|
|India|Hyderabad|June     |6|59.99|
|India|Hyderabad|July     |7|3.00|
|Indonesia|Jakarta|June     |6|3.99|
|Ireland|Dublin|May      |5|99.99|
|Israel|Tel Aviv-Yafo|May      |5|20.99|
|Japan|Minato|July     |7|113.94|
|Japan|Minato|August   |8|6.99|
|Japan|Osaka|May      |5|33.98|
|Japan|Yokohama|June     |6|419.94|
|Singapore|Singapore|July     |7|17.59|
|South Korea|Seoul|July     |7|17.98|
|Spain|Madrid|May      |5|119.00|
|Sweden|Stockholm|May      |5|12.99|
|Switzerland|Zurich|May      |5|49.98|
|Switzerland|Zurich|June     |6|3.99|
|Switzerland|Zurich|July     |7|1.99|
|Taiwan|Zhongli District|July     |7|5.99|
|Thailand|Bangkok|May      |5|99.99|
|Thailand|Bangkok|July     |7|26.98|
|United Kingdom|London|July     |7|164.96|
|United Kingdom|London|May      |5|155.96|
|United States|Ann Arbor|June     |6|374.15|
|United States|Ann Arbor|May      |5|61.47|
|United States|Ann Arbor|July     |7|41.59|
|United States|Atlanta|May      |5|954.71|
|United States|Atlanta|June     |6|155.98|
|United States|Atlanta|November |11|76.76|
|United States|Austin|July     |7|2136.94|
|United States|Austin|April    |4|559.93|
|United States|Austin|June     |6|315.98|
|United States|Austin|May      |5|165.88|
|United States|Cambridge|May      |5|189.49|
|United States|Charlotte|December |12|5889.79|
|United States|Chicago|July     |7|20851.87|
|United States|Chicago|January  |1|3499.94|
|United States|Chicago|June     |6|3496.91|
|United States|Chicago|May      |5|1138.89|
|United States|Chicago|August   |8|5.97|
|United States|Cupertino|July     |7|79.00|
|United States|Dallas|July     |7|6.99|
|United States|Denver|July     |7|65.94|
|United States|Detroit|June     |6|2.99|
|United States|Fremont|May      |5|357.00|
|United States|Houston|July     |7|17.50|
|United States|Irvine|June     |6|75.96|
|United States|Jersey City|December |12|2783.42|
|United States|Kirkland|February |2|815.66|
|United States|Kirkland|May      |5|322.83|
|United States|Kirkland|August   |8|71.95|
|United States|Kirkland|July     |7|35.88|
|United States|Los Angeles|June     |6|2975.00|
|United States|Los Angeles|May      |5|2459.93|
|United States|Los Angeles|March    |3|75.96|
|United States|Los Angeles|July     |7|47.96|
|United States|Milpitas|December |12|1584.00|
|United States|Milpitas|June     |6|341.82|
|United States|Mountain View|April    |4|18352.95|
|United States|Mountain View|May      |5|17267.59|
|United States|Mountain View|July     |7|6694.37|
|United States|Mountain View|June     |6|5482.30|
|United States|Mountain View|September|9|2045.90|
|United States|Mountain View|December |12|705.95|
|United States|Mountain View|January  |1|705.89|
|United States|Mountain View|February |2|101.94|
|United States|Nashville|December |12|1639.00|
|United States|New York|April    |4|5683.61|
|United States|New York|June     |6|2904.44|
|United States|New York|July     |7|2364.87|
|United States|New York|May      |5|1343.59|
|United States|New York|December |12|384.95|
|United States|New York|February |2|373.78|
|United States|New York|October  |10|197.98|
|United States|New York|August   |8|190.87|
|United States|New York|November |11|114.95|
|United States|New York|January  |1|54.95|
|United States|Palo Alto|June     |6|1472.00|
|United States|Palo Alto|April    |4|1043.00|
|United States|Palo Alto|July     |7|845.00|
|United States|Palo Alto|May      |5|616.99|
|United States|Paris|April    |4|1049.86|
|United States|Phoenix|January  |1|18.99|
|United States|Pittsburgh|August   |8|119.92|
|United States|Salem|February |2|11030.03|
|United States|Salem|January  |1|91.17|
|United States|San Bruno|July     |7|5997.98|
|United States|San Bruno|June     |6|2189.85|
|United States|San Bruno|May      |5|167.94|
|United States|San Diego|June     |6|2.99|
|United States|San Francisco|May      |5|5080.88|
|United States|San Francisco|November |11|796.00|
|United States|San Francisco|June     |6|558.72|
|United States|San Francisco|July     |7|133.99|
|United States|San Francisco|April    |4|0.00|
|United States|San Jose|May      |5|1170.50|
|United States|San Jose|July     |7|856.98|
|United States|San Jose|April    |4|548.96|
|United States|San Jose|June     |6|156.92|
|United States|San Jose|February |2|18.99|
|United States|San Jose|August   |8|9.98|
|United States|Santa Clara|June     |6|188.96|
|United States|Santa Monica|June     |6|104.93|
|United States|Seattle|July     |7|1851.00|
|United States|Seattle|June     |6|1393.70|
|United States|Seattle|October  |10|207.80|
|United States|Seattle|March    |3|43.96|
|United States|South San Francisco|June     |6|419.94|
|United States|Sunnyvale|June     |6|8363.11|
|United States|Sunnyvale|July     |7|3448.69|
|United States|Sunnyvale|May      |5|2319.84|
|United States|Sunnyvale|November |11|623.88|
|United States|Sunnyvale|December |12|170.91|
|United States|Sunnyvale|September|9|99.99|
|United States|Washington|June     |6|11.99|
|Uruguay|Montevideo|May      |5|29.99|
