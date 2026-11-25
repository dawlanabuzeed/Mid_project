# UC-05 – View Monthly Reports (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Optional** – Viewing only; the logic is in UC-07/08/09.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-05 |
| **Use Case Name** | View Monthly Reports |
| **Primary Actor** | System Administrator |
| **Supporting Actors** | None |
| **Description** | The System Administrator views the generated Resource Utilization and SLA reports of a chosen month. |
| **Trigger** | The System Administrator opens "Reports". |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The System Administrator is logged in.

**Post-conditions – Success Guarantee**
1. The chosen report is displayed; no data is changed.

**Post-conditions – Failure / Minimal Guarantee**
1. No report is displayed; a message explains why.

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The System Administrator opens "Reports".
2. The system lists the months for which reports were generated.
3. The System Administrator selects a month and a report type (Resource Utilization or SLA).
4. The system displays the report: utilization percentages as a chart grouped by equipment type and staff role, or the SLA average time grouped by event type.

### Alternative Flows
**2a. No report for the wanted month**
1. The month the administrator wants is not in the list.
2. The administrator can generate it with UC-06 Generate Report for Selected Month. The use case ends.


## 6. Traceability (for the RTM later – not for VP)
FR 94, 96, 97.
