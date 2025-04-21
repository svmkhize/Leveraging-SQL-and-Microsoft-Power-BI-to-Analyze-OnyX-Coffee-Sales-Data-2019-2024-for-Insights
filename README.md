# Leveraging SQL and Microsoft Power BI to Analyze OnyX Coffee Sales Data (2019-2024) for Business Insights.

For more of my projects, visit [My Portfolio](https://svmkhize.github.io/Portfolio4SibusisoMkhize.github.io/)

## Table of Contents
---
- [Project Background and Overview](#project-background-and-overview)
- [Data Structure Overview](#data-structure-overview) 
- [Executive Summary](#executive-summary) 
- [Insights Deep-Dive](#insights-deep-dive) 
- [Observation and Recommendation](#observation-and-recommendation)


![SQL_2_PowerBI_Banner](https://github.com/user-attachments/assets/af868e38-b6d0-4f89-849d-316644ffad23)


## Project Background and Overview
---
OnyX Coffee, established in 2018, is a South Africa-based company that sells coffee in three countries: South Africa, Namibia, and Botswana. 
The company has a significant amount of data on its sales, product offerings, and loyalty program, which has been previously underutilized. I'm partnering with the Head of Operations to thoroughly analyze this data to uncover insights that will enhance OnyX’s profitability and commercial success. 

The goal of this data analysis project is to give the head of operations at OnyX Coffee useful information based on the company's current sales, product, and loyalty program data. The main goal is to provide answers to important strategic questions so that the leadership team can make well-informed decisions that will increase profitability and lead to better commercial success. 
The following primary topics of inquiry will be addressed by the project's thorough investigation of the data that is already available:

**1. Customer Identification and Loyalty:** identifying, characterising, and assessing the top ten consumers based on their purchase value and loyalty program participation. This will assist in identifying the most valued clients and determining how well the loyalty program engages them. 

**2. Loyalty Program Penetration:** Calculating how many clients are presently enrolled in the loyalty program. This will give an indication of the program's total reach and adoption rate. 

**3. Geographic Customer Distribution:** Calculating how many clients come from South Africa, Namibia, and Botswana, the three operating nations. This will provide information on regional client concentration and market penetration.

**4. Product Profitability Analysis:** Examining each OnyX Coffee product's profit margin to determine which are the most and least profitable. Pricing and product strategy decisions will be influenced by this.

**5. High-Performing Products:** Finding goods that generate a sizable amount of income in addition to having high profit margins. This will showcase the main goods that contribute to financial success.

**6. Coffee Bean Type Performance:** Examining the earnings and profits produced by each unique variety of coffee bean that OnyX sells. This will yield useful data for product development and sourcing. 

**7. Roast Preference Analysis:** Identifying the coffee roast type that produces the largest total sales volume, such as light, medium, or dark. This will direct marketing and production activities.

**8. Product Distribution and Country-Specific Sales:** Analysing the volume of orders and sales of coffee bean products in each of the three nations. This will show demand and sales activity particular to the market. 

**9. Country-Specific Financial Performance:** Examining the overall revenue and profit made in each operating nation. The financial contribution of each market will be clearly understood as a result.

**10. Top Products by Country:** Determining which product sells the best in each of the three nations. This will draw attention to product preferences unique to a given market. 

**11. Country-Specific Bean Type Performance:** Analysing the earnings and profits of every variety of coffee bean in every nation. This will give a detailed picture of market-specific bean preferences and profitability.

**12. Top South African Cities for Sales:** Identifying the top 5 South African cities in terms of coffee bean sales volume. This will assist in concentrating distribution and marketing activities in the home market.

**13. Historical Annual Revenue Trends:** Examining OnyX Coffee's total revenue for each of the previous six fiscal years (2019–2024). An outline of the company's revenue growth trend will be provided by this.

**14. Historical Monthly Revenue Trends:** To find seasonal patterns and trends in sales performance, the monthly revenue numbers for the previous six years (2019–2024) are examined.

Through thorough data analysis, this project seeks to answer these questions and provide actionable insights that will enable OnyX Coffee to improve customer engagement, streamline operations, and eventually increase profitability and long-term commercial success in the Southern African market.


## Data Structure Overview
---
OnyX’s database structure as seen below consists of three tables: orders, customers , and products, with a total row of 142 550 records. 

The Dataset was exported from OnyX Coffee MySQL Database as an Excel Sheet which can be found [HERE](https://github.com/svmkhize/OnyX_Coffee_Dataset_Repository/blob/main/OnyX_Coffee_Raw_Dataset.xlsb).

This was inspired by [Kaggle Website Dataset](https://www.kaggle.com/datasets/saadharoon27/coffee-bean-sales-raw-dataset).

![Onyx Coffee ERD](https://github.com/user-attachments/assets/9f5775fb-999f-4bc4-9a12-7410cdebfc4e)


## Executive Summary
---
This report presents a comprehensive analysis of OnyX Coffee's sales, product, and customer loyalty data to provide actionable insights for enhancing profitability and commercial success.  The analysis covers key areas such as customer loyalty, product performance, and geographical distribution.  Key findings include that Liberica coffee beans are the most profitable, light roast is the most popular roast type, and South Africa is the primary market.  The analysis also indicates that only 51% of customers are enrolled in the loyalty program, with 60% of the top ten customers being members.  Recommendations focus on leveraging these insights to improve customer engagement, product strategy, and market penetration. 

## Insights Deep-Dive
---
 **1. Customer Identification and Loyalty:** 
 
Who are the 10 best customers? Are they loyalty members?

```sql
SELECT orders.customer_id, customers.customer_name,  customers.loyalty_card, SUM(Quantity) AS quantity_purchased,
ROUND(SUM((orders.Quantity * products.unit_price)), 2) AS money_spent
FROM orders
LEFT JOIN customers ON orders.customer_id = customers.customer_id
LEFT JOIN products ON orders.product_id = products.product_id
GROUP BY orders.customer_id, customers.customer_name, customers.loyalty_card
ORDER BY money_spent DESC
LIMIT 10;
```

![Q1](https://github.com/user-attachments/assets/e5c9daed-ce9f-4ab5-9a73-fa41e7ef5367)

According to customer sales, these are the top ten clients.  According to the table, just 60% of the top ten customers hold loyalty cards.

 **2. Loyalty Program Penetration:** 

How many customers have loyalty cards?

 ```sql
SELECT loyalty_card, COUNT(loyalty_card) AS Count 
FROM customers
GROUP BY loyalty_card;
```
![Q2](https://github.com/user-attachments/assets/4c9c350d-106f-4f8d-b611-9b80db5c5f6c)

Fifty-one percent of consumers possess loyalty cards, while forty-nine percent do not.  It's interesting that, out of the top 10 customers on the list, only 60% had loyalty cards, although 51% of all customers had them.  Customers that purchase more goods from a business are typically more likely to become members.

 **3. Geographic Customer Distribution:** 

  How many customers are from each country?

 ```sql
SELECT customers.country, COUNT(DISTINCT customers.customer_id) AS customer_count
FROM customers
GROUP BY customers.country
ORDER BY customer_count DESC;
```
![Q3](https://github.com/user-attachments/assets/d93ded8f-64f9-4a51-bd72-1901260002d1)

70% of the clients are from South Africa, making them the largest group.

 **4. Product Profitability Analysis:** 

Which product has the largest and the lowest profit margins?

  ```sql
SELECT *
FROM products
WHERE profit = (SELECT Max(profit) FROM products) OR profit = (SELECT Min(profit) FROM products);
```
![Q4](https://github.com/user-attachments/assets/90b0e6e6-f042-4ed4-af32-f36e73bea49f)

Robusta Dark Roast (0.2 kg) has the lowest profit margin of any coffee bean commodity, at ZAR 0.16, while Liberica Light Roast (2.5 kg) has the largest profit margin, at ZAR 4.74.

 **5. High-Performing Products:** 

Products that have the highest profit margins and revenue.

  ```sql
SELECT orders.product_id, products.coffee_type, products.roast_type, products.Size,
ROUND(SUM((orders.quantity * products.unit_price)), 2) AS Revenue,
ROUND(SUM(orders.quantity * products.profit), 2) AS profit
FROM orders
LEFT JOIN products ON orders.product_id = products.product_id
GROUP BY orders.product_id, products.coffee_type, products.roast_type, products.Size
ORDER BY profit DESC
LIMIT 10;
```
![Q5](https://github.com/user-attachments/assets/b14dc6cf-7afe-412f-811f-2d67aedb78b5)

Size 2.5 is the largest size that OnyX Coffee sells, and it makes up the majority of their coffee bean goods.  This suggests that the corporation often makes more money when selling larger quantities of the different kinds of coffee beans.  This makes sense because larger products sell more even though their production costs are probably just somewhat higher.

 **6. Coffee Bean Type Performance:** 

What is the revenue and profit for each coffee bean type?

  ```sql
SELECT products.coffee_type, ROUND(SUM(orders.quantity * products.unit_price), 2) AS Revenue,
ROUND(SUM(orders.quantity * products.profit), 2) AS profit
From orders
LEFT JOIN customers ON orders.customer_id = customers.customer_id
LEFT JOIN products ON orders.product_id = products.product_id
GROUP BY products.coffee_type
ORDER BY Revenue DESC;
```
![Q6](https://github.com/user-attachments/assets/5ff1448b-5574-487a-8e4f-f24ea54b6d09)

Liberica coffee beans are the most profitable, which suggests that their sales are higher.  Customers place larger orders for Liberica coffee beans as a result, increasing their profit margin.  This indicates that selling Liberica coffee beans is more profitable for the business, therefore they may concentrate their marketing efforts on increasing sales of these beans.

 **7. Roast Preference Analysis:** 

What type of roast generate the most sales?

  ```sql
SELECT products.roast_type, ROUND(SUM(orders.quantity * products.unit_price), 2) AS Revenue
From orders
LEFT JOIN customers ON orders.customer_id = customers.customer_id
LEFT JOIN products ON orders.product_id = products.product_id
GROUP BY products.roast_type
ORDER BY Revenue DESC;
```
![Q7](https://github.com/user-attachments/assets/6d2b216f-d65e-4a97-9d01-e3abb3ffba9b)

With ZAR 1,113,800.03 in revenue, light roast is the most popular roast for The Roasters coffee beans.

 **8. Product Distribution and Country-Specific Sales:** 

Quantity sold and Number of Orders By Country

  ```sql
SELECT customers.country, SUM(orders.quantity) AS quantity_sold, COUNT(DISTINCT orders.customer_id) AS orders
FROM orders
LEFT JOIN customers ON customers.customer_id = orders.customer_id
GROUP BY customers.country
ORDER BY quantity_sold DESC;
```
![Q8](https://github.com/user-attachments/assets/5e67ba21-c212-4df7-96e5-86a69c6c7b0c)

South Africa accounts for the majority of orders.

 **9. Country-Specific Financial Performance:** 

Profit and Revenue Per Country

  ```sql
SELECT customers.country, ROUND(SUM((orders.quantity * products.unit_price)), 2) AS Revenue,
ROUND(SUM(orders.quantity * products.profit), 2) AS Profit
FROM orders
LEFT JOIN customers ON customers.customer_id = orders.customer_id
LEFT JOIN products ON orders.product_id = products.product_id
GROUP BY customers.country;
```
![Q9](https://github.com/user-attachments/assets/2700f9a6-519c-4a38-b7d5-07a82b851252)

It should come as no surprise that South African clients are OnyX Coffee's primary source of income and profit given the volume of orders .

 **10. Top Products by Country:** 

What product is most popular in each country?

  ```sql
WITH coffee_type AS (
	SELECT customers.country, products.coffee_type, products.roast_type, products.Size, SUM(quantity) AS Quantity_Sold
	FROM orders
	LEFT JOIN customers ON orders.customer_id = customers.customer_id
	LEFT JOIN products ON orders.product_id = products.product_id
	GROUP BY customers.country, products.coffee_type, products.roast_type, products.size
),
quant_rank AS (
	SELECT *, DENSE_RANK() OVER(PARTITION BY country ORDER BY Quantity_Sold DESC) AS q_rank
    FROM coffee_type
)
SELECT country, coffee_type, roast_type, size, Quantity_Sold
FROM quant_rank
WHERE q_rank = 1
ORDER BY Quantity_Sold DESC;
```

![Q10](https://github.com/user-attachments/assets/e06ea60e-0a0b-480c-91e1-33ef68d9c473)

Robusta medium roast (0.2 kg) was the most favoured product among South African consumers.  Robusta dark roast (0.5 kg) was the most preferred among Botswana's customers.  Arabica medium roast (0.5 kg) was the most widely consumed type in Namibia.

 **11. Country-Specific Bean Type Performance:** 

What is the revenue and profit of each type of coffee bean for each country?

  ```sql
WITH coffee_sold AS (
 SELECT customers.country, ROUND(SUM(orders.quantity * products.unit_price), 2) AS Revenue, products.coffee_type
 FROM orders
 LEFT JOIN customers ON orders.customer_id = customers.customer_id
 LEFT JOIN products ON orders.product_id = products.product_id
 GROUP BY customers.country, products.coffee_type
),
coffee_ranks AS (
  SELECT *, DENSE_RANK() OVER(PARTITION BY country ORDER BY Revenue DESC) AS coffee_rank
        FROM coffee_sold
)
SELECT coffee_type, country, Revenue
FROM coffee_ranks
ORDER BY Revenue DESC;
```
![Q11](https://github.com/user-attachments/assets/f0001724-ca0f-4c98-8d18-34ccf161d5b5)

According to the table, coffee bean varieties are preferred differently in each nation.  Liberica coffee beans generated the most money for South African consumers, totalling ZAR 613,583.51.

Liberica coffee beans were also the biggest source of income for Botswana's consumers, bringing in ZAR 179,031.14.

With ZAR 82,939.10, Excelsa coffee beans were the biggest source of income for Namibian consumers.

 **12. Top South African Cities for Sales:** 

What are the top 5 cities in South Africa for coffee bean sales?

  ```sql
SELECT customers.city, customers.country, SUM(orders.quantity) as Quantity_Sold, 
ROUND(SUM(orders.quantity * products.unit_price), 2) AS Sales
FROM orders
LEFT JOIN customers ON orders.customer_id = customers.customer_id
LEFT JOIN products ON orders.product_id = products.product_id
WHERE customers.country = 'South Africa'
GROUP BY customers.country, customers.city
ORDER BY Sales DESC
LIMIT 5;
```
![Q12](https://github.com/user-attachments/assets/23135d42-a7ab-4bb1-b066-c180db55fe66)

According to the list, Cape Town and Johannesburg lead the Roasters in coffee bean sales.  Given that both cities rank among the nation's two largest and most urbanised, this is not surprising.   In large cities, more people drink coffee.

 **13. Historical Annual Revenue Trends:** 

  ```sql
SELECT YEAR(orders.order_date) AS Years,
ROUND(SUM(orders.quantity * products.unit_price), 2) AS Revenue
From orders
LEFT JOIN products ON orders.product_id = products.product_id
GROUP BY Years
ORDER BY Years ASC;
```

![Q13](https://github.com/user-attachments/assets/a3ff38fb-4702-4ce0-a3b5-f69b9284a0ca)

Overall, total sales have shown a gradual increase from ZAR 493,447 in 2019 to ZAR 500,853 in 2024.

 **14. Historical Monthly Revenue Trends:** 

How much revenue did we make each month in the last 6 years (2019 to 2024)

  ```sql
SELECT YEAR(orders.order_date)AS order_year, MONTHNAME(orders.order_date) AS order_month,
ROUND(SUM(orders.quantity * products.unit_price), 2) AS Sales 
FROM orders
LEFT JOIN products ON orders.product_id = products.product_id
GROUP BY order_year, order_month
ORDER BY Sales DESC;
```
![Q14](https://github.com/user-attachments/assets/b516f421-63ee-482d-9492-bc09d2514185)

During Southern Africa's winter months of May, June, July, and August, OnyX Coffee makes the most money.  Wintertime is often when people drink the most coffee.

## Observation and Recommendation
---

Based on the analysis of OnyX Coffee's data, the following observations and recommendations are made:

* **Customer Loyalty:** While 51% of customers are enrolled in the loyalty program, only 60% of the top ten customers are members. This suggests a potential opportunity to increase loyalty program penetration among high-value customers.<br>
  **Recommendation:** Implement targeted campaigns to encourage loyalty program sign-ups, focusing on high-value customers to increase retention and repeat purchases.

* **Product Profitability:** Liberica coffee beans have the highest profit margin.<br>
  **Recommendation:** Prioritize marketing and sales efforts towards Liberica coffee beans to maximize profitability.

* **Roast Preference:** Light roast coffee beans generate the most sales.<br>
  **Recommendation:** Ensure sufficient stock of light roast beans and consider featuring them in promotional activities.

* **Geographic Focus:** South Africa is the primary market, accounting for the majority of orders, revenue, and profit.<br>
  **Recommendation:** Continue to focus on the South African market while also developing strategies to grow sales in Botswana and Namibia.

* **Sales Trends:** Sales are higher during the winter months (May, June, July, and August) in Southern Africa.<br>
  **Recommendation:** Optimize inventory and staffing levels to meet the increased demand during these months. Consider running winter-themed promotions to further capitalize on this trend.


If you are interested in my SQL code you can find it [here](https://github.com/svmkhize/Analyzing-OnyX-Coffee-Sales-Data-from-2019-to-2024-with-SQL-for-Business-Insights/blob/main/OnyXCoffeeQueries.sql)

For more of my projects, visit [My Portfolio](https://svmkhize.github.io/Portfolio4SibusisoMkhize.github.io/)
