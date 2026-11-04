# **Activity Diagram: AD-UC-3.01**

| Field | Value |
| ----- | ----- |
| AD ID | AD-UC-3.01 |
| Use case | UC-3.01 Submit sale request |
| Feature | F-03 Sell Request & Preliminary Quote |
| Primary actor | Car Owner |
| Lanes | Car Owner · Tasco Mobility System |
| Inputs (L3-L5) | Context Diagram · Use Case Diagram (UC-3) · Sequence: Submit Sale Request Sequence Diagram (steps S-9.01, S-9.02) · W1 rows: FR-03-01 (FR-03-01-FE)   |
| Trigger | Car Owner enters current mileage, desired price and phone number and selects Submit sale request |
| Preconditions | Car Owner is signed in; the verified report was created for the VIN by UC-1.01 Enter VIN to initialize Report (FR-01-01) |
| Success end state | A sale request exists and the owner has been told it is being processed (stop1) |
| Failure end state(s) | Request rejected, an error is returned and no sale request exists (stop2) |
| FRs covered | FR-03-01 (L2, Must, EARS: create the sale request, trigger the legal status re-check and the packaging; IF the report is not initialized for the VIN, reject and display an error). L3 in W1: FR-03-01-FE only |
| Business rules applied | none |
| Version / status | v0.3 · Reviewed |
| Author | Nguyễn Hương Giang  |

1. ## **Code**

swimlane-beta TB

subgraph CO\["Car Owner"\]

start@{ shape: sm-circ }

a1**(**"Submit sale request with entering mileage**,** desired price and phone number (FR-03-01-FE**)**")

end

subgraph TMS\["Tasco Mobility System"\]

s1**(**"Receive sale request for the verified report (FR-03-01-BE**)**")

d1{"Has the report been created for this VIN? **(**FR-03-01-BE**)**"}

s2**(**"Create sale request (FR-03-01-BE**)**")

s3**(**"Notify car owner that the sale request is being processed (FR-03-01-FE**)**")

s4**(**"Reject request and return error (FR-03-01-BE**)**")

stop1@{ shape: fr-circ }

stop2@{ shape: fr-circ }

end

start \--\> a1 \--\> s1 \--\> d1

d1 \--\>\|"Yes"\| s2 \--\> s3 \--\> stop1

d1 \--\>\|"No"\| s4 \--\> stop2

2. ## **Node trace**

| Node | Kind | Text | Traces to |
| ----- | ----- | ----- | ----- |
| a1 | Action | Submit sale request with entering mileage, desired price and phone number | FR-03-01-FE |
| s1 | Action | Receive sale request for the verified report | FR-03-01-BE |
| d1 | Decision | Has the report been created for this VIN? | FR-03-01-BE |
| s2 | Action | Create sale request | FR-03-01-BE |
| s3 | Action | Notify car owner that the sale request is being processed | FR-03-01- FE |
| s4 | Action | Reject request and return error | FR-03-01-BE |

   ## 

3. ## **Branches and Acceptance tests**

| Branch | Given | When | Then |
| ----- | ----- | ----- | ----- |
| d1 \[Yes\] | This owner has  created a verified report (created by UC-1.01) | Owner submits mileage, desired price VND, phone  | Sale request is created, owner is told it is being processed, flow ends at stop1 |
| d1 \[No\] | This owner has not created a verified report  | Owner submits the same three values | Request is rejected, an error is returned, no sale request exists, flow ends at stop2 |

4. ## **Object states used**

| Object | States in this diagram | Full state list (all diagrams) |
| ----- | ----- | ----- |
| Vehicle report creating | Created, Not created | Created, Not created |
| Sale request submission | Created | Created, Sending, Sent |

5. ## **Screen touched**

6. ## **Open question**

| \# | Question | Blocking? | Default applied |
| ----- | ----- | ----- | ----- |
| 1 | \[NEEDS CLARIFICATION: FR-03-01-BE is cited three times (s1, s2, s4) but W1 has only FR-03-01-FE for FR-03-01. Add an FR-03-01-BE row (size, hours)?\] | No | Add the row; integrator re-runs W2 cost |
| 2 | \[NEEDS CLARIFICATION: The EARS of FR-03-01 says the system displays an error on rejection, but the only FE row describes the button, the form and the processing notification. Does FR-03-01-FE also cover the error display (s4)?\] | No | Yes, extend the FE description |
| 4 | \[NEEDS CLARIFICATION: The phone number appears in the diagram and the use case list, but neither the EARS nor the FE description of FR-03-01 mentions it (only mileage and desired price). Add it?\] | No | Add to the FE description |
| 5 | \[NEEDS CLARIFICATION: Are the three inputs validated (empty phone, non-numeric mileage)? No FR and no decision exist for invalid input\] | No | No validation in this release |

7. ## **Verification**

| Check | Result | By | Date |
| ----- | ----- | ----- | ----- |
| Renders on mermaid.live | Pass | \[owner\] | \[date\] |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass by reading: d1 has two guards that cover each other | Nguyễn Hương Giang | 2026-11-04 |
| No action has two incoming flows (merge used instead) | Pass by reading | Nguyễn Hương Giang | 2026-11-04 |
| Every fork has a join | Pass: no fork in this diagram | Claude draft | 2026-11-04 |
| Every system action cites an FR | Pass | Nguyễn Hương Giang | 2026-11-04 |
| Every FR of this use case appears in the diagram | Pass | Nguyễn Hương Giang | 2026-11-04 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass | Nguyễn Hương Giang | 2026-11-04 |
| AI critic prompt run, findings resolved or logged as open questions | Pass | Nguyễn Hương Giang | 2026-11-04 |

