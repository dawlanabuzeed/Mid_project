# UC-07 – Generate Monthly Reports (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Must** – Automatic month-end generation is the main SLA report path (Part B); primary actor is the timer.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-07 |
| **Use Case Name** | Generate Monthly Reports |
| **Primary Actor** | Month-End Scheduler <<System>> |
| **Supporting Actors** | None |
| **Description** | At the end of each month, triggered by the system clock, the system automatically generates the SLA and Resource Utilization reports for the month that ended. |
| **Trigger** | The system clock reaches the end of the month and the Month-End Scheduler signals the system. |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The server is running.

**Post-conditions – Success Guarantee**
1. The SLA and Resource Utilization reports of the month that ended are stored and available to the System Administrator.

**Post-conditions – Failure / Minimal Guarantee**
1. No report is stored for the month; the failure is written to the log.

## 3. Relationships
- **Includes:** UC-08 Generate SLA Report; UC-09 Generate Resource Utilization Report.
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The Month-End Scheduler signals that the month has ended.
2. The system determines the month that ended.
3. The system generates the SLA report for that month (includes UC-08 Generate SLA Report).
4. The system generates the Resource Utilization report for that month (includes UC-09 Generate Resource Utilization Report).
5. The system stores both reports and makes them available in "Reports" (UC-05).
6. The system writes the generation to the audit log.

### Exception Flows
**1a. Server was down at month end**
1. The server was not running at the end of the month.
2. When the server starts, the system detects the missing month and generates its reports from step 2. **[DECIDE]**

**3a. Generation error**
1. A database error occurs.
2. The system stores no report, logs the error and retries once later. **[DECIDE]**


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 11: SLA report within 10 seconds.
- NFR 16: new report type = one new class.

## 6. Traceability (for the RTM later – not for VP)
FR 93 (+ FR 94-97). NFR 11, 16.

## 7. Open decisions for the group (not for VP)
- Exact trigger time (e.g. 00:00 on the 1st of the next month).
- E7: catch up a missed month automatically (proposal) or only on demand?
