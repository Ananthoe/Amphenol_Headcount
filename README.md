# 📊 Amphenol Headcount Analytics Dashboard

A Power BI project developed to centralize and analyze Amphenol’s global headcount data across Business Units (BU), Groups, Divisions, Regions, Countries, Labor Types, and Cost Types from **2019 to 2025**. The primary audience is executive leadership — with the goal of driving insights on workforce trends, growth, and strategic allocations.

---

## 🧭 Project Objective

To design a dynamic, high-level headcount dashboard that helps executives answer:

- 📌 What is our **total headcount** at any point in time?
- 🔄 How has headcount changed **quarter over quarter** (QoQ) and **year over year** (YoY)?
- 🌍 How is headcount distributed by **Region**, **Division**, **Group**, **Country**, and **Business Unit**?
- 💰 What proportion of our workforce is **High Cost vs Low Cost**?
- 🧑‍🏭 What is the **labor type breakdown** (Hourly - Direct/Indirect, Salary)?

---

## 🗂️ Data Sources

> All data originated from raw Excel files pulled from **BPC (Business Planning & Consolidation)** — each representing a snapshot or rollup of headcount by region, cost type, BU, labor type, etc.

- 25+ Excel files initially ingested
- Raw data had inconsistent formatting, merged headers, totals, and duplicated structures
- Cleaned using **Power Query** and **ChatGPT-assisted bulk transformations**

---

## 🏗️ Data Modeling & Transformation

### ✅ Key Steps

- Removed totals, redundant columns, and merged headers
- Normalized **wide-format** tables into **long format**
- Created consolidated fact and dimension tables
- Used **Power Query M** for transformations and merging
- Defined relationships across 15+ tables (BU, Group, Division, Region, Date, Labor Type)

![Data Model Diagram](<"D:\Users\ajitha\Pictures\Screenshots\Screenshot 2025-11-09 163001.png">)

---

## 🧮 DAX Measures & Calculations

> DAX was extensively used to create dynamic measures powering slicer-driven visuals.

```dax
// 🔁 Dynamic change from selected quarter to Sep 2025
Dynamic_HC_Change_From_Chart =
VAR SelectedMonth = SELECTEDVALUE('Fact_Div_Grp_Hc'[Month_Year])
VAR BaseHC =
    CALCULATE(
        SUM('Fact_Div_Grp_Hc'[Headcount]),
        'Fact_Div_Grp_Hc'[Month_Year] = SelectedMonth
    )
VAR CurrentHC =
    CALCULATE(
        SUM('Fact_Div_Grp_Hc'[Headcount]),
        FILTER(ALL('Fact_Div_Grp_Hc'), 'Fact_Div_Grp_Hc'[Month_Year] = "Sep 2025")
    )
RETURN
IF(BaseHC <> 0, DIVIDE(CurrentHC - BaseHC, BaseHC))
