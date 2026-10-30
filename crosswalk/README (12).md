# OSHA ↔ CMS Nursing-Facility Crosswalk

A table linking OSHA Injury Tracking Application establishments to CMS-certified nursing homes, so that workplace-injury data and CMS quality, staffing, and inspection data can be analyzed for the same facilities.

All source data is public US Government work (17 U.S.C. §105). The crosswalk is reproducible from the notebook in this folder.

## Why this is needed

| | Identifier | Also has |
|---|---|---|
| OSHA Injury Tracking Application | OSHA establishment ID | Employer ID number (EIN), name, address |
| CMS nursing-home files | CMS Certification Number (CCN) | Name, address, certified beds |

The two systems share no identifier. Anyone who wants to ask whether staff injuries relate to resident outcomes, staffing, or inspection results must first link the two by name and address. This crosswalk does that once, grades every link, and publishes the result.

## What is published

`osha-cms-crosswalk-nf-2025.csv`, one row per matched pair.

| Column | Meaning |
|---|---|
| `osha_establishment_id` | OSHA ITA establishment ID |
| `cms_ccn` | CMS Certification Number (6 characters, keep as text) |
| `tier` | Match grade, A (strongest) to C (see below) |
| `name_score` | Similarity of normalized names, 0–1 |
| `addr_score` | Similarity of normalized street addresses, 0–1 |
| `hno_match` | House numbers are identical |
| `zip_match` | Five-digit ZIP codes are identical |

The table contains identifiers and scores only. Names and addresses remain in the source files.

## How matching works

1. **Normalize.** Addresses: lower-case, remove punctuation and suite numbers, standardize street suffixes and directions, extract the house number. Names: remove legal suffixes and words that appear in nearly every facility name (nursing, rehabilitation, center, healthcare, and similar). ZIP: first five digits.
2. **Block.** Compare only pairs in the same state and ZIP, or the same state, city, and house number (to catch ZIP typos).
3. **Score.** Name similarity by token-set ratio; address similarity by token-sort ratio.
4. **Grade.**

| Tier | Rule | Meaning |
|---|---|---|
| A | Same house number, address ≥ 0.85, name ≥ 0.60 | Same building, name agrees |
| B | Same house number and address ≥ 0.85; or name ≥ 0.90 and address ≥ 0.60 | Same building under another name, or same facility with the address written differently |
| C | Name ≥ 0.95 in the same ZIP, address unclear | Probable; use with care |

5. **Resolve.** Each OSHA establishment and each CMS facility is used at most once, best match first.

## Results

Sources: OSHA Form 300A summary data for 2025 (submissions through 2026-08-31); CMS nursing-home Provider Information, file dated 2026-10-01.

| | |
|---|---|
| OSHA establishments, NAICS 6231 | 9,539 |
| CMS-certified nursing homes | 14,686 |
| Candidate pairs scored | 21,821 |
| Matches | **8,047** |
| Share of OSHA establishments matched | 84.4% |
| Share of CMS facilities matched | 54.8% |

| Tier | Matches | Employees vs. certified beds (Spearman) | Median employees per bed |
|---|---|---|---|
| A | 6,654 | 0.579 | 1.09 |
| B | 1,165 | 0.533 | 1.09 |
| C | 228 | 0.428 | 1.02 |

Tiers A and B show similar size agreement, which indicates few wrong matches in tier B. Many tier B pairs carry entirely different names at the same street address, which is common where a facility's operating name differs from its licensed name or the operator has changed. Tier C agrees less well; reading its sample shows short or generic names (a single word matching within a longer name) that can coincide in the same ZIP code. Tier C should be excluded from analysis unless checked.

About half of CMS facilities are unmatched. Most of these are not failed matches: OSHA Form 300A submission is required only of establishments that meet size and industry criteria, so many facilities never appear in the OSHA file.

## How good the matches are

There is no hand-labeled truth set for this linkage. The notebook reports two checks:

- **Size agreement.** For correct matches, OSHA's employee count should rise with CMS's certified beds. The notebook reports the rank correlation and median employees per bed for each tier, written to `derived/match-quality-size-check.csv`. A tier with much weaker agreement contains more wrong matches.
- **Reading a sample.** Ten random matches per tier are printed for inspection when the notebook runs. That printed output names facilities, so it is cleared from the published notebook; re-running the cell reproduces it.

Match counts by tier and the source file dates are in `derived/match-summary.csv`.

## Recommended use

- Use tiers **A and B** for analysis. Report results with tier C included and excluded if it changes a conclusion.
- Keep `cms_ccn` as text; CCNs can begin with zero.
- An unmatched CMS facility usually means the facility was not required to submit to OSHA, not that matching failed. OSHA submission depends on establishment size.

## Application: staff injury rate and resident pressure-ulcer rate

Section 8 tests whether facilities with higher resident pressure-ulcer rates also have higher staff injury rates.

| | |
|---|---|
| Facilities (tiers A and B, over 10,000 hours worked, measure published) | 7,556 |
| Staff measure | DART rate: (days-away cases + restricted or transferred cases) × 200,000 ÷ hours worked, OSHA Form 300A, 2025 |
| Resident measure | CMS MDS: percentage of long-stay residents with pressure ulcers, four-quarter average (MDS Quality Measures file dated 2026-10-01) |
| Pearson r | 0.004 |
| Spearman ρ | 0.018 |

**There is no facility-level association.** Facilities where residents more often have pressure ulcers do not report more staff injuries. This does not show that the two problems are unrelated in any given facility; it shows that facility-level rates in these two federal datasets do not move together. A likely reason is measurement: most facilities report a few injury cases per year, too few to separate real differences from differences in reporting.

The result has held across data releases: r = −0.001 (n = 7,370) on the earlier OSHA release with CMS files dated 2026-08-01, and r = 0.004 (n = 7,556) on the current releases.

## Data release

OSHA republishes each year's files as late submissions arrive, and removes the earlier files when it does. CMS updates its nursing-home files monthly. Version 1.x of this repository used OSHA's release through 2026-03-15 and CMS files dated 2026-08-01 (8,975 OSHA establishments; 7,757 matches; A 6,406, B 1,128, C 223). The current figures use OSHA's release through 2026-08-31 and CMS files dated 2026-10-01. Match rate, tier structure, and size agreement changed little between the two.

## Limitations

- Match quality is assessed indirectly. Tier C is probable, not confirmed.
- Facilities renamed or sold between the OSHA submission and the CMS file date may be graded B or missed.
- Multi-building campuses may correspond to more than one record on either side; one-to-one assignment keeps only the best pair.
- The crosswalk reflects the file versions recorded in `derived/match-summary.csv`. CMS updates monthly; OSHA republishes as late submissions arrive.

## Reproducing

```
pip install duckdb pandas requests rapidfuzz scipy
jupyter notebook osha-cms-crosswalk.ipynb
```

When OSHA republishes, update the file names in Section 2. CMS files are located through the CMS catalog automatically.

## License

Code: MIT. Crosswalk and derived files: CC-BY-4.0. Source data is US Government work and not subject to copyright.

Yuxuan Huang · Vantara Medical Equipment (Mahoraga Vantara LLC), New York
