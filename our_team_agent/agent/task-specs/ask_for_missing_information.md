Ask for missing information Task Specification

## 1. Task Goal

- **Objective:** Obtain the minimum missing, non confidential information needed to classigy and safely route a technology problem request

## 2. Inbound Inputs

### Input 1

- **Input name:** Problem information record
- **What it contains:** A structured record containing the request ID, employee contact method, issue description, available device or service details, error message details, impact scope, and missing information status.
- **Source:** Gather problem information.

### Input 2

- **Input name:** Employee follow up response
- **What it contains:** A text response from the employee to a previous information request, linked to the request ID
- **Source:** Employee

## 3. Tool Permissions and Boundaries

### Task-Wide Limits

- **Total task timeout:** 24 hours, including employee response waiting time, tool calls, and retries.
- **Maximum tool calls:** 8
  
### Tool 1

- **Tool name:** review_missing_information
- **Tool type:** Language model and database query
- **Supports these permitted subtasks:** Identify missing information; evaluate employee response.
- **Allowed use:** Read the problem information record and identify whether required non confidential fields are missing
- **Prohibited use:** Do NOT access passwords, MFA codes, security answers, personal data unrelated to the request, or any data from outside the supprot record.
- **Approval required:** None within the allowed use.
- **Timeout per call:** 20 seconds
- **Maximum retries per call:** 1
- **Retry conditions and failure response:** Retry only for a temporary system or database error after a 10-second wait. If the retry fails, record the error and hand off to IT staff.

### Tool 2

- **Tool name:** send_information_request
- **Tool type:**  API request.
- **Supports these permitted subtasks:** Request missing information.
- **Allowed use:** Send one focused follow-up question to the employee using the approved support-message channel.
- **Prohibited use:** Do not ask for passwords, MFA codes, security answers, banking information, unnecessary personal information, or permission to change security settings.
- **Approval required:** None within the allowed use.
- **Timeout per call:** 30 seconds
- **Maximum retries per call:** 1
- **Retry conditions and failure response:**  Retry only for a confirmed message-delivery error after a 30-second wait. If delivery remains unsuccessful or the delivery result is uncertain, record the issue and hand off to IT staff.

## 4. How the Agent Should Reason

### Permitted Subtask 1

- **Subtask name:**  Identify missing information
- **Subtask description:** Review the problem information record to determine the most important missing field needed for classification, such as the device or service affected, exact error message, impact scope, or when the problem began.
- **Subtask boundary:** Use only the existing support record. Do not request confidential information or attempt to troubleshoot the issue.
- **Retry limits:** 1 additional attempt only for a temporary tool failure.

### Permitted Subtask 2

- **Subtask name:**  Request missing information
- **Subtask description:**  Create and send one clear, focused question for the most important missing non confidential detail.
- **Subtask boundary:** Ask only one focused question at a time. The agent may send no more than two total information requests for one request ID.
- **Retry limits:** 1 additional attempt only for a confirmed message delivery failure

### Permitted Subtask 3

- **Subtask name:**  Evaluate employee response
- **Subtask description:**  Determine whether the employee response supplies enough information to classify the issue or whether one additional focused question is needed.
- **Subtask boundary:**  If the response still lacks required information after the second request, do not continue asking questions. Prepare the case for IT handoff.
- **Retry limits:** 1 additional attempt only for a temporary tool failure.
- **Decision guidance:** After each subtask, use its findings to select the permitted subtask most likely to resolve the most important remaining uncertainty. Do not follow a fixed sequence. If no permitted subtask can make useful progress, stop and hand the case to a person.

## 5. When to Stop or Hand Off to a Human

- **Stop successfully when:** The request contains enough non-confidential information for T4 Identify problem type.
- **Hand off early when:** Two information requests do not produce usable information, the employee does not respond within 24 hours, there is a security concern, a tool fails, or the request requires information outside the agent’s boundaries.
- **Hand off to:**  T7 Gather necessary handoff information for IT staff review.

Stop at the first applicable budget limit or handoff condition. While awaiting review, take no further autonomous action.

## 6. Outbound Deliverable

*Revise these default items if your task needs a more specific deliverable, or retain them if they fit.*

- **Status:** Completed or escalated to human.
- **Result or recommendation:** The completed result. If escalated before reaching a supported result, write undetermined.
- **Evidence summary:** The most important evidence supporting the result or explaining why no result could be reached.
- **Subtasks performed:** Permitted subtasks completed, including repeated attempts.
- **Unresolved issues:** Remaining uncertainties or questions; use none only if no unresolved issue remains.
- **Handoff note:** Reason for stopping, unresolved questions, and what the reviewer needs to decide; write "Not applicable" for a completed task.
- **Next task or recipient:**  T4 Identify problem type if completed; T7 Gather necessary handoff information if escalated.
