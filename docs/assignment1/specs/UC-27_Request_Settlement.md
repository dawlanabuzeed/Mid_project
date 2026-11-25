# UC-27 – Request Settlement (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Must** – Included use case with the external system; links Close Event to the billing actor.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-27 |
| **Use Case Name** | Request Settlement |
| **Primary Actor** | None (included by UC-26) |
| **Supporting Actors** | National Billing & Settlement System |
| **Description** | The system sends a billing and budget reimbursement request with the event's cost summary to the National Billing & Settlement System and returns its answer. |
| **Trigger** | UC-26 step 5 asks for settlement. |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. UC-26 has calculated the event's operating costs.

**Post-conditions – Success Guarantee**
1. UC-26 receives the billing system's answer: approval with a Transaction Token, or rejection.

**Post-conditions – Failure / Minimal Guarantee**
1. UC-26 receives 'no answer' (timeout).

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The system builds a billing and budget reimbursement request with the event ID and the cost summary.
2. The system sends the request to the National Billing & Settlement System (through the settlement adapter).
3. The National Billing & Settlement System returns an approval with a Transaction Token, or a rejection.
4. The system returns the answer to UC-26.

### Exception Flows
**3a. No answer in time (E2)**
1. No answer arrives within 10 seconds. **[DECIDE]**
2. The system returns 'timeout' to UC-26, which handles it like a rejection.


## 6. Traceability (for the RTM later – not for VP)
FR 81, 99, 100.
