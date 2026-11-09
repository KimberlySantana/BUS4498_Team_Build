Provide resolving steps Task Specification

# BASIC INFORMATION
task_id: "T5"
task_name: "Provide resolving steps"
task_owner: "IT Support Agent"
```

## 1. Task Goal

- **Objective:** Give the employee safe, relevant troubleshooting steps that come only from the IT approved knowledge base for the identified problem type.

## 2. Inbound Inputs

### Input 1

- **Input name:** Problem classification
- **What it contains:** A structured record containing the request ID, problem type, urgency flag, supported status flag, and routing result.
- **Source:** T4 Identify problem type.

### Input 2

- **Input name:** Approved troubleshooting guidance
- **What it contains:** IT approved knowledge base entries containing supported problem types, troubleshooting steps, safety limitations, and escalation conditions.
- **Source:** IT approved knowledge base.\

### Input 3

- **Input name:** Employee troubleshooting feedback
- **What it contains:**  A text response from the employee stating whether the approved steps worked or describing the result of trying them.
- **Source:** Employee.

## 3. Tool Permissions and Boundaries

### Task-Wide Limits

- **Total task timeout:** 24 hours, including employee-response waiting time, tool calls, and retries.
- **Maximum tool calls:** 12

### Tool 1

- **Tool name:** retrieve_approved_resolving_steps
- **Tool type:** Database query.
- **Supports these permitted subtasks:** Retrieve approved guidance; compare approved options.
- **Allowed use:** Read only the IT-approved knowledge-base entries for the identified supported problem type.
- **Prohibited use:** Do not use unapproved websites, create new troubleshooting procedures, or retrieve confidential employee information.
- **Approval required:** None within the allowed use.
- **Timeout per call:** 20 seconds.
- **Maximum retries per call:** 1
- **Retry conditions and failure response:** Retry only for a temporary knowledge base error after a 10 second wait. If unavailable after retry, hand off to IT staff.\

### Tool 2

- **Tool name:** compose_resolving_steps
- **Tool type:** Language model call.
- **Supports these permitted subtasks:** Select approved steps; explain approved steps.
- **Allowed use:** Turn approved knowledge-base instructions into a clear employee-facing message without changing their meaning or adding new technical actions.
- **Prohibited use:** Do not invent steps, tell an employee to disable security settings, request credentials, change access permissions, or approve an action outside the knowledge base.
- **Approval required:** None within the allowed use.
- **Timeout per call:** 20 seconds.
- **Maximum retries per call:** 0
- **Retry conditions and failure response:** Not applicable. If the message cannot be safely produced from approved guidance, hand off to IT staff.

### Tool 3

- **Tool name:** send_resolving_steps
- **Tool type:**  API request.
- **Supports these permitted subtasks:** Deliver approved steps; request outcome feedback.
- **Allowed use:** Send the approved troubleshooting message to the employee through the approved support-message channel and record the delivery status.
- **Prohibited use:** Do not send messages to anyone other than the employee associated with the request ID or disclose the request to unauthorized people.
- **Approval required:** None within the allowed use.
- **Timeout per call:** 30 seconds.
- **Maximum retries per call:** 1
- **Retry conditions and failure response:** Retry only after a confirmed delivery failure and check the request ID and message status first to avoid duplicate messages. If the outcome is uncertain, hand off to IT staff.

## 4. How the Agent Should Reason

### Permitted Subtask 1

- **Subtask name:** Retrieve approved guidance
- **Subtask description:** Find knowledge base guidance that exactly matches the supported problem type and any relevant problem details.
- **Subtask boundary:** Use only approved knowledge base content. Do not proceed if the request is urgent, unsupported, unclear, or has no matching approved guidance.
- **Retry limits:** 1 additional attempt only for a temporary knowledge base failure.

### Permitted Subtask 2

- **Subtask name:** Select approved resolving steps
- **Subtask description:** Choose the most relevant approved primary step set based on the problem classification and available problem details.
- **Subtask boundary:** Select only approved guidance. Do not create, modify, or combine instructions in a way that changes their safety limits.
- **Retry limits:** 0


### Permitted Subtask 3

- **Subtask name:**  Evaluate troubleshooting feedback
- **Subtask description:** Review the employee’s result to determine whether the primary steps worked or whether an approved alternate step set may be relevant.
- **Subtask boundary:** The agent may offer no more than two alternate approved step sets after the primary step set. It does not decide that a problem is resolved; T6 performs that verification.
- **Retry limits:** 0

### Permitted Subtask 4

- **Subtask name:**  Escalate unsupported troubleshooting
- **Subtask description:** Identify when there is no approved next step, all approved options have been provided, or the case must be reviewed by IT staff.
- **Subtask boundary:** Do not continue troubleshooting beyond the approved primary set and two approved alternate sets.
- **Retry limits:** 0

- **Decision guidance:** After each subtask, use its findings to select the permitted subtask most likely to resolve the most important remaining uncertainty. Do not follow a fixed sequence. If no permitted subtask can make useful progress, stop and hand the case to a person.

## 5. When to Stop or Hand Off to a Human

- **Stop successfully when:** The employee has received the applicable approved step set or sets, delivery is recorded, and the request is ready for T6 Ask employee if problem was resolved.
- **Hand off early when:**  The issue is urgent, unsupported, unclear, has no approved knowledge-base guidance, requires a security-setting change, the employee is asked for confidential information, approved steps are exhausted, or a tool failure prevents safe completion.
- **Hand off to:** T7 Gather necessary handoff information for IT staff review.

Stop at the first applicable budget limit or handoff condition. While awaiting review, take no further autonomous action.

## 6. Outbound Deliverable

*Revise these default items if your task needs a more specific deliverable, or retain them if they fit.*

- **Status:** Completed or escalated to human.
- **Result or recommendation:** The completed result. If escalated before reaching a supported result, write undetermined.
- **Evidence summary:** The most important evidence supporting the result or explaining why no result could be reached.
- **Subtasks performed:** Permitted subtasks completed, including repeated attempts.
- **Unresolved issues:** Remaining uncertainties or questions; use none only if no unresolved issue remains.
- **Handoff note:** Reason for stopping, unresolved questions, and what the reviewer needs to decide; write "Not applicable" for a completed task.
- **Next task or recipient:** Who receives the completed output? Unresolved cases go to the handoff recipient above.
