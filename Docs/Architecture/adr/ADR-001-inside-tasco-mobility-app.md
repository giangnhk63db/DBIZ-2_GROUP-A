# ADR-001: Build the feature inside the TASCO Mobility app

| Field | Value |
|---|---|
| Status | Accepted |
| Date | Lesson 4 |

**Context.** Car owners already use the TASCO Mobility app for Tasco services. The Verified
Vehicle Report needs the owner to enter a VIN, see the report, upload invoices and submit a sell
request.

**Decision.** Build the Verified Vehicle Report as a feature inside the existing TASCO Mobility
app. No separate app for car owners.

**Consequences.** Car owners install nothing new and sign in with the account they already have.
The feature follows the release schedule of the TASCO Mobility app. Staff screens are not part of
this app (see ADR-003).
