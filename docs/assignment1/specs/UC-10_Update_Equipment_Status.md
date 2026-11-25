# UC-10 – Update Equipment Status (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Optional** – Simple status change; the important rule is E3/E4 (reserved items).
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-10 |
| **Use Case Name** | Update Equipment Status |
| **Primary Actor** | Logistics & Resources Officer |
| **Supporting Actors** | None |
| **Description** | The Logistics & Resources Officer marks an equipment item as Faulty/In Maintenance or returns it to Available. |
| **Trigger** | The officer selects an equipment item and clicks "Change Status". |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The officer is logged in.
2. The equipment item exists in the pool.

**Post-conditions – Success Guarantee**
1. The item has the new status.
2. The change is logged and pushed to all relevant workstations.

**Post-conditions – Failure / Minimal Guarantee**
1. The item's status is unchanged; the officer sees the reason.

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The officer opens the equipment pool.
2. The system lists all items with type, location and status.
3. The officer selects an item and chooses a new status (Faulty/In Maintenance or Available).
4. The system checks that the change is allowed.
5. The system saves the new status.
6. The system writes the change to the audit log and pushes it to all relevant workstations.

### Exception Flows
**4a. Item is reserved to a task**
1. The item is Reserved to Task and the new status is Available.
2. The system rejects the change and shows which task holds the item. The flow returns to step 3. **[DECIDE]**


## 6. Traceability (for the RTM later – not for VP)
FR 5, 6, 16, 19. NFR 7.

## 7. Open decisions for the group (not for VP)
- E3/E4 proposal: a reserved item can still be set to Faulty (handled like E1: released from the task, Dispatcher alerted); any other change is blocked.
