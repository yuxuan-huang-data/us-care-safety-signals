# Hospital Crosswalk: OSHA Establishments to CMS Hospitals

Links hospitals that report injuries to OSHA with the same hospitals in CMS data, so staff injury rates can be set beside hospital quality measures. The method is the one used for nursing homes in [`crosswalk/`](../crosswalk/).

All source data is public US Government work (17 U.S.C. §105). Every figure below is reproducible from the notebook in this folder.

## Data

OSHA Injury Tracking Application Form 300A, 2025 (submissions through 2026-08-31), hospitals (NAICS 622). CMS Hospital General Information (modified 2026-10-26), Complications and Deaths (2026-10-20), and Hospital-Acquired Condition Reduction Program (2026-01-26), from the CMS provider-data catalog. Facility injury rates from [`injury-rates/`](../injury-rates/).

## The crosswalk

`osha-cms-crosswalk-hospital-2025.csv`: one row per matched pair, with OSHA establishment ID, CMS Certification Number, tier, and match scores. Names and addresses are used for matching and are not published.

| | Count |
|---|---|
| OSHA hospital establishments | 6,608 |
| CMS hospitals | 5,435 |
| **Matched** | **3,180** (48.1% of OSHA, 58.5% of CMS) |
| Tier A: same ZIP code, name score ≥ 0.80, or same house number with name ≥ 0.90 | 2,536 |
| Tier B: same house number and address score ≥ 0.65 | 539 |
| Tier C: name score ≥ 0.95 in the same city | 105 |

**Match quality.** Critical access hospitals are limited by law to 25 inpatient beds. In every tier, matched critical access hospitals report far fewer employees than matched acute care hospitals, which is what correct matching should produce:

| Tier | Acute care, median employees | Critical access, median employees |
|---|---|---|
| A | 1,059 (n = 1,688) | 196 (n = 567) |
| B | 1,092 (n = 362) | 178 (n = 127) |
| C | 1,091 (n = 59) | 186 (n = 34) |

Reading 90 matched pairs by eye: tier A pairs are nearly all the same hospital under slightly different names; tier C pairs have matching names and differently written addresses. **Tier B is the weakest:** it matches on address, so a hospital may be matched to its parent system, or to a different facility on the same campus (for example, a long-term acute care hospital inside a general hospital). The analysis below is therefore repeated on tier A alone.

## Staff injury rates beside patient outcomes

Tiers A and B, hospitals in the injury-rate population: 1,697 hospitals with a PSI-03 score. Spearman rank correlation.

| Staff measure | Patient measure | Hospitals | ρ | p |
|---|---|---|---|---|
| Handling-injury rate | Hospital-acquired pressure injury (PSI-03) | 1,697 | −0.066 | 0.007 |
| Handling-injury rate | In-hospital fall with hip fracture (PSI-08) | 1,706 | −0.062 | 0.011 |
| **Lateral-transfer injury rate** | Pressure injury (PSI-03) | 1,697 | **−0.003** | 0.91 |
| All-injury rate | Pressure injury (PSI-03) | 1,697 | −0.074 | 0.002 |

**Tier A alone** gives the same picture: −0.076, −0.051, −0.002, −0.085.

**Hospitals penalized under the FY2026 HAC Reduction Program had a lower median staff handling-injury rate** than those not penalized: 0.42 against 0.52 per 100 full-time workers (Mann-Whitney p = 0.014; tier A: 0.42 against 0.52, p = 0.007).

**At the facility level, staff handling-injury rates are not associated with worse patient outcomes.** The correlations are negative and very small; a rank correlation of −0.07 explains well under one percent of the variation. The same was found for nursing homes in [`crosswalk/`](../crosswalk/) (r = 0.004).

These results do not show that staff and patient safety are unrelated at the bedside. Two explanations are at least as plausible as any true protective effect:

- **Reporting.** A hospital that records staff injuries thoroughly may also run stronger safety systems. Injury rates measure what is recorded.
- **Size and case mix.** Large teaching hospitals treat sicker patients, which raises PSI measures, and they differ from small hospitals in staffing and recordkeeping. No adjustment for size or case mix was made.

The result is reported as found.

## What this data cannot show

- Matching is by name and address. Tier C is excluded from the analysis above; tier B is included and tested separately.
- A hospital system may report to OSHA as one establishment or as several. One-to-one assignment keeps only the best match for each.
- The analysis compares establishments, not the same staff and patients in the same events. An association, or its absence, does not establish cause.
- PSI measures are risk-adjusted by CMS; staff injury rates are not adjusted for anything.
- No hospital is named in any published file.

## Files

| File | Contents |
|---|---|
| `hospital-crosswalk.ipynb` | Matching and analysis, with outputs |
| `osha-cms-crosswalk-hospital-2025.csv` | The crosswalk: 3,180 pairs with tier and scores |
| `derived/match-summary.csv`, `derived/match-quality-by-type.csv` | Match counts and the size check |
| `derived/staff-vs-patient-measures.csv`, `-tier-a.csv` | Correlations |
| `derived/handling-rate-by-hacrp-penalty.csv`, `-tier-a.csv` | Rates by penalty status |
| `derived/cms-source-files.csv` | CMS files used and their modification dates |

## License

Code: MIT. Derived files: CC-BY-4.0. Source data is US Government work and not subject to copyright.

Yuxuan Huang · Vantara Medical Equipment (Mahoraga Vantara LLC)
