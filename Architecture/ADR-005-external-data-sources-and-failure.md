# ADR-005: External vehicle and legal data, and what happens when a source fails

| Field | Value |
|---|---|
| Status | Accepted |
| Date | Lesson 4 |

**Context.** The report needs data from outside the system: mileage from toll passage history of VETC (Tasco's electronic toll collection company), maintenance and repair history from Carpla Service, battery data from EV (electric vehicle) charging stations, and inspection, unpaid fine and mortgage status from government portals. These sources can fail, and the government portals are outside the team's control.

**Decision.** Collect each kind of data through its own source in one Data collection service. When a government portal is unavailable, still send the sale request and the report to Carpla, with the legal data labelled as not updated and the date it was last retrieved. Failures are recorded as data issues for Data Infrastructure Staff.

**Consequences.** A failure of one portal does not block the owner's sale request. Carpla staff may see older legal data, and the label tells them so. The use case "Use Last Retrieved Legal Data" extends "Re-check Legal Status". Data Infrastructure Staff need monitoring and alert screens (ADR-007).
