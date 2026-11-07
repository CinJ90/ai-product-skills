---
name: requirement-refiner
description: Turn ambiguous product requests into implementation-ready requirements for product, design, engineering, QA, and analytics. Use when a feature request, ticket, stakeholder ask, or early concept lacks a clear objective, explicit behavior, or testable acceptance criteria.
---

# Requirement Refiner

Transform an incomplete product request into a shared, testable definition of the intended behavior. Preserve the requester's goal while exposing decisions that still need an owner.

## Working rules

- Treat only user-provided or authoritative source information as fact.
- Label inferred details as **Assumption** or **Recommendation**. Never turn an example into policy without saying so.
- Ask targeted questions before drafting when an answer would materially change the feature's scope, safety, irreversible behavior, or architecture. Otherwise, proceed with bounded assumptions and collect unresolved decisions under **Open questions**.
- Prefer observable product behavior over internal implementation. Include implementation constraints only when supplied or necessary to protect a stated requirement.
- Use `Not specified` or `Not applicable — <reason>` instead of silently omitting a required dimension.
- Keep non-goals explicit when adjacent behavior could reasonably be mistaken as in scope.
- Do not call a requirement implementation-ready while a blocking product decision remains unresolved. State its readiness honestly.

## Refine the request

1. Establish the **objective (why build this?)**. Capture the problem or opportunity, who it affects, and what should improve. Use what the requester already provided and ask one focused question about what is missing. If both the problem and outcome are unclear, ask “What problem should this feature solve, and what outcome would tell us it worked?” If an answer is unavailable, carry the question into the draft and label any inferred objective as a hypothesis. Treat the missing objective as blocking when it materially affects scope or the choice of approach.
2. State the user outcome, business outcome, and scope in plain language. Extract known facts and list any assumptions needed to make progress.
3. Resolve or expose the following dimensions:
   - actor;
   - trigger;
   - preconditions;
   - current, target, and intermediate state;
   - expected behavior and state transitions;
   - persistence and session behavior;
   - failure paths and recovery;
   - platform impact;
   - analytics and measurement;
   - dependencies;
   - edge cases;
   - acceptance criteria; and
   - open questions, with an owner or decision needed when known.
4. Write the result using [references/requirement-template.md](references/requirement-template.md). Adapt the amount of detail to the risk and complexity, but account for every section.
5. Check the output against the quality gate before returning it.

## Acceptance criteria

- Use independently testable scenarios, preferably `Given / When / Then` when sequence and state matter.
- Cover the primary success path, meaningful state transitions, persistence boundaries, and material failure or recovery paths.
- Include platform-specific criteria only where behavior differs.
- Describe the outcome a user or system can observe; avoid prescribing UI layout, API shape, database schema, or event transport unless those are explicit constraints.
- Make each criterion traceable to a stated behavior, rule, edge case, or risk.

## Open questions

Prioritize questions instead of producing a long discovery checklist:

- **Blocking:** implementation or safe release should not begin without the answer.
- **Non-blocking:** the team can proceed using a stated assumption.

For each question, explain what decision it affects. If the user answers a question, incorporate the answer into the requirement rather than leaving it open.

## Quality gate

Before returning the refined requirement, verify that:

- every required dimension is addressed or explicitly marked not specified/applicable;
- state transitions and persistence boundaries are unambiguous;
- failure behavior tells the user what happened and what they can do next;
- the objective is stated or explicitly open, and any proposed success signal connects to it without inventing a baseline or target;
- analytics have a purpose and avoid unnecessary sensitive data;
- dependencies identify the capability or decision required, without inventing a team owner;
- acceptance criteria are observable and collectively cover the important behavior; and
- facts, assumptions, recommendations, and unresolved questions cannot be confused with one another.

End with a readiness label:

- **Ready** — no blocking questions remain.
- **Ready with assumptions** — implementation can proceed if the listed assumptions are accepted.
- **Needs decisions** — one or more blocking questions remain.
