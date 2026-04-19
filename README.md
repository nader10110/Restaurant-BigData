# 🍽️ Restaurant Sales Dashboard — Big Data Pipeline & Power BI Analytics

<div align="center">

![Dashboard Preview](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Databricks](https://img.shields.io/badge/Databricks-Data%20Processing-FF3621?style=for-the-badge&logo=databricks&logoColor=white)
![Fivetran](https://img.shields.io/badge/Fivetran-ETL%20Pipeline-0073E6?style=for-the-badge&logo=fivetran&logoColor=white)
![Microsoft Fabric](https://img.shields.io/badge/Microsoft%20Fabric-Cloud%20Deploy-742774?style=for-the-badge&logo=microsoft&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-Data%20Engineering-4479A1?style=for-the-badge&logo=postgresql&logoColor=white)

**Transforming 11 Million rows of restaurant data into actionable business intelligence — in under 4 minutes.**

[![🚀 Live Dashboard](https://img.shields.io/badge/🚀%20Live%20Dashboard-View%20on%20Microsoft%20Fabric-742774?style=for-the-badge&logo=microsoft&logoColor=white)](https://app.fabric.microsoft.com/view?r=eyJrIjoiMGQ0YmUyMmQtMGViNS00ZWI3LWIzMGUtN2U0MzA0NjdlZGIyIiwidCI6IjJiYjZlNWJjLWMxMDktNDdmYi05NDMzLWMxYzZmNGZhMzNmZiIsImMiOjl9)

[📊 View Dashboard](#dashboard-pages) · [🏗️ Architecture](#architecture) · [📐 Data Model](#data-model) · [📈 Key Insights](#key-business-insights)

</div>

---

## 📌 Project Overview

This project presents a **full end-to-end Big Data pipeline and interactive Power BI dashboard** built for a multi-branch restaurant chain operating across 6 Egyptian cities. The pipeline ingests ~**11 million rows** distributed across **9 files in mixed formats (CSV & JSON)**, processes them in the cloud, and delivers a 4-page analytical dashboard that drives real business decisions.

> *"Most people see a dashboard as charts and colors. The truth is — when you build the right data pipeline, you're building the compass that guides business investment toward the right decision."*

---

## 🚀 Live Dashboard

> **The dashboard is publicly deployed on Microsoft Fabric — no login required.**

<div align="center">

### 👇 Click to open the interactive dashboard

[![Live Dashboard](https://img.shields.io/badge/▶%20OPEN%20LIVE%20DASHBOARD-Microsoft%20Fabric-742774?style=for-the-badge&logo=microsoft&logoColor=white&labelColor=1a1a2e)](https://app.fabric.microsoft.com/view?r=eyJrIjoiMGQ0YmUyMmQtMGViNS00ZWI3LWIzMGUtN2U0MzA0NjdlZGIyIiwidCI6IjJiYjZlNWJjLWMxMDktNDdmYi05NDMzLWMxYzZmNGZhMzNmZiIsImMiOjl9)

🔗 `https://app.fabric.microsoft.com/view?r=eyJrIjoiMGQ0YmUyMmQtMGViNS00ZWI3LWIzMGUtN2U0MzA0NjdlZGIyIiwidCI6IjJiYjZlNWJjLWMxMDktNDdmYi05NDMzLWMxYzZmNGZhMzNmZiIsImMiOjl9`

</div>

| Page | What You'll See |
|------|----------------|
| **Executive Overview** | Revenue 2.90bn EGP · 11M Orders · YoY +19.9% · MoM Growth Chart |
| **Menu Performance** | Top item كباب · Scatter analysis · Category Treemap |
| **Branch Analysis** | Cairo leads · Tanta most efficient · Channel mix per branch |
| **Customer & Quality** | Peak Hour 8PM · Weekday vs Weekend demand · Top 10 loyal customers |

> 💡 **Tip:** Use the filter panel on the left side of the dashboard to slice by year, branch, or category — all 4 pages update simultaneously.

---

## 🎯 Business Problem

A restaurant chain with **6 branches** across Egypt needed a unified view of performance across:

- **Revenue & growth trends** — Is the business growing and where?
- **Menu performance** — Which items drive the most revenue and value?
- **Branch efficiency** — Which locations are smart, not just busy?
- **Customer behavior** — When do customers come, how loyal are they, and what's the service quality?

**The challenge:** ~11M rows, 9 source files, mixed formats, no unified structure.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     DATA PIPELINE FLOW                           │
└─────────────────────────────────────────────────────────────────┘

  Source Files          Ingestion          Processing         Serving
  ─────────────         ─────────          ──────────         ───────
  
  📄 CSV × 7    ──┐
                  │──→  🔄 Fivetran  ──→  ⚡ Databricks  ──→  📊 Power BI
  📋 JSON × 2  ──┘       (< 4 min)        (SQL + Lake)      (DirectQuery)
                                                │
                                                ▼
                                      ☁️ Microsoft Fabric
                                       (Cloud Deployment)
```

### Why This Stack?

| Tool | Role | Why |
|------|------|-----|
| **Fivetran** | Data Ingestion / ETL | Automated pipeline that loaded all 9 files in **< 4 minutes** — no manual ETL coding |
| **Databricks** | Data Processing | Unified platform for Analysts, Engineers & Scientists. Used SQL to merge all files into one clean table |
| **Power BI** | Visualization | DirectQuery mode connected to Databricks for live, scalable reporting |
| **Microsoft Fabric** | Cloud Deployment | Ensures scalability, high performance, and enterprise-grade cloud infrastructure |

> **DirectQuery mode** was chosen deliberately — it keeps the report lightweight while querying the full 11M row dataset live from Databricks without importing data into Power BI.

---

## 📐 Data Model

```
┌──────────────────────────────┐         ┌─────────────────────────────┐
│       restaurant_final        │         │          Date_Table          │
├──────────────────────────────┤         ├─────────────────────────────┤
│ _fivetran_synced             │         │ Date (PK)                   │
│ _line                        │         │ Day_NUM                     │
│ branch                       │         │ Is_Weekend                  │
│ category                     │  *───1  │ Month                       │
│ customer_id                  │         │ Month_Name                  │
│ discount                     │         │ Month_Num                   │
│ hour                         │         │ Month_Year                  │
│ is_weekend                   │         │ Quarter                     │
│ item_name                    │         │ Weekday                     │
│ order_date ──────────────────┼────────→│ Year                        │
│ order_id                     │         └─────────────────────────────┘
│ order_type                   │
│ payment_method               │
│ price                        │
│ quantity                     │
│ rating                       │
│ total_amount                 │
└──────────────────────────────┘
```

**Relationship:** `restaurant_final[order_date]` → `Date_Table[Date]` | Many-to-One | Single filter direction

---

## 📊 Dashboard Pages

### Page 1 — Executive Overview
> *"How is the entire business performing?"*

| KPI | Value | Insight |
|-----|-------|---------|
| 💰 Total Revenue | **2.90bn EGP** | ▲ +19.9% YoY |
| 📦 Number of Orders | **11M** | ▲ +19.9% YoY |
| 🧾 Average Order Value | **261 EGP** | Flat — growth is volume-driven |
| 👥 Number of Customers | **200K** | Stable customer base |

**Key Visuals:**
- 📈 Revenue vs. Orders Growth — MoM% dual-line chart (Revenue & Orders momentum side by side)
- 🏗️ Revenue Trend & YoY Growth — Waterfall chart with **Best Month** and **Growth Driver** dynamic cards
- 🏪 Total Revenue by Branch — Horizontal bar ranking all 6 branches
- 🍩 Orders Distribution by Type — Donut (Dine-in 40% · Takeaway 30% · Delivery 30%)

> **Smart Feature:** When a user selects a specific month, the waterfall chart dynamically updates to show the **Best Month** and the **Top Growth Driver Branch** — transforming the chart into a real-time analytical tool.

---

### Page 2 — Menu Performance
> *"What's selling, what's valuable, and what needs attention?"*

| KPI | Value |
|-----|-------|
| 🥇 Top Item | **كباب** (EGP 510M) |
| ⭐ Avg Rating | **3.70 / 5** |
| 🍽️ Number of Items | **15** |

**Key Visuals:**
- 📊 Total Revenue by Item — All 15 items ranked by revenue
- ⭕ Revenue & Quantity Scatter — Items plotted by quantity vs. revenue with average reference lines (4-quadrant menu engineering view)
- 🗺️ Total Revenue by Category — Treemap: مشويات (1.01bn) · طواجن (0.81bn) · محاشي (0.54bn) · مقبلات (0.34bn) · مشروبات (0.20bn)

> **Key Insight:** كفتة has higher **revenue per unit** than كباب — meaning كباب wins on volume, but كفتة wins on margin efficiency.

---

### Page 3 — Branch Analysis
> *"Which branches lead, which are efficient, and which are at risk?"*

| KPI | Value |
|-----|-------|
| ⚡ Most Efficient Branch | **طنطا** (highest AOV) |
| 🚀 Top Growth Branch | **القاهرة** |
| ⚠️ Quality Risk Branch | **أسيوط** (lowest avg rating) |

**Key Visuals:**
- 🥧 Revenue by Payment Method — Cash 50% · Card 30% · Wallet 20%
- 📊 Revenue & AOV by Branch — Combo chart: Revenue bars + AOV line per branch
- 📊 Revenue by Branch & Order Type — Stacked bars showing channel mix per branch

> **Strategic Insight:** Cairo leads in revenue (1.01bn) but Tanta leads in **AOV efficiency** — proving that raw revenue volume ≠ operational smartness.

---

### Page 4 — Customer & Quality Insights
> *"Who are our customers, when do they come, and is service quality holding?"*

| KPI | Value | Insight |
|-----|-------|---------|
| 🧾 AOV | **261 EGP** | Consistent across hours |
| 📦 Revenue per Quantity | **81.38 EGP** | Value per item sold |
| 🕗 Peak Hour | **20 (8PM)** | Highest demand window |

**Key Visuals:**
- 📈 Demand Volume Trends — Weekday vs. Weekend area chart by hour (10–24)
- 🍩 Sales Contribution by Day Type — Weekend 33.3% (0.97bn) vs. Weekday 66.7% (1.93bn)
- 📊 Revenue Pulse & Service Quality by Hour — Combo: Revenue bars + Avg Rating line per hour
- 🏆 Top 10 High-Value Loyal Customers — Table with Revenue, Orders, Rating

> **Quality Alert Logic:** The Revenue + Rating by Hour chart acts as a **real-time quality monitor** — if average rating drops during peak hours (8PM+), management can intervene before service quality deteriorates.

---

## 📈 Key Business Insights

```
┌─────────────────────────────────────────────────────────────────┐
│  💡 TOP 5 BUSINESS INSIGHTS FROM THIS DASHBOARD                 │
└─────────────────────────────────────────────────────────────────┘

  1. GROWTH IS VOLUME-ONLY
     Revenue ▲19.9% | Orders ▲19.9% | AOV → 0.0%
     The business grows by selling more, not by increasing order value.
     Action: Upselling & bundling strategies needed to lift AOV.

  2. CAIRO IS THE REVENUE ENGINE — TANTA IS THE SMART ONE
     Cairo: 1.01bn revenue (highest)
     Tanta: Highest AOV — doing more with less traffic.
     Action: Study Tanta's operational model and replicate it.

  3. THE 8PM DANGER ZONE
     Peak Hour = 20 (8PM). Rating tends to dip during peak hours.
     Action: Increase staff capacity at 8PM to protect service quality.

  4. MAY IS CONSISTENTLY THE BEST MONTH
     Dynamic "Best Month" card confirms May peaks every year.
     Action: Launch promotions before May to maximize peak-season revenue.

  5. كباب VS كفتة — VOLUME VS VALUE
     كباب: Highest total revenue (510M)
     كفتة: Highest revenue per unit sold
     Action: Feature كفتة more prominently on menus to improve margins.
```

---

## 🗂️ Repository Structure

```
📁 restaurant-sales-dashboard/
│
├── 📁 data/
│   ├── 📁 raw/                    # Original source files (CSV + JSON)
│   └── 📁 processed/              # Merged & cleaned table from Databricks
│
├── 📁 databricks/
│   └── 📄 merge_pipeline.sql      # SQL script to unify 9 files into 1 table
│
├── 📁 powerbi/
│   └── 📄 restaurant_dashboard.pbix  # Power BI report file
│
├── 📁 measures/
│   └── 📄 dax_measures.md         # All DAX measures with formulas & explanations
│
├── 📁 screenshots/
│   ├── 🖼️ executive_overview.png
│   ├── 🖼️ menu_performance.png
│   ├── 🖼️ branch_analysis.png
│   └── 🖼️ customer_quality.png
│
└── 📄 README.md
```

---

## ⚙️ DAX Measures Reference

<details>
<summary><strong>📐 Click to expand — Core Measures</strong></summary>

```dax
-- Total Revenue
Total_Revenue = SUM(restaurant_final[total_amount])

-- Average Order Value
AOV = DIVIDE([Number_Orders] , [Total_Revenue], 0)

-- Number of Orders
Number_Orders = DISTINCTCOUNT(restaurant_final[order_id])

-- Number of Customers
Number_customer = DISTINCTCOUNT(restaurant_final[customer_id])

-- Average Rating
Avg_Rating = AVERAGE(restaurant_final[rating])

-- Revenue per Quantity
Revenue_per_Quantity = DIVIDE([Total_Revenue], SUM(restaurant_final[quantity]), 0)

-- Peak Hour
Peak_Hour = 
CALCULATE(
    FIRSTNONBLANK(restaurant_final[hour], 1),
    TOPN(1, VALUES(restaurant_final[hour]), [Total_Revenue], DESC)
)
```

</details>

<details>
<summary><strong>📅 Click to expand — Time Intelligence Measures</strong></summary>

```dax
-- Previous Year Revenue
Revenue_PY = 
CALCULATE([Total_Revenue], SAMEPERIODLASTYEAR(Date_Table[Date]))

-- YoY Growth %
Revenue_Growth_YOY% = 
DIVIDE([Total_Revenue] - [Revenue_PY], [Revenue_PY], 0)

-- Previous Month Revenue
PM_Revenue = 
CALCULATE([Total_Revenue], DATEADD(Date_Table[Date], -1, MONTH))

-- MoM Growth %
Orders_Growth_% = 
DIVIDE([Number_Orders] - [Order_PY], [Order_PY], 0)

-- AOV Previous Year
AOV_PY = 
CALCULATE([AOV], SAMEPERIODLASTYEAR(Date_Table[Date]))

-- AOV Growth %
AOV_Growth% = 
DIVIDE([AOV] - [AOV_PY], [AOV_PY], 0)
```

</details>

<details>
<summary><strong>🏪 Click to expand — Branch Intelligence Measures</strong></summary>

```dax
-- Most Efficient Branch (Highest AOV)
Most_Efficient_Branch = 
CALCULATE(
    FIRSTNONBLANK(restaurant_final[branch], 1),
    TOPN(1, VALUES(restaurant_final[branch]), [AOV], DESC)
)

-- Top Growth Branch
Top_Growth_Branch = 
VAR BranchGrowth = 
    ADDCOLUMNS(
        VALUES(restaurant_final[branch]),
        "Growth", CALCULATE(DIVIDE([Total_Revenue] - [Revenue_PY], [Revenue_PY], 0))
    )
RETURN
    MAXX(TOPN(1, BranchGrowth, [Growth], DESC), restaurant_final[branch])

-- Quality Risk Branch (Lowest Rating)
Quality_Risk_Branch = 
CALCULATE(
    FIRSTNONBLANK(restaurant_final[branch], 1),
    TOPN(1, VALUES(restaurant_final[branch]), 
         CALCULATE(AVERAGE(restaurant_final[rating])), ASC)
)
```

</details>

<details>
<summary><strong>🍽️ Click to expand — Menu & Item Measures</strong></summary>

```dax
-- Top Item by Revenue
Top_Item = 
CALCULATE(
    FIRSTNONBLANK(restaurant_final[item_name], 1),
    TOPN(1, VALUES(restaurant_final[item_name]), [Total_Revenue], DESC)
)

-- Item Efficiency (Revenue per Unit Sold)
Item_Efficiency = 
DIVIDE([Total_Revenue], SUM(restaurant_final[quantity]), 0)

-- Number of Unique Items
Number_Items = DISTINCTCOUNT(restaurant_final[item_name])

-- Avg Item Quantity (for scatter reference line)
Avg_Item_Quantity = 
AVERAGEX(VALUES(restaurant_final[item_name]), CALCULATE(SUM(restaurant_final[quantity])))

-- Avg Item Revenue (for scatter reference line)
Avg_Item_Revenue = 
AVERAGEX(VALUES(restaurant_final[item_name]), CALCULATE([Total_Revenue]))
```

</details>

<details>
<summary><strong>👥 Click to expand — Customer Measures</strong></summary>

```dax
-- Purchase Frequency
Purchase_Frequency = 
DIVIDE([Number_Orders], [Number_customer], 0)

-- Customers Previous Year
Customers_PY = 
CALCULATE([Number_customer], SAMEPERIODLASTYEAR(Date_Table[Date]))

-- Customer Growth %
Customer_Growth% = 
DIVIDE([Number_customer] - [Customers_PY], [Customers_PY], 0)

-- Avg Discount %
Avg_Discount% = AVERAGE(restaurant_final[discount])
```

</details>

---

## 🛠️ How to Run This Project

### ✅ Option 1 — View Live (No setup needed)

Click the link below to open the fully deployed dashboard on Microsoft Fabric instantly:

> 🔗 **[https://app.fabric.microsoft.com/view?r=eyJrIjoiMGQ0YmUyMmQtMGViNS00ZWI3LWIzMGUtN2U0MzA0NjdlZGIyIiwidCI6IjJiYjZlNWJjLWMxMDktNDdmYi05NDMzLWMxYzZmNGZhMzNmZiIsImMiOjl9](https://app.fabric.microsoft.com/view?r=eyJrIjoiMGQ0YmUyMmQtMGViNS00ZWI3LWIzMGUtN2U0MzA0NjdlZGIyIiwidCI6IjJiYjZlNWJjLWMxMDktNDdmYi05NDMzLWMxYzZmNGZhMzNmZiIsImMiOjl9)**

### 🖥️ Option 2 — Run Locally

#### Prerequisites
- Power BI Desktop (latest version)
- Access to Databricks workspace *(or use the processed CSV in `/data/processed/`)*
- Microsoft Fabric account *(for cloud deployment)*

#### Steps

```bash
# 1. Clone the repository
git clone https://github.com/YOUR_USERNAME/restaurant-sales-dashboard.git
cd restaurant-sales-dashboard

# 2. (Optional) Run the Databricks SQL pipeline
# Open databricks/merge_pipeline.sql in your Databricks workspace
# Execute to generate the unified restaurant_final table

# 3. Open Power BI report
# Open powerbi/restaurant_dashboard.pbix in Power BI Desktop

# 4. Update data source connection
# Home → Transform Data → Data Source Settings
# Point to your Databricks endpoint OR the local processed CSV

# 5. Refresh & explore
# Click Refresh — all 4 dashboard pages will load
```

---

## 🧰 Tech Stack

| Category | Technology |
|----------|-----------|
| **Data Ingestion** | Fivetran |
| **Data Processing** | Databricks (Apache Spark + SQL) |
| **Data Visualization** | Microsoft Power BI |
| **Cloud Platform** | Microsoft Fabric |
| **Query Mode** | DirectQuery (live connection) |
| **Data Formats** | CSV, JSON |
| **Modeling Language** | DAX (Data Analysis Expressions) |
| **Scale** | ~11 Million rows |

---

## 📊 Dashboard Screenshots

| Page | Preview |
|------|---------|
| Executive Overview | Revenue 2.90bn · Orders 11M · YoY +19.9% |
| Menu Performance | Top Item: كباب · 15 Items · Avg Rating 3.70 |
| Branch Analysis | Cairo leads · Tanta most efficient · Assiut quality risk |
| Customer & Quality | Peak Hour 8PM · AOV 261 · Revenue per unit 81.38 |

> *Screenshots available in the `/screenshots/` folder*


---

## 👤 Author

**[Your Name]**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=flat-square&logo=linkedin)](https://linkedin.com/in/YOUR_PROFILE)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat-square&logo=github)](https://github.com/YOUR_USERNAME)

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

**⭐ If this project helped you, please give it a star!**

*Built with passion for data · Powered by modern data engineering*

</div>
