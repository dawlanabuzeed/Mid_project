# UC-06 – Generate Report for Selected Month (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Must** – Part B covers generating the SLA report, and this is how it is generated on demand (and in the demo).
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-06 |
| **Use Case Name** | Generate Report for Selected Month |
| **Primary Actor** | System Administrator |
| **Supporting Actors** | None |
| **Description** | The System Administrator generates, on demand, the Resource Utilization and SLA reports for a selected past month. |
| **Trigger** | The System Administrator clicks "Generate Report" and selects a month. |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The System Administrator is logged in.

**Post-conditions – Success Guarantee**
1. The SLA report and the Resource Utilization report for the selected month are stored and displayed.

**Post-conditions – Failure / Minimal Guarantee**
1. No report is stored; the System Administrator sees an error message.

## 3. Relationships
- **Includes:** UC-08 Generate SLA Report; UC-09 Generate Resource Utilization Report.
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The System Administrator clicks "Generate Report".
2. The system asks for a month.
3. The System Administrator selects a past month and confirms.
4. The system validates that the month has already ended.
5. The system generates the SLA report for the month (includes UC-08 Generate SLA Report).
6. The system generates the Resource Utilization report for the month (includes UC-09 Generate Resource Utilization Report).
7. The system stores both reports and displays them.
8. The system writes the generation to the audit log.

### Alternative Flows
**7a. Reports already exist for the month**
1. Reports for that month already exist.
2. The system replaces them with the new version. **[DECIDE]**


### Exception Flows
**4a. Month not ended**
1. The selected month is the current month or a future month.
2. The system shows "Select a month that has ended" and the flow returns to step 3.

**5a. Generation error**
1. A database error occurs while generating.
2. The system stores no report and shows an error. The use case ends.


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 11: the SLA report for one month is generated within 10 seconds.
- NFR 16: adding a new report type requires one new class only.

## 6. Traceability (for the RTM later – not for VP)
FR 98 (+ FR 97 via UC-08, FR 94-96 via UC-09). NFR 11, 16.

## 7. Open decisions for the group (not for VP)
- Allow the current (unfinished) month? Proposal: no.
- Regenerating an existing month: replace (proposal) or keep both?
