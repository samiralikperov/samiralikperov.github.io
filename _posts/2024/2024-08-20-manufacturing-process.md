---
title: Manufacturing processes
date: 2024-09-10 18:00:00
layout: post
categories: [SQL]
tags: [DATA ANALYSIS,BUSINESS PROCESS]
image:
  path: assets/img/headers/post_3.png
  alt: Manufacturing processes
---

For this project, the goal is to support a team aiming to enhance their control and monitoring of a manufacturing process. This involves implementing Statistical Process Control (SPC), a data-driven methodology to ensure product quality and consistent manufacturing performance. The task is to analyze historical data and use SQL queries to calculate control limits—upper control limit (UCL) and lower control limit (LCL)—to identify any anomalies in the process that require attention.

This acceptable range is defined by an upper control limit (UCL) and a lower control limit (LCL), the formulas for which are:

![formula](assets/img/pictures/manproc_formula.png)

The UCL defines the highest acceptable height for the parts, while the LCL defines the lowest acceptable height for the parts. Ideally, parts should fall between the two limits.

To learn more about me and my work, expand the ABOUT ME section.

<details>
  <summary style="font-size: 24px; font-weight: bold; cursor: pointer;">ABOUT ME</summary>
  
  Learn more about me on my <a href="https://samiralikperov.github.io/about/" target="_blank">ABOUT</a> page. Below, you can find links to explore more of my projects categorized by topics, access my resume, and contact me.

  <br>

  | <a href="https://samiralikperov.github.io/categories/" target="_blank"><strong>PROJECTS</strong></a> | <a href="https://docs.google.com/document/d/1BEL5l5ZnlTdJc5OKiuH1SkiMQf6hS6HRAZUZvlrRANM/edit#heading=h.ifsro82jsgea" target="_blank"><strong>RESUME</strong></a> | <a href="https://www.linkedin.com/in/samiralikperov/" target="_blank"><strong>CONTACT</strong></a> |
</details>

## **Analyzing Manufacturing Process for SPC Implementation**

### **Situation:**
The team in charge of a manufacturing process wants to improve the way they monitor and control their workflow. They plan to introduce Statistical Process Control (SPC) to ensure that product quality remains consistent. The goal is to identify any anomalies in the product measurements (specifically height) that may require process adjustments.
The data is available in the `manufacturing_parts` table which has the following fields:
- `item_no`: the item number
- `length`: the length of the item made
- `width`: the width of the item made
- `height`: the height of the item made
- `operator`: the operating machine

### **Task:**
I was tasked with analyzing the historical manufacturing data using SQL to calculate the Upper Control Limit (UCL) and Lower Control Limit (LCL) based on height measurements. By applying these limits, I was responsible for determining if any items in the process fall outside the acceptable range and require attention.

### **Action:**
1. I created a SQL query to calculate the **average height** and **standard deviation** of height for each product, using **window functions** to capture measurements over a sliding window of 5 items.
2. I applied formulas to calculate the **Upper Control Limit (UCL)** and **Lower Control Limit (LCL)** for each group of 5 products.
3. I flagged any items where the height fell outside these control limits (greater than UCL or less than LCL) using a **CASE** statement.
4. I utilized SQL’s **nested queries** and **window functions** to handle the data efficiently and filter results for further analysis.

Here’s the SQL query used for this analysis:

```sql
WITH calculated_values AS (
    SELECT
        operator,
        item_no,
        height,
        AVG(height) OVER (PARTITION BY operator ORDER BY item_no ROWS BETWEEN 4 PRECEDING AND CURRENT ROW) AS avg_height,
        STDDEV(height) OVER (PARTITION BY operator ORDER BY item_no ROWS BETWEEN 4 PRECEDING AND CURRENT ROW) AS stddev_height,
        ROW_NUMBER() OVER (PARTITION BY operator ORDER BY item_no) AS row_number
    FROM
        manufacturing_parts
),
control_limits AS (
    SELECT
        operator,
        item_no,
        row_number,
        height,
        avg_height,
        stddev_height,
        (avg_height + 3 * stddev_height / SQRT(5)) AS ucl,
        (avg_height - 3 * stddev_height / SQRT(5)) AS lcl
    FROM
        calculated_values
    WHERE
        row_number >= 5 -- Exclude incomplete windows
),
alerts AS (
    SELECT
        operator,
        item_no,
        row_number,
        height,
        avg_height,
        stddev_height,
        ucl,
        lcl,
        CASE
            WHEN height > ucl OR height < lcl THEN TRUE
            ELSE FALSE
        END AS alert
    FROM
        control_limits
)
SELECT
    operator,
    row_number,
    height,
    avg_height,
    stddev_height,
    ucl,
    lcl,
    alert
FROM
    alerts
ORDER BY
    item_no;
```

### **Result:**
The query successfully calculated the control limits for each batch of 5 items and identified any items that exceeded the Upper Control Limit (UCL) or fell below the Lower Control Limit (LCL). This allowed the team to pinpoint which steps in the process require adjustments to ensure product quality remains within acceptable bounds.

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
      <th>operator</th>
      <th>row_number</th>
      <th>height</th>
      <th>avg_height</th>
      <th>stddev_height</th>
      <th>ucl</th>
      <th>lcl</th>
      <th>alert</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Op-1</td>
      <td>5</td>
      <td>19.46</td>
      <td>19.778</td>
      <td>1.062812</td>
      <td>21.203912</td>
      <td>18.352088</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Op-1</td>
      <td>6</td>
      <td>20.36</td>
      <td>19.912</td>
      <td>1.090812</td>
      <td>21.375477</td>
      <td>18.448523</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Op-1</td>
      <td>7</td>
      <td>20.22</td>
      <td>20.030</td>
      <td>1.084574</td>
      <td>21.485108</td>
      <td>18.574892</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Op-1</td>
      <td>8</td>
      <td>21.03</td>
      <td>19.934</td>
      <td>0.931225</td>
      <td>21.183369</td>
      <td>18.684631</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Op-1</td>
      <td>9</td>
      <td>19.78</td>
      <td>20.170</td>
      <td>0.598832</td>
      <td>20.973418</td>
      <td>19.366582</td>
      <td>False</td>
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
      <th>415</th>
      <td>Op-20</td>
      <td>17</td>
      <td>20.96</td>
      <td>20.370</td>
      <td>0.853698</td>
      <td>21.515356</td>
      <td>19.224644</td>
      <td>False</td>
    </tr>
    <tr>
      <th>416</th>
      <td>Op-20</td>
      <td>18</td>
      <td>19.68</td>
      <td>20.362</td>
      <td>0.861464</td>
      <td>21.517775</td>
      <td>19.206225</td>
      <td>False</td>
    </tr>
    <tr>
      <th>417</th>
      <td>Op-20</td>
      <td>19</td>
      <td>19.19</td>
      <td>20.098</td>
      <td>0.996454</td>
      <td>21.434883</td>
      <td>18.761117</td>
      <td>False</td>
    </tr>
    <tr>
      <th>418</th>
      <td>Op-20</td>
      <td>20</td>
      <td>21.60</td>
      <td>20.146</td>
      <td>1.075119</td>
      <td>21.588423</td>
      <td>18.703577</td>
      <td>True</td>
    </tr>
    <tr>
      <th>419</th>
      <td>Op-20</td>
      <td>21</td>
      <td>21.47</td>
      <td>20.580</td>
      <td>1.086163</td>
      <td>22.037241</td>
      <td>19.122759</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>420 rows × 8 columns</p>
</div>


## **Summary of Project**
This project focuses on monitoring and controlling a manufacturing process using Statistical Process Control (SPC). By applying SQL window functions and nested queries, I calculated the Upper Control Limit (UCL) and Lower Control Limit (LCL) for part measurements. The goal was to identify any deviations from the acceptable range to ensure high-quality production. Alerts were generated for parts exceeding the control limits, providing valuable insights for maintaining smooth and efficient manufacturing operations.
