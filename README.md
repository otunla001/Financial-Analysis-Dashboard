# Financial Analysis Dashboard – Power BI

# Data Portfolio: Excel – Power BI







# Table of contents 

- [Objective](#objective)
- [Data Source](#data-source)
- [Design](#design)
 - [Tools](#tools)
- [Development](#development)
   - [Data Exploration](#data-exploration)
  - [Data Cleaning](#data-cleaning)
  - [Transform the Data](#transform-the-data)
  - [Create the SQL View](#create-the-sql-view)
- [Visualization](#visualization)
- [Conclusion](#conclusion)




# Objective 

- What is the key Main point? 

The Finance Department wants to evaluate the company’s financial performance over time — across segments, products, and territories — to identify high-performing areas and opportunities for improvement.


- What is the ideal solution? 

To create a dashboard that provides insights into the Pizzza Sales that includes their 
- Profit by Product
- Profit Margin by Product
- Month Sales Trend
- Year-over-Year (YoY) Sales and Profit Comparison
- Discount Trends
- Performance by Manager and Territory


This will help the marketing team make informed decisions about which Pizza size, catgory that makes the highest and also lowest sales.
## User story 

As a Finance Analyst, I want to use the Financial Performance Dashboard to, 

- Identify top-performing products, segments, and time periods.
- Evaluate key financial metrics like Sales, Profit, and Discount.
- Track YoY performance for better strategic decision-making.


## 📚 Data Source

- **Source**:Kaggle (an Excel extract)
- **Date range**: September 2021 – December 2022

**Tables used**:
- `Sales` table  
- `Manager` table  
- `Date` table (manually created for DAX time intelligence)

---

## ✏️ Design

Dashboard components include:
- KPI Cards for **Sales**, **Profit**, **Discount**
- **Line Charts** for YoY Sales and Profit trends
- **Bar Charts** for Product, Segment, Month comparisons
- **Profit Margin** visualized by Product/Country

---

## 🛠 Tools Used

| Tool        | Purpose                          |
|-------------|----------------------------------|
| Power BI    | Dashboard development            |
| Excel       | Source data & structure          |
| Power Query | Data cleaning & transformation   |
| DAX         | KPI creation & time intelligence |
| GitHub      | Project hosting                  |



# Development

## Pseudocode

- What's the general approach in creating this solution from start to finish?

1. Get the data
2. Explore the data in Excel
3. Load the data into Power Query
4. Clean the data with Power Query
5. Load the data into power BI
6. Visualize the data in Power BI
7. Generate the findings based on the insights
8. Write the documentation + commentary
9. Publish the data to GitHub Pages

## Data exploration notes

This is the stage where you have a scan of what's in the data, errors, inconcsistencies, bugs, weird and corrupted characters etc  


- What are your initial observations with this dataset? What's caught your attention so far? 

1. There are at least 17 columns that contain the Sale table while 6 in the manager table.  
2. We have more data than we need.




## Data cleaning 
- What do we expect the clean data to look like? (What should it contain? What contraints should we apply to it?)

The aim is to refine our dataset to ensure it is structured and ready for analysis. 

The cleaned data should meet the following criteria and constraints:

- Only relevant columns should be retained.
- All data types should be appropriate for the contents of each column.
- No column should contain null values, indicating complete data for all records.

Below is a table outlining the constraints on our cleaned dataset:

| Property | Description |
| --- | --- |
| Table 1 | Sale |
| Number of Rows | 701 |
| Number of Columns | 17 |
| Table 2 | Managers |
| Number of Rows | 26 |
| Number of Columns | 6 |



- What steps are needed to clean and shape the data into the desired format?

1. Remove unnecessary columns by only selecting the ones you need
2. Ensured correct data types (dates, currency)
3. Create a date table using dax 
4. Connected tables using 1-to-many relationships



### 🔄 Data Modeling
**Relationships:**
- `Sales[Manager ID]` → `Manager[Manager ID]`
- `Sales[Date]` → `Date[Date]`

**Schema:**
- **Fact Table**: Sales  
- **Dimension Tables**: Manager, Date  
- **Model**: Star schema for optimal DAX performance





### Transform the data 





## Analyze key Indicators 
### DAX 
```DAX
-- 🔸 Sales Amount
Sales Amount = SUM(Sales[Sales])

-- 🔸 Sales Amount Last Year
Sale Amount LY = IF(ISBLANK(CALCULATE([Sale Amount],DATEADD('Date'[Date],-1,YEAR))),0,CALCULATE([Sale Amount],DATEADD('Date'[Date],-1,YEAR)))

-- 🔸 Profit
Profit = SUM(Sales[Profit])

-- 🔸 Profit Last Year
Profit LY = IF(ISBLANK(CALCULATE([Profit],DATEADD('Date'[Date],-1,YEAR))),0,CALCULATE([Profit],DATEADD('Date'[Date],-1,YEAR)))
-- 🔸 Discount
Discount = SUM(Sales[Discounts])

-- 🔸 Discount Last Year
Discount LY = IF(ISBLANK(CALCULATE([Discount],DATEADD('Date'[Date],-1,YEAR))),0,CALCULATE([Discount],DATEADD('Date'[Date],-1,YEAR)))
-- 🔸 Profit Margin
Profit Margin = DIVIDE([Profit], [Sales Amount], 0)

-- 🔸 Profit Margin Last Year
Profit Margin LY = IF(ISBLANK(CALCULATE([Profit Margin],DATEADD('Date'[Date],-1,YEAR))),0,CALCULATE([Profit Margin],DATEADD('Date'[Date],-1,YEAR)))
-- 🔸 orders
Orders = SUM(Sales[Units Sold])

-- 🔸 Order Last Year
Orders LY = IF(ISBLANK(CALCULATE([Orders],DATEADD('Date'[Date],-1,YEAR))),0,CALCULATE([Orders],DATEADD('Date'[Date],-1,YEAR)))

-- 🔸  Date = 
ADDCOLUMNS(
    CALENDARAUTO(),
    "YEAR", YEAR([Date]),
    "Month", FORMAT([Date],"mmm"),
    "WeekType", IF(WEEKDAY([Date])=1,"Weekend", IF(WEEKDAY([Date])=7,"Weekend","Weekday")),
    "Weekday",FORMAT([Date],"ddd"),
    "MonthNum",MONTH([Date])
)

```

# Visualization 


## Results

- What does the dashboard look like?

![Excel Dashboard](Document/Pizza%20D%203.png)
 




# Analysis 

## Findings

- What did we find?
- Top Segment by Sales:
  - Government
- Most Profitable Product:
  - Paseo
- Highest Profit Margin Product:
  - Amarilla
- Date Range:
  - September 2021 – December 2022
- Top Sales Months
  - October and December
- YoY Profit Margin Trend
  - Dropped by -3.97% in 2022 compared to 2021
 
👤 Author
Abdulrahman Otunla
📧 otunlaabdulrahman@gmail.com
🌐 Portfolio: https://otunla.vercel.app/
