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

select top 1
	customer_name, customer_segment,
	sum(sales) as totalsales
	from [dbo].[KSA SQL CASE STUDY]
	where Customer_Segment = 'small business'
	 group by customer_name, customer_segment
	 order by totalsales desc;

--QUESTION 8 (CORPORATE CUSTOMER WITH MOST ORDERS 2009 -2012)


SELECT TOP 1
	CUSTOMER_NAME, CUSTOMER_SEGMENT, 
	COUNT(ORDER_ID) AS TOTALORDERS
	FROM [dbo].[KSA SQL CASE STUDY]
	WHERE CUSTOMER_SEGMENT = 'CORPORATE'
	AND ORDER_DATE BETWEEN '2009-01-01' AND '2012-12-31'
	GROUP BY CUSTOMER_NAME, CUSTOMER_SEGMENT
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


---QUESTION 11 

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

--NOTE; THE QUERY WRITTEN WAS DONE USING THE SHIP_COST INSTEAD OF SALES - PROFIT AS THE LATTER WILL ADD OTHER EXPENDITURES AND NOT ONLY SHIP_COSTS
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
 ![image](https://github.com/user-attachments/assets/dc7d044b-2262-40b1-9e29-033064850415)

OUTPUT FOR QUESTION 8
 ![image](https://github.com/user-attachments/assets/214bc641-ebc3-475a-8ccc-f9f3bed0301a)

OUTPUT FOR QUESTION 9
  ![image](https://github.com/user-attachments/assets/c951f2be-2f25-451d-845c-7ed7432ff652)

OUTPUT FOR QUESTION 10
  ![image](https://github.com/user-attachments/assets/f40c0fa3-9ea8-47b8-a915-364ebd9ffdc8)

OUTPUT FOR QUESTION 11
  ![image](https://github.com/user-attachments/assets/d2f3f8ab-05c4-43b4-80b8-578b6063ac76)

## INSIGHTS AND RECOMMENDATIONS
---
From the analysis done on the Kultra Mega Stores dataset, The following interpretations were drawn:

1. The Top products category is Technology with 5,984,248.491
   
2. The Top 3 regions by sales are West, Ontario, and Prarie with total sales of 3,597,549.403, 3,063,212.596, and 2,837,304.602 respectively. The bottom 3 Regions are Yukon, Northwest Territories, and Nunavut, with total sales of 975,867.393, 800,847.351, and 116,376.47 implying that Nanuvut is at the very bottom having the least total sales.

3. The total sales for appliances in Ontario is 202,346.839.

4. The Bottom 10 customers by sales are;
   ◇ Jeremy Farry
   ◇ Natalie Dechemey
   ◇ Nicole Fjeld
   ◇ Katrina Edelman
   ◇ Dorothy Disckson
   ◇ Christine Kargatis
   ◇ Eric Murdock
   ◇ Chris Mcafee
   ◇ Rick Huthwaite
   ◇ Mark Hamilton
   
Advices to improve the revenue of the Bottom 10 Customers
-  Obtain Customer information:
comapny shoul get informations about the above 10 customers, reach out to them and know the cause for their poor purchase
- Customer Feedback:
Managment should encourage and collect feedback from customers to understand their needs and preferences
- Product Improvement:
Management should provide and improve products based on preferences and needs from customers feedback (if need be).
- Improve Company-customer Relationship (customer care):
Company should provide a platform to build loyalty by providing or improving (an already existing) customer care service
- Company Feedbacks to Customers:
Managementshould create a follow up team to follow products bought to boost customer confidence.
The above advices can also be implemented for other customers.

5. The Ship-mode with the most incurred cost is Delivery Truck with a total cost of 51,971.939

6. The most valuable customers are
   1. Emily Phan
   2. Jasper Cacioppo
   3. Craigo Carreria
All three of them bought the same product ( Polycom view Station "ISON")

7. The small Business Customer who with the highest sales is Dennis Kane with a total sales of 75,967.592

8. Adam Hart is the corporate customer with the most orders between 2009 to 2012, placing a total order of 27.

9. The top consumer customer is Emily phan

10. A whooping 573 customers returned purchased items, below are the names of 10 of them:
    - Tamara Dehlen
    - Jonathan Dohety
    - Michael Dominquez
    - Anne Pryor
    - Ern Creighton
    - Frank Gastineau
    - Cari Sayre
    - Sheri Gordon
    - Dave Hallsten
    - Dorothy Badders

11. 

## ABOUT ME
---
I'm Rachel Adenike Adebowale, a passionate Data Analyst, HND in Science Laboratory Technology (Chemistry) from Petroleum Training Institute. I’m skilled in tools like Microsoft Excel, Word, PowerPoint, Power BI, and SQL, and I enjoy uncovering insights from data to solve real-world problems.

Beyond analytics, I'm also a Drama Minister Who loves God passionately with a strong drive to contribute meaningfully to the Oil and Gas sector through innovation, research, and data-driven solutions.

I'm open to professional opportunities, collaborations, and faith-based engagements that align with my purpose and technical growth.

CONTACTS
📧 Email: adenikke2004@gmail.com

📞 Phone: +234 901 429 2606

