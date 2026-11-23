# ADR-004: Each service calls its own external systems

| Field | Value |
|---|---|
| Status | Accepted |
| Date | Lesson 4 |

**Context.** The system reads data from nine external systems: VETC, Carpla Service, EV charging
stations, the Vehicle Inspection Portal, the Traffic Fine Lookup Portal, the Secured Transaction
Registry Portal, the Electronic Invoice Lookup Portal, Nextgen Việt Nam and Chợ Tốt. Each
service needs only a few of them, and the sets hardly overlap.

**Decision.** Each service calls the external systems it needs directly. No shared connector
between the services and the external systems.

**Consequences.** The diagram shows clearly which service uses which external system, and
services stay independent. Storing credentials, retrying and limiting the call rate are done
inside each service; for the two listing partners this is a work item of the Market price
service in the function list (W1).
