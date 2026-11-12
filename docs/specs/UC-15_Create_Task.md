# UC-15 – Create Task (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Recommended** – Defines the package that UC-18/20 reserve, and the Blocked/Ready start rule.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-15 |
| **Use Case Name** | Create Task |
| **Primary Actor** | Dispatcher |
| **Supporting Actors** | None |
| **Description** | The Dispatcher adds a task to an open event with a name, estimated duration, prerequisite tasks and the required resource package. |
| **Trigger** | The Dispatcher clicks "Add Task" on an open event's status screen. |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The Dispatcher is logged in.
2. The event is Open.

**Post-conditions – Success Guarantee**
1. The task exists in the event with status Blocked (if any prerequisite is not Completed) or Ready (otherwise).
2. The task appears in the event's task tree on all relevant workstations.
3. The creation is logged.

**Post-conditions – Failure / Minimal Guarantee**
1. No task is created.

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The Dispatcher clicks "Add Task".
2. The system displays the task form, with the event's existing tasks as possible prerequisites.
3. The Dispatcher enters the name and estimated duration and selects prerequisite tasks.
4. The Dispatcher defines the required resource package: staff roles and equipment types with quantities, including exactly one Field Commander.
5. The Dispatcher clicks "Create".
6. The system validates the data and the package.
7. The system creates the task with status Blocked if any prerequisite is not Completed, otherwise Ready.
8. The system writes the creation to the audit log and pushes the updated task tree to all relevant workstations.

### Alternative Flows
**3a. No prerequisites**
1. The Dispatcher selects no prerequisite.
2. The flow continues at step 4; the task will be created Ready.


### Exception Flows
**6a. Missing or invalid data**
1. The name is empty or the estimated duration is not positive.
2. The system highlights the field and the flow returns to step 3.

**6b. Package without exactly one Field Commander**
1. The package has zero or more than one Field Commander.
2. The system shows "A task needs exactly one Field Commander" and the flow returns to step 4.

**6c. Cancelled prerequisite**
1. A selected prerequisite is Cancelled, so it can never be completed.
2. The system rejects it and the flow returns to step 3. **[DECIDE]**


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 7: push within 2 seconds.

## 6. Traceability (for the RTM later – not for VP)
FR 32-34, 40, 41. NFR 7.

## 7. Open decisions for the group (not for VP)
- E10: the package holds a 'Field Commander' slot; the specific commander is chosen at reservation (proposal).
- E13: prerequisites only from the same event (proposal).
