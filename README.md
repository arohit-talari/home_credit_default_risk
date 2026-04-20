# Home Credit Group - Characteristics of Default Behavior 

SQL-based credit risk analysis transforming 300K+ borrower records through structured cleaning and EDA to quantify the drivers of default risk.

## Project Overview

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

With a fully prepared dataset, the analysis identified a clear high-risk borrower profile:

- Borrowers who are:
  - **Under 40 years old**
  - **Low or Mid-income**
  - Living in **unstable housing**
  - Carrying a **debt-to-income ratio > 0.20**

Borrowers who meet this criteria defaults at **14.30%**, which is **77% higher** than the **8.07% portfolio benchmark**  

This **8.88 percentage point spread** directly quantifies what separates high-risk from low-risk borrowers in this portfolio, providing a clear, actionable foundation for credit risk strategy.
