## Superstore's Performance, Customer, and Product Analysis


![hanson-lu-sq5P00L7lXc-unsplash](https://github.com/user-attachments/assets/74fc54bb-9619-4bf2-98d0-559ff9517e1b)


## Project Overview

This dataset contains 11,978 transactional records for a superstore, including sales, profits, customer information, and product categories. The primary goal is to leverage this data to identify key drivers of profitability, understand customer purchasing behaviour, optimise product inventory and pricing, and ultimately provide actionable insights to increase overall store performance and reduce losses from returned items.

---
## Objectives and Business Questions

To analyse 4 key areas and answer the following business questions:

**1. Profitability & Loss Analysis:**

    • Which product categories, sub-categories, or specific products are the most and least profitable? 
	• What are the key factors that correlate with a product or order generating a loss (negative profit)? 
    • Are the loss-making products concentrated in certain categories that might be used to attract customers? 
	• Is there a relationship between discount levels and profitability? At what discount level do products typically become unprofitable? 
    
**2. Customer Segmentation and Sales Analysis:**

    • Who are our most valuable customers? Can we segment them by sales, profit, or region?
    • What patterns distinguish high-value customers from others? 
	• How do sales fluctuate over time, and are there seasonal peaks?
    • Is there a relationship between the quantity of items purchased, the discount offered, and the total sales value? 
    
**3. Product and Inventory Management:**

    • Can we segment products into groups to tailor marketing and inventory strategies? 
    • How do sales and profit performance vary by category and sub-category?
    • What is the typical sales amount and profit margin based on category and region? 
    
**4. Returns Analysis:**

    • Why are products being returned? What patterns distinguish returned orders from kept orders? 
    • Can we predict the probability of an order being returned based on factors like discount, product category, and profit? 
---
## Methods

    1. Descriptive Analytics: Summarising key metrics (total sales, average profit, overall profit margin, return rate, quantity sold by category). 
    2. Correlation Analysis: Identifying relationships between variables (e.g., discount vs. profit, sales vs. quantity, profit vs. probability of return).
    3. Predictive Modelling: Using historical data to predict future outcomes (e.g., Logistic Regression to predict if an order will be returned based on discount, category, or profit).
    4. Clustering: Using algorithms like K-Means to segment customers into distinct groups based on their total sales, profit generated, and frequency of orders.
---
## Tools 

	• Excel: Initial data exploration and visualisation.
	• MySQL: To query the database and prepare specific datasets for deeper analysis.
	• Python (Pandas, Scikit-learn, Seaborn, Matplotlib): The primary tool for data cleaning, processing, manipulation and deep analysis.
	• Tableau: To build an interactive Superstore Performance Dashboard for management.
       
---
## Procedures

**Data Processing, Cleaning and Manipulation**

1. **Join and save file**: Joined and saved multiple Excel files (2016 - 2020 Orders) into a combined CSV file.
2. **Rename columns**: Renamed columns in the CSV file.
3.  **Fix data types**: Converted order_date and ship_date to datetime objects for accurate analysis.
4. **Handle missing data**: Checked and removed null values in the returned column.
5. **Remove duplicates**: Checked and removed duplicate rows.
6. **Reset the table's index column**:To ensure that the index is sequential without any gaps from the removed row.
7. **Add new calculated columns:** Calculated profit margin, unit price, total cost, discount amount, net sales, net profit margin, and order processing time, in days columns for better comparisons.    

   
 ---  
 **Exploratory Data Analysis**
 
Calculated KPIs:
	<img width="1350" height="489" alt="image" src="https://github.com/user-attachments/assets/e4bd2da2-7455-4b59-b8c3-1f71de242828" />

- There are a total of **793 customers**, with an average of **7.42 orders per customer (OPC)**.
- The **Average Order Value (AOV) is $1,865**, driven by a significant number of units sold per transaction, averaging 30 units.
- The average **discount percentage is 15.58%**, indicating that the business relies heavily on discounts to stimulate sales.
- The business maintains a **profit margin of 12%**, which is commendable given the high discount rate.
- The difference between the **average selling price** and **average cost per unit is $7.85**. With a discount of 15.58% on the average price, this amounts to $9.75.
- The business is effectively "giving away" more in discounts ($9.75) than it retains as gross profit per unit ($7.85). 

**Recommendations:**

Based on the KPI analysis, the business should consider the following:

1. **Protecting and Nurturing the Customer Base**: The business should initiate initiatives to improve customer retention, such as loyalty programs, exclusive offers, and exceptional service. These strategies will yield a much higher return than focusing solely on acquiring new customers with similar lifetime values.

2. **Analyse the Discount Strategy**: The business should use discounts strategically to attract new customers or clear out slow-selling inventory, rather than applying them as a blanket policy or for merely retaining loyal customers. Additionally, it may be beneficial to reduce the discount percentage, for example to 14%, to evaluate its impact on profits or sales volume.

3. **Increase Customer Lifetime Value (LTV)**: Since customers tend to make frequent purchases, the business can encourage them to add one more item to each order through upselling or cross-selling strategies. It might also consider introducing a new, higher-margin product to the lineup.

---
## Profitability & Loss Analysis

1.**Which product categories, sub-categories, or specific products are the most and least profitable?**

-- Profitability by category (ranked from the most to the least profitable):

<img width="1202" height="452" alt="image" src="https://github.com/user-attachments/assets/44ad94de-2029-4622-87c2-5f0cdba69044" />

  

-- Profitability by sub-categories

<img width="1202" height="622" alt="image" src="https://github.com/user-attachments/assets/29b2bde1-ea22-4e20-86b7-2795aecb6ba4" />



-- Top 10 most profitable specific products

<img width="1202" height="632" alt="image" src="https://github.com/user-attachments/assets/eb83ca8b-46e9-411f-901e-ac8b7a93a2f9" />


-- Top 10 least profitable products 

<img width="1202" height="555" alt="image" src="https://github.com/user-attachments/assets/676a6e6f-d7a3-436f-ac37-11101e17134a" />


---

2.**What factors correlate with a product or an order generating a loss (negative profit)?**
        
<img width="975" height="695" alt="image" src="https://github.com/user-attachments/assets/d4d42373-3e5a-419f-ab2f-151ba2aa95fa" />


**Key Insight:** 

- There is **a strong positive correlation between discount and is_loss** (r = 0.753921). This suggests that a higher discount is significantly associated with a higher likelihood of a transaction resulting in a loss. In other words, larger discounts directly reduce profit margins.
- There is **a weak positive correlation between the dollar amount of the discount and is_loss** (r = 0.169223). This suggests that a higher absolute dollar discount also correlates with losses. Larger discount amounts (in dollars) increase the risk of incurring losses.
- The **correlation between total cost and is_loss is very weak** (r = 0.118892). The cost of an item has little to no direct linear relationship with whether a transaction results in a loss. Other factors, such as sales price and discount, play a more significant role.
- Unit price, sales, processing days, and quantity show **no significant linear relationship with the occurrence of a loss**. All of these variables have correlations very close to zero, ranging from -0.002504 to 0.025537.
- Profit margin (r = -0.765811) has **a strong negative correlation**. As the profit margin decreases, the likelihood of a loss increases. A low profit margin directly leads to a loss, while higher profit margins strongly protect against losses.
- Net profit margin (r = -0.591829) exhibits **a moderately strong negative correlation**. Similar to profit margin, a lower net profit margin is associated with a greater chance of incurring a loss.
- There is **a weak negative relationship between profit and is_loss** (r = -0.234539). As profit decreases, the likelihood of a loss tends to increase.

---
3.**Is there a relationship between discount levels and profit margin? At what discount level do products typically become unprofitable?**

<img width="975" height="156" alt="image" src="https://github.com/user-attachments/assets/ee1cb8c9-f70a-4bb5-8c57-ee2739e70126" />


**Key Insight:** 

- There exists a **strong negative linear relationship between discount and profit margin**. This finding corroborates the previous correlation results (r = -0.76).
- The intercept of 42.63% indicates that **when the discount is 0%**, the **predicted profit margin is 42.63%**.
- The slope of -195.73 suggests that **for each 1 unit increase in discount, the profit margin is expected to decrease by 195.73 units**.
- The R-squared value of 0.7475 (or 74.8%) signifies that **74.8% of the variability in profit margin can be attributed to the discount variable alone**.
- Therefore, **discount serves as a very strong predictor of profit margin** within this model.


<img width="975" height="103" alt="image" src="https://github.com/user-attachments/assets/c90d5229-e6f4-4d1d-95e6-549793b04cfd" />


**Key Insight:** 

The **products will become unprofitable when the discount exceeds 21.8%**.

---

4.**Are the loss-making products concentrated in certain categories that might be used to attract customers?**

-- Analysis of loss-making products concentration

<img width="1350" height="117" alt="image" src="https://github.com/user-attachments/assets/d830cd07-e535-4d61-a3be-6dc94a351923" />


**Key Insight:** 

- A significant 32.51% of all loss-making products are concentrated in a single category: Furniture.
- This suggests a highly uneven distribution of loss-making products across different categories.

----

## Customer Segmentation and Sales Analysis

1. **Who are our most valuable customers? Can we segment them by sales, profit, or region?**

-- Most valuable customers by sales and profits


<img width="1517" height="529" alt="image" src="https://github.com/user-attachments/assets/2370b654-0425-4bac-9c0c-9513c958b4e7" />


**Key Insight:** 
- Customer ID SC-20095 is **the most valuable customer**, as they generate the highest sales and profit.


-- Customer segmentation by profit and tier

  <img width="1022" height="677" alt="image" src="https://github.com/user-attachments/assets/293bde8a-a99d-496f-80da-63224b1c10e2" />


**Key Insight:** 
- The Gold Tier is the most profitable customer segment.
- Customers such as Sanjit Chand and Tamara Chand generate significantly more profit than customers in other tiers.
- Sanjit Chand, in particular, is a highly valuable customer, contributing over $9,000 in profit.

-- Regional customer value analysis

<img width="947" height="677" alt="image" src="https://github.com/user-attachments/assets/a5a4fd3e-9bf1-4dab-aeed-398c88b8861c" />


**Key Insight:** 
- The West region has the highest number of profitable customers, totalling 615, and boasts an impressive profitability rate of 90.31%.
- This indicates that more than 90% of customers in the West are profitable, making it the most valuable region for the business.


-- Customer segmentation 

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

**-- Regional analysis of high-value customers:**

    SELECT
        region,
        value_segment,
        COUNT(*) AS customer_count,
        ROUND(SUM(total_sales), 2) AS region_sales,
        ROUND(SUM(total_profit), 2) AS region_profit
    FROM
        customer_segmented
    GROUP BY
        region, value_segment
    ORDER BY
        region,
        CASE value_segment
            WHEN 'Platinum' THEN 1
            WHEN 'Gold' THEN 2
            WHEN 'Silver' THEN 3
            ELSE 4
        END;

<img width="1502" height="617" alt="image" src="https://github.com/user-attachments/assets/25432eb2-ccef-452a-8235-a794a8d714d4" />


*3. Is there a relationship between the quantity of items purchased, the discount offered, and the total sales value?*

    import pandas as pd
    import numpy as np
    import seaborn as sns
    import matplotlib.pyplot as plt
    from scipy import stats

-- Selecting the variables for analysis

    variables = ['quantity', 'discount', 'sales', 'profit']
    analysis_df = ss[variables].copy()

-- Calculating correlations

    correlation_matrix = analysis_df.corr()
    print("Correlation Matrix:")
    print(correlation_matrix.round(3))

-- Calculating correlations amongst the variables

    print("\nKey Relationships:")
    print(f"Quantity vs Sales: {analysis_df['quantity'].corr(analysis_df['sales']):.3f}")
    print(f"Discount vs Sales: {analysis_df['discount'].corr(analysis_df['sales']):.3f}")
    print(f"Quantity vs Discount: {analysis_df['quantity'].corr(analysis_df['discount']):.3f}")
    print(f"Discount vs Profit: {analysis_df['discount'].corr(analysis_df['profit']):.3f}")

<img width="1500" height="336" alt="image" src="https://github.com/user-attachments/assets/1efe945d-b49b-449f-b21e-6ad0e7a2ab26" />

-- Correlation heatmap

    plt.figure(figsize=(10, 8))
    sns.heatmap(correlation_matrix, 
            annot=True, 
            cmap='RdBu_r', 
            center=0,
            square=True,
            fmt='.2f',
            linewidths=0.5)
    plt.title('Correlation Heatmap: Quantity, Discount, Sales & Profit')
    plt.tight_layout()
    plt.show()

<img width="1341" height="1184" alt="image" src="https://github.com/user-attachments/assets/985bdfa9-5ebb-4ca6-8426-b222e3baf592" />
