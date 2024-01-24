# DATA CLEANING AND ANALYSIS IN SQL USING THE NORTHWIND DATASET

### Dataset Overview

Northwind Dataset is about a company named "Northwind Traders" that imports and exports specialty 
foods from around the world. It contains the following detailed information between 1996-1998:

- Suppliers/Vendors of Northwind -who supply to the company.
- Customers of Northwind- who buy from Northwind.
- Employee details of Northwind traders-who work for Northwind.
- Product information-the products that Northwind trades in
- The inventory details/transactions -the details of the inventory held by Northwind traders.
- The shippers -details of the shippers who ship the products from the traders to the end-customers
- Purchase Order transactions-details of the transactions taking place between vendors & the company.
- Sales Order transaction-details of the transactions taking place between the customers & the company.

### PROJECT GOAL AND OBJECTIVES:
This project seeks to the analyze and determine the following insights:
1. Sales/Revenue Analysis: the Sales trend over time.
2. Customer Analysis: the key and top customers enhancing the company's productivity.
3. Product Analysis: The best and worst products of the company.
4. Geographic Analysis: the country where the best and worst order originates.
5. Order Analysis:TOP orders by Most Expensive Freight.

### DATA SOURCE:
The dataset was downloaded from Kaggle in a CSV format [download here](https://www.kaggle.com/datasets/jeetahirwar/northwind-traders)

## DATA PREPARATION AND EXPLORATION:
The dataset was downloaded in a CSV format containing 11 different tables. It was imported into Postresql Server by creating a new database named "Northwind" and thus, created different tables in accordance with the file to allow easy import.

The dataset was thoroughly explored and it was discovered that the dataset contained 11 tables named:
- Categories: 8 rows and 4 columns
- Customers: 91 rows and 11 columns
- Employees Territories: 49 rows and 2 columns
- Employees: 9 rows and 18 columns
- Order Details: 2155 rows and 5 columns
- Orders: 830 rows and 17 columns
- Products: 77 rows and 15 columns
- Region: 4 rows and 2 columns
- Shippers: 6 rows and 3 columns
- Suppliers: 29 rows and 12 columns
- Territories: 53 rows and 3 columns

Null values were discovered in a number of Columns. 
Redundant and irrelevant columns were also discovered.

### DATA CLEANING AND PROCESSING:
This was done with PostgreSQL.
In order to check the data quality and ensure an accurate and consistent analysis, the null values in the Region columns were replaced with 'No Region' as deleting the rows would affect the analysis. 
The Numeric Null columns like Fax and postal_code columns were replaced with 0.

Picture column in Categories Table, Photo column in Employee Table and Homepage Column in Suppliers Table were deleted as they were redundant and irrelevant to the analysis.

### LIMITATION
A new Revenue column was created in the Order Details Table and the formula, unit price*quantity was used to populate the new column.

### ANALYSIS:
The Key Performance Indicators are as follows:
- Total Customers
```sql
SELECT COUNT(DISTINCT customer_id) AS Total_Customer 
FROM customers
```
- Total Order
```sql
SELECT COUNT(DISTINCT order_id) AS Total_Order   
FROM orders
```
- Total Quantity Sold
```sql
SELECT SUM(quantity) AS Total_Quantity
FROM order_details
```
- Total Revenue
```sql
SELECT SUM(revenue)AS Total_Revenue
FROM order_details
```
- Total Product
```sql
SELECT COUNT(DISTINCT product_id)AS Total_Product
FROM products
```
- Average Revenue/Sales
```sql
SELECT ROUND(AVG(revenue),0) AS Avg_revenue
FROM order_details
```
The Generated Insights Include:
- Sales Analysis
```sql
SELECT o.order_date,od.revenue
FROM orders o
JOIN order_details od
ON o.order_id=od.order_id
ORDER BY o.order_date Desc
```
The Average sales/Revenue was used as a threshold value to determine sales performance
```sql
SELECT order_id,revenue,
      CASE
	     WHEN revenue > 629 THEN 'High_Sale'
		 WHEN revenue < 629 THEN 'Poor_Sale'
		 ELSE 'Medium_Sale'
		END AS revenue_analysis
FROM order_details
```

- Customer Analysis:
- The Average Sales Order was used as a threshold value to determine the Top/Key customers
```sql
SELECT o.customer_id,od.revenue
FROM orders o
JOIN order_details od
ON o.order_id=od.order_id
WHERE od.revenue > 629
ORDER BY od.revenue DESC
LIMIT 10
```

- Product Analysis:
- Top product by Revenue
```sql
SELECT  p.product_name, od.revenue
FROM products p
JOIN order_details od
ON p.product_id=od.product_id
ORDER BY od.revenue DESC
LIMIT 3
```
- Bottom Product by Revenue
```sql
SELECT  p.product_name, od.revenue
FROM products p
JOIN order_details od
ON p.product_id=od.product_id
ORDER BY od.revenue 
LIMIT 3
```

- Geographic Analysis:
- Order by Top Countries
```sql
SELECT o.order_id,c.country,od.revenue
FROM customers c
JOIN orders o
ON c.customer_id=o.customer_id
JOIN order_details od
ON o.order_id=od.order_id
ORDER BY od.revenue DESC
LIMIT 2
```
- Order by Bottom Countries
```sql
SELECT o.order_id,c.country,od.revenue
FROM customers c
JOIN orders o
ON c.customer_id=o.customer_id
JOIN order_details od
ON o.order_id=od.order_id
ORDER BY od.revenue 
LIMIT 2
```

- Order Aalysis:
- Order by most expensive Freight
```sql
SELECT order_id,freight
FROM orders
ORDER BY freight DESC
LIMIT 10
```

### KEY FINDINGS AND INSIGHTS:

- The Total Customer between was 91
- The Total Order was 830
- The Total Revenue is $1,354,459
- The Total Product is 77
- The Total Quantity Sold is 51,317
- The Average Revenue is $629
- There was a drop in sale in 1998, this could be due to poor market strategy.The company experienced poor quality sale compared to high quality sale
- The key/Top 3 customers are HANAR, QUICK and PICCO
- The top product is Cote de Blaye while the bottom products are Konbu, Teatime Chocolate Biscuit and Filo Mix
- The most order originated from Germany and Brazil while the least orders originated from UK and Spain
- The Order with ID, 10540 has the most expensive freight of 1008.


### RECOMMENDATIONs
- There should be an improvement in the marketing strategy. More advert should be made to create awareness for the
company and to improve the business sale.
- The sale channel for the least selling products should be enhanced and the quality of the most selling should be sustained.
- A research/finding should be carried out on why a product is demanded more by a country compared to another. This would be used to determine strategicsolution for the least wanted product.





