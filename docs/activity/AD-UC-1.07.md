# Activity Diagram: AD-UC-1.07 Refresh Legal Status

| Field | Value |
|---|---|
| AD ID | `AD-UC-1.07` |
| Use case | `UC-1.07 Refresh Legal Status` |
| Feature | `F-01 Vehicle Passport` |
| Primary actor | Car Owner |
| Lanes | Car Owner · TASCO Mobility System · Vehicle Inspection Portal · Traffic Fine Lookup Portal · Secured Transaction Registry Portal |
| Inputs (L3-L5) | [Context Diagram](../context/context-diagram.md) · [Use Case Diagram](../use-case/use-case-diagram.md) · Sequence: [SD-UC-1.07](../sequence/SD-UC-1.07.md) · [W1 Function List](../function-list/w1-function-list.md) (`FR-01-09-FE`, `FR-01-10-INT`, `FR-01-11-INT`, `FR-01-12-INT`, `FR-01-13-BE`, `FR-01-13-FE`) |
| Trigger | Car Owner taps `"Làm mới"` in the `"Tình trạng pháp lý"` section on `S-F01-02` to update the vehicle's consolidated legal status |
| Preconditions | Car Owner is signed in and viewing an existing vehicle profile on `S-F01-02` |
| Success end state | Legal status data is successfully refreshed in parallel from all three external portals, aggregated, stored, and the updated consolidated `"Tình trạng pháp lý"` card is displayed on `S-F01-02` |
| Failure end state(s) | One or more external portals time out (5-second limit) or fail; `TASCO Mobility System` logs the individual errors, aggregates available partial data, displays valid rows normally, and marks failed rows as `"Đang chờ cập nhật"` with a retry prompt |
| FRs covered | `FR-01-09-FE`, `FR-01-10-INT`, `FR-01-11-INT`, `FR-01-12-INT`, `FR-01-13-BE`, `FR-01-13-FE` |
| Business rules applied | `BR-REF-01`, `BR-ERR-01` |
| Version / status | v1.0 · Reviewed |
| Author | Nguyễn Danh Quang |

## 1. Diagram

```mermaid
swimlane-beta TB
  subgraph CAR["Car Owner"]
    start@{ shape: sm-circ }
    a1("Tap 'Làm mới' in 'Tình trạng pháp lý'")
    a2("View consolidated 'Tình trạng pháp lý' card")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end

  subgraph SYS["TASCO Mobility System"]
    d1{"Request already in progress?"}
    c1("Disable 'Làm mới' and ignore duplicate click (FR-01-09-FE)")
    f1@{ shape: fork }
    s1("Query Inspection Portal (FR-01-10-INT)")
    s2("Query Traffic Fine Portal (FR-01-11-INT)")
    s3("Query Secured Transaction Portal (FR-01-12-INT)")
    d2{"Inspection response OK?"}
    c2("Log Inspection error (FR-01-10-INT)")
    d3{"Traffic Fine response OK?"}
    c3("Log Traffic Fine error (FR-01-11-INT)")
    d4{"Mortgage response OK?"}
    c4("Log Mortgage error (FR-01-12-INT)")
    j1@{ shape: join }
    s4("Aggregate & store results (FR-01-13-BE)")
    s5("Display consolidated 'Tình trạng pháp lý' card (FR-01-13-FE)")
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
  d1 -->|"[Yes]"| c1 --> stop2
  d1 -->|"[No]"| f1
  f1 --> s1 --> e1 --> d2
  f1 --> s2 --> e2 --> d3
  f1 --> s3 --> e3 --> d4
  d2 -->|"[Yes]"| j1
  d2 -->|"[No]"| c2 --> j1
  d3 -->|"[Yes]"| j1
  d3 -->|"[No]"| c3 --> j1
  d4 -->|"[Yes]"| j1
  d4 -->|"[No]"| c4 --> j1
  j1 --> s4 --> s5 --> a2 --> stop1
```

Reading notes:

- **Duplicate Request Guard (`d1`, `c1`):** `TASCO Mobility System` immediately disables `"Làm mới"` (`FR-01-09-FE`) while a refresh request is active so duplicate taps do not trigger redundant portal calls.
- **Concurrent Multi-Portal Queries (`BR-REF-01`, `f1`):** Fork node `f1` dispatches parallel queries to `Vehicle Inspection Portal` (`FR-01-10-INT`), `Traffic Fine Lookup Portal` (`FR-01-11-INT`), and `Secured Transaction Registry Portal` (`FR-01-12-INT`) to minimize latency.
- **Partial Fault Tolerance (`BR-ERR-01`, `d2`–`d4`, `j1`):** External portals act as black boxes; if any query times out (`5s` limit) or fails, the system logs that failure independently (`c2`, `c3`, `c4`), synchronizes all branches at `j1`, aggregates available data (`FR-01-13-BE`), and renders valid rows alongside `"Đang chờ cập nhật"` placeholders for failed rows (`FR-01-13-FE`).

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| d1 | Decision | Request already in progress? | `FR-01-09-FE` |
| c1 | Action | Disable `"Làm mới"` and ignore duplicate click | `FR-01-09-FE` |
| f1 | Fork | Dispatch parallel portal queries | `BR-REF-01` |
| s1 | Action | Query Inspection Portal | `FR-01-10-INT` |
| e1 | Action | Process and return response (`Vehicle Inspection Portal`) | N/A (External system) |
| s2 | Action | Query Traffic Fine Portal | `FR-01-11-INT` |
| e2 | Action | Process and return response (`Traffic Fine Lookup Portal`) | N/A (External system) |
| s3 | Action | Query Secured Transaction Portal | `FR-01-12-INT` |
| e3 | Action | Process and return response (`Secured Transaction Registry Portal`) | N/A (External system) |
| d2 | Decision | Inspection response OK? | `FR-01-10-INT` / `BR-ERR-01` |
| c2 | Action | Log Inspection error | `FR-01-10-INT` / `BR-ERR-01` |
| d3 | Decision | Traffic Fine response OK? | `FR-01-11-INT` / `BR-ERR-01` |
| c3 | Action | Log Traffic Fine error | `FR-01-11-INT` / `BR-ERR-01` |
| d4 | Decision | Mortgage response OK? | `FR-01-12-INT` / `BR-ERR-01` |
| c4 | Action | Log Mortgage error | `FR-01-12-INT` / `BR-ERR-01` |
| j1 | Join | Synchronize parallel query results | N/A (Control Node) |
| s4 | Action | Aggregate & store results | `FR-01-13-BE` |
| s5 | Action | Display consolidated `"Tình trạng pháp lý"` card | `FR-01-13-FE` |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [Yes] | A legal status refresh request is actively processing on `S-F01-02` | Car Owner taps `"Làm mới"` again | System disables `"Làm mới"` (`#E4E6EB` fill, `#6B7280` text) and terminates the duplicate click without spawning new queries |
| d1 [No] | No legal status refresh request is currently in progress on `S-F01-02` | Car Owner taps `"Làm mới"` in `"Tình trạng pháp lý"` | System disables `"Làm mới"` and forks (`f1`) parallel queries to all three external government portals |
| d2 [Yes] | `Vehicle Inspection Portal` has been queried with license plate and `VIN` | The portal returns valid inspection data within 5 seconds | System routes the inspection validity (e.g., `"Hạn đến 16/10/2027"`) to join node `j1` for aggregation |
| d2 [No] | `Vehicle Inspection Portal` has been queried | The portal connection times out (`> 5s`) or returns an error | System logs the inspection error (`c2`) and routes a pending indicator (`"Đang chờ cập nhật"`) to join node `j1` |
| d3 [Yes] | `Traffic Fine Lookup Portal` has been queried with license plate | The portal returns valid traffic violation data within 5 seconds | System routes the fine status (e.g., `"Không có vi phạm"`) to join node `j1` for aggregation |
| d3 [No] | `Traffic Fine Lookup Portal` has been queried | The portal connection times out (`> 5s`) or returns an error | System logs the traffic fine error (`c3`) and routes a pending indicator (`"Đang chờ cập nhật"`) to join node `j1` |
| d4 [Yes] | `Secured Transaction Registry Portal` has been queried with `VIN` | The portal returns valid mortgage registry data within 5 seconds | System routes the mortgage status (e.g., `"Đang thế chấp"`) to join node `j1` for aggregation |
| d4 [No] | `Secured Transaction Registry Portal` has been queried | The portal connection times out (`> 5s`) or returns an error | System logs the mortgage error (`c4`) and routes a pending indicator (`"Đang chờ cập nhật"`) to join node `j1` |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| Consolidated Legal Record | Outdated -> Refreshing -> Aggregated | Empty, Outdated, Refreshing, Aggregated, Partially Aggregated |
| External Portal Query (Inspection / Fine / Mortgage) | Sent -> Responded / Error | Pending, Sent, Responded, Timeout, Error |

## 5. Screens touched

| Actor action | Screen ID |
|---|---|
| `a1` Tap `"Làm mới"` in `"Tình trạng pháp lý"` | `S-F01-02` |
| `a2` View consolidated `"Tình trạng pháp lý"` card | `S-F01-02` |

## 6. Open questions

| # | Question | Blocking? | Default applied |
|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: What is the specific timeout limit for the external portal queries before the system logs an error and proceeds to the join node?] | No | A strict 5-second timeout is applied to each parallel API call to prevent UI freezing. |
| 2 | [NEEDS CLARIFICATION: How should the UI render the `"Tình trạng pháp lý"` card if one or more portals return an error (partial failure)?] | No | Display successfully retrieved rows normally and show `"Đang chờ cập nhật"` with a retry prompt on failed rows (aligned with `S-F01-02` rule `SR-002`). |

## 7. Verification log

| Check | Result | By | Date |
|---|---|---|---|
| Renders on mermaid.live | Pass | Nguyễn Danh Quang | 2026-11-28 |
| Every decision has guards on all outgoing flows, no overlap, no gap | Pass | Nguyễn Danh Quang | 2026-11-28 |
| No action has two incoming flows (merge used instead) | Pass | Nguyễn Danh Quang | 2026-11-28 |
| Every fork has a matching join | Pass (`f1` matched with `j1`) | Nguyễn Danh Quang | 2026-11-28 |
| Every system action cites an FR | Pass | Nguyễn Danh Quang | 2026-11-28 |
| Every FR of this use case appears in the diagram | 6 of 6 (`FR-01-09-FE`, `FR-01-10-INT`, `FR-01-11-INT`, `FR-01-12-INT`, `FR-01-13-BE`, `FR-01-13-FE`) | Nguyễn Danh Quang | 2026-11-28 |
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass — strictly follows Prof. Seki's black-box lane rule (`TASCO Mobility System` and the 3 external portals match Context Diagram; DB persistence details remain in `SD-UC-1.07`) | Nguyễn Danh Quang | 2026-11-28 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass | Nguyễn Danh Quang | 2026-11-28 |
| AI critic prompt run, findings resolved or logged as open questions | Pass | Nguyễn Danh Quang | 2026-11-28 |

---

Template source: DBIZ2 Lesson 7, VJCBI College - FTU. Notation: OMG UML 2.5.1, Clause 15.
Position: the black-box activity diagram of a use case (MBSE Grid cell "Use Cases", item 2), the cell
left open in Prof. Seki's Lesson 4. The inside of the system stays in the Lesson 4 sequence diagrams.
Structure aligned with the DBIZ3 Spec Document, section 4 (Flows), so the file can be linked from a
module spec without rewriting.
