**Activity Diagram: AD-UC-1.07 Refresh Legal Status**

*1\. Diagram*

**Reading notes:**

- The workflow strictly prevents duplicate user actions at the UI level (FR-01-09-FE) before initiating any external queries.
- A fork node (f1) is utilized to dispatch three parallel queries to independent government portals to optimize data retrieval time.
- External portals act as black boxes (Process and return response). The system independently evaluates each response (d2, d3, d4) to determine success or failure.
- Fault tolerance is integrated by logging individual API failures (c2, c3, c4) without interrupting the overall process.
- All paths synchronize at the join node (j1), allowing the system to aggregate and store the consolidated results (FR-01-13-BE) and display partial results even if some queries fail.

**2\. Node trace**

| Node | Kind | Text | Traces to |
| ----- | ----- | ----- | ----- |
| d1 | Decision | Request already in progress? | FR-01-09-FE |
| c1 | Action | Disable duplicate click | FR-01-09-FE |
| s1 | Action | Query Inspection Portal | FR-01-10-INT |
| s2 | Action | Query Traffic Fine Portal | FR-01-11-INT |
| s3 | Action | Query Secured Transaction | FR-01-12-INT |
| d2 | Decision | Inspection response OK? | FR-01-10-INT |
| c2 | Action | Log Inspection error | FR-01-10-INT |
| d3 | Decision | Traffic Fine response OK? | FR-01-11-INT |
| c3 | Action | Log Traffic Fine error | FR-01-11-INT |
| d4 | Decision | Mortgage response OK? | FR-01-12-INT |
| c4 | Action | Log Mortgage error | FR-01-12-INT |
| s4 | Action | Aggregate & store results | FR-01-13-BE |
| s5 | Action | Display consolidated card | FR-01-13-FE |
