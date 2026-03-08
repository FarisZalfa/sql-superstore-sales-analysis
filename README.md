# sql-superstore-sales-analysis

# 📊 Sample - Superstore Sales Analysis

![SQL](https://img.shields.io/badge/SQL-PostgreSQL-blue)
![Portfolio](https://img.shields.io/badge/Portfolio-Data%20Analyst-brightgreen)

## 📌 Project Overview
This project simulates a real-world business scenario where I acted as a Data Analyst for a retail superstore. The objective was to identify profit drivers, loss areas, and actionable strategies to improve margins.

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
| **Technology** leads profits | $145,454 profit at 17.4% margin | Stock more tech products, create bundles |
| **Furniture** underperforms | Only 2.49% margin, Tables lose (~-17% margin) | Renegotiate with suppliers, reduce discounts |
| **Copiers** are top product | The Canon imageCLASS 2200 Advanced Copier generated $25K+ profit from just 5 orders | Indicates extremely high-margin products |
| **West region** dominates | 37% of total profit, highest margins | Study West strategies for other regions |
| **Discounts > 50%** hurt profits | Products with 70-80% discounts lose money | Cap discounts at 40%, require approval |
| **Office Supplies** steady | 17% margin with consistent sales | Maintain inventory, focus on bundling |

# Category Profit Performance
![image alt](https://github.com/FarisZalfa/sql-superstore-sales-analysis/blob/c70a627a4a18524c60af53870a16787ebe82eee6/category_profit.png)

# Discount Impact Analysis
![image alt](https://github.com/FarisZalfa/sql-superstore-sales-analysis/blob/c70a627a4a18524c60af53870a16787ebe82eee6/discount_impact.png)

# Discount Impact Analysis
![image alt](https://github.com/FarisZalfa/sql-superstore-sales-analysis/blob/c70a627a4a18524c60af53870a16787ebe82eee6/regional_performance.png)

## 💡 Business Recommendations
- **Reduce Furniture inventory** - Low margins (2.49%), Tables sub-category loses 17%. The company should review pricing strategy, supplier costs, and discount policies before expanding inventory in this category.
- **Expand Technology section** - Technology products generate the highest profit margins (17.4%) and strong sales volume. Increasing product availability and cross-selling accessories may further increase revenue.
- **Investigate Tables** - The Tables sub-category shows negative profitability despite high sales volume. This suggests pricing issues, excessive discounting, or high supplier costs. Further investigation is recommended.
- **West region strategies** - 38% of total profit, outperforming other regions. Further analysis may identify operational or pricing strategies that could be replicated in underperforming regions.
- **Promote high-margin copier products** – The Canon imageCLASS 2200 Advanced Copier generated over $25K profit from just 5 orders, suggesting copier products have very high margins. Targeting business customers and bundling with office equipment could increase revenue.
- **Cap discounts at 40%** - Discounts above 40–50% are strongly associated with negative profit margins. Implementing discount controls or approval processes for high discounts may help protect profitability.

## 📁 Data Source
Dataset: [Superstore Dataset](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final)

The dataset contains 9,994 sales transactions across 21 variables including product category, region, discount, and profit.

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
