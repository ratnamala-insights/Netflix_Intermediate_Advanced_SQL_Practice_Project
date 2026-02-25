# Netflix_Intermediate_Advanced_SQL_Practice_Project
This project focuses on analyzing Netflix content data using Microsoft SQL Server. The primary goal of this project was to demonstrate practical SQL skills by extracting meaningful insights from real-world data and solving business-related problems. 

Overall, this project demonstrates a solid understanding of SQL, data cleaning, and analytical problem-solving. It also highlights the ability to handle real-world messy data and transform it into actionable insights. This work is an excellent showcase of SQL expertise and practical analytical skills for any aspiring data analyst.

**Overview**
**The objective of this project is to demonstrate:**

Database creation and table design
Data import using BULK INSERT
Data exploration and cleaning
**Use of SQL concepts like:**
GROUP BY
CASE
RANK()
WINDOW FUNCTIONS
STRING_SPLIT
CROSS APPLY
DATE FUNCTIONS
Aggregations and filtering


This project analyzes Netflix content data to answer business-related questions using SQL.
**The dataset includes:**
Movies and TV Shows
Release Year
Rating
Country
Duration
Genre
Director
Cast
Description

**The goal is to practice SQL querying and derive insights from raw data.**

**Database & Table Creation**

Created a database named netflix
Designed table structure matching CSV columns
Imported dataset using BULK INSERT
Verified successful data load


**Analysis & Business Questions Solved**

**Below is the step-by-step analysis performed on the dataset:**

Total Content Available on Netflix

Select count(*) as total_content 
from netflix;

```sql

SELECT name, age
FROM users
WHERE age > 18
ORDER BY age DESC;

