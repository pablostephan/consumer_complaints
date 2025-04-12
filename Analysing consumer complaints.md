# 📊 Consumer Complaints Analysis

This project performs a monthly analysis of consumer complaints, comparing complaint volumes against historical averages.

## 📋 SQL Code Overview

The SQL code performs the following main operations:

1. Calculates the number of complaints per month
2. Determines the overall monthly complaint average
3. Compares each month against the general average
4. Classifies months as above/below average
5. Shows a sample of the original data

## 🔍 Detailed Explanation

### 1. CTE `monthly_summary`

```sql
WITH monthly_summary AS (
  SELECT 
    STRFTIME('%Y-%m', 
             SUBSTR(date_received, 7, 4) || '-' ||  -- Extracts year (last 4 characters)
             SUBSTR(date_received, 1, 2) || '-' ||  -- Extracts month (first 2 characters)
             SUBSTR(date_received, 4, 2)) AS receipt_month,  -- Extracts day (position 4-5)
    COUNT(*) AS complaint_count
  FROM consumer_complaints
  WHERE date_received IS NOT NULL
  GROUP BY receipt_month
)
```

**Function**:  
Transforms dates from MM-DD-YYYY format to YYYY-MM and counts complaints per month.

### 2. CTE `overall_average`

```sql
overall_average AS (
  SELECT AVG(complaint_count) AS avg_complaints
  FROM monthly_summary
)
```

**Function**:  
Calculates the monthly complaint average based on aggregated data.

### 3. Main Query

```sql
SELECT 
  m.receipt_month,
  m.complaint_count,
  ROUND(a.avg_complaints) AS avg_complaints,
  CASE 
    WHEN m.complaint_count > a.avg_complaints THEN 
      ROUND(((m.complaint_count - a.avg_complaints) / a.avg_complaints) * 100, 1)
    WHEN m.complaint_count < a.avg_complaints THEN 
      ROUND(((a.avg_complaints - m.complaint_count) / a.avg_complaints) * 100, 1)
    ELSE 0
  END AS percentage_difference,
  CASE 
    WHEN m.complaint_count > a.avg_complaints THEN 'Above average'
    WHEN m.complaint_count < a.avg_complaints THEN 'Below average'
    ELSE 'Equal to average'
  END AS status
FROM monthly_summary m
JOIN overall_average a
ORDER BY m.complaint_count DESC;
```

**Resulting fields**:
- `receipt_month`: Month/year in YYYY-MM format
- `complaint_count`: Number of complaints in the month
- `avg_complaints`: Rounded monthly average
- `percentage_difference`: Percentage variation compared to average
- `status`: Text classification (Above/Below/Equal to average)

### 4. Data Sample Query

```sql
SELECT *
FROM consumer_complaints
LIMIT 10;
```

**Purpose**:  
Shows a sample of raw data for reference.

## 📊 Results Interpretation

The results allow identification of:
- Months with complaint peaks
- Seasonal patterns
- Significant deviations from the average
- Trends over time

## 🛠️ Technologies Used

- SQL (SQLite syntax)
- Date functions (STRFTIME, SUBSTR)
- Aggregations (COUNT, AVG)
- Conditional expressions (CASE WHEN)
