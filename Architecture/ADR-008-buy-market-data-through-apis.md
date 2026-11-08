# ADR-008: Buy market price data through marketplace APIs, no crawler

| Field | Value |
|---|---|
| Status | Accepted |
| Date | Lesson 4 |

**Context.** The market price needs listing and price data from online used car marketplaces such as VuCar and ValuCar. A crawler (an automatic program that copies data from websites) was considered first.

**Decision.** Buy the data through the APIs (application programming interfaces) of online used car marketplaces. Do not build a crawler.

**Consequences.** The data comes in an agreed format, and the team does not maintain a program that breaks when a website changes. The system depends on the providers' contracts and prices, which must appear in the W6 cash flow. UC-2.06 Sync 180-Day Listing History reads from these APIs.
