---
title: LeetCode Top 50 SQL Questions
date: 2024-09-30 18:00:00
layout: post
categories: [SQL]
tags: [DATA ANALYSIS]
image:
  path: assets/img/headers/post_5.png
  alt: LeetCode SQL - Top 50 SQL Questions
---


## LeetCode SQL - Top 50 SQL Questions

I decided to take on this challenge over a few days to test my knowledge. You can find the challenge at [this link](https://leetcode.com/studyplan/top-sql-50/). This collection showcases my solutions to a curated selection of SQL problems, ranging from beginner to intermediate levels. Each question covers various SQL concepts and queries to help solidify my understanding.


To learn more about me and my work, expand the ABOUT ME section.

<details>
  <summary style="font-size: 24px; font-weight: bold; cursor: pointer;">ABOUT ME</summary>
  
  Learn more about me on my <a href="https://samiralikperov.github.io/about/" target="_blank">ABOUT</a> page. Below, you can find links to explore more of my projects categorized by topics, access my resume, and contact me.

  <br>

  | <a href="https://samiralikperov.github.io/categories/" target="_blank"><strong>PROJECTS</strong></a> | <a href="https://docs.google.com/document/d/1BEL5l5ZnlTdJc5OKiuH1SkiMQf6hS6HRAZUZvlrRANM/edit#heading=h.ifsro82jsgea" target="_blank"><strong>RESUME</strong></a> | <a href="https://www.linkedin.com/in/samiralikperov/" target="_blank"><strong>CONTACT</strong></a> |
</details>


## What You'll Find Here

Each solution includes:

- **SQL Query**: The complete SQL code to solve the problem.
- **Explanation**: A brief summary of the approach and thought process behind the solution.


## SELECT

<details>
  <summary> 1757. Recyclable and Low Fat Products</summary> 

> **Table: Products**  
>   
> | Column Name | Type    |  
> |-------------|---------|  
> | product_id  | int     |  
> | low_fats    | enum    |  
> | recyclable  | enum    |  
>   
> - `product_id` is the primary key (column with unique values) for this table.  
> - `low_fats` is an `ENUM` (category) of type `('Y', 'N')` where `'Y'` means this product is low fat and `'N'` means it is not.  
> - `recyclable` is an `ENUM` (category) of types `('Y', 'N')` where `'Y'` means this product is recyclable and `'N'` means it is not.  
>   
> **Problem Statement:**  
> Write a solution to find the IDs of products that are both low fat and recyclable.  
> Return the result table in any order.  
> The result format is in the following example.  
> 
> **Solution:**
> 
> ```sql  
> SELECT  
>     product_id  
> FROM Products  
> WHERE low_fats = 'Y'   -- Filters only products that are low fat  
>   AND recyclable = 'Y'; -- Filters only products that are recyclable  
> ```  
>   
> **Output:**  
>   
> | product_id |  
> |------------|  
> | 1          |  
> | 3          |  
>   
> **Explanation:**  
> - The query selects the `product_id` from the `Products` table.  
> - It uses the `WHERE` clause to filter the rows where both `low_fats` and `recyclable` columns have the value `'Y'`.  
> - This ensures that only products that are both low fat and recyclable are returned.

</details>


<details>
  <summary> 584. Find Customer Referee</summary> 

> **Table: Customer**  
>   
> | Column Name | Type    |  
> |-------------|---------|  
> | id          | int     |  
> | name        | varchar |  
> | referee_id  | int     |  
>   
> - `id` is the primary key column for this table.  
> - Each row of this table indicates the `id` of a customer, their `name`, and the `id` of the customer who referred them.  
>   
> **Problem Statement:**  
> Find the names of the customers that are not referred by the customer with `id = 2`.  
> Return the result table in any order.  
> The result format is in the following example.  
> 
> **Solution:**
> 
> ```sql  
> SELECT name  
> FROM Customer  
> WHERE 1=1
> AND referee_id IS NULL -- Filters customers who were not referred
> OR referee_id != 2;  -- by customer with id = 2 or have no referee 
> ```  
>   
> **Output:**  
>   
> | name |  
> |------|  
> | Will |  
> | Jane |  
> | Bill |  
> | Zack |  
>   
> **Explanation:**  
> - The query selects the `name` from the `Customer` table.  
> - It uses the `WHERE` clause to filter rows where `referee_id` is either `NULL` or not equal to `2`.  
> - This ensures that only customers who are not referred by the customer with `id = 2` are returned.

</details>

<details>
  <summary> 595. Big Countries</summary> 

> **Table: World**  
>   
> | Column Name | Type    |  
> |-------------|---------|  
> | name        | varchar |  
> | continent   | varchar |  
> | area        | int     |  
> | population  | int     |  
> | gdp         | bigint  |  
>   
> - `name` is the primary key (column with unique values) for this table.  
> - Each row of this table gives information about the name of a country, the continent to which it belongs, its area, the population, and its GDP value.  
>   
> **Problem Statement:**  
> A country is considered big if:  
> - it has an area of at least three million (i.e., 3000000 km²), or  
> - it has a population of at least twenty-five million (i.e., 25000000).  
> Write a solution to find the `name`, `population`, and `area` of the big countries.  
> Return the result table in any order.  
> The result format is in the following example.  
> 
> **Solution:**
> 
> ```sql  
> SELECT name, population, area  
> FROM World  
> WHERE area >= 3000000 OR population >= 25000000;  -- Filters countries with area >= 3,000,000 or population >= 25,000,000  
> ```  
>   
> **Output:**  
>   
> | name        | population | area    |  
> |-------------|------------|---------|  
> | Afghanistan | 25500100   | 652230  |  
> | Algeria     | 37100000   | 2381741 |  
>   
> **Explanation:**  
> - The query selects the `name`, `population`, and `area` from the `World` table.  
> - It uses the `WHERE` clause to filter the rows where either `area` is greater than or equal to `3000000` or `population` is greater than or equal to `25000000`.  
> - This ensures that only the big countries are returned.

</details>

<details>
  <summary> 1148. Article Views I</summary> 

> **Table: Views**  
>   
> | Column Name | Type    |  
> |-------------|---------|  
> | article_id  | int     |  
> | author_id   | int     |  
> | viewer_id   | int     |  
> | view_date   | date    |  
>   
> - There is no primary key (column with unique values) for this table; the table may have duplicate rows.  
> - Each row of this table indicates that some viewer viewed an article (written by some author) on some date.  
> - Note that equal `author_id` and `viewer_id` indicate the same person.  
>   
> **Problem Statement:**  
> Write a solution to find all the authors that viewed at least one of their own articles.  
> Return the result table sorted by `id` in ascending order.  
> The result format is in the following example.
> 
> **Solution:**
> 
> ```sql  
> SELECT DISTINCT author_id AS id  
> FROM Views  
> WHERE author_id = viewer_id  -- Filters rows where the author viewed their own article  
> ORDER BY author_id;  -- Orders the result by id in ascending order  
> ```  
>   
> **Output:**  
>   
> | id |  
> |----|  
> | 4  |  
> | 7  |  
>   
> **Explanation:**  
> - The query selects distinct `author_id` from the `Views` table where the `author_id` is the same as `viewer_id`.  
> - This condition checks if an author has viewed their own article.  
> - The result is sorted in ascending order by `id`.

</details>

<details>
  <summary> 1683. Invalid Tweets</summary> 

> **Table: Tweets**  
>   
> | Column Name | Type    |  
> |-------------|---------|  
> | tweet_id    | int     |  
> | content     | varchar |  
>   
> - `tweet_id` is the primary key (column with unique values) for this table.  
> - This table contains all the tweets in a social media app.  
>   
> **Problem Statement:**  
> Write a solution to find the IDs of the invalid tweets. A tweet is considered invalid if the number of characters used in the content of the tweet is strictly greater than 15.  
> Return the result table in any order.  
> The result format is in the following example.
> 
> **Solution:**
> 
> ```sql  
> SELECT tweet_id  
> FROM Tweets  
> WHERE LENGTH(content) > 15;  -- Filters tweets where the content length is greater than 15 characters  
> ```  
>   
> **Output:**  
>   
> | tweet_id |  
> |----------|  
> | 2        |  
>   
> **Explanation:**  
> - The query selects `tweet_id` from the `Tweets` table where the length of `content` is greater than 15 characters.  
> - This ensures that only the IDs of invalid tweets are returned.

</details>


## BASIC JOINS

<details>
  <summary> 1378. Replace Employee ID With The Unique Identifier</summary> 

> **Table: Employees**  
>   
> | Column Name | Type    |  
> |-------------|---------|  
> | id          | int     |  
> | name        | varchar |  
>   
> - `id` is the primary key (column with unique values) for this table.  
> - Each row of this table contains the `id` and the `name` of an employee in a company.  
>   
> **Table: EmployeeUNI**  
>   
> | Column Name | Type    |  
> |-------------|---------|  
> | id          | int     |  
> | unique_id   | int     |  
>   
> - `(id, unique_id)` is the primary key (combination of columns with unique values) for this table.  
> - Each row of this table contains the `id` and the corresponding `unique_id` of an employee in the company.  
>   
> **Problem Statement:**  
> Write a solution to show the `unique_id` of each user. If a user does not have a `unique_id`, show `null`.  
> Return the result table in any order.  
> The result format is in the following example.
> 
> **Solution:**
> 
> ```sql  
> SELECT u.unique_id, e.name  
> FROM Employees AS e  
> LEFT JOIN EmployeeUNI AS u ON u.id = e.id;  -- Joins the tables on employee id and retrieves unique_id; null if not found  
> ```  
>   
> **Output:**  
>   
> | unique_id | name     |  
> |-----------|----------|  
> | null      | Alice    |  
> | null      | Bob      |  
> | 2         | Meir     |  
> | 3         | Winston  |  
> | 1         | Jonathan |  
>   
> **Explanation:**  
> - The query performs a `LEFT JOIN` between the `Employees` and `EmployeeUNI` tables on the `id` column.  
> - It selects the `unique_id` and `name` of each employee. If an employee does not have a `unique_id`, the result is `null`.

</details>

<details>
  <summary>1068. Product Sales Analysis I</summary>  

> **Table: Sales**  
>  
> | Column Name | Type  |  
> |-------------|-------|  
> | sale_id     | int   |  
> | product_id  | int   |  
> | year        | int   |  
> | quantity    | int   |  
> | price       | int   |  
>  
> (sale_id, year) is the primary key (combination of columns with unique values) of this table.  
> product_id is a foreign key (reference column) to Product table.  
> Each row of this table shows a sale on the product product_id in a certain year.  
> Note that the price is per unit.  
>  
> **Table: Product**  
>  
> | Column Name  | Type    |  
> |--------------|---------|  
> | product_id   | int     |  
> | product_name | varchar |  
>  
> product_id is the primary key (column with unique values) of this table.  
> Each row of this table indicates the product name of each product.  
>  
> **Problem Statement:**  
> Write a solution to report the product_name, year, and price for each sale_id in the Sales table.  
> Return the resulting table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT p.product_name, s.year, s.price  
> FROM sales as s  
> LEFT JOIN product as p ON p.product_id = s.product_id;  
> ```  
>  
> **Output:**  
>  
> | product_name | year | price |  
> | ------------ | ---- | ----- |  
> | Nokia        | 2008 | 5000  |  
> | Nokia        | 2009 | 5000  |  
> | Apple        | 2011 | 9000  |  
>  
> **Explanation:**  
> - The query retrieves the product name, the year of the sale, and the price for each sale from the Sales table.  
> - A LEFT JOIN is used to join the Product table with the Sales table on product_id.  
> - The result includes all records from Sales and matches the corresponding product name from Product.
> 
</details>

<details>
  <summary>1581. Customer Who Visited but Did Not Make Any Transactions</summary>  

> **Table: Visits**  
>  
> | Column Name | Type    |  
> |-------------|---------|  
> | visit_id    | int     |  
> | customer_id | int     |  
>  
> visit_id is the column with unique values for this table.  
> This table contains information about the customers who visited the mall.  
>  
> **Table: Transactions**  
>  
> | Column Name    | Type    |  
> |----------------|---------|  
> | transaction_id | int     |  
> | visit_id       | int     |  
> | amount         | int     |  
>  
> transaction_id is the column with unique values for this table.  
> This table contains information about the transactions made during the visit_id.  
>  
> **Problem Statement:**  
> Write a solution to find the IDs of the users who visited without making any transactions and the number of times they made these types of visits.  
> Return the result table sorted in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT v.customer_id, COUNT(v.visit_id) as count_no_trans  
> FROM visits as v  
> LEFT JOIN transactions as t ON v.visit_id = t.visit_id  
> WHERE t.visit_id IS NULL  
> GROUP BY v.customer_id;  
> ```  
>  
> **Output:**  
>  
> | customer_id | count_no_trans |  
> | ----------- | -------------- |  
> | 30          | 1              |  
> | 54          | 2              |  
> | 96          | 1              |  
>  
> **Explanation:**  
> - The query joins the `Visits` table with the `Transactions` table using a LEFT JOIN to keep all visits, even if no transaction was made.  
> - The `WHERE t.visit_id IS NULL` clause filters out any visits that had a transaction.  
> - The result is grouped by customer_id, and the COUNT function calculates how many visits did not result in a transaction.  
> 
</details>


<details>
  <summary>197. Rising Temperature</summary>  

> **Table: Weather**  
>  
> | Column Name   | Type    |  
> |---------------|---------|  
> | id            | int     |  
> | recordDate    | date    |  
> | temperature   | int     |  
>  
> id is the column with unique values for this table.  
> There are no different rows with the same recordDate.  
> This table contains information about the temperature on a certain day.  
>  
> **Problem Statement:**  
> Write a solution to find all dates' `id` with higher temperatures compared to the previous day.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT w1.id  
> FROM weather as w1  
> JOIN weather as w2 ON w1.recordDate = DATEADD(DAY, 1, w2.recordDate)  
> WHERE w1.temperature > w2.temperature;  
> ```  
>  
> **Output:**  
>  
> | id |  
> |----|  
> | 2  |  
> | 4  |  
>  
> **Explanation:**  
> - The query joins the `Weather` table with itself to compare the temperature on each day with the temperature of the previous day.  
> - The `DATEADD(DAY, 1, w2.recordDate)` condition ensures that we are comparing consecutive days.  
> - The `WHERE w1.temperature > w2.temperature` filters out the rows where the temperature of the current day is lower or equal to the previous day, and only selects those where the temperature increased.
>  
</details>


<details>
  <summary>1661. Average Time of Process per Machine</summary> 

> **Table: Activity**  
>   
> | Column Name    | Type    |  
> |----------------|---------|  
> | machine_id     | int     |  
> | process_id     | int     |  
> | activity_type  | enum    |  
> | timestamp      | float   |  
>   
> - `machine_id`: The ID of the machine.  
> - `process_id`: The ID of the process running on the machine.  
> - `activity_type`: An ENUM of either `'start'` or `'end'`, indicating the beginning and end of a process.  
> - `timestamp`: A float representing the time in seconds when the activity occurred.  
> 
> **Problem Statement:**  
> Write a solution to find the average time each machine takes to complete a process.  
> The time for a process is calculated by subtracting the 'start' timestamp from the 'end' timestamp.  
> The result should include the `machine_id` and the average processing time, rounded to 3 decimal places.
> 
> **Solution:**
> 
> ```sql
> SELECT 
>     a.machine_id, 
>     ROUND(AVG(b.timestamp - a.timestamp), 3) AS processing_time
> FROM 
>     Activity AS a
> JOIN 
>     Activity AS b
> ON 
>     a.machine_id = b.machine_id
>     AND a.process_id = b.process_id
>     AND a.activity_type = 'start'
>     AND b.activity_type = 'end'
> GROUP BY 
>     a.machine_id;
> ```
> **Output:**  
> | machine_id | processing_time |
> | ---------- | --------------- |
> | 0          | 0.894           |
> | 1          | 0.995           |
> | 2          | 1.456           |
>
> **Explanation:**
>
> The query joins the Activity table to itself to match the start and end times of each process.
> It then calculates the time difference between the 'start' and 'end' timestamps for each process.
> The result is grouped by machine_id, and the average processing time for each machine is returned.
> The AVG function calculates the average processing time for each machine, and the ROUND function rounds it to 3 decimal places.

</details>

<details>
  <summary>577. Employee Bonus</summary> 

> **Table: Employee**  
>   
> | Column Name | Type    |  
> |-------------|---------|  
> | empId       | int     |  
> | name        | varchar |  
> | supervisor  | int     |  
> | salary      | int     |  
>   
> - `empId` is the primary key of the Employee table.  
> - Each row represents an employee's information, including their `empId`, `name`, `supervisor`, and `salary`.  

> **Table: Bonus**  
>   
> | Column Name | Type    |  
> |-------------|---------|  
> | empId       | int     |  
> | bonus       | int     |  
>   
> - `empId` is the primary key of the Bonus table and a foreign key to `empId` in the Employee table.  
> - Each row represents the `empId` of an employee and their respective `bonus`.  
>   
> **Problem Statement:**  
> Write a query to return the `name` and `bonus` of each employee whose bonus is less than 1000 or is missing (null).  
> Return the result in any order.
> 
> **Solution:**
> 
> ```sql
> SELECT 
>     e.name, 
>     b.bonus 
> FROM 
>     Employee e
> LEFT JOIN 
>     Bonus b ON e.empID = b.empID
> WHERE 
>     b.bonus < 1000 OR b.bonus IS NULL;
> ```
> **Output:**  
> | name | bonus |
> | ---- | ----- |
> | Brad | null  |
> | John | null  |
> | Dan  | 500   |
>
> **Explanation:**  
> The query performs a LEFT JOIN between the Employee and Bonus tables to ensure that all employees are included, even if they have no bonus (null).  
> The WHERE clause filters the employees who either have a bonus less than 1000 or no bonus at all.  
> The result returns the names of employees along with their respective bonus amounts, or null if they have no bonus.  


</details>


<details>
  <summary>1280. Students and Examinations</summary>

> **Table: Students**  
> 
> | Column Name   | Type    |  
> |---------------|---------|  
> | student_id    | int     |  
> | student_name  | varchar |  
> 
> - `student_id` is the primary key (column with unique values) for this table.  
> - Each row of this table contains the ID and the name of one student in the school.  
> 
> **Table: Subjects**  
> 
> | Column Name  | Type    |  
> |--------------|---------|  
> | subject_name | varchar |  
> 
> - `subject_name` is the primary key (column with unique values) for this table.  
> - Each row of this table contains the name of one subject in the school.  
> 
> **Table: Examinations**  
> 
> | Column Name  | Type    |  
> |--------------|---------|  
> | student_id   | int     |  
> | subject_name | varchar |  
> 
> - There is no primary key (column with unique values) for this table. It may contain duplicates.  
> - Each student from the `Students` table takes every course from the `Subjects` table.  
> - Each row of this table indicates that a student with ID `student_id` attended the exam of `subject_name`.  
> 
> **Problem Statement:**  
> Write a solution to find the number of times each student attended each exam.  
> Return the result table ordered by `student_id` and `subject_name`.  
> 
> **Solution:**  
> 
> ```sql
> WITH att AS (
>     SELECT st.student_id, st.student_name, sb.subject_name
>     FROM Students AS st
>     CROSS JOIN Subjects AS sb
> )
> SELECT a.student_id, a.student_name, a.subject_name, COUNT(e.subject_name) AS attended_exams
> FROM att a
> LEFT JOIN Examinations AS e
>     ON a.student_id = e.student_id
>     AND a.subject_name = e.subject_name
> GROUP BY a.student_id, a.subject_name
> ORDER BY a.student_id, a.subject_name;
> ```  
> 
> **Output:**  
> 
> | student_id | student_name | subject_name | attended_exams |  
> |------------|--------------|--------------|----------------|  
> | 1          | Alice        | Math         | 3              |  
> | 1          | Alice        | Physics      | 2              |  
> | 1          | Alice        | Programming  | 1              |  
> | 2          | Bob          | Math         | 1              |  
> | 2          | Bob          | Physics      | 0              |  
> | 2          | Bob          | Programming  | 1              |  
> | 6          | Alex         | Math         | 0              |  
> | 6          | Alex         | Physics      | 0              |  
> | 6          | Alex         | Programming  | 0              |  
> | 13         | John         | Math         | 1              |  
> | 13         | John         | Physics      | 1              |  
> | 13         | John         | Programming  | 1              |
>
> 
> **Explanation:**  
> CROSS JOIN: We perform a CROSS JOIN between the Students and Subjects tables to ensure that each student is paired with every subject, even if they haven't attended any exams for that subject.  
> LEFT JOIN: We then use a LEFT JOIN to link this full list of student-subject combinations to the Examinations table. This ensures that even if a student has not attended an exam for a subject, the student-subject pair will still appear in the results.  
> COUNT: We count how many times each student attended the exam for each subject using COUNT(e.subject_name). If a student did not attend an exam for a particular subject, the count will be 0.  
> GROUP BY and ORDER BY: Finally, we group the results by student_id and subject_name to aggregate the exam attendances, and order the output by these columns for a clear and organized result.  

</details>


<details>
  <summary>570. Managers with at Least 5 Direct Reports</summary>  

> **Table: Employee**  
>  
> | Column Name | Type    |  
> |-------------|---------|  
> | id          | int     |  
> | name        | varchar |  
> | department  | varchar |  
> | managerId   | int     |  
>  
> id is the primary key (column with unique values) for this table.  
> Each row of this table indicates the name of an employee, their department, and the id of their manager.  
> If managerId is null, then the employee does not have a manager.  
> No employee will be the manager of themself.  
>  
> **Problem Statement:**  
> Write a solution to find managers with at least five direct reports.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT m.name  
> FROM Employee as m  
> LEFT JOIN Employee as e   
>     ON m.id = e.managerId  
> GROUP BY e.managerId  
> HAVING COUNT(e.managerId) >= 5;  
> ```  
>  
> **Output:**  
>  
> | name |  
> |------|  
> | John |  
>  
> **Explanation:**  
> - The query joins the `Employee` table with itself to count how many employees each manager (identified by `managerId`) directly supervises.  
> - We group the results by `e.managerId` and use the `HAVING` clause to filter only those managers who have 5 or more direct reports.  
> - The `LEFT JOIN` ensures that managers with no employees reporting to them are still considered, though filtered out by the `HAVING` clause.  
>  
</details>


<details>
  <summary>1934. Confirmation Rate</summary>  

> **Table: Signups**  
>  
> | Column Name | Type     |  
> |-------------|----------|  
> | user_id     | int      |  
> | time_stamp  | datetime |  
>  
> user_id is the column of unique values for this table.  
> Each row contains information about the signup time for the user with ID user_id.  
>  
> **Table: Confirmations**  
>  
> | Column Name | Type     |  
> |-------------|----------|  
> | user_id     | int      |  
> | time_stamp  | datetime |  
> | action      | ENUM     |  
>  
> (user_id, time_stamp) is the primary key (combination of columns with unique values) for this table.  
> user_id is a foreign key (reference column) to the Signups table.  
> action is an ENUM (category) of the type ('confirmed', 'timeout').  
> Each row of this table indicates that the user with ID user_id requested a confirmation message at time_stamp and that confirmation message was either confirmed ('confirmed') or expired without confirming ('timeout').  
>  
> **Problem Statement:**  
> The confirmation rate of a user is the number of 'confirmed' messages divided by the total number of requested confirmation messages. The confirmation rate of a user that did not request any confirmation messages is 0.  
> Round the confirmation rate to two decimal places.  
> Write a solution to find the confirmation rate of each user.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT s.user_id  
>       ,ROUND(SUM(CASE WHEN c.action = 'confirmed' THEN 1 ELSE 0 END)/COUNT(*), 2) as confirmation_rate  
> FROM Signups as s  
> LEFT JOIN Confirmations as c  
>     ON s.user_id = c.user_id  
> GROUP BY s.user_id;  
> ```  
>  
> **Output:**  
>  
> | user_id | confirmation_rate |  
> |---------|-------------------|  
> | 3       | 0                 |  
> | 7       | 1                 |  
> | 2       | 0.5               |  
> | 6       | 0                 |  
>  
> **Explanation:**  
> - The query joins the `Signups` table with the `Confirmations` table to count how many confirmation requests each user made and how many were successfully confirmed.  
> - For each user, we calculate the confirmation rate by dividing the number of 'confirmed' messages by the total number of confirmation messages.  
> - The `LEFT JOIN` ensures that users who did not request any confirmation messages still appear with a confirmation rate of 0.
>  
</details>




## BASIC AGGREGATE FUNCTIONS

  <details>
  <summary>620. Not Boring Movies</summary>  

> **Table: Cinema**  
>  
> | Column Name | Type     |  
> |-------------|----------|  
> | id          | int      |  
> | movie       | varchar  |  
> | description | varchar  |  
> | rating      | float    |  
>  
> id is the primary key (column with unique values) for this table.  
> Each row contains information about the name of a movie, its genre, and its rating.  
> rating is a 2 decimal places float in the range [0, 10].  
>  
> **Problem Statement:**  
> Write a solution to report the movies with an odd-numbered ID and a description that is not "boring".  
> Return the result table ordered by `rating` in descending order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT *  
> FROM Cinema  
> WHERE id % 2 = 1 AND description != 'boring'  
> ORDER BY rating DESC;  
> ```  
>  
> **Output:**  
>  
> | id | movie      | description | rating |  
> |----|------------|-------------|--------|  
> | 5  | House card | Interesting | 9.1    |  
> | 1  | War        | great 3D    | 8.9    |  
>  
> **Explanation:**  
> - The query selects all movies with an odd-numbered ID using the condition `id % 2 = 1`.  
> - It filters out movies where the `description` is "boring" and orders the results by `rating` in descending order.
>  
</details>


<details>
  <summary>1251. Average Selling Price</summary>  

> **Table: Prices**  
>  
> | Column Name | Type  |  
> |-------------|-------|  
> | product_id  | int   |  
> | start_date  | date  |  
> | end_date    | date  |  
> | price       | int   |  
>  
> (product_id, start_date, end_date) is the primary key (combination of columns with unique values) for this table.  
> Each row of this table indicates the price of the product_id in the period from `start_date` to `end_date`.  
> For each product_id there will be no two overlapping periods.  
>  
> **Table: UnitsSold**  
>  
> | Column Name   | Type |  
> |---------------|------|  
> | product_id    | int  |  
> | purchase_date | date |  
> | units         | int  |  
>  
> This table may contain duplicate rows.  
> Each row of this table indicates the date, units, and product_id of each product sold.  
>  
> **Problem Statement:**  
> Write a solution to find the average selling price for each product. `average_price` should be rounded to 2 decimal places.  
> If a product does not have any sold units, its average selling price is assumed to be 0.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT p.product_id  
>      , IFNULL(ROUND(SUM(p.price * u.units)/SUM(u.units),2),0) as average_price  
> FROM Prices as p  
> LEFT JOIN UnitsSold as u  
>     ON p.product_id = u.product_id  
>     AND u.purchase_date BETWEEN p.start_date AND p.end_date  
> GROUP BY p.product_id;  
> ```  
>  
> **Output:**  
>  
> | product_id | average_price |  
> |------------|---------------|  
> | 1          | 6.96          |  
> | 2          | 16.96         |  
>  
> **Explanation:**  
> - The query calculates the average selling price for each product by multiplying the price by the units sold for the respective date range and dividing by the total number of units sold.  
> - The `IFNULL` ensures that if no units were sold for a product, the `average_price` is set to 0.  
> - The query uses `LEFT JOIN` to ensure that even products without any sold units are included in the result.  
>  
</details>


<details>
  <summary>1075. Project Employees I</summary>  

> **Table: Project**  
>  
> | Column Name | Type |  
> |-------------|------|  
> | project_id  | int  |  
> | employee_id | int  |  
>  
> (project_id, employee_id) is the primary key of this table.  
> employee_id is a foreign key to the Employee table.  
> Each row of this table indicates that the employee with employee_id is working on the project with project_id.  
>  
> **Table: Employee**  
>  
> | Column Name      | Type    |  
> |------------------|---------|  
> | employee_id      | int     |  
> | name             | varchar |  
> | experience_years | int     |  
>  
> employee_id is the primary key of this table. It's guaranteed that experience_years is not NULL.  
> Each row of this table contains information about one employee.  
>  
> **Problem Statement:**  
> Write an SQL query that reports the average experience years of all the employees for each project, rounded to 2 digits.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT p.project_id  
>      , ROUND(AVG(e.experience_years), 2) as average_years  
> FROM Project as p  
> LEFT JOIN Employee as e  
>     ON p.employee_id = e.employee_id  
> GROUP BY p.project_id;  
> ```  
>  
> **Output:**  
>  
> | project_id | average_years |  
> |------------|---------------|  
> | 1          | 2             |  
> | 2          | 2.5           |  
>  
> **Explanation:**  
> - The query joins the `Project` table with the `Employee` table using `employee_id`.  
> - For each project, the query calculates the average experience years of all employees working on that project, rounded to two decimal places.  
> - The `GROUP BY` clause ensures that the results are grouped by `project_id`.  
>  
</details>

<details>
  <summary>1633. Percentage of Users Attended a Contest</summary>  

> **Table: Users**  
>  
> | Column Name | Type    |  
> |-------------|---------|  
> | user_id     | int     |  
> | user_name   | varchar |  
>  
> user_id is the primary key (column with unique values) for this table.  
> Each row of this table contains the name and the id of a user.  
>  
> **Table: Register**  
>  
> | Column Name | Type    |  
> |-------------|---------|  
> | contest_id  | int     |  
> | user_id     | int     |  
>  
> (contest_id, user_id) is the primary key (combination of columns with unique values) for this table.  
> Each row of this table contains the id of a user and the contest they registered into.  
>  
> **Problem Statement:**  
> Write a solution to find the percentage of the users registered in each contest rounded to two decimals.  
> Return the result table ordered by percentage in descending order. In case of a tie, order it by contest_id in ascending order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT contest_id  
>      , ROUND(COUNT(r.user_id)*100 / (SELECT COUNT(*) FROM Users), 2) as percentage  
> FROM Register as r  
> GROUP BY contest_id  
> ORDER BY percentage DESC, contest_id ASC;  
> ```  
>  
> **Output:**  
>  
> | contest_id | percentage |  
> |------------|------------|  
> | 208        | 100        |  
> | 209        | 100        |  
> | 210        | 100        |  
> | 215        | 66.67      |  
> | 207        | 33.33      |  
>  
> **Explanation:**  
> - The query calculates the number of users registered for each contest (`COUNT(r.user_id)`) and divides it by the total number of users (`COUNT(*) FROM Users`) to get the percentage.  
> - `ROUND` is used to round the result to two decimal places.  
> - The `GROUP BY` ensures that the results are grouped by `contest_id`, and the `ORDER BY` clause sorts the result by `percentage` in descending order and `contest_id` in ascending order for ties.  
>  
</details>

<details>
  <summary>1211. Queries Quality and Percentage</summary>  

> **Table: Queries**  
>  
> | Column Name | Type    |  
> |-------------|---------|  
> | query_name  | varchar |  
> | result      | varchar |  
> | position    | int     |  
> | rating      | int     |  
>  
> This table may have duplicate rows.  
> This table contains information collected from some queries on a database.  
> The `position` column has a value from 1 to 500.  
> The `rating` column has a value from 1 to 5. Query with rating less than 3 is a poor query.  
>  
> We define query quality as:  
> - The average of the ratio between query rating and its position.  
>  
> We also define poor query percentage as:  
> - The percentage of all queries with rating less than 3.  
>  
> **Problem Statement:**  
> Write a solution to find each `query_name`, the `quality` and `poor_query_percentage`.  
> Both `quality` and `poor_query_percentage` should be rounded to 2 decimal places.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT query_name  
>      , ROUND(SUM(rating / position) / COUNT(*), 2) as quality  
>      , ROUND(SUM(CASE WHEN rating < 3 THEN 1 ELSE 0 END) * 100 / COUNT(*), 2) as poor_query_percentage  
> FROM Queries  
> WHERE query_name IS NOT NULL  
> GROUP BY query_name;  
> ```  
>  
> **Output:**  
>  
> | query_name | quality | poor_query_percentage |  
> |------------|---------|-----------------------|  
> | Dog        | 2.50    | 33.33                 |  
> | Cat        | 0.66    | 33.33                 |  
>  
> **Explanation:**  
> - The query calculates the `quality` by summing the ratio of `rating / position` for each query and dividing by the number of queries.  
> - The `poor_query_percentage` is the percentage of queries with a `rating` less than 3.  
> - `ROUND` is used to round the values to two decimal places, and `GROUP BY` ensures that the result is grouped by `query_name`.  
>  
</details>

<details>
  <summary>1193. Monthly Transactions I</summary>  

> **Table: Transactions**  
>  
> | Column Name   | Type    |  
> |---------------|---------|  
> | id            | int     |  
> | country       | varchar |  
> | state         | enum    |  
> | amount        | int     |  
> | trans_date    | date    |  
>  
> id is the primary key of this table.  
> The table has information about incoming transactions.  
> The `state` column is an enum of type ['approved', 'declined'].  
>  
> **Problem Statement:**  
> Write an SQL query to find for each month and country, the number of transactions and their total amount, the number of approved transactions and their total amount.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT  
>   DATE_FORMAT(trans_date, '%Y-%m') AS MONTH  
> , country  
> , COUNT(*) AS trans_count  
> , SUM(  
>     CASE  
>       WHEN state = 'approved' THEN 1  
>       ELSE 0  
>     END  
>   ) AS approved_count  
> , SUM(amount) AS trans_total_amount  
> , SUM(  
>     CASE  
>       WHEN state = 'approved' THEN amount  
>       ELSE 0  
>     END  
>   ) AS approved_total_amount  
> FROM  
>   Transactions  
> GROUP BY 1, 2;  
> ```  
>  
> **Output:**  
>  
> | MONTH   | country | trans_count | approved_count | trans_total_amount | approved_total_amount |  
> |---------|---------|-------------|----------------|--------------------|-----------------------|  
> | 2018-12 | US      | 2           | 1              | 3000               | 1000                  |  
> | 2019-01 | US      | 1           | 1              | 2000               | 2000                  |  
> | 2019-01 | DE      | 1           | 1              | 2000               | 2000                  |  
>  
> **Explanation:**  
> - The query groups transactions by month and country using `DATE_FORMAT` to extract the year and month from `trans_date`.  
> - It counts the total number of transactions (`trans_count`) and approved transactions (`approved_count`) by using `SUM(CASE)` conditions.  
> - The total transaction amount and approved transaction amount are calculated similarly using the `SUM()` function.  
>  
</details>

<details>
  <summary>1174. Immediate Food Delivery II</summary>  

> **Table: Delivery**  
>  
> | Column Name                 | Type    |  
> |-----------------------------|---------|  
> | delivery_id                 | int     |  
> | customer_id                 | int     |  
> | order_date                  | date    |  
> | customer_pref_delivery_date | date    |  
>  
> delivery_id is the column of unique values for this table.  
> The table holds information about food deliveries where customers place orders on a certain date and specify a preferred delivery date (either on the order date or later).  
>  
> **Problem Statement:**  
> If the customer's preferred delivery date is the same as the order date, then the order is called immediate; otherwise, it is scheduled.  
> The first order of a customer is the one with the earliest order date.  
> Write a solution to find the percentage of immediate orders in the first orders of all customers, rounded to 2 decimal places.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT  
>   ROUND(  
>     SUM(  
>       CASE  
>         WHEN order_date = customer_pref_delivery_date THEN 1  
>         ELSE 0  
>       END  
>     ) * 100 / COUNT(customer_id)  
>   , 2  
>   ) AS immediate_percentage  
> FROM  
>   Delivery  
> WHERE  
>   (customer_id, order_date) IN (  
>     SELECT  
>       customer_id  
>     , MIN(order_date) AS first_order  
>     FROM  
>       Delivery  
>     GROUP BY  
>       customer_id  
>   );  
> ```  
>  
> **Output:**  
>  
> | immediate_percentage |  
> |----------------------|  
> | 50                   |  
>  
> **Explanation:**  
> - The query calculates the percentage of immediate orders among the first orders for all customers.  
> - A subquery identifies the first order for each customer by selecting the minimum `order_date`.  
> - The main query checks if the `order_date` matches the `customer_pref_delivery_date` to determine if the order is immediate.  
> - The percentage is then calculated by dividing the number of immediate orders by the total number of first orders and rounding the result to 2 decimal places.  
>  
</details>

<details>
  <summary>550. Game Play Analysis IV</summary>  

> **Table: Activity**  
>  
> | Column Name  | Type    |  
> |--------------|---------|  
> | player_id    | int     |  
> | device_id    | int     |  
> | event_date   | date    |  
> | games_played | int     |  
>  
> (player_id, event_date) is the primary key (combination of columns with unique values) of this table.  
> This table shows the activity of players of some games.  
> Each row is a record of a player who logged in and played a number of games (possibly 0) before logging out on a specific day using some device.  
>  
> **Problem Statement:**  
> Write a solution to report the fraction of players that logged in again on the day after their first login day, rounded to 2 decimal places.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT  
>   ROUND(  
>     COUNT(player_id) / (  
>       SELECT  
>         COUNT(DISTINCT player_id)  
>       FROM  
>         Activity  
>     )  
>   , 2  
>   ) AS fraction  
> FROM  
>   Activity  
> WHERE  
>   (player_id, event_date) IN (  
>     SELECT  
>       player_id  
>     , ADDDATE(MIN(event_date), INTERVAL 1 DAY)  
>     FROM  
>       Activity  
>     GROUP BY  
>       player_id  
>   );  
> ```  
>  
> **Output:**  
>  
> | fraction |  
> |----------|  
> | 0.33     |  
>  
> **Explanation:**  
> - The query calculates the fraction of players who logged in again on the day after their first login.  
> - The subquery in the `WHERE` clause identifies the first login day for each player using `MIN(event_date)` and checks if the player logged in again on the following day.  
> - The `COUNT` function in the outer query counts the number of players who logged in on two consecutive days, and this is divided by the total number of distinct players in the `Activity` table.  
> - The result is rounded to 2 decimal places to match the expected output.  
>  
</details>

















## SORTING AND GROUPING

<details>
  <summary>2356. Number of Unique Subjects Taught by Each Teacher</summary>  

> **Table: Teacher**  
>  
> | Column Name | Type |  
> |-------------|------|  
> | teacher_id  | int  |  
> | subject_id  | int  |  
> | dept_id     | int  |  
>  
> (subject_id, dept_id) is the primary key (combinations of columns with unique values) of this table.  
> Each row in this table indicates that the teacher with teacher_id teaches the subject subject_id in the department dept_id.  
>  
> **Problem Statement:**  
> Write a solution to calculate the number of unique subjects each teacher teaches in the university.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT t.teacher_id, COUNT(DISTINCT t.subject_id) as cnt  
> FROM Teacher as t  
> GROUP BY t.teacher_id;  
> ```  
>  
> **Output:**  
>  
> | teacher_id | cnt |  
> |------------|-----|  
> | 1          | 2   |  
> | 2          | 4   |  
>  
> **Explanation:**  
> - The query groups the data by `teacher_id` to count how many distinct `subject_id` values each teacher is teaching.  
> - The `DISTINCT` keyword ensures that only unique subjects are counted for each teacher.  
> - The `COUNT` function calculates the number of distinct subjects per teacher, and the results are grouped by each teacher's ID.  
>  
</details>

<details>
  <summary>1141. User Activity for the Past 30 Days I</summary>  

> **Table: Activity**  
>  
> | Column Name   | Type    |  
> |---------------|---------|  
> | user_id       | int     |  
> | session_id    | int     |  
> | activity_date | date    |  
> | activity_type | enum    |  
>  
> This table may have duplicate rows.  
> The `activity_type` column is an ENUM (category) of type ('open_session', 'end_session', 'scroll_down', 'send_message').  
> The table shows the user activities for a social media website. Each session belongs to exactly one user.  
>  
> **Problem Statement:**  
> Write a solution to find the daily active user count for a period of 30 days ending 2019-07-27 inclusively. A user was active on any day if they made at least one activity on that day.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT activity_date AS DAY,  
>        count(DISTINCT user_id) AS active_users  
> FROM Activity  
> WHERE activity_date > date_sub('2019-07-27', interval 30 DAY)  
>   AND activity_date <= '2019-07-27'  
> GROUP BY DAY;  
> ```  
>  
> **Output:**  
>  
> | DAY        | active_users |  
> |------------|--------------|  
> | 2019-07-20 | 2            |  
> | 2019-07-21 | 2            |  
>  
> **Explanation:**  
> - The query retrieves data for a 30-day window ending on '2019-07-27'.  
> - For each day in the given period, the query counts the distinct number of users (`user_id`) who were active on that day.  
> - The `GROUP BY` ensures that we get the user count for each specific day (`activity_date`).  
> - The `DISTINCT` keyword ensures that each user is counted only once per day, regardless of the number of activities.  
>  
</details>


<details>
  <summary>1070. Product Sales Analysis III</summary>  

> **Table: Sales**  
>  
> | Column Name | Type  |  
> |-------------|-------|  
> | sale_id     | int   |  
> | product_id  | int   |  
> | year        | int   |  
> | quantity    | int   |  
> | price       | int   |  
>  
> (sale_id, year) is the primary key (combination of columns with unique values) of this table.  
> product_id is a foreign key (reference column) to the Product table.  
> Each row of this table shows a sale on the product product_id in a certain year.  
> Note that the price is per unit.  
>  
> **Table: Product**  
>  
> | Column Name  | Type    |  
> |--------------|---------|  
> | product_id   | int     |  
> | product_name | varchar |  
>  
> product_id is the primary key (column with unique values) of this table.  
> Each row of this table indicates the product name of each product.  
>  
> **Problem Statement:**  
> Write a solution to select the product_id, year, quantity, and price for the first year of every product sold.  
> Return the resulting table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT  
>   product_id  
> , YEAR AS first_year  
> , quantity  
> , price  
> FROM Sales  
> WHERE (product_id, YEAR) IN (  
>     SELECT  
>       product_id  
>     , MIN(YEAR) AS YEAR  
>     FROM  
>       Sales  
>     GROUP BY  
>       product_id  
>   );  
> ```  
>  
> **Output:**  
>  
> | product_id | first_year | quantity | price |  
> |------------|------------|----------|-------|  
> | 100        | 2008       | 10       | 5000  |  
> | 200        | 2011       | 15       | 9000  |  
>  
> **Explanation:**  
> - The query selects the product_id, year, quantity, and price for the first year in which each product was sold.  
> - The `MIN(YEAR)` function is used to determine the first year a product was sold.  
> - The subquery returns the product_id and the minimum year for each product. The main query retrieves the quantity and price for these results.  

</details>

<details>
  <summary>596. Classes More Than 5 Students</summary>  

> **Table: Courses**  
>  
> | Column Name | Type    |  
> |-------------|---------|  
> | student     | varchar |  
> | class       | varchar |  
>  
> (student, class) is the primary key (combination of columns with unique values) for this table.  
> Each row of this table indicates the name of a student and the class in which they are enrolled.  
>  
> **Problem Statement:**  
> Write a solution to find all the classes that have at least five students.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT s.class  
> FROM Courses AS s  
> GROUP BY s.class  
> HAVING COUNT(s.student) > 4;  
> ```  
>  
> **Output:**  
>  
> | class |  
> |-------|  
> | Math  |  
>  
> **Explanation:**  
> - The query selects classes from the Courses table.  
> - It groups the results by class and uses the `HAVING` clause to filter classes with more than four students.  

</details>


<details>
  <summary>1729. Find Followers Count</summary>  

> **Table: Followers**  
>  
> | Column Name | Type  |  
> |-------------|-------|  
> | user_id     | int   |  
> | follower_id | int   |  
>  
> (user_id, follower_id) is the primary key (combination of columns with unique values) for this table.  
> This table contains the IDs of a user and a follower in a social media app where the follower follows the user.  
>  
> **Problem Statement:**  
> Write a solution that will, for each user, return the number of followers.  
> Return the result table ordered by user_id in ascending order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT user_id, COUNT(follower_id) AS followers_count  
> FROM Followers  
> GROUP BY user_id  
> ORDER BY user_id;  
> ```  
>  
> **Output:**  
>  
> | user_id | followers_count |  
> |---------|-----------------|  
> | 0       | 1               |  
> | 1       | 1               |  
> | 2       | 2               |  
>  
> **Explanation:**  
> - The query counts the number of followers for each user in the Followers table.  
> - It groups the results by user_id and orders them in ascending order.  

</details>


<details>
  <summary>619. Biggest Single Number</summary>  

> **Table: MyNumbers**  
>  
> | Column Name | Type  |  
> |-------------|-------|  
> | num         | int   |  
>  
> This table may contain duplicates (In other words, there is no primary key for this table in SQL).  
> Each row of this table contains an integer.  
>  
> **Problem Statement:**  
> A single number is a number that appeared only once in the MyNumbers table.  
> Find the largest single number. If there is no single number, report null.  
>  
> **Solution:**  
>  
> ```sql  
> WITH single AS (  
>     SELECT num  
>     FROM MyNumbers  
>     GROUP BY num  
>     HAVING COUNT(*) = 1  
> )  
> SELECT MAX(num) AS num  
> FROM single;  
> ```  
>  
> **Output:**  
>  
> | num |  
> |-----|  
> | 6   |  
>  
> **Explanation:**  
> - The query identifies numbers that appear only once in the MyNumbers table.  
> - It uses a Common Table Expression (CTE) to filter for single occurrences and then finds the maximum of those numbers.  

</details>


<details>
  <summary>1045. Customers Who Bought All Products</summary>  

> **Table: Customer**  
>  
> | Column Name  | Type  |  
> |--------------|-------|  
> | customer_id  | int   |  
> | product_key  | int   |  
>  
> This table may contain duplicate rows.  
> customer_id is not NULL.  
> product_key is a foreign key (reference column) to the Product table.  
>  
> **Table: Product**  
>  
> | Column Name  | Type  |  
> |--------------|-------|  
> | product_key  | int   |  
>  
> product_key is the primary key (column with unique values) for this table.  
>  
> **Problem Statement:**  
> Write a solution to report the customer ids from the Customer table that bought all the products in the Product table.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT c.customer_id  
> FROM Customer AS c  
> GROUP BY c.customer_id  
> HAVING COUNT(DISTINCT c.product_key) = (  
>     SELECT COUNT(p.product_key)  
>     FROM Product AS p  
> );  
> ```  
>  
> **Output:**  
>  
> | customer_id |  
> |-------------|  
> | 1           |  
> | 3           |  
>  
> **Explanation:**  
> - The query groups customers by their ID and counts distinct product keys for each customer.  
> - It compares this count to the total number of products in the Product table to determine if the customer bought all products.  

</details>

















## ADVANCED SELECT AND JOINS

<details>
  <summary>1731. The Number of Employees Which Report to Each Employee</summary>  

> **Table: Employees**  
>  
> | Column Name  | Type     |  
> |--------------|----------|  
> | employee_id  | int      |  
> | name         | varchar  |  
> | reports_to   | int      |  
> | age          | int      |  
>  
> employee_id is the column with unique values for this table.  
> This table contains information about the employees and the id of the manager they report to. Some employees do not report to anyone (reports_to is null).  
>  
> **Problem Statement:**  
> For this problem, we will consider a manager an employee who has at least 1 other employee reporting to them.  
> Write a solution to report the ids and the names of all managers, the number of employees who report directly to them, and the average age of the reports rounded to the nearest integer.  
> Return the result table ordered by employee_id.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT  
>   e.employee_id,  
>   e.name,  
>   COUNT(es.employee_id) AS reports_count,  
>   ROUND(AVG(es.age)) AS average_age  
> FROM Employees AS e  
>   INNER JOIN Employees AS es ON e.employee_id = es.reports_to  
> GROUP BY e.employee_id  
> ORDER BY e.employee_id;  
> ```  
>  
> **Output:**  
>  
> | employee_id | name  | reports_count | average_age |  
> |-------------|-------|---------------|-------------|  
> | 9           | Hercy | 2             | 39          |  
>  
> **Explanation:**  
> - The query counts the number of employees reporting to each manager and calculates the average age of these employees.  
> - It uses an inner join to match managers with their direct reports and groups the results by manager ID.  

</details>


<details>
  <summary>1789. Primary Department for Each Employee</summary>  

> **Table: Employee**  
>  
> | Column Name   | Type   |  
> |---------------|--------|  
> | employee_id   | int    |  
> | department_id | int    |  
> | primary_flag  | varchar|  
>  
> (employee_id, department_id) is the primary key (combination of columns with unique values) for this table.  
> employee_id is the id of the employee.  
> department_id is the id of the department to which the employee belongs.  
> primary_flag is an ENUM (category) of type ('Y', 'N'). If the flag is 'Y', the department is the primary department for the employee. If the flag is 'N', the department is not the primary.  
>  
> Employees can belong to multiple departments. When the employee joins other departments, they need to decide which department is their primary department. Note that when an employee belongs to only one department, their primary column is 'N'.  
>  
> **Problem Statement:**  
> Write a solution to report all the employees with their primary department. For employees who belong to one department, report their only department.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT  
>   employee_id,  
>   department_id  
> FROM Employee  
> GROUP BY employee_id  
> HAVING COUNT(department_id) = 1  
>  
> UNION  
>  
> SELECT  
>   employee_id,  
>   department_id  
> FROM Employee  
> WHERE primary_flag = 'Y';  
> ```  
>  
> **Output:**  
>  
> | employee_id | department_id |  
> |-------------|---------------|  
> | 1           | 1             |  
> | 3           | 3             |  
> | 2           | 1             |  
> | 4           | 3             |  
>  
> **Explanation:**  
> - The query retrieves employees who have only one department or their primary department if they belong to multiple departments.  
> - It uses a union of two select statements to combine the results.  

</details>


<details>
  <summary>610. Triangle Judgement</summary>  

> **Table: Triangle**  
>  
> | Column Name | Type  |  
> |-------------|-------|  
> | x           | int   |  
> | y           | int   |  
> | z           | int   |  
>  
> In SQL, (x, y, z) is the primary key column for this table.  
> Each row of this table contains the lengths of three line segments.  
>  
> **Problem Statement:**  
> Report for every three line segments whether they can form a triangle.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT  
>   *,  
>   (  
>     CASE  
>       WHEN x + y > z  
>       AND x + z > y  
>       AND y + z > x THEN 'Yes'  
>       ELSE 'No'  
>     END  
>   ) AS triangle  
> FROM Triangle;  
> ```  
>  
> **Output:**  
>  
> | x  | y  | z  | triangle |  
> |----|----|----|----------|  
> | 13 | 15 | 30 | No       |  
> | 10 | 20 | 15 | Yes      |  
>  
> **Explanation:**  
> - The query checks the triangle inequality theorem for each set of line segments to determine if they can form a triangle.  
> - It uses a `CASE` statement to return 'Yes' or 'No' based on the conditions.  

</details>


<details>
  <summary>180. Consecutive Numbers</summary>  

> **Table: Logs**  
>  
> | Column Name | Type    |  
> |-------------|---------|  
> | id          | int     |  
> | num         | varchar  |  
>  
> In SQL, id is the primary key for this table.  
> id is an autoincrement column starting from 1.  
>  
> **Problem Statement:**  
> Find all numbers that appear at least three times consecutively.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT DISTINCT num AS ConsecutiveNums  
> FROM (  
>     SELECT *,  
>     LAG(num, 1) OVER (ORDER BY id) AS nlag,  
>     LEAD(num, 1) OVER (ORDER BY id) AS nlead  
>     FROM Logs  
> ) temp1  
> WHERE num = nlag  
>   AND nlag = nlead;  
> ```  
>  
> **Output:**  
>  
> | ConsecutiveNums |  
> |------------------|  
> | 1                |  
>  
> **Explanation:**  
> - The query uses window functions `LAG` and `LEAD` to compare each number with its previous and next values.  
> - It filters for numbers that are the same as their neighbors, indicating at least three consecutive occurrences.  

</details>


<details>
  <summary>1164. Product Price at a Given Date</summary>  

> **Table: Products**  
>  
> | Column Name   | Type    |  
> |---------------|---------|  
> | product_id    | int     |  
> | new_price     | int     |  
> | change_date   | date    |  
>  
> (product_id, change_date) is the primary key (combination of columns with unique values) of this table.  
> Each row of this table indicates that the price of some product was changed to a new price at some date.  
>  
> **Problem Statement:**  
> Write a solution to find the prices of all products on 2019-08-16. Assume the price of all products before any change is 10.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT  
>   product_id,  
>   new_price AS price  
> FROM Products  
> WHERE (product_id, change_date) IN (  
>     SELECT  
>       product_id,  
>       MAX(change_date) AS change_date  
>     FROM Products  
>     WHERE change_date <= '2019-08-16'  
>     GROUP BY product_id  
> )  
>  
> UNION  
>  
> SELECT  
>   product_id,  
>   10 AS price  
> FROM Products  
> GROUP BY product_id  
> HAVING MIN(change_date) > '2019-08-16';  
> ```  
>  
> **Output:**  
>  
> | product_id | price |  
> |------------|-------|  
> | 2          | 50    |  
> | 1          | 35    |  
> | 3          | 10    |  
>  
> **Explanation:**  
> - The first part of the query retrieves the most recent price change for each product on or before 2019-08-16.  
> - The second part handles products that had no price changes by assigning them a default price of 10.  

</details>


<details>
  <summary>1204. Last Person to Fit in the Bus</summary>  

> **Table: Queue**  
>  
> | Column Name   | Type    |  
> |---------------|---------|  
> | person_id     | int     |  
> | person_name   | varchar  |  
> | weight        | int     |  
> | turn          | int     |  
>  
> person_id column contains unique values.  
> This table has the information about all people waiting for a bus.  
> The person_id and turn columns will contain all numbers from 1 to n, where n is the number of rows in the table.  
> turn determines the order of which the people will board the bus, where turn=1 denotes the first person to board and turn=n denotes the last person to board.  
> weight is the weight of the person in kilograms.  
>  
> **Problem Statement:**  
> There is a queue of people waiting to board a bus. However, the bus has a weight limit of 1000 kilograms, so there may be some people who cannot board.  
> Write a solution to find the person_name of the last person that can fit on the bus without exceeding the weight limit. The test cases are generated such that the first person does not exceed the weight limit.  
> Note that only one person can board the bus at any given turn.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT person_name  
> FROM (  
>     SELECT *,  
>     SUM(weight) OVER (ORDER BY turn) AS total_weight  
>     FROM Queue  
> ) subs  
> WHERE total_weight <= 1000  
> ORDER BY turn DESC  
> LIMIT 1;  
> ```  
>  
> **Output:**  
>  
> | person_name |  
> |-------------|  
> | John Cena   |  
>  
> **Explanation:**  
> - The query calculates the cumulative weight of people boarding the bus in order of their turn.  
> - It then filters for those whose total weight is within the limit and selects the last person able to board.  

</details>


<details>
  <summary>1907. Count Salary Categories</summary>  

> **Table: Accounts**  
>  
> | Column Name | Type  |  
> |-------------|-------|  
> | account_id  | int   |  
> | income      | int   |  
>  
> account_id is the primary key (column with unique values) for this table.  
> Each row contains information about the monthly income for one bank account.  
>  
> **Problem Statement:**  
> Write a solution to calculate the number of bank accounts for each salary category. The salary categories are:  
> - "Low Salary": All the salaries strictly less than $20000.  
> - "Average Salary": All the salaries in the inclusive range [$20000, $50000].  
> - "High Salary": All the salaries strictly greater than $50000.  
> The result table must contain all three categories. If there are no accounts in a category, return 0.  
>  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT 'Low Salary' AS category,  
>        SUM(CASE WHEN income < 20000 THEN 1 ELSE 0 END) AS accounts_count  
> FROM Accounts  
> UNION  
> SELECT 'Average Salary' AS category,  
>        SUM(CASE WHEN income BETWEEN 20000 AND 50000 THEN 1 ELSE 0 END) AS accounts_count  
> FROM Accounts  
> UNION  
> SELECT 'High Salary' AS category,  
>        SUM(CASE WHEN income > 50000 THEN 1 ELSE 0 END) AS accounts_count  
> FROM Accounts;  
> ```  
>  
> **Output:**  
>  
> | category       | accounts_count |  
> |----------------|----------------|  
> | Low Salary     | 1              |  
> | Average Salary | 0              |  
> | High Salary    | 3              |  
>  
> **Explanation:**  
> - The query counts the number of accounts in each salary category using conditional aggregation.  
> - Each category is represented in the result set, ensuring that all categories are included regardless of whether there are any accounts in that category.  

</details>















## SUBQUERIES

<details>
  <summary>1978. Employees Whose Manager Left the Company</summary>  

> **Table: Employees**  
>  
> | Column Name  | Type    |  
> |--------------|---------|  
> | employee_id  | int     |  
> | name         | varchar  |  
> | manager_id   | int     |  
> | salary       | int     |  
>  
> In SQL, employee_id is the primary key for this table.  
> This table contains information about the employees, their salary, and the ID of their manager. Some employees do not have a manager (manager_id is null).  
>  
> **Problem Statement:**  
> Find the IDs of the employees whose salary is strictly less than $30000 and whose manager left the company. When a manager leaves the company, their information is deleted from the Employees table, but the reports still have their manager_id set to the manager that left.  
> Return the result table ordered by employee_id.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT employee_id  
> FROM Employees  
> WHERE manager_id NOT IN (  
>     SELECT employee_id  
>     FROM Employees  
> )  
> AND salary < 30000  
> ORDER BY employee_id;  
> ```  
>  
> **Output:**  
>  
> | employee_id |  
> |-------------|  
> | 11          |  
>  
> **Explanation:**  
> - The query selects employees with a salary below $30000 whose managers are no longer listed in the Employees table.  
> - It checks for managers by using a subquery to filter out any current employees based on their manager_id.  

</details>


<details>
  <summary>626. Exchange Seats</summary>  

> **Table: Seat**  
>  
> | Column Name | Type    |  
> |-------------|---------|  
> | id          | int     |  
> | student     | varchar  |  
>  
> id is the primary key (unique value) column for this table.  
> Each row of this table indicates the name and the ID of a student.  
> The ID sequence always starts from 1 and increments continuously.  
>  
> **Problem Statement:**  
> Write a solution to swap the seat id of every two consecutive students. If the number of students is odd, the id of the last student is not swapped.  
> Return the result table ordered by id in ascending order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT (  
>     CASE  
>       WHEN id % 2 = 0 THEN id - 1  
>       WHEN id = (SELECT COUNT(*) FROM Seat) THEN id  
>       ELSE id + 1  
>     END
>     ) AS id
>     , student  
> FROM Seat  
> ORDER BY id;  
> ```  
>  
> **Output:**  
>  
> | id | student   |  
> |----|-----------|  
> | 1  | Doris     |  
> | 2  | Abbot     |  
> | 3  | Green     |  
> | 4  | Emerson   |  
> | 5  | Jeames    |  
>  
> **Explanation:**  
> - The query uses a `CASE` statement to determine the new id for each student based on whether their current id is even or odd.  
> - It adjusts the ids accordingly, ensuring that pairs are swapped while leaving the last student unchanged if there’s an odd number of students.  

</details>


<details>
  <summary>1341. Movie Rating</summary>  

> **Table: Movies**  
>  
> | Column Name  | Type    |  
> |--------------|---------|  
> | movie_id     | int     |  
> | title        | varchar  |  
>  
> movie_id is the primary key (column with unique values) for this table.  
> title is the name of the movie.  
>  
> **Table: Users**  
>  
> | Column Name  | Type    |  
> |--------------|---------|  
> | user_id      | int     |  
> | name         | varchar  |  
>  
> user_id is the primary key (column with unique values) for this table.  
> The column 'name' has unique values.  
>  
> **Table: MovieRating**  
>  
> | Column Name  | Type    |  
> |--------------|---------|  
> | movie_id     | int     |  
> | user_id      | int     |  
> | rating       | int     |  
> | created_at   | date    |  
>  
> (movie_id, user_id) is the primary key (column with unique values) for this table.  
> This table contains the rating of a movie by a user in their review.  
> created_at is the user's review date.  
>  
> **Problem Statement:**  
> Write a solution to:  
> 1. Find the name of the user who has rated the greatest number of movies. In case of a tie, return the lexicographically smaller user name.  
> 2. Find the movie name with the highest average rating in February 2020. In case of a tie, return the lexicographically smaller movie name.  
>  
> **Solution:**  
>  
> ```sql  
> (SELECT u.name AS results  
> FROM MovieRating AS mr  
>     INNER JOIN Users AS u ON mr.user_id = u.user_id  
> GROUP BY mr.user_id  
> ORDER BY COUNT(mr.user_id) DESC, u.name ASC  
> LIMIT 1)  
>  
> UNION ALL  
>  
> (SELECT m.title AS results  
> FROM MovieRating AS mr  
>     INNER JOIN Movies AS m ON mr.movie_id = m.movie_id  
> WHERE mr.created_at LIKE '2020-02%'  
> GROUP BY mr.movie_id  
> ORDER BY AVG(mr.rating) DESC, m.title ASC  
> LIMIT 1);  
> ```  
>  
> **Output:**  
>  
> | results  |  
> |----------|  
> | Daniel   |  
> | Frozen 2 |  
>  
> **Explanation:**  
> - The first part of the query finds the user who rated the most movies, ordering by the count of ratings and the name for ties.  
> - The second part calculates the highest average rating for movies in February 2020, with ties resolved lexicographically by movie title.  

</details>


<details>
  <summary>1321. Restaurant Growth</summary>  

> **Table: Customer**  
>  
> | Column Name   | Type    |  
> |---------------|---------|  
> | customer_id   | int     |  
> | name          | varchar  |  
> | visited_on    | date    |  
> | amount        | int     |  
>  
> In SQL, (customer_id, visited_on) is the primary key for this table.  
> This table contains data about customer transactions in a restaurant.  
> visited_on is the date on which the customer with ID (customer_id) has visited the restaurant.  
> amount is the total paid by a customer.  
>  
> **Problem Statement:**  
> You are the restaurant owner and you want to analyze a possible expansion (there will be at least one customer every day).  
> Compute the moving average of how much the customer paid in a seven days window (i.e., current day + 6 days before). The average_amount should be rounded to two decimal places.  
> Return the result table ordered by visited_on in ascending order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT  
>      visited_on  
>     ,(SELECT SUM(amount)  
>      FROM customer  
>      WHERE visited_on BETWEEN DATE_SUB(c.visited_on, INTERVAL 6 DAY) AND c.visited_on) AS amount  
>     ,ROUND((SELECT SUM(amount)/7  
>            FROM customer  
>            WHERE visited_on BETWEEN DATE_SUB(c.visited_on, INTERVAL 6 DAY) AND c.visited_on), 2) AS average_amount  
> FROM customer c  
> WHERE visited_on >= (  
>     SELECT DATE_ADD(MIN(visited_on), INTERVAL 6 DAY)  
>     FROM customer  
> )  
> GROUP BY visited_on;  
> ```  
>  
> **Output:**  
>  
> | visited_on | amount | average_amount |  
> |------------|--------|----------------|  
> | 2019-01-07 | 860    | 122.86         |  
> | 2019-01-08 | 840    | 120.00         |  
> | 2019-01-09 | 840    | 120.00         |  
> | 2019-01-10 | 1000   | 142.86         |  
>  
> **Explanation:**  
> - The query calculates the total amount spent and the moving average for the past seven days for each day where there is data.  
> - It uses a subquery to sum the amounts for the past six days and the current day, rounding the average to two decimal places.  

</details>


<details>
  <summary>602. Friend Requests II: Who Has the Most Friends</summary>  

> **Table: RequestAccepted**  
>  
> | Column Name   | Type    |  
> |---------------|---------|  
> | requester_id  | int     |  
> | accepter_id   | int     |  
> | accept_date   | date    |  
>  
> (requester_id, accepter_id) is the primary key (combination of columns with unique values) for this table.  
> This table contains the ID of the user who sent the request, the ID of the user who received the request, and the date when the request was accepted.  
>  
> **Problem Statement:**  
> Write a solution to find the people who have the most friends and the most friends number.  
> The test cases are generated so that only one person has the most friends.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT  
>    requester_id AS id  
>   ,COUNT(*) AS num  
> FROM  
>   (  
>     SELECT requester_id  
>     FROM RequestAccepted
> 
>     UNION ALL
> 
>     SELECT accepter_id  
>     FROM RequestAccepted  
>   ) AS tabl  
> GROUP BY id  
> ORDER BY num DESC  
> LIMIT 1;  
> ```  
>  
> **Output:**  
>  
> | id | num |  
> |----|-----|  
> | 3  | 3   |  
>  
> **Explanation:**  
> - The query combines both requester and accepter IDs to count the total number of friends for each user.  
> - It then groups the results by user ID and orders them by the count of friends, returning the user with the highest number of friends.  

</details>


<details>
  <summary>585. Investments in 2016</summary>  

> **Table: Insurance**  
>  
> | Column Name | Type   |  
> |-------------|--------|  
> | pid         | int    |  
> | tiv_2015    | float  |  
> | tiv_2016    | float  |  
> | lat         | float  |  
> | lon         | float  |  
>  
> pid is the primary key (column with unique values) for this table.  
> Each row of this table contains information about one policy where:  
> - pid is the policyholder's policy ID.  
> - tiv_2015 is the total investment value in 2015 and tiv_2016 is the total investment value in 2016.  
> - lat is the latitude of the policyholder's city. It's guaranteed that lat is not NULL.  
> - lon is the longitude of the policyholder's city. It's guaranteed that lon is not NULL.  
>  
> **Problem Statement:**  
> Write a solution to report the sum of all total investment values in 2016 (tiv_2016), for all policyholders who:  
> 1. have the same tiv_2015 value as one or more other policyholders, and  
> 2. are not located in the same city as any other policyholder (i.e., the (lat, lon) attribute pairs must be unique).  
> Round tiv_2016 to two decimal places.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT ROUND(SUM(tiv_2016), 2) AS tiv_2016  
> FROM Insurance  
> WHERE  1=1
> AND (lat, lon) IN (  
>     SELECT lat, lon  
>     FROM Insurance  
>     GROUP BY lat, lon  
>     HAVING COUNT(*) = 1  
> )  
> AND tiv_2015 IN (  
>     SELECT tiv_2015  
>     FROM Insurance  
>     GROUP BY tiv_2015  
>     HAVING COUNT(*) > 1  
> );  
> ```  
>  
> **Output:**  
>  
> | tiv_2016 |  
> |----------|  
> | 45       |  
>  
> **Explanation:**  
> - The query first filters for unique (lat, lon) pairs to ensure the policyholders are not located in the same city.  
> - It also checks for tiv_2015 values that are shared among multiple policyholders.  
> - Finally, it sums up the tiv_2016 values for the filtered records and rounds the result to two decimal places.  

</details>


<details>
  <summary>185. Department Top Three Salaries</summary>  

> **Table: Employee**  
>  
> | Column Name  | Type    |  
> |--------------|---------|  
> | id           | int     |  
> | name         | varchar  |  
> | salary       | int     |  
> | departmentId | int     |  
>  
> id is the primary key (column with unique values) for this table.  
> departmentId is a foreign key (reference column) of the ID from the Department table.  
> Each row of this table indicates the ID, name, and salary of an employee. It also contains the ID of their department.  
>  
> **Table: Department**  
>  
> | Column Name | Type    |  
> |-------------|---------|  
> | id          | int     |  
> | name        | varchar  |  
>  
> id is the primary key (column with unique values) for this table.  
> Each row of this table indicates the ID of a department and its name.  
>  
> **Problem Statement:**  
> A company's executives are interested in seeing who earns the most money in each of the company's departments. A high earner in a department is an employee who has a salary in the top three unique salaries for that department.  
> Write a solution to find the employees who are high earners in each of the departments.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT Department, Employee, Salary  
> FROM (  
>     SELECT  
>         d.name AS Department    
>        , e.name AS Employee  
>        , e.salary AS Salary  
>        , DENSE_RANK() OVER (PARTITION BY d.name ORDER BY Salary DESC) AS dr  
>     FROM Employee AS e  
>     JOIN Department AS d ON e.departmentId = d.id  
>    ) AS tabl  
> WHERE dr <= 3;  
> ```  
>  
> **Output:**  
>  
> | Department | Employee | Salary |  
> |------------|----------|--------|  
> | IT         | Max      | 90000  |  
> | IT         | Joe      | 85000  |  
> | IT         | Randy    | 85000  |  
> | IT         | Will     | 70000  |  
> | Sales      | Henry    | 80000  |  
> | Sales      | Sam      | 60000  |  
>  
> **Explanation:**  
> - The query uses the `DENSE_RANK()` window function to assign a rank to each employee's salary within their department.  
> - It selects the top three unique salaries for each department and returns the corresponding employee names and salaries.  

</details>


## ADVANCED STRING FUNCTION/REGEX/CLAUSE

<details>
  <summary>1667. Fix Names in a Table</summary>  

> **Table: Users**  
>  
> | Column Name    | Type    |  
> |----------------|---------|  
> | user_id        | int     |  
> | name           | varchar  |  
>  
> user_id is the primary key (column with unique values) for this table.  
> This table contains the ID and the name of the user. The name consists of only lowercase and uppercase characters.  
>  
> **Problem Statement:**  
> Write a solution to fix the names so that only the first character is uppercase and the rest are lowercase.  
> Return the result table ordered by user_id.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT  
>   user_id  
>  ,CONCAT(UPPER(SUBSTR(name, 1, 1)), LOWER(SUBSTR(name, 2))) AS name  
> FROM Users  
> ORDER BY user_id;  
> ```  
>  
> **Output:**  
>  
> | user_id | NAME  |  
> |---------|-------|  
> | 1       | Alice |  
> | 2       | Bob   |  
>  
> **Explanation:**  
> - The query modifies each name by converting the first letter to uppercase and the rest to lowercase using string functions.  
> - It orders the results by user_id to maintain the original sequence.  

</details>

<details>
  <summary>1527. Patients With a Condition</summary>  

> **Table: Patients**  
>  
> | Column Name   | Type    |  
> |---------------|---------|  
> | patient_id    | int     |  
> | patient_name  | varchar  |  
> | conditions     | varchar  |  
>  
> patient_id is the primary key (column with unique values) for this table.  
> 'conditions' contains 0 or more codes separated by spaces.  
> This table contains information about the patients in the hospital.  
>  
> **Problem Statement:**  
> Write a solution to find the patient_id, patient_name, and conditions of the patients who have Type I Diabetes. Type I Diabetes always starts with the DIAB1 prefix.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT *  
> FROM Patients  
> WHERE conditions LIKE 'DIAB1%'  
>    OR conditions LIKE '% DIAB1%';  
> ```  
>  
> **Output:**  
>  
> | patient_id | patient_name | conditions   |  
> |------------|--------------|--------------|  
> | 3          | Bob          | DIAB100 MYOP |  
> | 4          | George       | ACNE DIAB100 |  
>  
> **Explanation:**  
> - The query selects all patients whose conditions include Type I Diabetes, checking both the beginning and the presence of the code within the string of conditions.  
> - It uses the `LIKE` operator to filter for the desired prefix.  

</details>


<details>
  <summary>196. Delete Duplicate Emails</summary>  

> **Table: Person**  
>  
> | Column Name | Type    |  
> |-------------|---------|  
> | id          | int     |  
> | email       | varchar  |  
>  
> id is the primary key (column with unique values) for this table.  
> Each row of this table contains an email. The emails will not contain uppercase letters.  
>  
> **Problem Statement:**  
> Write a solution to delete all duplicate emails, keeping only one unique email with the smallest id.  
>  
> **Solution:**  
>  
> ```sql  
> DELETE FROM Person  
> WHERE id NOT IN (  
>     SELECT min_id  
>     FROM (  
>         SELECT email, MIN(id) AS min_id  
>         FROM Person  
>         GROUP BY email  
>     ) tabl  
> );  
> ```  
>  
> **Output:**  
>  
> | id | email            |  
> |----|------------------|  
> | 1  | john@example.com |  
> | 2  | bob@example.com  |  
>  
> **Explanation:**  
> - The query deletes duplicate emails by selecting the minimum id for each unique email and keeping those rows while deleting others.  
> - The inner query groups the emails and finds the minimum id, and the outer query deletes entries not matching those ids.  

Output
| SecondHighestSalary |
| ------------------- |
| 200                 |


</details>


<details>
  <summary>176. Second Highest Salary</summary>  

> **Table: Employee**  
>  
> | Column Name | Type  |  
> |-------------|-------|  
> | id          | int   |  
> | salary      | int   |  
>  
> id is the primary key (column with unique values) for this table.  
> Each row of this table contains information about the salary of an employee.  
>  
> **Problem Statement:**  
> Write a solution to find the second highest distinct salary from the Employee table. If there is no second highest salary, return null (return None in Pandas).  
>  
> **Solution:**  
>  
> ```sql  
> SELECT MAX(salary) AS SecondHighestSalary  
> FROM Employee  
> WHERE salary NOT IN (  
>     SELECT MAX(salary)  
>     FROM Employee  
> );  
> ```  
>  
> **Output:**  
>  
> | SecondHighestSalary |  
> |---------------------|  
> | 200                 |  
>  
> **Explanation:**  
> - The query retrieves the maximum salary that is not the highest salary by excluding the highest salary from the results.  
> - If there's no second highest salary, it returns null as required.  

</details>



<details>
  <summary>1484. Group Sold Products By The Date</summary>  

> **Table: Activities**  
>  
> | Column Name | Type    |  
> |-------------|---------|  
> | sell_date   | date    |  
> | product     | varchar  |  
>  
> There is no primary key (column with unique values) for this table. It may contain duplicates.  
> Each row of this table contains the product name and the date it was sold in a market.  
>  
> **Problem Statement:**  
> Write a solution to find for each date the number of different products sold and their names.  
> The sold products names for each date should be sorted lexicographically.  
> Return the result table ordered by sell_date.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT  
>   sell_date,  
>   COUNT(DISTINCT product) AS num_sold,  
>   GROUP_CONCAT(DISTINCT product ORDER BY product SEPARATOR ',') AS products  
> FROM Activities  
> GROUP BY sell_date  
> ORDER BY sell_date;  
> ```  
>  
> **Output:**  
>  
> | sell_date  | num_sold | products                     |  
> |------------|----------|------------------------------|  
> | 2020-05-30 | 3        | Basketball,Headphone,T-Shirt |  
> | 2020-06-01 | 2        | Bible,Pencil                 |  
> | 2020-06-02 | 1        | Mask                         |  
>  
> **Explanation:**  
> - The query counts the distinct products sold on each date and concatenates the names of those products in sorted order.  
> - It groups the results by sell_date to ensure each date is represented in the output.  

</details>


<details>
  <summary>1327. List the Products Ordered in a Period</summary>  

> **Table: Products**  
>  
> | Column Name      | Type    |  
> |------------------|---------|  
> | product_id       | int     |  
> | product_name     | varchar  |  
> | product_category  | varchar  |  
>  
> product_id is the primary key (column with unique values) for this table.  
> This table contains data about the company's products.  
>  
> **Table: Orders**  
>  
> | Column Name   | Type    |  
> |---------------|---------|  
> | product_id    | int     |  
> | order_date     | date    |  
> | unit          | int     |  
>  
> This table may have duplicate rows.  
> product_id is a foreign key (reference column) to the Products table.  
> unit is the number of products ordered on order_date.  
>  
> **Problem Statement:**  
> Write a solution to get the names of products that have at least 100 units ordered in February 2020 and their amount.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT  
>   product_name,  
>   SUM(unit) AS unit  
> FROM Orders o  
>   JOIN Products p USING (product_id)  
> WHERE order_date >= '2020-02-01'  
>   AND order_date < '2020-03-01'  
> GROUP BY product_name  
> HAVING SUM(unit) >= 100;  
> ```  
>  
> **Output:**  
>  
> | product_name       | unit |  
> |--------------------|------|  
> | Leetcode Solutions  | 130  |  
> | Leetcode Kit       | 100  |  
>  
> **Explanation:**  
> - The query joins the Orders and Products tables to filter products ordered in February 2020.  
> - It groups the results by product name, summing the units ordered, and only includes those with at least 100 units.  

</details>


<details>
  <summary>1517. Find Users With Valid E-Mails</summary>  

> **Table: Users**  
>  
> | Column Name   | Type    |  
> |---------------|---------|  
> | user_id       | int     |  
> | name          | varchar  |  
> | mail          | varchar  |  
>  
> user_id is the primary key (column with unique values) for this table.  
> This table contains information about the users signed up on a website. Some e-mails are invalid.  
>  
> **Problem Statement:**  
> Write a solution to find the users who have valid emails.  
> A valid e-mail has a prefix name and a domain where:  
> - The prefix name is a string that may contain letters (upper or lower case), digits, underscore '_', period '.', and/or dash '-'. The prefix name must start with a letter.  
> - The domain is '@leetcode.com'.  
> Return the result table in any order.  
>  
> **Solution:**  
>  
> ```sql  
> SELECT *  
> FROM Users  
> WHERE  
>   mail REGEXP '^[A-Za-z][A-Za-z0-9_.-]*@leetcode[.]com$';  
> ```  
>  
> **Output:**  
>  
> | user_id | name      | mail                    |  
> |---------|-----------|-------------------------|  
> | 1       | Winston   | winston@leetcode.com    |  
> | 3       | Annabelle | bella-@leetcode.com     |  
> | 4       | Sally     | sally.come@leetcode.com |  
>  
> **Explanation:**  
> - The query uses a regular expression to filter valid email addresses based on the specified rules for the prefix name and domain.  
> - It retrieves all user information for those with valid emails matching the criteria.  

</details>
