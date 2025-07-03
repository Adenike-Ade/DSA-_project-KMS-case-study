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
```SQL QUERIES
CREATE DATABASE DSA_CAPSTONE_PROJECT
SELECT * FROM [dbo].[KSA SQL CASE STUDY]

--QUESTION 1 product category with highest sales--

SELECT PRODUCT_CATEGORY, SUM(SALES) AS totalsales
from [dbo].[KSA SQL CASE STUDY]
group by Product_CATEGORY
Order by totalsales desc

-- QUESTION 2 top 3 and bottom 3 regions by sales

--top 3
select top 3 region,
SUM(sales) AS TOTALSALES
FROM[dbo].[KSA SQL CASE STUDY]
group by Region
ORDER BY TOTALSALES DESC;

select top 3 region,
SUM(sales) AS TOTALSALES
FROM[dbo].[KSA SQL CASE STUDY]
group by Region
ORDER BY TOTALSALES asc;


--QUESTION 3 (total sales of appliances in ontario)--

SELECT region, sum(sales) as [total sals]
from [dbo].[KSA SQL CASE STUDY]
where Product_sub_Category = 'appliances' 
and region = 'ontario'
group by region

---QUESTION 4 (Advice to the managment of KMS to increase revenue of the bottom 10).

	 select top 10 
	customer_name, count(order_id) as totalorders, sum(sales) as totalsales,
	(select max(customer_segment) from [dbo].[KSA SQL CASE STUDY]) as SegDetails,
	(select max(region) from [dbo].[KSA SQL CASE STUDY]) as RegDetails
	from [dbo].[KSA SQL CASE STUDY]
	group by customer_name
	order by totalsales asc;

--FROM THE ABOVE QUERY BOTTOM 10, THE ADVICES BELOW ARE WRITTEN IN QUERIES TO GET IT IN TABULAR FORM.

SELECT 
'Advice for Bottom 10 Customers' AS Advice,
'1. Obtain Customer information' AS Strategic_Point,
'get information of the customers, reach out to them and know the cause for their poor purchase' AS Description
UNION ALL
SELECT
'Advice for Bottom 10 Customers' AS Advice,
'2. Customer Feedback' AS Strategic_Point,
'Collect feedback from customers to understand their needs and preferences' AS Description
UNION ALL
SELECT
'Advice for Bottom 10 Customers' AS advice,
'3. Product Improvement' AS Strategic_Point,
'provide and improve products based on preferences and needs from customers feedback [if need be]' AS description
UNION ALL
SELECT
'Advice for Bottom 10 Customers' AS Advice,
'4. Improve Company-customer Relationship (customer care)' AS Strategic_Point,
'Provide a platform to build loyalty by providing or improving [an already existing] customer care service' AS Description
UNION ALL
SELECT
'Advice for Bottom 10 Customers' AS Advice,
'5.Company Feedbacks to Customers' AS Strategic_Point,
'Get details of customers and follow-up products bought to boost customer confidence' As Description

----QUESTION 5 (WHICH SHIPPING METHOD INCURRED THE MOST SHIPPING COST)

SELECT TOP 3 Ship_mode,
sum(Shipping_Cost) as [totalshipping cost]
from [dbo].[KSA SQL CASE STUDY]
group by ship_mode
order by [totalshipping Cost] desc;

--QUESTION 6 (who is the most valuable customer, what product or service do they purchase)

 select top 3 
	customer_name, Product_Sub_Category, product_name,
	sum(sales) as totalsales
	from [dbo].[KSA SQL CASE STUDY]
	 group by customer_name, Product_Sub_Category, product_name
	 order by totalsales desc;

---QUESTION 7 (small business owners with the highest sales)

select top 3 
	customer_name, customer_segment,
	sum(sales) as totalsales
	from [dbo].[KSA SQL CASE STUDY]
	where Customer_Segment = 'small business'
	 group by customer_name, customer_segment
	 order by totalsales desc;

--QUESTION 8 (CORPORATE CUSTOMER WITH MOST ORDERS 2009 -2012)

SELECT TOP 10
	CUSTOMER_NAME, CUSTOMER_SEGMENT, ORDER_DATE, 
	COUNT(DISTINCT ORDER_ID) AS TOTALORDERS
	FROM [dbo].[KSA SQL CASE STUDY]
	WHERE CUSTOMER_SEGMENT = 'CORPORATE'
	AND ORDER_DATE BETWEEN '2009-01-01' AND '2021-12-31'
	GROUP BY CUSTOMER_NAME, CUSTOMER_SEGMENT, ORDER_DATE
	ORDER BY TOTALORDERS DESC;

	---QUESTION 9 CONSUMER CUSTOMER WITH THE HIGHEST PROFIT)

SELECT TOP 1
	CUSTOMER_NAME, CUSTOMER_SEGMENT, 
	SUM(PROFIT) AS TOTALPROFITS
	FROM [dbo].[KSA SQL CASE STUDY]
	WHERE CUSTOMER_SEGMENT = 'CONSUMER'
	GROUP BY CUSTOMER_NAME, CUSTOMER_SEGMENT
	ORDER BY TOTALPROFITS DESC;

	----QUESTION 10 (customer who returned items and the segment they belonged to).

select  DISTINCT [dbo].[KSA SQL CASE STUDY].Order_ID, 	
			 Customer_Name,	
			 Customer_Segment	
			Product_Category, 	
			[dbo].[Order_Status_302963135].[Status]
from [dbo].[KSA SQL CASE STUDY]
inner join [dbo].[Order_Status_302963135]
on [dbo].[Order_Status_302963135].Order_ID =[dbo].[KSA SQL CASE STUDY].Order_ID

SELECT * FROM[dbo].[Order_Status_302963135]


QUESTION 11 

SELECT order_priority, ship_mode,
		count(Order_ID) as TotalOrder,
		round(sum(sales - profit),2) as [Estimated shipping Cost],
		avg(datediff(day,[order_date],[ship_date])) as avgshipdays
from [dbo].[KSA SQL CASE STUDY]
group by  order_priority, ship_mode
order by   order_priority, ship_mode desc;

SELECT order_priority, ship_mode,
		count(distinct Order_ID) as TotalOrder,
		SUM (Shipping_cost) as [Estimated shipping Cost],
		avg(datediff(day,[order_date],[ship_date])) as avgshipdays
from [dbo].[KSA SQL CASE STUDY]
group by  order_priority, ship_mode
order by   order_priority, ship_mode desc;
```
IMORTED TABLE
   ![image](https://github.com/user-attachments/assets/f93d27b0-c326-4c17-b1f0-82e5e1477955)

OUTPUT FOR QUESTION 1
   ![image](https://github.com/user-attachments/assets/6a4edbf4-1df5-4706-afd4-1274e548a079)

OUTPUT FOR QUESTION 2
   ![image](https://github.com/user-attachments/assets/46b78639-872b-4974-bdbb-1ba11e66b0d1)

OUTPUT FOR QUESTION 3
   ![image](https://github.com/user-attachments/assets/b465c451-c272-4b84-ab62-3ca9dbc99c5d)

OUTPUT FOR QUESTION 4 BOTTOM 10 CUSTOMERS WITH FEW DETAILS
   ![image](https://github.com/user-attachments/assets/3f0de0d4-4b45-497c-80b1-ab6c1b3960a4)
 
OUTPUT FOR QUESTION 4 ADVICE
   ![image](https://github.com/user-attachments/assets/c6bf15a3-b161-44bb-877e-4026f6a82862)

OUTPUT FOR QUESTION 5
   ![image](https://github.com/user-attachments/assets/639fcc0b-fa84-4f7f-9195-c2fc09cbd02c)

OUTPUT FOR QUESTION 6
  ![image](https://github.com/user-attachments/assets/b222c7f8-8233-49e7-a322-6a3b6d40c6c9)

OUTPUT FOR QUESTION 7 CODE
  ![image](https://github.com/user-attachments/assets/ae79afb7-8b8a-40ca-bd0c-dc066539b0af)

OUTPUT FOR QUESTION 8
  ![image](https://github.com/user-attachments/assets/976950f1-cc31-4e17-8957-4c6a2880c1f2)

OUTPUT FOR QUESTION 9
  ![image](https://github.com/user-attachments/assets/c951f2be-2f25-451d-845c-7ed7432ff652)

OUTPUT FOR QUESTION 10
  ![image](https://github.com/user-attachments/assets/5b2e5da0-c65d-4b4c-8d72-63edf517c67f)


