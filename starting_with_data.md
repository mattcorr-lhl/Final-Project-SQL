# My Questions

## Question 1: What webpage do users spend the most time on?

### Answer: Users tend to spend the most time on the Order Completed, Review Order and Payment pages. This would make sense as it is time consuming to enter payment information or leave a review. The long time spent on the order completed page could be due to users leave the page open in another tab after completing their purchase.

### SQL Query:
```SQL
SELECT pagepathlevel1 as webpage,
	   ROUND(AVG(distinct_analytics.timeonsite),2) as avg_timeonsite
FROM all_sessions 
JOIN (SELECT DISTINCT * FROM analytics) as distinct_analytics
ON all_sessions.fullvisitorid = distinct_analytics.fullvisitorid
GROUP BY webpage
ORDER BY avg_timeonsite DESC;
```

### Output:
|webpage|avg_timeonsite|
|:----|:----|
|/ordercompleted.html|1386.16|
|/revieworder.html|1362.68|
|/payment.html|1066.82|
|/store.html|689.21|
|/asearch.html|508.24|
|/google+redesign/|497.34|
|/storeitem.html|450.00|
|/yourinfo.html|342.26|
|/asearch.html/|263.31|

## Question 2: We know from a previous question (starting_with_questions #5) that the summer months are much more active in terms of revenue. Is the same true for the amount of visits the site receives?

### Answer: The May-July period does receive more overall pageviews than the rest of the year, but the difference is less pronounced than the revenue breakdown. This may indicate that the site either has large sales or increased marketing efforts in the May-July period that would make customers more likely to buy.

### SQL Queries:
```SQL
-- Summarize pageviews by Month
SELECT
		TO_CHAR(all_sessions.date, 'Month') as month,
		SUM(distinct_analytics.pageviews) as total_pageviews
FROM all_sessions
JOIN (SELECT DISTINCT * FROM analytics WHERE units_sold > 0) as distinct_analytics 
ON all_sessions.fullvisitorid = distinct_analytics.fullvisitorid
GROUP BY TO_CHAR(all_sessions.date, 'Month')
ORDER BY total_pageviews DESC;
```

```SQL
-- Same query with a CASE statement for grouping
SELECT CASE
	WHEN TRIM(TO_CHAR(all_sessions.date, 'Month')) IN ('May', 'June', 'July') THEN 'High Revenue Period (May-July)' ELSE 'Rest of Year' 
	END AS timeofyear,
    SUM(distinct_analytics.pageviews) as totalpageviews
FROM all_sessions
JOIN (SELECT DISTINCT * FROM analytics WHERE units_sold > 0) as distinct_analytics 
ON all_sessions.fullvisitorid = distinct_analytics.fullvisitorid
GROUP BY CASE
    WHEN TRIM(TO_CHAR(all_sessions.date, 'Month')) IN ('May', 'June', 'July') THEN 'High Revenue Period (May-July)' ELSE 'Rest of Year'
    END
ORDER BY totalpageviews DESC;
```

### Output (by Month):
|month|total_pageviews|
|:----|:----|
|May      |22400|
|June     |14958|
|July     |14773|
|November |8490|
|August   |7700|
|December |5470|
|March    |5016|
|April    |4539|
|February |1259|
|January  |877|
|September|211|
|October  |56|

### Output (Summarized):
|timeofyear|totalpageviews|
|:----|:----|
|High Revenue Period (May-July)|52131|
|Rest of Year|33618|

## Question 3: Is there anything we can extrapolate from the way the users are introduced to our site?

### Answer: It appears that organic searches to our site and direct sales are the most profitable means of reaching our userbase. However it is worth noting that Display sales appear to sell more units on average than any other category, this may indicate that we could significantly improve our revenue if we start doing more Display sales.

### SQL Query:
```SQL
SELECT 	sessions.channelgrouping, 
		ROUND(SUM(sessions.productprice * distinct_analytics.units_sold), 2) AS revenue,
		ROUND(AVG(units_sold), 2) as avg_units_sold,
		COUNT(*) as num_orders
FROM all_sessions as sessions
JOIN (SELECT DISTINCT * FROM analytics WHERE units_sold > 0) AS distinct_analytics
ON sessions.fullvisitorid = distinct_analytics.fullvisitorid
GROUP BY sessions.channelgrouping
HAVING SUM(sessions.productprice * distinct_analytics.units_sold) IS NOT NULL
ORDER BY revenue DESC;
```
### Output:
|channelgrouping|revenue|avg_units_sold|num_orders|
|:----|:----|:----|:----|
|Organic Search|387800.89|22.42|1156|
|Direct|242784.70|28.75|576|
|Referral|186959.72|8.36|1040|
|Display|144405.82|44.94|250|
|Paid Search|1546.56|1.10|40|
|Affiliates|368.91|1.00|9|



## Question 4: Is there any correlation between the number of page views and the number of items purchased?

### Answer: I could not find a strong correlation between the amount of times a customer visited the site and the number of items purchased. While the average amount of items purchased does slightly increase for users over 60 pageviews, the difference is negligible (5.2 vs 4.8).

### SQL Queries:
```SQL
-- Grouped by users over 60 pageviews vs under 60 pageviews
SELECT 
    CASE WHEN pageviews > 60 THEN 'Over 60 Pageviews' ELSE 'Under 60 Pageviews' END AS pageview_group,
    ROUND(AVG(units_sold), 1) AS avg_units_sold
FROM (SELECT DISTINCT * FROM analytics WHERE units_sold > 0) as distinct_analytics
WHERE pageviews IS NOT NULL
GROUP BY pageview_group
ORDER BY pageview_group ASC;
```

```SQL
-- Full pageview breakdown
SELECT pageviews, ROUND(AVG(units_sold), 1) as avg_units_sold
FROM (SELECT DISTINCT * FROM analytics WHERE units_sold > 0) as distinct_analytics
WHERE pageviews IS NOT NULL
GROUP BY pageviews
ORDER by pageviews DESC;
```

### Output (Summarized):
|pageview_group|avg_units_sold|
|:----|:----|
|Over 60 Pageviews|5.2|
|Under 60 Pageviews|4.8|


### Output (Full Breakdown):
###### *apologies for long table*
|pageviews|avg_units_sold|
|:----|:----|
|186|1.4|
|175|2.0|
|169|3.3|
|168|1.0|
|145|1.2|
|139|1.0|
|136|1.0|
|134|1.0|
|131|1.1|
|127|2.2|
|122|1.9|
|119|1.0|
|118|3.8|
|117|1.1|
|114|3.9|
|112|4.4|
|111|1.0|
|109|18.8|
|108|1.1|
|106|1.2|
|105|1.0|
|104|21.2|
|103|1.0|
|102|1.0|
|100|1.1|
|99|1.1|
|98|1.1|
|97|1.0|
|96|1.2|
|93|1.7|
|92|1.3|
|91|1.1|
|90|18.5|
|89|1.1|
|88|1.0|
|87|1.2|
|86|5.7|
|85|1.0|
|84|5.8|
|83|2.5|
|82|5.7|
|81|1.0|
|80|2.3|
|79|2.1|
|78|1.4|
|77|1.5|
|76|10.6|
|75|2.2|
|74|5.6|
|73|2.3|
|72|1.0|
|71|1.6|
|70|7.2|
|69|1.8|
|68|1.1|
|67|1.3|
|66|2.1|
|65|7.3|
|64|2.3|
|63|11.1|
|62|19.0|
|61|2.7|
|60|1.2|
|59|13.7|
|58|15.5|
|57|6.7|
|56|2.1|
|55|9.7|
|54|2.7|
|53|6.5|
|52|4.4|
|51|13.1|
|50|3.3|
|49|5.0|
|48|4.1|
|47|14.2|
|46|3.9|
|45|3.2|
|44|3.5|
|43|2.9|
|42|3.0|
|41|4.3|
|40|4.9|
|39|6.6|
|38|8.5|
|37|2.7|
|36|5.1|
|35|3.8|
|34|5.2|
|33|5.5|
|32|5.5|
|31|5.4|
|30|10.2|
|29|7.0|
|28|5.2|
|27|4.3|
|26|5.1|
|25|3.4|
|24|5.9|
|23|6.8|
|22|3.3|
|21|4.1|
|20|3.6|
|19|4.4|
|18|4.5|
|17|4.1|
|16|3.4|
|15|4.7|
|14|7.3|
|13|3.2|
|12|3.5|
|11|3.7|
|10|3.6|
|9|3.9|
|8|3.4|
|7|2.9|
|6|4.4|
|5|4.4|
|4|9.7|
|3|4.9|
|2|11.8|
|1|5.2|


