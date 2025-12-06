# CEE 252 Group Project: Road Usage Charge (RUC) Equity Analysis

## Project Overview
This project investigates the distributional impacts of replacing the gas tax with a **Road Usage Charge (RUC)** in California. Using household travel survey data, we analyze the equity implications of a flat-rate RUC versus a progressive policy design.

The analysis focuses on:
1.  **Baseline Scenario:** A flat per-mile tax rate ($0.03/mile).
2.  **Policy Scenario:** A progressive structure with "free mile" allowances based on income groups, maintained at revenue neutrality.
3.  **Equity Metrics:** Comparing the Affordability Index (Tax/Income) and Gini Coefficients between scenarios.

---

## Repository Structure

| File | Description |
| :--- | :--- |
| `252_project_code.ipynb` | The main analysis notebook. Performs statistical summaries, RUC calculations, and visualizations. |
| `household_ruc_ready.csv` | The pre-processed dataset containing cleaned household income, location, and VMT data. |
| `README.md` | Project documentation and data dictionary. |

---

## Data Source & Processing
The data used in this project is derived from the **Transportation Secure Data Center (TSDC)** - California National Household Travel Survey (NHTS).

* **Source:** [NREL Transportation Secure Data Center](https://www.nrel.gov/transportation/secure-transportation-data/tsdc-nhts-california)
* **Original Files Used:** `survey_household.csv`, `survey_household_weights_7day.csv`, `survey_vehicle.csv`.

### Data Processing Workflow
The raw data was processed to generate `household_ruc_ready.csv` through the following steps:

1.  **Merge & Weight:** Household data was merged with final survey weights (`wthhfin`).
2.  **Income Cleaning:** Income bracket codes were mapped to estimated median USD values. Missing values (-7/-8/-9) were removed.
    * *Groups:* Low, Lower-Middle, Upper-Middle, High.
3.  **Location Mapping:** Households were categorized based on `urbrur` and `urbansize`:
    * `Rural`: urbrur code 2.
    * `Suburban`: Urban + Small urban size (1 or 2).
    * `Urban`: All other urban codes.
4.  **VMT Aggregation:** Vehicle-level miles were aggregated to calculate total **Annual Household VMT** (`annual_miles`).

### Final Dataset Schema (`household_ruc_ready.csv`)
| Column | Description |
| :--- | :--- |
| `household_id` | Unique Household ID (sampno) |
| `income_group` | Categorical: low / lower_middle / upper_middle / high |
| `annual_income_usd` | Estimated annual household income in USD |
| `location_type` | Categorical: urban / suburban / rural |
| `hh_weight` | Household survey weight (for representative analysis) |
| `annual_miles` | Total annual vehicle miles traveled by the household |

---

## How to Run the Analysis

### Prerequisites
Need Python installed with the following libraries:
* pandas
* numpy
* matplotlib

### Steps
1.  Clone this repository:
    ```bash
    git clone [https://github.com/YichiZhang0321/252-group-project.git](https://github.com/YichiZhang0321/252-group-project.git)
    ```
2.  Ensure the dataset `household_ruc_ready.csv` is in the same directory.
3.  Open `252_project_code.ipynb` in Jupyter Notebook or VS Code.
4.  Run all cells to generate the statistics and equity comparison charts.

---

## Methodology Summary

The code implements the following logic:

1.  **Baseline Calculation:**
    * Rate ($r$) = $0.03 / mile.
    * Tax = $r \times \text{Annual Miles}$.
2.  **Policy Intervention (Progressive):**
    * **Free Miles Allowance:**
        * Low Income: 8000 miles
        * Lower-Middle: 4,000 miles
        * Upper-Middle: 0 miles
        * High Income: 0 miles
    * **Revenue Neutrality:** A new rate ($r_{policy}$) is calculated to ensure the total revenue remains the same as the baseline.
3.  **Evaluation:**
    * **Affordability Index (AI):** $\frac{\text{Tax Paid}}{\text{Annual Income}}$. Lower is better/more affordable.
    * **Gini Coefficient:** Measures income inequality before and after the tax policy.

---

## References
1.  **Data:** National Renewable Energy Laboratory (NREL). (2025). *Transportation Secure Data Center: California National Household Travel Survey*. Retrieved from [https://www.nrel.gov/transportation/secure-transportation-data/tsdc-nhts-california](https://www.nrel.gov/transportation/secure-transportation-data/tsdc-nhts-california)
