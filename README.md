# Which Handling Tasks Appear in Injury Narratives: Hospitals and Nursing Facilities

What kind of patient movement was under way when a handling injury occurred: a lateral transfer between two surfaces, repositioning in bed, a seated or standing transfer, recovery from the floor, or catching a person who was falling?

All source data is public US Government work (17 U.S.C. §105). Every figure below is reproducible from the notebooks in this folder.

## Data

OSHA Injury Tracking Application case detail data, 2025 (submissions through 2026-08-31). Hospitals (NAICS 622): 181,690 cases. Nursing care facilities (NAICS 6231): 25,860 cases. Handling cases are identified with the frozen rule from [`nursing-facility-handling/`](../nursing-facility-handling/): 20,685 in hospitals (11.4%) and 5,654 in nursing facilities (21.9%).

## Task types

Each handling case is assigned the first task its narrative names, in this order:

1. **Lateral transfer between surfaces**: a lying person moved from one surface to another (bed to bed, bed to an operating, procedure, exam, or imaging table, table to bed, bed to or from a stretcher or gurney). A seated person moved from a wheelchair to a stretcher or table is a seated transfer.
2. Recovery from the floor
3. Catching or supporting a falling person
4. Seated or standing transfer
5. Repositioning in bed
6. Ambulation
7. Transfer, type not stated
8. Handling, task not stated

The full rule is in Section 3 of `handling-task-types.ipynb` and in `derived/classification-rule.txt`.

## Results

| Task | Hospitals | Share | Nursing facilities | Share |
|---|---|---|---|---|
| **Lateral transfer between surfaces** | **1,726** | **8.3%** | **22** | **0.4%** |
| Recovery from the floor | 280 | 1.4% | 145 | 2.6% |
| Catching or supporting a falling person | 780 | 3.8% | 230 | 4.1% |
| Seated or standing transfer | 1,942 | 9.4% | 788 | 13.9% |
| Repositioning in bed | 5,347 | 25.8% | 1,231 | 21.8% |
| Ambulation | 304 | 1.5% | 24 | 0.4% |
| Transfer, type not stated | 2,951 | 14.3% | 1,482 | 26.2% |
| Handling, task not stated | 7,355 | 35.6% | 1,732 | 30.6% |

Hospital lateral-transfer cases average 6.5 days away; 105 cases were away more than 90 days (`derived/severity-by-task.csv`).

**Lateral transfers are a hospital task.** Nursing facilities recorded 22 in the year; hospitals recorded over a thousand. Among hospital cases that name a specific task, lateral transfers are about one in six (1,726 of 10,379).

## How accurate the rules are

The author reviewed 200 cases blind: 150 drawn at random from handling cases (75 hospital, 75 nursing facility) and 50 further cases the rule called lateral, oversampled because lateral transfers are rare. The reviewer did not see the rule's output or know which cases were oversampled. Scoring is in [`task-review.ipynb`](task-review.ipynb); the reviewed cases are in `derived/task-review-sample.csv` and the labels in `derived/task-review-labels.csv`.

The reviewed sample was drawn before the lateral rule's final revision, which moved wheelchair-to-table transfers to seated transfers. Its 150 random cases are identical to those the final notebook draws; its 50 oversampled cases were selected by the earlier rule. The final rule is the one scored. `derived/task-validation-sample.csv` is the sample the final notebook draws; it has not been reviewed.

**Handling rule, share of flagged cases that are handling:**

| Setting | Handling / reviewed | Precision | 95% CI |
|---|---|---|---|
| Hospitals | 60 / 75 | 0.80 | 0.70–0.88 |
| Nursing facilities | 67 / 75 | 0.89 | 0.80–0.95 |

The handling rule was built and validated on nursing-facility narratives. In hospitals, where nearly every narrative mentions a patient, one flagged case in five is not a handling injury (for example, a needlestick during care).

**Lateral-transfer rule:**

| | Result |
|---|---|
| Precision, hospitals | 45 / 54 = 0.83 (95% CI 0.71–0.91) |
| Random-sample cases the reviewer called lateral that the rule also called lateral | 7 of 9 |

All nine cases the rule wrongly called lateral were seated transfers or catches: a patient lifted from a wheelchair onto an exam table, from a stretcher back to a wheelchair, from a stair chair to a gurney, or a transfer the narrative itself called "lateral" between a commode and a recliner. The rule was not changed after the review.

**Applying the measured precision:** about **1,440** of the 1,726 hospital cases the rule calls lateral are lateral transfers (95% CI about 1,230–1,570). This is a lower bound on hospital lateral-transfer injuries: it excludes the third of hospital handling narratives that name no task and the cases the rule missed. Among true handling cases in the random hospital sample, the reviewer found lateral transfers in 7 of 60 (11.7%).

## What this data cannot show

- **Injury counts are not task frequency.** The data counts injuries, not transfers. A task can account for few injuries because it is rare or because it is safe.
- **Many narratives name no task.** They are reported as such, not assigned a guess.
- **Case detail submission is required only of larger establishments in designated industries.** Small hospitals are under-represented.
- **One reviewer.** The review was done by one person, the author.
- **No establishment is named.** A recorded injury does not establish fault.

## Files

| File | Contents |
|---|---|
| `handling-task-types.ipynb` | Counts by task and setting, severity, validation sample |
| `task-review.ipynb` | Blind-review scoring; reads only published files |
| `derived/task-by-setting.csv`, `derived/severity-by-task.csv` | Results tables |
| `derived/task-review-sample.csv` | The 200 reviewed narratives (public OSHA text) |
| `derived/task-validation-sample.csv` | The sample the final notebook draws (not reviewed) |
| `derived/task-review-labels.csv` | Reviewer labels |
| `derived/review-*.csv` | Review results |

## Disclosure

The author distributes a powered lateral-transfer system. That is a reason to report this module's results whichever way they fall. No product is named or evaluated.

## License

Code: MIT. Derived files: CC-BY-4.0. Source data is US Government work and not subject to copyright.

Yuxuan Huang · Vantara Medical Equipment (Mahoraga Vantara LLC)
