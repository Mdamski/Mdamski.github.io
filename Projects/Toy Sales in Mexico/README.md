# Toy Sales in Mexico - SQL Data Analysis Project

## Tools and Skills
### Tools Used: SQL
### Skills Demonstrated: Data Querying, Aggregation, Joins, CTE, Data Analysis,

## Project Overview
This project explores sales and inventory data of toy stores in Mexico. It focuses on analyzing inventory, profit margins, sales trends, and potential stockout effects. The goal is to provide actionable insights using SQL queries.

## Dataset
The dataset contains three key tables:
- Sales: Contains transaction-level data with Sale_ID, Date, Product_ID, Store_ID, and Units Sold.
- Inventory: Contains information on Product_ID, Store_ID, and Stock_On_Hand.
- Products: Contains product details including Product_ID, Product_Name, Product_Category, Product_Cost_$, and Product_Price_$.

## Code
```
-- How long will stores' stocks last?

with 
daily_sales as 
(
	select 
	    Product_ID,
	    Store_ID,
	    sum(Units) / (select count(distinct Date) from sales) as avg_sales_per_day 
	from sales s
	group by 1,2
),
stock as
(
	select
	Store_ID ,
	Product_ID ,
	Stock_On_Hand 
	from inventory i 
)
select
	ds.Product_ID,
	ds.Store_ID,
	ds.avg_sales_per_day,
	round( st.Stock_On_Hand / ds.avg_sales_per_day ,1) as days_until_out_of_stock
from daily_sales ds
inner join stock st on ds.Product_ID = st.Product_ID and ds.Store_ID = st.Store_ID
order by 1,2

-- How much money is tied up in toy store inventory?

select 
	p.Product_ID ,
	p.Product_Name ,
	round( sum(i.Stock_On_Hand * p.`Product_Cost_$`),2) as total_cost_,
	round( sum(i.Stock_On_Hand * p.`Product_Price_$`),2) as potencial_sale_value 
from products p 
inner join inventory i on p.Product_ID = i.Product_ID 
group by 1,2

-- Which product categories generate the most profit? 

select 
	p.Product_Category,
	sum( (p.`Product_Price_$` - p.`Product_Cost_$`) * s.Units)  as net_income
from sales s 
inner join products p on s.Product_ID = p.Product_ID 
group by 1
order by 2 desc 

-- Are there any seasonal trends or patterns in the sales data??

select
	month (`Date`) as month,
	sum(Units),
	sum( (p.`Product_Price_$` - p.`Product_Cost_$`) * s.Units)  as net_income
from sales s 
inner join products p on s.Product_ID = p.Product_ID 
group by 1
order by 1

-- Are products out of stock in some locations causing a decline in sales?

select
    s.`Date`,
    s.Product_ID,
    s.Store_ID,
    sum(s.Units) as total_units_sold,
    i.Stock_On_Hand,
    case
        when i.Stock_On_Hand = 0 then 'Out of Stock'
        else 'In Stock'
    end as stock_status
from sales s
inner join inventory i on s.Product_ID = i.Product_ID and s.Store_ID = i.Store_ID
group by 1, 2, 3, 5
order by 1, 2, 3
```
