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

As part of my SQL certification journey, I tackled an intriguing challenge with FoodYum, a U.S.-based grocery store chain. FoodYum offers a wide range of products—from fresh produce and meat to dairy, baked goods, snacks, and other household staples. With the rising costs of food, the challenge for FoodYum is to strategically manage their inventory. They aim to stock products in every category across various price ranges to accommodate a diverse customer base.

To learn more about me and my work, expand the ABOUT ME section.

<details>
  <summary style="font-size: 24px; font-weight: bold; cursor: pointer;">ABOUT ME</summary>
  
  Learn more about me on my <a href="https://samiralikperov.github.io/about/" target="_blank">ABOUT</a> page. Below, you can find links to explore more of my projects categorized by topics, access my resume, and contact me.

  <br>

  | <a href="https://samiralikperov.github.io/categories/" target="_blank"><strong>PROJECTS</strong></a> | <a href="https://docs.google.com/document/d/1BEL5l5ZnlTdJc5OKiuH1SkiMQf6hS6HRAZUZvlrRANM/edit#heading=h.ifsro82jsgea" target="_blank"><strong>RESUME</strong></a> | <a href="https://www.linkedin.com/in/samiralikperov/" target="_blank"><strong>CONTACT</strong></a> |
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

## SQL Query to Find Missing Year Added Values

### **Situation:**
In 2022, a bug was discovered in the product system where some products added during the year did not have the `year_added` value set. Since the year a product was added could potentially affect its price, it's crucial to have this information available.

### **Task:**
We need to write a query that determines how many products have a missing `year_added` value. The query should output a single column named `missing_year` containing the count of rows where the `year_added` is missing (i.e., set to `NULL`).

### **Action:**
We can achieve this by using the SQL `COUNT` function and filtering the rows where the `year_added` column is `NULL`. The `COUNT` function will count the total number of such rows, and the result will be labeled as `missing_year`.


```sql
SELECT COUNT(*) AS missing_year
FROM products 
WHERE year_added IS NULL;
```

### **Result:**
The output successfully provided the number of products missing the year_added value.

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



## SQL Query for Data Cleaning of the `products` Table

### **Situation:**
I was tasked with ensuring the data in the `products` table is clean and accurate before proceeding with further analysis. The data included missing values in various columns such as `product_type`, `brand`, `weight`, `price`, `average_units_sold`, `year_added`, and `stock_location`, which needed to be replaced based on specific criteria. Additionally, I needed to calculate median values for the `weight` and `price` columns to use as replacements for missing data.

### **Task:**
My objective was to write an SQL query that would clean the data by handling missing values in accordance with the provided criteria:
- **Nominal data** (e.g., `product_type`, `brand`, `stock_location`): replace missing values with "Unknown."
- **Continuous data** (e.g., `weight`, `price`): replace missing values with the overall median.
- **Discrete data** (e.g., `average_units_sold`): replace missing values with 0.
- **Year data** (`year_added`): replace missing values with the year 2022.

### **Action:**
To clean the data, I wrote an SQL query that:
1. Used **COALESCE** to provide fallback values for columns with missing values.
2. Replaced missing nominal values with 'Unknown' and replaced missing continuous values (such as `weight` and `price`) with the median values calculated using the **PERCENTILE_CONT** function.
3. For columns like `average_units_sold`, I used **COALESCE** to replace missing values with 0.
4. Implemented **REGEXP_REPLACE** and **CAST** to clean and process the `weight` column, ensuring it only contains numeric values.
5. Applied the **ROUND** function to ensure values are rounded to two decimal places where necessary.

Here’s the query I used:

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

### **Result:**
The query successfully cleaned the data in the products table. All missing values were handled appropriately:

Nominal values (e.g., product_type, brand) were replaced with 'Unknown' where missing.
Continuous values (e.g., weight, price) were filled with the respective median values.
Discrete values (e.g., average_units_sold) were replaced with 0.
Missing year_added values were filled with 2022.


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



## Finding Minimum and Maximum Prices by Product Type

### **Situation:**
The task involved determining the price range for each product type within the `products` table. Specifically, I needed to calculate both the minimum and maximum prices for each product type, which would allow to see the variation in price within each category of products.

### **Task:**
I was asked to write an SQL query that would return:
- The **product_type** (i.e., category of the product).
- The **min_price** (the minimum price for each product type).
- The **max_price** (the maximum price for each product type).

### **Action:**
To solve this, I wrote an SQL query that:
1. Used the **MIN()** function to find the lowest price for each `product_type`.
2. Used the **MAX()** function to find the highest price for each `product_type`.
3. Applied **GROUP BY** to group the data by `product_type`, ensuring that the results would be aggregated accordingly.

Here’s the query I used:

```sql
SELECT 
    product_type,
    MIN(price) AS min_price,
    MAX(price) AS max_price
FROM products 
GROUP BY product_type;
```
### **Result:**
The query successfully returned the minimum and maximum prices for each product_type


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



## Identifying Meat and Dairy Products with High Sales

### **Situation:**
The team wanted to focus on **Meat** and **Dairy** products, particularly those that had an average sales volume greater than 10 units per month. This would help them identify which products in these categories were performing well in terms of sales.

### **Task:**
I was tasked with writing a query to extract the following details for these high-performing products:
- **product_id**: The unique identifier for the product.
- **price**: The price of the product.
- **average_units_sold**: The average number of units sold per month.

### **Action:**
To address the task, I created a query that:
1. Filters the `products` table to include only rows where the `product_type` is either `'Meat'` or `'Dairy'`.
2. Adds another filter to return only rows where the `average_units_sold` is greater than 10.
3. Selects the relevant columns: `product_id`, `price`, and `average_units_sold`.

Here’s the query:

```sql
SELECT
    product_id,
    price,
    average_units_sold
FROM products 
WHERE 1=1
  AND product_type IN('Meat', 'Dairy')
  AND average_units_sold > 10;
```

### **Result:**
The query successfully returned a list of Meat and Dairy products with high sales volume, giving the team valuable insights into which products were performing the best.


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

## Summary of Tasks
Task 1: Identify Products with Missing Year Information
I identified products that were missing the year_added field. This information is crucial for analyzing how the year a product was added impacts its pricing.

Task 2: Clean and Prepare the Product Data
I cleaned the product data by replacing missing values with appropriate defaults (e.g., using the median for continuous variables like weight and price, or 'Unknown' for categorical data like brand and product_type).

Task 3: Determine Price Range by Product Type
I wrote a query to calculate the minimum and maximum prices for each product type, giving the team a clear understanding of price variation across product categories.

Task 4: Identify High-Selling Meat and Dairy Products
I filtered the products to find Meat and Dairy items where the average units sold exceeded 10, providing the team with valuable insights into top-performing products in these categories.
