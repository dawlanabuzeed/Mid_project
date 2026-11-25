# UC-22 – Cancel Task (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Must** – Final-state rule, release of resources, and the cascade (extended by UC-23); also the E5 race with completion.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-22 |
| **Use Case Name** | Cancel Task |
| **Primary Actor** | Dispatcher |
| **Supporting Actors** | None |
| **Description** | The Dispatcher cancels a task that is not in a final status; its resources are released. |
| **Trigger** | The Dispatcher selects a task on the event status screen and clicks "Cancel Task". |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The Dispatcher is logged in.
2. The task is Blocked, Ready or Active (not final).

**Post-conditions – Success Guarantee**
1. The task is Cancelled and its resources are released (Available; faulty equipment stays Faulty).
2. The change is logged and pushed.

**Post-conditions – Failure / Minimal Guarantee**
1. The task and its resources are unchanged.

## 3. Relationships
- **Includes:** none
- **Extended by:** UC-23 Cancel Dependent Tasks - at extension point "Dependent Tasks Exist" (step 3).

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The Dispatcher selects the task and clicks "Cancel Task".
2. The system verifies that the task is not in a final status.
3. The system finds the non-final tasks that depend on it directly or indirectly; there are none.
4. The system asks the Dispatcher to confirm.
5. The Dispatcher confirms.
6. In one transaction, the system sets the task to Cancelled and releases its resources.
7. The system writes the changes to the audit log and pushes them to all relevant workstations.

### Alternative Flows
**5a. Dispatcher does not confirm**
1. The Dispatcher clicks "No".
2. Nothing changes. The use case ends.


### Exception Flows
**2a. Task already final**
1. The task is already Completed or Cancelled.
2. The system shows a message. The use case ends.

**3a. Dependent Tasks Exist (extension point -> UC-23)**
1. One or more non-final tasks depend on this task.
2. UC-23 runs instead of steps 4-7.

**6a. Completion reported at the same moment (E5)**
1. The Field Commander's completion committed first.
2. The system cancels nothing, shows "Task was already completed" and refreshes the screen. The use case ends.


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 7: push within 2 seconds.

## 6. Traceability (for the RTM later – not for VP)
FR 48, 52. NFR 7.

## 7. Open decisions for the group (not for VP)
- E5: first commit wins (proposal).
