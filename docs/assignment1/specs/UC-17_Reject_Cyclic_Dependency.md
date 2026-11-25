# UC-17 – Reject Cyclic Dependency (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Must** – Extend use case in the diagram, so it needs at least a short spec.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-17 |
| **Use Case Name** | Reject Cyclic Dependency |
| **Primary Actor** | Dispatcher (inherited from UC-16) |
| **Supporting Actors** | None |
| **Description** | When a dependency change would create a cycle, the system rejects the whole change and tells the Dispatcher. |
| **Trigger** | In UC-16 step 6, the system finds that the new dependencies would create a cycle. |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. UC-16 is running and has reached its extension point "Cycle Detected".

**Post-conditions – Success Guarantee**
1. The cyclic change is not saved; the task keeps its previous prerequisites and status.
2. The Dispatcher has been told which tasks form the cycle.

**Post-conditions – Failure / Minimal Guarantee**
1. (Same as success: nothing is saved.)

## 3. Relationships
- **Extends:** UC-16 Define Task Dependencies - at extension point "Cycle Detected" (step 6).
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The system discards the requested dependency change; nothing is saved.
2. The system shows the Dispatcher "This change would create a cycle" and the tasks that form it.
3. The system writes the rejected attempt to the audit log. **[DECIDE]**
4. The flow returns to UC-16 step 4 so the Dispatcher can change the selection, or the Dispatcher cancels.

## 6. Traceability (for the RTM later – not for VP)
FR 36, 37.
