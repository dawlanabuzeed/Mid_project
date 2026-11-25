# UC-08 – Generate SLA Report (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Must** – This is the Part B operation (2): its rules become FRs, RTM rows and BDD tests.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-08 |
| **Use Case Name** | Generate SLA Report |
| **Primary Actor** | None (included by UC-06 and UC-07) |
| **Supporting Actors** | None |
| **Description** | The system computes, for a given month, the average time from an event's opening until its last task was closed, grouped by event type. |
| **Trigger** | An including use case (UC-06 or UC-07) asks for the SLA report of a month. |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The including use case has supplied a month that has ended.

**Post-conditions – Success Guarantee**
1. An SLA report for the month exists, with one row per event type: number of events counted and average time.

**Post-conditions – Failure / Minimal Guarantee**
1. No SLA report is produced; the including use case is told about the error.

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The system selects the events counted for the month: events whose last task reached a final state in that month, excluding cancelled events. **[DECIDE]**
2. For each counted event, the system computes the time from the event's opening time to the time its last task was closed.
3. The system groups the counted events by event type and computes the average time per type.
4. The system builds the report: event type, number of events, average time (hours:minutes).
5. The system returns the report to the including use case.

### Alternative Flows
**3a. No events of a type**
1. No counted event has a certain event type.
2. The system shows "No events" for that type instead of an average.


### Exception Flows
**1a. Database error**
1. The event data cannot be read.
2. The system returns an error to the including use case. The use case ends.


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 11: within 10 seconds for one month.
- NFR 16: one new class for a new report type.

## 6. Traceability (for the RTM later – not for VP)
FR 97. NFR 11, 16.

## 7. Open decisions for the group (not for VP)
- E11: count cancelled events? (proposal: no). Count cancelled tasks as 'last task'? (proposal: only Completed tasks). Event that spans two months belongs to the month its last task closed (proposal). Still-open events: not counted (proposal).
