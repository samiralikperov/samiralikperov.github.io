---
title: Hotel Operations
date: 2024-08-20 18:00:00
layout: post
categories: [SQL,EDA]
tags: [DATA ANALYSIS,CUSTOMER SERVICE,OPERATIONS MANAGEMENT,BUSINESS ANALYTICS]
image:
  path: assets/img/headers/post_2.png
  alt: Hotel Operations
---

## Introduction: Hotel Operations

LuxurStay Hotels is a major, international chain of hotels. They offer hotels for both business and leisure travellers in major cities across the world. The chain prides themselves on the level of customer service that they offer. 

However, the management has been receiving complaints about slow room service in some hotel branches. As these complaints are impacting the customer satisfaction rates, it has become a serious issue. Recent data shows that customer satisfaction has dropped from the 4.5 rating that they expect. 

You are working with the Head of Operations to identify possible causes and hotel branches with the worst problems. 

## Data

The following schema diagram shows the tables available. You have only been provided with data where customers provided a feedback rating.

![hotel_operations](/assets/img/pictures/hotel_operations.png)

## Task 1

Before you can start any analysis, you need to confirm that the data is accurate and reflects what you expect to see. 

It is known that there are some issues with the `branch` table, and the data team have provided the following data description. 

Write a query to return data matching this description. You must match all column names and description criteria.

| Column Name | Criteria                                                |
|-------------|---------------------------------------------------------|
|id | Nominal. The unique identifier of the hotel. </br>Missing values are not possible due to the database structure.|
| location | Nominal. The location of the particular hotel. One of four possible values, 'EMEA', 'NA', 'LATAM' and 'APAC'. </br>Missing values should be replaced with “Unknown”. |
| total_rooms | Discrete. The total number of rooms in the hotel. Must be a positive integer between 1 and 400. </br>Missing values should be replaced with the default number of rooms, 100. |
| staff_count | Discrete. The number of staff employeed in the hotel service department. </br>Missing values should be replaced with the total_rooms multiplied by 1.5. |
| opening_date | Discrete. The year in which the hotel opened. This can be any value between 2000 and 2023. </br>Missing values should be replaced with 2023. |
| target_guests | Nominal. The primary type of guest that is expected to use the hotel. Can be one of 'Leisure' or 'Business'. </br>Missing values should be replaced with 'Leisure'. |


```python
-- Write your query for task 1 in this cell
SELECT
	id,
	COALESCE(TRIM(location), 'Unknown') as location,
	COALESCE(NULLIF(total_rooms,0),100) as total_rooms,
	COALESCE(NULLIF(staff_count,0),COALESCE(NULLIF(total_rooms,0),100) * 1.5) as staff_count,
	COALESCE(NULLIF(CAST(opening_date AS integer),0),2023) as opening_date,
	CASE WHEN TRIM(target_guests) IN ('B.', 'Busniess') THEN 'Business'
		 ELSE COALESCE(TRIM(target_guests), 'Leisure')
	END AS target_guests
FROM (
 SELECT 
        id,
        location,
        total_rooms,
        staff_count,
        CASE 
            WHEN opening_date ~ '^[0-9]+$' THEN CAST(opening_date as integer) 
            ELSE NULL 
        END AS opening_date,
        target_guests
    FROM 
        branch
) AS processed_branch;
	
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>location</th>
      <th>total_rooms</th>
      <th>staff_count</th>
      <th>opening_date</th>
      <th>target_guests</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>LATAM</td>
      <td>168</td>
      <td>178</td>
      <td>2017</td>
      <td>Business</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>APAC</td>
      <td>154</td>
      <td>82</td>
      <td>2010</td>
      <td>Leisure</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>APAC</td>
      <td>212</td>
      <td>467</td>
      <td>2003</td>
      <td>Leisure</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>APAC</td>
      <td>230</td>
      <td>387</td>
      <td>2023</td>
      <td>Business</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>APAC</td>
      <td>292</td>
      <td>293</td>
      <td>2002</td>
      <td>Business</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>95</th>
      <td>96</td>
      <td>APAC</td>
      <td>237</td>
      <td>257</td>
      <td>2000</td>
      <td>Business</td>
    </tr>
    <tr>
      <th>96</th>
      <td>97</td>
      <td>APAC</td>
      <td>107</td>
      <td>169</td>
      <td>2005</td>
      <td>Business</td>
    </tr>
    <tr>
      <th>97</th>
      <td>98</td>
      <td>EMEA</td>
      <td>196</td>
      <td>126</td>
      <td>2002</td>
      <td>Leisure</td>
    </tr>
    <tr>
      <th>98</th>
      <td>99</td>
      <td>APAC</td>
      <td>242</td>
      <td>251</td>
      <td>2021</td>
      <td>Business</td>
    </tr>
    <tr>
      <th>99</th>
      <td>100</td>
      <td>LATAM</td>
      <td>349</td>
      <td>612</td>
      <td>2020</td>
      <td>Business</td>
    </tr>
  </tbody>
</table>
<p>100 rows × 6 columns</p>
</div>



## Task 2

The Head of Operations wants to know whether there is a difference in time taken to respond to a customer request in each hotel. They already know that different services take different lengths of time. 

Calculate the average and maximum duration for each branch and service. Your output should include the columns `service_id`, `branch_id`, `avg_time_taken` and `max_time_taken`. Values should be rounded to two decimal places where appropriate. 


```python
-- Write your query for task 2 in this cell
SELECT
	service_id,
	branch_id,
	ROUND(AVG(time_taken),2) as avg_time_taken,
	ROUND(MAX(time_taken),2) as max_time_taken
FROM request
GROUP BY service_id,
		 branch_id;
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>service_id</th>
      <th>branch_id</th>
      <th>avg_time_taken</th>
      <th>max_time_taken</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>46</td>
      <td>13.09</td>
      <td>16.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>99</td>
      <td>9.13</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>8</td>
      <td>2.56</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>13</td>
      <td>13.53</td>
      <td>17.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>46</td>
      <td>2.08</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>380</th>
      <td>4</td>
      <td>73</td>
      <td>9.43</td>
      <td>13.0</td>
    </tr>
    <tr>
      <th>381</th>
      <td>4</td>
      <td>88</td>
      <td>9.36</td>
      <td>12.0</td>
    </tr>
    <tr>
      <th>382</th>
      <td>1</td>
      <td>89</td>
      <td>2.77</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>383</th>
      <td>4</td>
      <td>31</td>
      <td>9.00</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>384</th>
      <td>4</td>
      <td>72</td>
      <td>9.14</td>
      <td>11.0</td>
    </tr>
  </tbody>
</table>
<p>385 rows × 4 columns</p>
</div>



## Task 3

The management team want to target improvements in `Meal` and `Laundry` service in Europe (`EMEA`) and Latin America (`LATAM`). 

Write a query to return the `description` of the service, the `id` and `location` of the branch, the id of the request as `request_id` and the `rating` for the services and locations of interest to the management team. 

Use the original branch table, not the output of task 1. 


```python
-- Write your query for task 3 in this cell
SELECT
	s.description as description,
	b.id as id,
	b.location as location,
	r.id as request_id,
	r.rating as rating
FROM request as r
JOIN service as s
	ON r.service_id = s.id
JOIN branch as b
	ON r.branch_id = b.id
WHERE 1=1
	AND (s.description = 'Meal' or s.description = 'Laundry')
	AND (b.location = 'EMEA' or b.location = 'LATAM');
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>description</th>
      <th>id</th>
      <th>location</th>
      <th>request_id</th>
      <th>rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Laundry</td>
      <td>63</td>
      <td>EMEA</td>
      <td>3</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Laundry</td>
      <td>69</td>
      <td>LATAM</td>
      <td>6</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Meal</td>
      <td>44</td>
      <td>EMEA</td>
      <td>18</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Laundry</td>
      <td>57</td>
      <td>LATAM</td>
      <td>19</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Meal</td>
      <td>1</td>
      <td>LATAM</td>
      <td>21</td>
      <td>4</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5042</th>
      <td>Meal</td>
      <td>30</td>
      <td>EMEA</td>
      <td>17662</td>
      <td>4</td>
    </tr>
    <tr>
      <th>5043</th>
      <td>Meal</td>
      <td>64</td>
      <td>LATAM</td>
      <td>17669</td>
      <td>4</td>
    </tr>
    <tr>
      <th>5044</th>
      <td>Meal</td>
      <td>51</td>
      <td>LATAM</td>
      <td>17674</td>
      <td>5</td>
    </tr>
    <tr>
      <th>5045</th>
      <td>Meal</td>
      <td>23</td>
      <td>EMEA</td>
      <td>17681</td>
      <td>5</td>
    </tr>
    <tr>
      <th>5046</th>
      <td>Meal</td>
      <td>41</td>
      <td>LATAM</td>
      <td>17682</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
<p>5047 rows × 5 columns</p>
</div>



## Task 4

So that you can take a more detailed look at the lowest performing hotels, you want to get service and branch information where the average rating for the branch and service combination is lower than 4.5 - the target set by management.  

Your query should return the `service_id` and `branch_id`, and the average rating (`avg_rating`), rounded to 2 decimal places.


```python
-- Write your query for task 4 in this cell
SELECT
	r.service_id as service_id,
	r.branch_id as branch_id,
	ROUND(AVG(r.rating),2) as avg_rating
FROM request as r
JOIN branch as b
	ON r.branch_id = b.id
JOIN service as s
	ON r.service_id = s.id
GROUP BY r.service_id,r.branch_id
HAVING 
	AVG(r.rating) < 4.5;
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>service_id</th>
      <th>branch_id</th>
      <th>avg_rating</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4</td>
      <td>99</td>
      <td>3.83</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>46</td>
      <td>3.78</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>8</td>
      <td>3.64</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>46</td>
      <td>3.81</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3</td>
      <td>15</td>
      <td>4.00</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>210</th>
      <td>3</td>
      <td>8</td>
      <td>3.38</td>
    </tr>
    <tr>
      <th>211</th>
      <td>1</td>
      <td>64</td>
      <td>3.59</td>
    </tr>
    <tr>
      <th>212</th>
      <td>4</td>
      <td>88</td>
      <td>3.60</td>
    </tr>
    <tr>
      <th>213</th>
      <td>4</td>
      <td>93</td>
      <td>3.72</td>
    </tr>
    <tr>
      <th>214</th>
      <td>4</td>
      <td>31</td>
      <td>4.00</td>
    </tr>
  </tbody>
</table>
<p>215 rows × 3 columns</p>
</div>


