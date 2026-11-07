# Save a campaign promo before adding an eligible item

## Readiness

**Ready with assumptions.** Delivery can proceed if assumptions A1–A4 are accepted; changing them may alter session storage, pricing, or UX behavior.

## Requirement summary

- **Objective (hypothesis):** Campaign shoppers may leave before adding an eligible item if they have to remember and re-enter the promo. Letting them save it earlier could help more of them reach a cart with the discount applied.
- **User outcome:** A shopper can save a valid campaign promo while the cart has no eligible items and receive the discount automatically after adding an eligible item in the same shopping session.
- **Business outcome:** Reduce campaign-to-cart friction while measuring how often saved promos lead to eligible cart additions and successful discount application.
- **In scope:** Promo capture, validation, pending and applied states, automatic revalidation after cart changes, session persistence, web and mobile behavior, failure feedback, and analytics.
- **Non-goals:** Creating promotion rules, changing item eligibility, supporting cross-device promo transfer, or defining the visual design of promo messages.

## Facts and assumptions

### Known facts

- The campaign launches next month.
- A shopper may submit the promo while the cart has no eligible item.
- The discount should apply automatically after an eligible item is added.
- The behavior is required on web and mobile.

### Assumptions

- **A1 — Actor:** Guest and signed-in shoppers can save the promo.
- **A2 — Session boundary:** A pending promo survives page refresh, navigation, and mobile app background/foreground within the current shopping session. It does not transfer to another browser or device and is cleared on sign-out.
- **A3 — Source of truth:** The promotion service validates status, dates, market, customer eligibility, item eligibility, and discount value whenever cart pricing is recalculated.
- **A4 — Stacking:** Only one promo can be pending or applied. Submitting another valid promo replaces the first after the shopper confirms the replacement.

### Recommendations

- **R1 — Pending message:** Explain that the promo is saved and will apply when an eligible item is added; do not show a zero-value “discount applied” state.
- **R2 — Privacy:** Record the internal campaign/promotion identifier in analytics, not the shopper-entered promo code.

## Behavioral definition

- **Actor:** Guest or signed-in shopper on the web storefront or mobile app.
- **Trigger:** The shopper submits a campaign promo from a campaign entry point or cart promo entry control.
- **Preconditions:** The storefront can reach the promotion service; the campaign identifier is recognized; and checkout has not been completed.
- **Expected behavior:**
  1. The client sends the promo for server-side validation.
  2. If the promo is valid but no eligible cart item exists, the system saves it as `Pending` for the current shopping session.
  3. The shopper sees that the promo is saved and what must happen for it to apply.
  4. Each cart change triggers normal server-side pricing and promo revalidation.
  5. When at least one eligible item is present and all other rules pass, the system changes the promo to `Applied`, updates totals, and shows the discount.
  6. If the promo becomes ineligible, the system follows the relevant failure or state-transition behavior below.

## State model

| Current state | Trigger or condition | Resulting state | Observable outcome |
| --- | --- | --- | --- |
| No promo | Shopper submits a valid promo; no eligible item is present | Pending | Promo is shown as saved, with no discount deducted |
| No promo | Shopper submits an invalid, inactive, or unavailable promo | No promo | Promo is not stored; reason and next action are shown |
| Pending | Eligible item is added and all promo rules pass | Applied | Cart totals include the discount and confirm application |
| Pending | Promo expires or shopper becomes ineligible before application | Rejected | Pending promo is removed; reason and recovery are shown |
| Pending | Shopper removes the promo | No promo | Promo and pending message disappear |
| Applied | Last eligible item is removed while the promo remains otherwise valid | Pending | Discount is removed; promo remains saved for this session |
| Applied | Promo fails revalidation after any cart change | Rejected | Discount is removed; totals refresh; reason is shown |
| Pending or Applied | Shopper confirms replacement with a second valid promo | Pending or Applied | First promo is removed; second promo takes the state its eligibility produces |
| Pending or Applied | Shopper signs out or the shopping session ends | No promo | Promo does not appear in the next session |

## Persistence and session behavior

- On web, `Pending` and `Applied` survive navigation and refresh in the same browser shopping session.
- On mobile, both states survive navigation and background/foreground in the same app shopping session.
- A pending promo does not transfer across browsers or devices under A2.
- Sign-out clears pending and applied promo state after the cart is repriced. A later sign-in does not restore it.
- The server revalidates the promo after restoration or cart recalculation; persisted client state alone never determines the discount.
- Exact session timeout follows the storefront's existing shopping-session policy; this feature does not define a new duration.

## Failure paths and recovery

| Failure or interruption | Expected handling | User-visible outcome | Recovery |
| --- | --- | --- | --- |
| Promotion service unavailable on submission | Do not save an unvalidated promo | Explain that the promo could not be checked | Allow retry without re-entering the code during the current screen visit |
| Promo invalid, inactive, or not available for shopper/market | Do not store the promo | Show a non-technical reason when safely available | Shopper may correct or remove the promo |
| Promo expires while pending | Remove pending state on next validation | Explain that the saved promo expired | Shopper may enter another promo |
| Revalidation fails temporarily after cart change | Do not show a newly applied discount; preserve last confirmed cart total until repricing resolves | Explain that totals could not be refreshed | Retry pricing and prevent checkout with an unconfirmed total |
| App closes or network drops after submission | Treat the server-confirmed session state as authoritative on return | Show restored pending/applied state, or a retry message if submission was never confirmed | Shopper can resubmit when online |

## Platform impact

| Platform or surface | Impact | Differences or constraints |
| --- | --- | --- |
| Responsive web | Capture and display pending/applied promo; restore session state | Must survive page refresh in the same session |
| iOS and Android apps | Same states, messages, and pricing outcome as web | Must restore after background/foreground; offline submission is not accepted |
| Cart/pricing service | Store the session promo candidate and revalidate on cart changes | Remains the source of truth for discount and totals |
| Checkout | Consume only a server-confirmed applied promo | Must block submission when totals are awaiting confirmation |
| Customer support | Needs visibility of applied promo outcome where cart/order tools already expose promotions | No new ability to override campaign rules is in scope |

## Analytics and measurement

| Event or measure | When captured | Required properties | Purpose |
| --- | --- | --- | --- |
| `promo_submitted` | Shopper submits a promo | campaign ID, platform, cart eligibility state | Measure attempts and empty-cart usage |
| `promo_saved_pending` | Valid promo enters `Pending` | campaign ID, platform, actor type | Measure successful saves before eligibility |
| `promo_applied` | Promo enters `Applied` | campaign ID, platform, actor type, eligible item count, discount value/currency | Measure conversion to applied discount |
| `promo_rejected` | Submission or revalidation is rejected | campaign ID when known, platform, reason category, previous state | Identify rule and expiry friction |
| Pending-to-applied rate | Reporting metric | count of pending promos that later apply / pending promos saved | Evaluate feature effectiveness |

Do not include raw promo codes, customer-entered text, or direct personal identifiers in these events.

## Dependencies

- **D1 — Promotion service:** Must return validity, eligibility, reason category, and stable campaign/promotion identifier.
- **D2 — Cart/pricing service:** Must persist the promo candidate at shopping-session scope and revalidate it on cart changes.
- **D3 — Existing session policy:** Must define session expiry and sign-out behavior consistently across supported platforms.
- **D4 — Content/design:** Must provide accessible pending, applied, replacement-confirmation, and failure messages.
- **D5 — Analytics taxonomy:** Event names and allowed reason categories require confirmation before instrumentation.

## Edge cases

- **E1 — Multiple quantities:** Apply the campaign's existing quantity and discount-cap rules; do not infer “one item only.”
- **E2 — Mixed eligibility:** Apply the discount only to eligible lines while keeping ineligible lines unchanged.
- **E3 — Item becomes ineligible:** Revalidate price or catalog changes before checkout and transition to `Rejected` if the rule no longer passes.
- **E4 — Remove then re-add:** A still-valid promo returns from `Pending` to `Applied` when an eligible item is re-added in the same session.
- **E5 — Second promo:** Do not replace the first promo until the shopper confirms; an invalid second promo leaves the first unchanged.
- **E6 — Concurrent cart updates:** Use the latest server-confirmed cart version so an older response cannot overwrite newer promo or pricing state.
- **E7 — Boundary time:** Promotion service time is authoritative when submission or checkout occurs at the campaign start/end boundary.

## Acceptance criteria

### AC1 — Save a valid promo before eligibility

- **Given** a shopper has no eligible item in the cart
- **When** they submit a currently valid campaign promo
- **Then** the promo is stored as `Pending` for the current shopping session
- **And** no discount is deducted
- **And** the shopper is told the promo will apply when an eligible item is added

### AC2 — Apply automatically after an eligible item is added

- **Given** a valid promo is `Pending`
- **When** the shopper adds an eligible item and server-side revalidation passes
- **Then** the promo becomes `Applied`
- **And** the cart total includes the correct discount
- **And** the shopper receives confirmation without resubmitting the promo

### AC3 — Return to pending when eligibility is removed

- **Given** a promo is `Applied`
- **When** the shopper removes the last eligible item and the promo remains otherwise valid
- **Then** the discount is removed from totals
- **And** the promo returns to `Pending` for the current shopping session

### AC4 — Reject an invalid submission

- **Given** no promo is saved
- **When** the shopper submits an invalid, inactive, or unavailable promo
- **Then** no promo is stored
- **And** the shopper sees an appropriate reason and can try another promo

### AC5 — Handle expiry while pending

- **Given** a promo is `Pending`
- **When** it expires before an eligible item is added and the system next validates it
- **Then** the promo becomes `Rejected` and is removed from pending state
- **And** the shopper is told that it expired

### AC6 — Preserve confirmed state within a session

- **Given** a promo is `Pending` or `Applied`
- **When** the shopper refreshes web, navigates within the storefront, or backgrounds and resumes the mobile app in the same session
- **Then** the server-confirmed state is restored
- **And** the promotion is revalidated when cart pricing is recalculated

### AC7 — Clear state at the defined boundary

- **Given** a promo is `Pending` or `Applied`
- **When** the shopper signs out or the existing shopping session ends
- **Then** the promo state is cleared
- **And** it is not restored in the next session

### AC8 — Keep web and mobile outcomes consistent

- **Given** equivalent shopper, campaign, session, and cart conditions
- **When** the flow is completed on responsive web, iOS, or Android
- **Then** validation, state transitions, discount outcomes, and failure reasons are functionally equivalent

### AC9 — Record outcome analytics safely

- **Given** the shopper submits, saves, applies, or is rejected for a promo
- **When** the corresponding outcome occurs
- **Then** the defined event is recorded once with campaign ID, platform, and outcome context
- **And** raw promo code and direct personal identifiers are excluded

## Open questions

| Priority | Question | Decision affected | Suggested owner |
| --- | --- | --- | --- |
| Non-blocking | What problem prompted the request, and what outcome would count as success for the campaign? | Confirms the objective and whether pending-to-applied rate is the right success signal | Product |
| Non-blocking | Are A1–A4 accepted as product policy for this release? | Actor scope, session behavior, validation, and stacking | Product |
| Non-blocking | Which existing session timeout applies on web and mobile? | Test setup and expiry timing | Product / Engineering |
| Non-blocking | What final analytics names and reason categories match the data taxonomy? | Instrumentation contract | Data / Product |

There are no blocking questions if A1–A4 are accepted before implementation starts.
