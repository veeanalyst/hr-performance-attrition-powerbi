# HR Employee Attrition & Performance Analysis | Power BI

![Power BI](https://img.shields.io/badge/Tool-Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![Dataset](https://img.shields.io/badge/Rows-50%2C020-blue)
![Fields](https://img.shields.io/badge/Fields-20-green)
![Status](https://img.shields.io/badge/Status-In%20Progress-orange)

## Overview
This project analyses HR employee attrition and performance data for 50,020 employees across multiple departments. The goal is to identify the key drivers of employee attrition, measure workforce performance, and provide actionable recommendations to improve employee retention and productivity.

---

## Business Questions
- What is the overall attrition rate and which departments are most affected?
- Does overtime correlate with higher attrition?
- Which job roles have the lowest performance ratings?
- How does monthly income vary across job levels and departments?
- Does work-life balance influence attrition and job satisfaction?
- What is the total cost of attrition to the organisation?
- Do employees who travel frequently show higher attrition rates?

---

## Dataset
| Field | Description |
|---|---|
| `employee_id` | Unique employee identifier |
| `age` | Employee age |
| `department` | Department the employee belongs to |
| `job_role` | Employee's specific role |
| `gender` | Employee gender |
| `monthly_income` | Monthly salary |
| `job_level` | Seniority level (1–5) |
| `years_at_company` | Total years with the company |
| `years_in_role` | Years in current role |
| `education_field` | Field of education |
| `business_travel` | Travel frequency |
| `marital_status` | Marital status |
| `work_life_balance` | Work-life balance score (1–4) |
| `job_satisfaction` | Job satisfaction score (1–4) |
| `performance_rating` | Performance rating (1–4) |
| `overtime` | Whether employee works overtime (Yes/No) |
| `num_companies_worked` | Number of previous companies |
| `training_times_last_year` | Number of training sessions attended |
| `attrition` | Whether employee left the company (Yes/No) |
| `attrition_cost` | Estimated cost of attrition per employee |

**Source:** HR Attrition Dirty Dataset  
**Rows:** 50,020 &nbsp;·&nbsp; **Fields:** 20  

---

## Data Cleaning (Power Query)

The raw dataset was cleaned in Power Query before loading into Power BI.

| Step | Transformation Applied |
|---|---|
| Removed empty rows | Deleted rows where all fields were blank |
| Removed duplicates | Based on `employee_id` column |
| Replaced values | Fixed embedded spaces e.g. `E M P00015` → `EMP00015` |
| Filtered invalid records | Removed rows with inconsistent or corrupt data |
| Validated employee_id | Kept only records with exactly 8 characters |

**Original dataset:** 50,020 rows  
**Clean dataset:** 49,977 rows  
**Records removed:** 43  

---

## DAX Measures
Key measures created in Power BI to support the analysis:

```dax
-- Total Employees
Total Employees = COUNTROWS('HR_Data')

-- Attrition Rate
Attrition Rate = 
DIVIDE(
    COUNTROWS(FILTER('HR_Data', 'HR_Data'[attrition] = "Yes")),
    COUNTROWS('HR_Data'),
    0
)

-- Total Attrition Cost
Total Attrition Cost = SUM('HR_Data'[attrition_cost])

-- Average Monthly Income
Avg Monthly Income = AVERAGE('HR_Data'[monthly_income])

-- Average Performance Rating
Avg Performance Rating = AVERAGE('HR_Data'[performance_rating])

-- Average Job Satisfaction
Avg Job Satisfaction = AVERAGE('HR_Data'[job_satisfaction])

-- Overtime Attrition Rate
Overtime Attrition Rate = 
CALCULATE(
    [Attrition Rate],
    'HR_Data'[overtime] = "Yes"
)
```
## DAX Measures

All measures are stored in a dedicated `_Measures` table and 
numbered for easy navigation.

| # | Measure | Description |
|---|---|---|
| 01 | Total Employees | Count of all employees in the dataset |
| 02 | Total Attritions | Count of employees who left the company |
| 03 | Attrition Rate | Percentage of employees who left |
| 04 | Total Attrition Cost | Sum of all attrition-related costs |
| 05 | Avg Monthly Income | Average monthly salary across all employees |
| 06 | Avg Performance Rating | Average performance rating (1–4 scale) |
| 07 | Avg Job Satisfaction | Average job satisfaction score (1–4 scale) |
| 08 | Overtime Attrition Rate | Attrition rate for employees working overtime |
---

## Dashboard Pages
The Power BI report contains the following pages:

| Page | Description |
|---|---|
| **Overview** | Total employees, attrition rate, cost of attrition, KPI cards |
| **Attrition Analysis** | Attrition by department, job role, overtime, business travel |
| **Performance & Satisfaction** | Performance ratings, job satisfaction, work-life balance scores |
| **Workforce Profile** | Age distribution, income by job level, years at company |

---

## Dashboard Preview
*Screenshots will be added once the dashboard is complete.*

<!-- Once dashboard is built, replace the line above with:
![Overview](screenshots/overview.png)
![Attrition Analysis](screenshots/attrition_analysis.png)
-->

---

## Key Findings
*To be updated after analysis is complete.*

- Finding 1
- Finding 2
- Finding 3

---

## Recommendations
*To be updated after analysis is complete.*

- Recommendation 1
- Recommendation 2
- Recommendation 3

---

## Tools Used
- **Power BI Desktop** — data modelling, DAX, dashboard
- **Power Query** — data cleaning and transformation
- **CSV** — raw data source (50,020 rows)
