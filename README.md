# AML Transaction Backlog - Russian Invasion Context 


---

## 🧰 Tech Stack
- Excel (Data Cleaning)
- SQL (Querrying, Transforming, and Analysing)
- Power BI (Visualising Data)

---

## Table of contents 

- [Background](#Background)
- [Project Objectives](#Project-Objectives)
- [Scope of Work](#Scope-of-Work)
- [Responsibilities](#Responsibilities)
- [Example Workflow](#Example-Workflow)
- [Challenges](#Challenges)
- [Outcome](#Outcome)
- [How to Run](#How-to-Run)
- [Employment Search Purpose](#Employment-Search-Purpose)
- [Contribution Guidelines](#Contribution-Guidelines)
- [License](#License)


## Background:

Following the Russian invasion of Ukraine, my client experienced a significant increase in flagged transactions due to enhanced sanctions and AML monitoring. This surge resulted in a transaction backlog that required immediate attention. The goal was to analyse the backlog, prioritise reviews, identify trends, and improve operational efficiency while ensuring compliance with global sanctions.
As a Data Analyst, my role was to analyse the backlog, detect patterns, and provide actionable insights to help the AML team prioritise high-risk cases and clear the backlog efficiently.

## Project Objectives:

1. Analyse and Prioritise Backlogged Transactions:

   - Identify high-risk transactions involving Russian entities, sanctioned individuals, or suspicious patterns.
2. Detect Patterns and Trends:

   - Uncover common characteristics in flagged transactions to refine screening processes and reduce false positives.
   
3. Enhance Efficiency:

   - Create dashboards and reports to help AML analysts and management track progress and focus on critical areas.

## Scope of Work:

1. Data Sources:

   - Transaction Monitoring System (TMS) alerts.
   - Sanctions screening results.
   - Customer and account data.
   - Payment metadata (e.g., timestamps, geolocations).
   
2. Key Fields to Analyse:

   - Counterparty details (e.g., country, bank, ultimate beneficiary).
   - Transaction amounts and frequencies.
   - Flags raised (e.g., sanctioned individual, unusual payment structure).
   - Customer risk ratings and KYB/KYC completeness.

## Responsibilities:   


![Excel to PowerBI workflow](./kaggle_to_powerbi.gif)


### 1. Data Cleaning and Preparation

  Tool Used: Excel

-  Objective: Prepare raw data from Transaction Monitoring System (TMS) alerts, sanctions screening results, and entity/account datasets for analysis.


This table contains information about transactions flagged for review
| Transaction_ID | Customer_ID | Date       | Amount    | Country | Flag_Reason       | Risk_Level |
|----------------|-------------|------------|-----------|---------|-------------------|------------|
| 1              | 101         | 2024-12-01 | 15000.50  | Russia  | Sanctioned Entity | High       |
| 2              | 102         | 2024-12-02 | 7500.00   | Belarus | Unusual Payment   | Medium     |
| 3              | 103         | 2024-12-03 | 20000.75  | Ukraine | High Volume       | High       |
| 4              | 104         | 2024-12-04 | 5000.00   | USA     | Round Number      | Low        |
| 5              | 105         | 2024-12-05 | 10000.00  | Russia  | Sanctioned Entity | High       |



This table lists sanctioned entities and their associated countries. 
| Entity_Name    | Country  |
|----------------|----------|
| VTB Bank       | Russia   |
| Alfa Group     | Belarus  |
| Gazprom        | Russia   |



This table includes details about entities associated with the flagged transactions.
| Entity_ID | Name              | KYC_Completed | Risk_Rating |
|-----------|-------------------|---------------|-------------|
| 101       | Acme Corp         | Yes           | High        |
| 102       | Global Ventures   | No            | Medium      |
| 103       | Apex Industries   | Yes           | High        |
| 104       | Horizon LLC       | Yes           | Low         |
| 105       | Titan Holdings    | No            | High        |





- Steps Taken:

   - Removed duplicates and filtered irrelevant alerts using Excel's Advanced Filters and Remove Duplicates function.

   - Standardized fields like country codes, transaction dates, and flag reasons using formulas such as TEXT, TRIM, and IFERROR.

   - Addressed missing or incomplete data by:

     - Filling gaps with estimated values (e.g., using averages).

     - Highlighting incomplete customer profiles for manual review using conditional formatting.

 ### 2. Data Analysis

  Tool Used: SQL

-  Objective: Query and analyze large datasets to uncover trends, prioritize high-risk transactions, and detect patterns.

-  SQL Queries Implemented:

  1. High-Risk Transaction Identification:

```sql
SELECT 
    t.transaction_id,
    t.customer_id,
    t.amount,
    t.country,
    t.flag_reason,
    c.risk_rating
FROM 
    transactions t
JOIN 
    customers c ON t.customer_id = c.customer_id
WHERE 
    t.risk_level = 'High'
    AND (t.country IN (SELECT sanctioned_country FROM sanctions_list)
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
    country,
    COUNT(*) AS flagged_transactions,
    ROUND(SUM(amount), 2) AS total_amount
FROM 
    transactions
GROUP BY 
    country
ORDER BY 
    flagged_transactions DESC;
```
-  Purpose: Determine high-risk geographies for flagged transactions.

### 3. Visualisation and Reporting

Tool Used: Power BI

- Objective: Created a dashboards to track progress, trends, and performance metrics.

- Dashboards Developed:

 1. Backlog Breakdown:

![testing the error page](./Distribution_Of_Transactions_By_Risk_Level.png)

    - Visual: Pie chart showing the distribution of transactions by Risk_Level (High, Medium, Low).

![testing the error page](./KPI_Dashboard.png)

    - KPI Metric: Total number of flagged transactions and cleared backlog percentages.
   
 2. Geographical Heatmap:

![Testing](./Visual_map_png.png) 

    - Visual: Static map highlighting flagged transactions by Counterparty_Country.
   
    - Purpose: Identify regions with the highest risk exposure.
   
 3. Trend Analysis:

![Testing](./Volume_Of_Flagged_Transactions_Over_Time.png)

    - Visual: Line chart showing the volume of flagged transactions over time.
   
   
4.  Flag Reason Insights:

![Checking and testing](./Matplotlib_Chart.png)

    - Visual: Bar chart displaying the frequency of each Flag_Reason and corresponding transaction amounts.
  
- Interactive Features:

   -  Slicers for filtering by Risk_Level, Country, and Date.
 
   -  Drill-through functionality to examine flagged transactions at a granular level.

 ### 4. Collaboration:

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
  
## Example Workflow:

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

## Challenges:
1. Incomplete Data: Some transactions lacked key details, requiring assumptions or data imputation.
2. Dynamic Risks: Constantly updating sanctions lists and evasion tactics by bad actors.
3. Scalability: Efficiently handling large datasets in Excel while providing actionable insights quickly.        

## Outcome

Using this technical stack, the AML team was able to:

- Focus on high-risk flagged transactions efficiently.

- Clear the backlog faster by prioritizing cases based on risk scores.

- Monitor compliance performance through interactive Power BI dashboards.

## How to Run

1. Clone the repository.

2. Set up the database and import the dataset.

3. Execute the provided SQL queries.

4. Review the output and reports.


## Employment Search Purpose

This project serves as a practical demonstration of my technical skills in data querying, financial analysis, and database management. It highlights my ability to extract actionable insights from complex datasets—skills highly relevant for data analyst and financial crime roles.

## Contribution Guidelines

- Fork the repository.

- Create a feature branch.

- Commit changes with clear messages.

- Submit a pull request.

## License

This project is licensed under the MIT License.


Disclaimer: This project is for educational and professional portfolio purposes, leveraging data provided by The World Bank.
