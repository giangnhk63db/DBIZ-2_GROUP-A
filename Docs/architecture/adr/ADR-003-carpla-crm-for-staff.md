# ADR-003: Carpla staff use the existing CRM

| Field | Value |
|---|---|
| Status | Accepted |
| Date | Lesson 4 |

**Context.** Carpla staff must open each sell request, read the verified report and the market
price, and enter a preliminary quote. Tasco already has a CRM (Customer Relationship Management,
software for managing customers) that Carpla staff use every day.

**Decision.** Carpla staff review the sell request and enter the preliminary quote in the
existing Carpla CRM. This system builds no new staff screens.

**Consequences.** No staff screens to design or build, and staff keep working in the tool they
already know. The design depends on the CRM receiving the report package and returning the quote;
the team still needs to confirm this with Tasco and Carpla. Staff screens appear in the
presentation layer as part of the CRM, not as part of this system.
