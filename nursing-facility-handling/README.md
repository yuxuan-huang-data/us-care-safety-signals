# Resident-Handling Injuries in US Nursing Facilities: What OSHA's 2025 Case Data Shows

Analysis of employer-reported injury cases from US nursing care facilities (NAICS 6231), drawn from OSHA's Injury Tracking Application (ITA) case detail data for calendar year 2025.

All source data is public US Government work (17 U.S.C. §105). Every figure below is reproducible from the notebook in this folder.

## Key findings

In 2025, US nursing facilities reported 25,860 work-related injury and illness cases to OSHA. 5,654 of them (21.9%) occurred while a caregiver was moving a resident.

| | |
|---|---|
| Nursing-facility cases, 2025 | 25,860 |
| Resident-handling cases | 5,654 (21.9%) |
| Sustained by nursing assistants and aides | 4,541 (80.3%) |
| Days away from work | 58,178 |
| Days of restricted or transferred duty | 125,621 |
| Cases with zero days away | 3,598 (63.6%) |
| Cases over 90 days away | 215 |
| Longest absence | 348 days |

Restricted and transferred duty totals 2.2 times the days away from work. Those days do not appear in lost-time counts.

The distribution is long-tailed. Most handling injuries cost no lost time; 215 cost more than three months.

| Days away | Cases | Share |
|---|---|---|
| 0 | 3,598 | 63.6% |
| 1–7 | 1,088 | 19.2% |
| 8–30 | 504 | 8.9% |
| 31–90 | 249 | 4.4% |
| over 90 | 215 | 3.8% |

The back or lumbar region is named in the injury field of 2,071 handling cases (36.6%). Strain is the most common description.

## How accurate the classification is

OSHA does not code injuries by task. A case is classified as **handling** when its narrative names a person being cared for (resident, patient) *and* a movement of that person (transfer, lift, reposition, boost, turn, and related terms). The full rule is in Section 5 of the notebook and exported to `derived/classification-rule.txt`. The rule is frozen and used unchanged in `../resident-violence/`.

The rule was frozen, then checked against 200 randomly drawn nursing-facility cases. The sample was drawn blind to the rule: the file held only the narrative, not the rule's prediction. Each case was labeled `handling`, `care_general`, or `other` by an AI language model (Claude, Anthropic) under a written standard: a case is handling when the injury occurred while the employee was moving, supporting, or catching a resident's body. Sixteen boundary cases are explained in the `note` column. The author then labeled the same 200 cases independently, blind to the AI labels and to the rule's predictions, under the same written standard (see Human blind review below). Both sets of labels are published in `derived/validation-labeled.csv`, in the columns `label` (AI) and `human_label` (author), so that anyone can check them.

| Rule | Precision | Recall |
|---|---|---|
| Strict (used for all figures above) | 0.846 | 0.717 |
| Broad (adds movement described in other words) | 0.698 | 0.804 |

These scores use the AI labels as the reference. Scores against the author's labels are in the next section.

In the labeled sample, 46 of 200 cases were handling (23.0%; 95% CI 17.7%–29.3%). The strict rule's 21.9% falls inside that range. The broad rule gives 7,622 cases (29.5%), 79,573 days away and 161,604 days restricted; readers who prefer a higher-recall estimate can use those figures, at the cost of precision.

The sample was drawn from OSHA's earlier release of the same year's data (see Data release below). Precision and recall depend only on the labeled narratives, so they carry over unchanged.

### Human blind review

The author labeled all 200 cases without seeing the AI labels or the rule's predictions. A machine translation of each narrative into Chinese was shown beside the original English as a reading aid; the English text governed. Labels were not changed after the AI labels were seen. The comparison is reproduced by [`human-review.ipynb`](human-review.ipynb), which reads only the published file.

**Agreement between the two labelers**

| | Agreement | Cohen's kappa |
|---|---|---|
| Handling vs not handling | 91.5% | 0.758 |
| Three categories | 78.0% | 0.658 |

The AI labeled 46 cases handling; the author labeled 45. Most three-category disagreement is between `care_general` and `other`, a boundary that does not affect the handling rule.

**The frozen rule against each reference**

| Rule | Against AI labels (precision / recall) | Against author's labels (precision / recall) |
|---|---|---|
| Strict | 0.846 / 0.717 | 0.846 / 0.733 |
| Broad | 0.698 / 0.804 | 0.698 / 0.822 |

The rule's measured accuracy is nearly the same whichever labeler is the reference. In the author's labels, 45 of 200 cases were handling (22.5%; 95% CI 17.3%–28.8%); the strict rule's 21.9% falls inside that range.

The 17 handling disagreements fall into a few groups: moving equipment with no resident in it; catching or helping up a resident who was falling; a resident striking the employee just before or during a move; and "changing a resident" with no movement described. The cases are listed in the notebook.

**Independent test against OSHA's own coding.** The frozen strict rule was scored against OSHA's OIICS codes on 28,074 nursing-facility cases from 2024, a year the rule had never seen (see [`validation-oiics/`](../validation-oiics/)). The OIICS counterpart is event code 71, "Overexertion while moving or manipulating external object(s)", with a person as the source. Against it the rule scores precision 0.695, recall 0.676, and Cohen's kappa 0.614. It assigns 18.2% of cases to handling against OSHA's 18.7%. Part of the disagreement is a counting convention: OIICS assigns one primary event per case, while this rule can co-occur with other labels. Rule gaps found in the test are listed in `validation-oiics/` and were deliberately not fixed against the 2024 data.

## What did not hold, or cannot be shown

These are stated because they limit what this data can be used to argue.

**Fatigue.** If handling injuries accumulated as a shift went on, their share would rise with hours worked. The share of handling cases by hour into the shift is 5.9% in the first hour, between 10.6% and 12.6% in hours two through six, and then falls as shifts end (9.7% in the seventh hour, 4.4% in the ninth). Hours two through six rise slightly. This is a share of cases, not a rate per worker on duty, so it cannot separate fatigue from changes in staffing or in the tasks done at each hour. No claim is made that handling injuries accumulate with hours worked.

**Night shift severity.** Night-shift handling injuries average 32.3 days away (304 cases) against 27.5 for day shift (1,525 cases). The medians are 8.0 and 6.0 days. The difference is small, the night sample is small, and shift assignment depends on a boundary chosen by the analyst. No claim is made.

**Facility-level link to pressure injury.** Facilities with higher resident pressure-ulcer rates (CMS) do not have higher staff injury rates (OSHA). Across 7,556 facilities linked through the [`crosswalk`](../crosswalk/), the DART injury rate and the long-stay pressure-ulcer rate are uncorrelated (Pearson r = 0.004; Spearman ρ = 0.018; OSHA release through 2026-08-31, CMS files dated 2026-10-01). The result held on the earlier releases as well (r = −0.001, n = 7,370).

The likely explanation is measurement: a few injury cases per facility per year is too few to separate real differences from differences in reporting. The null result concerns facility-level correlation only.

## Data source

| Dataset | Scope | Source |
|---|---|---|
| ITA Case Detail Data, 2025 (submissions through 2026-08-31) | 782,582 cases, all industries; 25,860 in NAICS 6231 | osha.gov/itadata |

OSHA's server returns HTTP 403 without a browser User-Agent header; the notebook sets one.

## Data release

OSHA republishes each year's file as late submissions arrive, and removes the earlier file when it does. Version 1.x of this repository used the release covering submissions through 2026-03-15 (697,201 cases; 24,292 in NAICS 6231; 5,280 handling cases, 21.7%). That file is no longer available from OSHA. The current figures use the release through 2026-08-31. The late submissions added 1,568 nursing-facility cases; the handling share moved from 21.7% to 21.9% and the share sustained by nursing assistants and aides from 81.3% to 80.3%.

## Reproducing

```
pip install duckdb pandas requests
jupyter notebook nursing-facility-handling.ipynb
```

The notebook downloads the OSHA file and, if not present, `derived/validation-labeled.csv` from this repository. When OSHA republishes, update the file name in Section 2.

## Limitations

- ITA submission is required only of establishments meeting OSHA's size and industry thresholds. Small facilities are under-represented.
- Narratives are free text written by employers. Detail and wording vary.
- The handling classification is a keyword rule, not an OSHA field. Its precision and recall against AI labels, against the author's blind labels, and against OSHA's coding are reported above.
- The human review was done by one person, the author. A second independent human reviewer would strengthen it.
- Open cases undercount final days away.
- No establishment is named in this analysis. OSHA: recording an injury does not mean the employer was at fault or violated any OSHA rule.

## Disclosure

The author distributes a powered patient transfer system to US healthcare facilities. No product is named, evaluated, or recommended here.

## Citation

See `CITATION.cff` at the repository root.

## License

Code: MIT. Derived files: CC-BY-4.0. Source data is US Government work and not subject to copyright.

Yuxuan Huang · Vantara Medical Equipment (Mahoraga Vantara LLC), New York
