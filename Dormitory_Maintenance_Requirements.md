# Dormitory Maintenance Worklist System – Requirements

## 1. Purpose

The system coordinates dormitory maintenance requests between students, administrators, technicians, and inventory managers using CPEE and the CPEE Worklist.

## 2. Actors

- Student
- Administrator
- Technician
- Inventory Manager

## 3. Functional Requirements

### FR-01 Request submission

A student shall be able to submit a request containing:

- dormitory location
- room number
- problem category
- description
- urgency

### FR-02 Administrative review

An administrator shall be able to approve or reject a submitted request.

### FR-03 Rejected requests

A rejected request shall terminate without starting technical work.

### FR-04 Technical assessment

For an approved request, a technician shall record assessment notes and indicate whether materials are required.

### FR-05 Material issue

If materials are required, an inventory manager shall confirm that they have been issued.

### FR-06 Repair

A technician shall perform the repair and record completion notes.

### FR-07 Student confirmation

The student shall confirm whether the issue has been resolved and may provide a rating and feedback.

### FR-08 Repeated repair

If the student is not satisfied, the repair cycle shall be repeated.

### FR-09 Organisational routing

Worklist tasks shall be assigned according to role and, where applicable, dormitory unit.

### FR-10 Synchronous processing

The synchronous main model shall wait for one child process to finish before accepting the next request.

### FR-11 Asynchronous processing

The asynchronous main model shall:

- keep request submission available;
- add submitted requests to a FIFO queue;
- remove one request at a time from the queue;
- start child processes asynchronously;
- support multiple requests running concurrently.

### FR-12 Data transfer

Request data shall be transferred correctly from a main process to each child process. Concurrent requests shall use independent child-process data.

## 4. User Interface Requirements

- Worklist forms shall use named HTML controls.
- Controls shall belong to `worklist-form`.
- Required fields shall use HTML validation.
- Forms shall not implement a separate callback or manual HTTP submission.
- Optional fields shall initially be empty.

## 5. Process Data Requirements

The system shall maintain the following data where applicable:

- location
- room
- category
- description
- urgency
- admin_decision
- admin_note
- materials_needed
- materials_list
- materials_issued
- assessment_notes
- repair_notes
- satisfied
- rating
- feedback

The asynchronous main process shall additionally maintain:

- `queue`
- `item`

## 6. Acceptance Criteria

The project is accepted when:

1. Worklist forms submit successfully.
2. Data is transferred between all relevant tasks.
3. Approved and rejected paths work correctly.
4. The materials branch works correctly.
5. An unsatisfied student causes another repair cycle.
6. Main-Sync processes requests sequentially.
7. Main-Async accepts and processes concurrent requests.
8. All exported process models are valid XML.
