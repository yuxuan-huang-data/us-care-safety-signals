# Analysis Plan: Patient Harm in FDA MAUDE Reports for Patient-Transfer and Lifting Devices

Written and committed 2026-11-07, before the second validation sample (Sample B) is drawn or read. The commit time of this file is the record that the questions, definitions, and rules below were fixed in advance.

## Data

FDA MAUDE reports received 2015-01-01 to 2025-12-31 for nine FDA product codes: FRZ, FMR, IKX (patient transfer devices and aids), FNG, FSA (patient lifts), FPO, INK, FPP (stretchers), ILK (powered patient transport). 14,439 reports. The selection and the reasons for excluding 46 other candidate codes are in `derived/product-codes.csv`.

Brand, model, and manufacturer are never kept as data. Report text names them, so no report text is published; published files carry report numbers only.

## Questions

1. **Scale.** What share of reports describe harm to a patient, and how many reports does that imply?
2. **Stage.** Among reports describing harm to anyone, at what stage of the device's use did the event arise?
3. **Visibility of lateral transfers.** How often do reports describe a lateral transfer of a lying patient between two surfaces?

Question 2 was added after the first review showed that many described events arose from maintenance, equipment age, accessory compatibility, or use, rather than from the device's design. It was not suggested by any count.

## Samples

- **Sample A.** 200 reports drawn at random (`random_state=2026`), reviewed blind in October 2026. Report numbers: `derived/maude-validation-ids.csv`.
- **Sample B.** 200 further reports drawn at random (`random_state=2027`) from the 14,239 reports not in Sample A. Report numbers: `derived/maude-validation-ids-B.csv`.

The reviewer does not see any rule's output for either sample.

## Review questions for Sample B

Each report is labeled on four questions.

1. **Did the patient fall, slide from, or drop out of the device?** `yes` / `no` / `unclear`. Answered whether or not the patient was injured.
2. **Who was injured?** `patient` / `staff` / `visitor` / `none` / `unclear`. If a patient and someone else were both injured: `patient`, with the other person noted.
3. **Patient's injury.** `fall`, `fracture`, `head`, `skin`, `pressure`, `entrapment`, `line`, `death`, `other`, `none`. The most severe applies: death, then fracture, then head, then the rest.
4. **Stage at which the event arose.** One of:
   - `device_defect`: a part broke, failed, or behaved contrary to its design during normal use, with no stated maintenance, age, accessory, or use problem.
   - `maintenance_age`: the report states missed or overdue service, use beyond the stated service life, or wear.
   - `accessory`: a sling, pad, hanger, or other accessory was incompatible, not from the device maker, or not suited to the device.
   - `use_training`: the report states the device was used contrary to its instructions, or a step was missed (for example, a strap not attached, a brake not set, one caregiver where two were required).
   - `patient_behavior`: the patient's own movement or behavior is stated as what led to the event.
   - `environment`: the floor, a threshold, a doorway, or another feature of the setting is stated as what led to the event.
   - `unclear`: the report does not say.

   Assign the stage the report itself states. Do not infer one the report does not state. If more than one is stated, choose the one stated as leading to the event.

Question 4 is also applied, in a second pass, to the 45 reports in Sample A in which anyone was injured. That pass uses the same definitions and is done before any Sample B result is computed.

## Rules tested against Sample B

**Patient fall (revised).** A patient word (patient, pt, resident, client, individual, user, occupant, he, she, they) followed within six words by a fall verb (fell, fall, fallen, dropped, slid off/out/down/from, slipped off/out/from/through, came out of the sling, landed on the floor, was lowered to the floor); or "dropped the patient/resident"; or "patient's fall". Excluded when the only fall stated is by staff, a nurse, caregiver, aide, technician, visitor, family member, crew, paramedic, EMT, operator, or attendant, and when the text states that no fall occurred or that a fall was prevented.

The revised rule was written while reading Sample A. Its score on Sample A is a development score only. Its accuracy is measured on Sample B against review question 1.

The original fall rule is also scored on Sample B against question 1.

## Analysis

1. **Scale.** The share of reports with `patient` injured, from Samples A and B combined (400), with a Wilson 95% interval, applied to the 14,439 reports. Each sample is also reported separately.
2. **Stage.** Counts by stage among reports with anyone injured, Sample A second pass and Sample B combined. Reported as counts with no extrapolation: the number of injured reports is too small to estimate national shares precisely.
3. **Lateral transfers.** The count of reports describing a lateral transfer, from Samples A and B.
4. **Rules.** Precision and recall of the original and revised fall rules on Sample B.
5. **Deaths and injuries as reported.** Counts of the reporter's own event type (Death 261; Injury 2,115) are reported as filed, with the caution that the reviewed samples show the "Injury" type includes staff injuries and that reports filed as "Malfunction" can describe patient injury.

Every result above is published whichever way it falls. Rules are not changed after Sample B is read; any further revision would be tested on a third sample.

## Correction recorded

In the first review, the injury question recorded only the most severe injury per patient, and a patient who fell without injury was recorded as no one injured. The original fall rule detects falls, not the most severe injury. Scoring the rule against that question gave a precision of 0.23, which understated the rule's accuracy for detecting falls because the two measured different things. The error was in the design of the review question. Sample B asks about falls directly (question 1).
