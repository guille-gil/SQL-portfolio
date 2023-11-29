### Hypothesis 4
1. **Breakfast vs Lunch vs Dinner**

To investigate the comparison between the different meal times, the following query is utilised. 

```sql
-- SQL Query for Time of Day Analysis
SELECT
  time_day,
  month,
  total_revenue,
  revenue_rank,
  previous_rev,
  ROUND(((total_revenue - previous_rev) / previous_rev) * 100) AS percentage_change
FROM
  (
    SELECT
      month,
      total_revenue,
      time_day,
      RANK() OVER (PARTITION BY month ORDER BY total_revenue DESC) AS revenue_rank,
      LAG(total_revenue) OVER (PARTITION BY time_day) AS previous_rev
    FROM
      (
        SELECT
          CASE
            WHEN HOUR(order_time) BETWEEN 0 AND 6 THEN 'night'
            WHEN HOUR(order_time) BETWEEN 7 AND 12 THEN 'breakfast'
            WHEN HOUR(order_time) BETWEEN 13 AND 18 THEN 'lunch'
            WHEN HOUR(order_time) BETWEEN 19 AND 23 THEN 'dinner'
          END AS time_day,
          MONTH(order_date) AS month,
          ROUND(SUM(final_price), 0) AS total_revenue
        FROM orders
        GROUP BY month, time_day
      ) q1
  ) q2
ORDER BY month, revenue_rank;
```

Query Explanation:

1. **Categorizing Time of Day:** The innermost query (q1) categorizes orders based on the time of day (night, breakfast, lunch, dinner).
2. **Calculating Total Revenue:** Sums up the final_price to calculate the total revenue for each month and time of day.
3. **Ranking Revenue:** Utilizes the RANK() window function to rank total revenue within each month and time of day.
4. **Calculating Previous Revenue:** Uses the LAG() window function to retrieve the previous month's revenue for comparison.
5. **Calculating Percentage Change:** Calculates the percentage change in revenue compared to the previous month.

This analysis aims to uncover patterns and fluctuations in revenue based on different times of the day. 

The results reveal variations in total revenue across different times of the day and months:

| Time of Day | Month | Total Revenue | Revenue Rank | Previous Revenue | Percentage Change |
|-------------|-------|---------------|--------------|-------------------|-------------------|
| night       | 6     | 110,714       | 1            | NULL              | NULL              |
| lunch       | 6     | 94,827        | 2            | NULL              | NULL              |
| breakfast   | 6     | 94,021        | 3            | NULL              | NULL              |
| dinner      | 6     | 48,016        | 4            | NULL              | NULL              |
| night       | 7     | 95,625        | 1            | 110,714           | -14               |
| breakfast   | 7     | 86,458        | 2            | 94,021            | -8                |
| lunch       | 7     | 85,633        | 3            | 94,827            | -10               |
| dinner      | 7     | 40,885        | 4            | 48,016            | -15               |
| night       | 8     | 90,736        | 1            | 95,625            | -5                |
| lunch       | 8     | 78,404        | 2            | 85,633            | -8                |
| breakfast   | 8     | 76,378        | 3            | 86,458            | -12               |
| dinner      | 8     | 37,847        | 4            | 40,885            | -7                |
| night       | 9     | 83,388        | 1            | 90,736            | -8                |
| breakfast   | 9     | 71,282        | 2            | 76,378            | -7                |
| lunch       | 9     | 67,687        | 3            | 78,404            | -14               |
| dinner      | 9     | 35,804        | 4            | 37,847            | -5                |

For each month, the 'night' time of day consistently ranks as the top revenue contributor. There are noticeable fluctuations in revenue rankings, indicating shifts in customer behavior during specific times of the day. Percentage change values highlight the month-over-month variations, with negative values indicating a decrease compared to the previous month.


---


