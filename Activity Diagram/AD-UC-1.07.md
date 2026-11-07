# Activity Diagram: AD-UC-1.07 Refresh Legal Status

| Field | Value |
|---|---|
| AD ID | `AD-UC-1.07` |
| Use case | `UC-1.07 Refresh Legal Status` |
| Feature | `F-01 Vehicle Identification & Status` |
| Primary actor | Car Owner |
| Lanes | Car Owner · Verified Vehicle Report · Vehicle Inspection Portal · Traffic Fine Lookup Portal · Secured Transaction Registry Portal |
| Inputs (L3-L5) | Context Diagram v1 · Use Case Diagram v1 · Sequence: SD-UC-1.07 · W1 rows (FR-01-09-FE, FR-01-10-INT, FR-01-11-INT, FR-01-12-INT, FR-01-13-BE, FR-01-13-FE) |
| Trigger | Car owner clicks "Refresh Legal Status" to update the vehicle's consolidated information |
| Preconditions | User is authenticated and viewing an existing vehicle profile on the system |
| Success end state | Legal status data is successfully refreshed from all three external portals, aggregated, stored, and the consolidated card is displayed to the user |
| Failure end state(s) | One or more external portals timeout or fail; the system logs the errors, aggregates the available partial data, and displays it with specific error indicators |
| FRs covered | `FR-01-09-FE`, `FR-01-10-INT`, `FR-01-11-INT`, `FR-01-12-INT`, `FR-01-13-BE`, `FR-01-13-FE` |
| Business rules applied | `BR-REF-01`, `BR-ERR-01` |
| Version / status | v0.1 - Draft |
| Author | Nguyễn Danh Quang |


## 1. Diagram

```mermaid
swimlane-beta TB
  subgraph CAR["Car Owner"]
    start@{ shape: sm-circ }
    a1("Click Refresh Legal Status")
    a2("View consolidated legal status")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end
  
  subgraph SYS["Verified Vehicle Report"]
    d1{"Request already in progress?"}
    c1("Disable duplicate click")
    f1@{ shape: fork }   
    s1("Query Inspection Portal (FR-01-10-INT)")
    s2("Query Traffic Fine Portal (FR-01-11-INT)")
    s3("Query Secured Transaction (FR-01-12-INT)")
    d2{"Inspection response OK?"}
    c2("Log Inspection error")
    d3{"Traffic Fine response OK?"}
    c3("Log Traffic Fine error")
    d4{"Mortgage response OK?"}
    c4("Log Mortgage error")
    j1@{ shape: join }
    s4("Aggregate & store results (FR-01-13-BE)")
    s5("Display consolidated card (FR-01-13-FE)")
  end
  
  subgraph PORTAL1["Vehicle Inspection Portal"]
    e1("Process and return response")
  end
  subgraph PORTAL2["Traffic Fine Lookup Portal"]
    e2("Process and return response")
  end
  subgraph PORTAL3["Secured Transaction Registry Portal"]
    e3("Process and return response")
  end
  
  start --> a1 --> d1
  d1 -->|"Yes"| c1 --> stop2
  d1 -->|"No"| f1
  f1 --> s1 --> e1 --> d2
  f1 --> s2 --> e2 --> d3
  f1 --> s3 --> e3 --> d4
  d2 -->|"Yes"| j1
  d2 -->|"No"| c2 --> j1
  d3 -->|"Yes"| j1
  d3 -->|"No"| c3 --> j1
  d4 -->|"Yes"| j1
  d4 -->|"No"| c4 --> j1
  j1 --> s4 --> s5 --> a2 --> stop1
```
**Reading notes:**
- The workflow strictly prevents duplicate user actions at the UI level (FR-01-09-FE) before initiating any external queries.
- A fork node (`f1`) is utilized to dispatch three parallel queries to independent government portals to optimize data retrieval time.
- External portals act as black boxes (Process and return response). The system independently evaluates each response (`d2`, `d3`, `d4`) to determine success or failure.
- Fault tolerance is integrated by logging individual API failures (`c2`, `c3`, `c4`) without interrupting the overall process.
- All paths synchronize at the join node (`j1`), allowing the system to aggregate and store the consolidated results (FR-01-13-BE) and display partial results even if some queries fail.

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| d1 | Decision | Request already in progress? | `FR-01-09-FE` |
| c1 | Action | Disable duplicate click | `FR-01-09-FE` |
| f1 | Fork | Dispatch parallel portal queries | `BR-REF-01` |
| s1 | Action | Query Inspection Portal | `FR-01-10-INT` |
| s2 | Action | Query Traffic Fine Portal | `FR-01-11-INT` |
| s3 | Action | Query Secured Transaction | `FR-01-12-INT` |
| d2 | Decision | Inspection response OK? | `FR-01-10-INT` / `BR-ERR-01` |
| c2 | Action | Log Inspection error | `FR-01-10-INT` |
| d3 | Decision | Traffic Fine response OK? | `FR-01-11-INT` / `BR-ERR-01` |
| c3 | Action | Log Traffic Fine error | `FR-01-11-INT` |
| d4 | Decision | Mortgage response OK? | `FR-01-12-INT` / `BR-ERR-01` |
| c4 | Action | Log Mortgage error | `FR-01-12-INT` |
| j1 | Join | Synchronize parallel query results | N/A (Control Node) |
| s4 | Action | Aggregate & store results | `FR-01-13-BE` |
| s5 | Action | Display consolidated card | `FR-01-13-FE` |
## Business rule description

| Rule ID | Description | Applied Node |
|---|---|---|
| `BR-REF-01` | Concurrent Multi-Portal Query Rule: When refreshing the legal status, the system must simultaneously query the three external government portals (Inspection, Traffic Fine, Secured Transaction) in parallel via a fork node to minimize overall latency. | `f1` |
| `BR-ERR-01` | Partial Data Fault Tolerance Rule: During the parallel legal status check, if any individual external portal query fails or times out, the system must independently log the error, bypass that failed component without crashing, and proceed to aggregate whatever data is successfully retrieved. | `d2`, `d3`, `d4` |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
|---|---|---|---|
| `d1 [Yes]` | A legal status refresh request is actively processing | The car owner clicks "Refresh Legal Status" again | System disables the duplicate click and terminates the redundant workflow |
| `d1 [No]` | No legal status request is currently in progress | The car owner clicks "Refresh Legal Status" | System forks the process and queries the three external portals in parallel |
| `d2 [Yes]` | The Vehicle Inspection Portal has been queried | The portal returns a valid data response | System routes the successful data to the join node for aggregation |
| `d2 [No]` | The Vehicle Inspection Portal has been queried | The portal connection times out or returns an error | System logs the inspection error and proceeds to the join node with missing data |
| `d3 [Yes]` | The Traffic Fine Portal has been queried | The portal returns a valid data response | System routes the successful data to the join node for aggregation |
| `d3 [No]` | The Traffic Fine Portal has been queried | The portal connection times out or returns an error | System logs the traffic fine error and proceeds to the join node with missing data |
| `d4 [Yes]` | The Secured Transaction Registry has been queried | The portal returns a valid data response | System routes the successful data to the join node for aggregation |
| `d4 [No]` | The Secured Transaction Registry has been queried | The portal connection times out or returns an error | System logs the mortgage error and proceeds to the join node with missing data |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| Consolidated Legal Record | Outdated -> Refreshing -> Aggregated | Empty, Outdated, Refreshing, Aggregated, Partially Aggregated |
| External Portal Query (Inspection / Fine / Mortgage) | Sent -> Responded / Error | Pending, Sent, Responded, Timeout, Error |

## 5. Screens touched

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: What is the specific timeout limit for the external portal queries before the system logs an error and proceeds to the join node?] | No | A strict 5-second timeout is applied to each parallel API call to prevent UI freezing. |
| 2 | [NEEDS CLARIFICATION: How should the UI render the "consolidated card" if one or more portals return an error (partial failure)?] | No | Display successfully retrieved data normally and show a "Data unavailable at the moment" placeholder/badge for the failed sections. |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live | Pass | Nguyễn Danh Quang | 2026-11-04 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass | Nguyễn Danh Quang | 2026-11-04 |
| No action has two incoming flows (merge used instead) | Pass | Nguyễn Danh Quang | 2026-11-04 |
| Every fork has a matching join | Pass | Nguyễn Danh Quang | 2026-11-04 |
| Every system action cites an FR | Pass | Nguyễn Danh Quang | 2026-11-04 |
| Every FR of this use case appears in the diagram | 6 of 6 | Nguyễn Danh Quang | 2026-11-04 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass | Nguyễn Danh Quang | 2026-11-04 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass | Nguyễn Danh Quang | 2026-11-04 |
| AI critic prompt run, findings resolved or logged as open questions | Pass | Nguyễn Danh Quang | 2026-11-04 |
