# DSA_PROJECT_KMS_CASE_STUDY 
---
This repository contains details about the KMS case study project.

## PROJECT TITLE
---
Analysis of Kultra Mega Stores Inventory

## PROJECT OVERVIEW 
---
This project explores real-world sales and operations data from kultra Mega Stores (KMS) — a retail company headquartered in Lagos, Nigeria, specializing in office supplies and furniture. As a Business Intelligence Analyst supporting the Abuja division, KMS order data (2009–2012) was analysed to uncover key business insights and provide actionable recommendations.

The case study featured typical business scenarios, from identifying top customers and sales trends to evaluating cost efficiency in logistics. Queries were written and executed in SQL Server, covering both sales performance and operational decisions.

## DATA SOURCE
---
The dataset was provided by the DSA Incubatorhub Program and contains simulated order data (2009–2012) from Kultra Mega Stores (KMS). It includes sales, customers, shipping methods, product categories, regions, and order priorities. The data was shared in Excel format and analyzed using SQL Server for training purposes.

## TOOLS USED
- SQL Server Management Studio (SSMS)
- SQL (Aggregation, Grouping, Subqueries, Joins)
- GitHub (project documentation)

---
## DATA CLEANING AND PREPARATION 
---
- The dataset was first reviewed and cleaned in Excel, where duplicate records were checked and none found, the file was saved to CSV UTF-8 format.

- A new database was created in SQL Server Management Studio (SSMS) using SQL queries, after which the cleaned Excel file was imported.

- During the import process:
Appropriate data types were assigned to columns as needed.
NULL values were permitted for three specific columns to preserve data integrity and flexibility during querying.

## EXPLORATORY DATA ANALYSIS 
---
The Kultra Mega Stores Inventory Dataset was explored (Structured Query Language) to answer and draw insights from some questions such as:

Case Scenario I: Sales & Regional Performance
- Which product category had the highest sales?
- What are the top 3 and bottom 3 regions by total sales?
- What were the total sales of appliances in Ontario?
- Which shipping method incurred the highest cost?
-  What strategies can improve revenue from the bottom 10 customers?

Case Scenario II: Customer Profitability & Operational Insights
- Who are the most valuable customers and what do they typically purchase?
- Which small business customer had the highest sales?
- Which corporate customer placed the most orders (2009–2012)?
- Which consumer customer was the most profitable?
- Which customer(s) returned items, and what segments do they belong to?
- Was shipping cost aligned with order priority? (Delivery Truck vs Express Air)

## SQL QUERIES AND OUTPUTS
---
IMORTED TABLE
![image](https://github.com/user-attachments/assets/f93d27b0-c326-4c17-b1f0-82e5e1477955)

1. QUESTION 1
   ![image](https://github.com/user-attachments/assets/6a4edbf4-1df5-4706-afd4-1274e548a079)



