# Fashion Store Sales Dashboard (Power BI)

An interactive Power BI dashboard that analyses sales, estimated profit, quantity sold, category performance, brand-wise sales, seasonal performance and monthly sales trends for a fashion boutique.

**Experiment 3:** Build an Interactive Dashboard to Support Business Decisions using Power BI
**Course Outcome:** CO4 | **Bloom's Level:** Creating | **Tool:** Power BI Desktop

---

## Problem Statement

Build an interactive Fashion Store Dashboard to analyze sales, profit, quantity sold, clothing-category performance, brand-wise sales, regional performance, and monthly sales trends.

## Dataset

**Retail Fashion Boutique Data Sales Analytics 2025** (Kaggle, by pratyushpuri)

Link: https://www.kaggle.com/datasets/pratyushpuri/retail-fashion-boutique-data-sales-analytics-2025

- Synthetic retail data with 2,176 records
- 8 brands and 6 categories (Dresses, Tops, Bottoms, Outerwear, Shoes, Accessories)
- 4 seasons of data, with pricing, markdown and return information

**Original columns:** `product_id`, `category`, `brand`, `season`, `size`, `color`, `original_price`, `markdown_percentage`, `current_price`, `purchase_date`, `stock_quantity`, `customer_rating`, `is_returned`, `return_reason`

> Please check the dataset page on Kaggle for its license and terms of use. The data is not included in this repository.

---

## Data Preparation (Power Query)

### Cleaning
- Replaced empty `size` values with `N/A`
- Replaced empty `return_reason` values with `Not Returned`
- Set correct data types (date, decimal, whole number, true/false)

### Columns added

| Column | Formula | Purpose |
|---|---|---|
| `Quantity` | `1` | Each row is one sale (the dataset has no quantity-sold column) |
| `Sales` | `[current_price]` | Sales amount |
| `Discount Given` | `[original_price] - [current_price]` | Revenue given away through markdowns |
| `Estimated Profit` | `[current_price] - (0.5 * [original_price])` | Estimated profit (see assumptions) |
| `Returned Count` | `if [is_returned] = true then 1 else 0` | Used for return rate |
| `Year` | From `purchase_date` | Date analysis |
| `Month Number` | From `purchase_date` | Sorting months in order |
| `Month Name` | From `purchase_date` | Monthly trend axis |

`Month Name` is sorted by `Month Number` so months appear in calendar order.

### DAX measure

```DAX
Return Rate = DIVIDE(SUM(Fashion[Returned Count]), SUM(Fashion[Quantity]))
```

---

## Dashboard Contents

| Visual | Type | Fields |
|---|---|---|
| Headline metrics | Card (x4) | Sales, Quantity, Estimated Profit, Return Rate |
| Category performance | Clustered column chart | Category by Sales (drill-down: Category to Brand) |
| Brand-wise sales | Clustered bar chart | Brand by Sales |
| Seasonal performance | Donut chart | Season by Sales |
| Monthly sales trend | Line chart | Month Name by Sales |
| Filters | Slicers | Season, Brand, Purchase Date (Between) |
| Decision note | Text box | States the business decision supported |

### Interactivity
- **Slicers** filter every visual on the page
- **Cross-filtering:** selecting a bar or slice highlights related data in other visuals
- **Drill-down:** the category chart drills from Category to Brand

---

## Assumptions and Substitutions

The dataset lacks some fields requested in the problem statement, so these substitutions were made:

| Requirement | Limitation in the dataset | What was used |
|---|---|---|
| Profit | No cost or profit column | **Estimated Profit**, assuming cost = 50% of original price. This is an assumption, not real profit data |
| Quantity sold | Only `stock_quantity` (inventory) | `Quantity = 1` per transaction row |
| Regional performance | No region or location column | **Season** used instead |
| Sub-Category drill-down | No sub-category column | **Brand** used as the second drill-down level |

---

## Business Decision Supported

This dashboard helps managers decide which categories, brands and seasons to prioritise for stock, marketing and discounts, using sales, profit and return trends.

---


## Screenshots

<img width="756" height="422" alt="image" src="https://github.com/user-attachments/assets/d974bf88-0558-4e1e-a4d8-1577a6eab5b6" />

---

## How to Run

1. Download the dataset from the Kaggle link above.
2. Clone or download this repository.
3. Open `Fashion_Store_Dashboard.pbix` in **Power BI Desktop**.
4. If prompted, update the data source: **Home → Transform data → Data source settings → Change Source**, and point it to your downloaded CSV.
5. Click **Refresh**.

---


## Conclusion

Slicers, cross-filtering and drill-down turn a static report into an interactive decision-support tool. Managers can explore fashion store performance by brand, category, season and time without needing a separate report for each question.

---
