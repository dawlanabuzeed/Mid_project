# Requirements Specification – CrisisSync Emergency Management System
**Course:** Software Engineering Methods (61756)  
**Assignment:** 1 – Part B: Requirements, Traceability Matrix and Acceptance Tests  
**Group:** 10  
**Submission Date:** 19.11.2026  

### Team Members
| Full Name | ID | Braude Email |
| :--- | :--- | :--- |
| Gaith Mostafa | 327910717 | Gaith.Mostafa@e.braude.ac.il |
| Selen Daher | 214228884 | Selen.Daher@e.braude.ac.il |
| Mosa Abo Said | 212761282 | Mosa.Abo.Said@e.braude.ac.il |
| Dawlan Abu Zeed | 214049785 | Dawlan.Abu.Zeed@e.braude.ac.il |

---

## 1. Scope

This document defines the requirements and acceptance tests for the two operations required in Part B:

1. **Reserving a resource package for a task, including reporting task completion** – manual reservation (UC18), handling a reservation conflict (UC19), automatic allocation (UC20), handling an allocation failure (UC21) and reporting task completion (UC30).
2. **Generating the SLA (timeliness) report** – generation at month end (UC07), on-demand generation for a selected month (UC06) and the SLA calculation itself (UC08).

Use case IDs and names refer to the use case model in `G10_Assignment1.vpp`.

| Use Case ID | Use Case Name | Primary Actor |
| :--- | :--- | :--- |
| UC06 | Generate Report for Selected Month | System Administrator |
| UC07 | Generate Monthly Reports | Month-End Scheduler «System» |
| UC08 | Generate SLA Report | – (included by UC06, UC07) |
| UC18 | Reserve Resource Package Manually | Dispatcher |
| UC19 | Handle Reservation Conflict | Dispatcher (extends UC18) |
| UC20 | Auto-Allocate Resource Package | Dispatcher |
| UC21 | Handle Allocation Failure | Dispatcher (extends UC20) |
| UC22 | Cancel Task | Dispatcher (used in Edge Case 2) |
| UC30 | Report Task Completion | Field Commander |

---

## 2. Business Edge Cases & Concurrency Decisions

### Edge Case 1: Which events the SLA report counts
* **Scenario & Ambiguity:** The SLA report shows "the average time from opening an event until the closing of its last task, by event type". The story does not say whether cancelled events are counted, whether an event that still has open tasks at month end is counted, or to which month an event belongs if it was opened in one month and finished in the next.
* **Architectural Resolution:** An event is counted in the month in which its **last task reached a final state** (Completed or Cancelled). Its SLA time is measured from the event's opening timestamp to that moment. **Cancelled events are excluded.** Events that still have at least one non-final task at the end of the month are **not counted** in that month; they are counted in the month in which their last task reaches a final state. An event type with no counted events is shown as "No events" instead of an average of 0.
* **Impact on System State:** No data is changed. The report generator reads the event opening timestamps, the task state-transition timestamps from the audit log and the event status. The stored report contains, per event type, the number of counted events and the average time.

### Edge Case 2: Cancel and complete the same task at the same moment
* **Scenario & Ambiguity:** The Dispatcher confirms "Cancel Task" at the same moment the task's Field Commander reports its completion. Both actions end in a final state (Cancelled or Completed), and the story does not say which one wins.
* **Architectural Resolution:** Both actions run as SQL transactions that lock the task record (pessimistic locking, `SELECT ... FOR UPDATE`, the same mechanism as resource reservation). Inside the transaction each action first checks that the task is still non-final. **The first transaction to commit wins.** The second finds the task already final, changes nothing, and its user receives a clear message ("The task was already completed" / "The task was already cancelled") and a refreshed screen.
* **Impact on System State:** The task ends in exactly one final state and is never changed again. Only the winning action releases the resources and updates the dependent tasks (to Ready after a completion, or Cancelled after a cascading cancellation). Both attempts are written to the audit log.

### Edge Case 3: When the specific Field Commander is chosen
* **Scenario & Ambiguity:** Every task's resource package "always includes exactly one Field Commander", and a Field Commander may view and complete only the tasks he commands. The story does not say whether the Dispatcher names a specific commander when creating the task or only a "Field Commander" slot.
* **Architectural Resolution:** When creating a task, the Dispatcher defines the package by roles and equipment types, including one Field Commander slot. The **specific Field Commander is chosen at reservation time** (manually in UC18, or by the allocation criterion in UC20), like every other resource. Like any staff member, a Field Commander who is already reserved to another task cannot be chosen.
* **Impact on System State:** On a successful reservation the chosen Field Commander's status becomes Reserved to Task and he is recorded as the task's commander. From that moment the task appears on his screens and only he can report its completion.

---

## 3. Requirements Traceability Matrix (RTM)

All requirements are atomic (one rule per row). Every requirement is mapped to the use case that implements it and to the acceptance test that verifies it (Section 4).

### 3.1 Functional Requirements (FR)

| Req ID | Requirement Description | Type | Mapped Use Case | Test ID |
| :--- | :--- | :---: | :--- | :---: |
| **REQ-01** | The system shall allow the Dispatcher to manually select one specific Available resource for each item of a Ready task's required resource package. | FR | UC18 | TEST-01 |
| **REQ-02** | The system shall reject a resource reservation request for a task that is not in Ready status. | FR | UC18 | TEST-02 |
| **REQ-03** | The system shall reserve the entire resource package of a task in a single atomic SQL transaction. | FR | UC18 | TEST-01 |
| **REQ-04** | The system shall change a task's status from Ready to Active only after its entire resource package has been reserved. | FR | UC18 | TEST-01 |
| **REQ-05** | The system shall record the Field Commander reserved in a task's package as that task's commander. | FR | UC18 | TEST-01 |
| **REQ-06** | The system shall ensure that each staff member and equipment item, including a Field Commander, is reserved to at most one task at any moment. | FR | UC18 | TEST-04 |
| **REQ-07** | The system shall roll back the entire reservation, and keep the task in Ready status, if any selected resource is not Available at commit time. | FR | UC19 | TEST-03 |
| **REQ-08** | The system shall notify the Dispatcher whose reservation failed which resources were unavailable and refresh his screen with the current resource status. | FR | UC19 | TEST-03 |
| **REQ-09** | The system shall allow the Dispatcher to request automatic allocation of a Ready task's resource package by a selected criterion (e.g. geographic proximity to the event location). | FR | UC20 | TEST-05 |
| **REQ-10** | When a resource chosen by automatic allocation is taken by a concurrent allocation, the system shall automatically try the next best available alternative, up to 2 alternative attempts, without a new request from the Dispatcher. | FR | UC20 | TEST-06 |
| **REQ-11** | If no available alternative exists for an item of the package, the system shall fail the automatic allocation, reserve no resource, keep the task in Ready status and notify the Dispatcher which item could not be filled. | FR | UC21 | TEST-07 |
| **REQ-12** | The system shall allow the Field Commander of an Active task to report its completion. | FR | UC30 | TEST-08 |
| **REQ-13** | When completion is reported, the system shall change the task's status to Completed and record the completion timestamp. | FR | UC30 | TEST-08 |
| **REQ-14** | When a task is completed, the system shall immediately release all its reserved resources to Available, except equipment marked Faulty, which stays Faulty. | FR | UC30 | TEST-08 |
| **REQ-15** | The system shall automatically change a Blocked task to Ready when it has no remaining prerequisite task that is not Completed. | FR | UC30 | TEST-08 |
| **REQ-16** | The system shall reject a completion report from any user who is not the task's assigned Field Commander. | FR | UC30 | TEST-10 |
| **REQ-17** | If a cancellation and a completion of the same task are submitted at the same moment, the system shall apply only the first committed action and inform the other user that the task is already in a final state. | FR | UC30, UC22 | TEST-09 |
| **REQ-18** | The system shall automatically generate the SLA report of the month that has ended at the end of each month, according to the system clock. | FR | UC07, UC08 | TEST-11 |
| **REQ-19** | The system shall allow the System Administrator to generate the SLA report on demand for a selected month that has already ended. | FR | UC06, UC08 | TEST-12 |
| **REQ-20** | The system shall reject an on-demand report request for the current month or a future month. | FR | UC06 | TEST-14 |
| **REQ-21** | The SLA report shall show, for each event type, the number of counted events and the average time from event opening until the event's last task reached a final state. | FR | UC08 | TEST-11 |
| **REQ-22** | The SLA report shall count an event in the month in which its last task reached a final state and shall exclude cancelled events and events that still have a non-final task. | FR | UC08 | TEST-13 |
| **REQ-23** | The SLA report shall show "No events" for an event type with no counted events. | FR | UC08 | TEST-13 |

### 3.2 Non-Functional Requirements (NFR)

| Req ID | Requirement Description | Type | Mapped Use Case | Test ID |
| :--- | :--- | :---: | :--- | :---: |
| **REQ-24** | (Reliability) When two or more reservation or allocation requests target the same resource at the same moment, exactly one shall succeed, in 100% of cases. | NFR | UC19, UC20 | TEST-03, TEST-06 |
| **REQ-25** | (Reliability) A failed reservation or allocation shall leave 0 resources of the package reserved. | NFR | UC19, UC21 | TEST-03, TEST-07 |
| **REQ-26** | (Performance) A manual reservation of a resource package shall complete, successfully or with a failure message, within 2 seconds. | NFR | UC18, UC19 | TEST-01, TEST-03 |
| **REQ-27** | (Performance) An automatic allocation, including up to 2 alternative attempts, shall complete within 5 seconds, without raising other users' response times above 2 seconds. | NFR | UC20 | TEST-06 |
| **REQ-28** | (Performance) The server shall push every task or resource status change to all relevant workstations within 2 seconds, without client-side polling. | NFR | UC18, UC30 | TEST-08 |
| **REQ-29** | (Performance) The system shall generate the SLA report for one month within 10 seconds. | NFR | UC08 | TEST-11 |
| **REQ-30** | (Usability) A Dispatcher shall be able to reserve a resource package manually from the event status screen in no more than 5 clicks. | NFR | UC18 | TEST-01 |
| **REQ-31** | (Security) The system shall allow reservation and allocation only to the Dispatcher role and task completion only to the task's assigned Field Commander; 100% of other attempts shall be rejected. | NFR | UC18, UC30 | TEST-10 |
| **REQ-32** | (Traceability) 100% of task state transitions and resource status changes shall be written to the audit log with a timestamp precise to 1 second and the responsible user. | NFR | UC18, UC30 | TEST-08 |

---

## 4. BDD Acceptance Tests (Given-When-Then)

### Operation 1: Reserving a resource package, including reporting task completion

### Acceptance Test [TEST-01]: Manual reservation of a full package (Happy Path)
* **Given:** Event "Building Collapse – Haifa" is open, task "Debris Clearing" is in Ready status, and its package requires 1 Field Commander, 1 Heavy Engineering Equipment Operator, 1 Bulldozer and 1 Paramedic.
* **And:** At least one Available resource exists for each of the 4 items.
* **When:** The Dispatcher opens the event status screen, selects the task, chooses one Available resource for each item and confirms the reservation.
* **Then:** All 4 selected resources change to "Reserved to Task" for this task in one transaction.
* **And:** The task status changes from Ready to Active and the selected Field Commander is recorded as the task's commander.
* **And:** The confirmation is shown to the Dispatcher within 2 seconds of confirming.
* **And:** The whole action took no more than 5 clicks from the event status screen.

### Acceptance Test [TEST-02]: Reservation rejected for a task that is not Ready (Exception)
* **Given:** Task "Evacuate Wounded" is in Blocked status because its prerequisite task "Debris Clearing" is still Active.
* **When:** The Dispatcher tries to reserve resources for "Evacuate Wounded".
* **Then:** The system rejects the request with the message "This task is not Ready".
* **And:** No resource changes status and the task stays Blocked.

### Acceptance Test [TEST-03]: Two Dispatchers reserve the same bulldozer at the same moment (Concurrency / Recovery)
* **Given:** Bulldozer B-7 is Available.
* **And:** Dispatcher A (event in Haifa) and Dispatcher B (event in Tel Aviv) have each selected a full package that includes B-7 for a Ready task.
* **When:** Both Dispatchers confirm their reservations at the same moment.
* **Then:** Exactly one reservation succeeds and B-7 is reserved to that task only.
* **And:** The other reservation is rolled back completely: 0 resources of its package are reserved and its task stays Ready.
* **And:** The Dispatcher whose reservation failed sees, within 2 seconds, a message that B-7 is unavailable and a refreshed screen with the current resource status.

### Acceptance Test [TEST-04]: A Field Commander already reserved to another task cannot be chosen (Exception)
* **Given:** Field Commander "Cmdr. Levi" is Reserved to Task "Fire Suppression – Building 3", which is Active.
* **When:** The Dispatcher reserves the package of another Ready task.
* **Then:** "Cmdr. Levi" does not appear among the Available Field Commanders that can be selected.
* **And:** If a request including him reaches the server anyway, the reservation is rolled back and 0 resources are reserved.

### Acceptance Test [TEST-05]: Automatic allocation by proximity (Happy Path)
* **Given:** Task "Hazmat Containment" is Ready, and Available resources of every required type exist in the pool.
* **When:** The Dispatcher requests automatic allocation with the criterion "closest to the event location".
* **Then:** For each package item the system reserves the Available resource closest to the event location.
* **And:** The task status changes to Active and the Dispatcher sees the list of allocated resources.

### Acceptance Test [TEST-06]: Two automatic allocations compete for the same resource (Concurrency / Recovery)
* **Given:** Ambulance A-1 is the closest Available ambulance to both event E1 and event E2, and ambulance A-2 is the next closest Available ambulance.
* **When:** Automatic allocations for a task in E1 and a task in E2 run at the same moment.
* **Then:** Exactly one of the tasks receives A-1.
* **And:** The other allocation automatically continues with A-2 as its first alternative attempt, without a new request from the Dispatcher, and completes successfully.
* **And:** Each allocation completes within 5 seconds, and other users' operations during that time respond within 2 seconds.

### Acceptance Test [TEST-07]: Automatic allocation with no available alternative (Exception)
* **Given:** Task "Water Pumping" is Ready and its package requires 1 Generator.
* **And:** No Generator in the pool is Available (all are Reserved to Task or Faulty).
* **When:** The Dispatcher requests automatic allocation.
* **Then:** The allocation fails and 0 resources of the package are reserved.
* **And:** The task stays Ready, and the Dispatcher is notified that the "Generator" item could not be filled.

### Acceptance Test [TEST-08]: Field Commander reports task completion (Happy Path)
* **Given:** Task "Debris Clearing" is Active with 4 reserved resources, and "Cmdr. Cohen" is its assigned Field Commander.
* **And:** Task "Evacuate Wounded" is Blocked and its only prerequisite is "Debris Clearing".
* **When:** Cmdr. Cohen reports the completion of "Debris Clearing" and confirms.
* **Then:** "Debris Clearing" changes to Completed and its completion timestamp is recorded.
* **And:** All 4 resources, including Cmdr. Cohen, change to Available.
* **And:** "Evacuate Wounded" automatically changes from Blocked to Ready.
* **And:** The new statuses appear on every relevant workstation within 2 seconds, without a manual refresh.
* **And:** The audit log contains each state change with its timestamp (to the second) and the responsible user.

### Acceptance Test [TEST-09]: Cancel and complete the same task at the same moment (Concurrency / Recovery)
* **Given:** Task "Search Rubble" is Active.
* **When:** The Dispatcher confirms "Cancel Task" and the task's Field Commander confirms "Report Completion" at the same moment.
* **Then:** The task ends in exactly one final state: the state of the action that committed first.
* **And:** The user whose action came second receives the message "The task was already completed" or "The task was already cancelled", and his screen is refreshed.
* **And:** Only the winning action released resources and updated dependent tasks.

### Acceptance Test [TEST-10]: Completion reported by a user who is not the task's commander (Security)
* **Given:** Task "Debris Clearing" is Active and its assigned Field Commander is "Cmdr. Cohen".
* **When:** Field Commander "Cmdr. Levi", who is logged in but not assigned to this task, tries to report its completion.
* **Then:** The system rejects the request.
* **And:** The task stays Active and no resource changes status.

### Operation 2: Generating the SLA report

### Acceptance Test [TEST-11]: Automatic SLA report at month end (Happy Path)
* **Given:** In October 2026, two Fire events had their last task reach a final state 2 hours and 4 hours after the event was opened, and one Road Accident event after 1 hour.
* **When:** The system clock reaches the end of October 2026 and the Month-End Scheduler starts the monthly report generation.
* **Then:** The SLA report for October 2026 is generated within 10 seconds.
* **And:** It shows Fire: 2 events, average 3:00 hours; Road Accident: 1 event, average 1:00 hour.
* **And:** The report is stored and available to the System Administrator.

### Acceptance Test [TEST-12]: On-demand SLA report for a past month (Happy Path)
* **Given:** The current date is 17.11.2026 and events exist in September 2026.
* **When:** The System Administrator selects "Generate Report for Selected Month" and chooses September 2026.
* **Then:** The SLA report for September 2026 is generated and displayed.

### Acceptance Test [TEST-13]: Counting boundaries in the SLA report (Edge Case)
* **Given:** Event E1 (Fire) was opened on 30.10.2026 at 23:00 and its last task reached a final state on 01.11.2026 at 01:00.
* **And:** Event E2 (Fire) was cancelled in November 2026.
* **And:** Event E3 (Fire) was opened in November 2026 and still has an Active task at the end of November.
* **And:** No Natural Disaster event finished in November 2026.
* **When:** The SLA report for November 2026 is generated.
* **Then:** E1 is counted in November with an SLA time of 2:00 hours, and is not counted in October.
* **And:** E2 and E3 are not counted.
* **And:** The Natural Disaster row shows "No events".

### Acceptance Test [TEST-14]: On-demand report for a month that has not ended (Exception)
* **Given:** The current date is 17.11.2026.
* **When:** The System Administrator requests the report for November 2026 (or December 2026).
* **Then:** The system rejects the request with the message "Select a month that has ended".
* **And:** No report is generated or stored.

---

## 5. AI Usage Disclosure

Claude (Anthropic) was used to draft the use case specifications, check the Visual Paradigm model against the assignment instructions, and draft the requirements and acceptance tests. All content was reviewed by the group, and every member can explain it.
