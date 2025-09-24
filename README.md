## Superstore Performance Customer and Product Analysis

## Objective

This dataset contains 11978 transactional records for a superstore, including sales, profits, customer information, and product categories. The primary goal is to leverage this data to identify key drivers of profitability, understand customer purchasing behaviour, optimise product inventory and pricing, and ultimately provide actionable insights to increase overall store performance and reduce losses from returned items.


## Analysis and Business Questions

The analysis is divided into 4 key areas and seeks to answer the following business questions:

**1. Profitability & Loss Analysis:**

    •	What are the key factors that correlate with a product or order generating a loss (negative profit)? 
    •	Is there a relationship between discount levels and profitability? At what discount level do products typically become unprofitable? 
    •	Which product categories, sub-categories, or specific products are the most and least profitable? 
    •	Are the loss-making products concentrated in certain categories that might be used to attract customers? 
**2. Customer Segmentation and Sales Analysis:**
    •	Who are our most valuable customers? Can we segment them by sales, profit, or region?
    •	What patterns distinguish high-value customers from others? (e.g., do they buy specific categories, respond to discounts, come from certain regions?)
    •	Is there a relationship between the quantity of items purchased, the discount offered, and the total sales value? 
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

-- **Data processing:**

-- Joining multiple Excel sheets into a combined sheet

    import pandas as pd
    file_path = ("C:\\Users\\hp\\OneDrive\\Projects\\FMCG\\dataset\\Superstore (2016-2020).xlsx")
    sheets = ['orders_2016', 'orders_2017', 'orders_2018', 'orders_2019', 'orders_2020']
    superstore_list = []
    
    for sheet in sheets:
      data = pd.read_excel(file_path, sheet_name=sheet, index_col=0)
      superstore_list.append(data)
      sc = pd.concat(superstore_list, ignore_index=True)
    
    print(sc.head())

-- Saving the combined sheets as a CSV file

    import pandas as pd
    sc.to_csv("C:\\Users\\hp\\OneDrive\\Projects\\PYTHON\\Superstore\\superstore.csv", index=False)

-- Verifying if the file was created correctly

    import os
    file_path = "C:\\Users\\hp\\OneDrive\\Projects\\PYTHON\\Superstore\\superstore.csv"
    if os.path.exists(file_path):
    print("File successfully created!")

-- Renaming columns

    import pandas as pd

        sc = pd.read_csv("C:\\Users\\hp\\OneDrive\\Projects\\PYTHON\\Superstore\\superstore.csv")

    sc.rename(columns={
        'Order ID': 'order_id', 'Order Date': 'order_date','Ship Date': 'ship_date',
        'Ship Mode': 'ship_mode', 'Customer ID': 'customer_id','Customer Name': 'customer_name',  
        'Segment': 'segment', 'Country/Region': 'country','City': 'city',
        'State': 'state', 'Postal Code': 'postal_code','Region': 'region',
        'Product ID': 'product_id', 'Category': 'category','Sub-Category': 'sub_category',
        'Product Name': 'product_name', 'Sales': 'sales','Quantity': 'quantity',
        'Discount': 'discount', 'Profit': 'profit','Returned': 'returned'
    }, inplace=True)

    print(sc.columns)
<img width="956" height="108" alt="image" src="https://github.com/user-attachments/assets/6b76d2ea-8ff7-401e-abc1-46c8b85a2ac1" />

-- Checking data types

    sc.info()
<img width="985" height="501" alt="image" src="https://github.com/user-attachments/assets/abb9550b-f436-4e1e-9995-a4ee67f09205" />

-- Converting data types

    import pandas as pd
    import numpy as np

    sc['order_date'] = pd.to_datetime(sc['order_date'])
    sc['ship_date'] = pd.to_datetime(sc['ship_date'])

    categorical_cols = ['ship_mode', 'segment', 'country', 'region', 'category', 'sub_category', 'returned']
    for col in categorical_cols:
        sc[col] = sc[col].astype('category')

    sc['postal_code'] = sc['postal_code'].astype('Int64').astype('str')  # Int64 handles NaN values
    sc['postal_code'] = sc['postal_code'].replace('<NA>', 'Unknown')

    id_cols = ['order_id', 'customer_id', 'product_id']
    for col in id_cols:
        sc[col] = sc[col].astype('str')

    print("\nData types after conversion:")
    print(sc.dtypes)
<img width="963" height="444" alt="image" src="https://github.com/user-attachments/assets/c8ac3367-60e9-44fb-b8b1-41c098932555" />


--** Data cleaning:**

-- Checking for missing values

# For numeric values, using appropriate imputation techniques(such as mean, median, or mode) or using '0'. For text-based values, using 'unknown'

    print(sc.isnull().sum())
<img width="949" height="393" alt="image" src="https://github.com/user-attachments/assets/6a9b27ff-8e28-47a4-8300-d9e035bd3d74" />

# The dataset contains one (1) missing value in the returned column.

-- Handling missing values

    sc['returned'] = sc['returned'].fillna('No')

    print(sc.isnull().sum())
<img width="964" height="389" alt="image" src="https://github.com/user-attachments/assets/d81873e1-f871-4904-af19-1a452f06f302" />

# The dataset is free of missing values

-- Checking for duplicates

    duplicate_rows = sc.duplicated()
    print(f"Number of duplicate rows: {duplicate_rows.sum()}")

    duplicates = sc[sc.duplicated()]
    print(duplicates)
<img width="982" height="310" alt="image" src="https://github.com/user-attachments/assets/3c8517be-ff33-42d1-a86e-29859b569cff" />

## The dataset has 1 duplicate row 

-- Removing duplicates

    sc = sc.drop_duplicates()

    print(f"Number of duplicate rows after removal: {sc.duplicated().sum()}")

    print(f"DataFrame shape after removing duplicates: {sc.shape}")
<img width="972" height="51" alt="image" src="https://github.com/user-attachments/assets/d14dc2e8-603e-473d-8acc-f0e4f44706ff" />

