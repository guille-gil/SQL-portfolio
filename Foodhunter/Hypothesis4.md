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


2. **Vegetarian VS Non-Vegetarian**

To understand food preferences per customer over the months (veg or non veg), we need to find the number of orders per food type over the months joining different tables. 

```sql
-- SQL Query for Food Preferences & Quality Analysis (Part 2)
SELECT
  fi.food_type_new,
  SUM(oi.quantity) AS items_quantity
FROM orders_items oi
LEFT JOIN
  (
    SELECT
      item_id,
      CASE
        WHEN food_type LIKE 'veg%' OR food_type = 'vegetarian' THEN 'veg'
        ELSE 'non veg'
      END AS food_type_new
    FROM food_items
  ) fi
ON oi.item_id = fi.item_id
GROUP BY fi.food_type_new;
```

Query Explanation:

1. **Joining Tables:** The query performs a left join between the orders_items table (oi) and a subquery (fi) that categorizes food items into 'veg' or 'non veg' based on the food_type column in the food_items table.
2. **Categorizing Food Types:** The subquery (fi) assigns a new category (food_type_new) to each food item, distinguishing between vegetarian ('veg') and non-vegetarian ('non veg'). To handle variations in input data formats, the LIKE operator is used to match 'veg' and 'vegetarian'.
3. **Summing Item Quantities:** The main query calculates the total quantity of items for each food type category.

This analysis takes into account different formats of input data ('veg' and 'vegetarian') when categorizing food types, ensuring a comprehensive and accurate representation of customer food preferences. The results are:

| Food Type | Items Quantity |
|-----------|----------------|
| veg       | 23,239         |
| non veg   | 63,077         |

Customers seem to have a substantial preference for non-vegetarian items, as evidenced by the higher quantity in this category compared to vegetarian items.


---


### Conclusion

In conclusion, the combined findings strongly support the hypothesis that customer preferences play a pivotal role in ordering behavior. Therefore, we **accept hypothesis 4**. The integration of food type preferences and time-of-day patterns offers a holistic view, guiding strategic decisions for menu planning, marketing, and overall business optimization.

Potential corrective actions might include:
- Given the clear preference for non-vegetarian items, optimizing menu offerings and marketing strategies to align with this preference may enhance customer satisfaction and revenue.
- Addressing the observed decline in evening revenue through targeted promotions or menu adjustments could further contribute to business success.


