# Copilot DAX Development Notes

## Project: BrewMetrics Coffee Co.

This file documents how GitHub Copilot was used during the development of the DAX measures for the BrewMetrics Power BI mini project. Each suggestion was reviewed and tested in Power BI before being used.

---

## 1. Total Sales

### Copilot Suggestion

```DAX
Total Sales = SUM(Fact_Sales[sales_amount])
```

### Review

Copilot correctly identified `Fact_Sales[sales_amount]` as the sales value column and suggested a simple SUM measure.

### Correction

No correction was required.

### Final Measure

```DAX
Total Sales =
SUM(Fact_Sales[sales_amount])
```

### Testing

The measure was tested in Power BI using a Card visual and responded correctly to report filters such as City.

---

## 2. Month-over-Month Growth %

### Copilot Suggestion

```DAX
MoM Growth % =
VAR PreviousMonthSales =
    CALCULATE(
        [Total Sales],
        DATEADD(Dim_Date[Date], -1, MONTH)
    )
RETURN
    DIVIDE([Total Sales] - PreviousMonthSales, PreviousMonthSales)
```

### Review

Copilot correctly used the existing `Total Sales` measure and the `Dim_Date[Date]` column. The `DATEADD` function was used to retrieve the previous month's sales.

### Correction

No correction was required.

### Final Measure

```DAX
MoM Growth % =
VAR PreviousMonthSales =
    CALCULATE(
        [Total Sales],
        DATEADD(Dim_Date[Date], -1, MONTH)
    )
RETURN
    DIVIDE([Total Sales] - PreviousMonthSales, PreviousMonthSales)
```

### Testing

The measure was tested in a table using Month, Total Sales, and MoM Growth %. The first available month has no previous month in the analysis period, so its growth value is blank.

---

## 3. Running Total Sales

### Copilot Suggestion

```DAX
Running Total Sales =
VAR CurrentDate = MAX(Dim_Date[Date])
RETURN
    CALCULATE(
        [Total Sales],
        FILTER(
            ALL(Dim_Date[Date]),
            Dim_Date[Date] <= CurrentDate
        )
    )
```

### Review

Copilot correctly used the current date from `Dim_Date[Date]` and calculated cumulative sales up to that date.

### Correction

No correction was required.

### Final Measure

```DAX
Running Total Sales =
VAR CurrentDate = MAX(Dim_Date[Date])
RETURN
    CALCULATE(
        [Total Sales],
        FILTER(
            ALL(Dim_Date[Date]),
            Dim_Date[Date] <= CurrentDate
        )
    )
```

### Testing

The measure was tested using the date field in a visual. The cumulative value increased as the date progressed through the available data.

---

## 4. Item Rank

### Copilot Suggestion

```DAX
Item Rank =
IF(
    ISINSCOPE(Dim_Product[Item]),
    RANKX(
        ALLSELECTED(Dim_Product[Item]),
        [Total Sales],
        ,
        DESC,
        DENSE
    )
)
```

### Review

Copilot correctly used `RANKX` to rank products according to Total Sales. The use of `ALLSELECTED` allows the ranking to respond to the current report filter context.

### Correction

No correction was required.

### Final Measure

```DAX
Item Rank =
IF(
    ISINSCOPE(Dim_Product[Item]),
    RANKX(
        ALLSELECTED(Dim_Product[Item]),
        [Total Sales],
        ,
        DESC,
        DENSE
    )
)
```

### Testing

The measure was tested in a table containing Item, Total Sales, and Item Rank. The items were ranked from highest to lowest sales.

---

## 5. Average Transaction Value

### Copilot Suggestion

```DAX
Average Transaction Value =
DIVIDE(
    [Total Sales],
    DISTINCTCOUNT(Fact_Sales[sale_id])
)
```

### Review

Copilot correctly used the existing Total Sales measure and divided it by the distinct number of transactions from `Fact_Sales[sale_id]`.

### Correction

No correction was required.

### Final Measure

```DAX
Average Transaction Value =
DIVIDE(
    [Total Sales],
    DISTINCTCOUNT(Fact_Sales[sale_id])
)
```

### Testing

The measure was tested in Power BI and responded to filters such as City.

---

## 6. Cold Brew Sales %

### Copilot Suggestion

```DAX
Cold Brew Sales % =
DIVIDE(
    CALCULATE(
        [Total Sales],
        REMOVEFILTERS(Dim_Product[Category]),
        Dim_Product[Category] = "Cold Brew"
    ),
    CALCULATE(
        [Total Sales],
        REMOVEFILTERS(Dim_Product[Category])
    )
)
```

### Review and Correction

The initial Copilot suggestion needed correction.

The problem was that Copilot used:

`Dim_Product[Category] = "Cold Brew"`

In this project, `Category` represents the broader product category, while `Item` represents the specific product. Cold Brew is a specific item, not a category.

Therefore, the filter needed to use:

`Dim_Product[Item] = "Cold Brew"`

I corrected the measure to use the specific item column.

### Corrected Final Measure

```DAX
Cold Brew Sales % =
DIVIDE(
    CALCULATE(
        [Total Sales],
        REMOVEFILTERS(Dim_Product),
        Dim_Product[Item] = "Cold Brew"
    ),
    CALCULATE(
        [Total Sales],
        REMOVEFILTERS(Dim_Product)
    )
)
```

### Testing

The corrected measure was tested in Power BI with Month and City filters. The percentage changed according to the selected report context.

---

## Overall Copilot Reflection

GitHub Copilot was useful for generating the initial DAX structure for the measures. The suggestions for Total Sales, Month-over-Month Growth, Running Total Sales, Item Rank, and Average Transaction Value were directly usable after testing. The Cold Brew Sales % measure required a real correction because Copilot initially treated Cold Brew as a category instead of a specific item. This showed that Copilot can accelerate DAX development, but the generated expressions still need to be checked against the actual data model and business definitions before being accepted.