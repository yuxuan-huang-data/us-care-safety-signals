# Patient Harm Reported with Patient-Transfer and Lifting Devices: FDA MAUDE, 2015–2025

The OSHA modules in this repository describe injuries to staff. This module describes the other side: harm to patients in reports filed with FDA about devices used to lift, transfer, or transport patients.

All source data is public US Government work (17 U.S.C. §105). Every figure below is reproducible from the notebook in this folder.

The questions, definitions, and rules were fixed in [`analysis-plan.md`](analysis-plan.md) before the second review sample was drawn.

## Data

FDA Manufacturer and User Facility Device Experience (MAUDE) reports, received 2015-01-01 to 2025-12-31, for nine FDA product codes, downloaded through the openFDA API.

| Group | Product codes |
|---|---|
| Patient transfer devices and aids | FRZ (powered), FMR (manual), IKX |
| Patient lifts | FNG (AC-powered), FSA (non-AC-powered) |
| Stretchers | FPO (wheeled), INK (wheeled, powered), FPP (hand-carried) |
| Powered patient transport | ILK |

The search returned 55 candidate codes; the 46 excluded (microscope slides, specimen transport media, arm slings, and others) are listed with reasons in `derived/product-codes.csv`.

**14,439 reports.** As filed by the reporter (event types are read from FDA's current file; FDA can update a report's event type after it is filed): 261 deaths, 2,115 injuries, 11,963 malfunctions, 100 other or blank.

## Method

The reporter's own event type does not reliably separate patient harm from staff harm or from device problems (see Question 5), and keyword rules misread report text (see Question 4). Each question is therefore answered from a blind human review of 400 reports drawn at random:

- **Sample A:** 200 reports (`random_state=2026`), reviewed in October 2026.
- **Sample B:** 200 further reports (`random_state=2027`) from the remaining 14,239, drawn after the analysis plan was committed.

The reviewer was the author, blind to any rule's output. Report text was shown up to its first 3,000 characters; 19 reports in Sample A and 21 in Sample B were longer. Report text names manufacturers and products, so it is not published. The labels are published, keyed by report number, in `derived/review-sample-A.csv` and `derived/review-sample-B.csv`; any report can be read on FDA's MAUDE site by its number.

## Results

### 1. Scale: reports describing harm to a patient

| Sample | Patient injured | Share | 95% CI |
|---|---|---|---|
| A | 32 of 200 | 16.0% | 11.6%–21.7% |
| B | 40 of 200 | 20.0% | 15.0%–26.1% |
| **A and B** | **72 of 400** | **18.0%** | **14.5%–22.1%** |

Applied to all 14,439 reports, about **2,600 reports (2,100–3,190) describe harm to a patient**. Staff were injured in a further 23 of the 400 (5.8%).

### 2. Stage at which the event arose

Among the 100 reviewed reports in which anyone was injured, by the cause the report itself states:

| Stage | All injured | Patient injured |
|---|---|---|
| Device defect | 37 | 27 |
| **Use or training** | **37** | **26** |
| Maintenance or use beyond service life | 7 | 3 |
| Accessory (sling, pad, hanger) incompatible or not from the device maker | 6 | 5 |
| Patient behavior | 3 | 2 |
| Environment | 2 | 2 |
| Not stated | 8 | 7 |
| **Total** | **100** | **72** |

**Half of the injuries (50 of 100) arose from how the equipment was used, maintained, or fitted with accessories, not from a defect in the device.** These are counts from 100 reports and are not extrapolated.

### 3. Lateral transfers

A lateral transfer moves a lying patient between two surfaces: bed to bed, bed to an operating, procedure, or imaging table, table to bed, or bed to stretcher.

| Task under way | Reports (of 400) |
|---|---|
| Lift with sling or hoist | 118 |
| Stretcher transport | 119 |
| Not transferring (device idle, being cleaned or inspected) | 65 |
| Not stated | 85 |
| Sit-to-stand | 8 |
| Repositioning in bed | 2 |
| **Lateral transfer** | **3 (0.75%; 95% CI 0.26%–2.18%)** |

**Lateral transfers are rarely recorded in national device surveillance.** This is not evidence that lateral transfers are safe for patients. MAUDE records only events involving a regulated device, and many lateral transfers are done by hand or with sheets. In [`handling-task-types/`](../handling-task-types/), OSHA case data record staff injuries during lateral transfers in hospitals; no comparable national record exists for the patients.

### 4. Patient-fall rules

Scored on Sample B against the review question "did the patient fall, slide from, or drop out of the device", answered whether or not the patient was injured (42 yes; 2 unclear, excluded).

| Rule | Flagged | Correct | Precision | Recall |
|---|---|---|---|---|
| Original (written before any review) | 49 | 35 | 0.714 | 0.833 |
| Revised (written while reading Sample A) | 29 | 26 | 0.897 | 0.619 |

The revised rule trades recall for precision. Neither rule is used for any count in this module.

**Correction recorded.** The original rule was first scored against Sample A and appeared to have a precision of 0.23. That review recorded only the most severe injury per patient and recorded a patient who fell without injury as no one injured, while the rule detects falls. The two measured different things; the error was in the design of the review question. Sample B asked about falls directly.

### 5. The reporter's event type compared with the review

| Event type as filed | Patient injured | Staff injured | No one injured | Unclear |
|---|---|---|---|---|
| Death (5) | 4 | 1 | 0 | 0 |
| Injury (56) | 41 | 10 | 4 | 1 |
| Malfunction (335) | 27 | 11 | 293 | 4 |
| Blank (4) | 0 | 1 | 3 | 0 |

**27 of the 72 patient injuries (38%) were filed as malfunctions**, and 10 of the 56 reports filed as injuries (18%) describe injury to staff. Counts of the reporter's "injury" type are therefore not counts of injured patients.

## What this data cannot show

- **Not rates.** MAUDE is a passive reporting system. Many events are never reported, and the number of times each device is used is unknown.
- **Not cause.** FDA states that a report does not establish that a device caused or contributed to an event. The stage in Question 2 is the cause the report states, not a finding.
- **Only device-involved events.** Harm during a transfer done by hand does not appear.
- **One reviewer.** The review was done by one person, the author. A second independent reviewer would strengthen it.

## Published files

| File | Contents |
|---|---|
| `maude-transfer-devices.ipynb` | Download, sampling, and every analysis above, with outputs |
| `analysis-plan.md` | Questions, definitions, and rules, committed before Sample B was drawn |
| `derived/product-codes.csv` | All 55 candidate product codes, which were used, and why the rest were excluded |
| `derived/review-sample-A.csv`, `derived/review-sample-B.csv` | Review labels by report number |
| `derived/q1-scale.csv` … `derived/q5-event-type-vs-review.csv` | Results tables |
| `derived/reports-by-year-type.csv` | Reports by year and filed event type |

No brand, model, or manufacturer appears in any published file.

## Reproducing

```
pip install -r ../requirements.txt
jupyter notebook maude-transfer-devices.ipynb
```

openFDA requires a free API key for this volume of requests from shared addresses; set `API_KEY` in Section 1 and remove it before saving. FDA updates MAUDE continually; the samples are selected by their published report numbers, so the review analysis does not depend on the file staying identical.

## Disclosure

The author distributes a powered patient transfer system to US healthcare facilities. No product is named, evaluated, or compared here.

## License

Code: MIT. Derived files: CC-BY-4.0. Source data is US Government work and not subject to copyright.

Yuxuan Huang · Vantara Medical Equipment (Mahoraga Vantara LLC)
