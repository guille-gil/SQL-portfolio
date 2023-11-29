## Hypothesis 1: Time-based Problems
To explore if specific times of the day or days of the week correlate with decreased revenue, we'll analyze the **orders table**, focusing on the *order_id*, *order_time*, and *delivered_time* columns.


```sql
-- SQL Query for Hypothesis 1
SELECT
  DAYOFWEEK(order_time) AS day_of_week,
  COUNT(order_id) AS order_count,
  AVG(TIMESTAMPDIFF(MINUTE, order_time, delivered_time)) AS avg_delivery_time
FROM orders
GROUP BY day_of_week
ORDER BY day_of_week;
```

1. **Selecting Day of Week:** Extracts the day of the week using the DAYOFWEEK function from the order_time column.
2. **Counting Orders:** Calculates the number of orders for each day of the week using the COUNT function.
3. **Calculating Average Delivery Time:** Computes the average delivery time in minutes using the TIMESTAMPDIFF function.
4. **Grouping and Ordering:** Groups results by the day of the week to identify trends. Orders results for better analysis.

This query helps determine if certain days of the week exhibit longer delivery times or lower order counts, potentially contributing to decreased revenue. Adjust the time units or conditions based on your dataset's specifics.
