# Build guardrail review guide

Use the lenses to find material risks for the feature and the agent building it. Record each lens as relevant, not applicable with a reason, or needing context. Combine lenses when the same control addresses several risks.

| Lens | Questions to ask |
| --- | --- |
| Product and abuse | Which rules protect eligibility, pricing, entitlements, fairness, or workflow integrity? Can a user or partner game them? |
| Security | Who may perform each action and on whose resources? Where do untrusted inputs, secrets, privileged commands, and cross-tenant access enter? |
| Privacy and data governance | What data is necessary? Where can it appear in prompts, logs, analytics, exports, caches, or third-party tools? What governs access, retention, and deletion? |
| Legal, compliance, and policy | Which supplied obligations apply to this data, region, or workflow? Who confirms gaps, exceptions, and evidence requirements? |
| Reliability and recovery | What happens on timeout, retry, duplicate delivery, partial completion, or rollback? Can the system recover safely and explain its state? |
| Data integrity and migrations | Which states and transitions must hold? Are changes compatible with old clients and existing data? Can backfills and retries avoid corrupting state? |
| Accessibility and user trust | Can people understand and recover from a blocked action? Are feedback, controls, and state changes accessible on affected platforms? |
| Supply chain and delivery | Are dependency, build, config, and artifact changes trusted? Can a check be bypassed through another build, merge, or deploy route? |
| Performance and cost | Can spikes, loops, or abusive input exhaust quotas or cause unexpected cost? What limits protect other users? |
| Operations and observability | What tells operators the control is working or failing? What evidence is safe to collect, and who responds? |
| Agent and AI safety, when relevant | Can untrusted content steer tool use, expose secrets, or trigger writes outside scope? For AI features, where do model outputs influence consequential actions? |

## Guardrail record

For each material control, capture the following fields. Use a compact table or a short record per control.

| Field | What to record |
| --- | --- |
| Risk and impact | A concrete failure or misuse, affected parties, and priority with rationale |
| Invariant | The observable condition that must hold and its source, such as an acceptance criterion or confirmed policy |
| Trigger and enforcement | The event, layer, covered paths, and whether the control prevents, detects, or routes for review |
| Violation and unavailable checker | What happens in each case, what the user or agent sees, and how to recover |
| Bypass and exceptions | How the control can be skipped, what independent check covers that gap, and whether an approved exception exists |
| Verification and evidence | Allowed, rejected, and relevant bypass or failure fixtures plus evidence to keep |
| Owner and timing | Accountable role or unassigned, when the control must be in place, and current status |

If a required control is unavailable, define the behavior at its boundary. A check protecting an irreversible or privileged action may need to deny or defer that action until verification returns. For other controls, weigh availability and user impact before selecting a degraded mode. If a residual risk is accepted, record the decision, scope, owner, and revisit condition.
