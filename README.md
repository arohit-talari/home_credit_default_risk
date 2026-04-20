<p align="center">
  <img src="images/home_credit_group.png" width="400">
</p>

<h1 align="center">Home Credit Group - Characteristics of Default Behavior </h1>

<h1 align="center">Project Overview</h1>

Home Credit Group is a global consumer finance company specializing in loans to underbanked borrowers — a population largely excluded from traditional credit markets due to limited or nonexistent credit histories. Without conventional credit scores, the core underwriting challenge is identifying which borrower characteristics reliably predict repayment behavior.

That challenge defines this project:

> **What combination of borrower characteristics separates low-risk borrowers from high-risk ones?**

The dataset spans 307,511 loan applications across 122 columns — covering borrower demographics, income and employment profile, housing status, external risk scores, and credit history indicators. Column scoping was the first and most consequential decision. Working from understood variables outward, then filling gaps through research into Home Credit's specific lending context, 97 columns were excluded across three groups: property-related columns averaging 58% missingness, contact flag columns with near-zero analytical variance, and credit bureau inquiry columns with correlations near zero against the default outcome. Several columns initially flagged for removal were retained — as the basis for flag logic, segmentation bins, or derived metrics the analysis would depend on. The 25-column working dataset was deliberately constructed, not simply reduced.

Seven distinct missingness scenarios required resolution before analysis could begin — each handled on its own terms rather than through uniform imputation.

The project ran across two phases: data cleaning and transformation to resolve structural, missingness, and encoding issues, followed by exploratory data analysis to isolate the patterns defining borrower risk.

The analysis identified a clear high-risk profile. Borrowers who are under 40, Low or Mid income, living in unstable housing, and carrying a debt-to-income ratio above 0.20 default at **14.30%** — 77% above the **8.07%** portfolio benchmark. The low-risk profile defaults at **5.42%**. The **8.88 percentage point spread** directly answers the core question. Seven findings followed — spanning demographic risk signals, financial ratio thresholds, and composite borrower profiles — each with direct implications for credit underwriting decisions.

---
<h1 align="center">Skills Demonstrated</h1>

<h2 align="left">Missing Value Strategy</h2>

Resolved seven distinct missingness scenarios across five strategies — each decision made on the specific characteristics of the column rather than a blanket rule applied uniformly across the dataset. The approach preserved analytical signal where uniform imputation would have obscured it.

<h2 align="left">Financial Ratio Derivation</h2>

Identified DTI and LTV as the relevant risk metrics for this analytical question and constructed both from raw source fields — neither existed in the source data. Both ratios served as the primary quantitative inputs to segmentation and threshold analysis.

<h2 align="left">Threshold Analysis</h2>

Determined the point at which debt burden transitions from manageable to high-risk — testing default rates across progressive DTI bands until the inflection point became clear. The result is a concrete, defensible cutoff rather than a directional observation.

<h2 align="left">Composite Risk Segmentation</h2>

Combined age, income tier, housing stability, and DTI into a multi-variable classification framework to define high and low-risk borrower profiles. The **8.88 percentage point spread** between profiles directly answers the core analytical question and gives underwriting teams a concrete basis for risk-tiered decision making.

<h2 align="left">SQL Execution</h2>

Extracted statistical benchmarks, borrower segments, and risk flags across 307,511 records — applying window functions, nested subqueries, and conditional logic to support every layer of the analysis, from data preparation through final risk profiling.

---
<h1 align="center">Dataset</h1>

The analysis uses the Home Credit Default Risk dataset — borrower-level application data capturing creditworthiness indicators across a large population of underbanked loan applicants. The original dataset spans 307,511 records across 122 columns, covering demographic, financial, employment, and behavioral attributes.

Before analysis began, the dataset was narrowed to a single-table structure of 25 columns — retaining only variables with direct bearing on borrower risk assessment. The full scoping rationale and exclusion decisions are documented in the Data Cleaning section.

| | |
|---|---|
| **Source** | Kaggle — [Home Credit Default Risk](https://www.kaggle.com/competitions/home-credit-default-risk/data?select=application_train.csv) |
| **File** | application_train.csv |
| **Original Size** | 307,511 rows × 122 columns |
| **Working Dataset** | 307,511 rows × 25 columns |
| **Structure** | Single-level analytical dataset |

---
<h1 align="center">Data Cleaning Approach</h1>

The data presented three structural issues that required resolution before any cleaning strategy could be applied.

Prior to loading the data, a diagnostic pass identified seven columns carrying missing values. Once loaded into MySQL, null counts across all seven returned zero — empty strings had come in as empty strings rather than NULL, meaning the null framework the entire cleaning phase depended on had to be established before a single strategy could be executed.

With the null structure in place, two deeper problems emerged. 18% of the data carried an unusable placeholder value in a column central to employment-based analysis — present at a scale large enough that any query run against it uncorrected would have produced wrong conclusions. Separately, a large share of null values across the dataset didn't represent missing data — they represented a real and identifiable borrower category that had no label, and would have disappeared entirely without intervention.

With the core problems understood, the cleaning phase moved column by column across all seven. Missingness ranged from 2 records to 202,924 — a uniform approach would have treated fundamentally different problems identically and masked the signals that mattered most. Each column was evaluated on its own terms and handled accordingly.

| Strategy | Column | Decision |
|---|---|---|
| Median imputation | AMT_ANNUITY | 12 nulls · imputed median 24,903.0 |
| Median imputation | EXT_SOURCE_2 | 660 nulls · imputed median 0.5660 |
| Fixed-value imputation | CNT_FAM_MEMBERS | 2 nulls · imputed 1.0 — single-person household |
| Flag, do not impute | AMT_GOODS_PRICE | 278 nulls · all revolving loans — structurally valid absence |
| Flag, do not impute | EXT_SOURCE_3 | 60,965 nulls · missingness non-random — default rate 9.3% where null vs. 7.8% where present |
| Reclassification | OCCUPATION_TYPE | 96,391 nulls · overlap with DAYS_EMPLOYED anomaly · reclassified as 'Not Employed / Unknown' |
| Structural validation | OWN_CAR_AGE | 202,924 nulls · maps directly to FLAG_OWN_CAR = N — structurally valid |

With seven missingness scenarios resolved, encoding inconsistencies corrected, and anomalous records flagged rather than dropped, the data entering the EDA phase was structurally sound — each retained variable positioned to contribute directly to identifying what separates high-risk borrowers from low-risk ones.

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
