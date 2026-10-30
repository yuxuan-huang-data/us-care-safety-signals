# US Care Safety Signals

Analyses of publicly available US federal data on patient and caregiver
safety in healthcare facilities.

All source data is US Government work (17 U.S.C. §105). Every figure is
reproducible from the notebooks in this repository.

## Analyses

| Directory | Subject | Data sources |
|---|---|---|
| [`hospital-psi/`](hospital-psi/) | Hospital-acquired pressure injury and HACRP penalties across 3,056 US acute care hospitals | CMS Complications and Deaths, HAC Reduction Program |
| [`nursing-facility-handling/`](nursing-facility-handling/) | Resident-handling injuries: 5,654 of 25,860 nursing-facility injury cases reported for 2025 | OSHA Injury Tracking Application |
| [`resident-violence/`](resident-violence/) | Violence by residents and others against nursing-facility caregivers: 2,291 cases, 2025 | OSHA Injury Tracking Application |
| [`crosswalk/`](crosswalk/) | Links 8,047 OSHA establishments to CMS-certified nursing homes, graded by match quality | OSHA Form 300A; CMS Provider Information |
| [`validation-oiics/`](validation-oiics/) | Out-of-sample test of the handling and violence rules against OSHA's own OIICS coding, 28,074 cases from 2024 | OSHA Injury Tracking Application |

## Structure

    signals/                     Mapping of clinical problems to published regulatory measures
    hospital-psi/                Hospital PSI-03 and HACRP analysis
    nursing-facility-handling/   OSHA resident-handling injury analysis, with published validation sample
    resident-violence/           OSHA analysis of violence against nursing-facility caregivers
    crosswalk/                   OSHA establishment ID to CMS Certification Number, graded A to C
    validation-oiics/            Out-of-sample test of both narrative rules against OSHA's own coding
    benchmark/                   500 AI-labeled nursing-facility narratives used to develop the narrative rules
    data/                        Aggregated tables by state and establishment size, with column definitions
    METHODOLOGY.md               Method, data acquisition, and limitations for hospital-psi

Each directory has its own README with data sources, row counts, release
dates, and limitations.

## How the work is done

- **Public sources only.** Every federal input is downloaded from the agency by the notebook itself.
- **Validated before counted.** Where a figure depends on classifying free text, the rule is frozen and tested against a labeled random sample first, with the labeling method stated. The rules are then tested against OSHA's own coding on a year they were not built on. Precision, recall, and the labels are published.
- **Negative results are reported.** Each analysis states hypotheses that were tested and did not hold.
- **No establishment is named.** Facilities are identified by OSHA establishment ID or CMS Certification Number and state only. A recorded injury or a penalty does not by itself establish fault.
- **Data releases are recorded.** OSHA and CMS republish their files and remove earlier ones. Each README states the release used, and where a module was re-run on a newer release, what changed.

## Citation

See `CITATION.cff`.

## License

Code: MIT (`LICENSE`). Data and derived files: CC-BY-4.0 (`LICENSE-data`).
Source data is US Government work and not subject to copyright.

---

Yuxuan Huang · Vantara Medical Equipment (Mahoraga Vantara LLC), New York
