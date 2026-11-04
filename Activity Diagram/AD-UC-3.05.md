# **Activity Diagram: AD-UC-3.05**

| Field | Value |
| ----- | ----- |
| AD ID | AD-UC-3.05 |
| Use case | UC-3.05 Check mortgage status |
| Feature | F-03 Sell Request & Preliminary Quote (W0 Table 2\) |
| Primary actor | Car Owner · external system: Secured Transaction Registry Portal |
| Lanes | Car Owner · Tasco Mobility System · Secured Transaction Registry Portal |
| Inputs (L3-L5) | Context Diagram· Use Case Diagram (Sell Request & Preliminary Quote) · Sequence: Submit Sale Request Sequence Diagram (S-9.07, S-9.08, S-9.09) · W1 rows: FR-03-02-BE, FR-03-01-FE · Referenced activity diagrams: AD-UC-3.01  |
| Trigger | Car Owner submits a sale request (UC-3.01); the system re-checks legal status in real time (UC-3.02 includes this use case) |
| Preconditions | A sale request is being submitted; the vehicle has a license plate and VIN on its verified report |
| Success end state | Mortgage status is returned and used in the report (stop1) |
| Failure end state(s) | No record: “No data or API fail” is inserted in the report (stop2).  |
| FRs covered | FR-03-02 (L2, Should, NC). L3 in W1: FR-03-02-BE. The portal lookup cites FR-01-12-INT (L3 of F-01, UC-1.10) |
| Business rules applied | none |
| Version / status | v0.3 · Reviewed |
| Author | Nguyễn Hương Giang  |

1. ## **Code**

swimlane-beta TB

  subgraph CO\["Car Owner"\]

      start@{ shape: sm-circ }

      a1**(**"Submit Sale request (AD-UC-3.01**)**")

  end

  subgraph TMS\["Tasco Mobility System"\]

      s1**(**"Query mortgage status (FR-03-02-BE**)**")

      s2**(**"Return mortgage status \#quot;No mortgage\#quot; and indicate no outstanding mortgage in report (FR-03-02-BE**)**")

      s3**(**"Return mortgage status \#quot;Mortgaged\#quot; and attach a red flag with mortgage details to the report (FR-03-02-BE**)**")

      s4**(**"Insert \#quot;No data\#quot; in report (FR-03-02-BE**)**")

      d2{"Is the vehicle mortgaged?  (FR-03-02-BE**)**"}

      stop1@{ shape: fr-circ }

      stop2@{ shape: fr-circ }

      stop3@{ shape: fr-circ }

  end

  subgraph STRP\["Secured Transaction Registry Portal"\]

      v1**(**"Look up mortgage status (FR-01-12-INT**)**")

      d1{"Mortgage status data available and API connection?  (FR-03-02-BE**)**"}

  end

  start \--\> a1 \--\> s1 \--\> v1 \--\> d1

  d1 \--\>\|"Have data"\| d2

  d1 \--\>\|"No data or API fail"\| s4 \--\> stop3

  d2 \--\>\|"Not mortgaged"\| s2 \--\> stop1

  d2 \--\>\|"Mortgaged"\| s3 \--\> stop2

2. ## **Node trace**

| Node | Kind | Text | Traces to |
| ----- | ----- | ----- | ----- |
| a1 | Action | Submit Sale request | AD-UC-3.01  |
| s1 | Action | Query mortgage status | FR-03-02-BE |
| v1 | Action | Look up mortgage status | FR-01-12-INT  |
| d1 | Decision | Mortgage status data available and API connection?  | FR-03-02-BE |
| d2 | Decision | Is the vehicle mortgaged?  | FR-03-02-BE |
| s2 | Action | Return mortgage status “No mortgage” and indicate no outstanding mortgage in report | FR-03-02-BE |
| s3 | Action | Return mortgage status “Mortgaged” and attach a red flag with mortgage details to the report | FR-03-02-BE |
| s4 | Action | Insert “No data” in report | FR-03-02-BE |

3. ## **Branches and Acceptance tests**

| Branch | Given | When | Then |
| ----- | ----- | ----- | ----- |
| d1 \[Have data\] | Mortgage status query is sent to the Secured Transaction Registry Portal (v1) | Portal returns the mortgage status data successfully | System continues to check whether the vehicle is mortgaged (d2) |
| d1 \[No data or API fail\] | Mortgage status query is sent to the Secured Transaction Registry Portal (v1) | Portal answers but has no record for the vehicle, or its connection times out or returns an error | System inserts “No data” in the report (s4) and stops at stop3 |
| d2 \[Not mortgaged\] | Portal has returned the mortgage status data (d1 \[Have data\]) | The data shows that the vehicle has no mortgage | System returns the mortgage status “No mortgage”, indicates in the report that there is no outstanding mortgage (s2), and stops at stop1 |
| d2 \[Mortgaged\] | Portal has returned the mortgage status data (d1 \[Have data\]) | The data shows that the vehicle is mortgaged | System returns the mortgage status “Mortgaged”, attaches a red flag with the mortgage details to the report (s3), and stops at stop2 |

4. ## **Object states used**

| Object | States in this diagram | Full state list (all diagrams) |
| ----- | ----- | ----- |
| Mortgage status | No mortgage, Mortgaged, No data | No mortgage, Mortgaged, No data |
| Report | Red flag attached, No outstanding mortgage indicated, No data inserted | Red flag attached, No outstanding mortgage indicated, No data inserted |
| Sale request submission | Created | Created, Sending, Sent |

5. ## **Screen touched**

6. ## **Open question**

| \# | Question | Blocking? | Default applied |
| ----- | ----- | ----- | ----- |
| 1 | \[NEEDS CLARIFICATION: d1 merges 'no record' and 'portal did not answer' into one guard, so the report shows 'No data' in both cases. Should the report tell the two apart (not found vs not checked)?\] | No | Both shown as 'No data' |
| 2 | \[NEEDS CLARIFICATION: The EARS of FR-03-02 has no clause for the mortgage result: s2 (no outstanding mortgage) and s3 (red flag with details) and decision d2 have no wording. Add a mortgage clause to FR-03-02 (FR-01-12 already has 'IF active mortgage exists, attach warning details; IF released, return unencumbered status')?\] | No | Add a clause to FR-03-02; hours stay in FR-03-02-BE for now |
| 5 | \[NEEDS CLARIFICATION: Which mortgage details does the red flag show? The Context Diagram flow lists credit institution, loan amount and active / released\] | No | Those three |
| 3 | \[NEEDS CLARIFICATION: d2 sits in the Secured Transaction Registry Portal lane, but it is the system that checks the returned data. Should d2 move to the system lane?\] | No | Move it to lane Tasco Mobility System |
| 4 | \[NEEDS CLARIFICATION: Timeout and retry: how long does the system wait for the portal, and is there a retry (UC-3.06 retries 3 times)?\] | No | One attempt, no retry |

7. ## **Verification**

| Check | Result | By | Date |
| ----- | ----- | ----- | ----- |
| Renders on mermaid.live | Pass | Nguyễn Hương Giang | 2026-11-04 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass by reading: three guards, one per outcome | Nguyễn Hương Giang | 2026-11-04 |
| No action has two incoming flows (merge used instead) | Pass by reading | Nguyễn Hương Giang | 2026-11-04 |
| Every fork has a join | Pass: no fork in this diagram (the par block of the sequence is UC-3.02) | Nguyễn Hương Giang | 2026-11-04 |
| Every system action cites an FR | Pass | Nguyễn Hương Giang | 2026-11-04 |
| Every FR of this use case appears in the diagram | Pass | Nguyễn Hương Giang | 2026-11-04 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass | Nguyễn Hương Giang | 2026-11-04 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass | Nguyễn Hương Giang | 2026-11-04 |
| AI critic prompt run, findings resolved or logged as open questions | Pass | Nguyễn Hương Giang | 2026-11-04 |

