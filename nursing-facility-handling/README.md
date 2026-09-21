# Resident-Handling Injuries in US Nursing Facilities: What OSHA's 2025 Case Data Shows

Analysis of employer-reported injury cases from US nursing care facilities (NAICS 6231), drawn from OSHA's Injury Tracking Application case detail data for calendar year 2025.

All source data is public US Government work (17 U.S.C. §105). Every figure below is reproducible from the notebook in this folder.

## Key findings

In 2025, US nursing facilities reported 24,292 work-related injury and illness cases to OSHA. **5,280 of them (21.7%) occurred while a caregiver was moving a resident.**

| | |
|---|---|
| Nursing-facility cases, 2025 | 24,292 |
| Resident-handling cases | 5,280 (21.7%) |
| Sustained by nursing assistants and aides | 4,293 (81.3%) |
| Days away from work | 54,326 |
| Days of restricted or transferred duty | 117,031 |
| Cases with zero days away | 3,360 (63.6%) |
| Cases over 90 days away | 199 |
| Longest absence | 348 days |

Restricted and transferred duty totals 2.2 times the days away from work. Those days do not appear in lost-time counts.

The distribution is long-tailed. Most handling injuries cost no lost time; 199 cost more than three months.

| Days away | Cases | Share |
|---|---|---|
| 0 | 3,360 | 63.6% |
| 1–7 | 1,016 | 19.2% |
| 8–30 | 473 | 9.0% |
| 31–90 | 232 | 4.4% |
| over 90 | 199 | 3.8% |

The back or lumbar region is named in the injury field of 1,901 handling cases (36.0%). Strain is the most common description.

## How accurate the classification is

OSHA does not code injuries by task. A case is classified as **handling** when its narrative names a person being cared for (resident, patient) *and* a movement of that person (transfer, lift, reposition, boost, turn, and related terms). The full rule is in Section 5 of the notebook and exported to `derived/classification-rule.txt`.

The rule was checked against 200 randomly drawn nursing-facility cases labeled by hand before any figure was computed. The labels are published in `derived/validation-labeled.csv` so that anyone can check them.

| Rule | Precision | Recall |
|---|---|---|
| Strict (used for all figures above) | 0.846 | 0.717 |
| Broad (movement term not required with every person term) | 0.698 | 0.804 |

In the hand-labeled sample, 46 of 200 cases were handling (23.0%; 95% CI 17.7%–29.3%). The strict rule's 21.7% falls inside that range. The broad rule gives 7,112 cases (29.3%), 73,972 days away and 148,780 days restricted; readers who prefer a higher-recall estimate can use those figures, at the cost of precision.

## What did not hold

Three hypotheses were tested and not supported. They are stated here because they limit what this data can be used to argue.

**Fatigue.** If handling injuries accumulated as a shift went on, their share would rise with hours worked. It does not. The share by hour into the shift is flat through roughly the first six hours; the drop afterward reflects shifts ending, not fewer injuries.

**Night shift severity.** Night-shift handling injuries average 32.1 days away (284 cases) against 27.4 for day shift (1,428 cases). The medians are 7.5 and 6.0 days. The difference is small, the night sample is small, and shift assignment depends on a boundary chosen by the analyst. No claim is made.

**Facility-level link to pressure injury.** Facilities with higher pressure-ulcer rates (CMS) do not have higher injury rates (OSHA). Every specification tested fell between r = 0.01 and r = 0.11 (DART rate vs. long-stay pressure-ulcer rate: r = 0.011, n = 3,497). OSHA data carries no CMS Certification Number, so this test depends on name-and-address linkage. The linkage code will be published as its own module; until then this result is reported, not reproduced.

The likely explanation for the last result is measurement: roughly two handling cases per facility is too few to separate real differences from differences in reporting. The null result concerns facility-level correlation only.

## Data source

| Dataset | Scope | Source |
|---|---|---|
| ITA Case Detail Data, 2025 (submissions through 2026-03-15) | 697,201 cases, all industries; 24,292 in NAICS 6231 | osha.gov |

OSHA's server returns HTTP 403 without a browser User-Agent header; the notebook sets one. The download file name changes with each release.

## Reproducing

```
pip install duckdb pandas requests
jupyter notebook nursing-facility-handling.ipynb
```

Place `validation-labeled.csv` in `derived/` to reproduce the accuracy table.

## Limitations

- ITA submission is required only of establishments meeting OSHA's size and industry thresholds. Small facilities are under-represented.
- Narratives are free text written by employers. Detail and wording vary.
- The handling classification is a keyword rule, not an OSHA field. Its measured precision and recall are reported above.
- Open cases undercount final days away.
- **No establishment is named in this analysis.** OSHA: recording an injury does not mean the employer was at fault or violated any OSHA rule.

## Disclosure

The author distributes a powered patient transfer system to US healthcare facilities. No product is named, evaluated, or recommended here.

## Citation

See `CITATION.cff` at the repository root.

## License

Code: MIT. Derived files: CC-BY-4.0. Source data is US Government work and not subject to copyright.

Yuxuan Huang · Vantara Medical Equipment (Mahoraga Vantara LLC), New York
