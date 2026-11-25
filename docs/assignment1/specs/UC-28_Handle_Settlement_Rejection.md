# UC-28 – Handle Settlement Rejection (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Must** – Extend use case of UC-26 (story step 4 of closing).
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-28 |
| **Use Case Name** | Handle Settlement Rejection |
| **Primary Actor** | Dispatcher (inherited from UC-26) |
| **Supporting Actors** | None |
| **Description** | If the billing system rejects the settlement (or does not answer), the event is not closed and the Dispatcher is alerted. |
| **Trigger** | In UC-26 step 6, the settlement is rejected or times out. |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. UC-26 is running and has reached its extension point "Settlement Not Approved".

**Post-conditions – Success Guarantee**
1. The event stays Open (marked 'Settlement pending') and no closing time is recorded.
2. The Dispatcher has an alert with the reason; the attempt is logged.

**Post-conditions – Failure / Minimal Guarantee**
1. (Same as success.)

## 3. Relationships
- **Extends:** UC-26 Close Event - at extension point "Settlement Not Approved" (step 6).
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The system does not close the event; it stays Open and is marked 'Settlement pending'. **[DECIDE]**
2. The system writes the attempt and its result (rejected or timeout) to the audit log.
3. The system shows the Dispatcher an alert with the reason.
4. The Dispatcher may run Close Event again later.

## 6. Traceability (for the RTM later – not for VP)
FR 85, 86.

## 7. Open decisions for the group (not for VP)
- E2: retries allowed without limit (proposal).
