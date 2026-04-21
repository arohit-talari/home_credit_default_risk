<p align="center">
  <img src="images/home_credit_group.png" width="400">
</p>

<h1 align="center">Home Credit Group - Borrower Risk Profiling </h1>

<h2 align="center">Project Overview</h2>

Home Credit Group is a global consumer finance company specializing in loans to underbanked borrowers — a population largely excluded from traditional credit markets due to limited or nonexistent credit histories. Without conventional credit scores, the core underwriting challenge is identifying which borrower characteristics reliably predict repayment behavior.

That challenge defines this project:

> What combination of borrower characteristics separates low-risk borrowers from high-risk ones?

The dataset spans **307,511 loan applications** across 122 columns — covering borrower demographics, income and employment profile, housing status, external risk scores, and credit history indicators. Column scoping was the first and most consequential decision. Working from understood variables outward, then filling gaps through research into Home Credit's specific lending context, 97 columns were excluded across three groups. Several columns initially flagged for removal were retained — serving as inputs to segmentation logic, risk flags, and derived metrics the analysis would depend on. The 25-column working dataset was deliberately constructed, not simply reduced.

Seven distinct missingness scenarios required resolution before analysis could begin — each handled on its own terms rather than through uniform imputation.

The analysis identified a clear high-risk profile. Borrowers who are under 40, Low or Mid income, living in unstable housing, and carrying a debt-to-income ratio above 0.20 default at **14.30%** — **77%** above the **8.07%** portfolio benchmark. The low-risk profile defaults at **5.42%**. The **8.88 percentage point spread** between the two profiles gives underwriting teams a concrete, data-driven basis for separating borrowers who are likely to repay from those who aren't.

---
<h2 align="center">Dataset & Scope</h2>

The analysis uses the Home Credit Default Risk dataset — borrower-level application data capturing creditworthiness indicators across a large population of underbanked loan applicants. The original dataset spans **307,511 records** across 122 columns, covering demographic, financial, employment, and behavioral attributes.

Before analysis began, the dataset was narrowed to a single analytical table of 25 columns. The full scoping rationale and exclusion decisions are documented in the Data Cleaning section.

| | |
|---|---|
| **Source** | Kaggle — [Home Credit Default Risk](https://www.kaggle.com/competitions/home-credit-default-risk/data?select=application_train.csv) |
| **File** | application_train.csv |
| **Original Size** | 307,511 rows × 122 columns |
| **Working Dataset** | 307,511 rows × 25 columns |
| **Structure** | Single-level analytical dataset |

---
<h2 align="center">Analytical Methods</h2>

<h3 align="left">Missing Value Strategy</h3>

Resolved seven distinct missingness scenarios across five strategies — each decision made on the specific characteristics of the column rather than a blanket rule applied uniformly across the data.

<h3 align="left">Financial Ratio Derivation</h3>

Identified **Debt-to-income (DTI)** and **Loan-to-value (LTV)** as the relevant risk metrics for this analytical question and constructed both from raw source fields — neither existed in the source data. Both ratios served as the primary quantitative inputs to segmentation and threshold analysis.

<h3 align="left">Threshold Analysis</h3>

Determined the point at which debt burden transitions from manageable to high-risk — testing default rates across progressive DTI bands until the inflection point became clear.

<h3 align="left">Composite Risk Segmentation</h3>

Combined age, income tier, housing stability, and DTI into a borrower profile framework to define high and low-risk segments. The **8.88 percentage point spread** between the two directly answers the core analytical question and gives underwriting teams a concrete basis for separating borrowers by risk level before making lending decisions.

<h3 align="left">SQL Execution</h3>

Extracted statistical benchmarks, borrower segments, and risk flags across **307,511 records** — applying window functions, nested subqueries, and conditional logic to support every layer of the analysis, from data preparation through final risk profiling.

---
<h2 align="center">Data Cleaning & Transformation</h2>

Upon loading the data into MySQL, a diagnostic pass identified three structural issues that required resolution before any cleaning strategy could be applied.

The first was foundational. Seven columns were confirmed to carry missing values prior to load — yet null counts across all seven returned zero once inside MySQL. Empty strings had come in as empty strings rather than NULL, meaning the structure MySQL needed to recognize missing values had to be established before a single strategy could be executed.

With that structure in place, two deeper problems emerged. **18%** of the dataset carried an unusable placeholder value in a column central to employment-based analysis — a scale that would have corrupted every employment-based finding without correction. Separately, a large share of null values across the dataset didn't represent missing data — they represented a real and identifiable borrower category that would have disappeared entirely without intervention. The clearest illustration of this: EXT_SOURCE_3 carried 60,965 nulls where the missingness itself correlated with a higher default rate — **9.3%** where null versus **7.8%** where present — making imputation the wrong call.

With the core problems understood, the cleaning phase moved column by column across all seven. Missingness ranged from **2 records to 202,924** — a uniform approach would have treated fundamentally different problems identically. Each column was evaluated on its own terms and handled accordingly.

| Strategy | Column | Decision |
|---|---|---|
| Median imputation | AMT_ANNUITY | 12 nulls · imputed median 24,903.0 |
| Median imputation | EXT_SOURCE_2 | 660 nulls · imputed median 0.5660 |
| Fixed value imputation | CNT_FAM_MEMBERS | 2 nulls · imputed 1.0 — single-person household |
| Flag, do not impute | AMT_GOODS_PRICE | 278 nulls · all revolving loans — structurally valid absence |
| Flag, do not impute | EXT_SOURCE_3 | 60,965 nulls · missingness non-random — flagged, not imputed |
| Reclassification | OCCUPATION_TYPE | 96,391 nulls · DAYS_EMPLOYED overlap · reclassified 'Not Employed / Unknown' |
| Structural validation | OWN_CAR_AGE | 202,924 nulls · maps directly to FLAG_OWN_CAR = N — structurally valid |

With seven missingness scenarios resolved, encoding inconsistencies corrected, and anomalous records flagged rather than dropped, the data entering the EDA phase was structurally sound — each retained variable positioned to contribute directly to identifying what separates high-risk borrowers from low-risk ones.

--- 
<h2 align="center">Borrower Risk Analysis</h2>


*A note on DTI and LTV*

Two derived ratios serve as the primary financial inputs to the analysis. **Debt-to-income ratio (DTI)** measures how much of a borrower's income is committed to debt obligations — a high DTI signals a borrower who is financially stretched relative to what they earn. **Loan-to-value ratio (LTV)** measures how much of a purchase is being financed relative to the asset's value — a high LTV signals a borrower who is highly leveraged with little equity cushion. Neither ratio existed in the source data. Both were constructed from raw source fields before analysis began.

---

**Portfolio benchmark: 8.07% default rate across 307,511 records**

<h3 align="left">Finding 1 — A defined borrower profile drives a 77% increase in default risk</h3>

Borrowers under 40, in Low or Mid income tiers, living in unstable housing, and carrying a DTI above 0.20 represent the high-risk profile — defaulting at **14.30%**, **77%** above the **8.07%** portfolio benchmark. The low-risk profile defaults at **5.42%**. The **8.88 percentage point spread** between the two directly answers the core analytical question — default risk in this portfolio concentrates not within any single variable, but at the intersection of demographic, financial, and stability factors combined.

<h3 align="left">Finding 2 — Education level is the strongest demographic risk signal</h3>

Default rates range from **1.83%** at the academic degree level to **10.93%** at lower secondary — a nearly sixfold difference across the education spectrum. Secondary and secondary special borrowers default at **8.94%** across 218,391 records, making them the most consequential risk segment in the portfolio by volume.

<h3 align="left">Finding 3 — Age and regional risk tier are the two most reliable standalone risk indicators</h3>

Age declines without exception from **11.47%** among borrowers in their 20s (44,738 records) to **4.92%** among borrowers 60 and older (35,665 records) — every cohort lower than the one before it. Regional risk tiers follow the same pattern, scaling from **4.82%** in Low Risk regions (32,197 records) to **11.10%** in High Risk regions (48,330 records) — a **6.28 percentage point spread** across three tiers with no outlier breaking the sequence.

<h3 align="left">Finding 4 — DTI above 0.20 is the actionable financial threshold</h3>

Default risk accelerates meaningfully once a borrower's DTI exceeds 0.20 — establishing it as the primary financial signal for elevated risk. A borrower already carrying high debt relative to income who is also financing a purchase through debt rather than equity compounds that risk further. High DTI combined with High LTV produces a **12.66%** default rate across 5,127 records versus **7.33%** for Low DTI with Low LTV across 187,299 records. DTI above 0.20 flags the risk. Pairing it with high leverage amplifies it — though establishing a precise LTV threshold would require further analysis.

<h3 align="left">Finding 5 — Income tier alone is an unreliable standalone predictor</h3>

Mid and Low income borrowers default at nearly identical rates — **8.55%** and **8.23%** respectively — a 0.32 percentage point gap that is not meaningful at the portfolio level. High income is the only tier where income reliably suppresses default risk, at **5.43%**. The convergence between Mid and Low isn't coincidental — Low income borrowers carry the highest average DTI in the dataset at 0.2369, while Mid income borrowers earning more still carry debt burdens that close the gap earnings alone would suggest exists. Below the High income threshold, what a borrower earns matters less than how much of it is already committed. Income without debt burden context produces incomplete risk conclusions.

<h3 align="left">Finding 6 — Housing instability is a stronger default driver than employment status</h3>

Actively employed borrowers default at higher rates than unemployed and pensioner borrowers across every housing type examined — a pattern that holds without exception regardless of housing category. Employed renters carry the highest rate in the cross at **12.55%** across 4,676 records, while employed borrowers living with parents follow at **11.80%** across 14,621 records. Unemployed and pensioner borrowers flag at **5.40%** — well below the portfolio benchmark — because the segment is predominantly pensioners with stable fixed income rather than borrowers with no income. Housing instability is a stronger default driver than employment status in this portfolio, a finding that directly challenges conventional underwriting assumptions.

<h3 align="left">Finding 7 — Employer industry type is a meaningful and underutilized risk signal</h3>

Among organization types with more than 500 records, Transport: type 3 carries the highest default rate at **15.75%** across 1,187 records — nearly double the portfolio benchmark. Restaurant (**11.71%**, 1,811 records) and Construction (**11.68%**, 6,721 records) follow, both characterized by irregular income and limited employment security. Employer industry type warrants inclusion as a standalone screening variable in credit underwriting decisions.

---
<h2 align="center">Scripts & Documentation</h2>

- **[Data Cleaning Script](scripts/home_credit_data_cleaning.sql)**: Resolves the structural, missingness, and encoding issues documented in the Data Cleaning & Transformation section. Each script block is annotated with the reasoning behind every decision — not just what the code does, but why.

- **[Exploratory Data Analysis (EDA) Script](scripts/home_credit_eda.sql)**:  Runs univariate profiling, bivariate cross-tabulations, threshold analysis, and composite risk segmentation across the cleaned dataset. Each analytical block is documented with the question it was designed to answer.

---
<h2 align="center">Technical Stack</h2>

- **MySQL (v9.5.0)** — data cleaning, transformation, and exploratory analysis
