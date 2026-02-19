# sql-superstore-sales-analysis

# 📊 Sample - Superstore Sales Analysis

![SQL](https://img.shields.io/badge/SQL-PostgreSQL-blue)
![Portfolio](https://img.shields.io/badge/Portfolio-Data%20Analyst-brightgreen)

## 📌 Project Overview
Analyzed sales performance for a fictional superstore using PostgreSQL. Identified profitable products, regional performance, and areas losing money to provide actionable business insights.

## 🛠️ Skills Demonstrated
- **SQL:** Aggregations, CASE statements, Subqueries, CTEs
- **Data Analysis:** Profit margin calculations, performance tiering
- **PostgreSQL:** Table creation, CSV import, data type handling

## 🔍 Key Questions Answered
1. What is the overall profitability of the superstore?
2. Which product categories and sub-categories perform best?
3. What are our top 5 most profitable products?
4. How do different regions compare in sales and profit?
5. How do discounts impact profitability?
6. Which products are losing money and why?

## 📈 Key Findings

| Finding | Result | Business Impact |
|--------|--------|-----------------|
| **Technology** leads profits | $145,455 profit at 17.4% margin | Stock more tech products, create bundles |
| **Furniture** underperforms | Only 3.8% margin, Tables lose 18% | Renegotiate with suppliers, reduce discounts |
| **Copiers** are top product | $25K+ profit from just 68 orders | Cross-sell to existing customers |
| **West region** dominates | 31% of total profit, highest margins | Study West strategies for other regions |
| **Discounts > 50%** hurt profits | Products with 70-80% discounts lose money | Cap discounts at 40%, require approval |
| **Office Supplies** steady | 17% margin with consistent sales | Maintain inventory, focus on bundling |

## 💡 Business Recommendations
- **Reduce Furniture inventory** - low margins (3.8%), Tables sub-category loses 18%
- **Expand Technology section** - highest profit margins (17.4%) and sales
- **Investigate Tables** - losing money despite high sales, check suppliers/pricing
- **West region strategies** - 31% of total profit, replicate in other regions
- **Copiers are gold** - $25K+ profit from 68 orders, bundle with accessories
- **Cap discounts at 40%** - discounts above 50% consistently lose money

## 📁 Data Source
Dataset: [Superstore Dataset](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final)

## 🖥️ SQL Queries

```sql
-- 1. Overall Performance Snapshot
SELECT 
    COUNT(DISTINCT order_id) AS total_orders,
    ROUND(SUM(sales)::numeric, 2) AS total_sales,
    ROUND(SUM(profit)::numeric, 2) AS total_profit,
    ROUND((SUM(profit) / SUM(sales) * 100)::numeric, 2) AS profit_margin_percent
FROM superstore_orders;

-- 2. Category Performance
SELECT 
    category,
    ROUND(SUM(sales)::numeric, 2) AS total_sales,
    ROUND(SUM(profit)::numeric, 2) AS total_profit,
    ROUND((SUM(profit) / SUM(sales) * 100)::numeric, 2) AS profit_margin
FROM superstore_orders
GROUP BY category
ORDER BY total_profit DESC;

-- 3. Sub-Category Performance
SELECT 
    category,
    sub_category,
    ROUND(SUM(sales)::numeric, 2) AS total_sales,
    ROUND(SUM(profit)::numeric, 2) AS total_profit,
    ROUND((SUM(profit) / SUM(sales) * 100)::numeric, 2) AS profit_margin
FROM superstore_orders
GROUP BY category, sub_category
ORDER BY profit_margin;

-- 4. Top 5 Most Profitable Products
SELECT 
    sub_category,
    product_name,
    ROUND(SUM(profit)::numeric, 2) AS total_profit,
    COUNT(DISTINCT order_id) AS number_of_orders
FROM superstore_orders
GROUP BY sub_category, product_name
ORDER BY total_profit DESC
LIMIT 5;

-- 5. Regional Performance
SELECT 
    region,
    ROUND(SUM(sales)::numeric, 2) AS total_sales,
    ROUND(SUM(profit)::numeric, 2) AS total_profit,
    ROUND(100.0 * SUM(profit) / (SELECT SUM(profit) FROM superstore_orders), 1) AS profit_percentage
FROM superstore_orders
GROUP BY region
ORDER BY total_profit DESC;

-- 6. Discount Impact Analysis
SELECT 
    discount,
    ROUND(SUM(sales)::numeric, 2) AS total_sales,
    ROUND(SUM(profit)::numeric, 2) AS total_profit,
    COUNT(*) AS number_of_orders,
    ROUND(AVG(profit)::numeric, 2) AS avg_profit_per_order
FROM superstore_orders
GROUP BY discount
ORDER BY discount;

-- 7. Customer Segment Analysis
SELECT 
    segment,
    COUNT(DISTINCT customer_id) AS customer_count,
    ROUND(SUM(sales)::numeric, 2) AS total_sales,
    ROUND(SUM(profit)::numeric, 2) AS total_profit,
    ROUND((SUM(profit) / SUM(sales) * 100)::numeric, 2) AS profit_margin
FROM superstore_orders
GROUP BY segment
ORDER BY total_profit DESC;
