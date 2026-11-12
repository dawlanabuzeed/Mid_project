# UC-24 – Assign Replacement Equipment (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Recommended** – Story exception (equipment fails mid-task) and edge case E1.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-24 |
| **Use Case Name** | Assign Replacement Equipment |
| **Primary Actor** | Dispatcher |
| **Supporting Actors** | None |
| **Description** | After equipment fails during an Active task, the Dispatcher assigns an available item of the same type to the task. |
| **Trigger** | The Dispatcher clicks "Assign Replacement" on the failure alert or on the flagged task. |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The Dispatcher is logged in.
2. The task is Active and an equipment item of it was marked Faulty and released (UC-31).

**Post-conditions – Success Guarantee**
1. A replacement item of the same type is Reserved to Task for this task; the task's 'missing resource' flag is removed.
2. The change is logged and pushed.

**Post-conditions – Failure / Minimal Guarantee**
1. No item is reserved; the task stays Active with the 'missing resource' flag.

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The Dispatcher clicks "Assign Replacement" on the alert.
2. The system shows the failed item's type and the Available items of that type with their location.
3. The Dispatcher selects an item and confirms.
4. The system starts one SQL transaction, locks the item and checks that it is still Available.
5. The system reserves the item to the task, removes the 'missing resource' flag and commits.
6. The system writes the change to the audit log and pushes it to all relevant workstations.

### Exception Flows
**2a. No available item of that type (E1)**
1. No Available item of the needed type exists.
2. The task stays Active with the flag; the system will offer the replacement again when such an item becomes Available. The Dispatcher may also cancel the task (UC-22). The use case ends. **[DECIDE]**

**4a. Item taken meanwhile**
1. The item is no longer Available.
2. The system rolls back, shows a message, refreshes the list and the flow returns to step 3.


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 3, 7.

## 6. Traceability (for the RTM later – not for VP)
FR 21, 22. NFR 3, 7.

## 7. Open decisions for the group (not for VP)
- E1 proposal (see A1 file).
