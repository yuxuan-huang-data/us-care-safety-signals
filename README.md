# US Care Safety Signals

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.24408190.svg)](https://doi.org/10.5281/zenodo.24408190)

Analyses of public US federal data on injuries to caregivers and harm to patients during patient handling and transfer in hospitals and nursing facilities, with every classification rule validated against blind human review.

All source data is US Government work (17 U.S.C. §105). Every figure is reproducible from the notebooks in this repository.

## Key findings

**Caregivers (OSHA Injury Tracking Application, 2025)**

- In US nursing facilities, **5,654 of 25,860 reported injury cases (21.9%)** occurred while a caregiver was moving a resident. Nursing assistants and aides sustained 80.3% of them. [→](nursing-facility-handling/)
- **About four in five** of those narratives mention no lifting equipment. [→](handling-equipment/)
- In US hospitals, **about 1,440 reported injuries** (95% CI 1,230–1,570) occurred during lateral transfers of a lying patient between surfaces: bed to bed, bed to operating or imaging table, table to bed. Nursing facilities recorded 22. [→](handling-task-types/)
- **A nursing-facility worker is about twice as likely as a hospital worker to be injured moving a person**: 0.85 against 0.43 handling injuries per 100 full-time workers, adjusted for rule precision. [→](injury-rates/)
- **Rates have not moved in three years.** Hospital lateral-transfer injuries: 1,487, 1,207, 1,726 in 2023, 2024, 2025, at 0.035–0.046 per 100 full-time workers. [→](trends-2023-2025/)
- Lost-time handling injuries cost an estimated **$456M–$674M a year in hospitals and $124M–$182M in nursing facilities** in workers' compensation and indirect costs; **lateral transfers in hospitals, $42M–$62M**. These are low estimates built from published unit costs. [→](cost-estimates/)
- **2,291 nursing-facility injuries (8.9%)** resulted from violence by a person, almost always a resident. [→](resident-violence/)

**Patients (FDA MAUDE, 2015–2025)**

- Of 14,439 reports for patient lifts, transfer devices, and stretchers, **about 18% describe harm to a patient**, about 2,600 reports (2,100–3,190); 261 were filed as deaths. [→](maude-transfer-devices/)
- Among reports in which anyone was injured, **half describe a cause in use, training, maintenance, or accessories**, not a device defect. [→](maude-transfer-devices/)
- **Lateral transfers appear in under 1% of reports.** Staff injuries during lateral transfers are recorded nationally; harm to patients during them is not. [→](maude-transfer-devices/)
- **38% of reports describing patient injury were filed as "malfunction."** Counts of the reporter's own "injury" category are not counts of injured patients. [→](maude-transfer-devices/)

**Hospitals and facilities (CMS)**

- **719 hospitals** had Medicare payments reduced under the FY2026 Hospital-Acquired Condition Reduction Program; 95 of them were also rated worse than national on hospital-acquired pressure injury. [→](hospital-psi/)
- Across 1,697 linked hospitals, **staff handling-injury rates are not associated with patient pressure injury or in-hospital falls** (Spearman ρ = −0.07 and −0.06). The same was found across 7,556 nursing homes (r = 0.004). Reporting practice and case mix are at least as plausible an explanation as any protective effect. [→](hospital-crosswalk/)

## Analyses

| Directory | Subject | Source |
|---|---|---|
| [`nursing-facility-handling/`](nursing-facility-handling/) | Resident-handling injuries in nursing facilities | OSHA ITA |
| [`handling-task-types/`](handling-task-types/) | Which handling task was under way: lateral, seated, repositioning, floor recovery; hospitals and nursing facilities | OSHA ITA |
| [`handling-equipment/`](handling-equipment/) | Lifting equipment mentioned in handling-injury narratives | OSHA ITA |
| [`injury-rates/`](injury-rates/) | Injuries per 100 full-time workers by setting, state, and size; facility-level dataset of 7,554 establishments | OSHA ITA |
| [`trends-2023-2025/`](trends-2023-2025/) | The same rules and rates applied to each year OSHA has published | OSHA ITA |
| [`cost-estimates/`](cost-estimates/) | Workers' compensation and indirect cost of lost-time handling injuries | OSHA ITA, NCCI, OSHA $afety Pays |
| [`resident-violence/`](resident-violence/) | Violence against nursing-facility caregivers | OSHA ITA |
| [`maude-transfer-devices/`](maude-transfer-devices/) | Patient harm reported with lifts, transfer devices, and stretchers | FDA MAUDE |
| [`hospital-psi/`](hospital-psi/) | Hospital-acquired pressure injury and HACRP penalties, 3,056 hospitals | CMS |
| [`crosswalk/`](crosswalk/) | Links 8,047 OSHA establishments to CMS-certified nursing homes, graded A–C, precision measured by blind review | OSHA, CMS |
| [`hospital-crosswalk/`](hospital-crosswalk/) | Links 3,180 OSHA establishments to CMS hospitals; staff rates beside patient outcomes | OSHA, CMS |
| [`validation-oiics/`](validation-oiics/) | Out-of-sample test of the narrative rules against OSHA's own OIICS coding, 28,074 cases from 2024 | OSHA ITA |
| [`evaluation-protocol/`](evaluation-protocol/) | Pre-specified protocol for measuring staff and patient outcomes before and after a change in how patients are moved | — |
| [`usage-examples/`](usage-examples/) | Worked examples of joining the published files to CMS data | — |

Supporting material: [`benchmark/`](benchmark/) (500 labeled narratives used to develop the rules), [`data/`](data/) (tables by state and establishment size), [`signals/`](signals/) (machine-readable summaries), [`METHODOLOGY.md`](METHODOLOGY.md) (method for `hospital-psi/`).

## Dataset files

Three files are built to be reused on their own. All are CSV, CC BY 4.0, and identify facilities by federal identifier and state only — no facility name appears.

**OSHA–CMS nursing home crosswalk** — [`crosswalk/osha-cms-crosswalk-nf-2025.csv`](crosswalk/osha-cms-crosswalk-nf-2025.csv)
8,047 rows. Links OSHA establishments to CMS-certified nursing homes (CCN). Columns: `osha_establishment_id`, `cms_ccn`, `tier` (A/B/C), `name_score`, `addr_score`, `hno_match`, `zip_match`. A blind review of 150 pairs, stratified by tier, measures precision at 0.987 (95% CI 0.975–0.999) for tiers A and B, the recommended subset; the labels are published. Method and checks in [`crosswalk/`](crosswalk/).

**OSHA–CMS hospital crosswalk** — [`hospital-crosswalk/osha-cms-crosswalk-hospital-2025.csv`](hospital-crosswalk/osha-cms-crosswalk-hospital-2025.csv)
3,180 rows. Same structure, for CMS-certified hospitals. Method and checks in [`hospital-crosswalk/`](hospital-crosswalk/).

**Facility-level injury rates** — [`injury-rates/derived/facility-rates-2025.csv`](injury-rates/derived/facility-rates-2025.csv)
7,554 rows. Per-establishment employees, hours, case counts, handling and lateral-transfer counts, and rates per 100 full-time workers, with the CMS CCN and match tier where a crosswalk match exists.

OSHA and CMS share no facility identifier, so any analysis connecting staff injury to patient outcomes, staffing, or inspection results has to solve this linkage first. These files are that linkage, graded, with the quality checks and the review labels published alongside. Worked joins are in [`usage-examples/`](usage-examples/).


## How the work is done

- **Public sources only.** Every federal input is downloaded from the agency by the notebook itself.
- **Validated before counted.** Every rule that classifies free text is frozen, then scored against a blind human review; the results are published with the labels. The handling and violence rules are also tested against OSHA's own coding on a year they were not built on. Measured precision is applied to the counts that are cited.
- **Questions fixed in advance where it matters.** The FDA analysis was written as a plan and committed before its second review sample was drawn ([`analysis-plan.md`](maude-transfer-devices/analysis-plan.md)). The evaluation protocol is published before any pilot is run.
- **Errors and negative results are reported.** Each module states what did not hold, what a rule gets wrong, and any correction made.
- **No establishment, product, or manufacturer is named.** Facilities are identified by OSHA establishment ID or CMS Certification Number and state only. A recorded injury, report, or penalty does not by itself establish fault.
- **Data releases are recorded.** OSHA, CMS, and FDA republish their files. Each module states the release used and, where a module was re-run on a newer release, what changed.

## Environment

Notebooks were run on Google Colab with Python 3.13.18. Package versions are pinned in [`requirements.txt`](requirements.txt):

    pip install -r requirements.txt

The FDA module needs a free openFDA API key; see its README.

## Citation

Huang, Y. (2026). *US Care Safety Signals: federal data analyses of caregiver injury and patient harm during patient handling and transfer* (Version 2.0.1) [Data set]. Zenodo. https://doi.org/10.5281/zenodo.24408190

The DOI above always resolves to the latest version. Machine-readable metadata is in [`CITATION.cff`](CITATION.cff), which GitHub's "Cite this repository" button reads.

## License

Code: MIT ([`LICENSE`](LICENSE)). Data and derived files: CC-BY-4.0 ([`LICENSE-data`](LICENSE-data)). Source data is US Government work and not subject to copyright.

## Narrative text and product names

The case narratives are employer-written text published by OSHA. They are reproduced verbatim in the review samples so that the labels can be audited against the source. A few narratives name a product. Those are the employers' words. No product is evaluated, recommended, or compared anywhere in the analysis, and no inference about any product is drawn from these narratives. The token `hoyer` also appears in the frozen classification rule, where it stands for a mechanical floor lift — the genericized usage found in the narratives themselves.

## Disclosure

The author distributes a powered patient transfer system to US healthcare facilities. That is a reason every result here is reported whichever way it falls. No product is evaluated, recommended, or compared.

---

Yuxuan Huang · Mahoraga Vantara LLC
