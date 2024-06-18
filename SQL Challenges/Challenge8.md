## 🧼 A. Data Exploration and Cleansing

**1. Update the `fresh_segments.interest_metrics` table by modifying the `month_year` column to be a `date` data type with the start of the month**

```sql
ALTER TABLE fresh_segments.interest_metrics
ALTER month_year TYPE VARCHAR USING to_char(to_date(month_year, 'MM-YYYY'), 'YYYY-MM-DD');


SELECT * FROM fresh_segments.interest_metrics LIMIT 10;
```

| _month 	| _year 	| month_year 	| interest_id 	| composition 	| index_value 	| ranking 	| percentile_ranking 	|
|--------	|-------	|------------	|-------------	|-------------	|-------------	|---------	|--------------------	|
| 7      	| 2018  	| 2018-07-01 	| 32486       	| 11.89       	| 6.19        	| 1       	| 99.86              	|
| 7      	| 2018  	| 2018-07-01 	| 6106        	| 9.93        	| 5.31        	| 2       	| 99.73              	|
| 7      	| 2018  	| 2018-07-01 	| 18923       	| 10.85       	| 5.29        	| 3       	| 99.59              	|
| 7      	| 2018  	| 2018-07-01 	| 6344        	| 10.32       	| 5.1         	| 4       	| 99.45              	|
| 7      	| 2018  	| 2018-07-01 	| 100         	| 10.77       	| 5.04        	| 5       	| 99.31              	|
| 7      	| 2018  	| 2018-07-01 	| 69          	| 10.82       	| 5.03        	| 6       	| 99.18              	|
| 7      	| 2018  	| 2018-07-01 	| 79          	| 11.21       	| 4.97        	| 7       	| 99.04              	|
| 7      	| 2018  	| 2018-07-01 	| 6111        	| 10.71       	| 4.83        	| 8       	| 98.9               	|
| 7      	| 2018  	| 2018-07-01 	| 6214        	| 9.71        	| 4.83        	| 8       	| 98.9               	|
| 7      	| 2018  	| 2018-07-01 	| 19422       	| 10.11       	| 4.81        	| 10      	| 98.63              	|
***
  
**2. What is count of records in the `fresh_segments.interest_metrics` for each `month_year` value sorted in chronological order (earliest to latest) with the `null` values appearing first?**

```sql
SELECT 
  month_year,
  COUNT(*) AS record_count
FROM 
  fresh_segments.interest_metrics
GROUP BY 
  month_year
ORDER BY 
  CASE WHEN month_year IS NULL THEN 0 ELSE 1 END,
  month_year;
```
| month_year 	| record_count 	|
|------------	|--------------	|
| null       	| 1194         	|
| 2018-07-01 	| 729          	|
| 2018-08-01 	| 767          	|
| 2018-09-01 	| 780          	|
| 2018-10-01 	| 857          	|
| 2018-11-01 	| 928          	|
| 2018-12-01 	| 995          	|
| 2019-01-01 	| 973          	|
| 2019-02-01 	| 1121         	|
| 2019-03-01 	| 1136         	|
| 2019-04-01 	| 1099         	|
| 2019-05-01 	| 857          	|
| 2019-06-01 	| 824          	|
| 2019-07-01 	| 864          	|
| 2019-08-01 	| 1149         	|

**3. What do you think we should do with these `null` values in the `fresh_segments.interest_metrics`?**

The `null` values appear in `_month`, `_year`, `month_year`, and `interest_id`. The corresponding values in `composition`, `index_value`, `ranking`, and `percentile_ranking` fields are not meaningful without the specific information on `interest_id` and dates. 

Before dropping the values, it would be useful to find out the percentage of `null` values.

```sql
SELECT 
  ROUND(100 * (SUM(CASE WHEN _month IS NULL THEN 1 ELSE 0 END) * 1.0 / COUNT(*)), 2) AS month_null_perc,
  ROUND(100 * (SUM(CASE WHEN _year IS NULL THEN 1 ELSE 0 END) * 1.0 / COUNT(*)), 2) AS year_null_perc,
  ROUND(100 * (SUM(CASE WHEN month_year IS NULL THEN 1 ELSE 0 END) * 1.0 / COUNT(*)), 2) AS month_year_null_perc,
  ROUND(100 * (SUM(CASE WHEN interest_id IS NULL THEN 1 ELSE 0 END) * 1.0 / COUNT(*)), 2) AS interest_id_null_perc
FROM 
  fresh_segments.interest_metrics;

```

| month_null_perc 	| year_null_perc 	| month_year_null_perc 	| interest_id_null_perc 	|
|-----------------	|----------------	|----------------------	|-----------------------	|
| 8.37            	| 8.37           	| 8.37                 	| 8.36                  	|

The percentage of null values is 8.37%. We're gonna delete these 'null' values.

```sql
DELETE FROM fresh_segments.interest_metrics
WHERE interest_id IS NULL;

SELECT 
  ROUND(100 * (SUM(CASE WHEN _month IS NULL THEN 1 ELSE 0 END) * 1.0 / COUNT(*)), 2) AS month_null_perc,
  ROUND(100 * (SUM(CASE WHEN _year IS NULL THEN 1 ELSE 0 END) * 1.0 / COUNT(*)), 2) AS year_null_perc,
  ROUND(100 * (SUM(CASE WHEN month_year IS NULL THEN 1 ELSE 0 END) * 1.0 / COUNT(*)), 2) AS month_year_null_perc,
  ROUND(100 * (SUM(CASE WHEN interest_id IS NULL THEN 1 ELSE 0 END) * 1.0 / COUNT(*)), 2) AS interest_id_null_perc
FROM 
  fresh_segments.interest_metrics;
```

| month_null_perc 	| year_null_perc 	| month_year_null_perc 	| interest_id_null_perc 	|
|-----------------	|----------------	|----------------------	|-----------------------	|
| 0           	| 0           	| 0                 	| 0                 	|

***
  
**4. How many `interest_id` values exist in the `fresh_segments.interest_metrics` table but not in the `fresh_segments.interest_map` table? What about the other way around?**

```sql
SELECT 
  COUNT(DISTINCT map.id) AS map_id_count,
  COUNT(DISTINCT metrics.interest_id) AS metrics_id_count,
  SUM(CASE WHEN map.id is NULL THEN 1 END) AS not_in_metric,
  SUM(CASE WHEN metrics.interest_id is NULL THEN 1 END) AS not_in_map
FROM fresh_segments.interest_map map
FULL OUTER JOIN fresh_segments.interest_metrics metrics
  ON metrics.interest_id = map.id;
```

| map_id_count 	| metrics_id_count 	| not_in_metric 	| not_in_map 	|
|--------------	|------------------	|---------------	|------------	|
| 1209         	| 1202             	| 0             	| 7          	|

***
  
**5. Summarise the id values in the `fresh_segments.interest_map` by its total record count in this table.**

```sql
SELECT COUNT(*)
FROM fresh_segments.interest_map;
```

| id_count 	|
|--------------	|
| 1209         	|
***
  
**6. What sort of table join should we perform for our analysis and why? Check your logic by checking the rows where 'interest_id = 21246' in your joined output and include all columns from `fresh_segments.interest_metrics` and all columns from `fresh_segments.interest_map` except from the id column.**

We should be using `INNER JOIN` to perform our analysis.

```sql
SELECT im.*, 
       map.interest_name, 
       map.interest_summary, 
       map.created_at, 
       map.last_modified
FROM fresh_segments.interest_metrics im
INNER JOIN fresh_segments.interest_map map ON im.interest_id::INTEGER = map.id
WHERE im.interest_id = '21246';
```

| _month 	| _year 	| month_year 	| interest_id 	| composition 	| index_value 	| ranking 	| percentile_ranking 	| interest_name                    	| interest_summary                                      	| created_at               	| last_modified            	|
|--------	|-------	|------------	|-------------	|-------------	|-------------	|---------	|--------------------	|----------------------------------	|-------------------------------------------------------	|--------------------------	|--------------------------	|
| null   	| null  	| null       	| 21246       	| 1.61        	| 0.68        	| 1191    	| 0.25               	| Readers of El Salvadoran Content 	| People reading news from El Salvadoran media sources. 	| 2018-06-11T17:50:04.000Z 	| 2018-06-11T17:50:04.000Z 	|
| 4      	| 2019  	| 2019-04-01 	| 21246       	| 1.58        	| 0.63        	| 1092    	| 0.64               	| Readers of El Salvadoran Content 	| People reading news from El Salvadoran media sources. 	| 2018-06-11T17:50:04.000Z 	| 2018-06-11T17:50:04.000Z 	|
| 3      	| 2019  	| 2019-03-01 	| 21246       	| 1.75        	| 0.67        	| 1123    	| 1.14               	| Readers of El Salvadoran Content 	| People reading news from El Salvadoran media sources. 	| 2018-06-11T17:50:04.000Z 	| 2018-06-11T17:50:04.000Z 	|
| 2      	| 2019  	| 2019-02-01 	| 21246       	| 1.84        	| 0.68        	| 1109    	| 1.07               	| Readers of El Salvadoran Content 	| People reading news from El Salvadoran media sources. 	| 2018-06-11T17:50:04.000Z 	| 2018-06-11T17:50:04.000Z 	|
| 1      	| 2019  	| 2019-01-01 	| 21246       	| 2.05        	| 0.76        	| 954     	| 1.95               	| Readers of El Salvadoran Content 	| People reading news from El Salvadoran media sources. 	| 2018-06-11T17:50:04.000Z 	| 2018-06-11T17:50:04.000Z 	|
| 12     	| 2018  	| 2018-12-01 	| 21246       	| 1.97        	| 0.7         	| 983     	| 1.21               	| Readers of El Salvadoran Content 	| People reading news from El Salvadoran media sources. 	| 2018-06-11T17:50:04.000Z 	| 2018-06-11T17:50:04.000Z 	|
| 11     	| 2018  	| 2018-11-01 	| 21246       	| 2.25        	| 0.78        	| 908     	| 2.16               	| Readers of El Salvadoran Content 	| People reading news from El Salvadoran media sources. 	| 2018-06-11T17:50:04.000Z 	| 2018-06-11T17:50:04.000Z 	|
| 10     	| 2018  	| 2018-10-01 	| 21246       	| 1.74        	| 0.58        	| 855     	| 0.23               	| Readers of El Salvadoran Content 	| People reading news from El Salvadoran media sources. 	| 2018-06-11T17:50:04.000Z 	| 2018-06-11T17:50:04.000Z 	|
| 9      	| 2018  	| 2018-09-01 	| 21246       	| 2.06        	| 0.61        	| 774     	| 0.77               	| Readers of El Salvadoran Content 	| People reading news from El Salvadoran media sources. 	| 2018-06-11T17:50:04.000Z 	| 2018-06-11T17:50:04.000Z 	|
| 8      	| 2018  	| 2018-08-01 	| 21246       	| 2.13        	| 0.59        	| 765     	| 0.26               	| Readers of El Salvadoran Content 	| People reading news from El Salvadoran media sources. 	| 2018-06-11T17:50:04.000Z 	| 2018-06-11T17:50:04.000Z 	|
| 7      	| 2018  	| 2018-07-01 	| 21246       	| 2.26        	| 0.65        	| 722     	| 0.96               	| Readers of El Salvadoran Content 	| People reading news from El Salvadoran media sources. 	| 2018-06-11T17:50:04.000Z 	| 2018-06-11T17:50:04.000Z 	|

***

**7. Are there any records in your joined table where the `month_year` value is before the `created_at` value from the `fresh_segments.interest_map` table? Do you think these values are valid and why?**

```sql
SELECT COUNT(*)
FROM fresh_segments.interest_metrics im
INNER JOIN fresh_segments.interest_map map ON im.interest_id::INTEGER = map.id
WHERE im.month_year::DATE < map.created_at;
```

| count 	      |
|--------------	|
| 188         	|

***

## 📚 B. Interest Analysis
  
**1. Which interests have been present in all `month_year` dates in our dataset?**

Find out how many unique `month_year` in dataset.

```sql
SELECT 
  COUNT(DISTINCT month_year) AS unique_month_year_count, 
  COUNT(DISTINCT interest_id) AS unique_interest_id_count
FROM fresh_segments.interest_metrics;
```

| unique_month_year_count 	| unique_interest_id_count 	|
|-------------------------	|--------------------------	|
| 14                      	| 1202                     	|

There are 14 distinct `month_year` dates and 1202 distinct `interest_id`s.

```sql
WITH interest_cte AS (
SELECT 
  interest_id, 
  COUNT(DISTINCT month_year) AS total_months
FROM fresh_segments.interest_metrics
WHERE month_year IS NOT NULL
GROUP BY interest_id
)

SELECT 
  c.total_months,
  COUNT(DISTINCT c.interest_id)
FROM interest_cte c
WHERE total_months = 14
GROUP BY c.total_months
ORDER BY count DESC;
```

<img width="263" alt="image" src="https://user-images.githubusercontent.com/81607668/139029765-3403fb8b-e93d-4fde-989b-b648d62fcb3f.png">

480 interests out of 1202 interests are present in all the `month_year` dates.

***
  
**2. Using this same total_months measure - calculate the cumulative percentage of all records starting at 14 months - which total_months value passes the 90% cumulative percentage value?**

Find out the point in which interests present in a particular number of months are not performing well. For example, interest id 101 only appeared in 6 months due to non or lack of clicks and interactions, so we can consider to cut the interest off. 

```sql
WITH cte_interest_months AS (
SELECT
  interest_id,
  MAX(DISTINCT month_year) AS total_months
FROM fresh_segments.interest_metrics
WHERE interest_id IS NOT NULL
GROUP BY interest_id
),
cte_interest_counts AS (
  SELECT
    total_months,
    COUNT(DISTINCT interest_id) AS interest_count
  FROM cte_interest_months
  GROUP BY total_months
)

SELECT
  total_months,
  interest_count,
  ROUND(100 * SUM(interest_count) OVER (ORDER BY total_months DESC) / -- Create running total field using cumulative values of interest count
      (SUM(INTEREST_COUNT) OVER ()),2) AS cumulative_percentage
FROM cte_interest_counts;
```

<img width="446" alt="image" src="https://user-images.githubusercontent.com/81607668/139035737-cfe32a44-5c48-4376-a9bc-96c15daf162b.png">

Interests with total months of 6 and above received a 90% and above percentage. Interests below this mark should be investigated to improve their clicks and customer interactions. 
***

**3. If we were to remove all `interest_id` values which are lower than the `total_months` value we found in the previous question - how many total data points would we be removing?**

***

**4. Does this decision make sense to remove these data points from a business perspective? Use an example where there are all 14 months present to a removed interest example for your arguments - think about what it means to have less months present from a segment perspective. **

***

**5. If we include all of our interests regardless of their counts - how many unique interests are there for each month?**
  
***

## 🧩 C. Segment Analysis
 
1. Using the complete dataset - which are the top 10 and bottom 10 interests which have the largest composition values in any month_year? Only use the maximum composition value for each interest but you must keep the corresponding month_year
2. Which 5 interests had the lowest average ranking value?
3. Which 5 interests had the largest standard deviation in their percentile_ranking value?
4. For the 5 interests found in the previous question - what was minimum and maximum percentile_ranking values for each interest and its corresponding year_month value? Can you describe what is happening for these 5 interests?
5. How would you describe our customers in this segment based off their composition and ranking values? What sort of products or services should we show to these customers and what should we avoid?

***

## 👆🏻 D. Index Analysis

The `index_value` is a measure which can be used to reverse calculate the average composition for Fresh Segments’ clients. Average composition can be calculated by dividing the composition column by the index_value column rounded to 2 decimal places.

1. What is the top 10 interests by the average composition for each month?
2. For all of these top 10 interests - which interest appears the most often?
3. What is the average of the average composition for the top 10 interests for each month?
4. What is the 3 month rolling average of the max average composition value from September 2018 to August 2019 and include the previous top ranking interests in the same output shown below.
5. Provide a possible reason why the max average composition might change from month to month? Could it signal something is not quite right with the overall business model for Fresh Segments?

***
