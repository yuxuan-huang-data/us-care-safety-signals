# Evaluation Protocol for a Patient-Transfer Intervention

A pre-specified protocol for measuring what happens to staff and to patients when a hospital or nursing facility changes how it moves patients: new equipment, a new training programme, a revised safe-handling policy, or any combination.

It is written to be used by a facility, a researcher, or a vendor, with or without this repository's author. It is published before any pilot is run so that the measures, the analysis, and the commitment to publish cannot be chosen after the results are known.

**Why it exists.** The analyses in this repository found three gaps that no public dataset can close:

1. **No denominator.** OSHA records injuries, not transfers. Nothing in federal data says how often a lateral transfer is performed, so no public source can give the risk per transfer. ([`handling-task-types/`](../handling-task-types/))
2. **No patient-side surveillance of lateral transfers.** Of 400 FDA MAUDE reports reviewed for transfer and lifting devices, 3 described a lateral transfer. Staff injuries during lateral transfers are recorded nationally; harm to patients during them is not. ([`maude-transfer-devices/`](../maude-transfer-devices/))
3. **Half of device-related harm arises in use, not in the device.** Among 100 reviewed reports in which anyone was injured, 50 named use, training, maintenance, or accessories as the cause and 37 named a device defect. Equipment alone does not predict the outcome. ([`maude-transfer-devices/`](../maude-transfer-devices/))

A facility collects, as a matter of routine, exactly what is needed to close all three: it knows how many transfers it performs, it records patient incidents, and it records staff injuries. This protocol says how to use that.

## 1. Design

**Interrupted time series with a concurrent comparison group.** Both parts matter. A before-and-after comparison alone cannot separate the intervention from a seasonal change, a staffing change, or a change in reporting; a comparison group that does not change its practice shows what would have happened anyway.

| | |
|---|---|
| **Intervention units** | The units where the change is made. Name them before the baseline begins. |
| **Comparison units** | Units in the same facility, of similar type, size, and patient acuity, where practice does not change during the study. State how they were chosen. |
| **Baseline** | At least 12 months before the change. Shorter baselines cannot distinguish a trend from a fluctuation. |
| **Follow-up** | At least 12 months after. The first 1–3 months are reported separately as an implementation period, during which outcomes often worsen. |
| **Unit of analysis** | Unit-month. |

If only one unit can take part, the design becomes a single interrupted time series: state that, and treat the result as a case report.

## 2. Measures

Every measure below is one a facility already records. Nothing requires a new data collection system except item 3.1, which is the one genuinely new thing this protocol asks for.

### 2.1 Staff outcomes

Source: the facility's OSHA 300 log and workers' compensation records.

| Measure | Definition |
|---|---|
| Handling injuries | Recordable injuries occurring while a worker was moving, supporting, or catching a patient's body. Classified by the written standard in [`nursing-facility-handling/`](../nursing-facility-handling/). |
| Lateral-transfer injuries | Handling injuries occurring during a transfer of a lying patient between two surfaces, by the definition in [`handling-task-types/`](../handling-task-types/). |
| Days away from work | Per injury, from the 300 log. |
| Days of restricted duty or job transfer | Per injury, from the 300 log. |
| Lost-time injuries | Injuries with one or more days away. |

Rates use hours worked on the unit, per 100 full-time-equivalent workers (200,000 hours), as in [`injury-rates/`](../injury-rates/). Rates per transfer use item 3.1 as the denominator.

### 2.2 Patient outcomes

Source: the facility's incident reporting system and medical record.

| Measure | Definition |
|---|---|
| Falls during a transfer | A patient fall in which the incident report records a transfer as under way. |
| Hospital-acquired pressure injury, stage 2 and above | Present-on-admission excluded, as CMS defines it for PSI-03. |
| Skin tear or abrasion attributed to a transfer | As recorded. |
| Line, tube, or drain dislodgement during a transfer | As recorded. |
| Transfer-related patient incidents, any type | The union of the above. |

Rates use patient-days and, for transfer-specific measures, item 3.1.

### 2.3 Process and experience

| Measure | Source |
|---|---|
| Staff present per transfer | Observation or equipment log. |
| Time per transfer | Observation or equipment log. |
| Equipment available and in working order | Maintenance log; record downtime. |
| Proportion of eligible transfers using the intended method | Observation or equipment log. |
| Staff-reported physical effort | A validated scale, for example Borg CR10, at baseline and follow-up. |
| Staff turnover on the unit | Human resources records. |

**Adherence is a result, not a footnote.** If the intended method is used in a minority of eligible transfers, that is the finding, and the outcome measures must be read against it.

## 3. The denominator

### 3.1 Transfer count

**Count lateral transfers on each participating unit, every month, for the whole study.** This is what no public dataset contains, and it is what turns every measure above from a count into a rate.

Any of these is acceptable if applied the same way throughout:

- An equipment use log, where the equipment records each use.
- A tally kept on the unit.
- A structured sample: a full count on a fixed set of days each month, scaled by the number of days. State the sampling frame.
- An extract from the electronic record, if transfers are documented there.

State which method was used, and whether it changed. **A change in counting method between baseline and follow-up invalidates every rate per transfer**, so if the intervention itself introduces logging that did not exist before, baseline transfers must be counted by the method used before the change, and the comparison stated in those terms.

## 4. Confounding to record

Record these monthly for every unit, whether or not they are expected to change. Any analysis that does not report them cannot be interpreted.

- Patient census, patient-days, and a case-mix measure.
- Nursing hours per patient-day, and agency or temporary staff as a share of hours.
- Changes to the incident-reporting system, the 300 log recordkeeper, or reporting policy.
- Other safety initiatives started or stopped during the study.
- Unit closures, mergers, renovations, or bed changes.

**Reporting changes are the single most likely explanation for a change in recorded injuries.** A unit that starts a safe-handling programme often starts recording more carefully at the same time, which can make injuries appear to rise when risk has fallen. Record any such change, and if one occurs, say so beside the result.

## 5. Analysis

Specify before the baseline ends:

1. **Primary outcome.** One measure. The lateral-transfer injury rate per 1,000 lateral transfers is the natural choice where lateral transfers are the target; choose otherwise with a reason.
2. **Model.** Segmented regression of the unit-month series, estimating the level change and the slope change at the intervention point, with the comparison units included. Poisson or negative binomial for counts, with log person-time or log transfers as offset; state which and why.
3. **Implementation period.** The first 1–3 months after the change are excluded from the primary estimate and reported separately.
4. **Secondary outcomes.** Listed in advance. Reported with the number of comparisons made.
5. **Missing data.** How handled, decided in advance.
6. **Power.** Lateral-transfer injuries are rare: a unit may record only a handful a year. State before starting what size of change the study could detect. **If the answer is "only a very large one," say so in the report rather than presenting a null result as evidence of no effect.**

## 6. Publication

- **The protocol is registered before the baseline ends**, with the primary outcome and analysis fixed, in a public registry or as a dated public document.
- **The result is published whichever way it falls**, including no effect and an effect in the wrong direction.
- **Deviations from this protocol are reported**, with the date of each and the reason.
- **Analysis code and de-identified unit-month data are published**, as every module in this repository does.

A vendor may fund or take part in a study using this protocol. If so: the funding and the vendor's role in design, analysis, and the decision to publish are stated in the report, and the agreement gives the facility or the researcher the right to publish regardless of the result. **A study that does not grant that right should not be run under this protocol.**

## 7. What this protocol cannot deliver

- **One site is a case report.** It cannot establish an effect for other facilities.
- **It cannot blind.** Staff know what they are using, which can change both practice and reporting.
- **Rare outcomes need long follow-up or many units.** A single unit cannot measure a change in rare harms.
- **It measures what is recorded.** Injuries and incidents not reported do not appear.
- **An interrupted time series is not a randomised trial.** It cannot rule out an unmeasured change that happened at the same time as the intervention.

## Disclosure

The author distributes a powered patient transfer system to US healthcare facilities. That is the reason this protocol is published before any pilot is run, fixes its measures in advance, and requires publication of the result whichever way it falls.

## License

CC-BY-4.0. Reuse, adapt, and extend it; attribution is enough.

Yuxuan Huang · Vantara Medical Equipment (Mahoraga Vantara LLC)
