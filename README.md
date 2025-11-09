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

<img width="1367" height="653" alt="Screenshot 2025-11-09 163001" src="https://github.com/user-attachments/assets/882f4ab0-7799-4ded-b26b-bdaef59828ad" />


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


// 🔁 QoQ % Change between June 2025 and Sep 2025
QoQ Headcount % Change =
VAR Current_Q = CALCULATE(SUM(Fact_Div_Grp_Hc[Headcount]), Fact_Div_Grp_Hc[Month_Year] = "Sep 2025")
VAR Prev_Q = CALCULATE(SUM(Fact_Div_Grp_Hc[Headcount]), Fact_Div_Grp_Hc[Month_Year] = "Jun 2025")
RETURN DIVIDE(Current_Q - Prev_Q, Prev_Q)


// 📈 Absolute Headcount Change Mar 2025 vs Dec 2024
Headcount_Change_Mar_vs_Dec =
VAR Mar = CALCULATE(SUM(Fact_Div_Grp_Hc[Headcount]), Fact_Div_Grp_Hc[Date] = DATE(2025, 3, 31))
VAR Dec = CALCULATE(SUM(Fact_Div_Grp_Hc[Headcount]), Fact_Div_Grp_Hc[Date] = DATE(2024, 12, 31))
RETURN Mar - Dec
```
## 📊 Dashboard Pages

### 1. Executive Summary

- **KPI Cards**:
  - ✅ Total Headcount
  - 🔁 QoQ Growth
  - 📆 YoY Growth
  - 🌱 Organic Growth
  - 🧩 Acquisition Impact
  - 📈 Net Headcount Change
- **Charts & Visuals**:
  - Total Headcount by Quarter (line chart)
  - Headcount by Region (stacked bar)
  - Headcount by Country (donut chart)
  - Headcount by Labor Type (clustered bar)
  - Headcount by Cost Type (clustered bar)

---

### 2. Division & Group Overview

- QoQ Headcount Change by Group (clustered bar with dynamic title)
- Headcount by Business Unit:
  - Interactive drill-down by Region → Division → Group
  - Includes slicers for Quarter, Labor Type, Gender
- Matrix View:
  - Business Unit × Quarter
  - Columns for Total HC, Gender, Labor Split

---

### 3. Regional & Country View

- Region-level Trend Analysis (line chart over time)
- Headcount by Cost Type across Regions (100% stacked bar)
- Labor Type Breakdown (bar chart — Salary, Hourly Direct, Indirect)
- Matrix:
  - Region → Country → Business Unit hierarchy
  - Interactive search for any BU or Country
- Slicers:
  - Region, Country, Business Unit

---

## 🧰 Tools Used

| Tool         | Purpose                                      |
|--------------|----------------------------------------------|
| Power BI     | Data modeling, dashboard creation, DAX       |
| Power Query  | Data cleaning and transformation             |
| Excel        | Original source files                        |
| DAX          | Custom KPIs, growth logic, dynamic titles    |
| ChatGPT      | Used for bulk transformation and automation  |

---

## 🔐 Confidentiality Notice

> This dashboard was developed using internal data and confidential structure.  
> For public distribution, all business unit names, groups, and sensitive identifiers have been:
> - ✅ Renamed or replaced with generic placeholders
> - ✅ Hashed or masked in sample datasets
> - ✅ Screenshots curated to avoid exposing strategic or confidential insights



