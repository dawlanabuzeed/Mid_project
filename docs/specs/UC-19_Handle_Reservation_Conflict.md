# UC-19 – Handle Reservation Conflict (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Must** – Extend use case for the main Part B error (two Dispatchers, same resource).
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-19 |
| **Use Case Name** | Handle Reservation Conflict |
| **Primary Actor** | Dispatcher (inherited from UC-18) |
| **Supporting Actors** | None |
| **Description** | When a selected resource is no longer available at commit time, the system rolls back the whole reservation, keeps the task Ready, and shows the Dispatcher the current state. |
| **Trigger** | In UC-18 step 6, the system finds that at least one selected resource is no longer Available. |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. UC-18 is running and has reached its extension point "Resource Unavailable at Commit".

**Post-conditions – Success Guarantee**
1. No resource of the package is reserved (full rollback); all resources keep their previous status.
2. The task stays Ready.
3. The Dispatcher sees which resources were unavailable and the current resource status.

**Post-conditions – Failure / Minimal Guarantee**
1. (Same as success: nothing is reserved.)

## 3. Relationships
- **Extends:** UC-18 Reserve Resource Package Manually - at extension point "Resource Unavailable at Commit" (step 6).
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The system rolls back the whole transaction; no resource of the package is reserved.
2. The system keeps the task in Ready status.
3. The system notifies the Dispatcher which selected resources were unavailable.
4. The system refreshes the Dispatcher's screen with the current resource status.
5. The system writes the failed attempt to the audit log. **[DECIDE]**
6. The Dispatcher may select replacements for the unavailable resources; the flow returns to UC-18 step 4.

### Alternative Flows
**6a. Dispatcher gives up**
1. The Dispatcher closes the reservation window.
2. The task stays Ready. The use case ends.


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 3, 4, 9 (see UC-18).

## 6. Traceability (for the RTM later – not for VP)
FR 58-62. NFR 3, 4, 9.
