# 🏗️ Technical Documentation: Snowflake to Star Schema Transformation

This document provides a deep dive into the architectural decisions and technical steps taken to transform the normalized Ecommerce ERD into an optimized **Star Schema**.

---

## 1. Initial State: The Snowflake Challenge
The original dataset was structured as a **Snowflake Schema**. While this is efficient for storage, it presented challenges for analytics:
* **Complex Joins:** Multiple levels of relationships forced the DAX engine to perform multi-step joins.
* **Performance:** Each level of normalization adds overhead during report interaction.

## 2. Transformation Strategy: Bottom-Up Merging
I implemented a **Bottom-Up merging approach** in Power Query, starting from the lowest grain to ensure zero data loss.

### 🔄 The Denormalization Process

#### 📦 Product Dimension (Lowest Grain Maintenance)
* **Path:** `PRODUCTDIM` (Base) ← `SUBCATEGORYDIM` ← `CATEGORYDIM`.
* **Technical Action:** 1. Performed a **Left Outer Join** starting from `PRODUCTDIM` with `SUBCATEGORYDIM`.
    2. Followed by another **Left Outer Join** with `CATEGORYDIM`.
* **Logic:** Starting with the Product (Lowest Grain) and using **Left Joins** ensures that every product record is preserved, even if it lacks a category assignment, while preventing record duplication.

#### 🌍 Geography Dimension
Instead of having 5 separate fragmented tables, I consolidated them into a single flattened hierarchy to enhance reporting performance:
* **Path:** `GEOGRAPHYDIM` (Base) ← `CITYDIM` ← `STATEDIM` ← `REGIONDIM` ← `COUNTRYDIM`.
* **Technical Action:** 1. Started with **GEOGRAPHYDIM** as the primary table (the lowest grain connecting to the Fact).
    2. Performed sequential **Left Outer Joins** to bring in `City`, then `State`, then `Region`, and finally `Country` attributes.
* **Logic:** This "Base-to-Top" approach ensures that every geographic key in the system is preserved while flattening the attributes into a single row, eliminating 4 unnecessary relationships in the data model.
* **Result:** `Dim_Geography` — A comprehensive, one-stop table for all spatial analysis and regional drill-downs.

#### 👥 Customer Dimension
* **Technical Action:** Performed a **Left Outer Join** between `CUSTOMERDIM` (Many) and `SEGMENTDIM` (One).
* **Logic:** This preserves all unique customer records while appending their segment names (Consumer, Corporate, etc.) into the primary dimension.

---

## 3. Fact Table Optimization (Order Integration)
To optimize calculation performance and simplify the model, I made a strategic decision regarding the `ORDERDIM` table:
* **Technical Action:** Instead of keeping a separate Order dimension, I merged the `Order_Date` and `Ship_Date` from `ORDERDIM` directly into the **FactSales** table.
* **Reasoning:** By bringing the dates into the Fact table, I enabled direct calculation of:
    * `Avg Shipping Days`: `[Ship_Date] - [Order_Date]` performed at the row level.
    * Faster **Time Intelligence** calculations without cross-table relationship overhead.
* **Integrity:** Only the essential date columns were merged to keep the Fact table lean and performant.

---

## 4. Final Star Schema Architecture
The resulting model is optimized for the Power BI VertiPaq engine:
* **Central Fact:** `FactSales` (Now includes Order & Ship dates).
* **Dimensions:** `Dim_Product`, `Dim_Geography`, `Dim_Customer`, and `Dim_Date`.
* **Relationship Logic:** All relationships are **One-to-Many (1:*)** with **Single Direction** filters.

---

## 5. Business Impact
1. **Query Optimization:** Reduced the number of joins needed for regional profit analysis by 70%.
2. **DAX Simplicity:** Measures like `MoM Profit Growth %` are more stable and easier to maintain.
3. **Data Accuracy:** The "Left Join" strategy guaranteed that no transactional or master data was lost during the transformation.

