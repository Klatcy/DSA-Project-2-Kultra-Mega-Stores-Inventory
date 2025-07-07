---

# 📊 DSA_Project_June_2025

## Project Title: **Business Intelligence Analysis for Kultra Mega Stores (KMS)**

---

### 📌 Table of Contents

- [Introduction](#introduction)  
- [Project Overview](#project-overview)  
- [Data Source](#data-source)  
- [About the Dataset](#about-the-dataset)  
- [Tools Used](#tools-used)  
- [Data Cleaning and Preparation](#data-cleaning-and-preparation)  
- [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)  
- [Data Analysis](#data-analysis)  
- [Data Visualisation](#data-visualisation)  
- [Conclusion](#conclusion)  
- [Recommendations](#recommendations)  
- [Author](#author)

---

### 📘 Introduction

Kultra Mega Stores (KMS) is a retail and wholesale business specializing in office supplies and furniture, headquartered in Lagos, Nigeria. The Abuja division of KMS requested a data analysis to uncover performance insights and drive smarter decision-making using historical order data from 2009 to 2012.

---

### 📊 Project Overview

The objective of this project is to analyze customer behavior, sales trends, shipping efficiency, and profitability using SQL queries. Insights gained from this analysis will guide business strategy, improve logistics, and enhance customer targeting.

---

### 🔗 Data Source

The dataset was provided in Excel format and imported into SQL Server. It contains historical order data from 2009 to 2012. Two tables were used:
- `KMS New`: Contains sales, profit, order, and customer details.
- `Order_Status`: Contains status of each order (e.g., Returned).

---

### 📦 About the Dataset

Key columns in the dataset:
- `Order_ID`
- `Order_Date`, `Ship_Date`
- `Product_Name`, `Product_Category`
- `Customer_Name`, `Customer_Segment`
- `Sales`, `Profit`, `Shipping_Cost`
- `Ship_Mode`, `Order_Priority`
- `Region`
- `Status` (from `Order_Status` table)

---

### 🛠 Tools Used

- **SQL Server Management Studio (SSMS)**
- **Microsoft Excel (for preprocessing)**


---

### 🧹 Data Cleaning and Preparation

- Removed nulls and duplicates  
- Standardized date formats  
- Ensured relationships between `Order_ID` in both tables  
- Validated data types for numerical fields (Sales, Profit, Shipping_Cost)

---

### 📈 Exploratory Data Analysis (EDA)

- Distribution of sales across regions  
- Frequency of product categories  
- Shipping cost patterns by mode  
- Order timelines and delivery lags  

---

### 🧮 Data Analysis

#### **1. Product Category with Highest Sales**
```sql
SELECT TOP 1 Product_Category, SUM(Sales) AS Total_Sales
FROM [dbo].[KMS New]
GROUP BY Product_Category
ORDER BY Total_Sales DESC


---

2. Top 3 and Bottom 3 Regions by Sales

-- Top 3
SELECT TOP 3 Region, SUM(Sales) AS Total_Sales
FROM [dbo].[KMS New]
GROUP BY Region
ORDER BY Total_Sales DESC

-- Bottom 3
SELECT TOP 3 Region, SUM(Sales) AS Total_Sales
FROM [dbo].[KMS New]
GROUP BY Region
ORDER BY Total_Sales ASC


---

3. Total Sales of Appliances in Ontario

SELECT SUM(Sales) AS Total_Appliance_Sales
FROM [dbo].[KMS New]
WHERE Region = 'Ontario'
GROUP BY Region


---

4. Bottom 10 Customers by Sales

SELECT TOP 10 Customer_Name, SUM(Sales) AS Total_Sales
FROM [dbo].[KMS New]
GROUP BY Customer_Name
ORDER BY Total_Sales ASC




---

5. Shipping Method with Highest Cost

SELECT TOP 1 Ship_Mode, SUM(Shipping_Cost) AS Total_Cost
FROM [dbo].[KMS New]
GROUP BY Ship_Mode
ORDER BY Total_Cost DESC


---

6. Most Valuable Customers and Products Purchased

SELECT TOP 10 Customer_Name, Product_Name, SUM(Sales) AS Total_Sales
FROM [dbo].[KMS New]
GROUP BY Customer_Name, Product_Name
ORDER BY Total_Sales DESC


---

7. Top Small Business Customer by Sales

SELECT TOP 1 Customer_Name, Customer_Segment, SUM(Sales) AS Total_Sales
FROM [dbo].[KMS New]
WHERE Customer_Segment = 'Small Business'
GROUP BY Customer_Name, Customer_Segment
ORDER BY Total_Sales DESC


---

8. Corporate Customer with Most Orders (2009–2012)

SELECT TOP 1 Customer_Name, COUNT(Order_ID) AS Total_Orders
FROM [dbo].[KMS New]
WHERE Customer_Segment = 'Corporate'
AND YEAR(Order_Date) BETWEEN 2009 AND 2012
GROUP BY Customer_Name
ORDER BY Total_Orders DESC


---

9. Most Profitable Consumer Customer

SELECT TOP 1 Customer_Name, SUM(Profit) AS Total_Profit
FROM [dbo].[KMS New]
WHERE Customer_Segment = 'Consumer'
GROUP BY Customer_Name
ORDER BY Total_Profit DESC


---

10. Customers Who Returned Items + Their Segment

SELECT DISTINCT o.Customer_Name, o.Customer_Segment
FROM [dbo].[KMS New] o
JOIN [dbo].[Order_Status] r
ON o.Order_ID = r.Order_ID
WHERE r.Status = 'Returned'


---

11. Evaluation of Shipping Cost vs Order Priority

SELECT Order_Priority, Ship_Mode,
       COUNT(Order_ID) AS Total_Orders,
       ROUND(SUM(Sales - Profit), 2) AS Estimated_Shipping_Cost,
       AVG(DATEDIFF(DAY, Order_Date, Ship_Date)) AS Avg_Ship_Days
FROM [dbo].[KMS New]
GROUP BY Order_Priority, Ship_Mode
ORDER BY Order_Priority, Ship_Mode DESC

Summary of Findings:

Delivery Truck is the cheapest but slowest shipping mode

Express Air is the fastest but most expensive

Regular Air provides moderate speed and cost

Mismatch detected between shipping mode and order priority:

Some low-priority orders used Express Air, inflating costs

Some critical orders used Delivery Truck, causing delays


This misalignment leads to:

Wasted shipping expenses

Poor customer satisfaction

Reduced operational efficiency



Recommendations:

1. Automate Shipping Mode Selection Based on Order Priority

Critical → Express Air

Medium → Regular Air

Low → Delivery Truck



2. Restrict Use of Express Air to only high-priority orders.


3. Audit Logistics Operations regularly to ensure shipping aligns with order importance.


4. Implement Smart Logistics Tools with rule-based workflows.


5. Reallocate Shipping Savings to:

Speed up critical deliveries

Offer discounts to loyal customers

Improve last-mile delivery operations



---
Advice:

Analyze customer behavior, including purchase history, shipping mode, product type, and discounts.

Offer personalized promotions tailored to each customer's preferences.

Collect feedback to understand their needs and preferences.

Bundle complementary products to add extra value.
---

✅ Conclusion

Top-performing product categories and regions were successfully identified.

Key customer segments—including the most profitable, most frequent, and lowest-performing—were revealed.

Sales performance by business segment (Consumer, Small Business, Corporate) was analyzed.

Returned orders were traced to specific customers and segments for quality control follow-up.

Most critically, the company's shipping strategy was found to be misaligned with order priorities, leading to:

Overuse of expensive shipping for low-priority orders

Slow delivery on high-priority or critical items

Increased operational costs and customer dissatisfaction




---

💡 #### Recommendations

1. Customer Strategy

Offer personalized deals and promotions to bottom-tier customers

Bundle relevant products to increase average transaction value

Use feedback surveys to understand pain points



2. Sales Optimization

Focus marketing efforts on top-performing regions

Analyze and replicate high-sales product strategies across other regions



3. Logistics and Shipping

Automate shipping mode selection based on order priority

Restrict Express Air for critical/high-priority orders only

Conduct logistics audits to avoid misallocation

Reinvest saved costs into delivery speed for urgent orders



4. Operational Efficiency

Create business dashboards for real-time insights

Train logistics and sales teams to align with BI recommendations





---

👩‍💼 Author

OLANREWAJU BUKOLA
DSA Data Analysis Project | July 2025

---



