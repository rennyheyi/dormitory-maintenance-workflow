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

The synchronous main process:

- receives one student request;
- starts the child process with `wait_running`;
- waits until that request has been completed;
- then accepts the next request.

### Main-Async

The asynchronous main process:

- keeps the student task always available;
- stores submitted requests in a FIFO queue;
- removes requests with `data.queue.shift`;
- starts one child process per request with `fork_running`;
- allows multiple maintenance requests to run concurrently;
- waits briefly when the queue is empty.

## Sync and Async Comparison

| Model | Request handling | Subprocess mode | Parallel requests |
|---|---|---|---|
| Main-Sync | One request at a time | `wait_running` | No |
| Main-Async | FIFO request queue | `fork_running` | Yes |

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

## Testing

The complete child process was tested through the Worklist. The synchronous model was tested sequentially, and the asynchronous model was tested with multiple requests running concurrently.

## Technologies

- CPEE
- CPEE Worklist
- HTML and CSS
- Ruby expressions in CPEE
- XML organisation and process models
