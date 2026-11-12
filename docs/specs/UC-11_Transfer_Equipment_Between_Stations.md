# UC-11 – Transfer Equipment Between Stations (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Optional** – Simple location change.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-11 |
| **Use Case Name** | Transfer Equipment Between Stations |
| **Primary Actor** | Logistics & Resources Officer |
| **Supporting Actors** | None |
| **Description** | The Logistics & Resources Officer moves an equipment item from its current station to another station. |
| **Trigger** | The officer selects an equipment item and clicks "Transfer". |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The officer is logged in.
2. The item exists and is not Reserved to Task.

**Post-conditions – Success Guarantee**
1. The item's current location is the destination station.
2. The change is logged and pushed.

**Post-conditions – Failure / Minimal Guarantee**
1. The item's location is unchanged.

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The officer selects an equipment item and clicks "Transfer".
2. The system shows the item's current station and the list of stations.
3. The officer selects the destination station and confirms.
4. The system checks that the item is not reserved and that the destination differs from the current station.
5. The system updates the item's location.
6. The system writes the change to the audit log and pushes it to all relevant workstations.

### Exception Flows
**4a. Item is reserved**
1. The item is Reserved to Task.
2. The system rejects the transfer and shows the task that holds it. The use case ends.

**4b. Same station**
1. The destination is the current station.
2. The system shows an error and the flow returns to step 3.


## 6. Traceability (for the RTM later – not for VP)
FR 7. NFR 7.

## 7. Open decisions for the group (not for VP)
- E4: block transfers of reserved items (proposal).
