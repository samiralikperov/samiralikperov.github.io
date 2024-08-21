---
title: Manufacturing processes
date: 2024-08-20 18:00:00
layout: post
categories: [SQL,EDA]
tags: [DATA ANALYSIS,BUSINESS PROCESS]
image:
  path: assets/img/headers/post_3.png
  alt: Manufacturing processes
---


## Manufacturing processes

Manufacturing processes for any product is like putting together a puzzle. Products are pieced together step by step, and keeping a close eye on the process is important.

For this project, you're supporting a team that wants to improve how they monitor and control a manufacturing process. The goal is to implement a more methodical approach known as statistical process control (SPC). SPC is an established strategy that uses data to determine whether the process works well. Processes are only adjusted if measurements fall outside of an acceptable range. 

This acceptable range is defined by an upper control limit (UCL) and a lower control limit (LCL), the formulas for which are:

$$
ucl = \text{avg\_height} + 3 \times \frac{\text{stddev\_height}}{\sqrt{5}}
$$

$$
lcl = \text{avg\_height} - 3 \times \frac{\text{stddev\_height}}{\sqrt{5}}
$$


The UCL defines the highest acceptable height for the parts, while the LCL defines the lowest acceptable height for the parts. Ideally, parts should fall between the two limits.

Using SQL window functions and nested queries, you'll analyze historical manufacturing data to define this acceptable range and identify any points in the process that fall outside of the range and therefore require adjustments. This will ensure a smooth running manufacturing process consistently making high-quality products.

## The data
The data is available in the `manufacturing_parts` table which has the following fields:
- `item_no`: the item number
- `length`: the length of the item made
- `width`: the width of the item made
- `height`: the height of the item made
- `operator`: the operating machine


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
