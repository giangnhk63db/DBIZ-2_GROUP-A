# ADR-007: Send all notifications through one Notification adapter

| Field | Value |
|---|---|
| Status | Proposed |
| Date | Lesson 4 |

**Context.** Two kinds of messages leave the system: status messages to car owners about the sale request, and failure alerts to Data Infrastructure Staff when a data source stops working. The channel for each (app notification, email or other) is not decided yet.

**Decision.** Every service sends its messages through one Notification adapter, which talks to the external notification channel. No service sends a message directly.

**Consequences.** The channel can be changed in one place without changing the services. The adapter is the single place where notification failures are caught. The channel itself is an open question for the client.
