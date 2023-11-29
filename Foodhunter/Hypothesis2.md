### Hypothesis 2: Revenue-related Factors

1. **Analyzing the Effect of Discounts**
To investigate the impact of discounts on the final revenue, we'll use the following SQL query:

```sql
-- SQL Query for Discount Analysis
SELECT
  MONTH(order_date) AS month,
  ROUND(SUM(final_price), 2) AS TotalRevenue,
  ROUND(SUM(discount) / SUM(final_price), 2) AS discount_sales_ratio,
  ROUND(SUM(discount), 2) AS TotalDiscount,
  COUNT(order_id) AS OrderCount
FROM orders
GROUP BY month
ORDER BY month;
```

Query Explanation:

1. **Selecting Month:** Extracts the month from the order_date column.
2. **Calculating Total Revenue:** Sums up the final_price to calculate the total revenue for each month.
3. **Calculating Discount Sales Ratio:** Computes the ratio of the total discount to the total revenue, providing insight into the impact of discounts on sales.
4. **Calculating Total Discount:** Determines the overall discount for each month.
5. **Counting Orders:** Counts the number of orders for each month.

This analysis helps us understand whether the application of discounts has resulted in a noticeable change in total revenue, and it provides a ratio indicating the proportion of sales influenced by discounts.

The results from this query reveal stable trends in the relationship between discounts and revenue across multiple months:

| Month | Total Revenue | Discount Sales Ratio | Total Discount | Order Count |
|-------|---------------|-----------------------|-----------------|-------------|
| 6     | 347,577.5     | 0.14                  | 47,795          | 12,502      |
| 7     | 308,601.5     | 0.14                  | 41,730          | 11,144      |
| 8     | 283,365.9     | 0.14                  | 39,142.6        | 10,107      |
| 9     | 258,161.1     | 0.14                  | 35,012.4        | 9,365       |

The consistent discount sales ratio of 0.14 indicates a stable relationship between discounts and total revenue. Despite fluctuations in total revenue, the proportion of revenue influenced by discounts remains relatively constant.


---


### 2. Analyzing Price Levels
To assess whether prices are too high, we'll use the following SQL query:

```sql
-- SQL Query for Price Analysis
SELECT
  ROUND(AVG(final_price), 2) AS AveragePrice,
  MAX(final_price) AS MaxPrice,
  MIN(final_price) AS MinPrice
FROM orders;
```

Query Explanation:

1. **Calculating Average Price:** Computes the average final price across all orders.
2. **Finding Maximum Price:** Identifies the highest final price in the dataset.
3. **Finding Minimum Price:** Identifies the lowest final price in the dataset.

This analysis provides a snapshot of the overall pricing structure, helping us determine whether the prices are within a reasonable range or if there are outliers that might contribute to a negative impact on revenue.

The results for this query are:

| Average Price | Max Price | Min Price |
|---------------|-----------|-----------|
| 27.78         | 120       | 8         |

The average price of $27.78, along with the observed maximum and minimum prices, suggests a reasonable pricing structure that aligns with market competitiveness.


---


## Conclusion
In light of these findings, we **reject Hypothesis 2**, as there is no substantial evidence to support concerns about the negative impact of discounts on revenue or excessively high prices. The data indicates a balanced approach to discounts and competitive pricing, contributing positively to overall revenue.
