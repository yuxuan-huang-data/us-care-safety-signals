# Out-of-Sample Validation Against OSHA's Own Coding (OIICS)

Tests the two narrative classification rules in this repository, resident handling ([`nursing-facility-handling/`](../nursing-facility-handling/)) and violence ([`resident-violence/`](../resident-violence/)), against the injury codes OSHA attaches to each case using the Occupational Injury and Illness Classification System (OIICS).

All source data is public US Government work (17 U.S.C. §105). Every figure below is reproducible from the notebook in this folder.

## Why this test is independent

- **The rules had never seen this data.** Both were written on 2025 data and frozen. The test uses calendar year 2024.
- **The reference is OSHA's, not the author's.** OSHA added OIICS codes to its 2023 and 2024 case files. OSHA states these codes were generated in collaboration with the Bureau of Labor Statistics using BLS's automated coder; the file's column names end in `_pred` accordingly. They are an independent reference, not ground truth.

## What is compared

| Rule | OIICS counterpart (titles as they appear in the file) |
|---|---|
| Violence | Event code 11, "Violent acts by other person" |
| Handling | Event code 71, "Overexertion while moving or manipulating external object(s)", where the source is a person (source code 57, "Person--other than injured or ill worker", or another source naming a patient, resident, or client) |

## Results

OSHA ITA Case Detail Data, 2024 (release through 2025-12-31): 687,695 cases, all industries; **28,074** in US nursing care facilities (NAICS 6231).

| | Violence | Handling |
|---|---|---|
| OIICS count | 2,291 (8.2%) | 5,249 (18.7%) |
| Rule count | 2,202 (7.8%) | 5,105 (18.2%) |
| Precision | 0.810 | 0.695 |
| Recall | 0.779 | 0.676 |
| F1 | 0.794 | 0.685 |
| Cohen's kappa | 0.776 | 0.614 |

At the population level both rules agree with OSHA's coding to within half a percentage point. False positives and false negatives nearly cancel, so the shares reported in the analysis modules are close to what OSHA's own classification gives. Kappa of 0.776 is conventionally read as substantial agreement; 0.614 is at the upper end of moderate.

The OIICS violence count for 2024 (2,291) is the same number as the rule's violence count for 2025 in [`resident-violence/`](../resident-violence/). The two figures come from different years and different methods; the match is a coincidence.

## What the disagreements show

`derived/disagreement-sample.csv` holds 400 randomly drawn disagreements with both codes and both predictions. They fall into three groups.

**Differences in counting convention, not rule errors.** OIICS assigns one primary event per case; the rules can assign more than one label.

- "Employee twisted to avoid being kicked by a combative resident": OIICS codes bodily position and motion; the violence rule flags violence.
- "Using the Hoyer lift, foot slipped": OIICS codes a fall; the handling rule flags handling.

**Rule gaps.**

- Violence: "participant" is not in the rule's list of persons; the passive form "slapped by resident" is missing; an actor written only as "he" is not recognized.
- Handling: "pulling resident up in bed" does not match, because "resident" sits between "pulling" and "up"; "assisting a resident" followed by back pain carries no movement word, though OIICS codes it as handling; "turned around" (the employee turning) is misread as turning a resident.

**Information OSHA has that the narrative does not.** Some cases whose published narrative reads only "unknown" are coded as violence. OSHA's coding may draw on more than the published text.

## Why the rules were not changed

Tuning the rules against these disagreements would end the 2024 data's value as an independent test. Any revision will be developed on 2023 data, which also carries OIICS codes, and tested again on 2024.

## Reproducibility

This test was first run on 2026-09-21 and re-run from the published notebook on 2026-10-29. Both runs produced identical scores and an identical disagreement sample.

OSHA removes earlier files when it republishes. `derived/source-file.csv` records the download URL, the SHA-256 checksums of the zip and CSV files, and the row count, so a reader can confirm they have the same file.

```
pip install duckdb pandas requests
jupyter notebook validation-oiics.ipynb
```

## Published files

| File | Contents |
|---|---|
| `validation-oiics.ipynb` | The test, with outputs |
| `derived/rules-vs-oiics-2024.csv` | Scores for both rules |
| `derived/disagreement-sample.csv` | 400 random disagreements, with OIICS codes and rule predictions |
| `derived/source-file.csv` | Source URL, SHA-256 checksums, row count |

## Limitations

- OIICS is itself a coding judgment, produced with an automated coder. Agreement measures consistency with OSHA's coding, not with ground truth.
- OIICS records one primary event per case; the rules can record more than one. Some disagreement is structural.
- 481 of the 28,074 nursing-facility cases carry no event code. They count as neither violence nor handling on the OIICS side.
- The comparison inherits ITA's reporting thresholds; small facilities are under-represented.
- No establishment is named.

## License

Code: MIT. Derived files: CC-BY-4.0. Source data is US Government work and not subject to copyright.

Yuxuan Huang · Vantara Medical Equipment (Mahoraga Vantara LLC), New York
