# UC-18 – Reserve Resource Package Manually (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Must** – Part B operation (1); core concurrency rule of the story.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-18 |
| **Use Case Name** | Reserve Resource Package Manually |
| **Primary Actor** | Dispatcher |
| **Supporting Actors** | None |
| **Description** | The Dispatcher manually selects specific available staff members and equipment items for a Ready task's required resource package, and the system reserves the whole package in one all-or-nothing transaction, moving the task to Active. |
| **Trigger** | The Dispatcher selects a Ready task on an open event's live status screen and clicks "Reserve Resources Manually". |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The Dispatcher is logged in.
2. The task belongs to an open event (not closed or cancelled).
3. The task is in Ready status (all its prerequisite tasks are Completed).
4. The task has a defined required resource package that includes exactly one Field Commander.

**Post-conditions – Success Guarantee**
1. Every resource in the package is in status Reserved to Task for this task, and none of them is reserved to any other task.
2. The task is in Active status, and the selected Field Commander is recorded as the task's commander.
3. The task state change and every resource status change are written to the audit log with the exact time and the Dispatcher's user.
4. The change has been pushed to all relevant workstations (all Dispatchers' live status screens, the Field Commander, the reserved crew members).

**Post-conditions – Failure / Minimal Guarantee**
1. No resource of the package is reserved (0 reserved) and every resource keeps its previous status.
2. The task stays in Ready status.
3. The Dispatcher sees a clear failure message, and their screen shows the current resource status.
4. The failed attempt is written to the audit log. **[DECIDE]**

## 3. Relationships
- **Includes:** none
- **Extended by:** UC-19 Handle Reservation Conflict - at extension point "Resource Unavailable at Commit" (step 6).

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The Dispatcher selects a Ready task on the event's live status screen and chooses "Reserve Resources Manually".
2. The system verifies that the task is still in Ready status.
3. The system displays the task's required resource package (staff roles and equipment types) and, for each item, the list of currently Available matching staff members and equipment items, with their current location.
4. The Dispatcher selects one specific resource for each item in the package, including exactly one Field Commander. **[DECIDE]**
5. The Dispatcher confirms the reservation.
6. The system starts one SQL transaction, locks the records of all selected resources, and checks that every one of them is still Available.
7. The system sets all selected resources to Reserved to Task for this task, sets the task to Active, records the Field Commander as the task's commander, and commits the transaction.
8. The system writes the task state change and each resource status change to the audit log.
9. The system pushes the updated task and resource statuses to all relevant workstations and shows the Dispatcher a confirmation that the package was reserved.

### Alternative Flows
**4a. Incomplete or invalid selection**
1. The Dispatcher tries to confirm without selecting a resource for every package item, or with a number of Field Commanders other than one.
2. The system disables the confirmation and highlights the missing or invalid items.
3. The flow returns to step 4.

**4b. Dispatcher cancels the reservation**
1. The Dispatcher clicks "Cancel" before confirming.
2. The system discards the selection without reserving anything; the task stays Ready. The use case ends.


### Exception Flows
**2a. Task is no longer Ready**
1. The system finds that the task has changed status meanwhile (for example, another Dispatcher reserved it, or it was cancelled).
2. The system notifies the Dispatcher ("This task is no longer Ready") and refreshes the screen to the current state. The use case ends.

**3a. No available resource for a package item**
1. The system finds that at least one package item has no Available matching resource in the pool.
2. The system shows which item cannot be filled and does not allow confirmation; the task stays Ready.
3. The Dispatcher may leave the task Ready and try later. The use case ends.

**6a. Resource Unavailable at Commit (extension point -> UC-19 Handle Reservation Conflict)**
1. The system finds, inside the transaction, that at least one selected resource is no longer Available (reserved at the same moment by another Dispatcher, marked Faulty, or staff marked Unavailable).
2. UC-19 runs: the whole transaction is rolled back, nothing is reserved, and the task stays Ready.

**6b. Database or server error**
1. The transaction cannot be completed (database error or server timeout).
2. The system rolls back the whole transaction; no resource is reserved and the task stays Ready.
3. The system shows the Dispatcher an error message. The use case ends.


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 3: when two or more reservation requests target the same resource at the same moment, exactly one succeeds, in 100% of cases.
- NFR 4: a failed reservation leaves 0 resources of the package reserved.
- NFR 9: a manual reservation completes, successfully or with a failure message, within 2 seconds.
- NFR 7: status changes are pushed to all relevant workstations within 2 seconds, without client polling.
- NFR 13: the Dispatcher can reserve a package from the event status screen in no more than 5 clicks.

## 6. Traceability (for the RTM later – not for VP)
FR 18 (also covers a Field Commander already reserved to another task), 19, 33, 53-62. NFR 3, 4, 7, 9, 13.

## 7. Open decisions for the group (not for VP)
- E10: the specific Field Commander is chosen here, at reservation (proposal).
- Log failed attempts? (proposal: yes).
