# Lifting Equipment in Resident-Handling Injury Narratives

How often do employer-written narratives of resident-handling injuries in US nursing facilities mention patient-lifting equipment?

All source data is public US Government work (17 U.S.C. §105). Every figure below is reproducible from the notebooks in this folder.

## Data

OSHA Injury Tracking Application case detail data, 2025 (submissions through 2026-08-31), nursing care facilities (NAICS 6231): 25,860 cases, of which 5,654 are resident-handling cases under the frozen rule from [`nursing-facility-handling/`](../nursing-facility-handling/).

## Equipment groups

Each narrative is assigned one group by keyword rule (Section 3 of `handling-equipment.ipynb`; `derived/classification-rule.txt`):

- **Patient lift**: mechanical or powered lifts, including full-body and sit-to-stand lifts, and their slings and pads
- **Low-tech aid**: gait belts, draw or slide sheets, transfer boards, pads used to move a resident
- **Vehicle lift**: wheelchair lifts on vans or buses, counted separately
- **None mentioned**

## Results

**By the rule:**

| Group | Handling cases | Share |
|---|---|---|
| None mentioned | 4,809 | 85.1% |
| Patient lift | 615 | 10.9% |
| Low-tech aid | 212 | 3.7% |
| Vehicle lift | 18 | 0.3% |

**Corrected by the blind review** (below):

| Quantity | Estimate | 95% range | Share of handling cases |
|---|---|---|---|
| **Narrative mentions no equipment** | **about 4,520** | 4,210–4,670 | **about 80%** |
| Narrative involves a patient lift | about 790 (554 found by the rule + about 240 it missed) | wide | about 14% |

**About four in five resident-handling injury narratives in US nursing facilities mention no lifting equipment.** A narrative that names no equipment does not show the transfer was done by hand; employers may not have written it down. The finding is about what is recorded.

The low-tech aid group had no cases in the review sample and is not validated; its count is not cited.

**Severity by group (rule groups, not corrected):**

| Group | Cases | Days away | Cases over 90 days away |
|---|---|---|---|
| None mentioned | 4,809 | 48,547 | 175 |
| Patient lift | 615 | 7,144 | 30 |
| Low-tech aid | 212 | 1,812 | 6 |

Median days away is zero in every group. Differences between groups cannot be read as an effect of equipment: residents who need a lift are often heavier and less mobile, which raises the risk of the task regardless of how it is done.

## How accurate the rule is

The author reviewed 200 cases blind: 100 the rule says mention a patient lift and 100 handling cases the rule says mention no equipment, shuffled together. Scoring is in [`equipment-review.ipynb`](equipment-review.ipynb); labels are in `derived/equipment-review-labels.csv`.

| Rule says | Reviewer: lift | Reviewer: aid | Reviewer: none | Agreement | 95% CI |
|---|---|---|---|---|---|
| Patient lift (100) | 90 | 1 | 9 | 0.90 | 0.83–0.95 |
| None mentioned (100) | 5 | 1 | 94 | 0.94 | 0.88–0.97 |

Lifts the rule missed were described without a word for the device ("ran over foot"), with the model name redacted ("[redacted] tilted"), or as "the transfer lift" in a fall by staff. The rule was not changed after the review.

Section 4 of `handling-equipment.ipynb` groups lift narratives by the part of lift use described. Those keyword groups were not validated, one of them is known to mix resident falls with staff falls, and none of their counts are cited.

## What this data cannot show

- **Mention is not use.** A narrative that names no equipment does not show none was used.
- **No rates.** The data counts injuries, not transfers; it cannot compare the safety of transfers with and without equipment.
- **No product judgment.** No product, brand, or manufacturer is named or evaluated.
- **One reviewer.** The review was done by one person, the author.

## Files

| File | Contents |
|---|---|
| `handling-equipment.ipynb` | Equipment groups, severity, validation sample, with outputs |
| `equipment-review.ipynb` | Blind-review scoring; reads only published files |
| `derived/equipment-by-group.csv`, `derived/severity-by-equipment.csv` | Results by rule group |
| `derived/equipment-validation-sample.csv` | The 200 reviewed narratives (public OSHA text) |
| `derived/equipment-review-labels.csv` | Reviewer labels |
| `derived/equipment-review-*.csv`, `derived/equipment-corrected-counts.csv` | Review results |

## Disclosure

The author distributes a powered patient transfer system. No product is named, evaluated, or compared here.

## License

Code: MIT. Derived files: CC-BY-4.0. Source data is US Government work and not subject to copyright.

Yuxuan Huang · Vantara Medical Equipment (Mahoraga Vantara LLC)
