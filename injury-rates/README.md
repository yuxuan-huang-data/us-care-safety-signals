# Injury Rates: Resident and Patient Handling per 100 Full-Time Workers

The other modules count injuries. This module divides them by hours worked, so hospitals and nursing facilities, large and small establishments, and states can be compared. Rates are per 100 full-time-equivalent workers (200,000 hours), the OSHA convention.

All source data is public US Government work (17 U.S.C. §105). Every figure below is reproducible from the notebook in this folder.

## Data

OSHA Injury Tracking Application, 2025 (submissions through 2026-08-31): case detail (Forms 300/301, 782,582 cases) and establishment summaries (Form 300A, 400,288 records), for hospitals (NAICS 622) and nursing care facilities (NAICS 6231).

## Who is counted

Case detail is required only of establishments with 100 or more employees in designated industries, so rates cover those establishments. Two groups are excluded, and both are counted in `derived/coverage.csv`:

- **No case detail.** The establishment's 300A reports recordable cases but it submitted no case detail, so its handling injuries cannot be counted. Counting it as zero would understate rates.
- **Implausible hours.** Hours outside 500–4,000 per average employee. A full-time worker works about 2,000 hours a year.

| | Establishments, 100+ employees | No case detail | Implausible hours | **Included** |
|---|---|---|---|---|
| Hospitals | 4,845 | 811 | 76 | **3,958** |
| Nursing facilities | 5,365 | 1,559 | 210 | **3,596** |

**The hours filter was added after the first run.** Without it, nursing-facility hours in several states were inconsistent with employee counts (for example, 1.34 billion hours across 190 Illinois facilities, about 3,500 full-time workers each), consistent with company-wide hours entered on a single facility's form. The unfiltered nursing-facility all-case rate was 0.8 per 100 workers; filtered, 4.4. Both versions are published (`derived/rates-by-setting-no-hours-filter.csv`).

## Results

**Per 100 full-time workers, 2025:**

| | Hospitals | Nursing facilities |
|---|---|---|
| All recordable cases | 4.72 | 4.37 |
| **Resident or patient handling** | **0.54** | **0.95** |
| Handling, adjusted for measured rule precision | 0.43 | 0.85 |
| Handling with days away from work | 0.22 | 0.35 |
| **Lateral transfer between surfaces** | **0.045** | 0.004 |
| Lateral transfer, adjusted for measured rule precision | 0.037 | — |

Adjustments use the precision measured by blind review in [`handling-task-types/`](../handling-task-types/): hospital handling 0.80, nursing-facility handling 0.89, hospital lateral transfer 0.83.

- **A nursing-facility worker is about twice as likely as a hospital worker to be injured handling a resident** (adjusted 0.85 vs 0.43 per 100 workers).
- **Lateral-transfer injuries occur at about ten times the rate in hospitals** as in nursing facilities.
- For context, the Bureau of Labor Statistics survey reports 5.5 recordable cases per 100 full-time workers for nursing and residential care facilities (NAICS 623; [BLS industry profile](https://www.bls.gov/iag/tgs/iag623.htm), accessed 2026-11-13). The rates here, from establishment submissions to OSHA, are lower; the two sources differ in coverage, method, and industry scope.

**By establishment size** (handling per 100 workers):

| Employees | Hospitals | Nursing facilities |
|---|---|---|
| 100–249 | 0.60 | 1.11 |
| 250–499 | 0.55 | 1.10 |
| 500–999 | 0.58 | 1.06 |
| 1,000+ | 0.53 | 0.05 (25 establishments) |

Rates are similar across sizes. The 25 nursing facilities reporting 1,000 or more employees have an implausibly low rate; their employee counts may themselves be company-wide, and they are too few to interpret.

**By state:** 47 states for hospitals and 42 for nursing facilities have at least 10 included establishments (`derived/rates-by-state.csv`). Hospital handling rates range from 0.14 to 1.26 per 100 workers (median 0.55); nursing-facility rates from 0.42 to 3.62 (median 1.11). State differences reflect reporting practice, case mix, and staffing as well as risk, and are reported, not interpreted.

**Lost-time cases** (all case-detail records, not only the rate population; `derived/lost-time.csv`):

| | Cases | With days away | Days away | Days restricted |
|---|---|---|---|---|
| Hospital handling | 20,685 | 8,473 | 312,566 | 461,884 |
| **Hospital lateral transfer** | **1,726** | **743** | **28,358** | **41,597** |
| Nursing-facility handling | 5,654 | 2,056 | 58,178 | 125,621 |

These are rule counts, before adjustment for precision.

## Facility-level dataset

`derived/facility-rates-2025.csv`: one row for each of the 7,554 establishments in the rate population, with OSHA establishment ID, setting, state, average employees, hours, case counts, and rates. 3,217 nursing facilities carry their CMS Certification Number from the [`crosswalk`](../crosswalk/) (tiers A and B), so staff injury rates can be joined to CMS quality, staffing, and inspection data. No name or address is included.

## What this data cannot show

- **Only establishments with 100 or more employees** that submitted case detail.
- **Reported hours.** Errors in hours that pass the plausibility filter carry into the rates.
- **Rule-based classification.** Adjusted rates use precision measured on samples of 54–75 cases; their uncertainty is stated in `handling-task-types/`.
- **Rates are of reported injuries.** Differences in reporting practice affect them.
- **No establishment is named.** A recorded injury does not establish fault.

## Files

| File | Contents |
|---|---|
| `injury-rates.ipynb` | Every figure above, with outputs |
| `derived/coverage.csv` | Establishments included and excluded, by reason |
| `derived/rates-by-setting.csv`, `-by-state.csv`, `-by-size.csv` | Rates |
| `derived/rates-by-setting-no-hours-filter.csv` | Rates without the hours filter |
| `derived/lost-time.csv` | Lost-time counts and days |
| `derived/facility-rates-2025.csv` | One row per establishment |

## License

Code: MIT. Derived files: CC-BY-4.0. Source data is US Government work and not subject to copyright.

Yuxuan Huang · Vantara Medical Equipment (Mahoraga Vantara LLC)
