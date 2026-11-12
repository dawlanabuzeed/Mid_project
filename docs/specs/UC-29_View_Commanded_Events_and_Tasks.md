# UC-29 – View Commanded Events and Tasks (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Optional** – Read-only view; the access rule (only own tasks) is the point.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-29 |
| **Use Case Name** | View Commanded Events and Tasks |
| **Primary Actor** | Field Commander |
| **Supporting Actors** | None |
| **Description** | The Field Commander views the events in which they command a task, and those tasks, on the events' live status screens. |
| **Trigger** | The Field Commander opens "My Events". |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The Field Commander is logged in.

**Post-conditions – Success Guarantee**
1. The Field Commander sees the events and tasks they command; no data is changed.

**Post-conditions – Failure / Minimal Guarantee**
1. Nothing is shown; a message explains why.

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The Field Commander opens "My Events".
2. The system lists the open events in which the Field Commander commands at least one task.
3. The Field Commander selects an event.
4. The system shows the event's live status screen with the Field Commander's own tasks marked.
5. The system updates the screen immediately when the server pushes a change.

### Alternative Flows
**2a. No commanded tasks**
1. The Field Commander commands no task.
2. The system shows "No assigned tasks". The use case ends.


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 1: access only to own events/tasks.
- NFR 7: push within 2 seconds.

## 6. Traceability (for the RTM later – not for VP)
FR 43, 45, 77. NFR 1, 7.
