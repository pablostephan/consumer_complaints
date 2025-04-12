# 📊 Monthly Consumer Complaints Analysis

This project presents a structured SQL-based analysis of consumer complaint data to identify patterns and anomalies in complaint volume over time.

## 🔍 Objective

The goal of this project is to analyze the monthly volume of complaints received by consumers and compare them against the overall monthly average, in order to:

- Identify months with unusually high or low complaint volumes.
- Calculate the percentage deviation from the average.
- Provide insights for decision-making or further investigation.

## 🛠️ Tools Used

- SQL (SQLite dialect)
- DBeaver

## 📁 Dataset

The dataset used is assumed to have a column named date_received, which represents the date each complaint was submitted. The table is named consumer_complaints.

## 📌 Key Steps
**1. Data Aggregation:**

Monthly aggregation of the number of complaints using `STRFTIME()` to format dates.

**2. Average Calculation**
  
Compute the general average of monthly complaints.

**3. Comparison & Analysis**
  
Filter months that had more or less complaints than the average and calculate the percentage difference.

**4. Result Interpretation**

Classify each month as “Above Average”, “Below Average”, or “Equal to Average”.

## 🧠 Insights
This analysis can help stakeholders or regulatory agencies to:

- Detect seasonal trends in complaints.

- Investigate spikes or drops in complaint volumes.

- Align company resources and customer support strategies.
