# UC-30 – Report Task Completion (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Must** – Part B operation (1) explicitly includes reporting task completion.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-30 |
| **Use Case Name** | Report Task Completion |
| **Primary Actor** | Field Commander |
| **Supporting Actors** | None |
| **Description** | The Field Commander reports that a task they command is finished; the task becomes Completed, its resources are released, and dependent tasks whose prerequisites are all done become Ready. |
| **Trigger** | The Field Commander selects an Active task they command and clicks "Report Completion". |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The Field Commander is logged in.
2. The task is Active.
3. The Field Commander is the task's commander.

**Post-conditions – Success Guarantee**
1. The task is Completed and its completion time is recorded.
2. All its resources are Available again (faulty equipment stays Faulty), including the Field Commander.
3. Every dependent task whose prerequisites are now all Completed is Ready.
4. All changes are logged and pushed.

**Post-conditions – Failure / Minimal Guarantee**
1. The task and its resources are unchanged; the Field Commander sees the reason.

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The Field Commander selects an Active task they command and clicks "Report Completion".
2. The system verifies that the task is Active and that the user is its Field Commander.
3. The system asks for confirmation.
4. The Field Commander confirms.
5. In one transaction, the system sets the task to Completed, records the completion time, releases all its resources, and commits.
6. The system checks the tasks that depend on it and changes to Ready each one whose prerequisites are now all Completed.
7. The system writes the task change, each released resource and each promoted task to the audit log.
8. The system pushes all changes to all relevant workstations and shows the Field Commander a confirmation.

### Alternative Flows
**4a. Field Commander does not confirm**
1. The Field Commander clicks "No".
2. Nothing changes. The use case ends.


### Exception Flows
**2a. Not the task's commander or task not Active**
1. The user is not the task's commander, or the task is not Active.
2. The system rejects the action with a message. The use case ends.

**5a. Task cancelled at the same moment (E5)**
1. The Dispatcher's cancellation committed first.
2. The system changes nothing, shows "Task was already cancelled" and refreshes the screen. The use case ends.


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 7: push within 2 seconds.
- NFR 6: all changes logged.

## 6. Traceability (for the RTM later – not for VP)
FR 42, 44-47. NFR 6, 7.

## 7. Open decisions for the group (not for VP)
- E5: first commit wins (proposal).
