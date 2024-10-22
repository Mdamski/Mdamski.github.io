# Interactive Sales Dashboard (2014-2017) - Data Analysis Project
## Tools & Skills
### Tools Used: Power Query, Power BI, DAX
### Skills Demonstrated: Data Transformation, Data Cleaning, Data Modeling, Interactive Visualization
## Project Overview
This project focuses on the creation of an interactive sales dashboard that allows users to slice data by year (2014–2017) and by key metrics: Sales, Profit, and Quantity. The dashboard provides a dynamic view of performance over time, enabling comparisons of current year-to-date (YTD) metrics with prior year-to-date (PYTD) performance. This analysis is particularly useful for businesses looking to assess their growth and make data-driven decisions.

## Dataset
The dataset contains the following columns:

- Row ID: Sequential identifier for each row. (Numeric)
- Order ID: Unique identifier for each order. (Text)
- Order Date: Date when the order was placed. (Date)
- Ship Date: Date when the order was shipped. (Date)
- Ship Mode: The shipping mode used for the order (e.g., Standard Class, Second Class). (Text)
- Customer ID: Unique identifier for each customer. (Text)
- Customer Name: Name of the customer. (Text)
- Segment: Customer segment (e.g., Consumer, Corporate). (Text)
- Country: Country where the customer is located. (Text)
- City: City where the customer is located. (Text)
- Postal Code: Postal code of the customer’s location. (Numeric)
- Region: Geographical region. (Text)
- Product ID: Unique identifier for each product. (Text)
- Category: Product category (e.g., Furniture, Technology). (Text)
- Sub-Category: More specific classification within the product category (e.g., Bookcases). (Text)
- Product Name: Name of the product. (Text)
- Sales: Total sales amount for the order. (Numeric)
- Quantity: Number of units sold. (Numeric)
- Discount: Discount applied to the order. (Numeric, Percentage)
- Profit: Profit generated from the order. (Numeric)

## Data Cleaning and Preparation
Before performing the analysis, several data cleaning steps were carried out to ensure the integrity and usability of the data:

- Checking for duplicates: The dataset was scanned for duplicate entries to prevent double-counting of orders and to maintain accuracy in the sales and profit calculations. Duplicates, if found, were removed.
- Adjusting data types: Data types were carefully reviewed and adjusted where necessary. For example:
Date columns such as 'Order Date' and 'Ship Date' were formatted as date fields.
Numeric columns like 'Sales,' 'Quantity,' 'Profit,' and 'Postal Code' were set to appropriate numeric formats to allow for proper aggregations and calculations.

## Data Model
Two additional tables were created to enhance the functionality of the dashboard:

- Slc_Values: A table with one column containing three rows: 'Sales,' 'Profit,' and 'Quantity.' This table powers the slicer functionality, allowing users to switch between different metrics for analysis.
- Dim_Date: A date dimension table created using the CALENDAR function in DAX, containing dates from January 1, 2014, to December 31, 2017. This table is related to the Fact_Sales table on the 'Order Date' column in a one-to-many relationship.

## DAX
The following DAX measures were used to calculate key metrics:

Total Sales - Calculates the total sales amount.
 ```
Sales = SUM(Fact_sales[Sales])
```
Total Profit - Calculates the total profit amount.
```
Profit = SUM(Fact_sales[Profit])
```
Total Quantity - Calculates the total quantity of items sold.
```
Quantity = SUM(Fact_sales[Quantity])
```
Profit Margin - Calculates the profit margin as profit divided by sales.
```
Profit Margin = DIVIDE([Profit], [Sales])
```
Year-to-Date (YTD) Sales - Calculates the year-to-date (YTD) sales based on the 'Order Date.'
```
YTD_Sales = TOTALYTD([Sales], Fact_sales[Order Date])
```
Year-to-Date (YTD) Profit - Calculates the YTD profit based on the 'Order Date.'
```
YTD_Profit = TOTALYTD([Profit], Fact_sales[Order Date])
```
Year-to-Date (YTD) Quantity - Calculates the YTD quantity of items sold.
```
YTD_Quantity = TOTALYTD([Quantity], Fact_sales[Order Date])
```
Prior Year-to-Date (PYTD) Sales - Calculates the prior year-to-date PYTD sales.
```
PYTD_Sales = 
CALCULATE(
    [Sales],
    SAMEPERIODLASTYEAR(Dim_Date[Date]),
    Dim_Date[Inpast] = TRUE
)
```
Prior Year-to-Date (PYTD) Profit - Calculates the PYTD profit.
```
PYTD_Profit = 
CALCULATE(
    [Profit],
    SAMEPERIODLASTYEAR(Dim_Date[Date]),
    Dim_Date[Inpast] = TRUE
)
```
Prior Year-to-Date (PYTD) Quantity - Calculates the PYTD quantity of items sold.
```
PYTD_Quantity = 
CALCULATE(
    [Quantity],
    SAMEPERIODLASTYEAR(Dim_Date[Date]),
    Dim_Date[Inpast] = TRUE
)
```
Year-to-Date vs Prior Year-to-Date (YTD vs PYTD) - Calculates the difference between YTD and PYTD for the selected metric.
```
YTD vs PYTD = [S_YTD] - [S_PYTD]
```
Dynamic YTD Calculation - Dynamically calculates YTD (Sales, Profit, or Quantity) based on slicer selection.
```
S_YTD = 
VAR selected_value = SELECTEDVALUE(Slc_Values[Values])
VAR result = SWITCH(
    selected_value,
    "Sales", [YTD_Sales],
    "Quantity", [YTD_Quantity],
    "Profit", [YTD_Profit],
    BLANK()
)
RETURN
result
```
Dynamic PYTD Calculation - Dynamically calculates PYTD (Sales, Profit, or Quantity) based on slicer selection.
```
S_PYTD = 
VAR selected_value = SELECTEDVALUE(Slc_Values[Values])
VAR result = SWITCH(
    selected_value,
    "Sales", [PYTD_Sales],
    "Quantity", [PYTD_Quantity],
    "Profit", [PYTD_Profit],
    BLANK()
)
RETURN
result
```
## Model Relationships
A 1-to-many relationship was established between the Dim_Date table (1) and the Fact_sales table (many) using the 'Order Date' field. This relationship enables accurate time-based calculations for year-over-year analysis.

## Dashboard & Insights

![image](https://github.com/user-attachments/assets/e370c0e4-01f0-4e61-af39-aac397c9f366)
- State-Level Profit Growth: Indiana and California show notable growth in profitability YTD compared to PYTD.
- Profit by Ship Mode: Standard Class and Second Class delivery methods contributed the most to the overall profit growth YTD.
- Profit by Segment and Region: The Corporate and Consumer segments in the Central region experienced significant YTD profit growth, while the Home Office segment's growth remained moderate across most regions.

![image](https://github.com/user-attachments/assets/58bb4848-30de-4536-9fd6-d28a80007174)
- Quantity by State: Growth in quantity sold YTD outpaced PYTD in key regions like California and Texas.
- Monthly Performance: Quantity sold shows consistent growth YTD compared to PYTD, particularly in mid-year (May) and the last quarter.
- Quantity by Ship Mode: Similar to profit growth, Standard Class and First Class deliveries contributed significantly to quantity growth YTD.

![image](https://github.com/user-attachments/assets/aaf2f457-2e84-414d-8420-62a40d20c6de)

- Sales by the State: The biggest growth is noted in California and Indiana.
- Mothly Performance: Sales increased steadily throughout the year, with notable growth in mid-year (May) and the last quarter (October and December). A slight dip was observed in August YTD vs PYTD.
- Sales by Segment and Region: Corporate contributed to strong sales growth YTD compared to PYTD.

## Conclusion:
This analysis highlights substantial YTD growth across multiple metrics (profit, quantity, sales) compared to PYTD. Indiana and California are standout states for profit growth, while mid-year and year-end spikes in sales and quantity suggest seasonal shopping patterns. The steady performance of Standard and Second Class shipping methods also points to a consistent delivery preference linked to increased profitability and sales volume. 
