---
title: Grocery Store Sales
date: 2024-08-20 18:00:00
layout: post
categories: [SQL]
tags: [DATA ANALYSIS,SALES ANALYSIS]
image:
  path: assets/img/headers/post_4.png
  alt: Grocery Store Sales
---

## SQL Progect: Grocery Store Sales

FoodYum is a grocery store chain that is based in the United States.

FoodYum sells items such as produce, meat, dairy, baked goods, snacks, and other household food staples.

As food costs rise, FoodYum wants to make sure it keeps stocking products in all categories that cover a range of prices to ensure they have stock for a broad range of customers. 

<details>
  <summary style="font-size: 24px; font-weight: bold; cursor: pointer;">ABOUT ME</summary>
  
  Learn more about me on my [ABOUT](https://samiralikperov.github.io/about/){:target="_blank"} page. Below, you can find links to explore more of my projects categorized by topics, access my resume, and contact me.

[**PROJECTS**](https://samiralikperov.github.io/categories/){:target="_blank"} 
[**RESUME**](https://docs.google.com/document/d/1BEL5l5ZnlTdJc5OKiuH1SkiMQf6hS6HRAZUZvlrRANM/edit#heading=h.ifsro82jsgea){:target="_blank"} 
[**CONTACT**](https://www.linkedin.com/in/samiralikperov/){:target="_blank"}

</details>

## Data

The data is available in the table `products`.

The dataset contains records of customers for their last full year of the loyalty program.

| Column Name | Criteria                                                |
|-------------|---------------------------------------------------------|
|product_id | Nominal. The unique identifier of the product. Missing values are not possible due to the database structure.|
| product_type | Nominal. The product category type of the product, one of 5 values (Produce, Meat, Dairy, Bakery, Snacks).Missing values should be replaced with “Unknown”. |
| brand | Nominal. The brand of the product. One of 7 possible values. Missing values should be replaced with “Unknown”. |
| weight | Continuous. The weight of the product in grams. This can be any positive value, rounded to 2 decimal places. Missing values should be replaced with the overall median weight. |
| price | Continuous. The price the product is sold at, in US dollars. This can be any positive value, rounded to 2 decimal places. Missing values should be replaced with the overall median price. |
| average_units_sold | Discrete. The average number of units sold each month. This can be any positive integer value. Missing values should be replaced with 0. |
| year_added | Nominal. The year the product was first added to FoodYum stock. Missing values should be replaced with 2022. |
| stock_location | Nominal. The location that stock originates. This can be one of four warehouse locations, A, B, C or D. Missing values should be replaced with “Unknown”. |

## Task 1

Last year (2022) there was a bug in the product system. For some products that were added in that year, the `year_added` value was not set in the data. As the year the product was added may have an impact on the price of the product, this is important information to have. 

Write a query to determine how many products have the `year_added` value missing. Your output should be a single column, `missing_year`, with a single row giving the number of missing values.


```sql
SELECT COUNT(*) as missing_year
FROM products 
WHERE year_added IS NULL;
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
      <th>missing_year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>170</td>
    </tr>
  </tbody>
</table>
</div>



## Task 2

Given what you know about the year added data, you need to make sure all of the data is clean before you start your analysis. The table below shows what the data should look like. 
  

| Column Name | Criteria                                                |
|-------------|---------------------------------------------------------|
|product_id | Nominal. The unique identifier of the product. Missing values are not possible due to the database structure.|
| product_type | Nominal. The product category type of the product, one of 5 values (Produce, Meat, Dairy, Bakery, Snacks). Missing values should be replaced with “Unknown”. |
| brand | Nominal. The brand of the product. One of 7 possible values. Missing values should be replaced with “Unknown”. |
| weight | Continuous. The weight of the product in grams. This can be any positive value, rounded to 2 decimal places. Missing values should be replaced with the overall median weight. |
| price | Continuous. The price the product is sold at, in US dollars. This can be any positive value, rounded to 2 decimal places. Missing values should be replaced with the overall median price. |
| average_units_sold | Discrete. The average number of units sold each month. This can be any positive integer value. Missing values should be replaced with 0. |
| year_added | Nominal. The year the product was first added to FoodYum stock. Missing values should be replaced with last year (2022). |
| stock_location | Nominal. The location that stock originates. This can be one of four warehouse locations, A, B, C or D. Missing values should be replaced with “Unknown”. |


```sql
WITH median_values AS (
    SELECT 
        PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY CAST(REGEXP_REPLACE(weight, '[^\d.]', '', 'g') AS NUMERIC)) AS median_weight,
        PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY price) AS median_price
    FROM 
        products
)
SELECT
    product_id,
    COALESCE(TRIM(product_type), 'Unknown') AS product_type,
    COALESCE(NULLIF(TRIM(REPLACE(brand, '-', '')), ''), 'Unknown') AS brand,
    COALESCE(
        ROUND(CAST(NULLIF(REGEXP_REPLACE(weight, '[^\d.]', '', 'g'), '') AS NUMERIC), 2),
        ROUND((SELECT median_weight FROM median_values)::NUMERIC, 2)
    ) AS weight,
    COALESCE(
        TO_CHAR(ROUND(price::NUMERIC, 2), '9999999999.99'),
        TO_CHAR(ROUND((SELECT median_price FROM median_values)::NUMERIC, 2), '9999999999.99')
    ) AS price,
    COALESCE(average_units_sold, 0) AS average_units_sold,
    COALESCE(year_added, 2022) AS year_added,
    COALESCE(UPPER(TRIM(stock_location)), 'Unknown') AS stock_location
FROM 
    products;

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
      <th>product_id</th>
      <th>product_type</th>
      <th>brand</th>
      <th>weight</th>
      <th>price</th>
      <th>average_units_sold</th>
      <th>year_added</th>
      <th>stock_location</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Bakery</td>
      <td>TopBrand</td>
      <td>602.61</td>
      <td>11.00</td>
      <td>15</td>
      <td>2022</td>
      <td>C</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Produce</td>
      <td>SilverLake</td>
      <td>478.26</td>
      <td>8.08</td>
      <td>22</td>
      <td>2022</td>
      <td>C</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Produce</td>
      <td>TastyTreat</td>
      <td>532.38</td>
      <td>6.16</td>
      <td>21</td>
      <td>2018</td>
      <td>B</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Bakery</td>
      <td>StandardYums</td>
      <td>453.43</td>
      <td>7.26</td>
      <td>21</td>
      <td>2021</td>
      <td>D</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Produce</td>
      <td>GoldTree</td>
      <td>588.63</td>
      <td>7.88</td>
      <td>21</td>
      <td>2020</td>
      <td>A</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1695</th>
      <td>1696</td>
      <td>Meat</td>
      <td>TastyTreat</td>
      <td>503.99</td>
      <td>14.08</td>
      <td>25</td>
      <td>2017</td>
      <td>A</td>
    </tr>
    <tr>
      <th>1696</th>
      <td>1697</td>
      <td>Meat</td>
      <td>GoldTree</td>
      <td>526.89</td>
      <td>16.13</td>
      <td>25</td>
      <td>2016</td>
      <td>D</td>
    </tr>
    <tr>
      <th>1697</th>
      <td>1698</td>
      <td>Bakery</td>
      <td>YumMie</td>
      <td>583.85</td>
      <td>7.05</td>
      <td>16</td>
      <td>2021</td>
      <td>A</td>
    </tr>
    <tr>
      <th>1698</th>
      <td>1699</td>
      <td>Produce</td>
      <td>TopBrand</td>
      <td>441.64</td>
      <td>8.10</td>
      <td>19</td>
      <td>2019</td>
      <td>A</td>
    </tr>
    <tr>
      <th>1699</th>
      <td>1700</td>
      <td>Meat</td>
      <td>TopBrand</td>
      <td>518.60</td>
      <td>15.89</td>
      <td>24</td>
      <td>2021</td>
      <td>A</td>
    </tr>
  </tbody>
</table>
<p>1700 rows × 8 columns</p>
</div>



## Task 3

To find out how the range varies for each product type, your manager has asked you to determine the minimum and maximum values for each product type.   

Write a query to return the `product_type`, `min_price` and `max_price` columns. 


```sql
SELECT 
	product_type,
	MIN(price) as min_price,
	MAX(price) as max_price
FROM products 
GROUP BY product_type;
	 
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
      <th>product_type</th>
      <th>min_price</th>
      <th>max_price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Snacks</td>
      <td>5.20</td>
      <td>10.72</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Produce</td>
      <td>3.46</td>
      <td>8.78</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Dairy</td>
      <td>8.33</td>
      <td>13.97</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bakery</td>
      <td>6.26</td>
      <td>11.88</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Meat</td>
      <td>11.48</td>
      <td>16.98</td>
    </tr>
  </tbody>
</table>
</div>



## Task 4

The team want to look in more detail at meat and dairy products where the average units sold was greater than ten. 

Write a query to return the `product_id`, `price` and `average_units_sold` of the rows of interest to the team. 


```python
SELECT
	product_id,
	price,
	average_units_sold
FROM products 
WHERE 1=1
	AND product_type IN('Meat', 'Dairy')
	AND average_units_sold > 10;
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
      <th>product_id</th>
      <th>price</th>
      <th>average_units_sold</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>6</td>
      <td>16.20</td>
      <td>24</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8</td>
      <td>15.77</td>
      <td>28</td>
    </tr>
    <tr>
      <th>2</th>
      <td>9</td>
      <td>11.57</td>
      <td>30</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10</td>
      <td>13.94</td>
      <td>27</td>
    </tr>
    <tr>
      <th>4</th>
      <td>11</td>
      <td>9.26</td>
      <td>26</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>693</th>
      <td>1694</td>
      <td>16.00</td>
      <td>25</td>
    </tr>
    <tr>
      <th>694</th>
      <td>1695</td>
      <td>12.88</td>
      <td>20</td>
    </tr>
    <tr>
      <th>695</th>
      <td>1696</td>
      <td>14.08</td>
      <td>25</td>
    </tr>
    <tr>
      <th>696</th>
      <td>1697</td>
      <td>16.13</td>
      <td>25</td>
    </tr>
    <tr>
      <th>697</th>
      <td>1700</td>
      <td>15.89</td>
      <td>24</td>
    </tr>
  </tbody>
</table>
<p>698 rows × 3 columns</p>
</div>


