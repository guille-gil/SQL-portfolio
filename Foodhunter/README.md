# FoodHunter SQL Project

## Introduction

Welcome to the SQL project centered around FoodHunter, a well-funded food delivery app known for its expansive range of restaurant options featuring diverse cuisines and prompt delivery services. Despite previous success, FoodHunter has observed a consistent decline in monthly revenues over the last quarter. In response to this challenge, the company aims to uncover the various factors contributing to this downturn.

Our objective is to analyze the available relational database at the order level, including essential details such as cuisines, restaurants, delivery times, ratings, and more. This dataset is structured across six distinct tables within a relational database, offering a comprehensive snapshot of the company's operational landscape.

## Exploration Hypotheses

To guide our investigation, we have formulated six initial hypotheses or potential areas of concern that could be contributing to the observed trends:

1. **Time-based Problems:** Explore whether specific times of the day or days of the week correlate with decreased revenue.

2. **Revenue-related Factors:** Investigate any financial elements, such as pricing changes or promotional strategies, influencing the decline in revenues.

3. **Delivery Partner Problems:** Examine the performance of delivery partners to identify any issues affecting delivery times or customer satisfaction.

4. **Food Preferences & Quality:** Analyze the impact of different cuisines, restaurants, and overall food quality on customer satisfaction and repeat business.

5. **Marketing Factors:** Assess the effectiveness of marketing efforts and campaigns in attracting and retaining customers.

6. **Customer Reviews:** Investigate how customer reviews and ratings contribute to the overall revenue trends.

These hypotheses serve as the starting point for our exploration within the database. We will delve into the data to validate, refine, or discover new insights related to each hypothesis, aiming to provide actionable recommendations for FoodHunter. Stay tuned for updates as we progress through the various stages of this SQL project!

## Database Schema

Below is an illustration of the relational database schema for FoodHunter's dataset:

![Database Schema](https://github.com/guille-gil/SQL-portfolio/raw/main/Foodhunter/schema.png)

By delving into these hypotheses and thoroughly analyzing the data, we aim to provide actionable insights and recommendations for FoodHunter to reverse the downward revenue trend and enhance overall business performance. Stay tuned for updates as we progress through the various stages of this SQL project!



## Hypothesis 1: Time-based Problems
To explore if specific times of the day or days of the week correlate with decreased revenue, we'll analyze the **orders table**, focusing on the *order_id*, *order_time*, and *delivered_time* columns.


```sql
-- SQL Query Example
SELECT
  column1,
  column2,
  SUM(column3) AS total_column3
FROM
  your_table
WHERE
  condition = 'some_value'
GROUP BY
  column1, column2
ORDER BY
  total_column3 DESC;
```

1. **Selecting Day of Week:** Extracts the day of the week using the DAYOFWEEK function from the order_time column.
2. **Counting Orders:** Calculates the number of orders for each day of the week using the COUNT function.
3. **Calculating Average Delivery Time:** Computes the average delivery time in minutes using the TIMESTAMPDIFF function.
4. **Grouping and Ordering:** Groups results by the day of the week to identify trends. Orders results for better analysis.

This query helps determine if certain days of the week exhibit longer delivery times or lower order counts, potentially contributing to decreased revenue. Adjust the time units or conditions based on your dataset's specifics.
