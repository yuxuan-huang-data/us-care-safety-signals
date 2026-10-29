# Benchmark: 500 Nursing-Facility Injury Narratives with Multi-Label Reference Labels

Reference set used to develop and check the narrative classification rules in this repository. Each row is one OSHA-recorded injury case from a US nursing care facility (NAICS 6231), with the employer-written narrative and one or more cause-of-injury labels.

All source text is public US Government work (17 U.S.C. §105). OSHA removes personal identifiers before publication; redactions appear as `[redacted]`.

## What is in the file

`benchmark-500-ai-prelabeled.csv`, 500 rows.

| Column | Meaning |
|---|---|
| `row` | Position of the case in the nursing-facility table of the OSHA release it was drawn from |
| `narr` | The four OSHA narrative fields combined and lower-cased |
| `source_sample` | `v1_200`: the 200 cases of the handling validation sample in [`../nursing-facility-handling/`](../nursing-facility-handling/). `v2_300`: 300 further cases drawn at random |
| `prior_handling` | For the `v1_200` cases, the label from the handling validation sample (`handling`, `care_general`, `other`); empty otherwise |
| `ai_labels` | Cause-of-injury labels, separated by `;` |
| `ai_uncertain` | `True` where the labeler marked the case as uncertain |
| `review` | Cases set aside for human review: `uncertain` (61) and `random_audit` (50) |
| `human_labels`, `note` | Reserved for that human review. Empty: the review has not been done |

## How it was drawn

All 500 cases come from OSHA's ITA Case Detail Data for 2025, release covering submissions through 2026-03-15. OSHA has since replaced that release (see the Data release section in [`../nursing-facility-handling/`](../nursing-facility-handling/)). The narratives are stored in full, so the file does not depend on that release being available.

## How it was labeled

Each case was labeled by an AI language model (Claude, Anthropic) with every label that applies, describing the cause of injury rather than the activity:

| Label | Cases | Meaning |
|---|---|---|
| `fall` | 121 | Employee slipped, tripped, or fell |
| `handling_planned` | 98 | Deliberately lifting, transferring, repositioning, turning, or walking a resident |
| `struck_caught` | 62 | Struck by or against, or caught in or between, an object |
| `violence` | 56 | A person hit, kicked, bit, scratched, grabbed, or was described as combative |
| `infection` | 44 | Infectious illness or exposure to body fluids |
| `sharps` | 29 | Needlestick or other sharps injury |
| `overexertion_object` | 28 | Strain from lifting, pushing, or carrying objects |
| `other` | 21 | None of the above |
| `handling_unplanned` | 20 | Catching or holding a resident who was falling at that moment |
| `unclear` | 19 | Too short or vague to decide |
| `chemical_thermal` | 15 | Chemical, burn, steam, electric shock, heat |

A case can carry more than one label, so the counts sum to more than 500.

**These labels were not reviewed by a human.** They are a silver standard. 111 cases were set aside for human review (the `review` column); that review has not been done, and the `human_labels` column is empty.

## How it is used

**Violence rule, development score.** The violence rule in [`../resident-violence/`](../resident-violence/) was written while reading these cases. Against the 56 cases labeled `violence`, it scores precision 0.961 and recall 0.875. Because the rule was developed on this set, these figures overstate out-of-sample accuracy. They are reported as a development score only.

**Handling rule.** The 200 `v1_200` cases are the same narratives as `../nursing-facility-handling/derived/validation-labeled.csv`. The handling rule's accuracy is reported there.

## Limitations

- Labels are an AI model's judgment under a written standard, not ground truth, and not human-reviewed.
- 500 cases is enough to estimate accuracy for common categories; for rare ones (`chemical_thermal`, 15 cases) the estimates are imprecise.
- The violence rule was developed on this set; its score here is not an independent test.

## License

Labels and derived columns: CC-BY-4.0. Source narratives are US Government work and not subject to copyright.

Yuxuan Huang · Vantara Medical Equipment (Mahoraga Vantara LLC), New York
