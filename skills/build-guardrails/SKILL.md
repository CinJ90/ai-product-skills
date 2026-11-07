---
name: build-guardrails
description: Design and, when requested, implement risk-based guardrails for a feature entering the SDLC Build phase. Use to turn requirements and risks into agent hooks, repository or CI checks, application controls, and review gates with testable failure behavior.
---

# Build Guardrails

Help the build agent preserve the feature's objective and prevent concrete harms while it implements the feature. A guardrail states what must hold, where it is enforced, and how the team can verify it. A hook is one way to run a guardrail in response to an event.

## Establish the build context

- Start from the feature objective, acceptance criteria, and unresolved decisions. A refined requirement is useful input when available.
- Identify affected users, sensitive data and where it flows, trust and tenant boundaries, external dependencies, irreversible actions, and who can trigger them. Include the build agent's file, command, credential, and network access when in scope.
- Inspect relevant repository instructions, existing checks and tests, CI settings available to you, and supplied policies. Confirm the host's supported hook events and enforcement semantics before relying on a specific hook.
- Distinguish observed controls, confirmed obligations, assumptions, and gaps. Ask focused questions when the answer could change a high-impact control or release decision. Mark unclear legal, contractual, or policy applicability for the appropriate owner to verify.

## Review and prioritize

Use the [review lenses and guardrail record](references/review-guide.md). Consider every lens, keep a short reason for any material exclusion, and add domain-specific lenses when the feature warrants them.

1. Frame each material risk as a failure or misuse scenario and its impact. Consider accidents, abuse, partial failures, and the ways a control can be skipped or bypassed.
2. Prioritize by impact, likelihood or exposure, reversibility, and confirmed obligations. Separate controls needed before implementation, before merge or release, and after release. Flag decisions that prevent safe progress on the affected work.
3. Reuse existing controls where they cover the risk and identify any gap. For each proposed control, specify the invariant, trigger, enforcement point, failure behavior, accountable role, and verification. Record assumptions and dependencies.

## Choose where to enforce

| Layer | Use it for | Verify |
| --- | --- | --- |
| Agent tool hook | Check a supported tool action before it runs or collect evidence after it runs | Whether the host can actually block the action and what happens when the hook errors |
| Local developer hook | Give fast feedback on common mistakes | Which paths bypass the hook, including skipped hooks and commits from other clients |
| Required CI or review gate | Independently verify code, configuration, or approval before merge or release | Whether the check is required on every relevant path and whether its result can be overridden |
| Application or data boundary | Enforce runtime authorization, integrity, limits, or safe state changes | Coverage across direct API calls, background jobs, and other paths to the same action |
| Runtime monitoring | Detect regressions and trigger response or rollback | Signal quality, ownership, and a rehearsed response |

Place prevention at the boundary that controls the outcome. Use agent or local checks for early feedback and pair them with independent enforcement when bypass would expose a material risk. Use post-action hooks for detection or evidence. If a requested hook cannot enforce the intended policy, explain the gap and select a viable enforcement point.

## Deliver and verify

- For a planning request, return a prioritized guardrail plan with evidence, open decisions, and residual risk. For an implementation request, implement the in-scope controls using existing tooling and test them locally or in an authorized test environment.
- For hooks, define allowed inputs, exit or response behavior, timeout handling, and actionable feedback. Treat input as data. Keep privileges narrow and avoid recording secrets or sensitive payloads.
- Verify an allowed path, a violation, and a checker failure or bypass where relevant. Use synthetic data and safe fixtures. Document whether each control prevents, detects, or routes for human review.
- Describe any exception path with the approver, scope, expiry, and audit trail. Treat changes to approval policies, shared CI settings, production controls, and live data as separate actions that require authorization.
- Report each guardrail as **Proposed**, **Implemented** (with verification still due), or **Verified** (with evidence). State remaining checks and any decisions that block safe implementation, merge, or release.
