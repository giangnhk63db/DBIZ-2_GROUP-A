# ADR-002: Sign in with the existing TASCO Mobility account

| Field | Value |
|---|---|
| Status | Proposed |
| Date | Lesson 4 |

**Context.** Car owners already have a TASCO Mobility account, and the feature is part of the same app (ADR-001). A second sign-in would add effort for the owner, which goes against the goal of less manual work. F-00 Platform and Access needs to know who the user is and which role they have.

**Decision.** Identify the user with the existing TASCO Mobility account. The feature keeps no passwords of its own.

**Consequences.** The owner signs in once for the whole app. The feature depends on the TASCO Mobility account service, so when that service is down nobody can use the report. Staff accounts for the staff website are still to be decided with the client.
