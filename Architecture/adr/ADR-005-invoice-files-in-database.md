# ADR-005: Keep invoice files in the Vehicle Report database

| Field | Value |
|---|---|
| Status | Accepted |
| Date | Lesson 4 |

**Context.** Car owners upload external maintenance and repair invoices as PDF, XML or image
files. The files must stay linked to the vehicle record and to the data extracted from them.

**Decision.** Store the invoice files in the Vehicle Report database. No separate file storage.

**Consequences.** One data store to build and back up, and each file stays next to its vehicle
record. The database grows faster because of the files. If the volume becomes too large, the
files can move to a separate file storage in a later version.
