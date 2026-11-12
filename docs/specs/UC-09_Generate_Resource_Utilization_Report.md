# UC-09 – Generate Resource Utilization Report (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Recommended** – Included by both report use cases, but not part of Part B.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-09 |
| **Use Case Name** | Generate Resource Utilization Report |
| **Primary Actor** | None (included by UC-06 and UC-07) |
| **Supporting Actors** | None |
| **Description** | The system computes, for a given month, each resource's utilization (share of the month's time it was reserved) and shows it graphically by equipment type and staff role. |
| **Trigger** | An including use case (UC-06 or UC-07) asks for the utilization report of a month. |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The including use case has supplied a month that has ended.

**Post-conditions – Success Guarantee**
1. A utilization report exists, with percentages per equipment type and per staff role, ready for chart display.

**Post-conditions – Failure / Minimal Guarantee**
1. No utilization report is produced; the including use case is told about the error.

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The system reads, for each staff member and equipment item, the periods in the month when it was Reserved to Task.
2. The system computes each resource's utilization: reserved time divided by the month's total time, times 100.
3. The system groups the results by equipment type and by staff role and computes the average per group.
4. The system builds the report data for a chart display.
5. The system returns the report to the including use case.

### Exception Flows
**1a. Database error**
1. The reservation data cannot be read.
2. The system returns an error to the including use case.


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 16: one new class for a new report type.

## 6. Traceability (for the RTM later – not for VP)
FR 94-96. NFR 16.

## 7. Open decisions for the group (not for VP)
- Resource added in the middle of the month: divide by the whole month (proposal) or by its time in the system?
