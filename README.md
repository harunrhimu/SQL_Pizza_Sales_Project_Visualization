## Pizza Sales SQL Queries Presentation with Tableau Dashboard
## KPI's & Trends Distributions
![Full Projects](https://github.com/harunrhimu/SQL_Pizza_Sales_Project_Visualization/blob/main/KPI_Dashboard.jpg))
## Best & Worst Sell 
![best and worst dashboard](https://github.com/harunrhimu/SQL_Pizza_Sales_Project_Visualization/blob/main/bestworstdashboard.jpg)


### Background:
Our pizza chain needs a detailed analysis of our sales performance. To do this, we require SQL queries that will extract and present key metrics from our sales database. This will help us understand our business performance, identify trends, and make informed decisions to improve our operations and profitability.

### Objective:
The objective of this project is to develop and present a set of SQL queries that will extract critical KPIs and trends related to our pizza sales. The results will provide valuable insights into our sales performance, helping us make data-driven decisions.

### Problem Statement 1: KPI Requirements

The queries should provide real-time insights into the following key performance indicators (KPIs):

1.	Total Revenue: Calculate the total revenue generated from pizza sales.

2.	Average Order Value: Determine the average value of each order.

3.	Total Pizzas Sold: Count the total number of pizzas sold.

4.	Total Orders: Count the total number of orders placed.

5.	Average Pizzas Per Order: Calculate the average number of pizzas sold per order.

### Problem Statement 2: Trend and Distribution Requirements

1. Hourly Trend of Total Pizzas Sold: Extract data to display a Bar chart showing the total number of pizzas sold each hour.
2. Weekly Trend of Total Pizzas Sold: Extract data to display a line chart illustrating the weekly trend of total pizzas sold.

3. Percentage of Total Orders by Pizza Category: Extract data to present the distribution of total orders across different pizza categories using a donut chart.

4. Percentage of Sales by Pizza Size: Extract data to present the contribution of different pizza sizes to the total sales through a bar chart.

5. Top and Bottom Seller Analysis:

    - Top 5 Best Seller Pizzas by Revenue: Identify the top 5 pizzas generating the highest revenue.

    - Bottom 5 Best Seller Pizzas by Revenue: Identify the bottom 5 pizzas generating the lowest revenue.

    - Top 5 Best Seller Pizzas by Total Quantity: Identify the top 5 pizzas sold in the highest quantities.

    - Bottom 5 Best Seller Pizzas by Total Quantity: Identify the bottom 5 pizzas sold in the lowest quantities.

### Deliverables:
- SQL queries for each KPI and trend requirement.
- Presentation of the query results in charts and tables as specified.
- Analysis and insights derived from the data to support decision-making.

By completing this project, we aim to gain a comprehensive understanding of our pizza sales performance and uncover actionable insights to drive growth and efficiency.
# The Tools I Used:
I used several key tools to deep dive in the data analyst. 
- **SQL:** The backbone of my data analyst which is enable me to figure out what i wanted to solve the business problem.
- **PostgreSQL:** The database management system
- **Tableau Public:** The most powerfull industry demand visualisation tools used to visualized this entire project.
- **Visual Studio Code:** PostgreSQL connected with VS Code to excuting SQL queries.
- **Git & GitHub:** This is a essential tools for verson control and sharing my project and analysis. 



# The Analysis

The questions are illustrated below want to excute specific business problem in term of Pizza sales issues. 

### Problem Statement 1: KPI Requirements

The queries should provide real-time insights into the following key performance indicators (KPIs):

•	Total Revenue: Calculate the total revenue generated from pizza sales.

```sql
SELECT CAST(SUM(total_price) AS DECIMAL (10,2)) AS Total_Revenue
FROM pizza_sales

```
<p align="center">
  <img src="https://github.com/harunrhimu/SQL_Pizza_Sales_Project_Visualization/blob/main/assets/TotalRevenue.jpg?raw=true">
</p>


•	Average Order Value: Determine the average value of each order.

```sql
SELECT SUM(total_price) / COUNT(DISTINCT order_id) AS Average_Order_Value
FROM pizza_sales
```

<p align="center">
  <img src="https://github.com/harunrhimu/SQL_Pizza_Sales_Project_Visualization/blob/main/assets/averageOrderValue.jpg?raw=true">
</p>
•	Total Pizzas Sold: Count the total number of pizzas sold.

```sql
SELECT SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
```
<p align="center">
  <img src="https://github.com/harunrhimu/SQL_Pizza_Sales_Project_Visualization/blob/main/assets/TotalPizzaSold.jpg?raw=true">
</p>



•	Total Orders: Count the total number of orders placed.
```sql
SELECT count(distinct order_id) AS Total_Orders
FROM pizza_sales
```
<p align="center">
  <img src="https://github.com/harunrhimu/SQL_Pizza_Sales_Project_Visualization/blob/main/assets/TotalOrders.jpg?raw=true">
</p>

•	Average Pizzas Per Order: Calculate the average number of pizzas sold per order.
```sql
SELECT CAST(  --this is for 2 decimal
			CAST( --this is for total output will be decimal
				SUM(quantity) AS DECIMAL (10,2)) / 
			CAST( --this is for total output will be decimal
				COUNT (DISTINCT order_id) AS DECIMAL (10,2)) 
					AS DECIMAL(10,2)) AS Average_Pizza_Per_Orders   --this is for 2 decimal
FROM pizza_sales

```
<p align="center">
  <img src="https://github.com/harunrhimu/SQL_Pizza_Sales_Project_Visualization/blob/main/assets/AveragePizzaPerOrder.jpg?raw=true">
</p>





## Problem Statement 2: Trend and Distribution Requirements

### 1. Hourly Trend of Total Pizzas Sold:
 Extract data to display a Bar chart showing the total number of pizzas sold each hour.
```sql
--Datepart will extract hour data from order_time
SELECT DATEPART(HOUR, order_time) AS Order_Hour, SUM(quantity) AS TotalPizzaSold
FROM pizza_sales
GROUP BY DATEPART(HOUR, order_time)
ORDER BY DATEPART(HOUR, order_time)
```
<p align="center">
  <img src="https://github.com/harunrhimu/SQL_Pizza_Sales_Project_Visualization/blob/main/assets/HourleyTrendTotalPizza.jpg?raw=true">
</p>

### 2. Weekly Trend of Total Pizzas Sold:
 Extract data to display a line chart illustrating the weekly trend of total pizzas sold.

```sql
--Datepart will extract IS_Week data from order_time
SELECT DATEPART(ISO_WEEK, order_date) AS Week_Number, --it will shows total weeks of a year
		YEAR(order_date) as Order_Year, --years of 2015 showed
		count(distinct order_id) AS Total_Orders
FROM pizza_sales
GROUP BY DATEPART(ISO_WEEK, order_date), YEAR(order_date) --aggregated function are used as a group by statement (requirement)
ORDER BY DATEPART(ISO_WEEK, order_date), YEAR(order_date) --aggregated function are used as a order by statement (requirement)
```
<p align="center">
  <img src="https://github.com/harunrhimu/SQL_Pizza_Sales_Project_Visualization/blob/main/assets/WeeklyTrendTotalPizzaSold.jpg?raw=true">
</p>

### 3. Percentage of Total Orders by Pizza Category: 
Extract data to present the distribution of total orders across different pizza categories using a donut chart.

```sql
SELECT pizza_category AS P_Categrory, ROUND(sum(total_price),2) as Total_Sales , ROUND( SUM(total_price) * 100 / 
-- A subquery used here for correct result			
					(SELECT SUM(total_price) from pizza_sales WHERE MONTH(order_date) = 1 ),2) AS Percentage_OfTotalSales
FROM pizza_sales
--when we used filter in main query we should use this filter also subqueyr
WHERE MONTH(order_date) = 1 
--WHERE MONTH(order_date) = 1  indicateds that will shows the month of january = 4 shows the month of April
GROUP BY pizza_category
ORDER BY Percentage_OfTotalSales DESC, Total_Sales DESC
```
<p align="center">
  <img src="https://github.com/harunrhimu/SQL_Pizza_Sales_Project_Visualization/blob/main/assets/TotalOrder&PizzaSoldbyPizzaCategory.jpg?raw=true">
</p>

### 4. Percentage of Sales by Pizza Size: 
Extract data to present the contribution of different pizza sizes to the total sales through a bar chart.

```sql
SELECT pizza_size, CAST(SUM(total_price) AS DECIMAL (10,2)) AS Total_Sales, CAST(
					SUM(total_price) * 100 / 
-- A subquery used here for correct result	
-- Both subquery and main query should use filter (SELECT SUM(total_price) FROM pizza_sales WHERE DATEPART(QUARTER, order_date) = 1) AS decimal (10,2) ) AS Percent_SalesBYPizza_Size --rounded query end this line
FROM pizza_sales
--Where quarter is 1 
WHERE DATEPART(QUARTER, order_date) = 1
GROUP BY pizza_size
ORDER BY pizza_size, Total_Sales DESC
```
<p align="center">
  <img src="https://github.com/harunrhimu/SQL_Pizza_Sales_Project_Visualization/blob/main/assets/PercentageofSalesbyPizzaSize.jpg?raw=true">
</p>

### 5. Top and Bottom Seller Analysis:

•	Top 5 Best Seller Pizzas by Revenue: Identify the top 5 pizzas generating the highest revenue.
```sql
SELECT TOP 5 pizza_name, CAST(sum(total_price) AS DECIMAL (10,2)) AS Total_Revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue DESC 
```
<p align="center">
  <img src="https://github.com/harunrhimu/SQL_Pizza_Sales_Project_Visualization/blob/main/assets/Top5PizzabyRevenue.jpg?raw=true">
</p>

•	Bottom 5 Best Seller Pizzas by Revenue: Identify the bottom 5 pizzas generating the lowest revenue.
```sql
SELECT TOP 5 pizza_name, CAST(sum(total_price) AS DECIMAL (10,2)) AS Total_Revenue 
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue ASC --to figur out bottom 5 pizza
```
<p align="center">
  <img src="https://github.com/harunrhimu/SQL_Pizza_Sales_Project_Visualization/blob/main/assets/Bottom5PizzabyRevenue.jpg?raw=true">
</p>

•	Top 5 Best Seller Pizzas by Total Quantity: Identify the top 5 pizzas sold in the highest quantities.
```sql
SELECT TOP 5 pizza_name, CAST(sum(quantity) AS DECIMAL (10,2)) AS Total_Quantity
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Quantity DESC 
```
<p align="center">
  <img src="https://github.com/harunrhimu/SQL_Pizza_Sales_Project_Visualization/blob/main/assets/Top5PizzabyQuantity.jpg?raw=true">
</p>

•	 Bottom 5 Best Seller Pizzas by Total Quantity: Identify the bottom 5 pizzas sold in the lowest quantities.
```sql    
SELECT TOP 5 pizza_name, CAST(sum(quantity) AS DECIMAL (10,2)) AS Total_Quantity
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Quantity ASC --to figur out bottom 5 pizza
```
<p align="center">
  <img src="https://github.com/harunrhimu/SQL_Pizza_Sales_Project_Visualization/blob/main/assets/Bottom5PizzabyQuantity.jpg?raw=true">
</p>

•	 Top 5 Best Seller Pizza By Total Orders
```sql
SELECT TOP 5 pizza_name, CAST(count(distinct order_id) AS DECIMAL (10,2)) AS Total_Orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Orders DESC 
Output:
```
<p align="center">
  <img src="https://github.com/harunrhimu/SQL_Pizza_Sales_Project_Visualization/blob/main/assets/Top5PizzabyOrders.jpg?raw=true">
</p>
•	 Bottom 5 Best Seller Pizza By Total Orders 

```sql
SELECT TOP 5 pizza_name, CAST(count(distinct order_id) AS DECIMAL (10,2)) AS Total_Orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Orders ASC --to figur out bottom 5 pizza
```
<p align="center">
  <img src="https://github.com/harunrhimu/SQL_Pizza_Sales_Project_Visualization/blob/main/assets/Bottom5PizzabyOrders.jpg?raw=true">
</p>







## Pizza Sales Dashboard Insights

**Key Performance Indicators (KPI)**

- **Total Revenue**: $817.86K
- **Total Pizzas Sold**: 49.57K
- **Total Orders**: 21.35K
- **Average Order Value**: $38.31

**Order Patterns by Time and Week:**

- **Peak Hours**: Orders peak between 12:00 PM and 1:00 PM, and again in the evening from 4:00 PM to 7:00 PM.
- **Weekly Variations**: Significant fluctuations in weekly orders, with the highest peak occurring during the 48th week of the year, typically in December.

**Category Insights:**

- **Top Category**: The Classic Category leads in sales, total orders, and total pizzas sold.

**Size Insights:**

- **Top Size**: Among total sales Large Size pizza is highest percentage of sales generate which is 45.89%.

**Product Performance by Metrics:**

- **Top and Bottom Pizza sold by Revenue**: 
The Thai Chicken Pizza is the top contributor which generate $45.43K on the other side The Brie Carre Pizza contributes the least which is $11.59K.

- **Top and Bottom Pizza sold by Quantity**: 
The Classic Deluxe Pizza has the highest total quantities sold which is generate $2.45K. on the other side The Brie Carre Pizza has the lowest with $490 .

- **Top and Bottom Pizza sold by Total Orders**: 
The Classic Deluxe Pizza has the highest number of orders with 2329 pizza on the other side The Brie Carre Pizza has the fewest orders with 480.

## Recommendations for Pizza Sales Optimization

Based on the insights from the Pizza Sales Dashboard, here are several recommendations to optimize sales and improve overall performance:

**1. Enhance Peak Hour Operations: **
Given that orders peak between 12:00 PM - 1:00 PM and 4:00 PM - 7:00 PM, ensure adequate staffing during these hours to handle the higher volume of orders efficiently. Also ensure faster order processing  during peak hours to reduce wait times and improve customer satisfaction.
**2. Leverage Weekly Peaks: **
- **Special Promotions:** Capitalize on the high order volume during the 48th week of the year by offering special promotions or limited-time offers. This can further boost sales during this already high-performing week.  Use targeted marketing campaigns in the weeks leading up to this peak period to attract more customers and increase sales.
**3. Focus on Top Performing Categories and Sizes and Improve low-selling items:**
Since the Classic Category leads in sales, focus marketing efforts on this category. Offer combo deals this category and Large size of pizza which contributing 45.89% of sales. Consider either rebranding or improving the recipe for the Brie Carre Pizza, which is the lowest performer.
**5. Maintain Top Performers receiving customer feedback:**
These pizzas are the top performers in revenue and quantity sold, respectively. Promote them as signature items on the menu. Consider creating promotional campaigns featuring these pizzas to attract more customers. Continuously gather feedback on these top-performing pizzas to ensure they maintain their popularity and address any potential issues promptly.

**6. Seasonal and Event-Based Promotions:**
Given the peak during the 48th week (typically December), introduce seasonal pizzas or holiday-themed promotions to capitalize on the festive period.
Organize events or special promotions around major holidays or local events to drive traffic and boost sales.



**Thanks**


