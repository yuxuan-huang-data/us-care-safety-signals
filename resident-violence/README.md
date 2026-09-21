# Violence Against Nursing-Facility Caregivers: What OSHA's 2025 Case Data Shows

Analysis of employer-reported injury cases from US nursing care facilities (NAICS 6231) in which a resident, patient, visitor, or other person hit, kicked, bit, scratched, grabbed, or otherwise acted aggressively toward the employee. Source: OSHA Injury Tracking Application case detail data, calendar year 2025.

All source data is public US Government work (17 U.S.C. §105). Every figure below is reproducible from the notebook in this folder.

## Key findings

In 2025, US nursing facilities reported 24,292 work-related injury and illness cases to OSHA. **2,159 of them (8.9%) resulted from violence by a person, almost always a resident.**

| | |
|---|---|
| Nursing-facility cases, 2025 | 24,292 |
| Violence cases | 2,159 (8.9%) |
| Occurred during a resident-handling task | 345 (16.0% of violence cases) |
| Sustained by nursing assistants and aides | 1,628 (75.4%) |
| Days away from work | 15,435 |
| Days of restricted or transferred duty | 21,199 |
| Cases with zero days away | 1,475 (68.3%) |
| Cases over 90 days away | 56 |
| Longest absence | 312 days |

Violence cases average 7.1 days away per case; resident-handling cases average 10.3 (see [`nursing-facility-handling/`](../nursing-facility-handling/)). Violence is less often disabling but still produces nearly 10 days of restricted duty per case.

### Who is injured

| Occupation | Cases | Share |
|---|---|---|
| Nursing assistant / aide | 1,628 | 75.4% |
| LPN / LVN | 200 | 9.3% |
| Other / unspecified | 149 | 6.9% |
| Registered nurse | 136 | 6.3% |
| Housekeeping / dietary | 27 | 1.3% |
| Therapy | 19 | 0.9% |

### How

A case can name more than one mechanism.

| Mechanism named | Cases | Share |
|---|---|---|
| Hit, punch, strike | 955 | 44.2% |
| Grab, twist, pinch | 482 | 22.3% |
| Bite | 316 | 14.6% |
| Kick | 263 | 12.2% |
| Scratch | 187 | 8.7% |
| Push, shove | 125 | 5.8% |
| Spit | 31 | 1.4% |

### Where on the body

A case can name more than one body region.

| Body region named | Cases | Share |
|---|---|---|
| Head, face, eye | 646 | 29.9% |
| Hand, finger, wrist | 605 | 28.0% |
| Arm, shoulder | 560 | 25.9% |
| Back, neck | 391 | 18.1% |
| Chest, abdomen | 296 | 13.7% |
| Leg, knee, foot | 239 | 11.1% |

State and establishment-size breakdowns are in [`data/`](../data/).

## How "violence" is defined

OSHA does not code injuries by cause in the case detail file. A case is classified as **violence** when its combined narrative text names a person (resident, patient, client, visitor, family member) followed within a few words by a hostile act (hit, struck, punched, kicked, bit, scratched, slapped, spat, head-butted, pinched, squeezed, grabbed the employee, pushed the employee, wrapped arms around the employee), or describes the person as combative or aggressive together with such an act. Assaults by co-workers are included.

Two situations are deliberately excluded:

- **Resistance during care.** A resident who pulls back, pushes back, or stiffens during a transfer is classified as handling, not violence, unless a hostile act or aggressive behavior is also described.
- **Self-inflicted contact.** "Hit her head on the cabinet" is struck-against-object, not violence.

The full rule is exported to `derived/classification-rule.txt`.

## How accurate the classification is

The rule was compared against 500 randomly drawn nursing-facility cases carrying silver-standard labels (see [`benchmark/`](../benchmark/)).

| | |
|---|---|
| Cases labeled violence | 56 |
| Cases flagged by the rule | 51 |
| Precision | 0.961 |
| Recall | 0.875 |

**This is a development score.** The rule was written while reading the same 500 cases, so the figures above overstate out-of-sample accuracy. An independent check against a second, separately produced label set is planned and will replace these figures.

Of the seven cases the rule misses, four describe resistance rather than aggression (pulling away, pushing back), which the labeling guide treats as a boundary case.

## What this data does not show

**Rates.** The case detail file lists only establishments that had cases. Facilities with none do not appear, so injuries per worker or per hour cannot be computed from this file alone. The shares above are shares of reported cases.

**Differences between states.** Among the 15 states with the most cases, the violence share ranges from 4.7% (Minnesota) to 13.5% (New York). Reporting practice, facility size, and resident acuity all differ between states. The range is reported, not interpreted.

**Small facilities.** Case detail submission is required only of establishments with 100 or more employees in designated industries. 22,965 of the 24,292 cases come from facilities of that size.

## Data source

| Dataset | Scope | Source |
|---|---|---|
| ITA Case Detail Data, 2025 (submissions through 2026-03-15) | 697,201 cases, all industries; 24,292 in NAICS 6231 | osha.gov |

## Reproducing

```
pip install duckdb pandas requests
jupyter notebook resident-violence.ipynb
```

## Limitations

- Narratives are free text written by employers. Detail and wording vary.
- The violence classification is a keyword rule, not an OSHA field. Its development accuracy is reported above.
- Open cases undercount final days away.
- **No establishment is named in this analysis.** OSHA: recording an injury does not mean the employer was at fault or violated any OSHA rule.

## Disclosure

The author distributes a powered patient transfer system to US healthcare facilities. No product is named, evaluated, or recommended here.

## License

Code: MIT. Derived files: CC-BY-4.0. Source data is US Government work and not subject to copyright.

Yuxuan Huang · Vantara Medical Equipment (Mahoraga Vantara LLC), New York
