# Fresh_Segments Case_Study
<img width="544" alt="case study 1" src="https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/2ad790e1-e607-46a4-9d50-3d74813ea6d1">

## Business Task
Fresh Segments is a digital marketing agency that helps other businesses analyse trends in online ad click behaviour for their unique customer base.

Clients share their customer lists with the Fresh Segments team who then aggregate interest metrics and generate a single dataset worth of metrics for further analysis.

In particular - the composition and rankings for different interests are provided for each client showing the proportion of their customer list who interacted with online assets related to each interest for each month.

Danny has asked for your assistance to analyse aggregated metrics for an example client and provide some high level insights about the customer list and their interests.

## Entity Relationship Diagram
### Table: interest_metrics
- This table contains information about aggregated interest metrics for a specific major client of Fresh Segments which makes up a large proportion of their customer base.
  
| _month | _year | month_year | interest_id | composition | index_value | ranking | percentile_ranking |
|--------|-------|------------|-------------|-------------|-------------|---------|-------------------|
| 7      | 2018  | 07-2018    | 32486       | 11.89       | 6.19        | 1       | 99.86             |
| 7      | 2018  | 07-2018    | 6106        | 9.93        | 5.31        | 2       | 99.73             |
| 7      | 2018  | 07-2018    | 18923       | 10.85       | 5.29        | 3       | 99.59             |
| 7      | 2018  | 07-2018    | 6344        | 10.32       | 5.1         | 4       | 99.45             |
| 7      | 2018  | 07-2018    | 100         | 10.77       | 5.04        | 5       | 99.31             |
| 7      | 2018  | 07-2018    | 69          | 10.82       | 5.03        | 6       | 99.18             |
| 7      | 2018  | 07-2018    | 79          | 11.21       | 4.97        | 7       | 99.04             |
| 7      | 2018  | 07-2018    | 6111        | 10.71       | 4.83        | 8       | 98.9              |
| 7      | 2018  | 07-2018    | 6214        | 9.71        | 4.83        | 8       | 98.9              |
| 7      | 2018  | 07-2018    | 19422       | 10.11       | 4.81        | 10      | 98.63             |

### Table: interest_map
- This table contains information about interest map for a specific major client of Fresh Segments which makes up a large proportion of their customer base.
  

| id | interest_name             | interest_summary                                                       | created_at          | last_modified       |
|----|---------------------------|------------------------------------------------------------------------|---------------------|---------------------|
| 1  | Fitness Enthusiasts      | Consumers using fitness tracking apps and websites.                    | 2016-05-26 14:57:59 | 2018-05-23 11:30:12 |
| 2  | Gamers                    | Consumers researching game reviews and cheat codes.                     | 2016-05-26 14:57:59 | 2018-05-23 11:30:12 |
| 3  | Car Enthusiasts           | Readers of automotive news and car reviews.                             | 2016-05-26 14:57:59 | 2018-05-23 11:30:12 |
| 4  | Luxury Retail Researchers | Consumers researching luxury product reviews and gift ideas.            | 2016-05-26 14:57:59 | 2018-05-23 11:30:12 |
| 5  | Brides & Wedding Planners | People researching wedding ideas and vendors.                           | 2016-05-26 14:57:59 | 2018-05-23 11:30:12 |
| 6  | Vacation Planners         | Consumers reading reviews of vacation destinations and accommodations.  | 2016-05-26 14:57:59 | 2018-05-23 11:30:13 |
| 7  | Motorcycle Enthusiasts    | Readers of motorcycle news and reviews.                                | 2016-05-26 14:57:59 | 2018-05-23 11:30:13 |
| 8  | Business News Readers     | Readers of online business news content.                                | 2016-05-26 14:57:59 | 2018-05-23 11:30:12 |
| 12 | Thrift Store Shoppers     | Consumers shopping online for clothing at thrift stores and researching locations. | 2016-05-26 14:57:59 | 2018-03-16 13:14:00 |
| 13 | Advertising Professionals | People who read advertising industry news.                              | 2016-05-26 14:57:59 | 2018-05-23 11:30:12 |

## Data Exploration and Cleansing
### 1. Update the fresh_segments.interest_metrics table by modifying the month_year column to be a date data type with the start of the month

```SQL
-- Update the column to the date format
UPDATE fresh_segments.interest_metrics
SET month_year = CONCAT(SUBSTRING(month_year, 4, 4), '-', LEFT(month_year, 2), '-01');

-- Modify the column to the DATE type
ALTER TABLE fresh_segments.interest_metrics
MODIFY COLUMN month_year DATE;
```
![Screenshot 2024-04-16 200930](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/a3e023f0-1cc3-416a-ae31-100ac4842b6e)


### 2. What is count of records in the fresh_segments.interest_metrics for each month_year value sorted in chronological order (earliest to latest) with the null values appearing first?

```SQL
SELECT month_year, COUNT(*) AS Count_of_records
FROM interest_metrics
GROUP BY month_year
ORDER BY month_year;
```
![Screenshot 2024-04-16 201505](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/0ce11103-fa2b-4dfc-b2cf-c30db9deda3d)


### 3. What do you think we should do with these null values in the fresh_segments.interest_metrics?

- The presence of null values in the interest_id fields renders the corresponding values in the composition, index_value, ranking, and percentile_ranking fields meaningless without specific information on interest_id.
- Before dropping the values, it would be useful to find out the percentage of null values.
  
```SQL
SELECT COUNT(*) AS Count
FROM interest_metrics
WHERE interest_id is null;
```
![Screenshot 2024-04-16 203229](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/fe23f44a-4951-46e9-9239-0700ec425bd7)
- Let's delete the null values as we know the total count of null values.
```SQL
DELETE FROM interest_metrics
WHERE interest_id IS NULL;
-- Run again to check the null values
SELECT COUNT(*) AS Count
FROM interest_metrics
WHERE interest_id is null;
```
![Screenshot 2024-04-16 203336](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/94d05423-2c56-4c7e-9fec-032b6f157cca)
- There are no null values in fresh_segments.interest_metrics.

### 4. How many interest_id values exist in the fresh_segments.interest_metrics table but not in the fresh_segments.interest_map table? What about the other way around?

```SQL
SELECT 
    COUNT(DISTINCT metrics.interest_id) AS interest_count,
    COUNT(DISTINCT map.id) AS map_id_count,
    SUM(CASE WHEN metrics.interest_id IS NULL THEN 1 ELSE 0 END) AS not_in_metric,
    SUM(CASE WHEN map.id IS NULL THEN 1 ELSE 0 END) AS not_in_map
FROM interest_metrics AS metrics
LEFT JOIN interest_map AS map ON metrics.interest_id = map.id
UNION
SELECT 
    COUNT(DISTINCT metrics.interest_id) AS interest_count,
    COUNT(DISTINCT map.id) AS map_id_count,
    SUM(CASE WHEN metrics.interest_id IS NULL THEN 1 ELSE 0 END) AS not_in_metric,
    SUM(CASE WHEN map.id IS NULL THEN 1 ELSE 0 END) AS not_in_map
FROM interest_metrics AS metrics
RIGHT JOIN interest_map AS map ON metrics.interest_id = map.id;
```
![Screenshot 2024-04-16 205706](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/0296704b-fcb3-495d-af38-f1fb1233d116)

### 5. Summarise the id values in the fresh_segments.interest_map by its total record count in this table.

```SQL
SELECT id, interest_name, COUNT(*) AS Total_records
FROM interest_metrics AS metrics
JOIN interest_map AS map ON map.id = metrics.interest_id
GROUP BY id, interest_name
ORDER BY Total_records DESC, id;
```
![Screenshot 2024-04-16 214717](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/6e25d962-8a1d-4aa2-94c3-7e3aaa6f704a)

### 6. What sort of table join should we perform for our analysis and why? Check your logic by checking the rows where 'interest_id = 21246' in your joined output and include all columns from fresh_segments.interest_metrics and all columns from fresh_segments.interest_map except from the id column.

```SQL
SELECT *
FROM interest_metrics AS metrics
JOIN interest_map AS map
	ON map.id = metrics.interest_id
WHERE interest_id = 21246
	AND _month IS NOT NULL;
```
![Screenshot 2024-04-16 215337](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/3ae03dfa-335a-4068-a30e-8e09a4c98a55)

### 7. Are there any records in your joined table where the month_year value is before the created_at value from the fresh_segments.interest_map table? Do you think these values are valid and why?

```SQL
SELECT COUNT(*) AS Count_of_records
FROM interest_metrics AS metrics
JOIN interest_map AS map 
	ON map.id = metrics.interest_id
WHERE month_year < created_at;
```
![Screenshot 2024-04-16 220657](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/65a671e7-e9af-4b7e-a037-1907690e06c7)

- In theory, the count 188 should not be valid, however in this case, when we modified the month_year before, we put the first day of the month.
- So definitely there will always be records earlier than created_at, that is why we can assume that the raw data records in interest_metrics is not detailed enough.
- Considering the dates are in the same month, hence we will consider the records as valid.

## B. Interest Analysis
### 1. Which interests have been present in all month_year dates in our dataset?
  
  - First, how many month_year do we have? is there any gap month where no interest at all?
  ```SQL
  SELECT 
	COUNT(DISTINCT interest_id) AS interest_id_count,
    COUNT(DISTINCT month_year) AS month_year_count
  FROM interest_metrics;
  ```
  ![image](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/caa18477-957f-476f-878f-e5bf933d514c)

  - There are 14 distinct month_year dates and 1202 distinct interest_id's.

  ```SQL
  WITH interest_cte AS
  (SELECT interest_id,
		COUNT(DISTINCT month_year) AS Total_months
  FROM interest_metrics
  WHERE month_year IS NOT NULL
  GROUP BY interest_id)

  SELECT total_months, COUNT(DISTINCT interest_id) AS Total_interest
  FROM interest_cte
  WHERE Total_months = 14
  GROUP BY Total_months
  ORDER BY Total_interest DESC;
  ```
  ![Screenshot 2024-04-16 224026](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/e9fe8331-af10-4ab0-baeb-ce2df98309c2)

  - 480 interests out of 1202 interests are present in all the month_year dates.

### 2. Using this same total_months measure - calculate the cumulative percentage of all records starting at 14 months - which total_months value passes the 90% cumulative percentage value?

  ```SQL
    WITH interest_cte AS
      (SELECT interest_id, 
      	COUNT(DISTINCT month_year) AS Total_months
        FROM interest_metrics 
        WHERE interest_id IS NOT NULL
        GROUP BY interest_id
        ),
      interest_counts_cte AS 
      (SELECT Total_months, 
      	COUNT(DISTINCT interest_id) AS interest_count
       FROM interest_cte
       GROUP BY total_months
       )
       SELECT 
      	Total_months,
          interest_count,
          ROUND(100 * SUM(interest_count) OVER(ORDER BY total_months DESC) /
      		(SUM(interest_count) OVER()), 2) AS cumulative_percentage
        FROM interest_counts_cte;
  ```
  ![Screenshot 2024-04-16 225612](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/3628ef72-adb0-448e-869e-0ad151d04258)

  - Interests with total months of 6 and above received a 90% and above percentage. Interests below this mark should be investigated to improve their clicks and customer interactions.
 
### 3. If we were to remove all interest_id values which are lower than the total_months value we found in the previous question - how many total data points would we be removing?

- Firstly get removed interest_id values with less than 6 months of data.
  
```SQL
WITH cte_removed_interests AS (
SELECT
  interest_id
FROM interest_metrics
WHERE interest_id IS NOT NULL
GROUP BY interest_id
HAVING COUNT(DISTINCT month_year) < 6
)

SELECT
  COUNT(*) AS removed_rows
FROM interest_metrics 
WHERE  EXISTS (
  SELECT 1
  FROM cte_removed_interests
  WHERE interest_metrics.interest_id = cte_removed_interests.interest_id
);
```
![Screenshot 2024-04-16 231337](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/45c42970-5c39-4ca9-90a7-0f69c2ffc63b)

- This is for example if we want to remove interest_id values with less than 6 months of data.

### 4. Does this decision make sense to remove these data points from a business perspective? Use an example where there are all 14 months present to a removed interest example for your arguments - think about what it means to have less months present from a segment perspective.

```SQL
SELECT 
    COUNT(*) 
FROM interest_metrics
WHERE interest_id IN (
    SELECT
        interest_id
    FROM interest_metrics
    WHERE interest_id IS NOT NULL
    GROUP BY 1
    HAVING count(DISTINCT month_year) < 14);
```
![Screenshot 2024-04-16 231905](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/de1b22d8-8706-44cd-be62-21478094e8df)


