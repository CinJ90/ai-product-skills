# Build guardrails for saved campaign promos

This is a proposed Build handoff based on the [refined promo requirement](refined-requirement.md). The goal is to let shoppers save a promo before they have an eligible item while applying discounts only when eligibility and pricing are confirmed. This example assumes the product team accepts the requirement's session and stacking policies.

## Build-agent operating rules

| Decision | Proposed rule |
| --- | --- |
| Allowed | Edit the scoped cart and promo code and run existing local tests with synthetic campaign codes. |
| Approval required | Ask before adding a dependency, changing shared CI settings, or pushing to a shared branch. |
| Blocked | Keep real shopper data and live campaign codes out of prompts, fixtures, and logs. Keep tests away from production pricing services. |

The agent checks scope and data use before acting. Enforce file and network limits through host permissions where available. The fixture hook below can check covered file writes. Test with an in-scope fixture edit, a dependency install attempt that requires approval, and an attempt to reach a blocked production endpoint. Use synthetic inputs and an isolated test environment.

## Enforcement and related safeguards

| Risk and timing | Invariant and enforcement | Failure and recovery | Verification and owner |
| --- | --- | --- | --- |
| **High · before release.** A shopper receives a stale or unauthorized discount. | On cart recalculation and checkout, the trusted pricing path revalidates the promo against the current cart, shopper, and campaign. Apply a discount only after pricing is confirmed. | On a pricing timeout, preserve the last confirmed total for display with a stale-state message. Defer checkout until repricing succeeds. | Test a valid pending-to-applied transition, an expired or ineligible promo, a stale cart version, and a timeout. Exercise checkout directly through the API. Owner role: pricing engineering. |
| **High · before release.** A saved promo leaks into another shopping session. | Bind saved and applied promo state to the authorized shopping session. Clear it at the session boundary defined in the requirement. | Reject access using a different session and present the new session's own promo state. | Verify same-session refresh and app resume, cross-session isolation, and sign-out followed by sign-in. Owner role: cart engineering. |
| **High · before merge and release.** Raw promo codes reach analytics or logs. | Use an allowlisted analytics payload at event emission. Cover rejection and error logging as well as the happy path. A CI test checks that a synthetic promo string never reaches captured analytics or logs. | Omit the unsafe payload and record a redacted diagnostic. A failing regression check blocks merge once it is configured as required. | Verify allowed campaign IDs, rejected extra fields, and invalid-code logging with synthetic fixtures. Confirm required CI enforcement separately. Owner roles: cart engineering and data. |
| **Medium · before merge.** An agent edit uses realistic customer or campaign data in fixtures. | If the coding host supports a pre-write hook with reliable denial, flag known prohibited fixture inputs in the relevant paths. Give the agent the path and remediation without echoing the sensitive value. A separate CI check scans changed fixtures for the same known patterns. | Reject a match before writing where supported. If the local hook is bypassed, required CI catches covered patterns before merge. Document that pattern checks have limited detection coverage. | Test a synthetic allowed fixture, a seeded match, a checker timeout, and a change that reaches CI without the hook. Owner role: developer tooling. |
| **Medium · before release and after rollout.** Customers miss a promo status change or encounter a rollout regression. | Validate accessible pending and error feedback on affected clients. Monitor promo application and pricing failures using redacted signals. | Route unexpected failures to the on-call owner for investigation and a pause of the rollout if warranted. | Test status feedback with keyboard and assistive technology where applicable. Exercise the alert with synthetic data and confirm who responds. Owner roles: design, QA, and operations. |

Review remaining lenses with the team. Existing retention and regional campaign obligations need confirmation by their owners. A new migration is out of scope in this example unless storing promo state changes the data model. Promo validation load and retry limits need a review if cart recalculation increases service traffic. Review changed dependencies if new tooling is introduced.

All controls are **Proposed**. No exception is assumed. Confirm the agent hook capability and CI requirement before claiming those checks can block an action. Application enforcement covers direct calls even when a developer or agent hook never runs.
