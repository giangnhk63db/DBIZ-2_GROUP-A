# ADR-006: Put the legal status check in its own service

| Field | Value |
|---|---|
| Status | Accepted |
| Date | Lesson 4 |

**Context.** The legal status (inspection, unpaid traffic fines, mortgage) is queried from three
portals in two situations: when the car owner refreshes the vehicle report, and when the car
owner submits a sell request and the status must be re-checked before the report is forwarded.

**Decision.** Build the Legal status update service as its own service. Both the Vehicle report
service and the Sell request service call it.

**Consequences.** The three portal queries are written once and behave the same in both
situations. Two services depend on one service, so a change in the legal status check affects
both of them.
