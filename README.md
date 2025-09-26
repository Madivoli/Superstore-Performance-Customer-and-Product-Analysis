## SUPERSTORE'S PERFORMANCE, CUSTOMER, AND PRODUCT ANALYSIS

## Objective

This dataset contains 11,978 transactional records for a superstore, including sales, profits, customer information, and product categories. The primary goal is to leverage this data to identify key drivers of profitability, understand customer purchasing behaviour, optimise product inventory and pricing, and ultimately provide actionable insights to increase overall store performance and reduce losses from returned items.


## Analysis and Business Questions

The analysis is divided into 4 key areas and seeks to answer the following business questions:

**1. Profitability & Loss Analysis:**

    • What are the key factors that correlate with a product or order generating a loss (negative profit)? 
    • Is there a relationship between discount levels and profitability? At what discount level do products typically become unprofitable? 
    • Which product categories, sub-categories, or specific products are the most and least profitable? 
    • Are the loss-making products concentrated in certain categories that might be used to attract customers? 
    
**2. Customer Segmentation and Sales Analysis:**

    • Who are our most valuable customers? Can we segment them by sales, profit, or region?
    • What patterns distinguish high-value customers from others? 
    • Is there a relationship between the quantity of items purchased, the discount offered, and the total sales value? 
    
**3. Product and Inventory Management:**

    • Can we segment products into groups to tailor marketing and inventory strategies? 
    • How do sales and profit performance vary by category and sub-category?
    • What is the typical sales amount and profit margin based on category and region? 
    
**4. Returns Analysis:**

    • Why are products being returned? What patterns distinguish returned orders from kept orders? 
    • Can we predict the probability of an order being returned based on factors like discount, product category, and profit? 

**Proposed Methods**

    1. Descriptive Analytics: Summarising key metrics (total sales, average profit, overall profit margin, return rate, quantity sold by category). 
    2. Correlation Analysis: Identifying relationships between variables (e.g., discount vs. profit, sales vs. quantity, profit vs. probability of return).
    3. Predictive Modelling: Using historical data to predict future outcomes (e.g., Logistic Regression to predict if an order will be returned based on discount, category, or profit).
    4. Clustering: Using algorithms like K-Means to segment customers into distinct groups based on their total sales, profit generated, and frequency of orders.

**Tools and Their Proposed Application**

    • Excel: Initial data exploration and visualisation.
    • MySQL: To query the database and prepare specific datasets for deeper analysis.
    • Python (Pandas, Scikit-learn, Seaborn, Matplotlib): The primary tool for data cleaning, processing, manipulation and deep analysis.
    • Tableau: To build an interactive Superstore Performance Dashboard for management.
       

## DATA PROCESSING, CLEANING AND MANIPULATION

**DATA PROCESSING**

**-- Joining multiple Excel sheets into a combined sheet:**

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


**-- Saving the combined sheets as a CSV file:**

    import pandas as pd
    sc.to_csv("C:\\Users\\hp\\OneDrive\\Projects\\PYTHON\\Superstore\\superstore.csv", index=False)

**-- Verifying if the file was created correctly**

    import os
    file_path = "C:\\Users\\hp\\OneDrive\\Projects\\PYTHON\\Superstore\\superstore.csv"
    if os.path.exists(file_path):
    print("File successfully created!")

**-- Renaming columns**

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


**-- Checking data types**

    sc.info()

<img width="975" height="792" alt="image" src="https://github.com/user-attachments/assets/cd50e11f-5caa-482b-8585-568825badbad" />


**-- Converting data types**

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


**DATA CLEANING**

**-- Checking for missing values:**

For numeric values, using appropriate imputation techniques(such as mean, median, or mode) or using '0'. For text-based values, using 'unknown'

    print(sc.isnull().sum())

<img width="975" height="623" alt="image" src="https://github.com/user-attachments/assets/0cb5a9ab-17f4-49df-a3b0-00f7bb5373bc" />


*The dataset contains one (1) missing value in the returned column.*

**-- Handling missing values:**

    sc['returned'] = sc['returned'].fillna('No')

    print(sc.isnull().sum())

<img width="975" height="618" alt="image" src="https://github.com/user-attachments/assets/0755dbb9-f4be-49b6-87ad-e501f8910220" />


*The dataset is free of missing values.*

**-- Checking for duplicates:**

    duplicate_rows = sc.duplicated()
    print(f"Number of duplicate rows: {duplicate_rows.sum()}")

    duplicates = sc[sc.duplicated()]
    print(duplicates)

<img width="975" height="562" alt="image" src="https://github.com/user-attachments/assets/09148a39-412c-400f-9069-c476518a79d1" />

*The dataset has 1 duplicate row* 

**-- Removing duplicates:**

    sc = sc.drop_duplicates()

    print(f"Number of duplicate rows after removal: {sc.duplicated().sum()}")

    print(f"DataFrame shape after removing duplicates: {sc.shape}")

<img width="975" height="57" alt="image" src="https://github.com/user-attachments/assets/745a7c01-1f22-47bc-8cf9-a4240fdef198" />


**-- Resetting the table's index column:**

*This will ensure the index is sequential without any gaps from the removed row*

    sc = sc.reset_index(drop=True)

**DATA MANIPULATION**

**-- Creating new features:**
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


**-- Summary statistics for the new features:**

    print("\nSummary statistics:")
    print(sc[['profit_margin', 'unit_price', 'total_cost', 
          'discount_amount', 'net_profit_margin', 'processing_days']].describe())

<img width="975" height="597" alt="image" src="https://github.com/user-attachments/assets/31cc5d6f-e55d-4d88-88b8-00dc6d68cebe" />


**-- Creating a clean and enhanced CSV file:**

    sc.to_csv("C:\\Users\\hp\\OneDrive\\Projects\\PYTHON\\Superstore\\ss_clean.csv", index=False)

**-- Saving the CSV file:**

    import os
    file_path = "C:\\Users\\hp\\OneDrive\\Projects\\PYTHON\\Superstore\\ss_clean.csv"
    if os.path.exists(file_path):
        print("File successfully created!")

## DATA ANALYSIS AND VISUALIZATION

**PROFITABILITY & LOSS ANALYSIS**

*1. What factors correlate with a product or an order generating a loss (negative profit)?*
   
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


**Key Insight:** *There is **a strong positive correlation between discount and is_loss** (r = 0.753921). This suggests that a higher discount is significantly associated with a higher likelihood of a transaction resulting in a loss. In other words, larger discounts directly reduce profit margins.*

*2. Is there a relationship between discount levels and profitability? At what discount level do products typically become unprofitable?*

-- Developing a predictive model

        import pandas as pd
        import numpy as np
        import matplotlib.pyplot as plt
        import seaborn as sns
        from sklearn.linear_model import LinearRegression
        from sklearn.metrics import r2_score
        import scipy.stats as stats

-- Establishing variables

        X = ss[['discount']]  # Independent variable
        y = ss['profit_margin']  # Dependent variable

-- Removing any rows with missing values

        data = pd.concat([X, y], axis=1).dropna()
        X_clean = data[['discount']]
        y_clean = data['profit_margin']

-- Fitting SLR model

        model = LinearRegression()
        model.fit(X_clean, y_clean)

-- Making predictions

        y_pred = model.predict(X_clean)

-- Calculating R-squared

        r2 = r2_score(y_clean, y_pred)

-- Establishing regression coefficients

        slope = model.coef_[0]
        intercept = model.intercept_

        print("=== SIMPLE LINEAR REGRESSION RESULTS: Discount vs Profit Margin ===")
        print(f"Regression Equation: Profit Margin = {intercept:.2f} + ({slope:.2f} × Discount)")
        print(f"R-squared: {r2:.4f} ({(r2*100):.1f}% of variance explained)")
        print(f"Slope: {slope:.2f} (For each 1% increase in discount, profit margin changes by {slope:.2f}%)")
        print("\n")

<img width="975" height="156" alt="image" src="https://github.com/user-attachments/assets/ee1cb8c9-f70a-4bb5-8c57-ee2739e70126" />


**Key Insight:** *There exists a **strong negative linear relationship between discount and profit margin**. This finding corroborates the previous correlation results (r = -0.76). The intercept of 42.63% indicates that **when the discount is 0%**, the **predicted profit margin is 42.63%**. The slope of -195.73 suggests that **for each 1 unit increase in discount, the profit margin is expected to decrease by 195.73 units**. The R-squared value of 0.7475 (or 74.8%) signifies that **74.8% of the variability in profit margin can be attributed to the discount variable alone**. Therefore, **discount serves as a very strong predictor of profit margin** within this model.*

-- Calculating break-even discount (where profit margin = 0).

        break_even_discount = -intercept / slope if slope != 0 else np.nan
        print(f"Break-even discount: {break_even_discount:.2%}")
        print(f"Interpretation: Products typically become unprofitable when discounts exceed {break_even_discount:.1%}")

<img width="975" height="103" alt="image" src="https://github.com/user-attachments/assets/c90d5229-e6f4-4d1d-95e6-549793b04cfd" />


**Key Insight:** *The products will become unprofitable when the discount exceeds 21.8%.*

*3. Which product categories, sub-categories, or specific products are the most and least profitable?*

-- Creating a staging table

    CREATE TABLE ss_staging	
    LIKE ss_cleaned;
    
-- Inserting data

    INSERT ss_staging
    SELECT *
    FROM ss_cleaned;

-- Profitability by category (ranked from least to most profitable:

    SELECT 
        category,
        COUNT(DISTINCT order_id) AS total_orders,
        SUM(quantity) AS total_quantity_sold,
        ROUND(SUM(sales), 2) AS total_sales,
        ROUND(SUM(profit), 2) AS total_profit,
        ROUND(SUM(profit) / SUM(sales) * 100, 2) AS profit_margin_percent,
        ROUND(AVG(profit_margin), 2) AS avg_profit_margin_percent,
    
    -- Profitability ranking
        RANK() OVER (ORDER BY SUM(profit) DESC) AS profit_rank,
        CASE 
            WHEN SUM(profit) > 0 THEN 'Profitable'
        ELSE 'Loss-Making'
        END AS profitability_status
        FROM ss_staging
    GROUP BY category
    ORDER BY total_profit DESC;

<img width="977" height="452" alt="image" src="https://github.com/user-attachments/assets/1c2e932a-a531-4afd-94f9-451d1eec7784" />


-- Profitability by sub-categories

    SELECT 
        category,
        sub_category,
        COUNT(DISTINCT order_id) AS total_orders,
        SUM(quantity) AS total_quantity_sold,
        ROUND(SUM(sales), 2) AS total_sales,
        ROUND(SUM(profit), 2) AS total_profit,
        ROUND(SUM(profit) / SUM(sales) * 100, 2) AS profit_margin_percent,
        ROUND(AVG(profit_margin), 2) AS avg_profit_margin_percent,
    
        RANK() OVER (PARTITION BY category ORDER BY SUM(profit) DESC) AS category_profit_rank,
        RANK() OVER (ORDER BY SUM(profit) DESC) AS overall_profit_rank,
        CASE 
            WHEN SUM(profit) > 0 THEN 'Profitable'
            ELSE 'Loss-Making'
        END AS profitability_status
    FROM ss_staging
    GROUP BY category, sub_category
    ORDER BY total_profit DESC;

<img width="977" height="605" alt="image" src="https://github.com/user-attachments/assets/18037b69-d11c-47a3-9f17-14204b4aeee7" />


-- Top 10 most profitable specific products

    SELECT 
        product_id,
        product_name,
        category,
        sub_category,
        COUNT(DISTINCT order_id) AS total_orders,
        SUM(quantity) AS total_quantity_sold,
        ROUND(SUM(sales), 2) AS total_sales,
        ROUND(SUM(profit), 2) AS total_profit,
        ROUND(SUM(profit) / SUM(sales) * 100, 2) AS profit_margin_percent,
        ROUND(AVG(unit_price), 2) AS avg_unit_price,
        ROUND(AVG(discount) * 100, 2) AS avg_discount_percent
    FROM ss_staging
    GROUP BY product_id, product_name, category, sub_category
    HAVING total_sales > 0  
    ORDER BY total_profit DESC
    LIMIT 10;

<img width="977" height="642" alt="image" src="https://github.com/user-attachments/assets/c5462ddf-bc35-4bd4-bb2a-7cac521b4484" />


-- Top 10 least profitable products 

    SELECT 
        product_id,
        product_name,
        category,
        sub_category,
        COUNT(DISTINCT order_id) AS total_orders,
        SUM(quantity) AS total_quantity_sold,
        ROUND(SUM(sales), 2) AS total_sales,
        ROUND(SUM(profit), 2) AS total_profit,
        ROUND(SUM(profit) / SUM(sales) * 100, 2) AS profit_margin_percent,
        ROUND(AVG(unit_price), 2) AS avg_unit_price,
        ROUND(AVG(discount) * 100, 2) AS avg_discount_percent
    FROM ss_staging
    GROUP BY product_id, product_name, category, sub_category
    HAVING total_sales > 0  
    ORDER BY total_profit ASC  
    LIMIT 10;

<img width="977" height="555" alt="image" src="https://github.com/user-attachments/assets/6603a95c-3b42-4d4c-bcfe-055633d4027d" />


**4. Are the loss-making products concentrated in certain categories that might be used to attract customers?**

-- Analysis of loss-making products concentration

    SELECT 
        category,
    -- Product counts
        COUNT(DISTINCT product_id) AS total_products,
        SUM(CASE WHEN total_profit < 0 THEN 1 ELSE 0 END) AS loss_making_products,
        ROUND(SUM(CASE WHEN total_profit < 0 THEN 1 ELSE 0 END) * 100.0 / COUNT(DISTINCT product_id), 2) AS loss_product_percentage,
    
    -- Sales volume
        SUM(total_quantity) AS total_quantity_sold,
        SUM(CASE WHEN total_profit < 0 THEN total_quantity ELSE 0 END) AS loss_maker_quantity,
        ROUND(SUM(CASE WHEN total_profit < 0 THEN total_quantity ELSE 0 END) * 100.0 / SUM(total_quantity), 2) AS                                     loss_maker_quantity_percentage,
    
    -- Financial metrics
        ROUND(SUM(total_profit), 2) AS net_profit,
        ROUND(SUM(CASE WHEN total_profit < 0 THEN total_profit ELSE 0 END), 2) AS total_loss_amount,
    
    -- Loss leader assessment
        CASE 
            WHEN SUM(CASE WHEN total_profit < 0 THEN total_quantity ELSE 0 END) > AVG(total_quantity) * 2 THEN 'Potential Loss Leader                     Category'
            ELSE 'Regular Category'
            END AS loss_leader_assessment
    FROM (
        SELECT 
            category,
            product_id,
            SUM(quantity) AS total_quantity,
            SUM(sales) AS total_sales,
            SUM(profit) AS total_profit
        FROM ss_staging
        GROUP BY category, product_id
            ) product_summary
        GROUP BY category
        ORDER BY loss_maker_quantity_percentage DESC, total_loss_amount DESC;


<img width="1350" height="117" alt="image" src="https://github.com/user-attachments/assets/d830cd07-e535-4d61-a3be-6dc94a351923" />


**Key Insight:** *A significant 32.51% of all loss-making products are concentrated in a single category: Furniture. This suggests a highly uneven distribution of loss-making products across different categories.*



## CUSTOMER SEGMENTATION AND SALES ANALYSIS

*1. Who are our most valuable customers? Can we segment them by sales, profit, or region?* 

-- Most valuable customers analysis

## Top customers by sales and profits

    SELECT 
        customer_id,
        customer_name,
        region,
        ROUND(SUM(sales), 2) AS total_sales,
        ROUND(SUM(profit), 2) AS total_profit,
        COUNT(DISTINCT order_id) AS total_orders,
        ROUND(AVG(profit_margin), 2) AS avg_profit_margin
    FROM ss_staging
    GROUP BY customer_id, customer_name, region
    ORDER BY total_profit DESC
    LIMIT 3;


<img width="992" height="529" alt="image" src="https://github.com/user-attachments/assets/93c83421-4313-422c-abd9-35633324af33" />


**Key Insight:** *Customer ID SC-20095 is **the most valuable customer**, as they generate the highest sales and profit.*


## Customer segmentation by profit and tier

    SELECT 
        customer_name,
        region,
        total_sales,
        total_profit,
        CASE 
            WHEN total_profit > 10000 THEN 'Platinum'
            WHEN total_profit BETWEEN 5000 AND 10000 THEN 'Gold'
            WHEN total_profit BETWEEN 1000 AND 5000 THEN 'Silver'
            ELSE 'Bronze'
        END AS customer_tier,
        RANK() OVER (ORDER BY total_profit DESC) AS profit_rank
    FROM (
        SELECT 
            customer_name,
            region,
            ROUND(SUM(sales), 2) AS total_sales,
            ROUND(SUM(profit), 2) AS total_profit
        FROM ss_staging
        GROUP BY customer_name, region
    ) customer_summary
    ORDER BY total_profit DESC;


<img width="1022" height="677" alt="image" src="https://github.com/user-attachments/assets/293bde8a-a99d-496f-80da-63224b1c10e2" />


**Key Insight:** *The Gold Tier is the most profitable customer segment. Customers such as Sanjit Chand and Tamara Chand generate significantly more profit than customers in other tiers. Sanjit Chand, in particular, is a highly valuable customer, contributing over $9,000 in profit.*

-- Regional customer value analysis

    SELECT 
        region,
        COUNT(DISTINCT customer_id) AS total_customers,
        ROUND(SUM(sales), 2) AS regional_sales,
        ROUND(SUM(profit), 2) AS regional_profit,
        ROUND(AVG(profit), 2) AS avg_profit_per_customer,
        SUM(CASE WHEN profit > 0 THEN 1 ELSE 0 END) AS profitable_customers,
        ROUND(SUM(CASE WHEN profit > 0 THEN 1 ELSE 0 END) * 100.0 / COUNT(DISTINCT customer_id), 2) AS profitability_rate
    FROM (
        SELECT 
            region,
            customer_id,
            SUM(sales) AS sales,
            SUM(profit) AS profit
        FROM ss_staging
        GROUP BY region, customer_id
    ) customer_metrics
    GROUP BY region
    ORDER BY regional_profit DESC;


<img width="947" height="677" alt="image" src="https://github.com/user-attachments/assets/a5a4fd3e-9bf1-4dab-aeed-398c88b8861c" />


**Key Insight:** *The West region has the highest number of profitable customers, totalling 615, and boasts an impressive profitability rate of 90.31%. This indicates that more than 90% of customers in the West are profitable, making it the most valuable region for the business.*


## Customer segmentation 

 -- Step 1: Creating a temporary table for customer metrics and segmentation
 
        CREATE TEMPORARY TABLE customer_metrics AS
        SELECT
            customer_id,
            customer_name,
            region,
            ROUND(SUM(sales), 2) AS total_sales,
            ROUND(SUM(profit), 2) AS total_profit,
            COUNT(DISTINCT order_id) AS order_count,
            ROUND(SUM(sales) / COUNT(DISTINCT order_id), 2) AS avg_order_value,
            ROUND(SUM(discount_amount), 2) AS total_discount_amount,
            ROUND(SUM(discount_amount) / SUM(sales) * 100, 2) AS discount_rate_pct,

-- Category preferences
  
            ROUND(SUM(CASE WHEN category = 'Furniture' THEN sales ELSE 0 END), 2) AS furniture_sales,
            ROUND(SUM(CASE WHEN category = 'Office Supplies' THEN sales ELSE 0 END), 2) AS office_supplies_sales,
            ROUND(SUM(CASE WHEN category = 'Technology' THEN sales ELSE 0 END), 2) AS technology_sales,

-- Return behaviour

            SUM(CASE WHEN returned = 'Yes' THEN 1 ELSE 0 END) AS return_count
        FROM
            ss_staging
        GROUP BY
            customer_id, customer_name, region;


-- Step 2: Calculating percentile values for segmentation

        SET @profit_80 = (SELECT MAX(total_profit) FROM (
            SELECT total_profit FROM customer_metrics ORDER BY total_profit DESC LIMIT 80
        ) t);
        SET @profit_60 = (SELECT MAX(total_profit) FROM (
            SELECT total_profit FROM customer_metrics ORDER BY total_profit DESC LIMIT 60
        ) t);
        SET @profit_40 = (SELECT MAX(total_profit) FROM (
            SELECT total_profit FROM customer_metrics ORDER BY total_profit DESC LIMIT 40
        ) t);
        SET @sales_80 = (SELECT MAX(total_sales) FROM (
            SELECT total_sales FROM customer_metrics ORDER BY total_sales DESC LIMIT 80
        ) t);
        SET @sales_60 = (SELECT MAX(total_sales) FROM (
            SELECT total_sales FROM customer_metrics ORDER BY total_sales DESC LIMIT 60
        ) t);

-- Step 3: Creating a temporary table with segmentation

        CREATE TEMPORARY TABLE customer_segmented AS
        SELECT
            customer_id,
            customer_name,
            region,
            total_sales,
            total_profit,
            order_count,
            avg_order_value,
            discount_rate_pct,
    
-- Value segmentation

        CASE
            WHEN total_profit > @profit_80 AND total_sales > @sales_80 THEN 'Platinum'
            WHEN total_profit > @profit_60 AND total_sales > @sales_60 THEN 'Gold'
            WHEN total_profit > @profit_40 THEN 'Silver'
            ELSE 'Bronze'
        END AS value_segment,

-- Discount responsiveness

        CASE
            WHEN discount_rate_pct > 15 THEN 'Discount-Driven'
            WHEN discount_rate_pct BETWEEN 5 AND 15 THEN 'Moderate-Discount'
            ELSE 'Full-Price'
        END AS discount_segment,
    
-- Category preferences

            furniture_sales,
            office_supplies_sales,
            technology_sales,
            return_count
        FROM
        customer_metrics;

<img width="1500" height="764" alt="image" src="https://github.com/user-attachments/assets/02806d85-c029-4d74-956d-574ab350582e" />


-- Using the segmented data to answer the following three questions:

**-- 1. Who are our most valuable 20 customers?**


<img width="1502" height="677" alt="image" src="https://github.com/user-attachments/assets/41d703f3-d668-4f3b-b67d-ee7c1c104e85" />


**Key Insight:** *Greg Tran is the most frequent purchaser, with a total of 7 orders. He is followed by Adrian Barton, who has placed 6 orders, and Maria Etezadi, who has made 5 orders. A small group of customers — Tran, Barton, Etezadi, and Gary Hwang — accounts for a significant portion of the total orders. In contrast, many customers have only placed 1 or 2 orders. This pattern indicates that a few customers are driving the majority of the transaction volume. This creates a significant business risk if any customer churns.*


**-- 2. What patterns distinguish high-value customers?**

    SELECT
        value_segment,
        COUNT(*) AS customer_count,
        ROUND(AVG(total_sales), 2) AS avg_sales,
        ROUND(AVG(total_profit), 2) AS avg_profit,
        ROUND(AVG(order_count), 2) AS avg_orders,
        ROUND(AVG(avg_order_value), 2) AS avg_order_value,
        ROUND(AVG(discount_rate_pct), 2) AS avg_discount_rate,
        ROUND(AVG(furniture_sales), 2) AS avg_furniture_sales,
        ROUND(AVG(office_supplies_sales), 2) AS avg_office_supplies_sales,
        ROUND(AVG(technology_sales), 2) AS avg_technology_sales,
        ROUND(AVG(return_count), 2) AS avg_returns
    FROM
        customer_segmented
    GROUP BY
        value_segment
    ORDER BY
        CASE value_segment
            WHEN 'Platinum' THEN 1
            WHEN 'Gold' THEN 2
            WHEN 'Silver' THEN 3
            ELSE 4
        END;


*2. What patterns distinguish high-value customers from others? (e.g., do they buy specific categories, respond to discounts, come from certain regions?)*

-- RFM Analysis (Recency, Frequency, Monetary)

        WITH customer_metrics AS (
          SELECT 
            customer_id,
            region,
            COALESCE(DATEDIFF('2020-09-09', MAX(order_date)), 365) as Recency,
            COUNT(DISTINCT order_id) as Frequency,
            ROUND(COALESCE(SUM(sales), 0), 2) as Monetary,
            ROUND(COALESCE(SUM(profit), 0), 2) as Total_Profit,
            AVG(profit) as Avg_Profit_Per_Order
          FROM ss_staging
          GROUP BY customer_id, region
            ),
            rfm_scores AS (
        SELECT *,
        CASE 
              WHEN Recency <= 30 THEN 5    -- Last 30 days
              WHEN Recency <= 90 THEN 4    -- Last 3 months
              WHEN Recency <= 180 THEN 3   -- Last 6 months
              WHEN Recency <= 365 THEN 2   -- Last year
              ELSE 1                       -- Over 1 year
            END as R_Score,
        CASE 
              WHEN Frequency >= 15 THEN 5
              WHEN Frequency >= 10 THEN 4
              WHEN Frequency >= 5 THEN 3
              WHEN Frequency >= 2 THEN 2
              ELSE 1
            END as F_Score,
        CASE 
              WHEN Monetary >= 5000 THEN 5
              WHEN Monetary >= 2500 THEN 4
              WHEN Monetary >= 1000 THEN 3
              WHEN Monetary >= 500 THEN 2
              ELSE 1
            END as M_Score
          FROM customer_metrics
        )
        SELECT 
          customer_id,
          region,
          Recency,
          Frequency,
          Monetary,
          Total_Profit,
          R_Score,
          F_Score,
          M_Score,
          (R_Score + F_Score + M_Score) as RFM_Score,
      CASE 
            WHEN R_Score = 5 AND F_Score >= 4 AND M_Score >= 4 THEN 'Champions'
            WHEN R_Score >= 4 AND F_Score >= 4 AND M_Score >= 3 THEN 'Loyal Customers'
            WHEN R_Score = 5 AND F_Score <= 2 THEN 'New Customers'
            WHEN R_Score >= 3 AND F_Score >= 3 AND M_Score >= 3 THEN 'Potential Loyalists'
            WHEN R_Score = 2 AND M_Score >= 3 THEN 'At Risk'
            WHEN R_Score = 1 THEN 'Lost Customers'
            WHEN Total_Profit < 0 THEN 'Unprofitable'
            ELSE 'Need Attention'
          END as Segment
        FROM rfm_scores
        ORDER BY Monetary DESC, Recency ASC;


<img width="1500" height="1309" alt="image" src="https://github.com/user-attachments/assets/78180b42-1711-4bc9-a0ce-923a0506994a" />


**Key Insight:** *All customers have a recency score of 2, indicating that no one has made a purchase recently, which is a significant concern. The last order was placed 365 days ago. The frequency score ranges from 1 to 4, with a higher score indicating more recent orders. The monetary score ranges from 1 to 5, with a higher score indicating greater spending. All customers are categorised as "At Risk" due to their low R Score.*

**-- What patterns distinguish high-value customers?**

-- Comparing high-value customer segments vs other customers using RFM segments

    WITH customer_analysis AS (
      SELECT 
        customer_id,
    -- RFM Metrics
        COALESCE(DATEDIFF('2020-09-09', MAX(order_date)), 365) as recency,
        COUNT(DISTINCT order_id) as frequency,
        COALESCE(SUM(sales), 0) as monetary,
        COALESCE(SUM(profit), 0) as total_profit,
    
    -- Value Segment
        CASE 
          WHEN COALESCE(SUM(sales), 0) >= 10000 THEN 'Platinum'
          WHEN COALESCE(SUM(sales), 0) >= 5000 THEN 'Gold'
          WHEN COALESCE(SUM(sales), 0) >= 1000 THEN 'Silver'
          ELSE 'Bronze'
        END as value_segment,
    
    -- Additional metrics
        AVG(sales) as avg_order_value,
        AVG(discount) * 100 as discount_rate_pct,
        SUM(CASE WHEN category = 'Furniture' THEN sales ELSE 0 END) as furniture_sales,
        SUM(CASE WHEN category = 'Office Supplies' THEN sales ELSE 0 END) as office_supplies_sales,
        SUM(CASE WHEN category = 'Technology' THEN sales ELSE 0 END) as technology_sales,
        SUM(CASE WHEN returned = 'Yes' THEN 1 ELSE 0 END) as return_count
      FROM ss_staging
      GROUP BY customer_id
    )

    SELECT
        value_segment,
        COUNT(*) AS customer_count,
        ROUND(AVG(monetary), 2) AS avg_sales,
        ROUND(AVG(total_profit), 2) AS avg_profit,
        ROUND(AVG(frequency), 2) AS avg_orders,
        ROUND(AVG(monetary / NULLIF(frequency, 0)), 2) AS avg_order_value,
        ROUND(AVG(discount_rate_pct), 2) AS avg_discount_rate,
        ROUND(AVG(furniture_sales) / AVG(monetary) * 100, 2) AS furniture_pct,
        ROUND(AVG(office_supplies_sales) / AVG(monetary) * 100, 2) AS office_supplies_pct,
        ROUND(AVG(technology_sales) / AVG(monetary) * 100, 2) AS technology_pct,
        ROUND(AVG(return_count), 2) AS avg_returns
    FROM
        customer_analysis
    GROUP BY
        value_segment
    ORDER BY
        CASE value_segment
            WHEN 'Platinum' THEN 1
            WHEN 'Gold' THEN 2
            WHEN 'Silver' THEN 3
            ELSE 4
        END;


<img width="977" height="644" alt="image" src="https://github.com/user-attachments/assets/2d1f1c8a-43b5-4c2c-9f7c-1962f24e4b64" />

**Key Insight:** *Customers in the **Platinum segment are the most valuable**, generating more than half of the total revenue. However, this creates significant business risk if any Platinum customers churn. The analysis **reveals a classic "whale curve",** where a small percentage of customers (in this case, the Platinum segment) drives the majority of business revenue.*
