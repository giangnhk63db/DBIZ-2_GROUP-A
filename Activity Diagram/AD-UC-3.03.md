# **Activity Diagram: AD-UC-3.03**

| Field | Value |
| ----- | ----- |
| AD ID | AD-UC-3.03 |
| Use case | UC-3.03 Check inspection status |
| Feature | F-03 Sell Request & Preliminary Quote |
| Primary actor | Car Owner · Vehicle Inspection Portal |
| Lanes | Car Owner · Tasco Mobility System · Vehicle Inspection Portal |
| Inputs (L3-L5) | Context Diagram · Use Case Diagram ( Sell Request & Preliminary Quote) · Sequence Diagram (S-9.03, S-9.04)  · W1 rows: FR-03-02-BE, FR-03-01-FE  · Referenced activity diagrams: AD-UC-3.01 |
| Trigger | Car Owner submits a sale request (UC-3.01); the system re-checks legal status in real time (UC-3.02 includes this use case) |
| Preconditions | A sale request is being submitted; the vehicle has a license plate and VIN on its verified report |
| Success end state | Inspection status is returned and used in the report (stop1) |
| Failure end state(s) | No record: “No data” is inserted in the report (stop2).  |
| FRs covered | FR-03-02 (L2, Should, NC). L3 in W1: FR-03-02-BE only; trigger step FR-03-01-FE belongs to UC-3.01 |
| Business rules applied | none |
| Version / status | v0.3 · Reviewed |
| Author | Nguyễn Hương Giang  |

1. ## **Code**

```mermaid
swimlane-beta TB
subgraph CO["Car Owner"]
start@{ shape: sm-circ }
a1("Submit sale request (AD-UC-3.01)")
end
subgraph TMS["Tasco Mobility System"]
s1("Query Inspection status (FR-03-02-BE)")
s2("Return Inspection status and insert in report (FR-03-02-BE)")
s3("Insert #quot;No data#quot; in report (FR-03-02-BE)")
s4("Block submit sale request and show error (FR-03-02-BE)")
stop1@{ shape: fr-circ }
stop2@{ shape: fr-circ }
end
subgraph VIP["Vehicle Inspection Portal"]
v1("Look up inspection status (FR-01-10-INT)")
d1{"Inspection status data available and API connection? (FR-03-02-BE)"}
end
start --> a1 --> s1 --> v1 --> d1
d1 -->|"Have data"| s2 --> stop1
d1 -->|"No Data or API fail"| s3 --> stop2

```

2. ## **Node trace**

| Node | Kind | Text | Traces to |
| ----- | ----- | ----- | ----- |
| a1 | Action | Submit sale request | AD-UC-3.01 |
| s1 | Action | Query Inspection status | FR-03-02-BE |
| v1 | Action | Look up inspection status | FR-01-10-INT |
| d1 | Decision | Inspection status data available and API connection? | FR-03-02-BE |
| s2 | Action | Return Inspection status and insert in report | FR-03-02-BE |
| s3 | Action | Insert “No data” in report | FR-03-02-BE |

3. ## **Branches and Acceptance tests**

| Branch | Given | When | Then |
| ----- | ----- | ----- | ----- |
| d1 \[Have data\] | Inspection status query is sent to the Vehicle Inspection Portal (v1) | Vehicle Inspection Portal returns the inspection status successfully  | System returns the inspection status and insert into report (s2) and stops at stop1 |
| d1 \[No Data\] | Inspection status query is sent to the Vehicle Inspection Portal (v1) | Vehicle Inspection Portal answers but has no record for the vehicle or connection times out | System inserts “No data” in the report (s3) and stops at stop2 |

4. ## **Object states used**

| Object | States in this diagram | Full state list (all diagrams) |
| ----- | ----- | ----- |
| Inspection status | Have data, No data | Have data, No data |


## 

5. ## **Screen touched**

6. ## **Open question**

| \# | Question | Blocking? | Default applied |
| ----- | ----- | ----- | ----- |
| 1 | \[NEEDS CLARIFICATION: W1 has only FR-03-02-BE for FR-03-02 (NC, Should). The diagram cites FR-03-02-INT, which does not exist. The same portal lookups exist as FR-01-10-INT / FR-01-11-INT / FR-01-12-INT in feature F-01. Reuse them, or add three INT rows to F-03 (and adjust W2 cost)?\] | No | Cite FR-01-10-INT (reuse of the F-01 connector, no new hours); integrator to confirm |
| 2 | \[NEEDS CLARIFICATION: Portal failure: the EARS of FR-03-02 says 'IF a legal status source is unavailable, attach No data and still forward the request'. The diagram (\[API fail\]), UC-3.02 in the use case list and sequence Figure 16 ('Any API gateway fails') all block the submit. Which is current?\] | No | Fix the activity diagram, delete branch block submission  |
| 3 | \[NEEDS CLARIFICATION: a1 repeats the first action of UC-3.01. Replace it by a call step 'Submit sale request (AD-UC-3.01)', as the guide recommends for include?\] | No | Change from FR-03-01 to AD-UC-3.01 |
| 4 | \[NEEDS CLARIFICATION: On the have-data guard s2 only returns the result. Does the result also go into the report (as s3 does for No data)?\] | No | Yes, add action insert in report |
| 5 | \[NEEDS CLARIFICATION: Timeout and retry: how long does the system wait for the portal and is there a retry (UC-3.06 retries 3 times)?\] | No | One attempt, no retry |
| 6 | \[NEEDS CLARIFICATION: FR-03-02 is shared by UC-3.02, 3.03, 3.04 and 3.05 and is a single L3 row (4 h). Split per lookup so each use case has its own 'n of n'?\] | No | Keep shared |
| 7 | \[NEEDS CLARIFICATION: The \[No data\] guard has no counterpart in sequence Figure 16, which only has the alt 'Any API gateway fails / All checks succeed'. Add a branch to the sequence?\] | No | Add an alt for 'portal answers without a record' to the sequence |
| 8 | \[NEEDS CLARIFICATION: Lane name of our system: the Context Diagram calls it 'Verified Vehicle Report' (module of the Tasco Mobility app); this diagram and sequence Figure 16 call it 'Tasco Mobility System'. Which name is the source of truth?\] | No | Rename node in the Context diagram. Use Tasco Mobility system name. |

7. ## **Verification**

| Check | Result | By | Date |
| ----- | ----- | ----- | ----- |
| Renders on mermaid.live | Pass | Nguyễn Hương Giang | 2026-11-04 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass by reading: d1 has two guards that cover each other | Nguyễn Hương Giang | 2026-11-04 |
| No action has two incoming flows (merge used instead) | Pass by reading | Nguyễn Hương Giang | 2026-11-04 |
| Every fork has a join | Pass: no fork in this diagram | Claude draft | 2026-11-04 |
| Every system action cites an FR | Pass | Nguyễn Hương Giang | 2026-11-04 |
| Every FR of this use case appears in the diagram | Pass | Nguyễn Hương Giang | 2026-11-04 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass | Nguyễn Hương Giang | 2026-11-04 |
| AI critic prompt run, findings resolved or logged as open questions | Pass | Nguyễn Hương Giang | 2026-11-04 |

