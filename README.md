# AML Transaction Backlog – Russian Invasion Context

# Table of contents 

- [Background](#Background)
- [Project Objectives](#Project-Objectives)

# Background:

Following the Russian invasion of Ukraine, my client experienced a significant increase in flagged transactions due to enhanced sanctions and AML monitoring. This surge resulted in a transaction backlog that required immediate attention. The goal was to analyse the backlog, prioritise reviews, identify trends, and improve operational efficiency while ensuring compliance with global sanctions.
As a Data Analyst, my role was to analyse the backlog, detect patterns, and provide actionable insights to help the AML team prioritise high-risk cases and clear the backlog efficiently.

# Project Objectives:

1. Analyse and Prioritise Backlogged Transactions:

       * Identify high-risk transactions involving Russian entities, sanctioned individuals, or suspicious patterns.
2. Detect Patterns and Trends:

       * Uncover common characteristics in flagged transactions to refine screening processes and reduce false positives.
   
3. Enhance Efficiency:

       * Create dashboards and reports to help AML analysts and management track progress and focus on critical areas.

# Scope of Work:

1. Data Sources:

       * Transaction Monitoring System (TMS) alerts.
       * Sanctions screening results.
       * Customer and account data.
       * Payment metadata (e.g., timestamps, geolocations).
   
2. Key Fields to Analyse:

       * Counterparty details (e.g., country, bank, ultimate beneficiary).
       * Transaction amounts and frequencies.
       * Flags raised (e.g., sanctioned individual, unusual payment structure).
       * Customer risk ratings and KYB/KYC completeness.

# Responsibilities:   


## 1. Data Cleaning and Preparation

  Tool Used: Excel

-  Objective: Prepare raw data from Transaction Monitoring System (TMS) alerts, sanctions screening results, and customer/account datasets for analysis.

- Steps Taken:

   - Removed duplicates and filtered irrelevant alerts using Excel's Advanced Filters and Remove Duplicates function.

   - Standardized fields like country codes, transaction dates, and flag reasons using formulas such as TEXT, TRIM, and IFERROR.

   - Addressed missing or incomplete data by:

         * Filling gaps with estimated values (e.g., using averages).

         * Highlighting incomplete customer profiles for manual review using conditional formatting.

 ## 2. Data Analysis

  Tool Used: SQL

-  Objective: Query and analyze large datasets to uncover trends, prioritize high-risk transactions, and detect patterns.

-  SQL Queries Implemented:

  1. High-Risk Transaction Identification:

```sql
SELECT 
    t.transaction_id,
    t.customer_id,
    t.amount,
    t.counterparty_country,
    t.flag_reason,
    c.risk_rating
FROM 
    transactions t
JOIN 
    customers c ON t.customer_id = c.customer_id
WHERE 
    t.risk_level = 'High'
    AND (t.counterparty_country IN (SELECT sanctioned_country FROM sanctions_list)
         OR t.flag_reason LIKE '%Sanctioned Entity%');
```
-  Purpose: Identify flagged transactions linked to sanctioned entities or high-risk jurisdictions.

  2. Trend Analysis by Flag Reason:
 
```sql
SELECT 
    flag_reason,
    COUNT(*) AS total_flags,
    ROUND(AVG(amount), 2) AS avg_transaction_amount
FROM 
    transactions
GROUP BY 
    flag_reason
ORDER BY 
    total_flags DESC;
```
-  Purpose: Highlight the most common reasons for flagged transactions and their average amounts.

  3. Geographical Analysis:

```sql
SELECT 
    counterparty_country,
    COUNT(*) AS flagged_transactions,
    ROUND(SUM(amount), 2) AS total_amount
FROM 
    transactions
GROUP BY 
    counterparty_country
ORDER BY 
    flagged_transactions DESC;
```
-  Purpose: Determine high-risk geographies for flagged transactions.

## 3. Visualisation and Reporting

Tool Used: Power BI

- Objective: Create interactive dashboards to track progress, trends, and performance metrics.

- Dashboards Developed:

 1. Backlog Breakdown:

    - Visual: Pie chart showing the distribution of transactions by Risk_Level (High, Medium, Low).
   
    - KPI Metric: Total number of flagged transactions and cleared backlog percentages.
   
 2. Geographical Heatmap:

    - Visual: Interactive map highlighting flagged transactions by Counterparty_Country.
   
    - Purpose: Identify regions with the highest risk exposure.
   
 3. Trend Analysis:

    - Visual: Line chart showing the volume of flagged transactions over time.
   
    - Filters: Risk levels, flag reasons, and date ranges for drill-down analysis.
   
4.  Flag Reason Insights:

    - Visual: Bar chart displaying the frequency of each Flag_Reason and corresponding transaction amounts.
  
- Interactive Features:

   -  Slicers for filtering by Risk_Level, Country, and Date.
 
   -  Drill-through functionality to examine flagged transactions at a granular level.

 ## 4. Collaboration:

   - Worked closely with AML analysts and team leads to refine priority rules.
   - Provided data-driven recommendations to enhance TMS algorithms. 

Deliverables:

1. Reports:

   - Weekly updates on backlog status, including trends and patterns.
   - Risk-based segmentation to guide resource allocation.

2. Dashboards:
    * Live progress tracker for transaction reviews.
    * Heatmaps showing high-risk geographies or payment corridors.

3. Actionable Insights:
    * List of high-risk entities for immediate review.
    * Recommendations to reduce false positives (e.g., updating thresholds for specific flags)
  
# Example Workflow:

1. Step 1: Data Exploration
    * Query flagged transactions and customer data using SQL.
    * Identify common reasons for flags (e.g., sanctioned entity, large transaction, unusual patterns)

2. Step 2: Prioritisation Criteria
    * Develop a risk score for each transaction based on:
        * Amount: Higher amounts get more weight.
        * Counterparty Risk: Links to high-risk jurisdictions or sanctioned entities.
        * Transaction History: Unusual patterns compared to the customer’s normal behaviour.
     
3. Step 3: Visualise Trends
    * Use Excel and Power BI to plot:
        * Daily/weekly flag volumes.
        * Breakdown by risk levels or alert types.
        * Geographical trends (e.g., heat maps of Russian-linked transactions)
     
4. Step 4: Deliver Insights
    * Highlight critical areas needing attention, such as a spike in flagged transactions from specific regions.
    * Recommend adjustments to transaction monitoring rules to reduce false positives.

Challenges:
1. Incomplete Data: Some transactions lacked key details, requiring assumptions or data imputation.
2. Dynamic Risks: Constantly updating sanctions lists and evasion tactics by bad actors.
3. Scalability: Efficiently handling large datasets in Excel while providing actionable insights quickly.        

# Outcome

Using this technical stack, the AML team was able to:

- Focus on high-risk flagged transactions efficiently.

- Clear the backlog faster by prioritizing cases based on risk scores.

- Monitor compliance performance through interactive Power BI dashboards.
