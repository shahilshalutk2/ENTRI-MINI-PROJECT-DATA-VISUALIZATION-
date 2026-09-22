# 🚗 Car Sales Performance & Customer Analytics Dashboard

An interactive, multi-page **Power BI Desktop** analytics report built on a real-world automotive dataset. This project delivers high-level business intelligence covering revenue metrics, brand and vehicle performance, dealer sales channels, and customer demographic segmentation.

**Author:** Shahil Shalu TK  
**Tooling:** Power BI Desktop, Power Query, DAX, Microsoft Excel  

---

## 📌 Project Overview

This Power BI solution transforms raw automotive sales records into an interactive 3-page application designed for executive leadership, dealership managers, and marketing analysts.

### Key Objectives:
1. Track overall financial KPIs (Revenue, Units Sold, Average Vehicle Price, Customer Income).
2. Evaluate sales performance across vehicle categories, body styles, transmission systems, and engine types.
3. Analyze dealership performance and customer demographics to optimize targeted sales strategies.

---

## 🧹 Data Cleaning & ETL Process (Power Query)

Before constructing the data model, raw data was cleaned and transformed using **Power Query**:

* **Table Selection & Identification:** Extracted structured tabular data from the Excel workbook (`Car Sales xlsx - car_data`).
* **Column Cleanup:** Removed unneeded/redundant fields (e.g., `Phone` column).
* **Null & Standard Handling:** Identified missing values in vehicle category attributes (`Column1`) and standardized them as proper text categories (`Base Model`, `Mid-Range`, `Luxury`).
* **Data Type Enforcement:** Explicitly set data types for all fields:
  * `Price ($)` and `Annual Income` -> Fixed Decimal Number (Currency)
  * `Date` -> Date Type
  * `Car_id` -> Text / String ID
  * Quantitative counts -> Whole Numbers

---

## 🗓️ Data Modeling & DAX Measures

### 1. Calendar Table (`Dim_Date`)
To support time-intelligence metrics, a dedicated calendar table was generated in DAX and linked via a 1-to-many relationship with the primary transaction table:

```dax
Dim_Date = 
CALENDAR(
    MIN('Car Sales xlsx - car_data'[Date]), 
    MAX('Car Sales xlsx - car_data'[Date])
)
#### 2. DAX MEASURES.
// 1. Total Revenue
Total Revenue = SUM('Car Sales xlsx - car_data'[Price ($)])

// 2. Total Units Sold
Total Units Sold = COUNTROWS('Car Sales xlsx - car_data')

// 3. Average Price
Average Price = AVERAGE('Car Sales xlsx - car_data'[Price ($)])

// 4. Average Customer Income
Average Customer Income = AVERAGE('Car Sales xlsx - car_data'[Annual Income])

// 5. Total Models Sold
Total Models Sold = DISTINCTCOUNT('Car Sales xlsx - car_data'[Model])

// 6. Top Revenue Brand
Top Revenue Brand = 
TOPN(
    1, 
    VALUES('Car Sales xlsx - car_data'[Company]), 
    [Total Revenue], 
    DESC
)

// 7. Top Dealer
Top Dealer = 
TOPN(
    1, 
    VALUES('Car Sales xlsx - car_data'[Dealer_Name]), 
    [Total Revenue], 
    DESC
)

// 8. Verified Customer %
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


📊 Dashboard Structure & Pages
Page 1: Executive Overview
KPI Cards: Total Revenue, Units Sold, Average Vehicle Price, Average Customer Income.

Monthly Revenue Trend: Line and Clustered Column chart displaying revenue trajectory over time.

Sales by Dealer Region: Horizontal Clustered Bar chart displaying regional performance.

Body Style Breakdown: Donut chart showcasing units sold across SUVs, Hatchbacks, Sedans, Passenger, and Hardtop models.

Global Slicers: Synchronized Date, Region, and Income Status filters across all pages.

Page 2: Brand & Vehicle Performance
Top Performers: KPI cards for Top Revenue Brand (Chevrolet), Total Models Sold (154), and Total Revenue ($371M).

Top 10 Car Companies: Horizontal Bar chart configured with a dynamic Top N visual filter ranking brands by gross revenue.

Vehicle Hierarchy Matrix: Drill-down table organized by Company -> Model -> Car Category.

Engine & Transmission Distribution: Interactive Treemap categorizing sales across Auto/Manual transmissions and Overhead Camshaft engine builds.

Color Preference Breakdown: Donut chart ranking customer color preferences.

Page 3: Customer & Dealer Insights
Key Dealership Metrics: Top Dealer performance card (Rabun Used Car Sales) and Income Verification Ratio (77.82%).

Dealer Performance: Stacked Bar chart breaking down revenue per dealership segmented by verified vs. unverified customer income.

Income vs. Price Distribution: Scatter Chart mapping average annual income against average purchase price across automotive manufacturers.

Demographic Analysis: Clustered Column chart displaying units sold across vehicle body styles segmented by customer gender.

Transaction Table: Detailed transaction records including Car_id, Date, Customer Name, Company, Model, Price ($), and Dealer_Region.

🎮 Interactivity & Navigation
Page Navigator: Seamless application-style navigation menu built into the top ribbon across all 3 pages.

Sync Slicers: Interactive slicers configured to sync filter state universally across every report page.

Cross-Filtering: Visuals react dynamically to user selection for deeper drill-down analysis.
📁 Repository Structure
├── Data/
│   └── Data cleanin Mini Project First step1.xlsx    # Raw and preprocessed dataset
├── Reports/
│   └── Car_Sales_Analysis_Report.pbix               # Power BI Desktop report file
├── assets/
│   ├── page1_overview.png                            # Screenshot of Executive Overview
│   ├── page2_brand_performance.png                   # Screenshot of Brand Performance
│   └── page3_customer_insights.png                  # Screenshot of Customer Insights
└── README.md                                         # Documentation
🚀 How to Run the Report
1.Clone or download this repository to your local machine
2.Download and install Power BI Desktop.
3.Open PoweBI Entri Mini Project Final Step.pbix inside Power BI Desktop.
4.Use Ctrl + Click on the top Page Navigator buttons to seamlessly switch between pages within Desktop view.
