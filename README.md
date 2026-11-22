# ApexPlanet Data Analytics Internship — Task 4: Data Storytelling & Statistical Validation

**Author:** Senior Data Analyst, Statistical Modeling Specialist & BI Developer  
**Evaluation Track:** ApexPlanet Internship Task 4 Final Submission  
**Dataset Reference:** Cleaned Retail Sales Dataset (`data/apexplanet_cleaned_sales.xlsx`) | $N = 1,000$ Transaction Records | $992$ Distinct Original Order IDs  
**Source Repositories:**
- **Task 1 — Data Cleaning:** [https://github.com/Chida-46/apex_task_1](https://github.com/Chida-46/apex_task_1)
- **Task 2 — Exploratory Data Analysis & Business Intelligence:** [https://github.com/Chida-46/apex_task_2](https://github.com/Chida-46/apex_task_2)
- **Task 3 — Deep-Dive Analysis & Interactive Dashboarding:** [https://github.com/Chida-46/apex_task_3](https://github.com/Chida-46/apex_task_3)

---

## 1. Project Overview & Narrative Context

In Tasks 1 through 3, ApexPlanet analyzed retail sales transactions across eight major Indian commercial hubs. Initial exploratory analysis and Power BI reporting established core KPIs and highlighted **Electronics** as the leading category by revenue ($50.78\text{M Monetary Units}$), **Patna** as the highest-revenue city ($19.29\text{M Monetary Units}$), **Laptop** as the leading product by revenue ($25.44\text{M Monetary Units}$), **Mobile** as the leading product by quantity ($1,008\text{ units}$), and customers aged **36–45** as the highest-revenue demographic cohort ($31.53\text{M Monetary Units}$).

However, descriptive aggregates without inferential testing can produce **optical illusions**. Without statistical hypothesis testing, leadership risks conflating **transaction frequency (volume)** with **customer purchasing power (basket value)**.

Task 4 provides the statistical validation and business storytelling synthesis needed for robust strategic decision-making. Using Python (`pandas`, `numpy`, `scipy.stats`, `statsmodels`, `matplotlib`, `seaborn`, `python-pptx`, `reportlab`), this project:
1. **Audits and reconciles five documented cross-task data discrepancies** across Tasks 1–3 without fabricating data.
2. **Formulates and executes five formal hypothesis tests** ($\alpha = 0.05$), calculating exact test statistics, p-values, 95% confidence intervals, and standardized effect sizes ($\eta^2$, Cohen's $w$, Cohen's $d$, Cramer's $V$).
3. **Identifies the "Volume vs. Value Fallacy"**, proving that while Electronics dominates total revenue, its average transaction spend is statistically identical to all other product categories ($p = 0.5803$).
4. **Applies strict statistical interpretation guardrails**, noting that evidence against equal category frequencies demonstrates that observed transaction counts deviate from equal expectation ($200$ rows/category), but does not independently establish consumer preference or organic demand.
5. **Delivers an executive-ready suite of deliverables**: an executed Jupyter Notebook with clean relative paths, a formatted multi-tab Excel model, a 15-slide PowerPoint presentation, an executive PDF report, and 12 high-resolution visualizations.

---

## 2. Core Business Objectives

1. **Inferential Validation of Aggregate Metrics:** Test whether observed revenue leads in Electronics and middle-aged customers reflect higher customer spending or transaction row volume.
2. **Cross-Task Audit & Reconciliation:** Investigate and document root causes for discrepancies in Order ID counts, age-group boundary syntax, customer ID collisions, unclosed reporting months, and currency notation.
3. **Statistical Hypothesis Testing:** Evaluate five business hypotheses using appropriate parametric and non-parametric tests, checking all statistical assumptions.
4. **Distinguish Statistical vs. Practical Significance:** Provide nuanced commercial interpretations avoiding over-generalization.
5. **Commercial Action Plan:** Formulate actionable recommendations spanning cross-category merchandising, behavioral RFM segmentation, inventory allocation, and data governance.

---

## 3. Verified Baseline KPIs & Reconciliation

All baseline financial metrics are verified against the primary dataset (`data/apexplanet_cleaned_sales.xlsx`), covering transactions from **January 1, 2025 to January 1, 2026**:

| Metric Name | Verified Value | Calculation Formula | Commercial Significance |
| :--- | :---: | :--- | :--- |
| **Total Revenue** | **139,399,439.65 MU** | `SUM(Total_Sales)` | Gross platform turnover across all 1,000 transaction records. |
| **Transaction Records** | **1,000 Records** | `COUNT(Rows)` | Total transaction rows recorded in the sales ledger. |
| **Distinct Order IDs** | **992 Orders** | `DISTINCTCOUNT(Order_ID)` | Unique purchasing order events (reconciling 8 duplicate IDs). |
| **Average Order Value (AOV)** | **140,523.63 MU** | `Total Revenue / 992 Orders` | True average transaction expenditure per distinct order. |
| **AOV per Transaction Record** | **139,399.44 MU** | `Total Revenue / 1,000 Rows` | Average revenue per transaction line item. |
| **Active Customers** | **947 Customers** | `DISTINCTCOUNT(Customer_ID)` | Unique purchasing accounts (subject to 47 multi-cohort collisions). |
| **Revenue per Customer** | **147,201.10 MU** | `Total Revenue / Total Customers` | Average monetary yield per distinct customer entity. |
| **Total Units Sold** | **5,435 Units** | `SUM(Quantity)` | Platform physical goods volume (mean: 5.44 units/order). |

*Currency Note: All monetary values are reported in Monetary Units (MU). Inspection of raw Excel source formatting confirms zero currency symbols ($/INR). Indian operating cities strongly suggest Indian Rupees (INR), but values are maintained as MU to preserve strict audit integrity.*

---

## 4. Cross-Task Data Quality Audit & Reconciliation

| Audit Area | Previous Work Findings | Task 4 Root Cause & Resolution | Commercial Impact |
| :--- | :--- | :--- | :--- |
| **Transaction Rows vs. Order IDs** | Task 1 added suffixes `_FIX01` to `_FIX08` creating 1,000 unique IDs. Tasks 2 & 3 kept 992 distinct IDs. | `ORD100050` appears 9 times across different dates, customer IDs, and products in raw data. Distinct original Order IDs = **992**; Total records = **1,000**. | AOV based on 992 distinct orders is **140,523.63 MU**; AOV based on 1,000 transaction rows is **139,399.44 MU** (+0.8%). Neither is fabricated; both represent valid levels of analysis. |
| **Electronics Percentages** | Previous analysis cited 36.4% and 35.4% without full distinction. | **Verified Actual Figures:** Total Revenue = 139.40M MU; Electronics Revenue = 50.78M MU $\rightarrow$ **Revenue Share = 36.43%**. Total Records = 1,000; Electronics Records = 354 $\rightarrow$ **Transaction-Row Share = 35.40%**. Distinct Orders = 353 of 992 (35.58%). | Electronics commands 36.43% of revenue purely because it represents 35.40% of transaction rows. Average order spend is statistically identical across categories ($p = 0.5803$). |
| **Age Group Discrepancy** | Task 2: Ages **35–44** top revenue ($32.80\text{M MU}$). Task 3: Ages **36–45** top revenue ($31.53\text{M MU}$). | **Boundary Syntax Difference:** Task 2 SQL used closed interval `BETWEEN 35 AND 44` (includes age 35; 225 rows). Task 3 / Pandas used half-open `(35, 45]` (excludes 35, includes 45; 223 rows). | Neither cohort has higher spending power (ANOVA $p=0.9833$). The divergence is purely a syntax binning artifact. |
| **Customer ID Collisions** | Task 3 reported 47 Customer IDs in multiple age groups. | Investigation proves 47 IDs have conflicting names, ages, and cities across rows (e.g., `CUST1463` is *Customer_397*, age 21 in BLR, but *Customer_257*, age 56 in Gaya). | Synthetic data generation flaw or lack of CRM composite keys. Segment-level customer counts overlap and cannot be summed. |
| **Incomplete Final Month** | Task 2 & 3 showed an apparent sales drop in January 2026 ($809\text{k MU}$). | Data terminates on 2026-01-01 and contains only 5 orders from the first day of the month ($0.58\%$ of revenue). Full-year 2025 has 995 orders totaling $138.59\text{M MU}$. | January 2026 is an unclosed reporting period and must be isolated from MoM trend analysis to prevent false alarms. |
| **Currency Schema** | Currency code not specified in documentation. | Inspection of raw Excel source formatting confirms zero currency symbols or ISO codes ($/\text{INR}$). Operating cities are Indian Tier 1 & 2 hubs. | Formally reported in **Monetary Units (MU)**. Operationally implies INR, but requires formal ERP verification. |

---

## 5. Statistical Methodology & Hypothesis Testing Matrix

All tests were conducted at a significance level of **$\alpha = 0.05$**. Because `Total_Sales` is right-skewed (Shapiro-Wilk $W = 0.9064, p < 0.001$; Skewness $= +0.988$), parametric procedures were systematically paired with non-parametric equivalents. Equal variances were confirmed via Levene's test ($p = 0.4182$).

| Hypothesis ID & Scope | Null Hypothesis ($H_0$) | Statistical Test | Test Statistic | P-Value | Effect Size | Statistical Decision | Key Business Implication |
| :--- | :--- | :--- | :---: | :---: | :---: | :---: | :--- |
| **H1: Category Spend Parity** | $\mu_{\text{Elec}} = \mu_{\text{Edu}} = \dots = \mu_{\text{Fash}}$ | One-Way ANOVA & Kruskal-Wallis | $F = 0.7171$<br/>$H = 2.0066$ | 0.5803<br/>*(KW: 0.7345)* | $\eta^2 = 0.00288$<br/>*(Negligible)* | **Fail to Reject $H_0$** | Basket values are statistically identical across all categories (~127k–145k MU). Electronics revenue dominance is 100% volume-driven. |
| **H2: Category Demand Skew** | Records are uniformly distributed ($E_i = 200$) | Chi-Square Goodness-of-Fit | $\chi^2 = 150.1300$<br/>*(df = 4)* | **$1.91 \times 10^{-31}$**<br/>*(p < 0.001)* | Cohen's $w = 0.3875$<br/>*(Med-Large)* | **Reject $H_0$** | Record frequency deviates from equal 200 rows (Elec: 354). Does NOT independently prove consumer demand or preference. |
| **H3: Age-Group Spend Parity** | $\mu_{18-25} = \mu_{26-35} = \dots = \mu_{56-65}$ | One-Way ANOVA & Kruskal-Wallis | $F = 0.0974$<br/>$H = 1.4435$ | 0.9833<br/>*(KW: 0.8366)* | $\eta^2 = 0.00039$<br/>*(Zero effect)* | **Fail to Reject $H_0$** | Average transaction spend is invariant across age cohorts (~135k–141k MU). Revenue differences reflect cohort counts, not basket size. |
| **H4A: Gender Spend Parity** | $\mu_{\text{Male}} = \mu_{\text{Female}}$ | Welch's t-test & Mann-Whitney U | $t = 0.6826$<br/>$U = 127,014$ | 0.4950<br/>*(MWU: 0.6496)* | Cohen's $d = 0.0431$<br/>*(Trivial)* | **Fail to Reject $H_0$** | Male (141.8k MU) and Female (136.9k MU) shoppers display statistically identical basket spend. 95% CI of difference includes zero. |
| **H4B: Gender Category Choice** | Category choice is independent of Gender | Chi-Square Independence | $\chi^2 = 9.0936$<br/>*(df = 4)* | 0.0588<br/>*(Borderline)* | Cramer's $V = 0.0954$<br/>*(Small)* | **Fail to Reject $H_0$ at $\alpha=0.05$** | Category selection is independent at 5% significance, with a slight directional tendency: Males skew toward Electronics; Females toward Education. |
| **H5: Regional Sales Parity** | $\mu_{\text{Patna}} = \dots = \mu_{\text{Gaya}}$ | One-Way ANOVA & Kruskal-Wallis | $F = 0.9010$<br/>$H = 5.9301$ | 0.5048<br/>*(KW: 0.5479)* | $\eta^2 = 0.00640$<br/>*(Negligible)* | **Fail to Reject $H_0$** | Spend per transaction is uniform across all 8 operating cities (~123k–154k MU). Patna leads total revenue due to order count (135). |

### Verified 95% Confidence Intervals for Category Mean Sales:
- **Grocery:** $145,305.30 \pm 18,355.34 \rightarrow [126,949.97, 163,660.64]\text{ MU}$
- **Electronics:** $143,442.32 \pm 12,292.83 \rightarrow [131,149.49, 155,735.15]\text{ MU}$
- **Education:** $140,627.47 \pm 17,284.47 \rightarrow [123,343.00, 157,911.93]\text{ MU}$
- **Furniture:** $135,355.73 \pm 17,394.13 \rightarrow [117,961.61, 152,749.86]\text{ MU}$
- **Fashion:** $127,153.18 \pm 16,630.19 \rightarrow [110,522.99, 143,783.37]\text{ MU}$
*(All five intervals overlap heavily, demonstrating absence of category pricing power).*

---

## 6. Commercial Insights: The Volume vs. Value Fallacy

1. **Category Volume Dominance:** Electronics accounts for 36.43% of revenue ($50.78\text{M MU}$) because it commands 354 transaction records (35.40% row share), whereas Fashion commands only 156 records ($19.84\text{M MU}$). However, average transaction value is virtually identical ($p = 0.5803$). Electronics is an **acquisition engine**, not a premium basket driver.
2. **Chi-Square Interpretation Guardrail:** Rejection of $H_0$ in the Chi-Square test ($\chi^2 = 150.1300, p < 0.001$) confirms that observed record counts deviate significantly from the equal-frequency expectation of 200 rows per category. It does **NOT** independently establish customer preference or organic demand, as observed records may be shaped by platform inventory stocking, catalog visibility, or supplier contracts.
3. **Product Roles:** **Laptop** is the primary revenue anchor ($25.44\text{M MU}$; 970 units), while **Mobile** is the primary volume driver ($1,008\text{ units}$; $25.34\text{M MU}$).
4. **Demographic Spend Invariance:** Customer age and gender show zero correlation with spend ($r = 0.001, p = 0.967$). The 36–45 cohort leads revenue purely due to having 223 transactions. Age-tiered pricing is commercially unsupported.
5. **Geographic Parity:** Operating cities exhibit identical average basket spend ($p = 0.5048$). Regional expansion should focus on order volume acquisition rather than regional price discrimination.

---

## 7. Actionable Strategic Recommendations

- **Pillar 1: Cross-Category Merchandising:** Embed cross-selling recommendation carousels into Mobile and Laptop checkout funnels to bundle non-electronic items (Ergonomic Chairs, Books) and lift AOV above the 140k baseline.
- **Pillar 2: Behavioral RFM Segmentation:** Retire demographic age-tiered campaigns. Deploy an automated RFM (Recency, Frequency, Monetary) segmentation model to reward repeat buyers.
- **Pillar 3: Inventory Optimization:** Prioritize safety stock for Mobile and Laptop in high-volume regional hubs (Patna, Kolkata, Bengaluru). Audit the 13 unassigned 'Unknown' city records ($1.54\text{M MU}$).
- **Pillar 4: Enterprise Data Quality:** Enforce composite primary keys (`Customer_ID` + Phone/Email) to resolve the 47 customer ID collisions, eliminate duplicate order IDs in upstream ERPs, and enforce calendar-complete reporting windows.

---

## 8. Final Project Deliverables & Submission Structure

```text
ApexPlanet_Task4_Final_Submission/
│
├── Task4_Statistical_Analysis.ipynb      # Executed Jupyter Notebook with clean relative paths & no local paths
├── Task4_Statistical_Summary.pdf         # Comprehensive executive PDF report (17 formal sections)
├── Task4_Final_Presentation.pptx         # 15-Slide executive PowerPoint presentation
├── Task4_Hypothesis_Testing_Summary.xlsx # Multi-tab Excel model (Hypotheses, Descriptives, Audit)
├── requirements.txt                      # Complete list of Python library dependencies
├── README.md                             # Project overview, methodology, audit trail & instructions
│
├── data/                                 # Relative project dataset storage
│   ├── apexplanet_cleaned_sales.xlsx     # Primary analytical dataset (1,000 records)
│   └── ApexPlanet_DataAnalytics_Dataset.xlsx # Original raw dataset (1,000 records)
│
└── Task4_Visualizations/                 # High-Resolution Visualizations (300 DPI PNGs)
    ├── sales_performance.png             # Overall transaction sales distribution
    ├── category_analysis.png             # Category Revenue Share vs Row Share
    ├── product_analysis.png              # Product Revenue vs Quantity sold
    ├── city_analysis.png                 # Geographic revenue performance
    ├── customer_analysis.png             # Age-group revenue vs AOV
    ├── monthly_trends.png                # Monthly revenue trajectory (highlighting partial Jan 2026)
    ├── hypothesis_test_1.png             # Hypothesis 1: Category Mean Spend 95% CIs
    ├── hypothesis_test_2.png             # Hypothesis 2: Category Frequency Chi-Square Goodness of Fit
    ├── hypothesis_test_3.png             # Hypothesis 3: Age-Group Mean Spend 95% CIs
    ├── 01_overall_sales_distribution.png # Numbered variant
    ├── 02_monthly_sales_trend.png        # Numbered variant
    ├── 03_category_revenue_and_volume.png# Numbered variant
    ├── 04_product_performance_revenue_vs_quantity.png
    ├── 05_city_sales_performance.png
    ├── 06_customer_age_group_revenue_and_aov.png
    ├── 07_gender_spending_and_category_preference.png
    ├── 08_correlation_matrix_heatmap.png
    ├── 09_hypothesis1_category_sales_ci.png
    ├── 10_hypothesis2_category_frequency_gof.png
    ├── 11_hypothesis3_age_group_ci.png
    └── 12_hypothesis4_gender_sales_diff.png
```

---

## 9. Reproduction Instructions

To reproduce the analysis and regenerate all deliverables from scratch:

```bash
# 1. Install required dependencies:
pip install -r ApexPlanet_Task4_Final_Submission/requirements.txt

# 2. Generate high-resolution visualizations:
python generate_charts.py

# 3. Generate the multi-tab Excel model:
python create_excel_summary.py

# 4. Generate the executive PowerPoint presentation:
python build_presentation.py

# 5. Generate the executive PDF report:
python build_pdf_report.py

# 6. Build and cleanly execute the Jupyter Notebook:
python create_and_execute_clean_notebook.py
```

---

## 10. Final Verification & Privacy Sign-Off

- **Path Privacy Verified:** All absolute paths, computer usernames, and local Windows user directories have been stripped from the executed notebook, scripts, and documentation. All data loading uses relative project paths (`data/apexplanet_cleaned_sales.xlsx`).
- **Data Integrity Verified:** All percentages, test statistics, degrees of freedom, p-values, confidence intervals, and effect sizes are calculated directly from the actual data without fabrication.
- **Source Repositories Preserved:** The original Task 1, 2, and 3 repositories remain clean and unmodified.
