# UC-31 – Report Equipment Failure (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Recommended** – Story exception; starts the replacement flow (UC-24). Two actors.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-31 |
| **Use Case Name** | Report Equipment Failure |
| **Primary Actor** | Field Crew Member, Field Commander |
| **Supporting Actors** | None |
| **Description** | A Field Crew Member or the Field Commander reports from the field that an equipment item of their Active task has broken; it becomes Faulty at once and the Dispatcher is alerted. |
| **Trigger** | The user selects an equipment item of their Active task and clicks "Report Failure". |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The user is logged in and reserved to (or commands) an Active task that has equipment.

**Post-conditions – Success Guarantee**
1. The item is Faulty/In Maintenance and released from the task; the task stays Active with a 'missing resource' flag.
2. The Dispatcher has an alert to assign a replacement; the change is logged and pushed.

**Post-conditions – Failure / Minimal Guarantee**
1. The item's status is unchanged.

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The user opens their task and clicks "Report Failure".
2. The system lists the equipment items reserved to the task.
3. The user selects the broken item, optionally adds a short description, and submits.
4. The system immediately sets the item to Faulty/In Maintenance and releases it from the task.
5. The system keeps the task Active and flags it 'missing resource - replacement pending'.
6. The system writes the report to the audit log.
7. The system pushes the change and alerts the Dispatcher to assign replacement equipment.

### Exception Flows
**3a. Item already reported**
1. The item is already Faulty.
2. The system shows "Already reported". The use case ends.


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 7: push within 2 seconds.

## 6. Traceability (for the RTM later – not for VP)
FR 12, 13, 20, 21. NFR 7.

## 7. Open decisions for the group (not for VP)
- E1: task stays Active with a flag (proposal).
