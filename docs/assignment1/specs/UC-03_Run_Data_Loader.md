# UC-03 – Run Data Loader (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Optional** – Utility module (story note 2), not part of daily operation.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-03 |
| **Use Case Name** | Run Data Loader |
| **Primary Actor** | System Administrator |
| **Supporting Actors** | None |
| **Description** | The System Administrator loads initial data (users, staff and equipment) from a SQL, JSON or CSV file into the database. |
| **Trigger** | The System Administrator clicks "Load Initial Data" and selects a data file. |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The System Administrator is logged in.

**Post-conditions – Success Guarantee**
1. All records from the file are stored in the database.
2. The load is written to the audit log with the number of records loaded.

**Post-conditions – Failure / Minimal Guarantee**
1. No record from the file is stored (the load is all-or-nothing).
2. The System Administrator sees which records or lines caused the failure.

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The System Administrator clicks "Load Initial Data".
2. The system asks for a data file.
3. The System Administrator selects a SQL, JSON or CSV file.
4. The system validates the file format and every record (required fields, closed lists of roles and equipment types).
5. The system shows a summary (number of users, staff members and equipment items) and asks for confirmation.
6. The System Administrator confirms.
7. The system stores all records in one transaction.
8. The system writes the load to the audit log and shows a success message.

### Alternative Flows
**6a. Administrator cancels**
1. The System Administrator clicks "Cancel".
2. Nothing is loaded. The use case ends.


### Exception Flows
**4a. Unsupported or broken file**
1. The file is not valid SQL/JSON/CSV.
2. The system shows an error and nothing is loaded. The use case ends.

**4b. Invalid record**
1. A record has a missing field or a value outside a closed list.
2. The system lists the invalid records and nothing is loaded. The use case ends.

**7a. Duplicate IDs in the database**
1. A record's ID already exists in the database.
2. The system rolls back the transaction and reports the duplicates. The use case ends.


## 6. Traceability (for the RTM later – not for VP)
FR 4, 15, 17.

## 7. Open decisions for the group (not for VP)
- E9: loader on a database that already has data: reject duplicates (proposal), overwrite, or skip?
