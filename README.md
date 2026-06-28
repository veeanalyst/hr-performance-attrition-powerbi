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
| Removed leading and trailing whitespaces | Formatted all the cells in each column using TRIM |
| Removed invalid employee records | e.g 'employee_id' 12345
| Replaced values | Fixed embedded spaces e.g. `E M P00015` → `EMP00015` |
| Filtered invalid records | Removed rows with inconsistent or corrupt data |
| Validated employee_id | Kept only records with exactly 8 characters |
| Removed duplicates | Based on `employee_id` column |
| Replaced values | Based on 'attrition' column e.g 0=No, 1=Yes, Stayed=No, Left=Yes |
| Removed Outlier values | Based on 'Age' and 'monthly_income' columns e.g 0, 999 '-200', '-3000' | 
| Data type | Changed the columns to the correct data type |
...
- Identified and corrected inconsistent `business_travel` category values 
  during analysis (discovered when attrition rates appeared abnormally high 
  for one category — root cause was inconsistent text formatting)

**Original dataset:** 50,020 rows  
**Clean dataset:** 49,934 rows  
**Records removed:** 86  

---

## DAX Measures
Key measures created in Power BI to support the analysis:

```dax
-- 01 Total Employees
Total Employees = COUNTROWS('hr_attrition')

-- 02 Total Attritions = 
COUNTROWS(
    FILTER('hr_attrition', 'hr_attrition'[attrition] = "Yes")
)

-- 03 Attrition Rate
Attrition Rate = 
DIVIDE(
    COUNTROWS(FILTER('hr_attrition', 'hr_attrition'[attrition] = "Yes")),
    COUNTROWS('hr_attrition'),
    0
)

--04 Total Attrition Cost
Total Attrition Cost = SUM('hr_attrition'[attrition_cost])

-- 05 Average Monthly Income
Avg Monthly Income = AVERAGE('hr_attrition'[monthly_income])

-- 06 Average Performance Rating
Avg Performance Rating = AVERAGE('hr_attrition'[performance_rating])

-- 07 Average Job Satisfaction
Avg Job Satisfaction = AVERAGE('hr_attrition'[job_satisfaction])

-- 08 Overtime Attrition Rate
Overtime Attrition Rate = 
CALCULATE(
    [Attrition Rate],
    'hr_attrition'[overtime] = "Yes"
)
```
`_Measures`

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
![DAX Measures](screenshots/screenshots_dax_measures.png)

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
![Overview](screenshots/screenshots_overview.png)

![Attrition Analysis](screenshots/screenshots_attrition_analysis.png)


---

## Key Findings
- Attrition rate is consistent across department, job role, business travel, 
  and marital status (~14-17% in every category) — attrition is not isolated 
  to specific teams or groups.
- Overtime is the dominant driver of attrition: 82.55% of all employees who 
  left were working overtime, compared to only 17.45% who weren't.
- This suggests overtime — not job role, department, or personal 
  circumstances — is the strongest lever HR has to reduce attrition.
- Job satisfaction is a strong predictor of attrition: employees who left 
  the company had an average satisfaction score 0.76 points lower 
  (on a 4-point scale) than those who stayed.
- Performance ratings (avg 3.50) and job satisfaction (avg 2.50 overall) 
  are otherwise consistent across departments and job roles.
- Training frequency shows no measurable relationship with performance 
  rating (0.00 difference between high and low training groups).
- Overall, job satisfaction — not training, department, or role — appears 
  to be the strongest internal signal of employee performance risk and 
  retention risk in this dataset.

## Overall Insight
Two factors stand out as meaningful predictors in this dataset: 
**overtime** (driving 82.55% of attritions) and **job satisfaction** 
(0.76-point gap between those who stayed and left). Department, job role, 
business travel, marital status, and training frequency show little to 
no variation — suggesting HR interventions should focus on workload 
management and employee engagement rather than team-specific programs.

---

## Recommendations
- Investigate overtime policies and workload distribution, particularly in 
  teams with high overtime frequency.
- Consider implementing overtime caps or additional staffing in roles 
  requiring frequent overtime to reduce burnout-driven attrition.
- Since attrition is broad-based rather than concentrated, retention 
  strategies should be company-wide rather than department-specific.
- Recommendation 2
- Recommendation 3

---

## Tools Used
- **Power BI Desktop** — data modelling, DAX, dashboard
- **Power Query** — data cleaning and transformation
- **CSV** — raw data source (50,020 rows)
