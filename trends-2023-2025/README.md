# Trends, 2023–2025

The same frozen rules and the same rate method applied to each year OSHA has published case detail. Shows whether handling and lateral-transfer injuries are rising or falling, and gives a baseline against which any intervention can be measured.

All source data is public US Government work (17 U.S.C. §105). Every figure below is reproducible from the notebook in this folder.

## Data

OSHA Injury Tracking Application, case detail and Form 300A summaries, as published at https://www.osha.gov/itadata: 2023 (case detail through 12-31-2023, OIICS release; summaries through 12-31-2024), 2024 (through 12-31-2025), 2025 (through 08-31-2026). Hospitals (NAICS 622) and nursing care facilities (NAICS 6231).

## Results

**Hospitals**

| Year | Establishments reporting | All cases | Handling | Share | **Lateral transfers** | Handling per 100 FTE | Lateral per 100 FTE |
|---|---|---|---|---|---|---|---|
| 2023 | 3,917 | 168,874 | 18,909 | 11.2% | **1,487** | 0.572 | 0.045 |
| 2024 | 3,701 | 138,537 | 15,172 | 11.0% | **1,207** | 0.424 | 0.035 |
| 2025 | 3,967 | 181,690 | 20,685 | 11.4% | **1,726** | 0.544 | 0.046 |

**Nursing facilities**

| Year | Establishments reporting | All cases | Handling | Share | Lateral transfers | Handling per 100 FTE |
|---|---|---|---|---|---|---|
| 2023 | 5,354 | 42,449 | 6,380 | 15.0% | 29 | 1.008 |
| 2024 | 3,769 | 28,074 | 5,105 | 18.2% | 23 | 0.950 |
| 2025 | 3,998 | 25,860 | 5,654 | 21.9% | 22 | 1.011 |

**Rates are flat.** Handling injuries per 100 full-time workers sit near 0.42–0.57 in hospitals and 0.95–1.01 in nursing facilities across all three years. Counts move with the number of establishments reporting; 2024 is low in every hospital column, including all cases, which is consistent with fewer submissions rather than fewer injuries.

**Lateral-transfer injuries in hospitals recur every year:** 1,487, 1,207, and 1,726, at 0.035–0.046 per 100 full-time workers. Nursing facilities recorded 29, 23, and 22.

**Handling is a growing share of nursing-facility injuries:** 15.0%, then 18.2%, then 21.9%. A share does not depend on reported hours or on how many establishments submitted, so this is the most reliable movement in the table. It does not show that handling injuries are rising; nursing-facility cases of all kinds fell from 42,449 to 25,860 over the three years, while handling cases fell by less.

## How rates are computed

Population and filters as in [`injury-rates/`](../injury-rates/): establishments with 100 or more employees, hours between 500 and 4,000 per average employee, case detail submitted if the 300A reports recordable cases. One filter was added here: within each setting and year, the top 0.5% of establishments by total hours are dropped.

**Why.** Nursing-facility hours for 2023 totalled 3.10 billion, against 1.03 billion in 2024 and 1.04 billion in 2025, across a similar number of establishments. Hours per employee looked normal in each case (median about 1,500), so the existing filter did not catch them: both the hours and the employee count appear to be company-wide figures entered on one facility's form. After the trim, hours per establishment are 260,832 (2023), 274,902 (2024), and 272,266 (2025), and the three years are comparable. The filter is applied to every year and both settings, and the diagnostics that found the problem are in the notebook.

## What this data cannot show

- **Three years is a short series.** Changes can reflect changes in who submitted, not changes in risk.
- **2025 submissions were still open** when the file was downloaded (through 08-31-2026); 2023 and 2024 are closed.
- **Rule accuracy was measured on 2025 narratives only** (see `handling-task-types/`). Counts here are as found by the rules, not adjusted for precision; adjustment scales every year by the same factor and does not change the trend.
- **Establishments with fewer than 100 employees are not included.**
- No establishment is named.

## Files

| File | Contents |
|---|---|
| `trends-2023-2025.ipynb` | Downloads, classifies, and computes each year, with outputs |
| `derived/trend-2023-2025.csv` | Counts, rates, and hours diagnostics by year and setting |

## License

Code: MIT. Derived files: CC-BY-4.0. Source data is US Government work and not subject to copyright.

Yuxuan Huang · Vantara Medical Equipment (Mahoraga Vantara LLC)
