# Cleaning the Data
What issues will you address by cleaning the data?

I will attempt to remediate the following issues using various data-manipulation queries
- Duplicated values
- Inconsistent non-set/blank values
- Improper data entry (leading spaces, double spaces, escape values)
- Incorrect storage of attributes

As a side note, I used the PGAdmin GUI to alter data types when necessary (ie increasing/reducing VARCHAR limits), so those alterations will not show up here.

## Queries:

### all_sessions:

- country
    - Replace (not set) with NULL
        ```SQL
        UPDATE all_sessions SET country = NULL WHERE country = '(not set)';
        ```

- city
    - Replaced (not set) with NULL
        ```SQL
        UPDATE all_sessions SET city = NULL WHERE city = '(not set)';
        ```

    - Changed "not available in demo dataset" to "N/A" for readability
        ```SQL
        UPDATE all_sessions SET city = 'NULL' WHERE city = 'not available in demo dataset';
        ```

- totaltransactionrevenue
    - Divided value by 1,000,000
        ```SQL
        UPDATE all_sessions SET totaltransactionrevenue = totaltransactionrevenue / 1000000;
        ```
    
- date
  - Imported the date as a `VARCHAR`, needed to convert to `DATE` data type
    ```SQL
    ALTER TABLE all_sessions
    ALTER COLUMN date TYPE DATE;
    ```

- productprice
    - Divided value by 1,000,000
        ```SQL
        UPDATE all_sessions SET productprice = productprice / 1000000;
        ```

- productrevenue
    - Divided value by 1,000,000
        ```SQL
        UPDATE all_sessions SET productrevenue = productrevenue / 1000000;
        ```

- transactionrevenue
    - Divided value by 1,000,000
        ```SQL
        UPDATE all_sessions SET transactionrevenue = transactionrevenue / 1000000;
        ```

- v2ProductName
    - Remove all mentions of the word "Google", "YouTube" and "Android"
        ```SQL
        UPDATE all_sessions SET v2productname = REPLACE(v2productname, 'Google ', '');
        UPDATE all_sessions SET v2productname = REPLACE(v2productname, 'Android ', '');
        UPDATE all_sessions SET v2productname = REPLACE(v2productname, 'YouTube ', ''); 
        ```

    - Remove the extra space in front of " Woman's Muscle Tee" (Have to escape the single quote)
        ```SQL
        UPDATE all_sessions SET v2productname = REPLACE(v2productname, ' Women''s Muscle Tee', 'Women''s Muscle Tee');
        ```

    - Fix Typo of "16 oz. Hot/Cold Tumbler" to "16 oz. Hot and Cold Tumbler" (same SKU)
        ```SQL
        UPDATE all_sessions SET v2productname = REPLACE(v2productname, '16 oz. Hot/Cold Tumbler', '16 oz. Hot and Cold Tumbler');
        ```

    - Fix Typo of "22 oz Bottle Infuser" to "22 oz Infuser Bottle"
        ```SQL
        UPDATE all_sessions SET v2productname = REPLACE(v2productname, '22 oz Bottle Infuser', '22 oz Infuser Bottle');
        ```

    - Trim Leading Spaces
        ```SQL
        UPDATE all_sessions SET v2productname = LTRIM(v2productname);
        ```
    - Fix Double Spaces
        ```SQL
        UPDATE all_sessions SET v2productname = REPLACE(v2productname, '  ', ' ');
        ```

- v2CategoryName
  - **Objective:** Split column into distinct subcategories using the "/" as a delimiter
    ```SQL 
    ALTER TABLE all_sessions
    ADD COLUMN split_category1 VARCHAR(32),
    ADD COLUMN split_category2 VARCHAR(32),
    ADD COLUMN split_category3 VARCHAR(32),
    ADD COLUMN split_category4 VARCHAR(32);

    UPDATE all_sessions
    SET split_category1 = SPLIT_PART(v2productcategory, '/', 1),
        split_category2 = SPLIT_PART(v2productcategory, '/', 2),
        split_category3 = SPLIT_PART(v2productcategory, '/', 3),
        split_category4 = SPLIT_PART(v2productcategory, '/', 4);
    ```
  - **Objective:** Remove leading spaces and bad 'blank' values
    ```SQL
    UPDATE all_sessions SET split_category1 = NULL WHERE split_category1 = '${escCatTitle}';
    UPDATE all_sessions SET split_category1 = NULL WHERE split_category1 = '(not set)';
    UPDATE all_sessions SET split_category2 = NULL WHERE split_category2 = '';
    UPDATE all_sessions SET split_category3 = NULL WHERE split_category3 = '';
    UPDATE all_sessions SET split_category4 = NULL WHERE split_category4 = '';
    ```  

### analytics:
- Duplicate Removal (TOOK TO LONG, CANCELED after 1h 30m):
    ```SQL
    DELETE FROM analytics
    WHERE fullvisitorid NOT IN (
    SELECT MIN(fullvisitorid)
    FROM analytics
    GROUP BY visitnumber,visitid,visitstarttime,date,fullvisitorid,userid,channelgrouping,socialengagementtype,units_sold,pageviews,timeonsite,bounces,revenue,unit_price);
    ```

- Instead of using the above query I always ensured to join with `JOIN (SELECT DISTINCT * FROM analytics)` to avoid entirely duplicated rows (more on this in `QA.md`)

- unit_price
  - Divided by 1,000,000
    ```SQL
    UPDATE analytics SET unit_price = unit_price / 1000000;
    ```

### products:

- Trim Leading Spaces
    ```SQL
    UPDATE products SET name = LTRIM(name);
    ```