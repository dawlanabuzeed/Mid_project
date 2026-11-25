# UC-20 – Auto-Allocate Resource Package (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Must** – Second reservation path (Part B reserving), with the fallback rule (up to 2 alternatives) and the demo requirement.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-20 |
| **Use Case Name** | Auto-Allocate Resource Package |
| **Primary Actor** | Dispatcher |
| **Supporting Actors** | None |
| **Description** | The Dispatcher asks the system to choose and reserve a Ready task's resource package automatically by a selected criterion (e.g. nearest to the event); on conflicts the system moves on to the next best alternative by itself. |
| **Trigger** | The Dispatcher selects a Ready task and clicks "Auto-Allocate". |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The Dispatcher is logged in.
2. The task belongs to an open event and is Ready.
3. The task has a defined resource package with exactly one Field Commander.

**Post-conditions – Success Guarantee**
1. The whole package is Reserved to Task for this task; the task is Active with its Field Commander recorded.
2. All changes are logged and pushed; the Dispatcher sees which resources were allocated.

**Post-conditions – Failure / Minimal Guarantee**
1. No resource is reserved and the task stays Ready; the Dispatcher sees why allocation failed.

## 3. Relationships
- **Includes:** none
- **Extended by:** UC-21 Handle Allocation Failure - at extension point "No Available Alternative" (steps 5 and 7).

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The Dispatcher selects a Ready task and clicks "Auto-Allocate".
2. The system verifies that the task is still Ready.
3. The system displays the available allocation criteria (for example, nearest available resources to the event location).
4. The Dispatcher selects a criterion and confirms.
5. The system chooses, for each package item, the best Available resource according to the criterion, including exactly one Field Commander.
6. The system starts one SQL transaction and locks the chosen resources.
7. The system checks that every chosen resource is still Available.
8. The system reserves all chosen resources, sets the task to Active, records the Field Commander, and commits.
9. The system writes all changes to the audit log.
10. The system pushes the changes to all relevant workstations and shows the Dispatcher the allocated resources.

### Alternative Flows
**7a. Resource taken by a concurrent allocation**
1. A chosen resource was taken at the same moment by another allocation.
2. The system replaces only that resource with the next best Available alternative by the same criterion, without asking the Dispatcher and without delaying other users.
3. The system repeats this up to 2 alternative attempts; when all resources are free, the flow continues at step 8.


### Exception Flows
**2a. Task no longer Ready**
1. The task changed status meanwhile.
2. The system shows a message and refreshes the screen. The use case ends.

**5a. No Available Alternative (extension point -> UC-21)**
1. No Available resource matches a package item.
2. UC-21 runs.

**7b. No Available Alternative after conflict (extension point -> UC-21)**
1. No alternative is left, or the 2 alternative attempts are used up. **[DECIDE]**
2. UC-21 runs.


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 3: exactly one of concurrent requests for the same resource succeeds (100%).
- NFR 10: allocation incl. up to 2 alternative attempts completes within 5 seconds, without raising other users' response times above 2 seconds.
- NFR 15: a new allocation criterion = one new class.

## 6. Traceability (for the RTM later – not for VP)
FR 63-67, 56-57. NFR 3, 10, 15.

## 7. Open decisions for the group (not for VP)
- 'Up to 2 alternative attempts': per conflicting resource or per allocation in total? (proposal: in total).
- Story says it fails 'only if there is no alternative in the pool' - what after 2 attempts? (proposal: fail -> UC-21).
