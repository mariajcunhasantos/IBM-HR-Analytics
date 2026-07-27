# IBM HR Analytics – Employee Insights Project
End-to-end HR analytics solution built for a simulated IBM context.

---

## Project Overview

After the unexpected loss of the previous HR Manager, the organization was left without documented knowledge about its workforce. This project was developed to restore visibility and support data-driven decision-making in Human Resources.

### Business Context and Objectives

- Lack of visibility on employee satisfaction, gender diversity, age distribution and recruitment patterns
- Strategic goal: reach **50% women in all job levels**
- Need for continuous monitoring capability
- Create a foundation for a data-driven culture in HR

### Key Analytical Focus Areas

- Complete characterisation of the employee population
- Gender gap analysis and distance to equality targets
- Employee satisfaction and the impact of wellbeing initiatives (e.g. ping-pong table)
- Attrition patterns and flight-risk identification
- Performance, compensation and tenure relationships

---

## Datasets

| Dataset                | Description                                                                 | Records |
|------------------------|-----------------------------------------------------------------------------|---------|
| **IBM_DATA.csv**       | Simulated IBM HR snapshot (demographic, financial and satisfaction variables) | 1,470   |
| **PingPongSurvey.csv** | Follow-up satisfaction survey after the introduction of a ping-pong table   | Subset  |

The IBM dataset originates from Kaggle (IBM HR Analytics Employee Attrition and Performance).

---

## Project Pipeline

```
1. Exploratory Data Analysis (R)
2. Data Modelling & Normalisation (SQL Server – Star Schema)
3. Analytical Queries (SQL)
4. Data Transformation (Power Query)
5. Semantic Model + Measures (Power BI + DAX)
6. Interactive Dashboards (Power BI)
```

---

## 1. Exploratory Data Analysis (R)

Performed in Google Colab using `tidyverse` and `corrplot`.

**Main activities:**
- Data loading and missing-value check (no significant issues found)
- Descriptive statistics and attrition rate calculation
- Distribution analysis and group comparisons (Attrition Yes/No)
- Correlation analysis and scatter plots
- Boxplots by department, gender and job level
- Outlier analysis on monthly income

→ Notebook: `Data_Import_and_Exploratory_Data_Analysis_in_R.ipynb`

---

## 2. Data Modelling (SQL Server)

A dimensional model close to a **Star Schema** was implemented:

### Dimensions
- **D_Position** – Department, Job Role, Job Level (with surrogate key `PositionID`)
- **D_Employee** – Core employee attributes, demographic data, calculated fields (`Generation`, `BirthDate`, etc.) and data-quality constraints

### Fact Tables
- **F_Satisfaction** – Satisfaction scores over time (original + post ping-pong survey)
- **F_Performance** – Performance ratings
- **F_Employee_History** – Compensation, tenure, stock options and salary hikes

**Key features of the model:**
- Surrogate keys and referential integrity
- Data-quality constraints (age ≥ 16, coherence checks, etc.)
- Calculated columns for analytical enrichment (`Generation`, descriptive labels, Stock Option Category)
- Synthetic but realistic dates (`BirthDate`, `StartDate`)
- Indexes on foreign keys for performance
- Validation queries for nulls, row counts and temporal coherence

---

## 3. Analytical SQL Queries

Queries were developed to answer the main business questions:

- Workforce characterisation (headcount, attrition rate, demographic and professional distribution)
- Satisfaction analysis (overall and segmented averages, before vs after comparison)
- Gender equality (representation by level, pay gap, training access)
- Attrition patterns and risk factors (marital status, distance from home, tenure with manager, satisfaction)
- Performance & compensation analysis
- **Flight Risk Score** – composite indicator combining multiple risk factors to flag high-potential leavers

> Note: Some tenure-based analyses are subject to survival bias (only current employees are present in the snapshot).

---

## 4. Power Query & Power BI Model

- Data cleaned and enriched in Power Query (renaming, derived columns)
- Dimensional model implemented in Power BI Desktop following star-schema best practices
- Calendar table created for time intelligence
- Relationships configured with correct cardinality and filter direction
- Extensive set of **DAX measures** covering satisfaction, attrition, compensation and gender equality KPIs

---

## 5. Power BI Report

Interactive report with a consistent corporate visual identity and three main thematic pages:

| Page                       | Focus                                              |
|----------------------------|----------------------------------------------------|
| **Executive Overview**     | Who we are – headcount, demographics, attrition    |
| **Gender Gap**             | Distance to 50% target, representation & pay gap   |
| **Employee Satisfaction**  | Satisfaction drivers, evolution and wellbeing impact |

**Features:**
- Key performance indicators (KPIs)
- Interactive slicers (department, job role, gender, time period, etc.)
- Designed for both C-level and department managers

An executive PowerPoint summary (3 slides) was also produced:
1. Who are we?
2. Are we happy?
3. How far are we from gender equality?
