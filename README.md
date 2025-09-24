## SUPERSTORE PERFORMANCE, CUSTOMER, AND PRODUCT ANALYSIS

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
**3. Product and Inventory Management:**

    •	Can we segment products into groups (e.g., "High-Profit Stars," "Low-Sale Negligibles," "High-Risk Return Items") to tailor marketing and inventory strategies? 
    •	How does sales and profit performance vary by category (Furniture, Office Supplies, Technology) and sub-category?
    •	What is the typical sales amount and profit margin based on category and region? 
**4. Returns Analysis:**

    •	Why are products being returned? What patterns distinguish returned orders from kept orders? (e.g., higher discounts, specific product categories, certain shipping methods, lower profit margins?). 
    •	Can we predict the probability of an order being returned based on factors like discount, product category, and profit? 

**Proposed Methods**

    1.	Descriptive Analytics: Summarising key metrics (total sales, average profit, overall profit margin, return rate, quantity sold by category). 
    2.	Correlation Analysis: Identifying relationships between variables (e.g., discount vs. profit, sales vs. quantity, profit vs. probability of return).
    3.	Predictive Modelling: Using historical data to predict future outcomes (e.g., Logistic Regression to predict if an order will be returned based on discount, category, and profit).
    4.	Clustering: Using algorithms like K-Means to segment customers into distinct groups based on their total sales, profit generated, and frequency of orders.

**Tools and Their Proposed Application**

    •	Excel: Initial data exploration.
        o	Use filters and pivot tables to quickly get a sense of sales distribution by region, category, and year.
        o	Calculate quick metrics like overall profit margin.
    •	SQL: To query the database and prepare specific datasets for deeper analysis.
    •	Python (Pandas, Scikit-learn, Seaborn, Matplotlib): The primary tool for deep analysis.
        o	Pandas: For robust data cleaning (checking for duplicates, missing values) and manipulation (creating new features like profit_margin).
        o	Seaborn/Matplotlib: For creating visualisations.
            	Boxplots: Profit by Category or Discount by Return Status.
            	Correlation heatmaps to find relationships between numerical variables.
            	Bar charts for Total Sales by Sub-Category.
        o	Scikit-learn:
            	Build a classification model to predict returns using features like discount, category, profit, and quantity.
            	Use K-Means clustering to segment customers based on their purchasing behaviour.
    •	Tableau: To build an interactive Superstore Performance Dashboard for management.
        o	Key Performance Indicators (KPIs): Total Sales, Total Profit, Profit Margin %, Overall Return Rate.
        o	Filters: For Year, Region, Category, and Sub-Category to drill down into data.
        o	A map showing sales or profit distribution by U.S. state.
        o	A scatter plot showing Sales vs. Profit colored by Category (to easily identify profitable and loss-making products).
        o	A bar chart showing the Top 10 Most Profitable Customers.
        o	A trend line showing Sales and Profit over time (2016-2020).

## DATA PROCESSING, CLEANING AND MANIPULATION

**-- Data processing:**

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
<img width="975" height="1049" alt="image" src="https://github.com/user-attachments/assets/6f779907-d466-4346-9403-76079a3b328a" />

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
<img width="975" height="173" alt="image" src="https://github.com/user-attachments/assets/fd4238f4-80cc-494d-aaa6-d8b1edfb9aeb" />

-- Checking data types

    sc.info()
<img width="975" height="792" alt="image" src="https://github.com/user-attachments/assets/cd50e11f-5caa-482b-8585-568825badbad" />

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
<img width="975" height="654" alt="image" src="https://github.com/user-attachments/assets/289d2a53-a067-4d27-a7bf-69c0d7b1676d" />

**-- Data cleaning:**

-- Checking for missing values

For numeric values, using appropriate imputation techniques(such as mean, median, or mode) or using '0'. For text-based values, using 'unknown'

    print(sc.isnull().sum())
<img width="975" height="623" alt="image" src="https://github.com/user-attachments/assets/0cb5a9ab-17f4-49df-a3b0-00f7bb5373bc" />

The dataset contains one (1) missing value in the returned column.

-- Handling missing values

    sc['returned'] = sc['returned'].fillna('No')

    print(sc.isnull().sum())
<img width="975" height="618" alt="image" src="https://github.com/user-attachments/assets/0755dbb9-f4be-49b6-87ad-e501f8910220" />


The dataset is free of missing values

-- Checking for duplicates

    duplicate_rows = sc.duplicated()
    print(f"Number of duplicate rows: {duplicate_rows.sum()}")

    duplicates = sc[sc.duplicated()]
    print(duplicates)
<img width="975" height="562" alt="image" src="https://github.com/user-attachments/assets/09148a39-412c-400f-9069-c476518a79d1" />

The dataset has 1 duplicate row 

-- Removing duplicates

    sc = sc.drop_duplicates()

    print(f"Number of duplicate rows after removal: {sc.duplicated().sum()}")

    print(f"DataFrame shape after removing duplicates: {sc.shape}")
<img width="975" height="57" alt="image" src="https://github.com/user-attachments/assets/745a7c01-1f22-47bc-8cf9-a4240fdef198" />

-- Resetting the table's index column

This will ensure the index is sequential without any gaps from the removed row

    sc = sc.reset_index(drop=True)

**-- Data manipulation**

-- Creating new features:
    1. Profit margin
    2. Unit price
    3. Total cost
    4. Discount amount
    5. Net sales
    6. Net profit margin
    7. Order processing time, in days

    import pandas as pd
    import numpy as np

Calculating profit margin (profit as a percentage of sales)

    sc['profit_margin'] = (sc['profit'] / sc['sales']) * 100

Handle cases where sales are zero to avoid division by zero

    sc['profit_margin'] = np.where(sc['sales'] == 0, 0, sc['profit_margin'])

Calculating unit price (price per item)

    sc['unit_price'] = sc['sales'] / sc['quantity']

Calculating total cost (sales minus profit)

    sc['total_cost'] = sc['sales'] - sc['profit']

Calculating discount amount (sales * discount percentage)

    sc['discount_amount'] = sc['sales'] * sc['discount']

Calculating net sales (sales after discount)

    sc['net_sales'] = sc['sales'] - sc['discount_amount']

Calculating net profit margin (profit as a percentage of net sales)

    sc['net_profit_margin'] = (sc['profit'] / sc['net_sales']) * 100
    sc['net_profit_margin'] = np.where(sc['net_sales'] == 0, 0, sc['net_profit_margin'])

Creating a flag for profitable transactions

    sc['is_profitable'] = sc['profit'] > 0

Creating a flag for heavily discounted items (e.g., discount > 30%)

    sc['high_discount'] = sc['discount'] > 0.3

Calculating order processing time (ship date - order date)

    sc['processing_days'] = (sc['ship_date'] - sc['order_date']).dt.days

Displaying the new columns

    print(sc[['sales', 'profit', 'quantity', 'discount', 
          'profit_margin', 'unit_price', 'total_cost', 
          'discount_amount', 'net_sales', 'net_profit_margin',
          'is_profitable', 'high_discount']].head(10))
<img width="975" height="1291" alt="image" src="https://github.com/user-attachments/assets/02c3b9bf-5dd4-4216-a591-0407f52499f3" />

-- Summary statistics for the new features

    print("\nSummary statistics:")
    print(sc[['profit_margin', 'unit_price', 'total_cost', 
          'discount_amount', 'net_profit_margin', 'processing_days']].describe())
<img width="975" height="597" alt="image" src="https://github.com/user-attachments/assets/31cc5d6f-e55d-4d88-88b8-00dc6d68cebe" />

-- Creating a clean and enhanced CSV file

    sc.to_csv("C:\\Users\\hp\\OneDrive\\Projects\\PYTHON\\Superstore\\ss_clean.csv", index=False)

-- Saving the CSV file

    import os
    file_path = "C:\\Users\\hp\\OneDrive\\Projects\\PYTHON\\Superstore\\ss_clean.csv"
    if os.path.exists(file_path):
        print("File successfully created!")

## DATA ANALYSIS

**-- Profitability & Loss Analysis**

1. What factors correlate with a product or an order generating a loss (negative profit)?

    import pandas as pd
    import numpy as np
    import seaborn as sns
    import matplotlib.pyplot as plt

-- Creating a binary target variable for loss-making products

    ss['is_loss'] = (ss['profit'] < 0).astype(int)  

-- Selecting only numeric columns for correlation analysis

    numeric_columns = ss.select_dtypes(include=[np.number]).columns 
    
-- Calculating correlations with the loss indicator using only numeric columns

    correlation_with_loss = ss[numeric_columns].corr()['is_loss'].sort_values(ascending=False) 

    print("Correlation with Loss Making:")
    print(correlation_with_loss)
<img width="975" height="393" alt="image" src="https://github.com/user-attachments/assets/e6a67dae-ccbc-492a-8438-a91d4012f392" />
<img width="975" height="695" alt="image" src="https://github.com/user-attachments/assets/d4d42373-3e5a-419f-ab2f-151ba2aa95fa" />

**Interpretation:**

• There is **a strong positive correlation between discount and is_loss** (r = 0.753921). This indicates that a higher discount is strongly associated with an increased likelihood of a transaction resulting in a loss. In other words, larger discounts directly reduce profit margins.

• There is **a weak positive correlation between the dollar amount of the discount and is_loss** (r = 0.169223). This suggests that a higher absolute dollar discount also correlates with losses. Larger discount amounts (in dollars) increase the risk of incurring losses.

• The **correlation between total cost and is_loss is very weak** (r = 0.118892). The cost of an item has little to no direct linear relationship with whether a transaction results in a loss. Other factors, such as sales price and discount, play a more significant role.

• Unit price, sales, processing days, and quantity show **no significant linear relationship with the occurrence of a loss**. All of these variables have correlations very close to zero, ranging from -0.002504 to 0.025537.

• Profit margin (r = -0.765811) has **a strong negative correlation**. As the profit margin decreases, the likelihood of a loss increases. A low profit margin directly leads to a loss, while higher profit margins strongly protect against losses.

• Net profit margin (r = -0.591829) exhibits **a moderately strong negative correlation**. Similar to profit margin, a lower net profit margin is associated with a greater chance of incurring a loss.

• There is **a weak negative relationship between profit and is_loss** (r = -0.234539). As profit decreases, the likelihood of a loss tends to increase.


2. Is there a relationship between discount levels and profitability?






3. 











