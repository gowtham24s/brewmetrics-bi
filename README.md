# BrewMetrics BI

Power BI business intelligence solution for analyzing BrewMetrics Coffee Co. sales using a version-controlled workflow.

## Project Overview

BrewMetrics Coffee Co. operates Flagship stores, Kiosks, and Drive-Thrus across four cities. This project develops a Power BI business intelligence solution to help management explore sales performance, monthly trends, product performance, and city-level differences.

The project also demonstrates a version-controlled BI development workflow using Power BI Project (`.pbip`), GitHub, Visual Studio Code, and GitHub Copilot.

## Business Objectives

The dashboard is designed to help managers:

- Explore monthly sales trends.
- Analyze the seasonal sales pattern of Cold Brew.
- Compare sales performance across cities.
- Examine product rankings by sales.
- Explore performance from City down to Store Format.
- Monitor sales growth and transaction-level performance.

## Dataset

The project uses the provided `brewmetrics_sales.csv` transaction-level sales dataset.

The main fields include:

- `sale_id` — transaction identifier
- `date` — transaction date
- `city` — city where the sale occurred
- `store_format` — Flagship, Kiosk, or Drive-Thru
- `category` — broad product category
- `item` — specific product
- `quantity` — quantity sold
- `unit_price` — unit selling price
- `sales_amount` — transaction sales amount

## Data Model

The original flat CSV was transformed into a star schema using Power Query.

### Fact_Sales

`Fact_Sales` contains the transaction-level sales data used for analysis.

Key fields include:

- `sale_id`
- `date`
- `city`
- `store_format`
- `item`
- `category`
- `quantity`
- `unit_price`
- `sales_amount`

### Dim_Date

`Dim_Date` is the calendar dimension used for time-based analysis.

It contains:

- `Date`
- `Year`
- `Month Number`
- `Month`
- `Quarter`

`Dim_Date[Date]` is used as the marked date column for time-based analysis.

### Dim_City

`Dim_City` contains the cities used for geographic sales analysis.

### Dim_Product

`Dim_Product` contains product information including:

- `Item`
- `Category`
- `Unit Price`

### Dim_Store

`Dim_Store` contains the available store formats:

- Flagship
- Kiosk
- Drive-Thru

## Relationships

The semantic model follows a star-schema structure:

```text
Dim_Date
    |
    |
    v
Fact_Sales
^    ^    ^
|    |    |
|    |    +---- Dim_Store
|    +--------- Dim_Product
+-------------- Dim_City
```

The dimension tables are on the one side of one-to-many relationships with `Fact_Sales`.

## DAX Measures

The project includes the following DAX measures:

### Total Sales

Calculates total sales from the transaction table.

### MoM Growth %

Calculates month-over-month sales growth.

### Running Total Sales

Calculates cumulative sales over the selected date context.

### Item Rank

Ranks products by Total Sales using `RANKX`.

### Average Transaction Value

Calculates average sales value per distinct transaction.

### Cold Brew Sales %

Calculates Cold Brew sales as a percentage of total sales while preserving the relevant report context.

## Dashboard Features

The final dashboard includes:

- Total Sales KPI
- Average Transaction Value KPI
- Month-over-Month Growth KPI
- Cold Brew Sales Share KPI
- Monthly Sales Trend
- Cold Brew Seasonal Sales
- Sales Performance by City
- Top Product Ranking
- City slicer
- City → Store Format drill-down

## Key Insights

### 1. Cold Brew Seasonal Pattern

The dashboard highlights a stronger Cold Brew sales pattern during April and May, followed by a decline in June. This supports the seasonal behavior described in the BrewMetrics project scenario.

### 2. City-Level Performance Gap

The dashboard shows that Bengaluru consistently performs above the other three cities in sales performance, making the city comparison important for management analysis.

### 3. Product Performance

The product-ranking visual allows management to identify the highest-performing products by Total Sales and compare their relative positions using the Item Rank measure.

## Version-Controlled Workflow

The project was developed incrementally using GitHub and Power BI Project (`.pbip`) files.

The development history progresses through:

1. Initial project setup
2. Dataset addition
3. Star schema development
4. Power BI Project format conversion
5. DAX measure development
6. Copilot-assisted DAX documentation
7. Dashboard development
8. Final documentation

Each major development step was committed separately so that the evolution of the BI solution could be reviewed through Git history.

## Tools Used

- Power BI Desktop
- Power Query
- DAX
- Visual Studio Code
- GitHub
- GitHub Desktop
- GitHub Copilot

## Project Files

```text
brewmetrics-bi/
│
├── README.md
├── NOTES.md
├── brewmetrics_sales.csv
├── Power BI Project.pbip
├── Power BI Project.Report/
└── Power BI Project.SemanticModel/
```
