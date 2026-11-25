# UC-21 – Handle Allocation Failure (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Must** – Extend use case of UC-20.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-21 |
| **Use Case Name** | Handle Allocation Failure |
| **Primary Actor** | Dispatcher (inherited from UC-20) |
| **Supporting Actors** | None |
| **Description** | When no available alternative exists for a package item, automatic allocation fails as a whole and the task stays Ready. |
| **Trigger** | In UC-20 step 5 or 7, the system finds no Available alternative for a package item. |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. UC-20 is running and has reached its extension point "No Available Alternative".

**Post-conditions – Success Guarantee**
1. No resource is reserved (any open transaction is rolled back); the task stays Ready.
2. The Dispatcher knows which package item could not be filled.

**Post-conditions – Failure / Minimal Guarantee**
1. (Same as success.)

## 3. Relationships
- **Extends:** UC-20 Auto-Allocate Resource Package - at extension point "No Available Alternative".
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The system rolls back any open transaction; no resource is reserved.
2. The system keeps the task Ready.
3. The system notifies the Dispatcher that automatic allocation failed and which package item had no available resource.
4. The system writes the failed allocation to the audit log. **[DECIDE]**
5. The Dispatcher may try again later or reserve manually (UC-18).

## 6. Traceability (for the RTM later – not for VP)
FR 67.
