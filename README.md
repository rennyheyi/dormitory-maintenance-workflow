# Dormitory Maintenance Worklist System

A CPEE-based workflow system for processing dormitory maintenance requests through the CPEE Worklist.

## Workflow

Each request follows these steps:

1. A student submits a maintenance request.
2. An administrator approves or rejects it.
3. If approved, a technician performs an on-site assessment.
4. If materials are required, an inventory manager issues them.
5. The technician completes the repair.
6. The student confirms the result and provides a rating.
7. If the student is not satisfied, the repair cycle repeats.

Rejected requests terminate immediately.

## Roles

- `Student`: submits requests and confirms repairs
- `Admin`: approves or rejects requests
- `Technician`: assesses and repairs reported problems
- `InventoryManager`: issues required materials

The organisation model assigns users to dormitory units so that tasks can be routed to the appropriate workers.

## Process Models

### Dormitory Maintenance Worklist System

This is the child process that handles one maintenance request from administration review through student confirmation.

### Main-Sync

The synchronized-intake main process:

- exposes a Student task with `By Single Worker` handling;
- allows one student to claim the current request form;
- starts the child process with `fork_running`;
- immediately creates the next request task while the child process continues independently.

### Main-Async

The asynchronous main process:

- keeps the student task always available;
- stores submitted requests in a FIFO queue;
- removes requests with `data.queue.shift`;
- starts one child process per request with `fork_running`;
- allows multiple maintenance requests to run concurrently;
- waits briefly when the queue is empty.

## Sync and Async Comparison

| Model | Request handling | Subprocess mode | Concurrent form access |
|---|---|---|---|
| Main-Sync | Single-worker intake task | `fork_running` | No |
| Main-Async | Always-available task with FIFO queue | `fork_running` | Yes |

Both models allow multiple maintenance subprocesses to run at the same time. The difference is at the request-entry task: Main-Sync synchronizes access to each current form, while Main-Async accepts concurrent submissions through its queue.

## Request Data

A student request contains:

- location
- room
- category
- description
- urgency

Additional process data includes the administrator decision, assessment notes, required materials, repair notes, satisfaction, rating, and feedback.

## Project Structure

- `models/`: exported CPEE process models
- `forms/`: Worklist HTML forms and shared CSS
- `org/`: CPEE organisation model
- `Dormitory_Maintenance_Requirements.md`: project requirements
- `DOCUMENTATION.md`: complete technical documentation and verified test evidence
- `docs/images/`: process and Worklist screenshots used by the documentation

