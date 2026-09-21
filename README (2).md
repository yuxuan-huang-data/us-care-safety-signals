# Data Tables

Aggregated tables derived from public federal data. Each table is produced by a notebook in this repository and can be regenerated from source.

## `nf-injury-by-state-2025.csv`

US nursing care facilities (NAICS 6231), OSHA Injury Tracking Application case detail data for 2025, one row per state.

| Column | Meaning |
|---|---|
| `state` | Two-letter postal code of the establishment |
| `cases` | All reported injury and illness cases |
| `establishments` | Distinct establishments with at least one case |
| `handling` | Cases classified as resident handling (rule in `nursing-facility-handling/`) |
| `violence` | Cases classified as violence by a person (rule in `resident-violence/`) |
| `handling_share`, `violence_share` | Category cases ÷ all cases in the state |
| `handling_days_away`, `handling_days_restricted` | Sum of days away from work and of restricted or transferred duty, handling cases |
| `violence_days_away`, `violence_days_restricted` | Same, violence cases |

Column totals match the national figures: 24,292 cases, 5,280 handling, 2,159 violence.

## `nf-injury-by-size-2025.csv`

The same data by OSHA establishment size code, converted to employee ranges per the OSHA ITA data dictionary.

| Code | Employees |
|---|---|
| 1 | under 20 |
| 21 | 20–99 |
| 2 | 20–249 (code used before 2023 data; split into 21 and 22) |
| 22 | 100–249 |
| 3 | 250 or more |

Case detail submission is required only of establishments with 100 or more employees in designated industries, so rows below 100 employees are small and not representative of small facilities.

## What these tables cannot be used for

These are counts of reported cases. Establishments with no cases do not appear in the source file, so the tables do not support injury rates per worker or per hour, and differences between states should not be read as differences in risk.

## License

CC-BY-4.0. Source data is US Government work (17 U.S.C. §105).
