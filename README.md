# Credit Risk & Loan Portfolio Monitoring Report Using Power BI

## Project Overview

This project is an interactive **Credit Risk & Loan Portfolio Monitoring Report** built using **Power BI**. The report analyzes borrower profiles, loan portfolio exposure, default risk, high-risk segments, and business recommendations to support better lending and credit approval decisions.

The main objective of this project is to convert raw credit risk data into a professional business intelligence report that helps financial institutions identify risky borrower segments, monitor portfolio exposure, and reduce loan default risk.

---

## Business Problem

Banks, NBFCs, fintech companies, and financial institutions need to monitor loan applications and borrower risk carefully to reduce defaults and maintain portfolio quality.

This report helps answer important business questions such as:

- What is the overall default rate of the loan portfolio?
- Which borrower segments are more likely to default?
- Which income groups, loan grades, and loan purposes carry higher risk?
- How does previous default history affect current default behavior?
- Which borrowers should be approved, reviewed, or strictly monitored?
- What business actions can reduce loan default risk?

---

## Dataset Used

**Dataset:** Credit Risk Dataset  
**Source:** Kaggle  
**File Used:** `credit_risk_dataset.csv`

The dataset contains borrower and loan-related information, including:

- Borrower age
- Annual income
- Home ownership
- Employment length
- Loan purpose
- Loan grade
- Loan amount
- Interest rate
- Loan status
- Loan-to-income ratio
- Previous default history
- Credit history length

---

## Tools & Technologies Used

- Power BI Desktop
- Power Query
- DAX
- Data Modeling
- Data Cleaning
- Interactive Visualizations
- Drill-through
- Report Page Tooltip
- Conditional Formatting

---

## Project Objective

The objective of this project is to build a professional Power BI report that can help a financial institution:

- Monitor total loan exposure
- Track default rate and high-risk borrowers
- Analyze borrower risk by income, age, ownership, loan grade, and previous default history
- Identify key default drivers
- Create risk-based loan approval recommendations
- Support better credit risk management decisions

---

## Data Cleaning & Preparation

Data cleaning and transformation were performed using **Power Query**.

Key cleaning steps included:

- Renamed columns into professional and readable names
- Corrected data types for numeric and categorical columns
- Removed full-row duplicate records
- Handled missing values
- Created readable loan status labels
- Created borrower segmentation columns
- Created risk categories based on loan-to-income ratio and previous default history
- Prepared the dataset for interactive Power BI reporting

---

## Column Renaming

| Original Column | Renamed Column |
|---|---|
| `person_age` | Age |
| `person_income` | Annual Income |
| `person_home_ownership` | Home Ownership |
| `person_emp_length` | Employment Length |
| `loan_intent` | Loan Purpose |
| `loan_grade` | Loan Grade |
| `loan_amnt` | Loan Amount |
| `loan_int_rate` | Interest Rate |
| `loan_status` | Loan Status |
| `loan_percent_income` | Loan to Income Ratio |
| `cb_person_default_on_file` | Previous Default |
| `cb_person_cred_hist_length` | Credit History Length |

---

## Calculated Columns Created

The following calculated columns were created to improve borrower segmentation and risk analysis:

- Loan Status Label
- Risk Category
- Age Group
- Income Group
- Loan Amount Group

### Risk Category Logic

```DAX
Risk Category = 
SWITCH(
    TRUE(),
    'Credit Risk'[Loan to Income Ratio] >= 0.35 
        || 'Credit Risk'[Previous Default] = "Y", "High Risk",
    'Credit Risk'[Loan to Income Ratio] >= 0.20, "Medium Risk",
    "Low Risk"
)
```

### Age Group

```DAX
Age Group = 
SWITCH(
    TRUE(),
    'Credit Risk'[Age] < 25, "Below 25",
    'Credit Risk'[Age] >= 25 && 'Credit Risk'[Age] < 35, "25-34",
    'Credit Risk'[Age] >= 35 && 'Credit Risk'[Age] < 45, "35-44",
    'Credit Risk'[Age] >= 45 && 'Credit Risk'[Age] < 60, "45-59",
    "60+"
)
```

### Income Group

```DAX
Income Group = 
SWITCH(
    TRUE(),
    'Credit Risk'[Annual Income] < 30000, "Low Income",
    'Credit Risk'[Annual Income] >= 30000 
        && 'Credit Risk'[Annual Income] < 70000, "Middle Income",
    'Credit Risk'[Annual Income] >= 70000 
        && 'Credit Risk'[Annual Income] < 120000, "High Income",
    "Very High Income"
)
```

### Loan Amount Group

```DAX
Loan Amount Group = 
SWITCH(
    TRUE(),
    'Credit Risk'[Loan Amount] < 5000, "Small Loan",
    'Credit Risk'[Loan Amount] >= 5000 && 'Credit Risk'[Loan Amount] < 15000, "Medium Loan",
    'Credit Risk'[Loan Amount] >= 15000 && 'Credit Risk'[Loan Amount] < 30000, "Large Loan",
    "Very Large Loan"
)
```

---

## Key DAX Measures

```DAX
Total Applicants = COUNTROWS('Credit Risk')
```

```DAX
Total Loan Amount = SUM('Credit Risk'[Loan Amount])
```

```DAX
Average Loan Amount = AVERAGE('Credit Risk'[Loan Amount])
```

```DAX
Default Customers = 
CALCULATE(
    COUNTROWS('Credit Risk'),
    'Credit Risk'[Loan Status Label] = "Default"
)
```

```DAX
Default Rate = 
DIVIDE(
    [Default Customers],
    [Total Applicants]
)
```

```DAX
High Risk Customers = 
CALCULATE(
    COUNTROWS('Credit Risk'),
    'Credit Risk'[Risk Category] = "High Risk"
)
```

```DAX
High Risk Percentage = 
DIVIDE(
    [High Risk Customers],
    [Total Applicants]
)
```

```DAX
Average Loan to Income Ratio = 
AVERAGE('Credit Risk'[Loan to Income Ratio])
```

```DAX
Average Interest Rate % = 
DIVIDE(
    AVERAGE('Credit Risk'[Interest Rate]),
    100
)
```

---

## Report Structure

The Power BI report contains **4 main visible report pages** and advanced hidden pages for drill-through and tooltip analysis.

### Visible Report Pages

1. Executive Overview
2. Borrower & Risk Segmentation
3. Default & Loan Portfolio Analysis
4. Credit Risk Insights & Business Recommendations

### Advanced Hidden Pages

- Borrower Detail Drillthrough
- Risk Tooltip

---

# Report Pages

## 1. Executive Overview

The Executive Overview page provides a high-level summary of the loan portfolio and overall credit risk position.

### KPIs Displayed

- Total Applicants: **32K**
- Total Loan Amount: **310.99M**
- Average Loan Amount: **9,594**
- Default Customers: **7,089**
- Default Rate: **21.9%**
- High Risk Percentage: **23.7%**

### Visuals Used

- Loan Status Distribution
- Borrower Distribution by Risk Category
- Default Rate by Loan Grade
- Loan Exposure by Loan Purpose
- Report Filters for Loan Purpose, Loan Grade, Risk Category, and Home Ownership

### Executive Insight

The loan portfolio contains around **32K applicants** with total loan exposure of approximately **310.99M**. The overall default rate is **21.9%**, while **23.7%** of borrowers fall into the high-risk category. Loan grades **G, F, E, and D** show significantly higher default rates, indicating that weaker loan grades require stricter approval checks. Education, medical, and venture loans represent the highest loan exposure and should be monitored closely.

---

## 2. Borrower & Risk Segmentation

This page analyzes borrower risk across different customer segments such as age group, income group, home ownership, and risk category.

### KPIs Displayed

- Total Applicants: **32K**
- Default Rate: **21.9%**
- High Risk Customers: **7,678**
- Average Loan-to-Income Ratio: **17.0%**

### Visuals Used

- Default Rate by Age Group
- Default Rate by Income Group
- Default Rate by Home Ownership
- Income vs Loan Amount by Risk Category
- Risk Segmentation Matrix

### Key Findings

- Low-income borrowers show the highest default rate at **47.20%**.
- Middle-income borrowers show a default rate of **23.52%**.
- High-income borrowers show a default rate of **11.62%**.
- Very high-income borrowers show the lowest default rate at **8.36%**.
- High-risk borrowers show a default rate of **45.75%**.
- Low-risk borrowers show a much lower default rate of **10.38%**.
- Medium-risk borrowers show a default rate of **24.92%**.

### Risk Segmentation Insight

Low-income borrowers show the highest default risk, indicating strong repayment pressure in this segment. High-risk borrowers have a much higher default rate compared to low-risk borrowers. The analysis shows that income level, home ownership, loan-to-income ratio, and previous default history are important factors in borrower risk evaluation.

---

## 3. Default & Loan Portfolio Analysis

This page focuses on identifying the main drivers of loan default risk across loan grades, loan purposes, previous default history, interest rate, and borrower-level details.

### KPIs Displayed

- Default Customers: **7,089**
- Default Rate: **21.9%**
- Average Interest Rate: **11.02%**
- Average Loan-to-Income Ratio: **17.0%**

### Visuals Used

- Default Rate by Loan Grade
- Default Rate by Loan Purpose
- Default Rate by Previous Default History
- Loan Amount vs Interest Rate by Loan Status
- High-Risk Borrower Monitoring Table

### Key Findings

- Loan Grade G has the highest default rate at **98.44%**.
- Loan Grade F has a default rate of **70.54%**.
- Loan Grade E has a default rate of **64.49%**.
- Loan Grade D has a default rate of **59.06%**.
- Borrowers with previous default history show a default rate of **37.87%**.
- Borrowers without previous default history show a default rate of **18.43%**.
- Loan purposes such as debt consolidation, medical, and home improvement show higher default concentration.

### Default Analysis Insight

Default risk is highest among weaker loan grades, especially Grade G, F, E, and D. Borrowers with previous default history are almost twice as likely to default compared to borrowers without previous default history. Loan grade, previous default history, interest rate, and loan-to-income ratio are strong indicators of borrower default risk.

---

## 4. Credit Risk Insights & Business Recommendations

This page converts the analysis into practical business recommendations for better loan approval and portfolio monitoring.

### KPIs Displayed

- Total Applicants: **32K**
- Default Rate: **21.9%**
- High Risk Customers: **7,678**
- High Risk Percentage: **23.7%**

---

## Key Findings

1. The overall default rate is **21.9%**, which means nearly 1 out of every 5 borrowers is defaulting.

2. High-risk borrowers have the highest default rate at **45.75%**, compared to only **10.38%** for low-risk borrowers.

3. Low-income borrowers show the highest default rate at **47.20%**, indicating repayment pressure in this segment.

4. Loan grades **G, F, E, and D** show very high default rates and require stricter approval checks.

5. Borrowers with previous default history show a default rate of **37.87%**, almost double compared to borrowers without previous default history.

6. Debt consolidation, medical, and home improvement loans should be monitored closely because they show higher default concentration.

---

## Business Recommendations

1. Apply stricter approval rules for high-risk borrowers.

2. Reduce loan limits for low-income borrowers with high loan-to-income ratios.

3. Review loan grades D, E, F, and G carefully before approval.

4. Treat previous default history as a strong warning signal during loan evaluation.

5. Monitor debt consolidation, medical, and home improvement loans more closely.

6. Use income group, loan grade, previous default history, and risk category together for better credit decisions.

---

## Risk-Based Loan Approval Policy

| Risk Category | Suggested Decision | Action |
|---|---|---|
| Low Risk | Approve | Normal approval process |
| Medium Risk | Review | Check income, loan ratio, and credit history |
| High Risk | Strict Review | Reduce loan limit, request documents, or reject |

---

## Final Business Conclusion

The credit risk report shows that borrower default risk is strongly influenced by income level, risk category, loan grade, previous default history, and loan-to-income ratio. A risk-based approval policy can help reduce default exposure, improve portfolio quality, and support better lending decisions.

---

## Advanced Power BI Features Used

This report includes several advanced Power BI features:

- Interactive slicers
- KPI cards
- Drill-through page for borrower-level detail
- Report page tooltip
- Conditional formatting
- Risk segmentation matrix
- High-risk borrower monitoring table
- Navigation buttons
- Business recommendation page
- Clean professional report layout

---

## Dashboard Screenshots


### Executive Overview

![Executive Overview](https://github.com/mukul816/Credit-Risk-Loan-Portfolio-PowerBI-Report/blob/main/Credit%20Risk%20Loan%20Portfolio%20Report/Screenshots/Executive%20%20Overview.png)

### Borrower & Risk Segmentation

![Risk Segmentation](https://github.com/mukul816/Credit-Risk-Loan-Portfolio-PowerBI-Report/blob/main/Credit%20Risk%20Loan%20Portfolio%20Report/Screenshots/Risk%20Segmentation.png)

### Default & Loan Portfolio Analysis

![Default Analysis](https://github.com/mukul816/Credit-Risk-Loan-Portfolio-PowerBI-Report/blob/main/Credit%20Risk%20Loan%20Portfolio%20Report/Screenshots/Default%20Analysis.png)

### Credit Risk Insights & Business Recommendations

![Recommendations](https://github.com/mukul816/Credit-Risk-Loan-Portfolio-PowerBI-Report/blob/main/Credit%20Risk%20Loan%20Portfolio%20Report/Screenshots/Recommendations.png)

---


## How to Use This Project

1. Download or clone this repository.
2. Open the `.pbix` file using Power BI Desktop.
3. Use slicers to filter the report by loan purpose, loan grade, risk category, and home ownership.
4. Hover over visuals to view tooltip insights.
5. Use drill-through to analyze borrower-level details.
6. Review the recommendation page for business actions.

---

## Skills Demonstrated

- Power BI Report Development
- Credit Risk Analysis
- Loan Portfolio Monitoring
- Borrower Segmentation
- Default Risk Analysis
- DAX Measures
- Power Query Data Cleaning
- KPI Reporting
- Conditional Formatting
- Drill-through Reporting
- Report Page Tooltip
- Business Intelligence
- Data Storytelling
- Risk-Based Decision Making

---

## Project Outcome

This project demonstrates how Power BI can be used to build a complete credit risk monitoring report that supports data-driven lending decisions.

The report helps identify:

- High-risk borrowers
- Risky loan grades
- High-default income segments
- Impact of previous default history
- Loan purposes requiring closer monitoring
- Risk-based approval actions

By combining analytics, visualization, and business recommendations, this project reflects a practical credit risk reporting solution suitable for banking, NBFC, fintech, and financial analytics use cases.

---
