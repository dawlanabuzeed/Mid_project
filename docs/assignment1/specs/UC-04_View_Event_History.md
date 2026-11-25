# UC-04 – View Event History (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Optional** – Read-only screen; rules are simple (filters + read-only).
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-04 |
| **Use Case Name** | View Event History |
| **Primary Actor** | System Administrator |
| **Supporting Actors** | None |
| **Description** | The System Administrator views the list of open, closed and cancelled events, filters it, and inspects one event's tasks, resources, settlements and full action timeline (read-only). |
| **Trigger** | The System Administrator opens "Event History". |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The System Administrator is logged in.

**Post-conditions – Success Guarantee**
1. The requested history is displayed; no data is changed.

**Post-conditions – Failure / Minimal Guarantee**
1. No history is displayed and no data is changed; an error message is shown.

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The System Administrator opens "Event History".
2. The system displays all events (open, closed and cancelled) with name, type, status and dates.
3. The System Administrator sets filters: date range, event type and/or status.
4. The system displays the events that match the filters.
5. The System Administrator selects an event.
6. The system displays the event's tasks, the resources reserved to each task, the settlement approvals and Transaction Tokens, and the full timeline of actions with time and user, all read-only.

### Alternative Flows
**4a. No matching events**
1. No event matches the filters.
2. The system shows "No events found" and the flow returns to step 3.


### Exception Flows
**3a. Invalid date range**
1. The start date is after the end date.
2. The system shows an error and the flow returns to step 3.


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 6: 100% of state-changing actions are in the read-only audit log, time precise to 1 second, with the user.

## 6. Traceability (for the RTM later – not for VP)
FR 87-92. NFR 6.
