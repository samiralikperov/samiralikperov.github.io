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


# Practical Exam: Loan Insights

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
