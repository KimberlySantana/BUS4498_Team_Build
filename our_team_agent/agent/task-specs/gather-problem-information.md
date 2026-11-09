# Gather problem information Task Specification

## Basic Information

- **Task ID:** T2
- **Task name:** Gather problem information
- **Task type:** Retrieve
- **Task owner:** IT Support System

## 1. Task Description

The system reviews the logged support request and organizes the available details needed to understand the reported issue. It extracts information already provided, such as the affected device or service, error message, time the issue began, and scope of impact. This taks does not make a final routing decision.

## 2. Inputs

### Input 1

- **Input name:** Logged support request
- **Contents and format:** Structured request record containing the request ID, employee contact method, issue description, and submission time.
- **Source:** T1 Receive technology problem request
- **If a required input is missing or invalid:** [State what happens and identify the exception task or responsible person.]

## 3. Outputs

### Output 1

- **Output name:** Problem information record
- **Contents and format:** Structured record containing the request ID, available device or service detail, error-message details, start time, impact scope, and missing information status,
- **Next task or recipient:** T3 Ask for missing information if required details are missing; otherwise T4 identify problem type.
- **Complete when:** Available information has been organized and the missing information status is recorde

## 4. Planned Tools

### Tool 1

- **Tool name:** gather_problem_information
- **Input:** Logged support request
- **Output:** Problem information record
- **Implementation Route:** Functions and database queries
- **Integration approach:** Direct integration
- **Role in this task:** Extracts and organizes the request information into the problem information record
- **Task timeout:** 30 seconds
- **Maximum retries:** 2
- **Retry only when:** A temporary database or system retrieval error occurs. Retries use the same request ID and update the existing record.
- **On timeout, exhausted retries, or an error that cannot be retried:** Record the retrieval failure and send the request to T7 Gather necessary information for IT review.




