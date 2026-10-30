# Violence Against Nursing-Facility Caregivers: What OSHA's 2025 Case Data Shows

Analysis of employer-reported injury cases from US nursing care facilities (NAICS 6231) in which a resident, patient, visitor, or other person hit, kicked, bit, scratched, grabbed, or otherwise acted aggressively toward the employee. Source: OSHA Injury Tracking Application (ITA) case detail data, calendar year 2025.

All source data is public US Government work (17 U.S.C. §105). Every figure below is reproducible from the notebook in this folder.

## Key findings

In 2025, US nursing facilities reported 25,860 work-related injury and illness cases to OSHA. **2,291 of them (8.9%) resulted from violence by a person, almost always a resident.**

| | |
|---|---|
| Nursing-facility cases, 2025 | 25,860 |
| Violence cases | 2,291 (8.9%) |
| Occurred during a resident-handling task | 365 (15.9% of violence cases) |
| Sustained by nursing assistants and aides | 1,716 (74.9%) |
| Days away from work | 17,133 |
| Days of restricted or transferred duty | 22,596 |
| Cases with zero days away | 1,561 (68.1%) |
| Cases over 90 days away | 62 |
| Longest absence | 347 days |

Violence cases average 7.5 days away per case; resident-handling cases average 10.3 (see [`nursing-facility-handling/`](../nursing-facility-handling/)). Violence is less often disabling but still produces nearly 10 days of restricted duty per case.

### Who is injured

| Occupation | Cases | Share |
|---|---|---|
| Nursing assistant / aide | 1,716 | 74.9% |
| LPN / LVN | 210 | 9.2% |
| Other / unspecified | 174 | 7.6% |
| Registered nurse | 143 | 6.2% |
| Housekeeping / dietary | 26 | 1.1% |
| Therapy | 22 | 1.0% |

### How

A case can name more than one mechanism.

| Mechanism named | Cases | Share |
|---|---|---|
| Hit, punch, strike | 1,009 | 44.0% |
| Grab, twist, pinch | 520 | 22.7% |
| Bite | 336 | 14.7% |
| Kick | 278 | 12.1% |
| Scratch | 200 | 8.7% |
| Push, shove | 131 | 5.7% |
| Spit | 31 | 1.4% |

### Where on the body

A case can name more than one body region.

| Body region named | Cases | Share |
|---|---|---|
| Head, face, eye | 684 | 29.9% |
| Hand, finger, wrist | 635 | 27.7% |
| Arm, shoulder | 599 | 26.1% |
| Back, neck | 429 | 18.7% |
| Chest, abdomen | 317 | 13.8% |
| Leg, knee, foot | 247 | 10.8% |

State and establishment-size breakdowns are in [`data/`](../data/).

## How "violence" is defined

OSHA does not code injuries by cause in the narrative fields used here. A case is classified as **violence** when its combined narrative text names a person (resident, patient, client, visitor, family member) followed within a few words by a hostile act (hit, struck, punched, kicked, bit, scratched, slapped, spat, head-butted, pinched, squeezed, grabbed the employee, pushed the employee, wrapped arms around the employee), or describes the person as combative or aggressive together with such an act. Assaults by co-workers are included.

Two situations are deliberately excluded:

- **Resistance during care.** A resident who pulls back, pushes back, or stiffens during a transfer is classified as handling, not violence, unless a hostile act or aggressive behavior is also described.
- **Self-inflicted contact.** "Hit her head on the cabinet" is struck-against-object, not violence.

The full rule is exported to `derived/classification-rule.txt`. The handling rule used for the "during a resident-handling task" figure is the frozen rule from [`nursing-facility-handling/`](../nursing-facility-handling/), unchanged.

## How accurate the classification is

The rule was compared against the 500 nursing-facility cases in [`benchmark/`](../benchmark/). Their labels were produced by an AI language model and have not been reviewed by a human, so they are a silver standard.

| | |
|---|---|
| Cases labeled violence | 56 |
| Cases flagged by the rule | 51 |
| Precision | 0.961 |
| Recall | 0.875 |

**This is a development score.** The rule was written while reading the same 500 cases, so the figures above overstate out-of-sample accuracy. They are replaced by the independent test below.

Of the seven cases the rule misses, four describe resistance rather than aggression (pulling away, pushing back), which the labeling guide treats as a boundary case.

The score is computed on the 500 stored narratives only, so it does not change when OSHA republishes the case file.

**Independent test against OSHA's own coding.** The frozen rule was scored against OSHA's OIICS event codes on 28,074 nursing-facility cases from 2024, a year the rule had never seen (see [`validation-oiics/`](../validation-oiics/)). Against OIICS code 11, "Violent acts by other person", the rule scores precision 0.810, recall 0.779, and Cohen's kappa 0.776. It assigns 7.8% of cases to violence against OSHA's 8.2%. These lower figures are the accuracy estimate for this rule.

## What this data does not show

**Rates.** The case detail file lists only establishments that had cases. Facilities with none do not appear, so injuries per worker or per hour cannot be computed from this file alone. The shares above are shares of reported cases.

**Differences between states.** Among the 15 states with the most cases, the violence share ranges from 5.1% (Minnesota) to 13.2% (New York). Reporting practice, facility size, and resident acuity all differ between states. The range is reported, not interpreted.

**Small facilities.** Case detail submission is required only of establishments with 100 or more employees in designated industries. 24,521 of the 25,860 cases come from facilities of that size.

## Data source

| Dataset | Scope | Source |
|---|---|---|
| ITA Case Detail Data, 2025 (submissions through 2026-08-31) | 782,582 cases, all industries; 25,860 in NAICS 6231 | osha.gov/itadata |

## Data release

OSHA republishes each year's file as late submissions arrive, and removes the earlier file when it does. Version 1.x of this repository used the release covering submissions through 2026-03-15 (24,292 nursing-facility cases; 2,159 violence cases, 8.9%). That file is no longer available from OSHA. The late submissions in the current release added 132 violence cases; the violence share stayed at 8.9%, and the share sustained by nursing assistants and aides moved from 75.4% to 74.9%.

## Reproducing

```
pip install duckdb pandas requests
jupyter notebook resident-violence.ipynb
```

The notebook downloads the OSHA file and, if not present, the benchmark file from this repository. When OSHA republishes, update the file name in Section 2.

## Limitations

- Narratives are free text written by employers. Detail and wording vary.
- The violence classification is a keyword rule, not an OSHA field. Its development score and its out-of-sample accuracy against OSHA's coding are reported above.
- Open cases undercount final days away.
- **No establishment is named in this analysis.** OSHA: recording an injury does not mean the employer was at fault or violated any OSHA rule.

## Disclosure

The author distributes a powered patient transfer system to US healthcare facilities. No product is named, evaluated, or recommended here.

## License

Code: MIT. Derived files: CC-BY-4.0. Source data is US Government work and not subject to copyright.

Yuxuan Huang · Vantara Medical Equipment (Mahoraga Vantara LLC), New York
