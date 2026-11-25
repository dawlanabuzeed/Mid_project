# UC-32 – Report Shift Start and Readiness (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Optional** – Simple status report; edge case E3/E12 to decide.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-32 |
| **Use Case Name** | Report Shift Start and Readiness |
| **Primary Actor** | Field Crew Member |
| **Supporting Actors** | None |
| **Description** | A Field Crew Member reports the start of their shift and their readiness (Available or Unavailable). |
| **Trigger** | The crew member clicks "Start Shift". |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The crew member is logged in.

**Post-conditions – Success Guarantee**
1. The shift start time is recorded and the crew member's status is Available or Unavailable as reported.
2. The change is logged and pushed.

**Post-conditions – Failure / Minimal Guarantee**
1. The status is unchanged.

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The crew member clicks "Start Shift".
2. The system records the shift start time.
3. The system asks for readiness (Available / Unavailable).
4. The crew member selects a readiness status.
5. The system updates the crew member's status.
6. The system writes the change to the audit log and pushes it to the Dispatchers and the Logistics & Resources Officer.

### Alternative Flows
**1a. Change readiness during the shift**
1. The crew member clicks "Change Readiness".
2. The flow continues at step 3.


### Exception Flows
**5a. Crew member is reserved to a task**
1. The crew member is Reserved to Task and selects Unavailable.
2. The system rejects the change and tells them to contact the Field Commander. The use case ends. **[DECIDE]**


## 6. Traceability (for the RTM later – not for VP)
FR 9, 10.

## 7. Open decisions for the group (not for VP)
- E3 (reserved crew member reports Unavailable).
- E12: does the Field Commander also report readiness?
