# Chocolate shop Sales Analysis

## Project Overview
---
This Project focuses on Understanding Past and Present Performance (i.e Identifying what's selling and what's not,), Measuring sales team performance,
Tracking key performance indicators (KPIs) over time and in different countries

### Data Source
Primary data source: Click the url to obtain primary data 'https://www.kaggle.com/datasets/atharvasoundankar/chocolate-sales' .

### Tools
- Excel: Data Cleaning
- SQL Server: Data Analysis

### Data Cleaning/Preparation
The following tasks were performed:
- Data loading and inspection
- Data cleaning and formatting

### Exploratory Data Analysis (EDA)
The EDA provide answers to the following key questions, such as:
-Overall sales trend
-Top selling products generally and some specific countries
-Peak sales period
-sales trend month over month
-Staff KPI's

### Data Analysis
Some interesting codes/features used include:
- overview of chocolate table
```sql
SELECT *
FROM az_chocolate;
```

- Top (10) Performing Staffs
```sql
SELECT Sales_Person, sum(Amount) as Total_Amount
FROM az_chocolate
GROUP BY Sales_Person
ORDER BY Total_Amount DESC
LIMIT 10;
```

- Top (10) LOW Selling Staffs
```sql
SELECT Sales_Person, sum(Amount) as Total_Amount
FROM az_chocolate
GROUP BY Sales_Person
ORDER BY Total_Amount ASC
LIMIT 10;
```

- Best 5 Selling product
```sql
SELECT Product, sum(Boxes_Shipped) as Total_Box_sold
FROM az_chocolate
GROUP BY Product
ORDER BY Total_Box_sold DESC
LIMIT 5;
```

- Sales trend per month
```sql
SELECT SUM(Amount) AS Total_Sales,
		SUM(Boxes_Shipped) AS Total_Box_sold,
		month(Trade_Date) as Month
FROM az_chocolate
GROUP BY Month
ORDER BY Total_Sales DESC;
```

- Top selling countries
```sql
SELECT Country, SUM(Amount) aS Total_Sales, SUM(Boxes_Shipped) AS Total_Box_sold
FROM az_chocolate
GROUP BY Country
ORDER BY Total_Sales DESC;
```

- Top selling Product in Austalia
```sql
SELECT Product, sum(Amount) as Sales
FROM az_chocolate
where Country = 'Australia'
group by Product
order by Product DESC
limit 5;
```

- how many sale person operate within a country
```sql
select Country, COUNT(Sales_Person) as Emp_Total
from az_chocolate
group by Country
order by Emp_Total DESC;
```

- sales trend month over month in various country
```sql
with MonthlySales as
(
select Country,
		month(Trade_Date) as Month,
        sum(Amount) as Total_Sales
from az_chocolate
group by
	    Country,
		month(Trade_Date)
)

select Country,
		 Month,
         Total_Sales,
         lag(Total_Sales) over(partition by Country order by month) as PreviousMonthSales,
         Total_Sales - lag(Total_Sales) over(partition by Country order by month) as SalesMOM_Difference,
         ((
         Total_Sales - lag(Total_Sales) over(partition by Country order by month)
         ) / lag(Total_Sales) over(partition by Country order by month)) * 100 as Sales_Difference_Percentage
         
from MonthlySales 
```


### Results/Findings
Summary of the Analysis Result is as follow:
1. DarkBite Chocolate appears to be the best selling product with 35,614 boxes sold
2. Sales in Australia seems to be the highest, as it amount a total revenue of 2274734  which accounts for 18.3% of the total revenue generated
3. Month over Month(MOM) sales difference has not been impressive as there are huge fluctuations in diferences among counries

### Recommendations
Based on the analysis, certain actions are recommended, viz;
1. January and June, appears to be the sales-hike-months, so heavy stockings of goods is recommended
2. Monthly Incentives are highly recommended to be given to sales-men/women, who meets or crossed the monthly target, this to boost the MOM which has not been impressive lately



