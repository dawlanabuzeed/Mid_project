# UC-13 – Open Event (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Recommended** – Starts every scenario; short and clear.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-13 |
| **Use Case Name** | Open Event |
| **Primary Actor** | Dispatcher |
| **Supporting Actors** | None |
| **Description** | The Dispatcher opens a new emergency event with a name, type and location; the opening time is recorded automatically. |
| **Trigger** | The Dispatcher clicks "New Event". |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The Dispatcher is logged in.

**Post-conditions – Success Guarantee**
1. A new event with status Open exists, with its opening time recorded automatically.
2. A live status screen exists for the event and is shown on all Dispatchers' workstations.
3. The opening is written to the audit log.

**Post-conditions – Failure / Minimal Guarantee**
1. No event is created.

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The Dispatcher clicks "New Event".
2. The system displays the event form with the closed list of event types.
3. The Dispatcher enters a name, selects a type (Building Collapse, Fire, Road Accident, Hazardous Materials Leak, Natural Disaster) and enters the location.
4. The Dispatcher clicks "Open".
5. The system validates that all fields are filled.
6. The system creates the event with status Open and records the opening time automatically.
7. The system writes the opening to the audit log.
8. The system creates the event's live status screen and pushes it to all Dispatchers' workstations.

### Alternative Flows
**4a. Dispatcher cancels**
1. The Dispatcher clicks "Cancel".
2. No event is created. The use case ends.


### Exception Flows
**5a. Missing field**
1. A field is empty.
2. The system highlights it and the flow returns to step 3.


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 7: push within 2 seconds.

## 6. Traceability (for the RTM later – not for VP)
FR 24-26, 68. NFR 7.

## 7. Open decisions for the group (not for VP)
- Location format: address text, or coordinates? (auto-allocation by distance needs coordinates).
