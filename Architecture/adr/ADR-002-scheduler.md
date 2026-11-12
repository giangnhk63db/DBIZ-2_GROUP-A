# ADR-002: Add a scheduler for weekly jobs

| Field | Value |
|---|---|
| Status | Accepted |
| Date | Lesson 4 |

**Context.** Two jobs must start by themselves every week, without any user action: the
synchronization of used car listings from Nextgen Việt Nam and Chợ Tốt, and the calculation of
the weekly market price for every tracked car.

**Decision.** Add a Scheduler to the business layer. The Scheduler starts the Market price
service every week. The 12-week price history is not started by the Scheduler; it is built once,
when a car model is tracked for the first time.

**Consequences.** Weekly jobs run on time even when nobody opens the app. A failed job must raise
an alert and be re-run, which is recorded in the function list (W1). The Scheduler is one more
element to build and monitor.
