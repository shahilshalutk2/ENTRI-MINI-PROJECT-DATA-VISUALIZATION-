# 🚗 Car Sales Analytics & Performance Dashboard

A comprehensive, interactive 3-page Power BI dashboard designed to evaluate sales metrics, brand performance, customer profiles, and dealership operations for a car sales dataset.

**Author:** Shahil Shalu TK

**Tool Used:** Power BI Desktop, Power Query, DAX

## 📑 Table of Contents

* [Executive Overview](#-executive-overview)

* [Data Cleaning & Transformation](#-data-cleaning--transformation)

* [Data Model & Relationships](#-data-model--relationships)

* [DAX Measures](#-dax-measures)

* [Dashboard Architecture](#-dashboard-architecture)

  * [Page 1: Executive Overview](#page-1-executive-overview)

  * [Page 2: Brand & Vehicle Performance](#page-2-brand--vehicle-performance)

  * [Page 3: Customer & Dealer Insights](#page-3-customer--dealer-insights)

* [Key Business Insights](#-key-business-insights)

* [How to View the Report](#-how-to-view-the-report)

## 📊 Executive Overview

This Power BI project converts raw transactional car sales data into interactive, actionable business intelligence. The report provides granular insight into total sales revenue, units sold, top-performing brands, customer demographics (income status and gender distribution), and dealer-level revenue breakdown.

## 🧹 Data Cleaning & Transformation

Data cleaning and preprocessing were performed using **Power Query Editor** before modeling:

1. **Table Selection & Structuring**:

   * Isolated and loaded the structured data table (`Car Sales xlsx - car_data`) from the Excel workbook.

2. **Column Removal**:

   * Removed non-analytical fields such as customer `Phone` numbers to optimize performance and privacy.

3. **Handling Missing & Null Values**:

   * Handled null/blank values in `Car Category` by categorizing ambiguous entries and standardizing category names (`Base Model`, `Mid-Range`, `Luxury`).

4. **Data Type Formatting**:

   * Converted `Date` column to proper `Date` data type (`DD/MM/YYYY`).

   * Converted numeric metrics (`Price ($)`, `Annual Income`) to `Fixed Decimal Number` / `Currency`.

   * Set standard text data types for categorical columns (`Company`, `Model`, `Engine`, `Transmission`, `Dealer_Region`, `Gender`).

5. **Text Standardizing**:

   * Capitalized text fields and trimmed trailing spaces across string fields for smooth visual grouping.

## 🔗 Data Model & Relationships

The report utilizes a **Star Schema** data architecture:

* **Fact Table**: `Car Sales xlsx - car_data` (Contains transactional sales records)

* **Dimension Table**: `Dim_Date` (Dedicated calendar table created using DAX)

* **Relationship**:

  * `Dim_Date[Date]` `1` $\rightarrow$ `*` `Car Sales xlsx - car_data[Date]` (Single Directional, 1-to-Many).

```
   +------------------+             +-------------------------------+
   |     Dim_Date     | 1         * |  Car Sales xlsx - car_data    |
   +------------------+-------------+-------------------------------+
   | Date (PK)        |             | Car_id (PK)                   |
   | Year             |             | Date (FK)                     |
   | Month            |             | Company                       |
   | Year Month       |             | Model                         |
   | Quarter          |             | Price ($)                     |
   +------------------+             | Dealer_Region                 |
                                    +-------------------------------+

```

## 📐 DAX Measures

All measures were organized into a dedicated `_Measures` table:

```
// 1. Total Sales Revenue
Total Revenue = SUM('Car Sales xlsx - car_data'[Price ($)])

// 2. Total Volume Sold
Total Units Sold = COUNTROWS('Car Sales xlsx - car_data')

// 3. Average Price per Vehicle
Average Price = AVERAGE('Car Sales xlsx - car_data'[Price ($)])

// 4. Average Customer Income
Average Customer Income = AVERAGE('Car Sales xlsx - car_data'[Annual Income])

// 5. Top Revenue Generating Brand
Top Revenue Brand = 
TOPN(
    1, 
    VALUES('Car Sales xlsx - car_data'[Company]), 
    [Total Revenue], 
    DESC
)

// 6. Total Distinct Vehicle Models
Total Models Sold = DISTINCTCOUNT('Car Sales xlsx - car_data'[Model])

// 7. Top Performing Dealer
Top Dealer = 
TOPN(
    1, 
    VALUES('Car Sales xlsx - car_data'[Dealer_Name]), 
    [Total Revenue], 
    DESC
)

// 8. Verified Customer Percentage (Ignoring Slicer Context Filters)
Verified Customer % = 
DIVIDE(
    CALCULATE(
        COUNTROWS('Car Sales xlsx - car_data'), 
        'Car Sales xlsx - car_data'[Income_Status] = "Verified",
        REMOVEFILTERS('Car Sales xlsx - car_data'[Income_Status])
    ),
    CALCULATE(
        COUNTROWS('Car Sales xlsx - car_data'),
        REMOVEFILTERS('Car Sales xlsx - car_data'[Income_Status])
    ),
    0
)

```

## 🖥️ Dashboard Architecture

### Page 1: Executive Overview

Focused on high-level financial health and sales macro trends.

* **KPI Header**: Total Revenue (`$371.2M`), Total Units Sold (`13K`), Average Vehicle Price (`$27.99K`), Average Customer Income (`$810.58K`).

* **Monthly Revenue & Sales Trend**: Combo Line & Clustered Column chart (`Total Revenue` and `Total Units Sold` by `Year Month`).

* **Regional Performance**: Horizontal Clustered Bar chart displaying revenue by `Dealer_Region`.

* **Body Style Revenue Split**: Donut chart showing revenue proportions by `Body Style` (SUV, Hatchback, Sedan, Passenger, Hardtop).

* **Interactive Top Filter Bar**: Synchronized slicers for `Date Range`, `Dealer Region`, and `Income Status`.

### Page 2: Brand & Vehicle Performance

Focused on manufacturer rankings, vehicle attributes, and mechanical breakdown.

* **KPI Header Cards**: Top Revenue Brand (`Chevrolet`), Total Distinct Models (`154`), Total Revenue.

* **Top 10 Car Brands by Revenue**: Clustered Bar chart with Top 10 DAX filter applied on `Company`.

* **Vehicle Hierarchy Breakdown**: Multi-level Matrix Table (`Company` $\rightarrow$ `Model` $\rightarrow$ `Car Category`) showing Units, Revenue, and Avg Price.

* **Engine & Transmission Distribution**: Treemap displaying revenue split across `Transmission` (Auto vs. Manual) and `Engine` specifications.

* **Vehicle Color Preferences**: Donut chart displaying volume sold by vehicle `Color`.

### Page 3: Customer & Dealer Insights

Focused on customer demographic breakdown and dealership sales contributions.

* **KPI Header Cards**: Top Performing Dealership (`Rabun Used Car Sales`), Verified Customer Ratio (`77.82%`).

* **Dealer Revenue by Income Status**: Stacked Bar chart detailing revenue per dealer split by `Verified` vs. `Unverified` customer status.

* **Income vs. Price Distribution**: Scatter Plot measuring relationship between customer `Annual Income` and vehicle `Price ($)` segmented by brand/category.

* **Volume Sold by Body Style & Gender**: Clustered Column chart showing volume preference across `Male` vs. `Female` buyers.

* **Customer Transaction Records**: Full granular data table detailing `Car_id`, `Date`, `Customer Name`, `Company`, `Model`, `Price ($)`, and `Dealer Region`.

* **Page Navigation**: Seamless inter-page navigation bar embedded across all three pages.

## 💡 Key Business Insights

1. **Top Revenue Contributor**: **Chevrolet** leads overall brand revenue, followed closely by **Ford** and **Dodge**.

2. **Body Style Preference**: **SUVs** and **Hatchbacks** generate over 48% of total vehicle sales revenue.

3. **Customer Verification**: **\~77.8%** of total transactions are completed by income-verified customers, indicating low credit risk across dealerships.

4. **Regional Market Leaders**: **Austin** and **Janesville** are the top-performing dealer regions by total gross revenue.

## 🛠️ How to View the Report

1. Clone or download this repository:

   ```
   https://github.com/shahilshalutk2/ENTRI-MINI-PROJECT-DATA-VISUALIZATION-/blob/main/PoweBI%20Entri%20Mini%20Project%20Final%20Step.pbix
   
   ```

2. Open the `.pbix` file using **Power BI Desktop** (Free Download).

3. To navigate pages in Power BI Desktop, hold down `Ctrl` + Click on the **Page Navigator** buttons at the top of the canvas.