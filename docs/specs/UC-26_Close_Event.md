# UC-26 – Close Event (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Must** – Most complex flow: final-state check, cost calculation, settlement include, rejection extend.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-26 |
| **Use Case Name** | Close Event |
| **Primary Actor** | Dispatcher |
| **Supporting Actors** | National Billing & Settlement System (through UC-27) |
| **Description** | The Dispatcher closes an event whose tasks are all final; the system calculates operating costs, requests settlement from the external billing system, and closes the event only after approval. |
| **Trigger** | The Dispatcher clicks "Close Event" on an open event's status screen. |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The Dispatcher is logged in.
2. The event is Open.
3. All tasks of the event are Completed or Cancelled.

**Post-conditions – Success Guarantee**
1. The settlement approval and Transaction Token are stored.
2. The event is permanently Closed and its closing time is recorded automatically.
3. The closure and settlement are logged and pushed.

**Post-conditions – Failure / Minimal Guarantee**
1. The event stays Open and no closing time is recorded.
2. The Dispatcher receives an alert with the reason.

## 3. Relationships
- **Includes:** UC-27 Request Settlement (step 5).
- **Extended by:** UC-28 Handle Settlement Rejection - at extension point "Settlement Not Approved" (step 6).

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The Dispatcher clicks "Close Event".
2. The system verifies that all the event's tasks are Completed or Cancelled.
3. The system calculates the total operating costs: staff working hours, engine hours of engineering and medical equipment, and consumable materials. **[DECIDE]**
4. The system shows the cost summary and the Dispatcher confirms.
5. The system requests settlement from the National Billing & Settlement System (includes UC-27 Request Settlement).
6. The system receives an approval with a Transaction Token.
7. The system stores the approval and the Transaction Token, marks the event Closed and records the closing time.
8. The system writes the closure and settlement to the audit log.
9. The system pushes the change to all relevant workstations and shows the Dispatcher a confirmation.

### Alternative Flows
**4a. Dispatcher does not confirm**
1. The Dispatcher clicks "Cancel".
2. The event stays Open. The use case ends.


### Exception Flows
**2a. Not all tasks final**
1. At least one task is Blocked, Ready or Active.
2. The system lists those tasks and the event stays Open. The use case ends.

**6a. Settlement Not Approved (extension point -> UC-28)**
1. The billing system rejects the settlement, or does not answer in time (E2).
2. UC-28 runs.


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 6: closure and settlement logged.
- Billing timeout: 10 seconds (E2 proposal).

## 6. Traceability (for the RTM later – not for VP)
FR 78-86. NFR 6.

## 7. Open decisions for the group (not for VP)
- E6: who enters consumable materials, and when? (maybe ask in the course forum).
- Step 4 (Dispatcher confirms costs) is a proposal.
