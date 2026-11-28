# Activity Diagram: AD-UC-1.07 Refresh Legal Status

| Field | Value |
|---|---|
| AD ID | `AD-UC-1.07` |
| Use case | `UC-1.07 Refresh Legal Status` (includes `UC-1.08`, `UC-1.09`, `UC-1.10`) |
| Feature | `F-01 Vehicle Passport` |
| Primary actor | Car Owner |
| Lanes | Car Owner · TASCO Mobility System · Vehicle Inspection Portal · Traffic Fine Lookup Portal · Secured Transaction Registry Portal |
| Inputs (L3-L5) | Context Diagram v1 · Use Case Diagram v1 · Sequence: `S-04` · W1 rows (`FR-01-09-FE`, `FR-01-10-INT`, `FR-01-11-INT`, `FR-01-12-INT`, `FR-01-13-BE`, `FR-01-13-FE`) |
| Trigger | Car Owner taps `"Làm mới"` in the `"Tình trạng pháp lý"` section on `S-F01-02` to update the vehicle's consolidated legal status |
| Preconditions | Car Owner is signed in and viewing an existing vehicle profile on `S-F01-02` |
| Success end state | Legal status data (inspection validity, unpaid traffic fines, and mortgage status) is refreshed in parallel from all three external portals, aggregated into `Verified Vehicle Report`, and displayed on `S-F01-02` |
| Failure end state(s) | One or more external portals time out (5-second limit) or fail; `TASCO Mobility System` logs the individual errors, marks failed rows as `"Đang chờ cập nhật"`, updates `Verified Vehicle Report` with available partial data, and renders the consolidated card with retry prompts |
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
    a2("View latest legal status on 'Tình trạng pháp lý' card")
    stop1@{ shape: fr-circ }
    stop2@{ shape: fr-circ }
  end

  subgraph SYS["TASCO Mobility System"]
    d1{"Request already in progress?"}
    c1("Disable 'Làm mới' and ignore duplicate click (FR-01-09-FE)")
    f1@{ shape: fork }
    s1("Send plate and VIN to Inspection Portal (FR-01-10-INT)")
    s2("Send plate to Traffic Fine Portal (FR-01-11-INT)")
    s3("Send VIN to Secured Transaction Portal (FR-01-12-INT)")
    d2{"Inspection response OK?"}
    c2("Log Inspection error & mark status pending (FR-01-10-INT)")
    d3{"Traffic Fine response OK?"}
    c3("Log Traffic Fine error & mark status pending (FR-01-11-INT)")
    d4{"Secured Transaction response OK?"}
    c4("Log Secured Transaction error & mark status pending (FR-01-12-INT)")
    j1@{ shape: join }
    s4("Aggregate results & update Verified Vehicle Report (FR-01-13-BE)")
    s5("Display latest legal status on 'Tình trạng pháp lý' card (FR-01-13-FE)")
  end

  subgraph PORTAL1["Vehicle Inspection Portal"]
    e1("Process and return registration & inspection validity")
  end

  subgraph PORTAL2["Traffic Fine Lookup Portal"]
    e2("Process and return outstanding unpaid fines")
  end

  subgraph PORTAL3["Secured Transaction Registry Portal"]
    e3("Process and return mortgage status")
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
- **Concurrent Multi-Portal Queries (`BR-REF-01`, `f1`):** Fork node `f1` mirrors the `par` fragment in Sequence Diagram `S-04`, dispatching parallel queries to `Vehicle Inspection Portal` (`UC-1.08`, plate + `VIN`), `Traffic Fine Lookup Portal` (`UC-1.09`, plate), and `Secured Transaction Registry Portal` (`UC-1.10`, `VIN` for mortgage status).
- **Partial Fault Tolerance (`BR-ERR-01`, `d2`–`d4`, `j1`):** External portals act as black boxes; if any query times out (`5s` limit) or fails, the system logs that error independently and marks that row `"Đang chờ cập nhật"` (`c2`, `c3`, `c4`), synchronizes all branches at `j1`, updates `Verified Vehicle Report` (`FR-01-13-BE`), and renders the consolidated card (`FR-01-13-FE`).

## 2. Node trace

| Node | Kind | Text | Traces to |
|---|---|---|---|
| d1 | Decision | Request already in progress? | `FR-01-09-FE` |
| c1 | Action | Disable `"Làm mới"` and ignore duplicate click | `FR-01-09-FE` |
| f1 | Fork | Dispatch parallel portal queries (`par` block in `S-04`) | `BR-REF-01` |
| s1 | Action | Send license plate and `VIN` to `Vehicle Inspection Portal` (`UC-1.08`) | `FR-01-10-INT` |
| e1 | Action | Process and return registration & inspection validity (`Vehicle Inspection Portal`) | N/A (External system) |
| s2 | Action | Send license plate to `Traffic Fine Lookup Portal` (`UC-1.09`) | `FR-01-11-INT` |
| e2 | Action | Process and return outstanding unpaid fines (`Traffic Fine Lookup Portal`) | N/A (External system) |
| s3 | Action | Send `VIN` to `Secured Transaction Registry Portal` (`UC-1.10`) | `FR-01-12-INT` |
| e3 | Action | Process and return mortgage status (`Secured Transaction Registry Portal`) | N/A (External system) |
| d2 | Decision | Inspection response OK? | `FR-01-10-INT` / `BR-ERR-01` |
| c2 | Action | Log Inspection error & mark row status as `"Đang chờ cập nhật"` | `FR-01-10-INT` / `BR-ERR-01` |
| d3 | Decision | Traffic Fine response OK? | `FR-01-11-INT` / `BR-ERR-01` |
| c3 | Action | Log Traffic Fine error & mark row status as `"Đang chờ cập nhật"` | `FR-01-11-INT` / `BR-ERR-01` |
| d4 | Decision | Secured Transaction response OK? | `FR-01-12-INT` / `BR-ERR-01` |
| c4 | Action | Log Secured Transaction error & mark row status as `"Đang chờ cập nhật"` | `FR-01-12-INT` / `BR-ERR-01` |
| j1 | Join | Synchronize parallel query results | N/A (Control Node) |
| s4 | Action | Aggregate results & update `Verified Vehicle Report` | `FR-01-13-BE` |
| s5 | Action | Display latest legal status on `"Tình trạng pháp lý"` card | `FR-01-13-FE` |

## 3. Branches and acceptance tests

| Branch | Given | When | Then |
|---|---|---|---|
| d1 [Yes] | A legal status refresh request is actively processing on `S-F01-02` | Car Owner taps `"Làm mới"` again | System disables `"Làm mới"` (`#E4E6EB` fill, `#6B7280` text) and terminates the duplicate click without spawning new queries |
| d1 [No] | No legal status refresh request is currently in progress on `S-F01-02` | Car Owner taps `"Làm mới"` in `"Tình trạng pháp lý"` | System disables `"Làm mới"` and forks (`f1`) parallel queries to all three external government portals |
| d2 [Yes] | `Vehicle Inspection Portal` has been queried with license plate and `VIN` | The portal returns valid registration and inspection validity within 5 seconds | System routes the inspection validity (e.g., `"Hạn đến 16/10/2027"`) to join node `j1` for aggregation |
| d2 [No] | `Vehicle Inspection Portal` has been queried | The portal connection times out (`> 5s`) or returns an error | System logs the inspection error (`c2`) and routes `"Đang chờ cập nhật"` for `"Đăng kiểm"` to join node `j1` |
| d3 [Yes] | `Traffic Fine Lookup Portal` has been queried with license plate | The portal returns valid traffic violation data within 5 seconds | System routes the fine status (e.g., `"Không có vi phạm"`) to join node `j1` for aggregation |
| d3 [No] | `Traffic Fine Lookup Portal` has been queried | The portal connection times out (`> 5s`) or returns an error | System logs the traffic fine error (`c3`) and routes `"Đang chờ cập nhật"` for `"Phạt nguội"` to join node `j1` |
| d4 [Yes] | `Secured Transaction Registry Portal` has been queried with `VIN` | The portal returns valid mortgage status within 5 seconds | System routes the mortgage status (e.g., `"Đang thế chấp"`) to join node `j1` for aggregation |
| d4 [No] | `Secured Transaction Registry Portal` has been queried | The portal connection times out (`> 5s`) or returns an error | System logs the secured transaction error (`c4`) and routes `"Đang chờ cập nhật"` for `"Thế chấp"` to join node `j1` |

## 4. Object states used

| Object | States in this diagram | Full state list (all diagrams) |
|---|---|---|
| Verified Vehicle Report | Outdated -> Refreshing -> Updated | Initialized, Outdated, Refreshing, Updated, Completed, Expired |
| Consolidated Legal Record | Outdated -> Refreshing -> Aggregated | Empty, Outdated, Refreshing, Aggregated, Partially Aggregated |
| External Portal Query (Inspection / Fine / Secured Transaction) | Sent -> Responded / Error | Pending, Sent, Responded, Timeout, Error |

## 5. Screens touched

| Actor action | Screen ID |
|---|---|
| `a1` Tap `"Làm mới"` in `"Tình trạng pháp lý"` | `S-F01-02` |
| `a2` View latest legal status on `"Tình trạng pháp lý"` card | `S-F01-02` |

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
| Lane names match the Context Diagram and the use case actors; no Web App, API or database lanes | Pass — exact 5 lanes matching Context Diagram and Sequence Diagram `S-04` | Nguyễn Danh Quang | 2026-11-28 |
| Related sequence diagram uses the same names and FR IDs; its alt fragments appear here as decisions | Pass — synced with `S-04` (`par` block mapped to `f1`/`j1`, `Update Verified Vehicle Report` mapped to `s4`, `FR-01-09`..`FR-01-13`) | Nguyễn Danh Quang | 2026-11-28 |
| AI critic prompt run, findings resolved or logged as open questions | Pass | Nguyễn Danh Quang | 2026-11-28 |

---

Template source: DBIZ2 Lesson 7, VJCBI College - FTU. Notation: OMG UML 2.5.1, Clause 15.
Position: the black-box activity diagram of a use case (MBSE Grid cell "Use Cases", item 2), the cell
left open in Prof. Seki's Lesson 4. The inside of the system stays in the Lesson 4 sequence diagrams.
Structure aligned with the DBIZ3 Spec Document, section 4 (Flows), so the file can be linked from a
module spec without rewriting.
