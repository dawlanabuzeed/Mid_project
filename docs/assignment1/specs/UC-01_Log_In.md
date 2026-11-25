# UC-01 – Log In (spec draft for VP)

<mark>Fields and layout follow the "Start Ride" example (Overview, Conditions, Relationships, Basic Flow, alternative/exception flows branching from a numbered step, Special Requirements).</mark><br>
<mark>*According to course: שיטות – `עבודה עם USE CASE ו VP.docx` (Tutorial 1)*</mark>

> **Priority: Recommended** – Simple, but the group decided the invalid-login case lives only in the spec, so the spec is where it is shown.
> Draft for the group to approve. Assumptions to confirm are marked **[DECIDE]**. See `UseCase_Specs_Guide.pdf` for notes.

---

## 1. Overview & Boundaries  → VP: *Info* / *Description* tab
| Field | Value |
|---|---|
| **Use Case ID** | UC-01 |
| **Use Case Name** | Log In |
| **Primary Actor** | Registered User (any of the 5 roles) |
| **Supporting Actors** | None |
| **Description** | A registered user enters a username and password, and the system authenticates them and opens the interface of their role. |
| **Trigger** | The user enters a username and password on the login screen and clicks "Log In". |

## 2. Conditions  → VP: *Details* tab
**Pre-conditions**
1. The user has an account in the system (created by the System Administrator or by the data loader).
2. The client workstation is connected to the server over the LAN.

**Post-conditions – Success Guarantee**
1. The user is authenticated for this session.
2. The system shows the interface of the user's role (one of the five role interfaces).

**Post-conditions – Failure / Minimal Guarantee**
1. The user is not logged in and has no access to any system function.
2. An error message is shown on the login screen.

## 3. Relationships
- **Includes:** none
- **Extended by:** none

## 4. Flow of Events  → VP: *Flow of Events* tab
### Basic Flow (Main Success Scenario)
1. The user opens the CrisisSync client.
2. The system displays the login screen.
3. The user enters a username and password and clicks "Log In".
4. The system checks the username and compares the password with the stored salted hash.
5. The system identifies the user's role.
6. The system opens the main screen of that role.

### Alternative Flows
**3a. Empty field**
1. The user clicks "Log In" with an empty username or password.
2. The system highlights the empty field and the flow returns to step 3.


### Exception Flows
**2a. Server unreachable**
1. The client cannot connect to the server.
2. The system shows "Cannot connect to server" and offers to retry. The use case ends.

**4a. Invalid username or password**
1. The username does not exist or the password does not match.
2. The system shows "Invalid username or password" without saying which one is wrong.
3. The flow returns to step 3.

**4b. Account deactivated**
1. The account was deactivated by the System Administrator.
2. The system shows "Account is not active". The use case ends.


## 5. Special Requirements (NFRs)  → VP: *Details* / *Requirements*
**[DECIDE – all numbers are the group's proposals from FR_NFR_Requirements.pdf]**
- NFR 1: each operation is allowed only to the roles permitted for it.
- NFR 2: passwords are stored only as salted bcrypt hashes, cost factor at least 10.
- NFR 8: 95% of operations respond within 2 seconds.

## 6. Traceability (for the RTM later – not for VP)
FR 1, 2. NFR 1, 2, 8.

## 7. Open decisions for the group (not for VP)
- E8: lock the account after N failed logins? (story has no rule; leave out unless the group wants it).
- 4b exists only if Manage User Accounts can deactivate accounts.
