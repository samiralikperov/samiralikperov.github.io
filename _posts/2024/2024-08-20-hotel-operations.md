---
title: Hotel Operations
date: 2024-09-24 18:00:00
layout: post
categories: [SQL]
tags: [DATA ANALYSIS,OPERATIONS MANAGEMENT]
image:
  path: assets/img/headers/post_2.png
  alt: Hotel Operations
---

For this project, LuxurStay Hotels, a major international hotel chain catering to both business and leisure travelers, has always taken pride in their high level of customer service. However, recently there has been a rise in complaints about slow room service at certain branches, which has led to a drop in customer satisfaction ratings. The management, aiming to maintain their expected 4.5 rating, has raised this issue as a priority.

I'm currently collaborating with the Head of Operations to investigate the potential causes of these delays and identify which hotel branches are experiencing the most significant problems.

To learn more about me and my work, expand the ABOUT ME section.

<details>
  <summary style="font-size: 24px; font-weight: bold; cursor: pointer;">ABOUT ME</summary>
  
  Learn more about me on my <a href="https://samiralikperov.github.io/about/" target="_blank">ABOUT</a> page. Below, you can find links to explore more of my projects categorized by topics, access my resume, and contact me.

  <br>

  | <a href="https://samiralikperov.github.io/categories/" target="_blank"><strong>PROJECTS</strong></a> | <a href="https://docs.google.com/document/d/1BEL5l5ZnlTdJc5OKiuH1SkiMQf6hS6HRAZUZvlrRANM/edit#heading=h.ifsro82jsgea" target="_blank"><strong>RESUME</strong></a> | <a href="https://www.linkedin.com/in/samiralikperov/" target="_blank"><strong>CONTACT</strong></a> |
</details>

## Data

The following schema diagram shows the tables available. You have only been provided with data where customers provided a feedback rating.

![hotel_operations](/assets/img/pictures/hotel_operations.png)

## **Cleaning the Hotel Branch Data**

### Situation:
The management of LuxurStay Hotels is concerned about customer complaints regarding room service delays. To begin analyzing the data and pinpoint the branches with the worst problems, I was asked to ensure that the data in the branch table is accurate and matches the description provided by the data team.

### Task:
My task was to write a query that meets the following criteria:

Each hotel should have a valid identifier (id), and no values should be missing.
The location column should only contain four valid categories, with missing values replaced by “Unknown.”
The number of rooms in each hotel (total_rooms) must be a positive integer between 1 and 400, with any missing values defaulted to 100.
The number of staff employed in the hotel’s service department (staff_count) should reflect a ratio of 1.5 staff per room.
The opening_date should be between 2000 and 2023, with missing values defaulted to 2023.
The target_guests should be labeled as either "Leisure" or "Business," with missing values set to "Leisure."

### Action:
To clean and ensure the data is accurate:
I wrote a query that retrieved all columns from the branch table.
I used conditional logic (CASE statements) to replace any missing or invalid values according to the provided data description.
For staff_count, I calculated the correct value by multiplying the total_rooms by 1.5 in case of missing data.

Here’s the query:

```sql
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
### Result:
The query successfully returned cleaned data that adhered to the data team's description, making it ready for further analysis. This ensures that any further investigation into the room service delays is based on accurate and reliable data.

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



## **Analyzing Response Times Across Hotel Branches**

### Situation:
The Head of Operations at LuxurStay Hotels suspects that different branches may take varying amounts of time to respond to customer requests. Given that different services naturally have different response times, they asked me to analyze whether these times differ across branches.

### Task:
I was tasked with calculating:
The average time taken to respond to a customer request for each service and branch.
The maximum time taken to respond to a request for each service and branch. The output needed to include the following columns:
service_id: The unique identifier for the service provided.
branch_id: The identifier for the hotel branch.
avg_time_taken: The average time taken to respond to a customer request, rounded to two decimal places.
max_time_taken: The maximum time taken to respond to a customer request, also rounded to two decimal places.

### Action:
To complete this task:
I used the AVG and MAX functions to calculate the average and maximum response times for each service and branch.
I grouped the data by service_id and branch_id to ensure that the calculations were done separately for each service in each branch.
I used the ROUND function to ensure the times were rounded to two decimal places, as required.

Here’s the query:

```sql
SELECT
	service_id,
	branch_id,
	ROUND(AVG(time_taken),2) as avg_time_taken,
	ROUND(MAX(time_taken),2) as max_time_taken
FROM request
GROUP BY service_id,
		 branch_id;
```
### Result:
The query returned the average and maximum response times for each service in each branch, providing the Head of Operations with valuable insights into whether certain branches take longer to respond to customer requests. This analysis will allow the operations team to identify where improvements can be made to enhance customer service across different locations.

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



## **Focusing on Meal and Laundry Services in Key Regions**

### Situation:
The management team at LuxurStay Hotels wants to focus on improving the Meal and Laundry services in their branches located in Europe (EMEA) and Latin America (LATAM). They are specifically interested in reviewing the service descriptions, branch details, and customer ratings in these regions to determine where enhancements are needed.

### Task:
I was asked to retrieve the following details for services and branches of interest:
description: The description of the service (either Meal or Laundry).
id: The unique identifier of the branch.
location: The region where the branch is located (either EMEA or LATAM).
request_id: The ID associated with the service request.
rating: The customer rating for the service provided.
The goal was to use the original branch table to identify relevant data.

### Action:
To complete this task:
I wrote a query that filtered the data to include only Meal and Laundry services.
I further filtered the results to include only branches located in EMEA and LATAM.
I selected the relevant columns to match the management team's requirements.
I ensured the query pulled data from the original branch table, as specified.

Here’s the query:

```sql
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
### Result:
The query successfully returned a list of Meal and Laundry service requests from branches located in Europe (EMEA) and Latin America (LATAM). This data gave the management team clear insight into the performance of these services and helped them identify areas needing improvement.

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


## **Identifying Low-Performing Hotels**

### Situation:
The management at LuxurStay Hotels has set a target for customer satisfaction, aiming for an average rating of 4.5 or higher. However, there are some hotels where the average rating is falling below this target, and the operations team wants to identify which branch and service combinations are underperforming.

### Task:
I was tasked with retrieving the following information for services and branches that have an average rating lower than the target:
service_id: The unique identifier for the service.
branch_id: The unique identifier for the hotel branch.
avg_rating: The average rating for the service and branch combination, rounded to 2 decimal places.
The goal was to focus on services and branches where the average rating is below 4.5.

### Action:
To address this task, I:
Wrote a query to calculate the average rating for each combination of service and branch.
Filtered the data to include only those combinations where the average rating is below 4.5.
Ensured that the results were rounded to two decimal places for clarity.

Here’s the query:

```sql
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

### Result:
The query successfully returned the list of services and branches where the average rating was below the target of 4.5. This data allowed the operations team to identify underperforming hotels and focus on improving customer satisfaction at these locations.


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


