# Movies Data Analysis Dashboard Project

This project analyzes a movie dataset containing information about both **Hollywood** and **Bollywood** films, along with some movies of unknown industry origin. The goal was to create meaningful insights through **SQL data analysis** and an interactive **Power BI dashboard** to explore trends, performance metrics, and key business indicators in the movie industry.

## Problem Statement

The raw movie dataset contained several challenges:

**•** Budget and revenue values were inconsistent in currency (USD, INR) and units (Millions, Billions, Thousands).  
**•** No unified financial metrics to compare movies across industries.  
**•** Difficulty identifying trends in profitability and ROI across years and industries.  
**•** Lack of consolidated information about actors’ participation in movies.  

This project aimed to clean, transform, and analyze the data to produce reliable KPIs and insightful visuals for the movie industry.

## Project Goals

**•** Standardize financial figures into a single unit and currency for consistent analysis.  
**•** Create key metrics like Profit, ROI, and Profit Margin for every movie.  
**•** Visualize industry-wise trends for revenue, ROI, and movie ratings.  
**•** Identify prolific actors and analyze their participation in movies.  
**•** Enable easy exploration of movie data through an interactive Power BI dashboard.

## Data Cleaning and ETL Process

The data preparation process included:

**•** Loading raw data into Power BI from CSV files and SQL queries.  
**•** Cleaning missing and inconsistent values in key columns.  
**•** Standardizing budget and revenue figures:

    - Converted all financial values into **Millions of INR**.
    - Applied exchange rate of **1 USD = 70 INR**.
    - Adjusted figures based on unit (Thousands, Millions, Billions).

**•** Created calculated columns in Power BI for:

    - Normalized Budget
    - Normalized Revenue
    - Profit (Revenue – Budget)
    - ROI (Profit / Budget)
    - Profit Margin (Profit / Revenue)

## SQL Queries Used

### Retrieve All Movies

```sql
SELECT * FROM movies;
```

### Identify Actors with Less than 2 Movies in a Year

```sql
SELECT a.name, m.release_year, COUNT(m.title) AS movies_count
FROM movie_actor ma
JOIN actors a ON ma.actor_id = a.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
GROUP BY a.name, m.release_year
HAVING movies_count < 2
ORDER BY movies_count DESC;
```

### Studio Producing Most Movies

```sql
SELECT studio, COUNT(title) AS total
FROM movies 
GROUP BY studio 
ORDER BY total DESC
LIMIT 1;
```

### Average IMDB Rating by Industry (Post-2010)

```sql
SELECT industry, AVG(imdb_rating) AS avg_rating
FROM movies
WHERE release_year > 2010
GROUP BY industry
ORDER BY avg_rating DESC;
```

### Actors Appearing in 3 or More Movies

```sql
SELECT a.name, COUNT(m.title) AS movies_count
FROM movie_actor ma
JOIN actors a ON ma.actor_id = a.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
GROUP BY a.name
HAVING COUNT(m.title) >= 3
ORDER BY COUNT(m.title) DESC;
```

### Top 5 Movies by IMDB Rating with Normalized Budget

```sql
SELECT m.title, m.imdb_rating,
    (CASE 
        WHEN f.unit = 'Billions' THEN f.budget * 1000
        WHEN f.unit = 'Thousands' THEN f.budget / 1000
        ELSE f.budget
    END *
    CASE
        WHEN f.currency = 'USD' THEN 70
        ELSE 1
    END) AS budget_mln_inr
FROM movies m
JOIN financials f ON m.movie_id = f.movie_id
ORDER BY m.imdb_rating DESC
LIMIT 5;
```

### Movies and Their Actors (Single Pairing)

```sql
SELECT m.title, a.name
FROM movie_actor ma
JOIN actors a ON ma.actor_id = a.actor_id
JOIN movies m ON ma.movie_id = m.movie_id;
```

### Movies and All Actors (Grouped)

```sql
SELECT 
    m.title, 
    GROUP_CONCAT(a.name ORDER BY a.name SEPARATOR ', ') AS actors
FROM movie_actor ma
JOIN actors a ON ma.actor_id = a.actor_id
JOIN movies m ON ma.movie_id = m.movie_id
GROUP BY m.title;
```

## Dashboard Toolkit

**•** Power BI Desktop – For creating interactive visuals and reports.  
**•** Power Query – To transform and clean data prior to visualization.  
**•** DAX (Data Analysis Expressions) – To compute calculated columns and KPIs.  
**•** Data Modeling – For establishing relationships among tables and enabling cross-filtering.

## Dashboard Features

The Power BI dashboard includes:

**•** Financial KPIs (Profit, ROI, Profit Margin) for every movie.  
**•** Year-wise trend analysis of ROI and revenue.  
**•** Industry-wise revenue share (Hollywood, Bollywood, Others).  
**•** IMDB rating distributions across industries.  
**•** Top actors based on number of movies.  
**•** Visuals to explore movies, budgets, revenues, and ratings interactively.

## Impact and Benefits

**•** Unified financial metrics enable direct comparison across movies.  
**•** Interactive visuals support insights into industry performance and trends.  
**•** Helps stakeholders understand financial outcomes and ROI of movie projects.  
**•** Enables quick identification of high-performing movies and actors.

## Dashboard Screenshot

![Dashboard Screenshot](path/to/your/dashboard-image.png)

## Source of Data

[Link to Data Source](https://codebasics.io/resources/sql-tutorials-for-beginners)
