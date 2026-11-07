# Requirement Refiner evaluation rubric

Use this rubric to assess an output generated from one case in [`cases.yaml`](cases.yaml). Evaluate meaning and decision quality rather than exact headings or wording.

## Scoring

Score each dimension from 0 to 2:

- **0 — Missing or harmful:** The output omits the concern, contradicts the request, or invents unmarked policy.
- **1 — Partial:** The concern is present but ambiguous, incomplete, or difficult to test.
- **2 — Strong:** The concern is explicit, internally consistent, appropriately scoped, and useful to delivery.

| # | Dimension | A strong answer demonstrates |
| --- | --- | --- |
| 1 | Objective and scope | Captures the stated problem and desired improvement or asks a focused question when either is unclear. Labels any inferred objective and keeps scope aligned with the request. |
| 2 | Actor, trigger, preconditions | Identifies who/what initiates behavior and the conditions required |
| 3 | State model | Describes current, intermediate, success, and relevant exit/failure states with valid transitions |
| 4 | Expected behavior | Gives a coherent primary flow with observable outcomes |
| 5 | Persistence/session behavior | Defines relevant refresh, navigation, sign-in/out, timeout, device, or restart boundaries |
| 6 | Failures and recovery | Covers material failures, user feedback, system outcome, and recovery where possible |
| 7 | Platform and analytics | Identifies affected surfaces/differences and measurable events or explicitly justifies no tracking |
| 8 | Dependencies and edge cases | Exposes real external needs and high-value boundary conditions without speculative sprawl |
| 9 | Acceptance criteria | Provides independently testable coverage of success, state changes, persistence, and important failures |
| 10 | Factual discipline and questions | Separates facts, assumptions, recommendations, and prioritized open questions; gives an honest readiness status |

Maximum score: **20**.

## Pass criteria

An output passes when:

- total score is **16 or higher**;
- dimensions 1, 3, 5, 6, 9, and 10 each score at least **1**; and
- there is no critical failure.

## Critical failures

Fail the output regardless of score if it:

- materially changes the requester's goal without labeling a recommendation;
- presents invented business rules, legal obligations, security controls, or platform capabilities as facts;
- presents an inferred objective or a made-up success target as confirmed;
- labels the requirement **Ready** while a blocking decision is unresolved;
- omits a destructive or irreversible consequence evident in the request; or
- produces acceptance criteria that cannot be observed or tested.

## Suggested evaluation process

1. Run the same case in a fresh conversation with the skill enabled.
2. Save the complete response before comparing it with any example.
3. Score all ten dimensions and cite one piece of evidence for each score.
4. Record critical failures separately.
5. Improve the smallest relevant skill instruction or reference, then rerun the failed case and one unaffected regression case.
