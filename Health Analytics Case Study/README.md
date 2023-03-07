# 👩🏻‍⚕️ Health Analytics Case Study

## 📌 Solution

### 1. How many unique users exist in the logs dataset?

````sql
SELECT 
  COUNT(DISTINCT id) AS unique_users
FROM health.user_logs
````
**Answer:**

<img width="178" alt="Screenshot 2023-03-07 at 3 13 56 PM" src="https://user-images.githubusercontent.com/117857989/223350581-26fd2c39-3f3c-46fd-8a2f-3ee8dbd49935.png">

***

### To answer Q2 to Q8, we will create a temporary table.

````sql
DROP TABLE IF EXISTS user_measure_count;

CREATE TEMP TABLE user_measure_count AS(
SELECT
  id,
  COUNT(*) AS measure_count,
  COUNT(DISTINCT measure) AS unique_measure_count
FROM health.user_logs
GROUP BY id);
````

<img width="801" alt="image" src="https://user-images.githubusercontent.com/81607668/128625477-926f9d69-f307-4e6a-bdd6-40d026338fed.png">

Alright, once we have created the temp table, let's move on to our questions.

````
***

### 2. How many total measurements do we have per user on average?

Question is asking for the **average number of measurements for all users**.

````sql
SELECT 
  ROUND(AVG(measure_count),2) AS avg_measurement
FROM user_measure_count;
````

**Answer:**
<img width="172" alt="Screenshot 2023-03-07 at 3 16 50 PM" src="https://user-images.githubusercontent.com/117857989/223351315-10e82875-839f-4e4f-baa3-3c99bc42e67b.png">

***

### 3. What about the median number of measurements per user?

````sql
SELECT 
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY measure_count) AS median_value
FROM user_measure_count;
````
**Answer:**
<img width="251" alt="Screenshot 2023-03-07 at 3 18 33 PM" src="https://user-images.githubusercontent.com/117857989/223351686-9abf1abb-190a-4331-9907-12343119f2b8.png">

***

### 4. How many users have 3 or more measurements?

````sql
SELECT 
  COUNT(*)
FROM user_measure_count
WHERE measure_count >= 3
````
**Answer:**
<img width="152" alt="Screenshot 2023-03-07 at 3 19 14 PM" src="https://user-images.githubusercontent.com/117857989/223351829-0f9c22e6-4922-43ca-8a70-a838526a19f9.png">

***

### 5. How many users have 1,000 or more measurements?

````sql
SELECT 
  COUNT(*)
FROM user_measure_count
WHERE measure_count >= 1000
````
**Answer:**
<img width="170" alt="Screenshot 2023-03-07 at 3 19 52 PM" src="https://user-images.githubusercontent.com/117857989/223351943-9a30703a-1978-4b99-b36e-f04dba639bb3.png">

***

### Looking at the logs data -
### 6. What is the number and percentage of the active user base who have logged blood glucose measurements?

````sql
SELECT
  measure,
  COUNT(DISTINCT id) as no_of_ids,
  ROUND(100 * COUNT(DISTINCT id)::NUMERIC / SUM(COUNT(DISTINCT id)) OVER (),2) AS blood_glucose_percentage
FROM
  health.user_logs
WHERE
  measure = 'blood_glucose'
GROUP BY
  measure;
````
**Answer:**
<img width="732" alt="Screenshot 2023-03-07 at 3 20 55 PM" src="https://user-images.githubusercontent.com/117857989/223352152-c45888a0-72b7-4852-88cc-1eaec21774f1.png">

***

### 7. What is the number and percentage of the active user base who have at least 2 types of measurements?

````sql
WITH measure_more_than_2 AS (
SELECT *
FROM user_measure_count
WHERE unique_measure_count >= 2)

SELECT
  COUNT(DISTINCT m.id) AS unique_user,
  ROUND(100 * COUNT(DISTINCT m.id)::numeric / COUNT(DISTINCT u.id),2) AS unique_user_percentage
FROM user_measure_count AS u
LEFT JOIN measure_more_than_2 AS m
  ON u.id = m.id;
````

**Answer:**
<img width="370" alt="Screenshot 2023-03-07 at 3 29 00 PM" src="https://user-images.githubusercontent.com/117857989/223353840-0392f0d8-b82b-49f0-af95-a937da678a62.png">

***

### 8. What is the number and percentage of the active user base who have all 3 measures - blood glucose, weight and blood pressure?

````sql
WITH all_measures AS (
SELECT *
FROM user_measure_count
WHERE unique_measure_count = 3)

SELECT
  COUNT(DISTINCT m.id) AS unique_user,
  ROUND(COUNT(DISTINCT m.id)::numeric / COUNT(DISTINCT u.id),2) AS unique_user_percentage
FROM user_measure_count AS u
LEFT JOIN all_measures AS m
  ON u.id = m.id;
````
**Answer:**
<img width="414" alt="Screenshot 2023-03-07 at 3 30 28 PM" src="https://user-images.githubusercontent.com/117857989/223354177-09550290-56e4-42b1-b05c-cba560adfe58.png">

***
### 9. For users that have blood pressure measurements, what is the median systolic/diastolic blood pressure values?

````sql
SELECT
  'blood_pressure' AS measure_name,
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY systolic) AS systolic_median,
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY diastolic) AS diastolic_median
FROM health.user_logs
WHERE measure = 'blood_pressure';
````
**Answer:**
<img width="735" alt="Screenshot 2023-03-07 at 3 31 35 PM" src="https://user-images.githubusercontent.com/117857989/223354407-f9ca063c-6a4a-4333-ae1d-272f89596a1a.png">

