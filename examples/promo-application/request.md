# Ambiguous stakeholder request

> We are launching a campaign next month. Let customers apply the campaign promo even when their cart is empty. Once they add an eligible item, apply the discount automatically. It should work on web and mobile.

## Why this needs refinement

The request has a clear intention but leaves delivery-critical behavior undefined:

- Who can save the promo: guests, signed-in customers, or both?
- What does “apply” mean while no eligible item exists?
- How long should the promo survive, and across which session boundaries?
- What happens if the promotion expires or becomes invalid before an item is added?
- Can it stack with an existing promotion?
- How should web and mobile remain consistent?
- Which outcomes should be measured?

The companion [refined requirement](refined-requirement.md) shows one implementation-ready interpretation. Its assumptions are visible so stakeholders can approve or replace them.
