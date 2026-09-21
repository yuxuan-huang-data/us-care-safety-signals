# US Care Safety Signals

Analyses of publicly available US federal data on patient and caregiver
safety in healthcare facilities.

All source data is US Government work (17 U.S.C. §105). Every figure is
reproducible from the notebooks in this repository.

## Analyses

| Directory | Subject | Data sources |
|---|---|---|
| [`hospital-psi/`](hospital-psi/) | Hospital-acquired pressure injury and HACRP penalties across 3,056 US acute care hospitals | CMS Hospital Compare, HAC Reduction Program |
| [`nursing-facility-handling/`](nursing-facility-handling/) | Resident-handling injuries among 24,292 nursing-facility injury cases reported in 2025 | OSHA Injury Tracking Application |

## Structure

    signals/                     Mapping of clinical problems to published regulatory measures
    hospital-psi/                Hospital PSI-03 and HACRP analysis
    nursing-facility-handling/   OSHA resident-handling injury analysis, with hand-labeled validation sample
    METHODOLOGY.md               Method, data acquisition, and limitations for hospital-psi

Each analysis directory has its own README with data sources, row counts,
release dates, and limitations.

## How the work is done

- **Public sources only.** Every input is downloaded from a federal agency by the notebook itself.
- **Validated before counted.** Where a figure depends on classifying free text, the rule is tested against a random hand-labeled sample first. Precision, recall, and the labels are published.
- **Negative results are reported.** Each analysis states hypotheses that were tested and did not hold.
- **No establishment is named.** A recorded injury or a penalty does not by itself establish fault.

## Citation

See `CITATION.cff`.

## License

Code: MIT (`LICENSE`). Data and derived files: CC-BY-4.0 (`LICENSE-data`).
Source data is US Government work and not subject to copyright.

---

Yuxuan Huang · Vantara Medical Equipment (Mahoraga Vantara LLC), New York
