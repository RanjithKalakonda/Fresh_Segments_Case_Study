# Fresh_Segments Case_Study
<img width="544" alt="case study 1" src="https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/2ad790e1-e607-46a4-9d50-3d74813ea6d1">
  
# 📚 Table of Contents

- [Business Task](#business-task)
- [Fresh Segment Tables](#fresh-segment-tables)
- [Data Exploration and Cleansing](#data-exploration-and-cleansing)
- [Interest Analysis](#interest-analysis)
- [Segment Analysis](#segment-analysis)
- [Index Analysis](#index-analysis)
- Please note that all the information regarding the case study has been sourced from the following link: [here](https://8weeksqlchallenge.com/case-study-8/)

## 💼 Business Task
Fresh Segments is a digital marketing agency that helps other businesses analyse trends in online ad click behaviour for their unique customer base.

Clients share their customer lists with the Fresh Segments team who then aggregate interest metrics and generate a single dataset worth of metrics for further analysis.

In particular - the composition and rankings for different interests are provided for each client showing the proportion of their customer list who interacted with online assets related to each interest for each month.

Danny has asked for your assistance to analyse aggregated metrics for an example client and provide some high level insights about the customer list and their interests.

## 📂 Fresh Segment Tables

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

## 🧼 Data Exploration and Cleansing
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

## 📈 Interest Analysis
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

### 5. If we include all of our interests regardless of their counts - how many unique interests are there for each month?

```SQL
SELECT
    month_year, 
    COUNT(DISTINCT interest_id) interest_per_month
FROM interest_metrics
GROUP BY month_year
ORDER BY month_year DESC;
```
![Screenshot 2024-04-17 114122](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/7d6b7d98-48c4-4af7-9a9a-95adf87662f4)

## 🔍 Key Findings and Insights :

- Core Interests: A significant subset of interests, representing approximately 40% of all distinct interest IDs, remains consistently relevant to customers across all 14 month-year dates. These core interests play a pivotal role in driving customer engagement and should be prioritized in marketing strategies.

- Dominant Influence: Interests with a total of 6 months or more collectively contribute to over 90% of the dataset's cumulative percentage. This highlights the dominance of a relatively small number of interests in capturing the majority of customer interactions, emphasizing their critical role in shaping marketing efforts.

- Data Completeness and Variability: While certain interests demonstrate consistent presence, others exhibit variability with fewer than 14 months of data. This variability underscores the dynamic nature of customer preferences and highlights the importance of continuously monitoring and analyzing customer interactions to maintain data completeness and accuracy for informed decision-making.

## 📈 Segment Analysis

### 1. Using the complete dataset - which are the top 10 and bottom 10 interests which have the largest composition values in any month_year? Only use the maximum composition value for each interest but you must keep the corresponding month_year.

```SQL
WITH int_rank_cte AS
(SELECT month_year, 
		interest_name,
        composition,
        RANK() OVER(PARTITION BY interest_name 
					  ORDER BY composition DESC) AS int_rank
FROM interest_metrics AS metrics
JOIN interest_map AS map 
ON map.id = metrics.interest_id
WHERE month_year IS NOT NULL
),
 top_10_cte AS 
(SELECT month_year, 
		interest_name,
        composition
 FROM int_rank_cte
 WHERE int_rank = 1
 ORDER BY composition DESC
 LIMIT 10
 ),
  bottom_10_cte AS
(SELECT month_year, 
		interest_name,
        composition
 FROM int_rank_cte
 WHERE int_rank = 1
 ORDER BY composition 
 LIMIT 10
 ),
  Final_Output AS
(SELECT * FROM top_10_cte
 UNION
SELECT * FROM bottom_10_cte
)
SELECT * FROM Final_Output
ORDER BY composition DESC;
```
![image](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/0c9a5f2b-105a-4156-98fb-38705938a72e)

### 2. Which 5 interests had the lowest average ranking value?

```SQL
SELECT interest_name,
		ROUND(AVG(ranking), 1) AS avg_ranking,
        COUNT(*) AS records_count
FROM interest_metrics AS metrics
JOIN interest_map AS map
	ON map.id = metrics.interest_id
WHERE month_year IS NOT NULL
GROUP BY interest_name
ORDER BY avg_ranking;
```
![image](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/fda9c47e-a688-4386-b2ab-ac78a15f23fe)

### 3. Which 5 interests had the largest standard deviation in their percentile_ranking value?

```SQL
SELECT interest_name, 
		ROUND(STDDEV(CAST(percentile_ranking AS DECIMAL)), 2) AS strd_ranking,
        MAX(percentile_ranking) AS max_ranking,
        MIN(percentile_ranking) AS min_ranking,
        COUNT(*) AS records_count
FROM interest_metrics AS metrics
JOIN interest_map AS map
	ON map.id = metrics.interest_id
WHERE month_year IS NOT NULL
GROUP BY interest_name
ORDER BY strd_ranking DESC
LIMIT 5;
```
![image](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/52232ac7-80f6-4ef3-b429-63e34da8784a)

### 4. For the 5 interests found in the previous question - what was minimum and maximum percentile_ranking values for each interest and its corresponding year_month value? Can you describe what is happening for these 5 interests?

```SQL
SELECT interest_id, 
		interest_name,
		month_year,
		percentile_ranking,
		MAX(percentile_ranking) AS max_ranking,
		MIN(percentile_ranking) AS min_ranking
FROM interest_metrics AS metrics
JOIN interest_map AS map
	ON map.id = metrics.interest_id
WHERE month_year IS NOT NULL
		AND interest_id IN (6260, 23, 131, 150, 38992)
GROUP BY interest_id, 
		interest_name,
		month_year,
		percentile_ranking
ORDER BY  interest_name,
		  month_year;
```

![image](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/1e735662-f952-4810-b7ef-d716df77f1d9)

- We can observe with the output that the interest dropped significantly during early period vs the end period

## 🔍 Key Findings and Insights :

- Based on the data, our customers exhibit diverse interests, with some showing strong engagement and others less so. We should prioritize products or services aligned with highly engaged interests like "Work Comes First Travelers" and "Gym Equipment Owners." Conversely, niche interests with lower engagement, such as "World of Warcraft Enthusiasts," may not warrant significant investment. Overall, focusing on high-engagement interests will likely yield better results.

## 🚀 Recommendations:

- For customers showing high composition and ranking values, we should focus on showcasing products or services directly related to their interests, as they are likely to be more receptive. Tailored marketing campaigns, personalized recommendations, and exclusive offers can help enhance their engagement further. However, for customers with lower composition and ranking values, we may need to employ strategies to pique their interest, such as offering discounts, introducing new features, or providing educational content to encourage interaction. It's essential to avoid overwhelming them with irrelevant content and instead focus on gradually nurturing their interest to build a stronger connection with our brand.

## 📈 Index Analysis

### 1. What is the top 10 interests by the average composition for each month?

```SQL
SELECT interest_name, 
		month_year,
        composition/index_value AS ind_comp,
		DENSE_RANK() OVER(PARTITION BY month_year
						ORDER BY(composition/index_value)DESC
                        ) AS ind_rank
FROM interest_metrics AS metrics
JOIN interest_map AS map
	ON map.id = metrics.interest_id
WHERE month_year IS NOT NULL
ORDER BY month_year;
```
![image](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/612cc575-0b4f-4c9a-8058-46a17d94301e)

### 2. For all of these top 10 interests - which interest appears the most often?

```SQL
WITH cte_index AS (
  SELECT month_year,
			interest_name,
			composition / index_value AS index_composition, 
    DENSE_RANK() OVER (
      PARTITION BY month_year
      ORDER BY (composition / index_value) DESC
    ) AS index_rank
  FROM interest_metrics AS metrics
JOIN interest_map AS map
	ON map.id = metrics.interest_id
WHERE month_year IS NOT NULL
)
SELECT interest_name, count(*) as Count_records
FROM cte_index
WHERE index_rank <= 10 
group by interest_name
order by Count_records desc
limit 3;
```
![image](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/8ddc35b4-73be-46e5-8dc9-be3250f60989)

### 3. What is the average of the average composition for the top 10 interests for each month?

```SQL
WITH cte_index AS (
  SELECT month_year,
			interest_name,
			composition / index_value AS index_composition, 
    DENSE_RANK() OVER (
      PARTITION BY month_year
      ORDER BY (composition / index_value) DESC
    ) AS index_rank
  FROM interest_metrics AS metrics
JOIN interest_map AS map
	ON map.id = metrics.interest_id
WHERE month_year IS NOT NULL
)
SELECT month_year, round(avg(index_composition), 1) as avg_composition
FROM cte_index
WHERE index_rank <= 10 
group by month_year
order by month_year DESC;
```

![image](https://github.com/RanjithKalakonda/Fresh_Segments_Case_Study/assets/167210784/5a3dafa6-7fe3-488c-98f3-c3ed84d81684)

## 🔍 Key Findings and Insights :

- The maximum average composition can change from month to month due to several factors. Firstly, seasonal trends and holidays may influence consumer behavior, leading to fluctuations in interest and engagement levels with certain products or services. Additionally, marketing campaigns, promotions, or events targeted at specific interests can impact their popularity and composition values within a given period. Changes in market dynamics, such as shifts in competition, emerging trends, or economic factors, may also influence consumer preferences and affect the composition of interests. Furthermore, variations in product availability, quality, or pricing may attract or deter customers, thereby impacting the average composition. Overall, the dynamic nature of consumer behavior and market conditions contributes to the variability in the maximum average composition over time.


## 🚀 Recommendations:
 - The fluctuation in the maximum average composition from month to month could potentially signal underlying issues with the overall business model for Fresh Segments. If there are significant and consistent decreases in the maximum average composition over time, it might indicate challenges in attracting and retaining customers within key interest segments. This could be attributed to various factors such as ineffective marketing strategies, poor product offerings, increasing competition, or declining customer satisfaction. Additionally, a sharp decline in the maximum average composition could suggest a loss of relevance or appeal in the market, highlighting the need for strategic adjustments to the business model to address changing consumer preferences and market dynamics. Therefore, while fluctuations in composition values are natural, consistent downward trends may warrant further analysis and corrective action to ensure the long-term success and sustainability of Fresh Segments' business model.

### 📌 NOTE:
**The data underscores the importance of prioritizing core interests and highly engaged segments in marketing strategies. However, further analysis is needed to fully understand customer behaviors and preferences. Exploring potential patterns among customer interests could offer insights into expanding market reach and enhancing customer satisfaction.**

# 📋 Conclusion:

- In conclusion, the analysis underscores the importance of prioritizing core interests and high-engagement segments in marketing strategies. While certain interests consistently drive customer engagement, others exhibit variability, emphasizing the need for continuous monitoring and adaptation. Recommendations include tailoring marketing efforts to customer interests, avoiding irrelevant content, and addressing potential issues with the business model to ensure long-term success and relevance.
