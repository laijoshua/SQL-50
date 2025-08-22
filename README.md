# SQL-50
My answers to Leetcode's SQL 50 

## Select:
1. [Recyclable & Low Fat Products](#Recyclable-and-Low-Fat-Products)
2. [Find Customer Referee](#Find-Customer-Referee)
3. [Big Countries](#Big-Countries)
4. [Article Views I](#Article-Views-I)
5. [Invalid Tweets](#Invalid-Tweets)

## Basic Joins:
1. [Replace Employee ID with the Unique Identifier](#Replace-Employee-ID-with-the-Unique-Identifier)
2. [Product Sales Analysis I](#Product-Sales-Analysis-I)
3. [Customer who visited but did not make any transactions](#Customer-who-visited-but-did-not-make-any-transactions)
4. [Rising Temperature](#Rising-Temperature)
5. [Average Time of Process per Machine](#Average-Time-of-Process-per-Machine)
6. [Employee Bonus](#Employee-Bonus)
7. [Students & Examinations](#Students-and-Examinations)
8. [Managers with at least 5 Direct Reports](#Managers-with-at-least-5-Direct-Reports)
9. [Confirmation Rate](#Confirmation-Rate)

## Basic Aggregate Functions
1. [Not Boring Movies](#Not-Boring-Movies)
2. [Average Selling Price](#Average-Selling-Price)
3. [Project Employees I](#Project-Employees-I)
4. [Percentage of Users Attended a Contest](#Percentage-of-Users-Attended-a-Contest)
5. [Queries Quality & Percentage](#Queries-Quality-and-Percentage)
6. [Monthly Transactions I](#Monthly-Transactions-I)
7. [Immediate Food Delivery II](#Immediate-Food-Delivery-II)
8. [Game Play Analysis IV](#Game-Play-Analysis-IV)

## Sorting & Grouping
1. [Number of Unique Subjects Taught by Each Teacher](#Number-of-Unique-Subjects-Taught-by-Each-Teacher)
2. [User Activity for the Past 30 Days I](#User-Activity-for-the-Past-30-Days-I)
3. [Product Sales Analysis III](#Product-Sales-Analysis-III)
4. [Classes With at least 5 Students](#Classes-With-at-least-5-Students)
5. [Find Followers Count](#Find-Followers-Count)
6. [Biggest Single Number](#Biggest-Single-Number)
7. [Customers Who Bought All Products](#Customers-Who-Bought-All-Products)

## Advanced Select & Joins
1. [The Number of Employees Which Report to Each Employee](#The-Number-of-Employees-Which-Report-to-Each-Employee)
2. [Primary Department for Each Employee](#Primary-Department-for-Each-Employee)
3. [Triangle Judgement](#Triangle-Judgement)
4. [Consecutive Numbers](#Consecutive-Numbers)
5. [Product Price at a Given Date](#Product-Price-at-a-Given-Date)
6. [Last Person to Fit in the Bus](#Last-Person-to-Fit-in-the-Bus)
7. [Count Salary Categories](#Count-Salary-Categories)

## Subqueries
1. [Employees Whose Manager Left the Company](#Employees-Whose-Manager-Left-the-Company)
2. [Exchange Seats](#Exchange-Seats)
3. [Movie Rating](#Movie-Rating)
4. [Restaurant Growth](#Restaurant-Growth)
5. [Friend Requests II: Who has the most friends](#Friend-Requests-II-Who-has-the-most-friends)
6. [Investments in 2016](#Investments-in-2016)
7. [Department Top Three Salaries](#Department-Top-Three-Salaries)

## Advanced String Functions/Regex/Clause
1. [Fix Names in a Table](#Fix-Names-in-a-Table)
2. [Patients With a Condition](#Patients-With-a-Condition)
3. [Delete Duplicate Emails](#Delete-Duplicate-Emails)
4. [Second Highest Salary](#Second-Highest-Salary)
5. [Group Sold Products By The Date](#Group-Sold-Products-By-The-Date)
6. [List the Products Ordered in a Period](#List-the-Products-Ordered-in-a-Period)
7. [Find Users With Valid Emails](#Find-Users-With-Valid-Emails)


## Recyclable and Low Fat Products
```sql
SELECT product_id
FROM Products
WHERE low_fats = "Y" AND recyclable = "Y"
```

## Find Customer Referee
```sql
SELECT name
FROM Customer
WHERE referee_id IS NULL OR referee_id != 2
```

## Big Countries
```sql
SELECT name, population, area
FROM World
WHERE area >= 3000000 or population >= 25000000
```

## Article Views I
```sql
SELECT DISTINCT author_id AS id
FROM Views
WHERE author_id = viewer_id
ORDER BY author_id ASC
```

## Invalid Tweets
```sql
SELECT tweet_id
FROM Tweets
WHERE LENGTH(content) > 15
```

## Replace Employee ID with the Unique Identifier
```sql
SELECT unique_id, name
FROM Employees LEFT JOIN EmployeeUNI ON Employees.id = EmployeeUNI.id
```

## Product Sales Analysis I
```sql
SELECT product_name, year, price
FROM Sales JOIN Product ON Sales.product_id = Product.product_id
```

## Customer who visited but did not make any transactions
```sql
SELECT customer_id, COUNT(visit_id) AS count_no_trans
FROM Visits
WHERE visit_id NOT IN (SELECT visit_id FROM Transactions)
GROUP BY customer_id
```

## Rising Temperature
```sql
SELECT a.id
FROM Weather a, Weather b
WHERE a.temperature > b.temperature AND dateDiff(a.recordDate, b.recordDate)=1
```

## Average Time of Process per Machine
```sql
SELECT b.machine_id, ROUND(AVG(b.timestamp-a.timestamp),3) AS processing_time
FROM Activity a
JOIN Activity b ON a.machine_id = b.machine_id
WHERE a.activity_type = "start" AND b.activity_type = "end"
GROUP BY machine_id
```

## Employee Bonus
```sql
SELECT name, bonus
FROM Employee LEFT JOIN Bonus ON Employee.empId = Bonus.empId
WHERE bonus < 1000 OR bonus IS NULL
```

## Students and Examinations
```sql
SELECT s.student_id, student_name, su.subject_name, COUNT(e.subject_name) AS attended_exams
FROM Students s
    JOIN Subjects su 
    LEFT JOIN Examinations e ON s.student_id = e.student_id AND su.subject_name = e.subject_name
GROUP BY s.student_id, student_name, su.subject_name
ORDER BY s.student_id, student_name, su.subject_name
```

## Managers with at least 5 Direct Reports
```sql
SELECT name
FROM Employee
WHERE id IN (SELECT managerId FROM Employee GROUP BY managerId HAVING COUNT(*) >= 5)
```

## Confirmation Rate
```sql

```

## Not Boring Movies
```sql
SELECT id, movie, description, rating
FROM Cinema
WHERE description <> "boring" AND id % 2 = 1
ORDER BY rating DESC
```

## Average Selling Price
```sql

```

## Project Employees I
```sql
SELECT project_id, ROUND(SUM(experience_years)/COUNT(DISTINCT Project.employee_id), 2) AS average_years
FROM Project JOIN Employee on Project.employee_id = Employee.employee_id
GROUP BY project_id
```

## Percentage of Users Attended a Contest
```sql
SELECT contest_id, ROUND(COUNT(DISTINCT user_id)/(SELECT COUNT(DISTINCT user_id) FROM Users) * 100, 2)AS percentage
FROM Register
GROUP BY contest_id
ORDER BY percentage DESC, contest_id ASC
```

## Queries Quality and Percentage
```sql
SELECT 
    query_name, 
    ROUND(AVG(rating/position), 2) AS quality, 
    ROUND((SELECT COUNT(rating) FROM Queries b WHERE rating < 3 AND a.query_name = b.query_name)*100/COUNT(query_name), 2)AS poor_query_percentage
FROM Queries a
GROUP BY query_name
```

## Monthly Transactions I
```sql

```

## Immediate Food Delivery II
```sql

```

## Game Play Analysis IV
```sql

```

## Number of Unique Subjects Taught by Each Teacher
```sql
SELECT teacher_id, COUNT(DISTINCT subject_id) AS cnt 
FROM Teacher
GROUP BY teacher_id
```

## User Activity for the Past 30 Days I
```sql
SELECT activity_date AS day, COUNT(DISTINCT user_id) AS active_users
FROM Activity
WHERE activity_date BETWEEN '2019-06-28' AND '2019-07-27'
GROUP BY day
```

## Product Sales Analysis III
```sql
SELECT product_id, year AS first_year, quantity, price
FROM Sales
WHERE (product_id, year) IN (SELECT product_id, min(year) FROM Sales GROUP BY product_id)
```

## Classes With at least 5 Students
```sql
SELECT class 
FROM Courses
GROUP BY class
HAVING COUNT(DISTINCT student) >= 5
```

## Find Followers Count
```sql
SELECT user_id, COUNT(follower_id) AS followers_count
FROM Followers
GROUP BY user_id
ORDER BY user_id
```

## Biggest Single Number
```sql
SELECT MAX(num) AS num
FROM MyNumbers
WHERE num in (SELECT num FROM MyNumbers GROUP BY num HAVING COUNT(num) = 1)
```

## Customers Who Bought All Products
```sql
SELECT customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (SELECT COUNT(product_key) FROM Product)
```

## The Number of Employees Which Report to Each Employee
```sql

```

## Primary Department for Each Employee
```sql
SELECT employee_id, department_id
FROM Employee
WHERE primary_flag = 'Y' OR employee_id IN (SELECT employee_id FROM Employee GROUP BY employee_id HAVING COUNT(employee_id) = 1)
```

## Triangle Judgement
```sql

```

## Consecutive Numbers
```sql

```

## Product Price at a Given Date
```sql

```

## Last Person to Fit in the Bus
```sql

```

## Count Salary Categories
```sql

```

## Employees Whose Manager Left the Company
```sql

```

## Exchange Seats
```sql

```

## Movie Rating
```sql

```

## Restaurant Growth
```sql

```

## Friend Requests II Who has the most friends
```sql

```

## Investments in 2016
```sql

```

## Department Top Three Salaries
```sql

```

## Fix Names in a Table
```sql

```

## Patients With a Condition
```sql

```

## Delete Duplicate Emails
```sql

```

## Second Highest Salary
```sql

```

## Group Sold Products By The Date
```sql

```

## List the Products Ordered in a Period
```sql

```

## Find Users With Valid Emails
```sql

```

