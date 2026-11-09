Ask employee if problem was resolved Task Specification

## Basic Information

- **Task ID:** T6
- **Task name:** Ask employee if problem was resolved
- **Task type:** Verify
- **Task owner:** IT Support System


## 1. Task Description

The system sends the employee a standard question asking whether the approved resolving steps fixed the reported problem. It records the employee response. The employee has 24 hours to respond; this gives enough time to try the recommended steps

## 2. Inputs

### Input 1

- **Input name:** Troubleshooting status
- **Contents and format:** Structured record containing the request ID, employee contact method, appproved steps provided, and current troubleshooting status.
- **Source:** T5 Provide approved resolving steps
- **If a required input is missing or invalid:** Send the request to T7 Gather necessary information for IT review

## 3. Outputs

### Output 1

- **Output name:**. Resolution response
- **Contents and format:** Structured record containing the request ID, employee response of resolves, unresolved, or no response, and response time.
- **Next task or recipient:** Resolved requests fo to case closure. Unresolved or no response requests go to T7 Gather necessary information
- **Complete when:** The employee response is recorded or the 24 hour response window ends.

## 4. Planned Tools

### Tool 1

- **Tool name:** request_resolution_confirmation
- **Input:** Troubleshooting status
- **Output:** Resolution response
- **Implementation Route:** Web API calls and database queries
- **Integration approach:** Direct integration
- **Role in this task:** Sends the resolution question, records the response, and updates the request status
- **Task timeout:** 24 hours
- **Maximum retries:** 1
- **Retry only when:** No employee response is received; the system sends on reminder without creating a duplicate request.
- **On timeout, exhausted retries, or an error that cannot be retried:** Record no response or delivery failure and send the request to T7 Gather necessary information.
