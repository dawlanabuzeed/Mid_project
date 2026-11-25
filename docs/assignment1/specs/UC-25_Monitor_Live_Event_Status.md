# UC-25 – Monitor Live Event Status (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Recommended** – Main Dispatcher screen; server-push rule and display content are in the story.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-25 |
| **Use Case Name** | Monitor Live Event Status |
| **Primary Actor** | Dispatcher |
| **Supporting Actors** | None |
| **Description** | The Dispatcher watches the live status screens of all open events; every task or resource change is pushed by the server and shown immediately. |
| **Trigger** | The Dispatcher opens "Live Events". |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The Dispatcher is logged in.

**Post-conditions – Success Guarantee**
1. The Dispatcher sees the current status of the chosen event; no data is changed by this use case.

**Post-conditions – Failure / Minimal Guarantee**
1. The screen shows that it is not up to date (disconnected).

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The Dispatcher opens "Live Events".
2. The system lists all open events.
3. The Dispatcher selects an event.
4. The system shows the event's name, type, location and time since opening, the number of completed tasks out of all tasks, and the task tree with dependencies.
5. For each task the system shows its status (distinct color and text), Field Commander, reserved resources, and elapsed time against estimated duration; overdue tasks are highlighted.
6. When any task or resource status changes, the server pushes the change and the system updates the screen immediately, without polling.
7. The Dispatcher can start actions from the screen (reserve, auto-allocate, cancel task, close or cancel event).

### Alternative Flows
**2a. No open events**
1. There are no open events.
2. The system shows "No open events". The use case ends.


### Exception Flows
**6a. Connection lost**
1. The client loses the connection to the server.
2. The system shows "Disconnected", reconnects and reloads the current state. **[DECIDE]**


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 5: all workstations show the same status within 2 seconds.
- NFR 7: push within 2 seconds, no polling.
- NFR 14: color + text label per status.

## 6. Traceability (for the RTM later – not for VP)
FR 68-76. NFR 5, 7, 14.

## 7. Open decisions for the group (not for VP)
- Link Field Commander to this use case too (FR 77)? Optional diagram change.
