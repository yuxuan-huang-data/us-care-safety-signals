# Examples

Short worked examples of how to use this repository's published files. They read the published CSVs over the network; nothing is downloaded from OSHA and no classification is re-run.

| Notebook | What it shows |
|---|---|
| [`join-injury-rates-to-cms.ipynb`](join-injury-rates-to-cms.ipynb) | Joins `injury-rates/derived/facility-rates-2025.csv` to the current CMS Provider Information file through the CMS Certification Numbers in `crosswalk/`, then looks at staff handling-injury rates by nurse staffing, star rating, and ownership |

## The files worth starting from

| File | One row per | Key columns |
|---|---|---|
| `injury-rates/derived/facility-rates-2025.csv` | Establishment (7,554: hospitals and nursing facilities) | `establishment_id`, `setting`, `state`, `employees`, `hours`, `cases`, `handling`, `lateral`, `all_cases_rate`, `handling_rate`, `cms_ccn` |
| `crosswalk/osha-cms-crosswalk-nf-2025.csv` | Matched nursing home (8,047) | `osha_establishment_id`, `cms_ccn`, `tier`, match scores |
| `hospital-crosswalk/osha-cms-crosswalk-hospital-2025.csv` | Matched hospital (3,180) | same columns |
| `trends-2023-2025/derived/trend-2023-2025.csv` | Year and setting | counts, rates, and coverage diagnostics for 2023, 2024, 2025 |
| `benchmark/` and `handling-task-types/derived/task-review-labels.csv` | Narrative | human and AI labels, for developing or scoring a text classifier |

The crosswalk tiers grade match confidence: A is strongest, C is weakest. Published analyses in this repository use tiers A and B.

## Reading them

```python
import pandas as pd
BASE = 'https://raw.githubusercontent.com/yuxuan-huang-data/us-care-safety-signals/main/'
fac = pd.read_csv(BASE + 'injury-rates/derived/facility-rates-2025.csv',
                  dtype={'establishment_id': str, 'cms_ccn': str})
```

Keep `cms_ccn` as a string: CMS Certification Numbers have leading zeros.

## Before drawing conclusions

Each module's README states what its figures can and cannot show. Three limits apply to every file here: injury rates measure what was **reported**, establishments with fewer than 100 employees are **not** in the rate files, and the handling and task classifications are keyword rules whose measured precision is published alongside them.

## License

Code: MIT. Derived files: CC-BY-4.0. Source data is US Government work and not subject to copyright.

Yuxuan Huang · Mahoraga Vantara LLC
