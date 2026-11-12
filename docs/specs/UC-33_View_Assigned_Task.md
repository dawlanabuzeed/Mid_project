# UC-33 – View Assigned Task (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Optional** – Read-only view.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-33 |
| **Use Case Name** | View Assigned Task |
| **Primary Actor** | Field Crew Member |
| **Supporting Actors** | None |
| **Description** | A Field Crew Member views the details of the task they are reserved to. |
| **Trigger** | The crew member opens "My Task". |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The crew member is logged in.

**Post-conditions – Success Guarantee**
1. The crew member sees their task's details; no data is changed.

**Post-conditions – Failure / Minimal Guarantee**
1. Nothing is shown; a message explains why.

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The crew member opens "My Task".
2. The system finds the task the crew member is reserved to.
3. The system shows the event's name, type and location, and the task's name, status, Field Commander, other reserved resources, estimated duration and elapsed time.
4. The system updates the details immediately when the server pushes a change.

### Alternative Flows
**2a. No assigned task**
1. The crew member is not reserved to any task.
2. The system shows "No task assigned". The use case ends.


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 7: push within 2 seconds.

## 6. Traceability (for the RTM later – not for VP)
FR 11. NFR 7.
