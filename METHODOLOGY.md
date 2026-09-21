# Methodology

This document covers `hospital-psi/`. The method for `nursing-facility-handling/`,
including its classification rule and validation, is documented in that
directory's README and notebook.

## Scope

This analysis uses three CMS provider datasets to describe the distribution of
hospital-acquired pressure injury rates across US acute care hospitals, and to
identify hospitals that are both financially penalized under the Hospital-Acquired
Condition Reduction Program and rated worse than the national pressure ulcer rate.

It makes no causal claim and evaluates no intervention.

---

## Data acquisition

CMS publishes a DCAT catalog at `https://data.cms.gov/provider-data/data.json`
listing every provider dataset with its current CSV download URL. The file URLs
themselves contain a content hash and change when CMS republishes; the catalog
does not. The notebook therefore resolves URLs from the catalog at runtime rather
than hard-coding them.

Datasets retrieved 2026-09-17:

| Local name | CMS title | Rows | Released |
|---|---|---|---|
| `complications` | Complications and Deaths - Hospital | 95,800 | 2026-07-22 |
| `hac` | Hospital-Acquired Condition (HAC) Reduction Program | 3,055 | 2026-01-26 |
| `hosp_general` | Hospital General Information | 5,419 | 2026-07-22 |

---

## Transformation

### Complications and Deaths

Published in long format: one row per hospital per measure. Reshaped to one row
per hospital with the following measures pivoted into columns:

| Measure ID | Column | Description |
|---|---|---|
| `PSI_03` | `psi03_pu` | Pressure ulcer rate |
| `PSI_08` | `psi08_fall_fx` | In-hospital fall-associated fracture rate |
| `PSI_90` | `psi90` | Patient safety and adverse events composite |
| `COMP_HIP_KNEE` | `hip_knee_comp` | Complication rate, hip/knee replacement |

The `Compared to National` field is retained for PSI_03 and PSI_08. This field is
CMS's own determination, based on whether the hospital's risk-adjusted confidence
interval clears the national rate — not a calculation performed here.

All numeric fields are read as text and cast with `TRY_CAST`, so suppressed and
non-numeric values become null rather than failing.

### HAC Reduction Program

One row per hospital. Retained: `Total HAC Score`, `PSI 90 Composite Value`,
`PSI 90 W Z Score`, and `Payment Reduction` (Yes / No / N/A).

### Join

All three datasets key on `Facility ID`, the 6-digit CMS Certification Number.
Verified unique and non-null in the HAC and General Information files. The
Complications file is long-format and repeats the CCN across measure rows.

3,056 hospitals have a published PSI-03 value and form the analysis population.

---

## Measure definition

PSI-03 is an AHRQ Patient Safety Indicator. Per the AHRQ specification:

> Stage III or IV pressure ulcers or unstageable (secondary diagnosis) per
> 1,000 discharges among surgical or medical patients ages 18 years and older.
> Excludes stays less than 3 days; cases with a principal stage III or IV (or
> unstageable) pressure ulcer diagnosis; cases with a secondary diagnosis of
> stage III or IV pressure ulcer (or unstageable) that is present on admission;
> obstetric cases; severe burns; exfoliative skin disorders.

The "not present on admission" exclusion is what makes PSI-03 a measure of
hospital-acquired injury rather than prevalence.

PSI-03 is one of the component indicators inside the CMS PSI-90 composite, which
is in turn one of six measures in the HAC Reduction Program's Total HAC Score.
The other five are CLABSI, CAUTI, SSI, MRSA bacteremia, and C. difficile
infection, each reported to CDC's National Healthcare Safety Network.

---

## Limitations

**Claims-based measurement.** PSI-03 is derived from administrative discharge
data, not chart review. Published research has documented that coding practice
affects the rate — in particular, spinal cord injury cases not coded for
paralysis are not excluded and therefore flag as PSI-03 events.

**Suppression.** CMS does not publish a rate where case volume is too small.
1,692 hospitals in the Complications file show "Not Available" for PSI-03.

**Asymmetric ratings.** The `Compared to National` determination requires the
risk-adjusted confidence interval to clear the national rate. 2,829 of 3,056
hospitals are rated "no different." The 178 rated worse and 49 rated better are
the tails, not halves of a distribution.

**Measurement periods differ.** HACRP FY2026 uses PSI-90 data from 2022-07-01
through 2024-06-30 and infection data from 2023-01-01 through 2024-12-31. The
Complications file reports a separate period. Penalty status and PSI-03 rate are
therefore not measured over identical windows.

**Cross-sectional.** No time series, no comparison group, no intervention. This
analysis describes a distribution. It cannot support any statement about what
changes it.

**Multiple causation.** Pressure injury arises from nutrition, repositioning
frequency, support surface selection, perfusion, immobility duration, and
mechanical forces during repositioning and transfer. Nothing here isolates any
one of these.

---

## Disclosure

The author distributes a powered patient transfer system to US healthcare
facilities. That is how the author came to this data. No product is named,
evaluated, or recommended in this analysis, and no claim is made that any device
affects the measures described.

---

## Reproducibility

`hospital-psi/psi-analysis.ipynb` runs end to end from a clean environment: it
resolves URLs from the CMS catalog, downloads the three CSVs, builds the tables,
and produces every figure cited in the README. Requires `duckdb`, `pandas`,
`requests`.

Source data is US Government work under 17 U.S.C. §105 and is not subject to
copyright.
