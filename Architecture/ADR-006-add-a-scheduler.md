# ADR-006: Add a scheduler for timed jobs

| Field | Value |
|---|---|
| Status | Proposed |
| Date | Lesson 4 |

**Context.** Some work must start by itself, without an owner pressing a button: keeping the market listing and price history up to date (UC-2.06 Sync 180-Day Listing History and UC-2.10 Backfill 12-Week Price History) and refreshing data from the external sources.

**Decision.** Add a Scheduler in the business layer. It starts the Market price service jobs and the Data collection service jobs at the set times.

**Consequences.** Timed jobs have one place to run and one place to look at when they fail. The activity diagrams must show these jobs as started by the Scheduler, and the job times are listed in the Function List. To be confirmed against the activity diagrams of Lesson 7.
