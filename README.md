# SQL-AND-POWER-BI-PROJECT

### Project Objectives

The primary objectives of this project are to analyze the Superstore sales dataset and derive meaningful insights that can inform business decisions. Key focus areas include understanding sales trends, customer behavior, regional performance, and product popularity.

### Data Source
Superstoreorders: The primary dataset used for this analysis is "SuperStoreOrders.csv" file, containing detailed information about each sale made by the company. Dataset was downloaded on kaggle.com.

### Tools

  - MYSQL - Data cleaning, preprocessing and Data Analysis [Download here](https://dev.mysql.com/downloads/installer/)
- POWER BI - Creating reports and visualization [Download here](https://powerbi.microsoft.com/en-us/desktop/)

### Dataset Overview
Columns
- order_id: Unique identifier for each order.
- order_date: Date when the order was placed.
- ship_date: Date when the order was shipped.
- ship_mode: Shipping mode for the order.
- customer_name: Name of the customer placing the order.
- segment: Customer segment (e.g., consumer, corporate, home office).
- state: State where the order is placed.
- country: Country of the customer.
- market: Market segment (e.g., US, EU, APAC).
- region: Geographical region.
- product_id: Unique identifier for each product.
- category: Product category (e.g., office supplies, furniture, technology).
- sub_category: Sub-category of the product.
- product_name: Name of the product.
- sales: Sales revenue for the order.
- quantity: Quantity of products ordered.
- discount: Discount applied to the order.
- profit: Profit generated from the order.
- shipping_cost: Cost of shipping.
- order_priority: Priority of the order.
- year: Year of the order.

### Data cleaning and preprocessing

1. Changing Column Data Type
2. CHANGE COLUMN
3. Updating Date Columns

```sql
-- Change the data type of 'order_id' to VARCHAR(20).
ALTER TABLE sales
CHANGE COLUMN `order_id` order_id VARCHAR(20) NULL;
Note: This query modifies the data type of the 'order_id' column to VARCHAR(20).

Updating Date Columns
sql
Copy code
-- Update 'order_date' column with modified date values.
-- Update 'ship_date' column with modified date values.

SET SQL_SAFE_UPDATES = 0;

-- Update 'order_date' column
UPDATE sales
SET order_date = 
  CASE
    WHEN order_date LIKE '%/%' THEN STR_TO_DATE(order_date, '%m/%d/%Y')
    WHEN order_date LIKE '%-%' THEN STR_TO_DATE(order_date, '%d-%m-%Y')
    WHEN order_date LIKE '%/%/%' THEN STR_TO_DATE(order_date, '%d/%m/%Y')
    ELSE NULL
  END;

-- Modify 'order_date' column to DATE type
ALTER TABLE sales
MODIFY COLUMN order_date DATE;

-- Update 'ship_date' column
UPDATE sales
SET ship_date = 
  CASE
    WHEN ship_date LIKE '%/%' THEN STR_TO_DATE(ship_date, '%m/%d/%Y')
    WHEN ship_date LIKE '%-%' THEN STR_TO_DATE(ship_date, '%d-%m/%Y')
    WHEN ship_date LIKE '%/%/%' THEN STR_TO_DATE(ship_date, '%d/%m/%Y')
    ELSE NULL
  END;

-- Modify 'ship_date' column to DATE type
ALTER TABLE sales
MODIFY COLUMN ship_date DATE;
Note: These queries update and standardize the date format for 'order_date' and 'ship_date' columns.
```
 

### Data Analysis

include some interesting code/features worked with

```sql
SELECT 
    SUM(sales) AS total_sales,
    SUM(profit) AS total_profit
FROM 
    sales;


Note: This query returns the total sales and profit for all transactions in the dataset, providing an overview of overall performance.

SELECT 
    region,
    SUM(sales) AS total_sales,
    SUM(profit) AS total_profit
FROM 
    sales
GROUP BY 
    region;
Note: This query provides insights into sales and profit distribution across different regions, helping identify high-performing regions.

Top Selling Products

-- Identify the top-selling products and their respective sales figures.
SELECT
    product_name,
    sales
FROM
    sales
ORDER BY
    sales DESC
LIMIT 10;
Note: This query retrieves the top-selling products and their sales figures, aiding in product popularity assessment.

Monthly Sales Trend

-- Examine the monthly sales trend to understand seasonality.
SELECT 
    DATE_FORMAT(order_date, '%Y-%m') AS month,
    SUM(sales) AS total_sales
FROM 
    sales
GROUP BY 
    month
ORDER BY 
    month;
Note: This query presents the monthly sales trend, helping identify seasonal patterns and plan for demand fluctuations.

Customer Segment Analysis

-- Explore the distribution of sales among different customer segments.
SELECT 
    segment,
    SUM(sales) AS total_sales
FROM 
    sales
GROUP BY 
    segment;
Note: This query analyzes the distribution of sales across different customer segments, providing insights into customer behavior.

Ship Mode Impact Analysis

-- Investigate whether the choice of ship mode impacts profit margins.
SELECT 
    ship_mode,
    AVG(profit_margin) AS average_profit_margin
FROM (
    SELECT 
        ship_mode,
        (profit / sales) AS profit_margin
    FROM 
        sales
    WHERE 
        sales > 0  -- To avoid division by zero
) AS profit_margin_data
GROUP BY 
    ship_mode;
Note: This query assesses the impact of ship mode choice on profit margins, helping optimize shipping strategies.

Discount Analysis

-- Analyze how discounts influence sales and profit.
SELECT 
    discount,
    AVG(sales) AS average_sales,
    AVG(profit) AS average_profit
FROM 
    sales
GROUP BY 
    discount
ORDER BY 
    discount;
Note: This query examines the influence of discounts on average sales and profit, aiding in pricing strategy evaluation.

Market-wise Performance
-- Evaluate how sales performance varies across different markets.
SELECT 
    market,
    COUNT(*) AS number_of_transactions,
    SUM(sales) AS total_sales,
    AVG(sales) AS average_sales,
    SUM(profit) AS total_profit,
    AVG(profit) AS average_profit
FROM 
    sales
GROUP BY 
    market
ORDER BY 
    market;
Note: This query provides an overview of sales performance in different markets, supporting market-specific strategies.

Ship Mode Distribution
-- Explore how sales are distributed across different ship modes.
SELECT 
    ship_mode,
    COUNT(*) AS number_of_transactions,
    SUM(sales) AS total_sales,
    AVG(sales) AS average_sales
FROM 
    sales
GROUP BY 
    ship_mode
ORDER BY 
    ship_mode;
Note: This query shows the distribution of sales across various ship modes, assisting in optimizing shipping processes.

Top Customers

-- Identify the top customers based on their total sales.
SELECT 
    customer_name,
    SUM(sales) AS total_sales
FROM 
    sales
GROUP BY 
    customer_name
ORDER BY 
    total_sales DESC
LIMIT 10; 
Note: This query lists the top customers based on their total sales, assisting in customer relationship management.

Top Categories and Subcategories

-- Query to find the top categories based on total sales.
SELECT 
    category,
    SUM(sales) AS total_sales
FROM 
    sales
GROUP BY 
    category
ORDER BY 
    total_sales DESC
LIMIT 5;
Note: This query identifies the top product categories based on total sales, guiding inventory and marketing strategies.


-- Query to find the top subcategories based on total sales.
SELECT 
    sub_category,
    SUM(sales) AS total_sales
FROM 
    sales
GROUP BY 
    sub_category
ORDER BY 
    total_sales DESC
LIMIT 5;
Note: This query identifies the top product subcategories based on total sales, providing insights into product preferences.

Miscellaneous Queries

-- Query to count the total number of unique customers.
SELECT 
    COUNT(DISTINCT customer_name) AS total_customers
FROM 
    sales;
Note: This query counts the total number of unique customers, helping assess customer reach and engagement.


-- Query to find total sales and total profit across all categories and subcategories.
SELECT 
    SUM(sales) AS total_sales,
    SUM(profit) AS total_profit
FROM 
    sales;
Note: This query returns the overall total sales and total profit across all categories and subcategories, providing a snapshot of the business's financial health.


-- Query to calculate total shipping cost by product.
SELECT 
    product_id,
    product_name,
    SUM(shipping_cost) AS total_shipping_cost
FROM 
    sales
GROUP BY 
    product_id, product_name
ORDER BY 
    total_shipping_cost DESC;
Note: This query calculates the total shipping cost for each product, helping optimize shipping expenses.


-- Query to count orders for each order priority level.
SELECT 
    order_priority,
    COUNT(*) AS order_count
FROM 
    sales
GROUP BY 
    order_priority;
Note: This query counts the number of orders for each order priority level, aiding in order fulfillment strategies.


-- Query to count the number of unique products.
SELECT 
    COUNT(DISTINCT product_id) AS total_products
FROM 
    sales;
Note: This query counts the total number of unique products in the dataset, assisting in managing product variety.
```


### Results/Findings



- Insight:
The top-performing region is Africa, contributing the highest total sales of $11,565 and total profit of $1,516.69.
Top Products by Total Sales:

- Insight:
The product with the highest total sales is "Ikea Classic Bookcase, Metal," selling 987 units.
Distribution of Sales Among Customer Segments:

- Insight:
The consumer segment dominates with the highest total sales, accounting for 44,740 units.
Sales Across Different Markets:

- Insight:
The EU market stands out as the top performer with total sales amounting to $26,334.
Top 10 Best Customers:

- Insight:
Ken Lonsdale emerges as the top customer with the highest total sales of $2,377.
Top Sales by Categories:

- Insight:
The top-performing category is "Office Supplies," with total sales reaching $36,947.
Sales by Subcategories:

- Insight:
Among subcategories, "Storage" leads with the highest total sales of $13,128.
These insights provide a deeper understanding of the dataset, highlighting key performers and contributing factors in various aspects of Superstore sales.





### Recommendations

Regional Focus:

1. Given that Africa is the top-performing region, consider allocating additional resources and marketing efforts to further tap into this market's potential for increased sales and profitability.
Product Strategy:

2. The product "Ikea Classic Bookcase, Metal" stands out as a top seller. Consider promoting similar products or exploring strategies to enhance the visibility of this product to boost overall sales.
Customer Segment Targeting:

3. With the consumer segment contributing the highest total sales, tailor marketing campaigns and promotions to cater specifically to consumer preferences and needs.
Market Expansion:

4. While the EU market is currently the top performer, explore opportunities for market expansion in other regions. Analyze factors contributing to success in the EU and apply similar strategies in untapped markets.
Customer Relationship Management:

5. Recognize and appreciate top customers like Ken Lonsdale. Implement customer loyalty programs, personalized offers, or exclusive promotions to strengthen relationships with high-value customers.
Category Optimization:

6. Since "Office Supplies" is the top-performing category, optimize inventory management and marketing strategies within this category to maximize sales and profitability.
Subcategory Emphasis:

7. Given that "Storage" is the leading subcategory, consider emphasizing marketing efforts, promotions, or new product launches within this subcategory to capitalize on customer preferences.
These recommendations aim to guide strategic decision-making and actions based on the identified strengths and opportunities in the Superstore sales data. Implementing these suggestions can contribute to enhanced sales performance and overall business success.





