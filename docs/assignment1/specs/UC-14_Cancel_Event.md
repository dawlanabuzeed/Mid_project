# UC-14 – Cancel Event (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Recommended** – Clear story rules (cancel all non-final tasks, release resources, record time).
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-14 |
| **Use Case Name** | Cancel Event |
| **Primary Actor** | Dispatcher |
| **Supporting Actors** | None |
| **Description** | The Dispatcher cancels an open event at any stage (for example a false report); all its non-final tasks are cancelled and their resources released. |
| **Trigger** | The Dispatcher clicks "Cancel Event" on an open event's status screen. |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The Dispatcher is logged in.
2. The event is Open.

**Post-conditions – Success Guarantee**
1. The event is marked Cancelled and its cancellation time is recorded automatically.
2. Every task of the event that was not final is Cancelled.
3. All resources of those tasks are released (Available; faulty equipment stays Faulty).
4. All changes are logged and pushed.

**Post-conditions – Failure / Minimal Guarantee**
1. The event and its tasks are unchanged.

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The Dispatcher clicks "Cancel Event" on the event's status screen.
2. The system shows how many non-final tasks will be cancelled and asks for confirmation.
3. The Dispatcher confirms.
4. In one transaction, the system sets every non-final task to Cancelled, releases their resources, marks the event Cancelled and records the cancellation time.
5. The system writes all changes to the audit log.
6. The system pushes the changes to all relevant workstations and removes the event from the open events list.

### Alternative Flows
**3a. Dispatcher does not confirm**
1. The Dispatcher clicks "No".
2. Nothing changes. The use case ends.


### Exception Flows
**4a. Event no longer open**
1. Another user closed or cancelled the event meanwhile.
2. The system shows a message and refreshes the screen. The use case ends.


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 7: push within 2 seconds.

## 6. Traceability (for the RTM later – not for VP)
FR 27-31. NFR 7.

## 7. Open decisions for the group (not for VP)
- Confirmation step: the story asks for it only for task cancellation; adding it here is a proposal.
