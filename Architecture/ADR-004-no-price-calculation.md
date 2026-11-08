# ADR-004: The report does not calculate the car's price

| Field | Value |
|---|---|
| Status | Accepted |
| Date | Lesson 4 |

**Context.** Carpla confirmed that its preliminary quote is based on the information the customer sends, and that the final price needs an in-person inspection and depends on the market. The teacher's feedback was that the feature must not change Carpla's operations, and that a pricing formula is hard to verify.

**Decision.** The system does not calculate the car's price. It gives the owner and Carpla staff verified vehicle data, and a market price with its basis as a reference. Carpla staff give the preliminary quote.

**Consequences.** The market price shown to the owner is a reference, not a quote, and the screen wording must say so. The report does not replace the in-person inspection. No pricing model or its parameters are built, so no screen for managing them is needed.
