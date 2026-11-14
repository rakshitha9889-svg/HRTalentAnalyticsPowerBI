# HR Talent Analytics Dashboard (Power BI)

An interactive 3-page Power BI dashboard that analyses employee attrition, career progression and diversity for **1,470 employees**, and turns the findings into concrete recommendations for a Talent Management team.

![Workforce Overview](screenshots/page1_workforce_overview.png)

## Business question
**Who is leaving the organisation, why, and where should Talent Management act first?**

## Data
- **Source:** [IBM HR Analytics Employee Attrition & Performance](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset) (Kaggle)
- **Size:** 1,470 employees × 35 attributes (department, role, tenure, overtime, satisfaction, pay, training, promotion history)
- **Note:** the dataset is fictional, created by IBM data scientists for analytics practice.

## Method

### 1. Data cleaning in Power Query
- Validated and corrected data types for all columns
- Removed 3 constant columns (`EmployeeCount`, `Over18`, `StandardHours`)
- Translated coded values (1–5 scales) into readable labels: Education Level, Job Satisfaction, Environment Satisfaction, Work-Life Balance
- Engineered analysis columns: **Age Band**, **Tenure Band**, **Promotion Overdue** (5+ years since last promotion) and an **Attrition Flag** (1/0)
- Added sort-order columns so bands and labels display in logical order

![Power Query applied steps](screenshots/power_query_steps.png)

### 2. DAX measures
| Measure | Purpose |
|---|---|
| `Headcount` | Number of employees in the current filter context |
| `Leavers` | Employees with Attrition = Yes |
| `Attrition Rate` | Leavers ÷ Headcount |
| `Avg Tenure (Years)` | Average years at the company |
| `Avg Training Sessions` | Average training sessions last year |
| `Promotion Overdue %` | Share of employees with no promotion in 5+ years |
| `Female Share` | Share of female employees |
| `Avg Monthly Income` | Average monthly income |
| `Gender Pay Gap %` | 1 − (female avg income ÷ male avg income) |

```DAX
Attrition Rate = DIVIDE([Leavers], [Headcount])
Promotion Overdue % = DIVIDE(CALCULATE([Headcount], Employees[Promotion Overdue] = "Yes"), [Headcount])
Gender Pay Gap % = 1 - DIVIDE(
    CALCULATE([Avg Monthly Income], Employees[Gender] = "Female"),
    CALCULATE([Avg Monthly Income], Employees[Gender] = "Male"))
```

### 3. Report pages
1. **Workforce Overview:** headcount, attrition rate, tenure and gender KPIs; headcount by department and age band
2. **Attrition Drivers:** attrition by job role, tenure, overtime and job satisfaction
3. **Talent Development & Diversity:** promotion gaps, pay by job level and gender, training vs attrition

All pages share synced slicers for **Department, Gender and Job Level**.

## Key insights

| # | Finding | Recommendation |
|---|---|---|
| 1 | Employees working **overtime leave at 30.5% vs 10.4%**, about 3× higher | Review workloads in high-overtime roles; track overtime as an early-warning KPI |
| 2 | **34.9% attrition in the first 0–1 years**; highest-risk roles are **Sales Representatives (39.8%)**, Laboratory Technicians (23.9%) and Human Resources (23.1%) | Buddy and mentoring program with 30/60/90-day check-ins, starting in these roles |
| 3 | **20.6% of Sales staff** have had no promotion in 5+ years. The overall gender pay gap is −4.8% (women earn 4.8% more on average) | Promotion and career-path review in Sales; verify pay equity within each job level yearly, since averages can hide level-specific gaps |

Overall attrition: **16.1%** (237 of 1,470 employees).

A 3-slide summary for stakeholders is in [`docs/Talent_Analytics_Summary.pptx`](docs/Talent_Analytics_Summary.pptx).

## Screenshots
| Workforce Overview | Attrition Drivers | Talent Development & Diversity |
|---|---|---|
| ![](screenshots/page1_workforce_overview.png) | ![](screenshots/page2_attrition_drivers.png) | ![](screenshots/page3_talent_development.png) |

## Repository structure
```
hr-talent-analytics-powerbi/
├── hr_talent_analytics.pbix
├── data/raw/WA_Fn-UseC_-HR-Employee-Attrition.csv
├── screenshots/
├── docs/Talent_Analytics_Summary.pptx
└── README.md
```

## How to open
1. Install [Power BI Desktop](https://www.microsoft.com/power-bi/desktop) (Windows, free)
2. Open `hr_talent_analytics.pbix`
3. If prompted, update the data source path to `data/raw/WA_Fn-UseC_-HR-Employee-Attrition.csv` (Transform data → Data source settings)

## Tools
Power BI Desktop · Power Query · DAX · PowerPoint

## Author
**Rakshitha Ramya Sadanand** · [LinkedIn](https://www.linkedin.com/in/rakshitha-rs-37329617b) · [GitHub](https://github.com/rakshitha9889-svg)
