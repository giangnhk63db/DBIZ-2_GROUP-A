# ADR-003: Send the sale request and report to Carpla's existing CRM

| Field | Value |
|---|---|
| Status | Proposed |
| Date | Lesson 4 |

**Context.** Tasco already has a CRM (customer relationship management system) that Carpla staff use. The sale request and the verified vehicle report must reach Carpla staff, who give the preliminary quote. It is not yet confirmed with Tasco and Carpla that the CRM can receive the package and return the quote.

**Decision.** Package the sale request with the verified vehicle report and send it to the existing Carpla CRM. If sending fails, retry up to 3 times with a 5-second wait. Build no new screens for Carpla staff.

**Consequences.** Carpla staff keep working in the tool they already use. If Tasco confirms the CRM can do this, the staff screens for reviewing the report and sending the quote are not needed. If not, this ADR is revised and those staff screens are added. The question is open with the client.
