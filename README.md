# Mini Project A – Chicago City Salaries

This project analyzes **hourly wage disparities** between Fire/Police and other Chicago city departments using a public salaries dataset (~33k employees). The goal is to:

- Clean and standardize salary and hourly rate data,
- Convert all compensation to **comparable hourly rates**, and
- Quantify how much more Fire/Police earn relative to other departments.

The work was originally done as a UCLA Anderson mini-project and has been refactored into a reproducible R workflow.

---

## Key Question

> Do Fire and Police employees earn systematically higher hourly wages than other Chicago city employees, after standardizing salaries and hourly rates?

---

## Data

Source: Chicago city salaries dataset (≈33,000 rows, 8 columns):

- `Name` – employee name  
- `JobTitle` – job title  
- `Department` – department (e.g., "POLICE", "FIRE", "WATER MGMNT")  
- `Time` – employment type (`F` = full-time, `P` = part-time)  
- `PayType` – salary vs hourly  
- `Hours` – hours (for part-time hourly roles)  
- `Salary` – annual salary (string with `$` and commas)  
- `Rate` – hourly rate (string with `$` if applicable)

I cleaned the raw columns into numeric fields and constructed a unified hourly rate so full-time salary and hourly workers can be compared side-by-side.

---

## Methods

### 1. Cleaning and hourly conversion

In R (using `tidyverse`):

- Parsed `Salary` and `Rate` from strings to numeric using `readr::parse_number`.
- Converted all pay to **hourly**:

  - Full-time, salaried: `HourlyRate = Salary / 2080` (40 hours/week * 52 weeks)
  - Full-time hourly: `HourlyRate = Rate`
  - Part-time hourly: `HourlyRate = Rate`

- Created a unified hourly metric:

  - `UnifiedRate = HourlyRate`

- Removed noisy roles such as foster-related jobs (e.g. job titles containing “foster”) that could distort comparisons.

### 2. Fire/Police vs other departments

I then classified each row with:

- `is_fire_police = 1` if `Department` is `"FIRE"` or `"POLICE"`, else 0  
- `Category = "Fire/Police"` vs `"Other Departments"`

With this, I:

- Plotted a scatter/boxplot of `UnifiedRate` by `Category` to visualize spread.
- Ran a simple linear regression:

  - `UnifiedRate ~ is_fire_police`

### 3. Regression model

The core model:

- Response: `UnifiedRate` (hourly pay)
- Predictor: `is_fire_police` (1 = Fire/Police, 0 = everyone else)

Results (approximate):

- Intercept (Other Departments):  
  - **$35.77/hour** – average hourly rate for non–Fire/Police employees
- Fire/Police coefficient:  
  - **+$7.21/hour** – wage premium for Fire/Police relative to other departments
- R² ≈ 0.10, indicating that department group alone explains ~10% of the variation in hourly pay.

Residual diagnostics suggest a wide distribution in pay (some specialized roles earn much more), but the **average premium is clear and statistically significant**.

---

## Key Findings

- Non–Fire/Police staff earn on average **about \$35.77/hour**.
- Fire/Police earn **about \$7.21/hour more on average**, i.e. roughly **20% higher** pay relative to other departments.
- Visualizations (scatter + boxplot) show Fire/Police hourly rates are concentrated at a higher band than other departments.

This suggests that **city budget allocations heavily favor Fire/Police pay** compared with other departments.

---

## Recommendation

Given the magnitude of the wage premium:

- The city should **reassess compensation and funding allocations**, especially in the context of:
  - Crime trends,
  - Staffing needs, and
  - Alternative investments.

One policy direction:

- Maintain adequate Fire/Police staffing and safety levels, but gradually **shift incremental funds** toward:
  - Health programs,
  - Community services,
  - Prevention-focused initiatives that address the **root causes of crime**.

This balances short-term safety needs with longer-term, community-based crime reduction.

---

## Repository Structure (example)



- `data/chicago_salaries.csv` – original or cleaned Chicago salaries dataset  
- `R` – R code
- `notebooks` –  html  
- `README.md` – this file

---

## How to Run

1. Clone the repo and open in RStudio:

   ```r
   # in terminal
   git clone https://github.com/Quoc-V-Tran/chicago-city-salaries-analysis.git
