# 📊 Power BI: Snowflake to Star Schema Transformation & Advanced Analytics

## 🧠 Project Overview
This project demonstrates a high-level **Data Modeling** transformation within Power BI. The goal was to take a highly normalized **Snowflake Schema** (complex ERD with multiple levels of relationships) and re-engineer it into an optimized **Star Schema**. 

By applying a **"Bottom-Up" merging strategy** in Power Query, I ensured that the transformation maintained the **lowest grain of data**, preventing any data loss while significantly improving query performance and report usability.

---

## 🔎 Explore the Project

Navigate through the project components to understand the technical and business implementation:

* **📊 [Power BI Dashboard](Project_Files/Download_Link.md)**
    Download and explore the interactive dashboard with KPIs, segmentation, and time intelligence analysis.
* **🧱 [Data Modeling](docs/data_modeling.md)**
    Detailed explanation of transforming the schema into a Star Schema.
* **📊 [DAX Measures](docs/dax_measures.md)**
    Complete list of business metrics and KPIs (Total Profit, YOY, etc.) with technical explanations.
* **📈 [Business Insights](docs/insights.md)**
    Strategic recommendations and key findings derived from the Sales Performance analysis.
* **🖼️ [Images & Visuals](image/)**
    Dashboard screenshots and visual assets used in the project documentation and ERD and Data Modeling

---

## 🏗️ Schema Evolution (The Transformation)

The original dataset followed a strict Snowflake architecture with multiple sub-dimensions. I performed several **Power Query Merges** to denormalize the tables into a clean Star Schema.

### 🔄 Transformation Map

| Feature Area | Original Snowflake Tables (Before) | Optimized Star Dimension (After) |
| :--- | :--- | :--- |
| **Products** | `PRODUCTDIM` + `SUBCATEGORYDIM` + `CATEGORYDIM` | **Dim_Product** |
| **Geography** | `GEOGRAPHYDIM` + `CITYDIM` + `STATEDIM` + `REGIONDIM` + `COUNTRYDIM` | **Dim_Geography** |
| **Customers** | `CUSTOMERDIM` + `SEGMENTDIM` | **Dim_Customer** |
| **Orders** | `ORDERDIM` + `SHIPMODEDIM` | **Dim_Order** |
| **Sales** | `FACTSALES` | **FactSales** |

---

## ⚙️ Methodology: The Bottom-Up Approach

To guarantee **Data Integrity**, I followed a specific ETL workflow in Power Query:
1.  **Lowest Grain Identification:** Started from the most detailed tables (e.g., `CITYDIM`, `SHIPMODEDIM`).
2.  **Strategic Merging:** Merged higher-level attributes into the primary dimensions (e.g., merging `Category` into `Subcategory`, then merging the result into `Product`).
3.  **Data Flattening:** Removed redundant Foreign Keys and "ID" columns that were no longer needed after the merge, reducing the model size.
4.  **Star Schema Alignment:** Connected all flattened Dimensions to the `FACTSALES` table via a **1:Many relationship**, ensuring a clean and efficient analytical path.

---

## 📊 Business Intelligence & DAX

With the Star Schema in place, I developed a robust analytical layer focusing on profitability, growth, and regional performance.

### 📈 Key KPIs & Calculated Metrics
* **Profitability Suite:**
    * `Total Profit`: Sum of all realized profits.
    * `MoM Profit Growth %`: Monthly percentage change in profitability.
    * `MoM Profit Variance`: Absolute difference in profit month-over-month.
    * `Profit Last Year`: Benchmark for Year-over-Year (YoY) analysis.
* **Operational Metrics:**
    * `Avg Shipping Days`: (Ship Date - Order Date) to monitor logistics efficiency.
* **Volume & Reach:**
    * `Total Orders` & `Total Quantity`: Core volume tracking.
    * `Total Customers`: Unique customer reach.
    * `Total City` & `Number of Regions`: Geographic footprint analysis.

---

## 📌 Business Insights Derived
* **Geographic Bottlenecks:** Identified specific regions where `Avg Shipping Days` exceeded the company KPI, despite high profit margins.
* **Profit Volatility:** Using `MoM Profit Variance`, I pinpointed specific months with inconsistent performance, allowing for root-cause analysis.
* **Customer Segmentation:** The denormalized `Dim_Customer` allowed for instant filtering of profit trends by customer segment without complex multi-table joins.

---

## 🛠️ Tech Stack
* **Power BI Desktop:** Core platform for modeling and visualization.
* **Power Query (M):** Used for the complex "Bottom-Up" merge logic and data cleaning.
* **DAX:** Advanced time-intelligence and variance calculations.
* **Data Modeling:** Snowflake to Star Schema re-engineering.

---

## 📁 Project Structure
```text
Snowflake-to-Star-Schema-Project/
│
├── data/
│   └── Raw_ERD_Snowflake.png          # The original complex ERD
│
├── powerbi/
│   └── Sales_Analytics_Dashboard.pbix # The final Power BI file
│
├── images/
│   └── Star_Schema_Model.png          # Screenshot of the final 1:Many model
│   └── Dashboard_Preview.png          # Visual report highlights
│
└── README.md

---

👤 Author

Role: Data Analyst / Analytics Engineer
Specialization: Business Intelligence & Data Modeling

⭐ If you're interested in how I managed the complex merges in Power Query, feel free to explore the .pbix file or reach out!
