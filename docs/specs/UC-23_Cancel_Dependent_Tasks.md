# UC-23 – Cancel Dependent Tasks (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Must** – Extend use case of UC-22; story requires the count message + confirmation.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-23 |
| **Use Case Name** | Cancel Dependent Tasks |
| **Primary Actor** | Dispatcher (inherited from UC-22) |
| **Supporting Actors** | None |
| **Description** | When other non-final tasks depend on the cancelled task, the system shows how many will also be cancelled and, after confirmation, cancels them all and releases their resources. |
| **Trigger** | In UC-22 step 3, the system finds non-final tasks that depend (directly or indirectly) on the task. |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. UC-22 is running and has reached its extension point "Dependent Tasks Exist".

**Post-conditions – Success Guarantee**
1. The task and all its non-final dependent tasks are Cancelled; all their resources are released.
2. Each change is logged and pushed.

**Post-conditions – Failure / Minimal Guarantee**
1. No task is cancelled (the Dispatcher did not confirm).

## 3. Relationships
- **Extends:** UC-22 Cancel Task - at extension point "Dependent Tasks Exist" (step 3).
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The system shows a message stating how many dependent tasks will also be cancelled, and lists them.
2. The Dispatcher confirms.
3. In one transaction, the system cancels the task and all its non-final dependent tasks and releases all their resources.
4. The system writes each change to the audit log.
5. The system pushes the changes to all relevant workstations.

### Alternative Flows
**2a. Dispatcher does not confirm**
1. The Dispatcher clicks "No".
2. No task is cancelled. The use case ends.


## 6. Traceability (for the RTM later – not for VP)
FR 49-52.
