---
title: Loan Insights
date: 2024-08-20 18:00:00
layout: post
categories: [SQL,EDA]
tags: [DATA ANALYSIS,FINANCE,BANK,LOAN]
image:
  path: assets/img/headers/post_1.png
  alt: Loan Insights
---


## Introduction: Loan Insights

EasyLoan offers a wide range of loan services, including personal loans, car loans, and mortgages.
EasyLoan offers loans to clients from Canada, United Kingdom and United States.
The analytics team wants to report performance across different geographic areas. They aim to identify areas of strength and weakness for the business strategy team.
They need your help to ensure the data is accessible and reliable before they start reporting.

**Database Schema**  
![lending](/assets/img/pictures/lending_1.png)

## Task 1 

The analytics team wants to use the `client` table to create a dashboard for client details. For them to proceed, they need to be sure the data is clean enough to use.

The `client` table below illustrates what the analytics team expects the data types and format to be.

Write a query that makes the `client` table match the description provided. Your query should not update the `client` table.

| Column Name       | Description                                                      |
|-------------------|------------------------------------------------------------------|
| client_id         | Unique integer (set by the database, can’t take any other value) |
| date\_of\_birth       | Date of birth of the client, as a date  (format: YYYY-MM-DD)                              |
| employment_status        | Current employment status of the client, either employed or unemployed, as a lower case string                              |
| country          | The country where the client resides, either USA, UK or CA, as an upper case string                      |


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

## Task 2

You have been told that there was a problem in the backend system as some of the `repayment_channel` values are missing. 

The missing values are critical to the analysis so they need to be filled in before proceeding.

Luckily, they have discovered a pattern in the missing values:

- Repayment higher than 4000 dollars should be made via `bank account`.
- Repayment lower than 1000 dollars should be made via `mail`.

Write a query that makes the `repayment` table match this criteria.


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

## Task 3

Starting on January 1st, 2022, all US clients started to use an online system to sign contracts.

The analytics team wants to analyze the loans for US clients who used the new online system.

Write a query that returns the data for the analytics team. Your output should include `client_id`,`contract_date`, `principal_amount` and `loan_type` columns.


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

## Task 4

The business strategy team is considering offering a more competitive rate to the US market. 

The analytic team want to compare the average interest rates offered by the company for the same loan type in different countries to determine if there are significant differences.

Write a query that returns the data for the analytics team. Your output should include `loan_type`, `country` and `avg_rate` columns.

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


