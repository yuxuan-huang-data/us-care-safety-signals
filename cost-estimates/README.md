# Cost Estimates for Handling Injuries

A scenario estimate of the workers' compensation and indirect costs of lost-time handling injuries counted in [`injury-rates/`](../injury-rates/). Every input is a count from this repository or a published unit cost with its source. **The result is a range built from averages, not a measured cost.**

## Inputs

**Counts** (OSHA Injury Tracking Application, 2025; `injury-rates/derived/lost-time.csv`): handling cases with at least one day away from work. Counts are shown as the rule finds them and adjusted by the precision measured by blind review in [`handling-task-types/`](../handling-task-types/) (hospital handling 0.80, nursing-facility handling 0.89, hospital lateral transfer 0.83).

**Unit costs**

| Input | Value | Source |
|---|---|---|
| Unit cost, low | $32,023 per claim | Average direct cost of a strain claim, OSHA [$afety Pays](https://www.osha.gov/safetypays/estimator_text.html), from NCCI workers' compensation data |
| Unit cost, high | $47,316 per claim | Average cost of all lost-time workers' compensation claims, accidents in 2022–2023, NCCI, reported by the [National Safety Council](https://injuryfacts.nsc.org/work/costs/workers-compensation-costs/) |
| Indirect cost ratio | 1.1 × direct | OSHA [$afety Pays](https://www.osha.gov/safetypays/background), claims of $10,000 or more |

## Results

Annual, 2025, establishments with 100 or more employees. Adjusted for measured precision.

| | Lost-time cases | Direct cost | Direct + indirect |
|---|---|---|---|
| **Hospital lateral transfers** | **about 620** | **$20M–$29M** | **$42M–$62M** |
| Hospital handling, all tasks | about 6,780 | $217M–$321M | $456M–$674M |
| Nursing-facility handling | about 1,840 | $59M–$87M | $124M–$182M |

Rule counts before adjustment, and the days away and restricted for each group, are in `derived/cost-estimates.csv`. Hospital lateral-transfer cases accounted for 28,358 days away from work and 41,597 days of restricted duty (rule count).

## Why these are low

- **Cases without days away from work are not costed**, including restricted-duty cases. They are 59% of hospital handling cases and 64% of nursing-facility handling cases.
- **Establishments with fewer than 100 employees are not counted.** OSHA does not collect case detail from them.
- **Handling narratives that name no task are not counted as lateral transfers**, though some are.
- **Costs outside the indirect ratio are excluded:** replacing staff who leave, legal costs, and any harm to patients.

## Why they are uncertain

- The unit costs are national averages across all industries and injuries. Healthcare claims, and handling claims in particular, may cost more or less.
- The $afety Pays figures and the indirect cost ratio rest on older research; the ratio in particular is a general estimate.
- The counts come from keyword rules with measured, not perfect, precision.

## Files

| File | Contents |
|---|---|
| `cost-estimates.ipynb` | The calculation, with outputs |
| `derived/cost-estimates.csv` | Estimates by group, rule count and adjusted |
| `derived/unit-costs.csv` | Unit costs and sources |

## License

Code: MIT. Derived files: CC-BY-4.0.

Yuxuan Huang · Vantara Medical Equipment (Mahoraga Vantara LLC)
