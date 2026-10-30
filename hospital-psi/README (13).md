# Hospital-Acquired Pressure Injury: What the Federal Data Shows

Analysis of CMS-published pressure injury rates (PSI-03) across 3,056 US
acute care hospitals, combined with Hospital-Acquired Condition Reduction
Program penalty data for FY2026.

All source data is public US Government work (17 U.S.C. §105). Every figure
below is reproducible from the notebook in this folder.

Hospitals are identified by CMS Certification Number (CCN) and state only.
Names and addresses are not carried into the analysis; they remain in CMS's
Hospital General Information file, which anyone can join on CCN. A penalty or a
rating does not by itself establish fault.

---

## Key findings

**Only 49 hospitals in the United States are rated better than the national
pressure ulcer rate. 178 are rated worse.**

| | |
|---|---|
| Hospitals with published PSI-03 data | 3,056 |
| Median PSI-03 rate | 0.51 per 1,000 discharges |
| 90th percentile | 1.11 |
| Maximum | 8.31 |
| Rated *worse* than national | 178 |
| Rated *better* than national | 49 |
| Hospitals penalized under HACRP, FY2026 | 719 |
| **Penalized AND rated worse on PSI-03** | **95** |

The HAC Reduction Program reduces **all** Medicare inpatient payments by 1%
for hospitals in the worst-performing quartile. PSI-90 — which contains
PSI-03 — is one of six component measures.

---

## What did not hold

Two things worth stating, because they constrain what this data can support:

**PSI-03 does not isolate a single cause.** Pressure injury results from
nutrition, repositioning frequency, support surface, perfusion, and transfer
mechanics. This analysis does not establish which contributes most, and no
causal claim should be read into it.

**The rating distribution is not symmetric.** 178 hospitals rated worse
against 49 rated better does not mean most hospitals are below average. It
means CMS's risk-adjusted confidence intervals rarely clear the threshold in
either direction — 2,829 hospitals are rated "no different than the national
rate."

---

## Data sources

Retrieved 2026-10-29. The file titles and release dates used are also written
to `derived/source-files.csv`.

| Dataset | Rows | Released | Source |
|---|---|---|---|
| Complications and Deaths — Hospital | 95,800 | 2026-09-10 | data.cms.gov/provider-data |
| Hospital-Acquired Condition Reduction Program (FY2026) | 3,055 | 2026-01-26 | data.cms.gov/provider-data |
| Hospital General Information | 5,419 | 2026-07-22 | data.cms.gov/provider-data |

The Complications file covers 4,790 hospitals. 3,056 of them have a published
PSI-03 value. Of the rest, 1,692 show "Not Available" and 42 "Number of Cases
Too Small."

---

## What is published

| File | Contents |
|---|---|
| `derived/penalized-and-worse-psi03.csv` | The 95 hospitals both penalized under HACRP and rated worse on PSI-03: CCN, state, PSI-03, PSI-08, PSI-90, Total HAC Score |
| `derived/by-state.csv` | By state: hospitals with a PSI-03 value, penalized, rated worse, median PSI-03 |
| `derived/source-files.csv` | CMS file titles and release dates used |

---

## Data release

CMS republishes these files on its own schedule. Version 1.x of this repository
used a Complications file released 2026-07-22 (retrieved 2026-09-17). The current
figures use the file released 2026-09-10. Every PSI-03 figure in this README is
identical on both files, and the same 95 hospitals are both penalized and rated
worse. The HACRP file is unchanged (FY2026, released 2026-01-26).

Version 1.x also published hospital names and cities in the list of 95. They
were removed so that the repository identifies facilities by identifier only,
as its other modules do.

Join key is CMS Certification Number (`Facility ID`, 6 digits).

PSI-03 is defined by AHRQ as stage III/IV or unstageable pressure ulcers
recorded as a secondary diagnosis and **not present on admission**, per 1,000
discharges, ages 18 and older. Stays under three days, obstetric cases,
severe burns, and exfoliative skin disorders are excluded.

---

## Reproducing

    pip install duckdb pandas requests
    jupyter notebook psi-analysis.ipynb

The notebook resolves current download URLs from the CMS DCAT catalog at
`data.cms.gov/provider-data/data.json`. CMS rotates file URLs monthly; the
catalog does not.

---

## Limitations

- PSI-03 is claims-based. Coding practice varies between hospitals, and
  miscoding — particularly of paralysis, which excludes a case — affects the
  rate.
- CMS suppresses rates for hospitals with too few cases. 1,692 hospitals show
  "Not Available."
- HACRP penalty status reflects FY2026, with a PSI-90 measurement period
  ending 2024-06-30 and infection measures ending 2024-12-31.
- This is a cross-sectional analysis. It supports no inference about cause.

---

## Citation

See `CITATION.cff`.

## License

Code: MIT. Data and derived files: CC-BY-4.0. Source data is US Government
work and not subject to copyright.

---

Yuxuan Huang · Vantara Medical Equipment (Mahoraga Vantara LLC), New York
