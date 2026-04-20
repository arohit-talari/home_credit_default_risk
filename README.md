

<h1 align="center">Home Credit Group - Characteristics of Default Behavior </h1>

<h1 align="center">Project Overview</h1>

Home Credit Group is a financial institution serving underbanked borrowers. The core analytical question driving this project is:

> **What combination of borrower characteristics separates low-risk borrowers from high-risk ones?**

The analytical challenge was immediate — preparing **307,511 records** of raw borrower data for analysis while making defensible, business-driven decisions across:
- **122 initial columns**
- **7 distinct missingness patterns**
- **Systematic encoding inconsistencies**

Before beginning analysis, the dataset was intentionally refined:
- Reduced from **122 → 25 columns** using evidence-based feature selection  
- Applied **five distinct NULL-handling strategies** (no blanket imputation)  
- **Flagged anomalous records** instead of dropping them to preserve analytical signal  
- Engineered **derived metrics** to support deeper exploratory analysis  

The workflow was structured into two clear phases:
1. **Data Cleaning & Transformation** — resolving structural, missingness, and encoding issues  
2. **Exploratory Data Analysis (EDA)** — uncovering patterns tied to borrower risk  

With a fully prepared dataset, the analysis identified a clear high-risk borrower profile - borrowers who meet this criteria default at **14.30%**, which is **77% higher** than the **8.07% portfolio benchmark**:

- **High-Risk Borrower Criteria**:
  - **Under 40 years old**
  - **Low or Mid-income**
  - Living in **unstable housing**
  - Carrying a **debt-to-income ratio > 0.20**

This **8.88 percentage point spread** directly quantifies what separates high-risk from low-risk borrowers in this portfolio, providing a clear, actionable foundation for credit risk strategy.

---
<h1 align="center">Skills Demonstrated</h1>

---
<h1 align="center">Dataset</h1>

---
<h1 align="center">Data Cleaning Approach</h1>

--- 
<h1 align="center">Key Findings</h1>

---
<h1 align="center">Scripts</h1>

- **[Data Cleaning Script](scripts/home_credit_data_cleaning.sql)**: Prepares the dataset for analysis by resolving missing values, correcting data inconsistencies, applying type conversions, and constructing derived variables.

- **[Exploratory Data Analysis (EDA) Script](scripts/home_credit_eda.sql)**: Analyzes the cleaned dataset to identify borrower risk patterns, quantify default rate drivers, and generate the key findings presented in this project.

---
<h1 align="center">Tools Used</h1>

<div align="center">

![MySQL](https://img.shields.io/badge/MySQL-9.x-blue)
![SQL](https://img.shields.io/badge/SQL-Data%20Analysis-lightgrey)

</div>

- **MySQL (v9.5.0)** — data cleaning, transformation, and analysis
- **SQL** — query development and exploratory analysis
