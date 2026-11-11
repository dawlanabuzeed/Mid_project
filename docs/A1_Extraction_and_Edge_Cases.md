# CrisisSync – Step A1: Entity Extraction & Edge Case Discovery (DRAFT for group audit)

> **Status:** draft prepared with Claude, to be audited and decided **together** by the group before drawing the diagram.
> Items marked <mark>in yellow</mark> come from course material; the source is written under each one. Everything else comes from the CrisisSync story or the Assignment 1 brief.
>
> <mark>This step (extract actors, external systems, use cases by actor and edge cases, with no diagram yet, then audit and decide) is the course's first step before the use case diagram.</mark><br>
> <mark>*According to course: Software Engineering Methods (שיטות) — Tutorial 1, Part 1 Steps 2–3*</mark>

---

## 1. Human primary actors

| Actor | What the story says they do |
|---|---|
| **System Administrator** | Manages user accounts and permissions, runs the data loader, views the event history (audit log), views the monthly reports and generates a report for a selected past month. |
| **Logistics & Resources Officer** | Manages the equipment and staff pool: updates equipment maintenance status (faulty/in treatment ↔ available), transfers equipment between stations, updates exceptional staff availability (vacation, sickness). |
| **Dispatcher** | Opens and cancels events, builds task trees, reserves resources (manually or by auto-allocation), monitors events in real time, cancels tasks, assigns replacement equipment, closes events against the external billing system. |
| **Field Commander** | Views the events and tasks they command and reports their completion; can report equipment failure. **Dual role:** also a staff resource that can be reserved to at most one task at a time. |
| **Field Crew Member** | Reports shift start and readiness (available/unavailable), views the task they are assigned to, reports equipment failure from the field. |

## 2. Secondary actors / external systems

| Actor | Role |
|---|---|
| **National Billing & Settlement System** `<<System>>` | Receives the charge/budget-refund request when an event is closed; returns approval + Transaction Token, or a rejection. (Simulated by an Adapter / Mock in the implementation.) |
| **Month-End Scheduler** `<<System>>` | Timer based on the system clock; triggers the automatic generation of the monthly reports. |

<mark>Give non-human actors the `<<System>>` stereotype and place them outside the boundary on the right.</mark><br>
<mark>*According to course: Software Engineering Methods (שיטות) — guide “עבודה עם USE CASE ו VP”*</mark>

**Audit – NOT actors** (they are internal parts of our system, so they stay out of the diagram):
- The SQL database / ORM layer – internal persistence.
- The OCSF server and the client stations – internal architecture.
- The data file (SQL/JSON/CSV) used by the data loader – input data, not an actor.
- The Billing **Adapter/Mock** – the implementation of the link to the external system; the *actor* is the external system itself.

<mark>Do not draw hardware or systems as human actors, and do not put external systems inside the system boundary.</mark><br>
<mark>*According to course: Software Engineering Methods (שיטות) — Tutorial 1, “Common LLM failure modes”*</mark>

---

## 3. Core use cases grouped by actor

**All human actors**
- Log In

**System Administrator**
- Manage Users and Permissions
- Run Data Loader
- View Audit Log *(filter by date range, event type and state; open an event to see tasks, resources, settlement approvals and the full timeline – read-only)*
- View Monthly Reports
- Generate Report for Selected Month *(on demand, for a past month)*

**Logistics & Resources Officer**
- Update Equipment Status *(faulty/in treatment ↔ available)*
- Transfer Equipment Between Stations
- Update Staff Availability *(vacation, sickness)*

**Dispatcher**
- Open Event *(name, type from a closed list, location; opening time recorded automatically)*
- Cancel Event *(all non-final tasks → Cancelled, resources released, cancel time recorded)*
- Create Task *(name, estimated duration, prerequisite tasks, required resource package with exactly one Field Commander)*
- Define Task Dependencies *(add/change prerequisites – only while the task is Blocked)*
- Reserve Resource Package Manually
- Auto-Allocate Resource Package *(by a chosen criterion, e.g. nearest available)*
- Cancel Task *(cascades to dependent tasks after the Dispatcher confirms how many will be cancelled)*
- Assign Replacement Equipment *(after an equipment failure in an active task)*
- Monitor Live Event Status *(all open events)*
- Close Event *(only when all tasks are final; includes the settlement request)*

**Field Commander**
- View Assigned Events and Tasks
- Report Task Completion *(only for tasks they command)*
- Report Equipment Failure

**Field Crew Member**
- Report Shift Start and Readiness
- View Assigned Task
- Report Equipment Failure

**Month-End Scheduler**
- Generate Monthly Reports *(Resource Utilization report + SLA report)*

**National Billing & Settlement System**
- participates in: Request Settlement *(included in Close Event)*

### Error / alternative cases to model (as `<<extend>>` or as exception flows in the spec)
- Reservation fails – a resource in the package is already taken (whole reservation rolled back, task stays Ready).
- Dependency change rejected – it would create a cycle.
- Settlement rejected by the billing system – event stays open, Dispatcher alerted.
- Auto-allocation falls back to the next-best alternative (up to 2 attempts); fails only if no alternative exists.

<mark>*include* = the base always and necessarily performs it as part of its normal flow; *extend* = only in edge cases, conditional branches or errors.</mark><br>
<mark>*According to course: Software Engineering Methods (שיטות) — guide “עבודה עם USE CASE ו VP”*</mark>

### Audit – internal system steps, NOT use cases
These go as **steps inside the spec** of the use case they belong to, not as separate ovals:

| Internal step | Goes inside the spec of |
|---|---|
| Check for a dependency cycle (DAG check) | Define Task Dependencies *(a brand-new task can't close a cycle, since nothing depends on it yet – so Create Task only needs the "prerequisites from the same event" check)* |
| Move a task Blocked → Ready automatically | Report Task Completion, Define Task Dependencies |
| Lock resource records, check availability, update status (SQL transaction, all-or-nothing) | Reserve Manually, Auto-Allocate, Assign Replacement Equipment |
| Release a task's resources | Report Task Completion, Cancel Task, Cancel Event |
| Calculate operating costs | Close Event |
| Write to the audit log | every state-changing use case |
| Push status changes to the stations (Server Push) | every state-changing use case |

<mark>Don't invent internal database/programmatic functions as use cases (the course's examples: “Save to DB”, “Calculate Penalty”).</mark><br>
<mark>*According to course: Software Engineering Methods (שיטות) — Tutorial 1, “Common LLM failure modes”*</mark>

---

## 4. Ambiguities, missing rules and edge cases the story does NOT resolve

<mark>Find at least 3; then the group picks at least 2 and decides how the system handles them.</mark><br>
<mark>*According to course: Software Engineering Methods (שיטות) — Tutorial 1, Prompt 1 item 4 and Step 3*</mark>

| # | Edge case / ambiguity | What the story leaves open |
|---|---|---|
| E1 | **Equipment fails mid-task and no replacement is available** | The Dispatcher "is required" to assign a replacement, but what happens to the Active task if none exists? |
| E2 | **The billing system doesn't answer** (timeout / unavailable), or rejects again on retry | The story covers rejection only. Can the Dispatcher retry? Is there a timeout? |
| E3 | **A reserved crew member reports "not available", or Logistics marks them sick/on vacation**, while they are reserved to an Active task | Is the change allowed? Does it affect the task? |
| E4 | **Transfer of equipment that is currently reserved** | Can Logistics transfer or change the status of a reserved item? |
| E5 | **Race: the Dispatcher cancels a task at the same moment its Field Commander reports completion** | Which action wins? |
| E6 | **Consumable materials in the cost calculation** | Costs include consumables, but no actor/use case records consumables. Who enters them, and when? |
| E7 | **The month-end report run is missed** (server down at month end) | Is it generated later automatically, or only on demand by the Admin? |
| E8 | **Failed logins** | No rule for repeated wrong passwords (lockout?). |
| E9 | **The data loader is run on a database that already has data** | Duplicates? Overwrite? Reject? |
| E10 | **Which Field Commander, and when?** | The package "always includes exactly one Field Commander". Does the Dispatcher name a specific commander when creating the task, or only a "Field Commander" slot that is filled at reservation? This decides when the commander can first see the task. |
| E11 | **SLA report – cancelled and still-open events** | SLA = average time from opening to the closing of the event's last task. Do cancelled events count? Cancelled tasks? An event still open at month end? Which month does an event spanning two months belong to? *(Part B needs FRs for this report.)* |
| E12 | **Field Commander readiness** | Only the Field Crew Member reports shift start/readiness. The commander is also a staff resource – does he report readiness too, and can Logistics mark him sick while he's reserved? |
| E13 | **Cross-event dependencies** | The DAG is "the event's tasks". Can a task depend on a task in a different event? (Proposal: no – prerequisites only from the same event.) |

---

## 5. Business Edge Cases – proposed decisions (group to approve or change)

<mark>Format: Scenario & Ambiguity / Architectural Resolution / Impact on System State, under a `## Business Edge Cases` heading in REQUIREMENTS.md.</mark><br>
<mark>*According to course: Software Engineering Methods (שיטות) — REQUIREMENTS_TEMPLATE.md, section 2*</mark>

### Edge Case 1 (E1): Equipment failure with no replacement available
* **Scenario & Ambiguity:** During an Active task, a crew member reports that the bulldozer has broken down. The item becomes Faulty immediately, but the pool has no available bulldozer.
* **Architectural Resolution (proposal):** The task stays **Active** with its other resources still reserved. The system flags the task on the live status screen as *"Missing resource – replacement pending"* and shows the Dispatcher an alert. The Dispatcher can retry Assign Replacement Equipment at any time (the system offers it again when an item of that type returns to Available), or cancel the task.
* **Impact on System State:** Faulty item → status Faulty and released from the task; task state unchanged (Active) plus a "missing resource" flag; an audit-log entry for the failure report and for each replacement attempt.

### Edge Case 2 (E2): Billing system timeout / repeated rejection
* **Scenario & Ambiguity:** The Dispatcher closes an event; the billing system doesn't respond, or rejects the settlement.
* **Architectural Resolution (proposal):** If no answer arrives within **10 seconds**, the system treats it as a failed settlement (same as a rejection). The event stays open (all tasks final), with the state *"Settlement pending"*. The Dispatcher gets an alert with the reason and can **retry Close Event** later; there is no limit on retries, and each attempt is logged.
* **Impact on System State:** Event not closed and no closing time recorded; costs calculated but not confirmed; audit-log entry per attempt with the result (timeout/rejected/approved + Transaction Token).

### Edge Case 3 (E3/E4): Availability or transfer changes on a reserved resource
* **Scenario & Ambiguity:** Logistics marks a crew member as sick, or tries to transfer a bulldozer, while it is reserved to an Active task.
* **Architectural Resolution (proposal):** The system **blocks** transfers and availability changes for reserved resources and tells the Logistics Officer which task holds the resource. Exception: "Faulty" may always be set (same handling as E1). A sick crew member is handled by the Dispatcher replacing them first.
* **Impact on System State:** No change to the resource or the task; the rejected attempt is logged.

### Edge Case 4 (E5): Cancel vs. complete at the same moment
* **Scenario & Ambiguity:** The Dispatcher confirms cancelling a task while its Field Commander reports completion.
* **Architectural Resolution (proposal):** Both actions are SQL transactions that lock the task record (same mechanism as reservations); **the first to commit wins**. The second gets a clear message ("Task was already completed/cancelled") and the screen refreshes to the current state.
* **Impact on System State:** The task ends in exactly one final state; the losing action changes nothing; both attempts are logged.

> **Choosing which edge cases to write up:** Part B covers only (1) reserving a resource package incl. reporting task completion and (2) the SLA report. Edge cases that touch these (E1, E5, E10, E11) give you ready-made FRs and exception BDD tests, so they're the best picks for `## Business Edge Cases`.
>
> **Open for the group:** E6 (consumables), E7 (missed month-end run), E8 (login lockout), E9 (loader on existing data), E10–E13. Decide whether to resolve them now or ask in the course forum, especially **E6**, since it affects the Close Event spec.

---

## 6. Group audit checklist (before moving to the diagram)
- [ ] All 5 human actors and both `<<System>>` actors agreed; nothing internal listed as an actor.
- [ ] Every use case is a real actor ↔ system interaction; internal steps moved to the specs.
- [ ] The 4 error cases have a place (extend use case or exception flow).
- [ ] At least 2 edge cases decided and written in English.
- [ ] Everyone can explain every item.
