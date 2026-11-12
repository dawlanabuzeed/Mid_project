# UC-16 – Define Task Dependencies (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Must** – Main business rules of the task tree (Blocked only, cycle check, auto Ready); extended by UC-17.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-16 |
| **Use Case Name** | Define Task Dependencies |
| **Primary Actor** | Dispatcher |
| **Supporting Actors** | None |
| **Description** | The Dispatcher adds or removes prerequisite tasks for a Blocked task; the system rejects any change that would create a cycle. |
| **Trigger** | The Dispatcher selects a Blocked task and clicks "Edit Dependencies". |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The Dispatcher is logged in.
2. The task belongs to an open event and is in Blocked status.

**Post-conditions – Success Guarantee**
1. The task's prerequisites are updated and the dependency graph has no cycle.
2. If no open prerequisite remains, the task is Ready; otherwise it stays Blocked.
3. The change is logged and pushed.

**Post-conditions – Failure / Minimal Guarantee**
1. The task's prerequisites and status are unchanged; the Dispatcher sees the reason.

## 3. Relationships
- **Includes:** none
- **Extended by:** UC-17 Reject Cyclic Dependency - at extension point "Cycle Detected" (step 6).

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The Dispatcher selects a Blocked task and clicks "Edit Dependencies".
2. The system verifies that the task is in Blocked status.
3. The system displays the task's current prerequisites and the other tasks of the same event.
4. The Dispatcher adds and/or removes prerequisite tasks.
5. The Dispatcher clicks "Save".
6. The system checks that the new dependencies do not create a cycle.
7. The system saves the new prerequisites.
8. If none of the task's prerequisites is still open, the system changes the task to Ready.
9. The system writes the change to the audit log and pushes the updated task tree to all relevant workstations.

### Exception Flows
**2a. Task not Blocked**
1. The task is Ready, Active, Completed or Cancelled.
2. The system shows "Dependencies are locked in this status". The use case ends.

**6a. Cycle Detected (extension point -> UC-17 Reject Cyclic Dependency)**
1. The new dependencies would create a cycle.
2. UC-17 runs; nothing is saved.


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 7: push within 2 seconds.

## 6. Traceability (for the RTM later – not for VP)
FR 34-38, 42. NFR 7.

## 7. Open decisions for the group (not for VP)
- E13: only tasks of the same event can be prerequisites (proposal).
