# UC-02 – Manage User Accounts (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Optional** – Plain create/edit; few business rules in the story.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-02 |
| **Use Case Name** | Manage User Accounts |
| **Primary Actor** | System Administrator |
| **Supporting Actors** | None |
| **Description** | The System Administrator creates, edits or deactivates user accounts and sets each account's role (permissions). |
| **Trigger** | The System Administrator opens "User Management" and clicks "Add User". |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The System Administrator is logged in.

**Post-conditions – Success Guarantee**
1. The new or changed account is stored, with the password stored as a salted hash.
2. The change is written to the audit log.

**Post-conditions – Failure / Minimal Guarantee**
1. No account is created or changed.

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The System Administrator opens "User Management".
2. The system displays the list of user accounts with their roles.
3. The System Administrator clicks "Add User".
4. The system displays the account form.
5. The System Administrator enters username, initial password and role (one of the five roles), and for Field Commander / Field Crew Member links the matching staff record.
6. The system validates the data and stores the account with a hashed password.
7. The system writes the action to the audit log and shows the updated list.

### Alternative Flows
**3a. Edit existing account**
1. The System Administrator selects an account and clicks "Edit".
2. The system shows the account form with current data; the flow continues at step 5.

**3b. Deactivate account**
1. The System Administrator selects an account and clicks "Deactivate".
2. The system asks for confirmation; after confirmation it marks the account inactive and continues at step 7.


### Exception Flows
**6a. Username already exists**
1. The system finds the username in use.
2. The system shows "Username already exists" and the flow returns to step 5.

**6b. Missing or invalid data**
1. A required field is empty or the role is not in the list.
2. The system highlights the field and the flow returns to step 5.


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 1 (role-based access), NFR 2 (bcrypt).

## 6. Traceability (for the RTM later – not for VP)
FR 2, 3. NFR 1, 2.

## 7. Open decisions for the group (not for VP)
- Can the admin deactivate their own account? Proposal: no.
- Link Field Commander / Crew accounts to a staff record (needed so a commander user is also a resource).
