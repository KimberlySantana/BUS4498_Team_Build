# Send problem and necessary information to IT staff for review Task Specification

## Basic Information

- **Task ID:** T8
- **Task name:** Send problem and necessary information to IT staff for review
- **Task type:** Act
- **Task owner:** IT Support System


## 1. Task Description

The system sends the IT handoff package to the on duty IT Support Team through the shared IT service desk ticket queue. It creates or updates the ticket using the request ID so the same issue is not sent as multiple separate tickets.

## 2. Inputs

### Input 1

- **Input name:** IT handoff package
- **Contents and format:** Structured record containing the request ID, problem summary, classification, urgency, employee contact method, technical details, steps attempted, and escalation reason. 
- **Source:** T7 Gather necessary information
- **If a required input is missing or invalid:** Record the submission problem and provide the available handoff package to the IT Support Team for manual ticket creation.

## 3. Outputs

### Output 1

- **Output name:** IT service desk
- **Contents and format:** Ticket record containing the request ID, handoff package details, ticket status, ticket destination, and submission time.
- **Next task or recipient:** T9 IT staff review and respond
- **Complete when:** The IT service desk queue confirms that the ticket was created or updated

## 4. Planned Tools

### Tool 1

- **Tool name:** send_it_handoff
- **Input:** IT handoff package
- **Output:** IT service desk ticket
- **Implementation Route:** Web API calls and database queries
- **Integration approach:** Direct integration
- **Role in this task:** Creates or updates an IT service desk ticket and routes it to the on duty IT support Team
- **Task timeout:** 60 seconds
- **Maximum retries:** 2
- **Retry only when:** The ticket service repoerts a temporary error and confirms no ticket as created. The request ID is checked before retrying to avaid duplicate tickets.
- **On timeout, exhausted retries, or an error that cannot be retried:** Record the failed ticket submission and notify the IT Support Team for manual ticket creation.
