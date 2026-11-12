# UC-12 – Update Staff Availability (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Optional** – Simple status change; the rule to decide is E3.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-12 |
| **Use Case Name** | Update Staff Availability |
| **Primary Actor** | Logistics & Resources Officer |
| **Supporting Actors** | None |
| **Description** | The Logistics & Resources Officer marks a staff member as Unavailable (vacation, illness) or returns them to Available. |
| **Trigger** | The officer selects a staff member and clicks "Update Availability". |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The officer is logged in.
2. The staff member exists in the pool.

**Post-conditions – Success Guarantee**
1. The staff member has the new status (and the reason, for Unavailable).
2. The change is logged and pushed.

**Post-conditions – Failure / Minimal Guarantee**
1. The staff member's status is unchanged.

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The officer opens the staff list.
2. The system lists staff members with role, location and status.
3. The officer selects a staff member, chooses Unavailable and selects the reason (vacation or illness).
4. The system checks that the staff member is not Reserved to Task.
5. The system saves the new status.
6. The system writes the change to the audit log and pushes it to all relevant workstations.

### Alternative Flows
**3a. Return to Available**
1. The officer chooses Available for an Unavailable staff member.
2. The flow continues at step 4.


### Exception Flows
**4a. Staff member is reserved**
1. The staff member is Reserved to Task.
2. The system rejects the change and shows which task holds them. The use case ends. **[DECIDE]**


## 6. Traceability (for the RTM later – not for VP)
FR 8, 14. NFR 7.

## 7. Open decisions for the group (not for VP)
- E3: block (proposal) or allow and alert the Dispatcher?
- Enter an end date for the absence? Story doesn't say.
