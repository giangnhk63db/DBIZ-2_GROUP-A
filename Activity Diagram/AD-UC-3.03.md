# **Activity Diagram: AD-UC-3.04**

| Field | Value |
| ----- | ----- |
| AD ID | AD-UC-3.04 |
| Use case | UC-3.04 Check unpaid traffic fines |
| Feature | F-03 Sell Request & Preliminary Quote (W0 Table 2\) |
| Primary actor | Car Owner · Traffic Fine Lookup Portal |
| Lanes | Car Owner · Tasco Mobility System · Traffic Fine Lookup Portal |
| Inputs (L3-L5) | Context Diagram · Use Case Diagram (Figure 7, Sell Request & Preliminary Quote) · Sequence: Figure 16 Submit Sale Request Sequence Diagram (S-9.05, S-9.06) · W1 rows: FR-03-02-BE, FR-03-01-FE  |
| Trigger | Car Owner submits a sale request (UC-3.01); the system re-checks legal status in real time (UC-3.02 includes this use case) |
| Preconditions | A sale request is being submitted; the vehicle has a license plate and VIN on its verified report |
| Success end state | Unpaid fines is returned and used in the report (stop1) |
| Failure end state(s) | No record: “No data” is inserted in the report (stop2).  |
| FRs covered | FR-03-02. L3 in W1: FR-03-02-BE only; trigger step FR-03-01-FE belongs to UC-3.01 |
| Business rules applied | none |
| Version / status | v0.3 · Reviewed |
| Author | Nguyễn Hương Giang  |

1. ## **Code**

swimlane-beta TB

 subgraph CO\["Car Owner"\]

start@{ shape: sm-circ }

a1**(**"submit sale request (AD-UC-3.01**)**")

end

subgraph TMS\["Tasco Mobility System"\]

s1**(**"Query unpaid fines (FR-03-02-BE**)**")

s2**(**"Return Unpaid fines and insert into report (FR-03-02-BE**)**")

s3**(**"Insert \#quot;no data\#quot; in report (FR-03-02-BE**)**")

stop1@{ shape: fr-circ }

stop2@{ shape: fr-circ }

end

subgraph TFLP\["Traffic Fine Lookup Portal"\]

v1**(**"Lookup unpaid fines (FR-01-11-INT**)**")

d1{" "}

end

start \--\> a1 \--\> s1 \--\> v1 \--\> d1

d1 \--\>\|"Have data"\| s2 \--\> stop1

d1 \--\>\|"No data or API fail"\| s3 \--\> stop2

2. ## **Node trace**

| Node | Kind | Text | Traces to |
| ----- | ----- | ----- | ----- |
| a1 | Action | Submit sale request | FR-03-01-FE |
| s1 | Action | Query Unpaid fines | FR-03-02-BE |
| v1 | Action | Look up Unpaid fines | FR-01-11-INT |
| s2 | Action | Return Unpaid fines and insert into report | FR-03-02-BE |
| s3 | Action | Insert “No data” in report | FR-03-02-BE |

3. ## **Branches and Acceptance tests**

| Branch | Given | When | Then |
| ----- | ----- | ----- | ----- |
| d1 \[Have data\] | Unpaid fines query is sent to the Traffic Fine Lookup Portal (v1) | Traffic Fine Lookup Portal returns the unpaid fines successfully | System returns the unpaid fines and insert into report (s2) and stops at stop1 |
| d1 \[No data\] | Unpaid fines query is sent to the Traffic Fine Lookup Portal (v1) | Traffic Fine Lookup Portal answers but has no record for the vehicle or connection times out | System inserts “No data” in the report (s3) and stops at stop2 |

4. ## **Object states used**

| Object | States in this diagram | Full state list (all diagrams) |
| ----- | ----- | ----- |
| Unpaid fines | Have data, No data | Have data, No data |
| Sale request submission | Created | Created, Sending, Sent |

5. ## **Screen touched**

6. ## **Open question**

| \# | Question | Blocking? | Default applied |
| ----- | ----- | ----- | ----- |
| 1 | \[NEEDS CLARIFICATION: FR-03-02 is shared by UC-3.02, 3.03, 3.04 and 3.05 and is a single L3 row (4 h). Split per lookup so each use case has its own 'n of n'?\] | No | Keep shared; count 1 of 1 for each use case |
| 2 | \[NEEDS CLARIFICATION: Does “No data” mean 'no unpaid fines' or 'portal has no record for the plate'? They read differently in the report\] | Yes | Treat as 'portal has no record'; a clear plate returns a value, not No data |

7. ## **Verification**

| Check | Result | By | Date |
| ----- | ----- | ----- | ----- |
| Renders on mermaid.live | Not run: no browser in this pass. Owner to paste section 1 into mermaid.live | Nguyễn Hương Giang | 2026-11-04 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass by reading: d1 has two guards that cover each other | Nguyễn Hương Giang | 2026-11-04 |
| No action has two incoming flows (merge used instead) | Pass by reading | Nguyễn Hương Giang | 2026-11-04 |
| Every fork has a join | Pass: no fork in this diagram | Nguyễn Hương Giang | 2026-11-04 |
| Every system action cites an FR | Pass | Nguyễn Hương Giang | 2026-11-04 |
| Every FR of this use case appears in the diagram | 1 of 1 W1 rows (FR-03-01-FE) appear; FR-03-01-BE is cited but missing in W1 | Nguyễn Hương Giang | 2026-11-04 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Open: sequence has no sale request created step and no rejection for an uninitialized report  | Nguyễn Hương Giang | 2026-11-04 |
| AI critic prompt run, findings resolved or logged as open questions | Pass | Nguyễn Hương Giang | 2026-11-04 |

