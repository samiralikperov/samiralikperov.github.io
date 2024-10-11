---
title: Loan Insights
date: 2024-09-16 18:00:00
layout: post
categories: [SQL]
tags: [DATA ANALYSIS, LOAN]
image:
  path: assets/img/headers/post_1.png
  alt: Loan Insights
---

For this project, I had to work with EasyLoan, a company that offers a variety of loan services, including personal loans, car loans, and mortgages. EasyLoan operates in several markets, including Canada, the United Kingdom, and the United States.

The analytics team is responsible for reporting on the performance of these services across different regions. My task was to ensure the data was accessible and reliable before the team began analyzing and preparing reports for the strategy department.

**Database Schema**  
![lending](/assets/img/pictures/lending_1.png)

To learn more about me and my work, expand the ABOUT ME section.

<details>
  <summary style="font-size: 24px; font-weight: bold; cursor: pointer;">ABOUT ME</summary>
  
  Learn more about me on my <a href="https://samiralikperov.github.io/about/" target="_blank">ABOUT</a> page. Below, you can find links to explore more of my projects categorized by topics, access my resume, and contact me.

  <br>
 | <a href="https://samiralikperov.github.io/categories/" target="_blank"><strong>PROJECTS</strong></a> | <a href="https://docs.google.com/document/d/1BEL5l5ZnlTdJc5OKiuH1SkiMQf6hS6HRAZUZvlrRANM/edit#heading=h.ifsro82jsgea" target="_blank"><strong>RESUME</strong></a> | <a href="https://www.linkedin.com/in/samiralikperov/" target="_blank"><strong>CONTACT</strong></a> |
</details>

## **Cleaning Client Data for the Analytics Dashboard**

### Situation:
The analytics team at EasyLoan wanted to create a dashboard that provided detailed client information. However, they needed to ensure that the data in the `client` table met their expectations in terms of format and accuracy before they could proceed with the dashboard.

### Task:
I was assigned the task of writing a query to clean and format the `client` data to match the specific requirements. This involved:
- Ensuring the `date_of_birth` field is in the correct format (`YYYY-MM-DD`).
- Cleaning the `employment_status` field so that it only contains "employed" or "unemployed" in lowercase.
- Ensuring the `country` field is correctly listed as either "USA," "UK," or "CA" in uppercase.

### Action:
To achieve the task, I created a SQL query that:
1. Checked the format of the `date_of_birth` column and converted any incorrect formats to `YYYY-MM-DD`.
2. Standardized the `employment_status` column by converting all employment-related statuses to either "employed" or "unemployed," defaulting unknown values to "unknown."
3. Standardized the `country` column to only accept "USA," "UK," or "CA" in uppercase and replaced any invalid country values with "UNKNOWN."

Here’s the query:

```sql
SELECT
    client_id,
    CASE
        WHEN date_of_birth ~ '^\d{4}-\d{2}-\d{2}T' THEN date_of_birth
        ELSE TO_CHAR(TO_DATE(date_of_birth, 'Month DD, YYYY'), 'YYYY-MM-DD')
    END AS date_of_birth,
    CASE
        WHEN LOWER(employment_status) IN ('full-time', 'fulltime', 'part-time', 'parttime', 'employed', 'emplouyed') THEN 'employed'
        WHEN LOWER(employment_status) = 'unemployed' THEN 'unemployed'
        ELSE 'unknown'
    END AS employment_status,
    CASE
        WHEN UPPER(country) IN ('USA', 'UK', 'CA') THEN UPPER(country)
        ELSE 'UNKNOWN'
    END AS country
FROM 
    client;
```
### Result:
The query successfully standardized the client data by cleaning the format for date_of_birth, employment_status, and country, making the data ready for the analytics dashboard. This allowed the team to move forward with creating the client insights dashboard.

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
      <th>client_id</th>
      <th>date_of_birth</th>
      <th>employment_status</th>
      <th>country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>1963-07-08</td>
      <td>unemployed</td>
      <td>USA</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1957-02-07</td>
      <td>unemployed</td>
      <td>UK</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1993-02-21</td>
      <td>employed</td>
      <td>CA</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1978-03-19</td>
      <td>employed</td>
      <td>CA</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>2000-10-02</td>
      <td>employed</td>
      <td>USA</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>295</th>
      <td>296</td>
      <td>1963-05-02</td>
      <td>employed</td>
      <td>CA</td>
    </tr>
    <tr>
      <th>296</th>
      <td>297</td>
      <td>2004-08-25</td>
      <td>employed</td>
      <td>UK</td>
    </tr>
    <tr>
      <th>297</th>
      <td>298</td>
      <td>1997-08-03</td>
      <td>unemployed</td>
      <td>UK</td>
    </tr>
    <tr>
      <th>298</th>
      <td>299</td>
      <td>2004-08-16</td>
      <td>unemployed</td>
      <td>CA</td>
    </tr>
    <tr>
      <th>299</th>
      <td>300</td>
      <td>1965-03-02</td>
      <td>employed</td>
      <td>USA</td>
    </tr>
  </tbody>
</table>
<p>300 rows × 4 columns</p>
</div>

## **Filling in Missing Repayment Channels**


### Situation:
The backend system experienced an issue, causing some values in the repayment_channel column to be missing. These missing values are critical to the analysis of loan repayments, and they need to be filled before proceeding with further analysis. Fortunately, a pattern has been identified for the missing values, which allows us to derive the repayment channel based on the repayment_amount.

### Task:
I was tasked with writing a query that would fill in the missing or invalid repayment channel values based on the following criteria:

Repayments higher than 4000 dollars should be made via bank account.
Repayments lower than 1000 dollars should be made via mail.

### Action:
To address this task, I created a query that:

Checks for rows where repayment_channel is either missing (NULL) or contains invalid values ('-').
Fills in the missing values based on the repayment amount:
If the repayment_amount is greater than 4000, the repayment channel is set to 'bank account'.
If the repayment_amount is less than 1000, the repayment channel is set to 'mail'.
Returns the updated values of the repayment_channel while leaving other valid entries unchanged.

Here’s the query:

```sql
SELECT
	repayment_id,
	loan_id,
	repayment_date,
	repayment_amount,
	CASE
		WHEN (repayment_channel IS NULL OR repayment_channel = '-') AND repayment_amount > 4000 THEN 'bank account'
		WHEN (repayment_channel IS NULL OR repayment_channel = '-') AND repayment_amount < 1000 THEN 'mail'
		ELSE repayment_channel
	END AS repayment_channel
FROM
	repayment;
```

### Result:
The query successfully identified and replaced the missing or invalid repayment channel values with the appropriate repayment method, ensuring that the repayment table adheres to the required criteria.


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
      <th>repayment_id</th>
      <th>loan_id</th>
      <th>repayment_date</th>
      <th>repayment_amount</th>
      <th>repayment_channel</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>357</td>
      <td>2022-10-16 00:00:00+00:00</td>
      <td>1675.83</td>
      <td>bank account</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>805</td>
      <td>2023-01-12 00:00:00+00:00</td>
      <td>867.22</td>
      <td>debit card</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>843</td>
      <td>2022-06-02 00:00:00+00:00</td>
      <td>718.83</td>
      <td>phone</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>243</td>
      <td>2022-12-26 00:00:00+00:00</td>
      <td>1620.97</td>
      <td>credit card</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>991</td>
      <td>2023-03-18 00:00:00+00:00</td>
      <td>2182.17</td>
      <td>phone</td>
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
      <th>1495</th>
      <td>1496</td>
      <td>192</td>
      <td>2023-02-11 00:00:00+00:00</td>
      <td>2506.47</td>
      <td>debit card</td>
    </tr>
    <tr>
      <th>1496</th>
      <td>1497</td>
      <td>999</td>
      <td>2022-11-21 00:00:00+00:00</td>
      <td>1324.61</td>
      <td>bank account</td>
    </tr>
    <tr>
      <th>1497</th>
      <td>1498</td>
      <td>966</td>
      <td>2023-01-26 00:00:00+00:00</td>
      <td>1375.81</td>
      <td>debit card</td>
    </tr>
    <tr>
      <th>1498</th>
      <td>1499</td>
      <td>20</td>
      <td>2023-03-30 00:00:00+00:00</td>
      <td>1231.25</td>
      <td>debit card</td>
    </tr>
    <tr>
      <th>1499</th>
      <td>1500</td>
      <td>560</td>
      <td>2022-04-21 00:00:00+00:00</td>
      <td>361.11</td>
      <td>mail</td>
    </tr>
  </tbody>
</table>
<p>1500 rows × 5 columns</p>
</div>

## **Analyzing Loans for US Clients Using the Online Contract System**


### Situation:
As of January 1st, 2022, all US clients began using an online system to sign contracts. The analytics team needs to analyze loan data specifically for US clients who used this new system, in order to gain insights on how it has impacted loan agreements.

### Task:
I was tasked with writing a query that retrieves the relevant loan data for US clients who signed their contracts using the new online system starting from January 1st, 2022. The output should include the following columns:

client_id: The unique identifier for the client.
contract_date: The date the contract was signed.
principal_amount: The amount of the loan's principal.
loan_type: The type of loan taken by the client.

### Action:
To fulfill the task, I created a query that:

Filters the data to include only rows where the contract_date is on or after January 1st, 2022.
Restricts the results to clients who reside in the USA.
Selects the relevant columns: client_id, contract_date, principal_amount, and loan_type.

Here’s the query:
```sql
SELECT
    c.client_id,
    ct.contract_date,
    l.principal_amount,
    l.loan_type
FROM 
    loan l
JOIN 
    client c ON l.client_id = c.client_id
JOIN 
    contract ct ON l.contract_id = ct.contract_id
WHERE 
    c.country = 'USA'
    AND ct.contract_date >= '2022-01-01';
```
### Result:
The query successfully retrieved loan data for US clients who used the online contract system starting from January 1st, 2022, providing the analytics team with the necessary information for their analysis.

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
      <th>client_id</th>
      <th>contract_date</th>
      <th>principal_amount</th>
      <th>loan_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>267</td>
      <td>2022-03-08 00:00:00+00:00</td>
      <td>179230</td>
      <td>personal</td>
    </tr>
    <tr>
      <th>1</th>
      <td>50</td>
      <td>2022-01-13 00:00:00+00:00</td>
      <td>143729</td>
      <td>mortgage</td>
    </tr>
    <tr>
      <th>2</th>
      <td>280</td>
      <td>2022-01-02 00:00:00+00:00</td>
      <td>171122</td>
      <td>car</td>
    </tr>
    <tr>
      <th>3</th>
      <td>79</td>
      <td>2022-01-24 00:00:00+00:00</td>
      <td>43784</td>
      <td>mortgage</td>
    </tr>
    <tr>
      <th>4</th>
      <td>245</td>
      <td>2022-01-03 00:00:00+00:00</td>
      <td>95003</td>
      <td>mortgage</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>89</th>
      <td>8</td>
      <td>2022-01-13 00:00:00+00:00</td>
      <td>148282</td>
      <td>personal</td>
    </tr>
    <tr>
      <th>90</th>
      <td>232</td>
      <td>2022-02-08 00:00:00+00:00</td>
      <td>78386</td>
      <td>personal</td>
    </tr>
    <tr>
      <th>91</th>
      <td>95</td>
      <td>2022-03-20 00:00:00+00:00</td>
      <td>39098</td>
      <td>personal</td>
    </tr>
    <tr>
      <th>92</th>
      <td>123</td>
      <td>2022-03-06 00:00:00+00:00</td>
      <td>48213</td>
      <td>car</td>
    </tr>
    <tr>
      <th>93</th>
      <td>211</td>
      <td>2022-02-23 00:00:00+00:00</td>
      <td>147004</td>
      <td>mortgage</td>
    </tr>
  </tbody>
</table>
<p>94 rows × 4 columns</p>
</div>

## Comparing Interest Rates Across Countries for Different Loan Types

### Situation:
The business strategy team is considering offering more competitive interest rates to the US market. To support this initiative, the analytics team wants to compare the average interest rates for each loan type across different countries. This comparison will help determine if there are significant differences in the interest rates offered in the US compared to other countries.

### Task:
I was tasked with writing a query that retrieves the average interest rates for each loan type, broken down by country. The output should include:
loan_type: The type of loan being offered.
country: The country where the loan was offered.
avg_rate: The average interest rate for the corresponding loan type and country.


### Action:
To address this task, I wrote a query that:
Groups the data by loan_type and country.
Calculates the average interest rate (avg_rate) for each combination of loan type and country using the AVG() function.
Selects the relevant columns: loan_type, country, and avg_rate.

Here’s the query:

```sql
SELECT
	loan_type, 
	country,
	AVG(l.interest_rate)as avg_rate
FROM 
	loan l
JOIN
	client c ON l.client_id = c.client_id
GROUP BY l.loan_type, c.country
ORDER BY l.loan_type, c.country;
```
### Result:
The query successfully provided the analytics team with the average interest rates for each loan type across different countries, enabling them to compare rates and support the business strategy team's decision-making process.

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
      <th>loan_type</th>
      <th>country</th>
      <th>avg_rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>car</td>
      <td>CA</td>
      <td>0.112039</td>
    </tr>
    <tr>
      <th>1</th>
      <td>car</td>
      <td>UK</td>
      <td>0.122613</td>
    </tr>
    <tr>
      <th>2</th>
      <td>car</td>
      <td>USA</td>
      <td>0.103636</td>
    </tr>
    <tr>
      <th>3</th>
      <td>mortgage</td>
      <td>CA</td>
      <td>0.044068</td>
    </tr>
    <tr>
      <th>4</th>
      <td>mortgage</td>
      <td>UK</td>
      <td>0.042281</td>
    </tr>
    <tr>
      <th>5</th>
      <td>mortgage</td>
      <td>USA</td>
      <td>0.043860</td>
    </tr>
    <tr>
      <th>6</th>
      <td>personal</td>
      <td>CA</td>
      <td>0.217253</td>
    </tr>
    <tr>
      <th>7</th>
      <td>personal</td>
      <td>UK</td>
      <td>0.198738</td>
    </tr>
    <tr>
      <th>8</th>
      <td>personal</td>
      <td>USA</td>
      <td>0.202721</td>
    </tr>
  </tbody>
</table>
</div>


