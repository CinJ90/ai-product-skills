# Implementation-ready requirement template

Use this template for the final deliverable. Adjust table columns and verbosity to fit the request, but keep each section visible. Write `Not specified` or `Not applicable — <reason>` where necessary.

## Readiness

Choose **Ready**, **Ready with assumptions**, or **Needs decisions**, followed by one sentence explaining why.

## Requirement summary

- **Objective (why build this?):** The problem or opportunity, who it affects, and what should improve. Mark an unconfirmed objective as a hypothesis or record it as an open question.
- **User outcome:**
- **Business outcome:**
- **In scope:**
- **Non-goals:**

## Facts and assumptions

### Known facts

- Fact supplied by the requester or an authoritative source.

### Assumptions

- **A1 — Assumption:** What is being assumed and why it is reasonable enough to proceed.

### Recommendations

- **R1 — Recommendation:** A proposed product choice that is not yet a requirement.

## Behavioral definition

- **Actor:**
- **Trigger:**
- **Preconditions:**
- **Expected behavior:** Numbered primary flow with observable results.

## State model

| Current state | Trigger or condition | Resulting state | Observable outcome |
| --- | --- | --- | --- |
|  |  |  |  |

Include initial, intermediate, successful, expired/cancelled, and recoverable failure states when relevant.

## Persistence and session behavior

Define what survives refresh, navigation, sign-in/sign-out, app backgrounding, device changes, timeout, and a new session. Address only relevant boundaries, but make exclusions explicit.

## Failure paths and recovery

| Failure or interruption | Expected handling | User-visible outcome | Recovery |
| --- | --- | --- | --- |
|  |  |  |  |

## Platform impact

| Platform or surface | Impact | Differences or constraints |
| --- | --- | --- |
|  |  |  |

Include affected customer surfaces, internal tools, APIs, notifications, or operational processes when relevant.

## Analytics and measurement

Connect any proposed success signal to the objective. Include a baseline or target only if it is known or clearly labeled as a proposal.

| Event or measure | When captured | Required properties | Purpose |
| --- | --- | --- | --- |
|  |  |  |  |

State when analytics are not required. Avoid collecting raw secrets, unnecessary personal data, or free text by default.

## Dependencies

- **D1 — Dependency:** Required capability, policy decision, data, service, design, or operational process.

## Edge cases

- **E1 — Edge case:** Condition and expected outcome.

## Acceptance criteria

### AC1 — Short behavior name

- **Given** a clear starting state and relevant preconditions
- **When** the actor or system performs the trigger
- **Then** an observable result occurs
- **And** any required state or persistence change is visible or verifiable

Repeat for material success, state transition, persistence, failure, recovery, and platform-specific behavior.

## Open questions

| Priority | Question | Decision affected | Suggested owner |
| --- | --- | --- | --- |
| Blocking / Non-blocking |  |  |  |

Do not invent an owner. Use `Unassigned` when ownership is unknown.
