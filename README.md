# 🏦 Bank Loan Approval & Credit Risk Analytics Dashboard

An interactive **Power BI dashboard** for analyzing bank loan applications, approval and rejection patterns, applicant characteristics, and project-defined credit risk categories.

This project demonstrates an end-to-end **data analytics and business intelligence workflow**, including data preprocessing, data transformation, feature engineering, DAX calculations, interactive visualization, and advanced Power BI analytics.

---

## 📌 Project Overview

Loan approval analysis involves understanding different applicant and loan-related characteristics such as income, employment status, education, credit history, age, property area, and requested loan amount.

The objective of this project is to transform raw loan application data into an interactive analytical dashboard that helps users explore:

- Overall loan application performance
- Approved and rejected applications
- Applicant demographics and characteristics
- Employment and education patterns
- Credit history and loan approval patterns
- Income and loan amount relationships
- Loan-to-income patterns
- Project-defined risk distribution
- Factors associated with loan approval and rejection

The final dashboard consists of **three interactive Power BI pages**.

---

# 🎯 Project Objectives

The main objectives of this project are:

1. Analyze the overall loan application portfolio.
2. Calculate and visualize loan approval and rejection rates.
3. Understand applicant characteristics.
4. Analyze loan applications across employment and education categories.
5. Examine the relationship between credit history and loan approval.
6. Analyze loan amount relative to applicant income.
7. Create analytical income, age, loan amount, and risk groups.
8. Identify patterns associated with loan rejection.
9. Explore factors associated with loan approval.
10. Build an interactive and user-friendly Power BI dashboard.

---

# 📂 Dataset

The project uses a loan application dataset containing information about individual applicants and their loan applications.

### Dataset Size

| Property | Value |
|---|---:|
| Records | 3,192 |
| Columns | 14 |

### Dataset Attributes

| Column | Description |
|---|---|
| `Loan_ID` | Unique loan application identifier |
| `Gender` | Applicant gender |
| `Married` | Applicant marital status |
| `Dependents` | Number of dependents |
| `Education` | Applicant education level |
| `Employment_Status` | Applicant employment category |
| `Applicant_Income` | Applicant income |
| `Coapplicant_Income` | Coapplicant income |
| `Loan_Amount` | Requested loan amount |
| `Loan_Term` | Loan repayment term |
| `Credit_History` | Credit history indicator |
| `Property_Area` | Property location category |
| `Age` | Applicant age |
| `Loan_Status` | Loan approval status |

### Target / Outcome Variable

The primary outcome variable used for analysis is:

`Loan_Status`

Possible values:

- `Approved`
- `Rejected`

---

# 🧹 Data Preprocessing

Before creating the dashboard, the raw dataset was inspected and prepared for analysis.

The preprocessing stage included:

- Inspecting the dataset structure
- Checking for missing values
- Reviewing data types
- Checking categorical variables
- Checking consistency of categorical values
- Preparing the dataset for Power BI
- Creating analytical fields for visualization

Missing values were identified in fields including:

- Gender
- Married
- Education
- Employment Status
- Applicant Income
- Loan Amount

The data was prepared appropriately before performing the dashboard analysis.

---

# ⚙️ Feature Engineering

To make the analysis more meaningful, several calculated fields were created in Power BI.

---

## 1. Total Income

Total Income combines the applicant's income and coapplicant's income.

```DAX
Total Income =
Loans[Applicant_Income] + Loans[Coapplicant_Income]

This field is used in income-based analysis and the loan-to-income calculation.

2. Income Group

Applicants were grouped into three income categories:

Low Income
Medium Income
High Income
Income Group =
SWITCH(
    TRUE(),
    Loans[Total Income] < 50000, "Low Income",
    Loans[Total Income] < 100000, "Medium Income",
    "High Income"
)
3. Age Group

Applicants were categorized into different age groups.

Age Group =
SWITCH(
    TRUE(),
    Loans[Age] < 25, "Under 25",
    Loans[Age] < 35, "25-34",
    Loans[Age] < 45, "35-44",
    Loans[Age] < 55, "45-54",
    "55+"
)

The age groups are:

Under 25
25–34
35–44
45–54
55+
4. Loan Amount Group

Loan amounts were divided into different ranges.

Loan Amount Group =
SWITCH(
    TRUE(),
    Loans[Loan_Amount] < 100000, "Below 100K",
    Loans[Loan_Amount] < 200000, "100K-200K",
    Loans[Loan_Amount] < 300000, "200K-300K",
    "300K+"
)
5. Loan-to-Income Ratio

The Loan-to-Income Ratio compares the requested loan amount with the applicant's total income.

Loan to Income Ratio =
DIVIDE(
    Loans[Loan_Amount],
    Loans[Total Income],
    0
)

This metric helps analyze the requested loan amount relative to the applicant's income.

6. Credit History Category

The original binary credit history field was transformed into more understandable categories.

Credit History Category =
IF(
    Loans[Credit_History] = 1,
    "Positive Credit History",
    "Negative Credit History"
)
7. Risk Category

A project-defined analytical risk classification was created using credit history and loan-to-income ratio.

Risk Category =
SWITCH(
    TRUE(),
    Loans[Credit_History] = 0, "High Risk",
    Loans[Loan to Income Ratio] > 5, "Medium Risk",
    "Lower Risk"
)
Risk Categories
Category	Project Definition
Lower Risk	Positive credit history and lower loan-to-income ratio
Medium Risk	Positive credit history with higher loan-to-income ratio
High Risk	Negative credit history

Important: The Risk Category is a project-defined analytical classification created for this project. It is not an official banking credit score, financial risk rating, or default prediction model.

📐 DAX Measures

Several DAX measures were created to calculate the dashboard KPIs and analytical metrics.

Total Applications
Total Applications =
COUNTROWS(Loans)
Approved Loans
Approved Loans =
CALCULATE(
    [Total Applications],
    Loans[Loan_Status] = "Approved"
)
Rejected Loans
Rejected Loans =
CALCULATE(
    [Total Applications],
    Loans[Loan_Status] = "Rejected"
)
Approval Rate
Approval Rate =
DIVIDE(
    [Approved Loans],
    [Total Applications],
    0
)
Total Loan Amount
Total Loan Amount =
SUM(Loans[Loan_Amount])
Average Loan Amount
Average Loan Amount =
AVERAGE(Loans[Loan_Amount])
Average Income
Average Income =
AVERAGE(Loans[Total Income])
Average Age
Average Age =
AVERAGE(Loans[Age])
High Risk Applications
High Risk Applications =
CALCULATE(
    [Total Applications],
    Loans[Risk Category] = "High Risk"
)
📊 Dashboard Structure

The Power BI report contains three interactive pages.

🟦 Page 1 — Loan Portfolio Overview
Objective

The first page provides a high-level overview of the loan application portfolio.

It answers:

How is the overall loan application portfolio performing?

🎛️ Interactive Slicers

The page includes five interactive slicers:

Gender
Education
Employment Status
Property Area
Credit History Category

These slicers dynamically filter the dashboard visuals.

📊 KPI Cards

The following KPIs are displayed:

Total Applications
Approved Loans
Rejected Loans
Approval Rate
Total Loan Amount

These KPIs provide a quick overview of portfolio performance.

🍩 Loan Application Status

A donut chart showing the overall distribution of:

Approved applications
Rejected applications

This provides an immediate view of the portfolio's approval and rejection distribution.

👔 Applications by Employment Status

A column chart showing application volume across:

Salaried
Self-Employed
Unemployed

This helps identify the employment categories contributing the largest number of applications.

🏠 Applications by Property Area

A bar chart comparing applications across:

Rural
Semiurban
Urban

This provides an overview of the geographical distribution of applications based on property area.

🎓 Loan Approval by Education

A stacked column chart comparing:

Graduate
Not Graduate

The chart further divides each education category into:

Approved
Rejected

This allows comparison of approval patterns across education groups.

💰 Loan Approval by Income Group

A stacked column chart comparing approval patterns across:

Low Income
Medium Income
High Income

This helps analyze how approval outcomes are distributed across income groups.

🟧 Page 2 — Credit Risk & Applicant Analysis
Objective

The second page provides deeper analysis of applicant characteristics and project-defined risk patterns.

It answers:

What applicant characteristics and factors are associated with loan rejection and higher project-defined risk?

📊 KPI Cards

The page contains:

High Risk Applications
Average Income
Average Loan Amount
Average Applicant Age

These provide a quick summary of applicant and risk-related metrics.

💳 Credit History vs Loan Approval

A stacked column chart comparing:

Positive Credit History
Negative Credit History

against:

Approved
Rejected

This helps identify patterns between credit history and observed loan approval outcomes.

⚠️ Risk Distribution

A donut chart showing the distribution of applications across:

Lower Risk
Medium Risk
High Risk

The visualization provides an overall view of the project-defined risk composition.

👥 Approval by Age Group

A stacked column chart analyzing loan approval across:

Under 25
25–34
35–44
45–54
55+

Each group is divided into approved and rejected applications.

👔 Approval by Employment

A stacked column chart comparing loan approval patterns across:

Salaried
Self-Employed
Unemployed
🔵 Loan Amount vs Total Income

A scatter plot comparing:

X-axis: Total Income

Y-axis: Loan Amount

Each point represents an individual loan application.

Loan Status is used to visually distinguish approved and rejected applications.

This visualization helps explore the relationship between applicant income and requested loan amount.

📈 Loan-to-Income Ratio by Risk

A column chart comparing the average Loan-to-Income Ratio across:

Lower Risk
Medium Risk
High Risk

This helps analyze borrowing relative to income across the project-defined risk groups.

📋 Applicant Analysis Matrix

A detailed matrix containing:

Employment Status
Education
Total Applications
Approved Loans
Rejected Loans
Approval Rate
Average Loan Amount

The matrix provides a detailed numerical view of applicant segments.

🟪 Page 3 — Advanced Loan Risk Analysis
Objective

The third page focuses on advanced Power BI visuals and interactive analytical exploration.

It answers:

Which factors and applicant segments are associated with observed loan approval and rejection patterns?

🎯 Overall Loan Approval Rate

A Gauge visual showing the overall loan approval rate.

The gauge provides a quick visual representation of the proportion of applications that were approved.

🌳 Applicant Distribution by Employment & Education

A Treemap showing applicant distribution using:

Employment Status
Education

The size of each section represents application volume.

This makes it easy to identify major applicant segments.

🏠 Loan Approval by Property Area

A stacked column chart comparing:

Rural
Semiurban
Urban

Each category is divided into:

Approved
Rejected

This provides another perspective on observed approval patterns across property areas.

🔍 Decomposition of Rejected Applications

A Decomposition Tree is used to analyze rejected applications.

The analysis can be broken down by:

Credit History Category
Employment Status
Education
Property Area
Age Group
Income Group
Loan Amount Group

The interactive drill-down capability allows users to explore different combinations of applicant characteristics and identify segments contributing to observed rejected applications.

🧠 Key Influencers

The Key Influencers visual analyzes Loan_Status using applicant characteristics such as:

Credit History Category
Employment Status
Education
Property Area
Age Group
Income Group
Loan Amount Group

This visual helps identify characteristics that are associated with differences in observed loan approval outcomes.

Note: Key Influencers identifies statistical associations in the dataset. These relationships should not be interpreted as proof of causation.

🎛️ Interactive Dashboard Features

The dashboard includes:

Interactive Slicers

Users can filter the dashboard by:

Gender
Education
Employment Status
Property Area
Credit History Category
Cross-Filtering

Selecting elements in one visual dynamically filters other related visuals.

Drill-Down Analysis

The Decomposition Tree allows users to explore rejection patterns through multiple dimensions.

Key Influencers

Power BI's Key Influencers visual provides an advanced method of exploring factors associated with loan status.

Page Navigation

A Page Navigator allows users to move between all three dashboard pages.

visuals

📊 Project Highlights
✅ 3-page interactive Power BI dashboard
✅ Data preprocessing
✅ Data transformation
✅ Feature engineering
✅ DAX calculated columns
✅ DAX measures
✅ Interactive slicers
✅ KPI analysis
✅ Applicant segmentation
✅ Risk-oriented analysis
✅ Scatter plot analysis
✅ Decomposition Tree
✅ Key Influencers
✅ Treemap
✅ Gauge
✅ Interactive page navigation


👩‍💻 Author
Shreyasi Gidmare
B.Tech — Computer Science & Engineering
Honours in Artificial Intelligence & Machine Learning
