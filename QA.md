# Quality Assurance

## What are your risk areas? Identify and describe them.

- **Data Quality:** The database contains many duplicated records, missing fields and an unknown source. All of which can significantly effect the validity of my answers. While I attempted to and remedy and issueI came across, I don't doubt I have missed a lot (some of which I am aware of, just don't have the time to fix *see `README.md`* for more).
  
- **Data Accuracy & Validity:** During the initial analysis it became obvious that there may be some field with incorrectly entered data. While I can try to rectify the obvious issues, I am unable to validate everything (e.g. I have no way to verify the `timeonsite` data was recorded accurately, same goes for every column)

## QA Process
*Describe your QA processes and include the SQL queries used to execute it.*

- **Backing up Tables** Occasionally when I did not trust result of a risky query I would create a clone to ensure everything would run smoothly
    ```SQL
    -- Example for analytics table
    SELECT *
    INTO analytics_backup
    FROM analytics;
    ```

- **Duplicate Discovery:** I noticed there were a large amount of duplicate values in the analytics table. I used the following query on every table to determine which rows were duplicated, and how many times they appeared
    ```SQL
    WITH CTE AS (
    SELECT *,
    ROW_NUMBER() OVER (PARTITION BY visitnumber, visitid, visitstarttime, date, fullvisitorid, userid, channelgrouping, socialengagementtype, units_sold, pageviews, timeonsite, bounces, revenue,unit_price 
                        ORDER BY fullvisitorid ASC) AS dupe_count
                        FROM analytics
                        )
    SELECT *
    FROM CTE
    WHERE dupe_count > 1 AND units_sold > 0
    ORDER BY dupe_count DESC;
    ```

- **NULL Discovery:** Used for many columns to determine the amount of missing values
    ```SQL
    SELECT COUNT(*)
    FROM all_sessions
    WHERE city IS NULL;
    ```

- **Validating Limits:** Used `MIN` and `MAX` on various numerical columns to ensure values fell within expected ranges
    ```SQL
    SELECT MAX(units_sold), MIN(units_sold)
    FROM analytics;
    ```

- **Inconsistent Data Discovery:** The following query was used to determine which products have multiple names for a single SKU (There are 67 remaining, I did not leave enough time to fix them all)
    ```SQL
    -- Find items with multiple names
    SELECT productsku, COUNT(DISTINCT v2productname) as 
    FROM all_sessions
    GROUP BY productsku
    HAVING COUNT(DISTINCT v2productname) > 1;

    -- View the specific sku to see the name variants
    SELECT productsku, v2productname
    FROM all_sessions
    WHERE productsku = 'GGOEACCQ017299'
    GROUP BY productsku, v2productname;
    ```

