# US Care Safety Signals

Analyses of public US federal data on injuries to caregivers and harm to patients during patient handling and transfer in hospitals and nursing facilities, with every classification rule validated against blind human review.

All source data is US Government work (17 U.S.C. §105). Every figure is reproducible from the notebooks in this repository.

## Key findings

**Caregivers (OSHA Injury Tracking Application, 2025)**

- In US nursing facilities, **5,654 of 25,860 reported injury cases (21.9%)** occurred while a caregiver was moving a resident. Nursing assistants and aides sustained 80.3% of them. [→](nursing-facility-handling/)
- **About four in five** of those narratives mention no lifting equipment. [→](handling-equipment/)
- In US hospitals, **about 1,440 reported injuries** (95% CI 1,230–1,570) occurred during lateral transfers of a lying patient between surfaces: bed to bed, bed to operating or imaging table, table to bed. Nursing facilities recorded 22. [→](handling-task-types/)
- **2,291 nursing-facility injuries (8.9%)** resulted from violence by a person, almost always a resident. [→](resident-violence/)

**Patients (FDA MAUDE, 2015–2025)**

- Of 14,439 reports for patient lifts, transfer devices, and stretchers, **about 18% describe harm to a patient**, about 2,600 reports (2,100–3,190); 261 were filed as deaths. [→](maude-transfer-devices/)
- Among reports in which anyone was injured, **half describe a cause in use, training, maintenance, or accessories**, not a device defect. [→](maude-transfer-devices/)
- **Lateral transfers appear in under 1% of reports.** Staff injuries during lateral transfers are recorded nationally; harm to patients during them is not. [→](maude-transfer-devices/)
- **38% of reports describing patient injury were filed as "malfunction."** Counts of the reporter's own "injury" category are not counts of injured patients. [→](maude-transfer-devices/)

**Hospitals and facilities (CMS)**

- **719 hospitals** had Medicare payments reduced under the FY2026 Hospital-Acquired Condition Reduction Program; 95 of them were also rated worse than national on hospital-acquired pressure injury. [→](hospital-psi/)
- Across 7,556 nursing homes linked between OSHA and CMS, **staff injury rates and resident pressure-ulcer rates are uncorrelated** (r = 0.004). [→](crosswalk/)

## Analyses

| Directory | Subject | Source |
|---|---|---|
| [`nursing-facility-handling/`](nursing-facility-handling/) | Resident-handling injuries in nursing facilities | OSHA ITA |
| [`handling-task-types/`](handling-task-types/) | Which handling task was under way: lateral, seated, repositioning, floor recovery; hospitals and nursing facilities | OSHA ITA |
| [`handling-equipment/`](handling-equipment/) | Lifting equipment mentioned in handling-injury narratives | OSHA ITA |
| [`resident-violence/`](resident-violence/) | Violence against nursing-facility caregivers | OSHA ITA |
| [`maude-transfer-devices/`](maude-transfer-devices/) | Patient harm reported with lifts, transfer devices, and stretchers | FDA MAUDE |
| [`hospital-psi/`](hospital-psi/) | Hospital-acquired pressure injury and HACRP penalties, 3,056 hospitals | CMS |
| [`crosswalk/`](crosswalk/) | Links 8,047 OSHA establishments to CMS-certified nursing homes, graded A–C | OSHA, CMS |
| [`validation-oiics/`](validation-oiics/) | Out-of-sample test of the narrative rules against OSHA's own OIICS coding, 28,074 cases from 2024 | OSHA ITA |

Supporting material: [`benchmark/`](benchmark/) (500 labeled narratives used to develop the rules), [`data/`](data/) (tables by state and establishment size), [`signals/`](signals/) (machine-readable summaries), [`METHODOLOGY.md`](METHODOLOGY.md) (method for `hospital-psi/`).

## How the work is done

- **Public sources only.** Every federal input is downloaded from the agency by the notebook itself.
- **Validated before counted.** Every rule that classifies free text is frozen, then scored against a blind human review; the results are published with the labels. The handling and violence rules are also tested against OSHA's own coding on a year they were not built on. Measured precision is applied to the counts that are cited.
- **Questions fixed in advance where it matters.** The FDA analysis was written as a plan and committed before its second review sample was drawn ([`analysis-plan.md`](maude-transfer-devices/analysis-plan.md)).
- **Errors and negative results are reported.** Each module states what did not hold, what a rule gets wrong, and any correction made.
- **No establishment, product, or manufacturer is named.** Facilities are identified by OSHA establishment ID or CMS Certification Number and state only. A recorded injury, report, or penalty does not by itself establish fault.
- **Data releases are recorded.** OSHA, CMS, and FDA republish their files. Each module states the release used and, where a module was re-run on a newer release, what changed.

## Environment

Notebooks were run on Google Colab with Python 3.13.18. Package versions are pinned in [`requirements.txt`](requirements.txt):

    pip install -r requirements.txt

The FDA module needs a free openFDA API key; see its README.

## Citation

See [`CITATION.cff`](CITATION.cff), or use "Cite this repository" on GitHub.

## License

Code: MIT ([`LICENSE`](LICENSE)). Data and derived files: CC-BY-4.0 ([`LICENSE-data`](LICENSE-data)). Source data is US Government work and not subject to copyright.

## Disclosure

The author distributes a powered patient transfer system to US healthcare facilities. That is a reason every result here is reported whichever way it falls. No product is named, evaluated, or compared.

---

Yuxuan Huang · Vantara Medical Equipment (Mahoraga Vantara LLC)
