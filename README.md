## Superstore Performance Customer and Product Analysis

## Objective

This dataset contains detailed transactional data for a superstore, including sales, profits, customer information, and product categories. The primary goal is to leverage this data to identify key drivers of profitability, understand customer purchasing behavior, optimize product inventory and pricing, and ultimately provide actionable insights to increase overall store performance and reduce losses from returned items.
Analysis and Business Questions
Mirroring the structure of the provided guide, the analysis is divided into 4 key areas:
1. Profitability & Loss Analysis:
•	Question: What are the key factors that correlate with a product or order generating a loss (negative profit)? Python
•	Question: Is there a relationship between discount levels and profitability? At what discount level do products typically become unprofitable? SQL Python
•	Question: Which product categories, sub-categories, or specific products are the most and least profitable? SQL
•	Question: Are the loss-making products concentrated in certain categories that might be used to attract customers?  Python
2. Customer Segmentation and Sales Analysis:
•	Question: Who are our most valuable customers? Can we segment them by sales, profit, or region?✓ SQL
•	Question: What patterns distinguish high-value customers from others? (e.g., do they buy specific categories, respond to discounts, come from certain regions?) ✓ SQL
•	Question: Is there a relationship between the quantity of items purchased, the discount offered, and the total sales value? Python
3. Product and Inventory Management:
•	Question: Can we segment products into groups (e.g., "High-Profit Stars," "Low-Sale Negligibles," "High-Risk Return Items") to tailor marketing and inventory strategies? SQL ✓
•	Question: How does sales and profit performance vary by category (Furniture, Office Supplies, Technology) and sub-category?✓ EXCEL
•	Question: What is the typical sales amount and profit margin based on category and region? EXCEL ✓
4. Returns Analysis:
•	Question: Why are products being returned? What patterns distinguish returned orders from kept orders? (e.g., higher discounts, specific product categories, certain shipping methods, lower profit margins?). SQL
•	Question: Can we predict the probability of an order being returned based on factors like discount, product category, and profit? Python
Proposed Methods
1.	Descriptive Analytics: Summarizing key metrics (total sales, average profit, overall profit margin, return rate, quantity sold by category). ✓
2.	Correlation Analysis: Identifying relationships between variables (e.g., Discount vs. Profit, Sales vs. Quantity, Profit vs. Probability of Return).
3.	Predictive Modeling: Using historical data to predict future outcomes (e.g., Logistic Regression to predict if an order will be Returned based on Discount, Category, and Profit).
4.	Clustering: Using algorithms like K-Means to segment customers into distinct groups based on their total sales, profit generated, and frequency of orders.
Tools and Their Proposed Application
•	Excel: Initial data exploration.
o	Use filters and pivot tables to quickly get a sense of sales distribution by region, category, and year.
o	Calculate quick metrics like overall profit margin.
•	SQL: To query the database and prepare specific datasets for deeper analysis.
o	Example Query 1 (Studying Returns):
sql
SELECT
    Category,
    Sub_Category,
    AVG(Discount) AS Avg_Discount,
    AVG(Profit) AS Avg_Profit,
    COUNT(*) AS Return_Count
FROM
    superstore_table
WHERE
    Returned = 'Yes'
GROUP BY
    Category, Sub_Category
ORDER BY
    Return_Count DESC;
o	Example Query 2 (Customer Segmentation):
sql
SELECT
    Customer_ID,
    Customer_Name,
    SUM(Sales) AS Total_Sales,
    SUM(Profit) AS Total_Profit,
    COUNT(DISTINCT Order_ID) AS Order_Count
FROM
    superstore_table
GROUP BY
    Customer_ID, Customer_Name
ORDER BY
    Total_Profit DESC;
•	Python (Pandas, Scikit-learn, Seaborn, Matplotlib): The primary tool for deep analysis.
o	Pandas: For robust data cleaning (checking for duplicates, missing values) and manipulation (creating new features like profit_margin).✓
o	Seaborn/Matplotlib: For creating visualizations.
	Boxplots: Profit by Category or Discount by Return Status.✓
	Correlation heatmaps to find relationships between numerical variables.✓
	Bar charts for Total Sales by Sub-Category.✓
o	Scikit-learn:
	Build a classification model to predict Returned using features like Discount, Category, Profit, Quantity.
	Use K-Means clustering to segment customers based on their purchasing behavior.
•	Tableau: To build an interactive Superstore Performance Dashboard for management.
o	Key Performance Indicators (KPIs): Total Sales, Total Profit, Profit Margin %, Overall Return Rate.
o	Filters: For Year, Region, Category, and Sub-Category to drill down into data.
o	A map showing sales or profit distribution by U.S. state.
o	A scatter plot showing Sales vs. Profit colored by Category (to easily identify profitable and loss-making products).
o	A bar chart showing Top 10 Most Profitable Customers.
o	A trend line showing Sales and Profit over time (2016-2020).








## DATA PROCESSING, CLEANING AND MANIPULATION

-- Data processing:

    import pandas as pd
    file_path = ("C:\\Users\\hp\\OneDrive\\Projects\\FMCG\\dataset\\Superstore (2016-2020).xlsx")
    sheets = ['orders_2016', 'orders_2017', 'orders_2018', 'orders_2019', 'orders_2020']
    superstore_list = []
    
    for sheet in sheets:
      data = pd.read_excel(file_path, sheet_name=sheet, index_col=0)
      superstore_list.append(data)
      sc = pd.concat(superstore_list, ignore_index=True)
    
    print(sc.head())
