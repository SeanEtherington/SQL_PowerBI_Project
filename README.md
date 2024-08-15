# SQL Project: HR Data Analysis 

In this project, the goal was to clean and query the HR data using SQL and then use the results for visual analysis in Power BI. Below is an overview of the key SQL steps followed during this process.


## 2. Database Setup and Initial Data Import 
A new database named `projects` was created, and HR data was uploaded into a table called `hr`. This table stores all employee records.

```sql
CREATE DATABASE projects;
USE projects;
SELECT * FROM hr;
```

## 2. Data Cleaning 

-- The data required significant cleaning to ensure consistency.

-- Renaming Columns: Corrected the `id` column to `emp_id` for clarity.
ALTER TABLE hr CHANGE COLUMN ï»¿id emp_id VARCHAR(20) NULL;

-- Date Formatting: Birthdate, hire date, and termination date columns were cleaned by standardizing the formats to `YYYY-MM-DD`. Any invalid or incomplete date entries were set to `NULL`.

```sql
UPDATE hr
SET birthdate = CASE 
    WHEN birthdate LIKE '%/%' THEN date_format(str_to_date(birthdate, '%m/%d/%Y'), '%Y-%m-%d')
    WHEN birthdate LIKE '%-%' THEN date_format(str_to_date(birthdate, '%m-%d-%Y'), '%Y-%m-%d')
    ELSE NULL
END;
```

## 3. Adding Calculated Fields 
-- An `age` column was added to the data to support age-based analysis.
-- This was done by calculating the difference between birth dates and the current date.
```sql
ALTER TABLE hr ADD COLUMN age INT;
UPDATE hr SET age = timestampdiff(YEAR, birthdate, CURDATE());
```
## 4. Querying for Power Bi Insights  

-- Once the data was cleaned, several queries were written to gather key insights for Power BI visualizations.

-- Gender Breakdown: Identifying the gender distribution of active employees.
```sql
SELECT gender, COUNT(*) AS gender_count
FROM hr
WHERE termdate IS NULL
GROUP BY gender;
```
-- Age Distribution: Grouping employees by age brackets for better visualization.
```sql
SELECT
    CASE
        WHEN age BETWEEN 18 AND 24 THEN '18-24'
        WHEN age BETWEEN 25 AND 34 THEN '25-34'
        WHEN age BETWEEN 35 AND 44 THEN '35-44'
        WHEN age BETWEEN 45 AND 54 THEN '45-54'
        WHEN age BETWEEN 55 AND 64 THEN '55-64'
        ELSE '65+'
    END AS age_group,
    COUNT(*) AS count
FROM hr
WHERE termdate IS NULL
GROUP BY age_group
ORDER BY age_group;
```

## 5. Querying for Power Bi Insights  

The result sets from these queries were exported as CSV files and used as data sources in Power BI for deeper analysis and visualization.

