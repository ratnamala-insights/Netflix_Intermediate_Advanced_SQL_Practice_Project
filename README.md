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

**Total Content Available on Netflix**

```sql

Select count(*) as total_content 
from netflix;

```

**Find how many types of contents available in netflix**

```sql

Select distinct(types)
from netflix;

```
**Business Problems**

**Count the number of movies vs TV shows**
```sql
SELECT types, count(title) as total_content
FROM netflix
group by types;

```

**Find the most common rating for movies & TV shows**

```sql
Select types,rating from
	(
		Select types,rating,count(rating) as 'count_rating',
		rank() over(partition by types order by count(rating) desc) as Ranking 
		from netflix
		group by types,rating
	) as rk
where Ranking = 1;

```
**List all movies released in a speific year(e.g., 2020)**

```sql
Select *
from netflix
where release_year = 2020 and types = 'Movie';
```

**Find the top 5 countries with the most content on netflix**

```sql
SELECT TOP 5
    TRIM(value) AS country,
    COUNT(*) AS total_appearances
FROM netflix
CROSS APPLY STRING_SPLIT(country, ',')
GROUP BY TRIM(value)
ORDER BY total_appearances DESC;
```

**Identify the longest movie**

```sql
Select
title,duration
from netflix
where types = 'Movie' and duration = (Select max(duration)from netflix);
```

**Find content added in the last 5 years**

```sql
SELECT * 
FROM NETFLIX
WHERE cast(date_added as date) >= DATEADD(YEAR, -5, GETDATE()); -- DATE_ADDED>= THE DATE WHICH IS 5 YEARS AGO FROM TODAY, BUT date_added is sored 
--in form of varchar hence we need o convert it into date
```

**Find all the Movies/TV shows by director 'Rajiv Chilaka'**

```sql
Select title, director
from netflix
where director like '%Rajiv Chilaka%'
```

**list all TV Shows with more than 5 seasons**

```sql
SELECT types, title, duration
FROM netflix
WHERE types = 'TV Show'
AND LEFT(duration, CHARINDEX(' ', duration) - 1) > 5
```

**Count the number of content items in each genere**

```sql
Select TOP 5
trim(value) as genre,
count(*) as Total_content
from netflix
cross apply string_split(                           --cross apply allows you to: run a table valued function for each row of the table
						replace(listed_in, '&',',') -- replace(originaltext, text to find, new text)
						,',')                      -- String_split(string, seperator)
group by trim(value) 
order by count(*) desc;


Select * from netflix;
```

**Find each year and the average number of content release by India on netflix return top 5 year with highest average content release**

```sql
Select Top 5
Year(
		cast(date_added as date) -- change the date_added column to date datatype from varchar
		
	) as Year_added,
	count(*) as total_count,
	Round(
			cast(count(*) as float) / cast((Select count(*) from netflix where country like '%India%') as float) * 100,2) as Average_count 
from netflix
where country like '%India%'
group by Year(
		cast(date_added as date))
order by 'Average_count' desc;
```

**list all the movies that are documentary**

```sql
Select
*
from netflix
where types = 'Movie' and listed_in like '%Documentaries%';
```

**Find all the content without a director**

```sql
Select * from netflix
where director is null;
```
**Find how many movies actor 'Salman Khan' appeared in last 10 years**

```sql
Select *
from
Netflix
where casts like '%Salman Khan%' and release_year >= Year(GETDATE())-10
```

--Get the last 10th year from today]] -- Just for information/practice purpose not the task

```sql
select Year(
DATEADD(Year,-10,GETDATE()))

--or

Select year(getdate())-10

```
**Find the top 10 actors who have appeared in the highest number of  movies produced in India**

```sql

Select Top 10
trim(value) as casts,
count(*) as total_no_movies
from Netflix
cross apply string_split(casts,',')
where country like '%India%' and types = 'Movie'
group by trim(value)
order by count(*) desc;
```

**categorise the content based on the presence of the keywords 'Kill' and 'Violence' in the description field. label content 
containig these keyword as 'Bad' and all other content as 'good'. Count how many items fall into each category.**

```sql

Select
case
when description like '%kill%' or description like '%violence%'
then 'Bad Content'
Else 'Good Content'
End as content_feedback, 
count(*) as Total_count
from netflix
group by 
case
when description like '%kill%' or description like '%violence%'
then 'Bad Content'
Else 'Good Content'
End
```


